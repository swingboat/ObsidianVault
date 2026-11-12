---
type: video-production-dossier
project: video-creation-claude-opus
video_id: domino-amplifier
captured: '2026-11-12'
status: 已有 30 秒成片（out/多米诺放大器.mp4），媒体规格与代码推算吻合；未记录视觉验收和用户验收
source_fingerprint: a839f3cd3c48d3759793ecbf00dd56fac43d49714ef9180b96d5bf0a5e39f905
tags:
- video-creation
- production-capture
- blender
- rigid-body-simulation
- tts
---
# 多米诺放大器 · Blender 刚体模拟 + 变速重映射 + Node Canvas 叠加的 30 秒科普短片
<!-- capture-source-sha256:a839f3cd3c48d3759793ecbf00dd56fac43d49714ef9180b96d5bf0a5e39f905 -->

> 未找回制作对话，本档案的结论全部来自项目文件（源码、脚本、TTS 缓存、字幕和成片媒体头）。目录中没有 `prompt.md`、`plan/` 或 README，原始需求原文**未记录**。

## 1. 当前结论与制作路线

**状态**

| 项目 | 结论 | 证据等级 |
|---|---|---|
| 成片 | `out/多米诺放大器.mp4` 存在 | 最终产物存在 |
| 规格 | 1920×1080、30 fps、894 帧、29.80 s、H.264 yuv420p ≈3.78 Mb/s；AAC 48 kHz 双声道 ≈192 kb/s | ffprobe 读取（技术检查通过） |
| 与代码一致性 | 894 帧 = round(29.811 × 30)；29.811 s 可由 +7% 语速的 TTS 缓存时长按 `tts.py` 公式重算得到 | 推算吻合（见 §6） |
| 字幕 | `out/多米诺放大器.srt` 11 条，时点与 +7% 缓存吻合 | 文件存在 + 推算吻合 |
| 视觉检查 | 本轮未抽帧查看画面 | 无法确认 |
| 用户验收 | 未记录 | 无法确认 |

注意：源码文件修改时间（10-15 23:05）比 MP4（10-15 23:02）晚 3 分钟，所有源码时间戳完全相同，可能是整体复制/提交时写入，也可能是出片后又改过代码。**无法确认当前代码与成片完全对应**。

**画幅**：16:9 横屏（`look.py:201`、`lib.mjs:7` 均为 1920×1080，30 fps）。

**一句话管线**

```text
script.json（5 句旁白+数字）
 → tts.py：Edge TTS 合成 + 自动调语速凑 30 s → timeline.json / narration.wav / SRT
 → Blender sim.py：1200 Hz 刚体模拟 13 块 1.5 倍递增骨牌 → sim.npy / falls.json
 → Blender shots.py：按台词时点把模拟时间非线性重映射到 30 fps 输出时间，逐帧打关键帧 + 跟拍相机，EEVEE 渲染 → frames/f_####.jpg + events.json
 → Node render.mjs：@napi-rs/canvas 叠 HUD/字幕；audio.mjs 按碰撞事件合成音效与环境音乐
 → ffmpeg：原始 RGBA 管道 + 三轨混音（侧链闪避、限幅）→ out/多米诺放大器.mp4
```

| 步骤 | 输入 | 处理 | 输出 | 入口 |
|---|---|---|---|---|
| 需求/文案 | 无记录 | 文案与数字已固化在 JSON | `script.json` | `script.json` |
| 声音（旁白）先行 | `script.json` | edge-tts 合成、词边界对时、语速迭代 | `build/timeline.json`、`build/narration.wav`、`out/*.srt` | `tts.py:134` |
| 物理模拟 | `scene.py` 参数 | Blender Bullet 刚体，headless | `build/sim.npy`、`build/falls.json` | `sim.py:38` |
| 动作/镜头 | 模拟结果 + timeline | 时间重映射、相机轨迹、关键帧 | `build/events.json`、`build/frames/` | `shots.py:195` |
| 剪辑/叠加 | 帧序列 + 事件 + timeline | Canvas 叠加层，程序化音频 | `build/music.wav`、`build/sfx.wav` | `render.mjs` |
| 导出 | 以上全部 | ffmpeg libx264 CRF18 + AAC | `out/多米诺放大器.mp4` | `render.mjs:43` |

关键设计：**旁白先行，画面跟旁白对时**。模拟只跑一次，与文案解耦；改台词后只需重跑 `tts.py` 和 `shots.py`，变速曲线会自动重新求解，让第 1 块被碰到落在“碰”字上、第 13 块落地落在“一百三十倍”末尾。

## 2. 创作目标与需求

原始需求原文未记录。以下从 `script.json` 摘录（摘录）：

- 片名：`多米诺放大器`；声线：`zh-CN-XiaoxiaoNeural`。
- `sources`：
  - `Lorne A. Whitehead, "Domino 'chain reaction'", American Journal of Physics 51, 182 (1983)`
  - `Empire State Building: roof 381 m, tip 443 m`
- `numbers`：`first_mm: 5`、`growth: 1.5`、`d13_ratio: "1.5^12 ≈ 130"`、`d13_cm: "0.5 cm × 1.5^12 ≈ 65 cm"`、`d29_m: "0.5 cm × 1.5^28 ≈ 426 m"`。
- 五段旁白（摘录）：

| id | 文本 | gap |
|---|---|---|
| hook | 轻轻一碰，能推倒一座摩天楼吗？ | 0.6 |
| fact | 1983年，物理学家怀特黑德发现：一块多米诺骨牌，能推倒比它大一半的骨牌。（手动分 3 条字幕） | 默认 0.45 |
| why | 因为立着的骨牌存着重力势能，一碰就全部释放出来。 | 默认 0.45 |
| grow | 每块放大一点五倍，第十三块已经是第一块的一百三十倍。 | 0.7 |
| tower | 照这样摆下去，第二十九块就有四百多米，比帝国大厦楼顶还高！ | — |

从代码可归纳出的目标（推断）：

- 时长严格 ≤30 s（`tts.py:31` `TARGET = 30.0`）。
- 用**真实物理模拟**而不是手 K 动画来演示 Whitehead 的 1.5 倍放大链（13 块，5 mm → 约 65 cm）。
- 第 29 块（426 m）不进 3D 场景，只用 HUD 条形图与帝国大厦楼顶 381 m 对比（`hud.mjs:82`）。
- 定位为“物理小实验”科普短片（HUD 徽章文字，`hud.mjs:12`）。

**视频讲解的知识 vs 制作所用算法**：视频讲的是 Whitehead 多米诺放大（势能 ∝ 质量×重心高度 ∝ 尺寸⁴，每块势能约为上一块 1.5⁴≈5 倍，见 `hud.mjs:64-67`）；制作中用的是 Bullet 刚体模拟、单调时间重映射与二分求解、高斯平滑相机、程序化音频合成。两者不要混淆：项目并未推导或验证 Whitehead 的 1.5 倍上限，只是在模拟里设定 `GROWTH = 1.5` 并检查链条没断（`sim.py:93`）。

## 3. 技术与依赖

| 类别 | 内容 | 版本/来源 | 证据 |
|---|---|---|---|
| 3D/物理 | Blender（`bpy`，Bullet 刚体世界），headless `blender -b --factory-startup -P` | 版本**未记录**。使用 Principled BSDF `Coat Weight` 输入，推断 ≥ Blender 4.0；渲染引擎先试 `BLENDER_EEVEE` 再回退 `BLENDER_EEVEE_NEXT`（`look.py:187`），兼容新旧命名 | 代码 |
| 数值 | NumPy（Blender 内置 Python 中使用） | 未记录 | `sim.py`、`shots.py` |
| TTS | `edge_tts`（Edge 在线神经 TTS），`aiohttp` | 未记录（无 requirements 文件）；`.venv` 不在项目中 | `tts.py` |
| 字体提取 | `fontTools.ttLib.TTCollection` | 未记录 | `fonts.py` |
| 2D 合成 | `@napi-rs/canvas` | package.json `^1.0.11`，lock 锁定 **1.0.11** | `package-lock.json` |
| Node | ES module（`"type": "module"`，顶层 await） | Node 版本未记录 | `package.json`、`render.mjs` |
| 编码 | ffmpeg（libx264 / aac / loudnorm / sidechaincompress / alimiter） | 版本未记录 | `tts.py:180`、`render.mjs:43` |
| 平台 | macOS（系统字体路径、`/System/Library/AssetsV2/...` 字体资产） | 硬件未记录 | `fonts.py:12`、`lib.mjs:17-18` |

**TTS 参数**：声线 `zh-CN-XiaoxiaoNeural`，pitch +0 Hz，rate 由脚本自动求得（缓存显示用过 +0% 与 +7%，最终 +7%，见 §6），`boundary="WordBoundary"` 取词级时间戳。

**字体**：

- `PingFangSC-Semibold.ttf`（注册名 `PingB`，中文正文）与 `YuantiSC-Bold.ttf`（注册名 `YuanB`，代码中注册但 `ZH`/`NUM` 未引用，推断最终未使用）由 `fonts.py` 从系统 `.ttc` 集合中抽出单个字面，因为 Canvas 只注册 `.ttc` 的第一个字面（`fonts.py:3`）。
- `Arial Rounded Bold`（`RoundB`，数字）、`Hiragino Sans GB`（`HiraGB`，中文回退）。
- `ZH = 'PingB, HiraGB'`、`NUM = 'RoundB, PingB'`（`lib.mjs:19-20`）。
- `build/fonts/` 中两份 ttf 仍在（日志级证据：抽取执行过）。

**无模型**：本片没有使用任何生成式图像/视频模型；唯一的 AI 组件是 Edge 在线 TTS。

**可选 HDRI**：`look.py:172` 若存在 `build/hdri.hdr` 则作为世界环境贴图，否则只用背景强度 0.8。项目中没有该文件，也没有下载脚本——**是否在成片中使用 HDRI 无法确认**。

## 4. 镜头与时间轴

整片是**一个连续长镜头**（无剪辑点），相机由程序轨迹驱动。下表时点由 +7% 语速 TTS 缓存按 `tts.py` 的排布公式推算，并与 SRT 对上（推算，非渲染日志）。

| 输出时间 (s) | 内容 | 实现入口 | 关键参数 |
|---|---|---|---|
| 0.00–0.50 | 黑场淡入 | `hud.mjs:122` | 线性 alpha |
| 0.30–0.90 | 左上徽章“物理小实验 · 多米诺放大器”出现 | `hud.mjs:6` | 持续到 T_LAND+1.2 后淡出 |
| 0.00–1.95 | 钢球单摆从 −5° 摆入；镜头后移并左偏 20 mm 以框住来球 | `scene.py:116`、`shots.py:113` | 前段慢放 ≈7.1×（输出 1.949 s 对应模拟 0.275 s） |
| 1.40–3.89 | hook 旁白 | `tts.py:155` | LEAD_IN 1.4 s |
| **1.949** | **T_HIT：球碰第 1 块，正对“碰”字起点**；金属“叮”音 | `shots.py:44`、`audio.mjs:48` | 1.4 + 0.549 |
| 1.95 起 | 左上计数器“第 N 块 · 高度 · 是第 1 块的 X 倍高”，每碰到下一块弹跳刷新 | `hud.mjs:18` | `pop` 0.35 s，`easeBack` |
| 4.61–11.49 | fact 旁白；右上信息卡 “Whitehead, Am. J. Phys. 51, 182 (1983) / 一块骨牌能推倒约 1.5 倍大的下一块” | `hud.mjs:60` | 从“1983”字起，到段尾+0.4 |
| 12.06–16.14 | why 旁白；信息卡“势能 ∝ 质量 × 重心高度 ∝ 尺寸⁴ / 每块的势能 ≈ 上一块的 1.5⁴ ≈ 5 倍” | `hud.mjs:64` | 从“重力势能”字起 |
| 16.71–21.30 | grow 旁白 | — | gap 0.7 |
| 1.95–21.30 | 骨牌链依次倒下；相机沿链条跟拍，距离 = 7.5×当前骨牌高，方位 −102°→−84°，仰角 9°→18° | `shots.py:108` | 焦距 60 mm，f/5.6，高斯平滑 σ=10 帧 |
| **21.297** | **T_LAND：第 13 块（≈65 cm）落地，正对“一百三十倍”末尾**；低频 boom、音乐转 D 大调 | `shots.py:45`、`audio.mjs:97,109` | 后段慢放倍率由二分法求解 |
| 20.70–26.10 | 收尾：相机升高拉远看全链，距离对数插值到 3300 mm+160 mm/s，方位→−62°，仰角→34°，光圈 f/5.6→f/11 | `shots.py:120` | `smoothstep(T_LAND−0.6, T_LAND+4.8)` |
| 21.8–22.5 | 徽章与计数器淡出 | `hud.mjs:7,21` | T_LAND+0.5→+1.2 |
| 22.12–27.49 | tower 旁白；右上面板“5 mm × 1.5²⁸ ≈ 426 m”，三根条：第 13 块 65 cm / 第 29 块（数字从 0 滚到 426 m）/ 帝国大厦楼顶 381 m | `hud.mjs:82` | 第 29 块条从“第二十九”字起 1.2 s 生长；帝国大厦条从“帝国大厦”字起 1.0 s |
| 29.31–29.81 | 黑场淡出 | `hud.mjs:122` | TAIL 2.2 s |
| 全程 | 底部居中圆角字幕条，52 px | `hud.mjs:98` | 按词边界对时 |

镜头相机关键常量（`shots.py:108-129`、`shots.py:205-212`）：

```text
lens 60 mm, DOF on, focus_object = Focus 空物体（跟随 target）
k      = chain_index(t) + 0.35          # 看向“正在倒”的骨牌稍前方
target = (x + 0.25h, 0, 0.42h)
dist   = 7.5h × (1 + 0.35·pre)          # pre: 碰撞前的“来球”构图权重
az     = −102° + 18°·smoothstep(1, 21, t)
el     =    9° +  9°·smoothstep(2, 21, t)
fstop  = 5.6（收尾 → 11.0）；写入时 /1000（单位为 mm 的修正）
clip   = [0.05·d, 30·d]
```

## 5. 关键算法与实现

### 5.1 场景与单位（`scene.py`）

- **1 BU = 1 mm**：`scale_length = 0.001`，重力写成 `−9810 BU/s²`（`scene.py:47-50`）。毫米单位让 5 mm 小骨牌在 Bullet 中不至于尺寸过小。
- 骨牌几何：第 i 块高 `h = 5 × 1.5^i` mm，厚 `0.13h`、宽 `0.5h`，与下一块的间距 `0.65h`（以本块高度计）。

| 块 | 高 (mm) | 厚 | 宽 | 后沿 x | 相对质量 |
|---|---|---|---|---|---|
| 1 | 5.0 | 0.65 | 2.5 | 0 | 0.0081 |
| 4 | 16.9 | 2.19 | 8.4 | 18.5 | 0.31 |
| 7 | 57.0 | 7.4 | 28.5 | 81.0 | 12 |
| 10 | 192.2 | 25.0 | 96.1 | 292.1 | 462 |
| 13 | 648.7 | 84.3 | 324.4 | 1004.2 | 17 700 |

（由 `layout()` 公式本次手算，非日志；链条总长约 1.51 m。）质量 = `DENSITY × h × t × w = 6.5e-5 h³`，首尾质量比约 2.2×10⁶。

- 刚体：骨牌 `ACTIVE`，BOX 碰撞形状，摩擦 0.45、恢复系数 0.3，碰撞边距 `min(0.04, 0.02t)`，线阻尼 0、角阻尼 0.01，**关闭休眠**（`use_deactivation = False`，避免小骨牌被提前冻结）；地面是 6 m×6 m×5 cm 的 `PASSIVE` 盒子（摩擦 0.6，隐藏不渲染）。
- 世界：`substeps_per_frame = 2`、`solver_iterations = 40`、`use_split_impulse = True`。
- **高频模拟技巧**：`time_scale = fps / SIM_HZ = 30/1200`。场景 fps 仍是 30，但每个 Blender 帧只推进 1/1200 s 真实时间（每子步 1/2400 s）。这样用普通的逐帧缓存就得到 1200 Hz 的模拟采样，后续慢放 12× 仍有足够时间分辨率。

### 5.2 钢球单摆：解析驱动的运动学物体

钢球（半径 2.5 mm、摆线 300 mm）设为 `kinematic`，位置不由 Bullet 求解，而是用解析式逐帧打关键帧（`sim.py:21`，碰撞前后 ±0.08 s 每帧打 key，其余每 8 帧一个 key，线性插值）。

```text
ω  = sqrt(g/L) = sqrt(9810/300) ≈ 5.72 rad/s
θ0 = −5°，t_hit = π/(2ω) ≈ 0.275 s
阶段1（摆入）  θ(t) = θ0·cos(ωt)
阶段2（压入）  接触角速度 vc = −θ0·ω（线速度≈150 mm/s）
              ω2 = vc·L / 1.2   （刚好压入 1.2 mm 后停住）
              θ = (vc/ω2)·sin(ω2·τ)，持续 π/ω2 ≈ 25 ms
阶段3（回弹）  θ = −0.5·(vc/ω)·sin(ωτ)·e^(−0.8τ)   （半速反弹并衰减）
```

效果：球以确定的方式“推”第 1 块约 1.2 mm 然后弹回，触发时间完全可预测（`pendulum_hit_time()`），供后续对台词。限制：球对骨牌的作用是运动学强推，不是动量交换；回弹参数（0.5、0.8）是经验值。

### 5.3 模拟录制与链条判定（`sim.py`）

```text
for f in 1..MAX_T·SIM_HZ (最多 7200 帧):
    frame_set(f); 读取每个物体 evaluated matrix_world
    记录 [x y z qw qx qy qz]；记录倾角 tilt = acos(局部 Z 轴·世界 Z)
    第 13 块倾角 > 80° 后再多跑 0.6 s 就提前结束
sim.npy  = float32 [帧, 物体(13 骨牌+球), 7]
falls.json 每块: touch(倾角>0.3° 的首帧时间), half(>45°), land(首次达到 最终角−1.5°), final
CHAIN OK ⇔ 所有骨牌最终倾角 > 30°
```

- 支持命令行覆盖参数调参：`blender -b --factory-startup -P sim.py -- GAP=0.6 BOUNCE=0.3`（`sim.py:39-43`，用 `setattr(scene, k, float(v))` 改模块常量）。
- `falls.json` 中的 `touch` 与 `land` 是后面音效、计数器与变速求解的唯一时间来源。
- `build/sim.npy`、`build/falls.json` 现已不在项目中（构建产物被清理），因此**实际模拟时长、每块 touch/land 数值无法确认**。

### 5.4 时间重映射：让物理事件对上台词（`shots.py:40-75`）

核心问题：真实模拟里，从球碰第 1 块到第 13 块落地只有约 1–2 s 量级（推断，falls.json 已清理），而旁白从“碰”到“一百三十倍”有约 19.3 s。做法是定义一个随模拟时间变化的**慢放倍率** f(s)（每 1 s 模拟对应多少秒输出），积分得到单调映射 t(s)，再反查。

```text
S_HIT  = falls.ball_hit（≈0.275 s）       T_HIT  = “碰”字起点（≈1.949 s）
S_LAND = 第 13 块 land                     T_LAND = 最后一个含“倍”的词的结束（≈21.297 s）

pre = T_HIT / S_HIT                         # 碰撞前恒定慢放，≈7.1×
f(s) = pre                                    s < S_HIT
early(s) = pre + (12 − pre)·smoothstep(S_HIT, S_HIT+0.05, s)   # 碰后 50 ms 内升到 12×
f(s) = exp( ln early + (ln f_end − ln early)·smoothstep(0.55, 2.4, s) )  # 对数域过渡到 f_end

t(s) = ∫₀ˢ f(σ) dσ     # 20000 点梯形积分（build_map）
二分 40 次求 f_end ∈ [1, 12]，使 t(S_LAND) = T_LAND
sim_time(t) = interp(t, TS, SS)               # 输出时间 → 模拟时间
```

- 设计意图（推断）：小骨牌倒得极快，需要 12× 慢放才看得清；大骨牌本身慢，倍率逐渐降低，接近真实速度，从而“越往后越重”的感觉保留下来。
- 因为 t(s) 由正的 f 积分得到，映射严格单调，不会倒放。
- `pose(s, k)`：在 1200 Hz 采样之间对位置线性插值、对四元数 `slerp`（`shots.py:78`）。
- 每个输出帧（894 帧）对每个物体都打 `location` + `rotation_quaternion` 关键帧，插值类型设为 LINEAR。打 key 前若与上一帧四元数点积为负则取反（`shots.py:153`），避免 q 与 −q 跳变导致插值/运动模糊时物体翻转。
- 摆线：用一根半径 0.12 mm 的圆柱，每帧放在球心与支点的中点、按 `atan2` 角度旋转（`shots.py:159-165`）。
- `write_events()` 把模拟时间的事件（球“叮”、第 i 块撞第 i+1 块的 `clack`、每块落地 `land`）转换为输出时间写入 `events.json`，`clack` 的强度用**撞击者**（上一块）的高度，`land` 用自身高度。

### 5.5 跟拍相机（`shots.py:89-138`）

1. `TOUCH[i]` = 第 i 块被碰的输出时间；`chain_index(t)` 在这些时间点之间线性插值，得到连续的“当前正在倒的骨牌序号”（末端多加 1.2 s 走到 13）。
2. `domino_at(k)` 在相邻两块之间插值：位置线性，高度**几何插值** `h0·(h1/h0)^u`，与 1.5 倍几何增长一致，使镜头距离平滑放大。
3. 相机放在球坐标 `target + d·(cos el·cos az, cos el·sin az, sin el)`，朝向用 `to_track_quat("-Z","Y")`。
4. 原始轨迹 7 个通道（target xyz、ln d、az、el、fstop）先逐帧算出，再用 σ=10 帧、窗口 ±30 帧的高斯核卷积平滑（边缘 `edge` 填充）。对 **ln d** 而不是 d 平滑，适合跨两个数量级的距离变化。
5. 景深：Blender 的光圈按“场景单位视为米”计算，而本场景单位是 mm，所以写入 `fstop/1000`（`shots.py:171-172`）。否则 5 mm 的骨牌会几乎全部虚化（推断后果）。

### 5.6 外观与渲染（`look.py`）

- **骨牌材质**：色相按序号从 0 到 0.72 均匀分布（红→紫），HSV 饱和 0.88、明度 0.72；Roughness 0.28，清漆 Coat 0.7 / 0.06。点数与中线用**纯节点数学**生成：在 Generated 坐标上计算到 8 个点心的最小距离 `< 0.043` 即为点，`|z−0.5| < 0.007` 为中线；只在法线 |X|>0.7 的两个大面上出现；同一遮罩驱动颜色混合（米白 0.93/0.92/0.88）与反向 Bump（强度 0.4），形成凹点。无贴图文件。
- 倒角修改器：宽度 `0.1 × 最小尺寸`，3 段。
- **木地板**：Brick 纹理做 1.2 m × 14 cm 板条、1.2 mm 缝；Wave 纹理（Y 向、扭曲 7）做木纹并乘到颜色；Noise 映射粗糙度 0.4–0.62；两级 Bump（木纹 0.25 + 板缝 1.0）。平面 40 m。
- 钢球：金属 1.0、粗糙 0.07；摆线：浅米色、粗糙 0.8。
- 灯光：太阳光强度 3.2、角直径 0.12 rad、暖色 (1.0, 0.9, 0.78)；世界背景强度 0.8，可选 HDRI。
- **渲染设置**：EEVEE，`taa_render_samples = 64`；若属性存在则开启光线追踪、阴影（2 条光线、8 步）、Fast GI；运动模糊 shutter 0.5；1920×1080 JPEG 质量 94；色彩管理 AgX + “AgX - Punchy”。
- 渲染循环支持断点续渲：从已有 `f_*.jpg` 数量+1 开始（`shots.py:227`）；`--still t1 t2` 只渲染指定时刻预览。
- 单帧渲染耗时、总渲染时长**未记录**。

### 5.7 Python 与 Node 如何衔接

两边不直接调用，**只通过 `build/` 里的文件交接**（另有 `lib.mjs` 在缺字体时调用 `.venv/bin/python fonts.py` 这一处跨语言进程调用）。

| 文件 | 生产者 | 消费者 | 内容 |
|---|---|---|---|
| `build/timeline.json` | `tts.py`（.venv Python） | `shots.py`（Blender Python）、`lib.mjs` | 总时长、语速、每段 start/end、词级时间戳 `words[]`（含字符索引 `idx`）、字幕 `cues[]` |
| `build/narration.wav` | `tts.py`（ffmpeg） | `render.mjs` | 已响度归一化的旁白轨 |
| `build/sim.npy`、`build/falls.json` | `sim.py`（Blender） | `shots.py` | 1200 Hz 位姿记录与每块事件时间 |
| `build/events.json` | `shots.py` `write_events()` | `lib.mjs` → `hud.mjs`、`audio.mjs` | `t_hit`、`t_land`、`heights`、`touch[]`、按时间排序的 `clink/clack/land` 事件 |
| `build/frames/f_####.jpg` | `shots.py`（EEVEE） | `render.mjs` | 894 张 1080p 底图，文件名 1 起编号 |
| `build/fonts/*.ttf` | `fonts.py` | `lib.mjs` | 抽出的单字面字体 |

- `shots.py -- --events` 只写 `events.json` 不渲染，可在不重渲染的情况下更新 HUD/音效对时。
- 两端都只用 TTS 真实词边界取时（`lib.mjs:28` 注释 “never estimated”）：Python 端 `word_time(seg, text)`，Node 端 `word(id, w, nth)`/`at()`，按字符索引查找所在词。
- `render.mjs` 检查 894 帧是否齐全，缺帧直接报错；`--still` 预览模式下缺帧用灰底代替，可在 Blender 未出图时先调 HUD。
- 合成：`ctx.drawImage(JPG)` → `overlay(ctx, t)` → `getImageData` 得 RGBA → 写入 ffmpeg stdin（`-f rawvideo -pix_fmt rgba`），处理背压（`write` 返回 false 时等 `drain`）。不落地中间 PNG。

## 6. 声音与字幕

### 6.1 旁白（`tts.py`）

- 引擎：Edge 在线神经 TTS（`edge_tts.Communicate`），声线 `zh-CN-XiaoxiaoNeural`，pitch +0 Hz。
- 端点改写：`tts.py:18-24` 注释称 2026 年 10 月起旧主机 `speech.platform.bing.com` 合成返回 403，Edge 改走 `api.msedgeservices.com`，因此把 `edge_tts.communicate.WSS_URL` 覆盖为新地址（引用 rany2/edge-tts#490、#491；本次已访问核实，见 §10）。注释要求待 edge-tts 发版修复后删除此覆盖。
- 429 限流重试：最多 5 次，等待 8/16/24/32 s（`tts.py:51-61`）。
- 缓存键：`sha1("voice|rate|pitch|text")[:16]`，缓存 mp3 + 词边界 JSON（`build/tts_cache/`）。
- **自动语速**：时间预算 `available = 30 − LEAD_IN 1.4 − Σgap − TAIL 2.2`；每段语音时长取“最后一个词的结束 + 0.12 s”（注释：mp3 尾部约 0.6 s 静音会侵占间隔，所以不用文件时长）。超时则按超出比例一次跳到 `ceil(rate + (speech/available − 1)·100) + 1`，最多 +30%。

**缓存证据还原的两轮（缓存显示执行过）**：本次用同一键公式核对，10 个缓存正好是 5 段 × {rate +0, +7}。

| 段 | +0% 结束 (s) | +7% 结束 (s) | +7% 缓存键 |
|---|---|---|---|
| hook | 2.663 | 2.488 | 401329662045cb0c |
| fact | 7.363 | 6.881 | 268c6ee71e0407f3 |
| why | 4.363 | 4.077 | 1b4954a104e00f98 |
| grow | 4.913 | 4.591 | 46d458ca8cdb7f24 |
| tower | 5.750 | 5.374 | a34f68abd3c9ebff |

推算：+0% 总长 1.4 + 25.65 + 2.2 + 2.2 = 31.45 s > 30 → rate = ceil(5.99)+1 = **+7%** → 总长 29.811 s，与成片 29.80 s / 894 帧吻合（最终采用 +7%，推算）。

- 排布：段 i 起点 = 上段起点 + 上段语音时长 + gap；词时间加上段起点后写入 timeline。
- 混音：每段 `aresample=48000, adelay` 放到起点 → `amix normalize=0` → `highpass 80 Hz` → `loudnorm I=−16 : TP=−1.5 : LRA=11` → `apad, atrim` 到总长 → `build/narration.wav`（48 kHz 立体声）。

### 6.2 字幕

- 默认按中文标点切分，每条 ≤15 字、句末标点强制断开，去掉尾部 `，。：；`（`tts.py:105`）；`fact` 段在 `script.json` 中手动给出 3 条 `captions`。
- 对时：每条字幕覆盖的字符区间 `[a, b)` 内的词，取首词 start、末词 end（`tts.py:121`）。
- 输出两份：`out/多米诺放大器.srt`（11 条，独立文件）与烧录字幕（`hud.mjs:98`）。烧录版每条显示到下一条开始，段首提前 0.08 s 淡入，段尾在 end+0.12→+0.3 s 淡出；底部居中、52 px、半透明圆角底。

### 6.3 音效（`audio.mjs` `buildSfx`）

全部程序合成，无采样素材；按 `events.json` 放置，尺寸参数 `s = ln(h/5)/ln(650/5)`（5 mm→0，65 cm→1），“骨牌越大越低、越响、越长”。

| 事件 | 合成方式 | 参数 |
|---|---|---|
| clink（钢球碰骨牌） | 3 个非谐分音正弦 3400×{1, 2.7, 5.2} Hz 指数衰减 + 30 ms 6→4 kHz 带通噪声 | 振幅 0.12×{1, .5, .25} |
| clack（骨牌撞骨牌） | 带通噪声（中心 f·1.6→f，Q 2.2）+ 正弦 0.55f | f = 2600·0.22ˢ，amp = 0.25+0.55s，时长 0.05+0.12s |
| land（落地拍击木地板） | 带通噪声（2f→0.7f，Q 0.9）+ 下滑正弦（90+400(1−s) Hz） | f = 1500·0.12ˢ，amp = 0.2+0.9s²，时长到 0.85 s |

- 带通噪声：RBJ biquad 带通，每 32 采样更新一次系数，中心频率指数滑动；随机数为 mulberry32 风格 PRNG，固定种子（音效 7、音乐 42），可完全复现。
- 混响：4 路带低通反馈的并联梳状滤波器（延迟 1422–1617 采样，增益 0.77–0.8），湿声 0.22。

### 6.4 音乐（`buildMusic`）

- 每小节 3.2 s 的柔和铺底：Dm–B♭–Gm–A 循环，每个音 ±0.08 半音双失谐正弦；音量从 4 s 起随时间爬升（`lift` 0.6→1.2，到 `t_land − 3`）。
- T_LAND−0.5 s 之后的小节换成 D 大调和弦（`[50, 57, 62, 66, 69, 74]`），作为“巨块落地”的情绪释放。
- 每小节 4 下高八度拨弦作“时钟”滴答（注释说随链条加速变密，但代码中固定每小节 4 下——**注释与代码不一致**）。
- T_LAND 前 2.4 s 带通噪声上扬（300→4000 Hz，振幅 ∝ p²），T_LAND 一个 48→36 Hz 的低频 boom。
- 淡入 0.8 s、淡出 2.0 s；`writeWav` 峰值超过 0.98 时整体缩放，单声道复制成立体声 16-bit 48 kHz。

### 6.5 总混音（`render.mjs:47-50`）

```text
旁白 asplit → [v1] 作侧链，[v2] 进总线
音乐 volume 0.7 → sidechaincompress(threshold 0.05, ratio 5, attack 20 ms, release 400 ms)  # 旁白时自动闪避
音效 volume 0.8
amix(3, normalize=0, duration=first) → alimiter(limit 0.93)
视频 libx264 preset medium CRF 18 yuv420p；音频 AAC 192k；+faststart
```

- 最终响度：旁白单轨做过 loudnorm −16 LUFS；成片总混音后的整体响度**未记录**，本次也未测量。是否有人试听**未记录**。

## 7. 迭代、问题与修正

项目没有 plan/ 或制作日志，以下来自代码注释与缓存（证据等级逐条标注）。

| 问题/取舍 | 处理 | 依据 | 等级 |
|---|---|---|---|
| Edge TTS 旧端点 403 | 覆盖 WSS_URL 为 `api.msedgeservices.com` | `tts.py:18-24` | 代码中存在；缓存生成成功说明合成可用 |
| 免费端点 429 限流 | 指数退避重试 5 次 | `tts.py:55-61` | 代码中存在；是否触发过未记录 |
| 首轮旁白 31.45 s 超 30 s | 自动跳到 +7% | tts_cache 两组缓存 | 缓存显示执行过（推算） |
| mp3 尾部静音约 0.6 s 侵占间隔 | 用末词 end+0.12 s 排布 | `tts.py:146` | 代码注释 |
| Canvas 只注册 .ttc 第一个字面 | fontTools 抽出单字面 ttf | `fonts.py:3` | 代码中存在；`build/fonts` 有产物 |
| Blender 光圈按米解释 | fstop/1000 | `shots.py:171` | 代码注释 |
| 四元数符号跳变 | 与上一帧点积为负则取反 | `shots.py:153` | 代码中存在 |
| 链条参数调试 | 命令行 `-- GAP=… BOUNCE=…` 覆盖 | `sim.py:39` | 注释示例；实际试过哪些值未记录 |
| 渲染中断 | 按已有帧数续渲 | `shots.py:227` | 代码中存在；是否发生过未记录 |
| 小骨牌被 Bullet 休眠 | `use_deactivation = False` | `scene.py:77` | 代码中存在；动机为推断 |

最终参数（GAP 0.65、BOUNCE 0.3、FRICTION 0.45、θ0 −5°）是否为多轮调试结果，**无法确认**；`sim.py` 的 `CHAIN OK/BROKEN` 输出说明作者设计了链条完整性检查，但当次输出未保存。

## 8. 可复用制作经验

1. **旁白先行，用真实词边界驱动画面**：先合成 TTS 拿到词级时间戳，再让物理事件、HUD、音效全部引用具体字词（“碰”“一百三十倍”“第二十九”），两端都禁止估算时间。依据：`tts.py`、`shots.py:29-45`、`lib.mjs:28-38`。
2. **模拟一次，用单调变速映射对时**：物理模拟与文案解耦；定义慢放倍率 f(s) 并积分，二分求解末段倍率让关键事件落在目标时点。改台词只需重跑 TTS + 打 key，不必重新模拟或手调物理。依据：`shots.py:48-75`。
3. **高频刚体采样的低成本做法**：保持场景 30 fps，把 `time_scale` 设为 fps/1200，让每个 Blender 帧 = 1/1200 s，再按输出帧插值位置/slerp 旋转。适合高速碰撞后需要大倍率慢放的镜头。依据：`scene.py:88`、`shots.py:78-86`。
4. **跨数量级场景用 mm 单位 + 对数域插值**：5 mm 到 65 cm 的物体用 1 BU = 1 mm；相机距离在 ln 域平滑、骨牌高度几何插值；注意 Blender 光圈需按单位比例修正（/1000）。依据：`scene.py:3`、`shots.py:105,125,172`。
5. **自动调语速满足时长上限**：按超出比例一次跳到所需 rate，配合按“文本+声线+语速”哈希的缓存，迭代成本低且可复现。依据：`tts.py:143-153`。
6. **事件驱动的程序化音效**：从模拟导出碰撞/落地事件，按物体尺寸映射音高、响度、时长，固定随机种子，声音与画面天然同步。依据：`shots.py:182`、`audio.mjs:45-86`。
7. **Python 渲染底图、Node Canvas 叠 2D、ffmpeg stdin 管道出片**：3D 与 2D 分层，HUD 改动不必重渲染 Blender 帧；`--still` 和 `--events` 两个轻量入口方便局部预览。依据：`render.mjs`、`shots.py:196-224`。
8. **Canvas 中文字体先抽单字面**：@napi-rs/canvas 只认 .ttc 首字面，需用 fontTools 抽出需要的粗细。依据：`fonts.py`。

## 9. 复现入口

项目没有 `build.sh`/README，执行顺序由脚本间依赖推断（记录命令不代表本轮执行过）：

```bash
cd domino-amplifier
# 0) 依赖：Python venv（edge-tts、aiohttp、fontTools）、Blender（含 numpy）、Node + npm、ffmpeg
python3 -m venv .venv && .venv/bin/pip install edge-tts aiohttp fonttools   # 版本未记录（推断命令）
npm install                                    # @napi-rs/canvas 1.0.11（lock）
# 1) 旁白、时间轴、SRT
.venv/bin/python tts.py                        # → build/timeline.json, build/narration.wav, out/多米诺放大器.srt
# 2) 物理模拟（与 1 无依赖，可并行）
blender -b --factory-startup -P sim.py         # → build/sim.npy, build/falls.json；看 "CHAIN OK"
#    调参示例：blender -b --factory-startup -P sim.py -- GAP=0.6 BOUNCE=0.3
# 3) 事件 + 关键帧 + EEVEE 渲染（依赖 1 和 2）
blender -b --factory-startup -P shots.py -- --events     # 仅写 build/events.json
blender -b --factory-startup -P shots.py -- --still 2 12 # 预览单帧 → build/still_<t>.jpg
blender -b --factory-startup -P shots.py                 # → build/frames/f_0001..0894.jpg（可断点续渲）
# 4) 叠加 + 音频 + 出片
node render.mjs --still 8.0 22.9               # 预览叠加 → build/still_<t>.png
node render.mjs                                # → build/music.wav, build/sfx.wav, out/多米诺放大器.mp4
```

- 需要先恢复：`.venv`、`node_modules`、`build/sim.npy`、`build/falls.json`、`build/timeline.json`、`build/narration.wav`、`build/events.json`、`build/frames/`（均已清理）。
- 现存缓存：`build/tts_cache/`（10 组）可让 `tts.py` 不联网重现同样旁白；`build/fonts/` 可免去字体抽取。
- 字体依赖 macOS 系统字体资产；非 macOS 环境需替换 `lib.mjs` 中的字体路径。
- `.venv/bin/python` 在 `lib.mjs:12` 中写死，缺字体时会自动调用。
- 可选：放置 `build/hdri.hdr` 作为环境光（来源未记录）。
- Blender 版本、渲染耗时、硬件未记录；不同 Bullet 版本下链条可能需要重新调 GAP/BOUNCE（推断）。

## 10. 参考来源

| 来源 | 类型 | 借鉴/用途 | 落点 | 访问状态 |
|---|---|---|---|---|
| Lorne A. Whitehead, “Domino ‘chain reaction’”, *American Journal of Physics* 51, 182 (1983) | 技术资料（科普内容依据） | 1.5 倍放大系数、13 块链条的设定 | `script.json` sources；`scene.py` `GROWTH=1.5`、`N=13`；fact 段信息卡 | 项目只给文献引用，无 URL；本次未查阅原文，原文内容未核实 |
| 帝国大厦高度：楼顶 381 m、塔尖 443 m | 技术资料（数据） | tower 段对比条用楼顶 381 m | `script.json`；`hud.mjs:94` | 项目未给 URL；本次未联网核实 |
| https://github.com/rany2/edge-tts/issues/490 | 技术资料（故障） | 旧端点 403 问题 | `tts.py:18-20` 注释 | 2026-11-12 访问成功：issue “Edge-tts failed .WSServerHandshakeError: 403…”，2026-10-10 开，状态 Closed |
| https://github.com/rany2/edge-tts/issues/491（重定向到 pull/491） | 技术资料（修复方案） | 改用 `api.msedgeservices.com` | `tts.py:21-24` | 2026-11-12 访问成功：PR “Fix 403s: synthesize through api.msedgeservices.com…”，2026-10-12，状态 Closed，页面显示 “wants to merge”，即未合并 |

- 没有记录任何创作参考视频、X 帖子或前作链接。
- 片中数字（1.5¹² ≈ 130、0.5 cm × 1.5²⁸ ≈ 426 m、1.5⁴ ≈ 5）本次已手算核对，算术正确。

## 11. 证据与限制

**实际读取的文件**（项目相对路径 + sha256 前 16 位，取自 fingerprints.json）：

| 文件 | sha256 前 16 位 | 读取方式 |
|---|---|---|
| `domino-amplifier/script.json` | 1c908aa2aca0c947 | 全文 |
| `domino-amplifier/scene.py` | eed553d15e70907c | 全文 |
| `domino-amplifier/sim.py` | 0c0be6e770b2dfed | 全文 |
| `domino-amplifier/shots.py` | ff1954111ae98783 | 全文 |
| `domino-amplifier/look.py` | decf9ee2c978cd98 | 全文 |
| `domino-amplifier/tts.py` | 24c1041dcc6a12dc | 全文 |
| `domino-amplifier/fonts.py` | 401075443efdea74 | 全文 |
| `domino-amplifier/lib.mjs` | b58f85eeba437946 | 全文 |
| `domino-amplifier/hud.mjs` | bbdc308a7111d4e0 | 全文 |
| `domino-amplifier/audio.mjs` | ceae70068ed5257e | 全文 |
| `domino-amplifier/render.mjs` | b92ebc969f34b17f | 全文 |
| `domino-amplifier/package.json` | 775d6086bcec758b | 全文 |
| `domino-amplifier/package-lock.json` | 8b4d7c2b791ba20d | 版本字段 |
| `domino-amplifier/build/tts_cache/*.json`（10 个） | 见 fingerprints.json | 词边界与末词时间 |
| `domino-amplifier/out/多米诺放大器.srt` | 4971e3c634b0c8f6 | 全文 |
| `domino-amplifier/out/多米诺放大器.mp4` | —（14 843 089 字节） | 仅 ffprobe 媒体头 |

**未读取/未检查**：TTS 缓存 mp3 本体与字体文件内容；成片画面（未抽帧，无视觉检查）；成片总响度。

**缺失信息**：原始需求与提示词（无 `prompt.md`）、设计文档（无 `plan/`）、制作对话、Blender/Python 包/Node/ffmpeg 版本、硬件与渲染耗时、`sim.npy`/`falls.json` 的实际数值（每块倒下时间、最终慢放倍率 f_end）、是否使用 HDRI、用户验收意见。按项目规则，本作品缺少 `prompt.md`，与「一个作品一个目录」规则要求不完全一致。

**时间戳**：TTS 缓存 2026-10-15 21:51–21:52，SRT 21:53，字体 22:27，MP4 23:02，全部源码 23:05（见 §1 关于代码与成片一致性的说明）。
