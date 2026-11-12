---
type: video-production-dossier
project: video-creation-claude-opus
video_id: rukawa-buzzer-beater
captured: '2026-11-12'
status: v1 已有 30 秒完整 MP4（Cycles 路径追踪、手办风人物）；用户反馈「人物不够逼真」，后续版本转入 slamdunk-buzzer-anime
source_fingerprint: f74c41499b81dd9b94eed988b3d09cce61d88a931c68cf62b64244c74366338b
tags:
- video-creation
- production-capture
- blender
- cycles
- procedural-audio
---

# 流川枫 · 压哨三分 · Blender Cycles 全程序化建模 + 逐帧烘焙 + 事件表驱动声画

<!-- capture-source-sha256:f74c41499b81dd9b94eed988b3d09cce61d88a931c68cf62b64244c74366338b -->

> [!note] 归档说明
> 本档案为 2026-11-12 对源项目的只读回溯。**未找回制作对话，结论来自项目文件**（`prompt.md`、`plan/`、源码、`output.mp4` 媒体头）。本轮没有运行任何构建、渲染或音频脚本，也没有观看成片画面。项目 git 只有两次整体提交，`build/` 目录已清空（无 `rukawa.blend`、`events.json`、`frames/`、`audio.wav`、日志），因此迭代过程只能依据 `plan/00-architecture.md` 中的「实测与决定」段落。

## 与 slamdunk-buzzer-anime 的关系（两条路线）

两部作品是**同一需求的前后版本**，但目录、代码与证据各自独立，本档案只写 v1 的证据：

| | rukawa-buzzer-beater（本片，v1） | slamdunk-buzzer-anime（v2–v4） |
|---|---|---|
| 来源 | 用户原始需求：review 参考帖并用同样技术做 30 秒《灌篮高手》压哨三分 | 用户看过 v1 后反馈「挺好的，只是人物不够逼真」，转向动漫风（记录在 `slamdunk-buzzer-anime/prompt.md`） |
| 渲染 | Blender Cycles 路径追踪（Metal GPU）、AgX、运动模糊 | EEVEE + 卡通着色（Shader to RGB） |
| 人物 | 胶囊/椭球基本体组成的 FK 对象层级，手办/搪胶质感 | 骨架 + 蒙皮连续网格、动漫脸与发束；后又尝试 VRM 与原创角色 |
| 场所 | 夜场体育馆（悬挂 LED 记分牌） | 高中体育馆（高窗斜阳、手翻记分牌） |
| 共享 | `common.py` 时间系统、`motion/poses/ball/net/cameras` 编排、`audio/` 合成；slamdunk 的 `plan/00-architecture.md` 写明「从 v1 复制」 | 同左，并在其上改造 |

依据：`slamdunk-buzzer-anime/prompt.md`「原始需求（v1，见 ../rukawa-buzzer-beater/prompt.md）」「v2 需求」两节；两目录 `scene/common.py` 与 `audio/voices.py` 的 sha256 前 16 位相同（`eb16d3d7e995ff56`、`27aa120374920ed4`，取自盘点指纹），说明这两份文件在 v2 中原样保留。具体差异见 [[slamdunk-buzzer-anime]]。

## 1. 当前结论与制作路线

**状态：已完成 30 秒成片 `output.mp4`（v1）。** 证据等级：

| 等级 | 结论 |
|---|---|
| 代码/配置中存在 | 完整的场景构建、烘焙、渲染、音频、合成脚本 |
| 日志/缓存显示执行过 | 无法确认——`build/` 已清空，无日志留存 |
| 最终采用 | `output.mp4` 存在（文件时间晚于全部源码），推断为由 `post.sh` 产出的最终版 |
| 技术检查通过 | 本轮 ffprobe：媒体头规格与代码设定一致（见下表） |
| 视觉检查通过 | `plan/00-architecture.md` 记录了两轮静帧检查与修正；本轮未看画面 |
| 用户验收 | 部分：用户随后说「挺好的，只是人物不够逼真」（记录在 slamdunk 的 `prompt.md`），并据此要求重做人物；未见对 v1 的最终验收 |

**规格**

| 项 | 值 | 来源 |
|---|---|---|
| 分辨率 | 1920×1080，16:9 | ffprobe；`scene/lights.py:95` |
| 帧率 / 帧数 | 24 fps / 720 帧 | ffprobe `nb_frames=720`；`scene/common.py:6-7` |
| 时长 | 30.000 s | ffprobe |
| 视频编码 | H.264 High，yuv420p，约 7.85 Mb/s | ffprobe；`post.sh:8`（libx264 preset slow，CRF 16） |
| 音频 | AAC LC，48 kHz 立体声，约 259 kb/s | ffprobe；`audio/voices.py:5`（SR=48000）、`post.sh:9`（256k） |
| 文件大小 | 30,440,883 字节 | 盘点 |

**一句话管线**

参考帖技术拆解 → 0a 架构文档 / 0b 时间线文档 → `scene/*.py` 参数化生成球场、人物、观众、记分牌、灯光 → `motion.py` 以「故事时间 τ」逐帧解算人物/球/篮网/相机并烘焙为原生关键帧与形态键 → 事件表 `events.json` 同时驱动闪光灯光与 NumPy/SciPy 音效合成 → Cycles 逐帧路径追踪 PNG → ffmpeg 合成音画并首尾淡入淡出 → `output.mp4`（同时留下可编辑的 `build/rukawa.blend`）。

## 2. 创作目标与需求

原始需求（摘录自 `prompt.md`）：

> 请先review： https://x.com/guanmo_ai/status/2118258208045494688 这个帖子和视频内容。
> 总结制作这个视频所用到的技术和技巧。
>
> 然后使用同样的技术，再生成一个30秒的视频。以《灌篮高手》为背景，在一个篮球场，流川枫以一个漂亮的压轴三分球赢得比赛。请尽情自由发挥。

`prompt.md` 记录的参考帖要点（观默 × Codex《瓶中风暴》）：14 秒 / 336 帧、本机 Blender Cycles 路径追踪、不导入外部模型、不补帧；参数化波面 + 逐帧计算船底四点水位烘焙关键帧；发光曲线闪电同步驱动辅助光；NumPy + SciPy 合成声音、约 90 ms 声画延迟 + 低频滚动尾音；保留资产、针对对象迭代，产出原生 `.blend`。

本片对参考技术的「一一映射」（摘录自 `plan/00-architecture.md` 表格）：

| 《瓶中风暴》 | 本片对应实现 |
|---|---|
| 参数化波面 | 参数化 FIBA 球场：地板/线/三分弧/篮架全部由函数生成（`scene/court.py`） |
| 逐帧水位 → 烘焙升沉俯仰横摇 | 逐帧解算运球、抛物线投篮（含后旋）、篮网形变（每帧一个形态键）、落地弹跳（`ball.py`、`net.py`） |
| 闪电同步驱动辅助光 | 观众相机闪光（发光球 + 同步点光）、压哨红色 LED、比分切换，统一由 events 时间表驱动（`crowd.py`、`lights.py`、`board.py`） |
| NumPy/SciPy 合成，约 90 ms 延迟，低频尾音 | 合成运球、球鞋摩擦、哨声/蜂鸣、刷网、欢呼、击掌；按事件到相机距离加 d/343 s 声延迟；体育馆混响 + 低频尾音（`audio/`） |
| 保留资产迭代 | 模块化 `scene/*.py`，每个对象独立函数 |

自定的设计约束（`plan/00-architecture.md`）：

- 30 s / 24 fps / 720 帧 / 1920×1080（「依基准测试可降到 1600×900」，最终未降，见规格）。
- 不导入外部模型/贴图/音频，不补帧；动画全部是原生关键帧/形态键。
- 视觉风格：手办/搪胶质感风格化人物（光滑基本体 + 次表面皮肤），避免恐怖谷；夜场体育馆、暖白顶灯、抛光木地板反射；关键时刻红色 LED 与闪光。
- 版权边界：「致敬原作（湘北红 #11、红发 #10 击掌），不复制任何原作画面/商标图形」。

## 3. 技术与依赖

| 项 | 内容 | 来源/证据等级 |
|---|---|---|
| Blender | 5.2（Cycles，Metal GPU） | `plan/00-architecture.md` 声明；精确小版本未记录 |
| 渲染引擎 | Cycles，GPU，仅 Metal 设备 | `scene/lights.py:74-110`、`render.sh:15-19` |
| 降噪 | OpenImageDenoise（GPU） | `scene/lights.py:84-86` |
| 色彩管理 | AgX，look「AgX - Medium High Contrast」，曝光 −0.6 | `scene/lights.py:101-106` |
| Python 音频 | NumPy、SciPy（`signal.butter/sosfilt/fftconvolve/sawtooth/square`、`io.wavfile`） | `audio/*.py` 导入；版本未记录（`post.sh` 用 `build/.venv/bin/python`，该 venv 已清理，无 requirements 文件） |
| ffmpeg / ffprobe | libx264 + AAC 合成、规格核验 | `post.sh`；版本未记录 |
| 外部模型/贴图/音频/字体 | 无。文字由 Blender 内置字体曲线（`curves.new(..., "FONT")`）生成，未指定字体文件 | `board.py:14-26`、`figure.py:81-91` |
| TTS / AI 生成模型 | 无（本片无配音） | 全部源码 |
| 硬件 | Apple M3 Pro 18 核 GPU（Metal，仅 GPU） | `plan/00-architecture.md`「实测与决定」 |

**Cycles 渲染参数**（`scene/lights.py:74-110`，`build_scene.py` 默认 `--samples 48`）：

| 参数 | 值 |
|---|---|
| samples | 48（`render.sh` 默认 `SPP=48`；plan 实测记载「40 spp」，两者不一致，以代码为准） |
| adaptive_threshold | 0.02 |
| max / transmission / glossy / diffuse bounces | 6 / 6 / 3 / 3 |
| 焦散 | 反射、折射均关闭 |
| blur_glossy | 1.0 |
| persistent data | 开 |
| 运动模糊 | 开，shutter 0.5；观众对象单独关闭 |
| 输出 | PNG 8-bit，`use_overwrite=False` + `use_placeholder=True`（断点续渲） |

单帧耗时（`plan/00-architecture.md`）：1080p / 40 spp / 自适应 / OIDN 动画模式约 **8–11 s/帧**；观众运动模糊关闭前约 **45 s/帧**。按 8–11 s × 720 帧推算全片约 1.6–2.2 小时（推断，未记录实际总耗时）。

## 4. 镜头与时间轴

### 4.1 速度曲线（帧 → 故事时间 τ）

代码 `scene/common.py:18-30` 的 `SEGMENTS`（帧段、起止速度；变速段用 smoothstep 过渡），帧 0 对应 τ = −3.5 s：

| 帧段 | 速度 | 作用 |
|---|---|---|
| 0–185 | 1 | 片头（τ −3.5→0，计时冻结 5.6）+ 突破运球 |
| 185–205 | 1→0.2 | 收球起跳进入慢动作 |
| 205–312 | 0.2 | 出手、球飞行；f312 时 τ=5.6，计时归零、蜂鸣 |
| 312–372 | 0 | **时间定格**（子弹时间） |
| 372–432 | 0.2 | 球继续下落、刷网 |
| 432–444 | 0.2→1 | 恢复实速 |
| 444–488 | 1 | 庆祝 |
| 488–498 | 1→0.3 | 进入击掌慢动作 |
| 498–548 | 0.3 | 击掌慢动作 |
| 548–558 | 0.3→1 | 恢复 |
| 558–720 | 1 | 收尾 |

> [!warning] 文档与代码冲突
> `plan/01-timeline.md` 的后半段为 H 444–502 / I 502–512 / J 512–552 / K 552–562，代码为 444–488 / 488–498 / 498–548 / 548–558。另外 plan 写起跳 τ4.3、落地 5.2，代码 `motion.py:17` 为 `TAKEOFF, RELEASE_T, LANDING = 4.4, 4.75, 5.1`。以代码为当前版本；plan 自己也注明「实际关键 τ 对应帧在构建时打印校验」（`build_scene.py:52-53`），但构建日志已不存在。

按代码公式本次**自行推算**的关键帧（非构建日志）：τ0→f84、收球 3.95→f179、起跳 4.4→f190、出手 4.75→f210、落地 5.1→f252、蜂鸣 5.6→f312、空心入网 5.95→f414、比分 5.97→f416、球落地 6.45→f445、FINAL 7.6→f473、击掌 9.0→f538；f720 时 τ≈16.15。

### 4.2 镜头表

相机由时间轴标记绑定（`scene/cameras.py:10-11` `SHOTS`，`:146-149`），每台相机逐帧烘焙位置、四元数朝向、焦距、光圈、对焦距离（`:127-145`）。

| # | 帧 | 内容（plan/01-timeline.md） | 实现 | 焦距 / 光圈 |
|---|---|---|---|---|
| C1 | 0–84 | 从顶灯下摇到悬挂计分板：SHOHOKU 78 · SANNOH 80 · 5.6 · Q4 | `cameras.py:47-50` 机位 (−6,−8.8,1.2)→(−2,−8.4,2.2)，目标插值到记分牌 | 24→40 mm / f11 |
| C2 | 84–185 | 低机位侧向跟拍运球突破，防守者入画 | `:52-54`，对流川位置做指数平滑（α=0.12）造成滞后跟随 | 40 mm / f4 |
| C3 | 185–262 | 正前低角度仰拍起跳出手，环绕约 70° | `:56-63`，绕投篮点 42°→112°、半径 2.7 m，目标由胸口渐移向球 | 35 mm / f2.8 |
| C4 | 262–312 | 侧后方追拍空中球，背景计分板读秒到 0.0 | `:65-69`，跟随平滑后的球位（α=0.35） | 35 mm / f2.8 |
| C5 | 312–372 | 定格：绕空中球环绕 | `:71-75`，以 f312 球位为中心、半径 1.5 m、160°→235° | 40 mm / f2 |
| C6 | 372–444 | 篮下仰拍：球穿网、网鼓起涟漪、球落向镜头 | `:77-81`，机位 z 0.7，目标从篮筐下渐移向球 | 22 mm / f5.6 |
| C7 | 444–520 | 中景流川（背后沸腾观众），比分 81，樱木跑入 | `:83-88` | 45 mm / f2.8 |
| C8 | 520–580 | 击掌侧面特写慢动作，浅景深 | `:90-99`，两人连线法向、距离 3.1→2.7 m | 45 mm / f2.4 |
| C9 | 580–720 | 大全景后拉上升到计分板 FINAL 81:80，结尾淡出 | `:101-105`，(3,−6.5,1.7)→(−4.5,−15.5,8.5) | 35→28 mm / f8 |

相机统一 36 mm 传感器宽、景深开、clip 0.05–200 m；`to_track_quat("-Z","Y")` 求朝向，并与前一帧四元数点积为负时取反以避免插值翻转（`cameras.py:130-133`）。plan 写 C3「缓慢环绕 60°」、C5「环绕 120°」，代码实际为 70° 与 75°（推算自角度参数）。

### 4.3 故事事件（τ，秒）

摘录自 `plan/01-timeline.md`，代码数值不同处已标注：

- 0.0 流川 #11 持球；防守者 #9（白衣，SANNOH）后撤防守。
- 0–1.6 运球推进；1.6–2.1 变向急停（球鞋摩擦），1.85 由右手换左手（`motion.py:138`）。
- 2.1–4.0 加速到投篮点 (5.6, 3.6)，距篮约 7.7 m 的深三分。
- 起跳 4.4（plan 4.3）；4.75 最高点出手，后旋 2 转/秒；落地 5.1（plan 5.2）。防守者 4.45–4.95 晚跳封盖（`motion.py:234`）。
- 5.6 计时归零（红色 LED 亮、蜂鸣）；5.95 空心入网；5.97 比分 78→81；6.45 球落地后弹跳滚动。
- 6.0 起观众沸腾（站起跳动、闪光风暴）；樱木 #10（红发）跑向流川。
- 7.6 记分牌显示 FINAL，随后出现 `BUZZER BEATER`、`#11  RUKAWA` 横幅（`board.py:76-84`）。
- 9.0 两人击掌（闪光 + 「啪」）；之后流川侧身离开，樱木原地握拳跳两下（`motion.py:192-195`）；防守者垂头走开（`SLUMP` 姿态，`motion.py:20-24`、`:233`）。

### 4.4 角色规格（`scene/motion.py:7-15`）

| 角色 | 身高 | 球衣 / 边 / 号码色 | 发型 | 其他 |
|---|---|---|---|---|
| Rukawa #11 SHOHOKU | 1.87 m | #c8141e / #111111 / 白 | `rukawa`：13 根遮眉刘海锥 + 两侧各 4 根鬓发 + 5 根后颈发，#0c0c10 | 红色球鞋 #b5121b、白护腕 |
| Sakuragi #10 SHOHOKU | 1.89 m | 同上 | `sakuragi`：26 根黄金角分布的刺状短发，#d81e10 | 黑鞋 |
| Defender #9 SANNOH | 1.84 m | #f2f2f2 / #1b2a55 / #1b2a55 | `short`：仅发帽 | 白鞋 |

## 5. 关键算法与实现

本片是叙事短片，**没有讲解知识点**；以下全部是「制作视频所用的算法」，均来自源码阅读，未运行验证。

### 5.1 故事时间重映射（慢动作 / 定格的核心）

所有运动函数只接受故事时间 τ，不直接用帧号；变速、定格只改速度曲线，不必改动作。`common.py:55-89`：

```python
# speed_at(f): 在 SEGMENTS 中查段，变速段用 smoothstep 插值速度
TAU = [TAU0]                         # TAU0 = -3.5
for f in range(FRAMES + 1):          # 每帧 8 个子样本做中点积分
    acc = sum(speed_at(f + (k + 0.5) / 8) for k in range(8))
    TAU.append(TAU[-1] + acc / 8 / FPS)
tau_of_frame(f)  -> TAU 线性插值
frame_of_tau(t)  -> 线性查找第一个 TAU[i+1] >= t 的区间再插值
game_clock(t) = 5.6 if t < 0 else max(0, 5.6 - t)
```

效果：定格段速度为 0，τ 停在 5.6，球、人物、网、观众、记分牌都自然冻结，只有相机继续运动（C5 子弹时间）。限制：定格段内 `frame_of_tau` 返回该段第一帧，事件落在定格段时会被挤到段首。

### 5.2 人物：基本体 FK 层级 + 解剖学角度姿态库

- **建模**（`figure.py`）：每个关节是一个网格对象、原点在关节处，四肢沿 −Z 下垂，角色面向 −Y。`capsule()` 用上下两个半球环 + 椭圆截面生成锥形胶囊（`:21-49`）；`ellipsoid()` 是长度为 0 的胶囊。按身高缩放 `k = height / 1.935`。号码、队名是 Blender 文字曲线并父子到胸部。皮肤材质次表面 0.25，形成「搪胶手办」质感。
- **姿态**（`poses.py`）：姿态是 `{关节: (x前屈, y外展, z扭转)}` 的角度字典；`SIGN` 表把解剖学正方向换算到左右肢不同的欧拉符号（`:12-20`），`REST` 给默认肘/膝微屈。`mix()` 线性混合两个姿态，`add()` 叠加偏移姿态。关键姿态：`STAND/ATHLETIC/DEFEND/GATHER/SET/RELEASE/LAND/CLOSEOUT/FIST/HIGH_FIVE_R`。
- **程序化跑步**（`poses.py:113-127`）：相位 `phase = 已走弧长 / 步幅`（`motion.py:133`，流川步幅 2.6 m），用弧长而非时间驱动步态，避免脚滑。大腿 `amt*(20+42*sin)`，膝盖、脚踝、手臂与之耦合。
- **运球手臂覆盖**（`poses.py:130-137`）：以运球相位的余弦 `u` 驱动上臂/前臂/手腕，与球的弹跳同周期（0.5 s）。
- **贴地**（`motion.py:96-104`）：没有 IK，根据两腿大腿/小腿角度算出较低一只脚的高度差，抬降根节点让脚着地。
- **路径**（`motion.py:42-59`）：`(τ, x, y)` 控制点上的 Catmull-Rom/Hermite 插值；朝向由速度方向、面向篮筐、面向队友之间用 `lerp_ang` 渐变。
- **跳跃**：`jump(t,t0,t1)` 为对称抛体，`v0 = g·T/2`。
- **朝向解缠绕**（`motion.py:247-249`）：烘焙时把本帧航向角换算到与上一帧相差 ±π 内。plan 记录这是为修正「运动模糊里人物整圈旋转」。

### 5.3 篮球轨迹

`ball.py`：

1. **运球**（`:85-91`）：球在身体侧前方（局部 ±0.3, −0.42），高度 `R + (0.92 − R)·|cos(π·phase)|`；1.5–2.0 s 间从右侧平移到左侧（换手）。
2. **持球**：两手中点沿胸口前向偏 0.1 m、上抬 0.05 m（`:98-102`），需先 `frame_set` 让 FK 层级求值后读 `matrix_world`。
3. **投篮**（`:105-110`）：给定出手点 p0 和出手时刻 4.75、入网时刻 5.95，直接解抛体初速度
   `v0 = (hoop − p0)/T;  v0.z += ½·g·T`，保证在 T = 1.2 s 时精确到达篮圈中心（空心）。日志会打印出手点、初速度、顶点高度（`:159-160`）。
4. **后旋**：飞行期每帧绕「水平飞行方向 × Z」轴旋转 2 转/秒 × dτ（`:183-184`）。定格时 dτ=0，旋转自然停止。
5. **入网后物理**（`:113-145`）：以 960 Hz 显式欧拉积分。入网时水平速度 ×0.22、竖直 ×0.5；网内（篮圈下 0.45 m 内）加水平弹簧 `−30·c − 3·v` 把球拉回中心、竖直阻尼 `2.6·(−v_z)`；落地恢复系数 0.74、水平摩擦 0.9，竖直速度 < 0.25 m/s 时转为滚动，滚动减速 0.5 m/s²；撞到 x=15.3 的端墙反弹 0.5。每次有效弹跳写入 `bounce` 事件（含强度）。
6. **滚动旋转**：落地后按 `|v|/R·dt` 绕 `Z × v` 轴转动，保证不打滑（`:185-189`）。

### 5.4 篮网：每帧一个形态键

`net.py`：

- 网格为 12 圈 × 7 层的菱形网（相邻层错半格），层距 0.068 m、向下收窄；Wireframe 修改器（粗 6.5 mm）+ 1 级细分生成绳子（`:8-50`）。
- `deform()`（`:53-82`）：对每个顶点，若与球心高度差在 1.6 倍接触半径内，则沿球的竖直轴径向把顶点推到球面外，强度按 `exp(−25·超出量)` 衰减，并在球通过后整体下拽；球穿过某一层后，该层开始做阻尼振荡涟漪：
  `amp = exp(−u/0.45)·(0.4+0.6·层号比)`，`z += 0.045·amp·sin(2π·3.2·u)`，半径 `×(1+0.06·amp·sin(2π·3.2u+1.3))`。
- `bake_swish()`（`:85-111`）：只对球靠近或穿网后 2 s 内的帧生成形态键，每帧一个键，并在 f−1/f/f+1 打 0/1/0 关键帧，相当于逐帧切换「网格缓存」。这是参考帖「逐帧计算并烘焙」思路在软体上的移植，避免布料模拟。

### 5.5 记分牌：可见性关键帧

`board.py`：中央悬挂四面 + 两端墙面共 6 块显示面（`:88-105`）。比分、计时每个可能取值都是一个独立文字对象，用常量插值的 `hide_render/hide_viewport` 关键帧控制显隐（`common.key_visible`）。计时按 `ceil(clock·10)` 逐 0.1 s 切换（`:42-43`、`:68-75`），0.0 用红色；比分 81 出现后闪烁三次（`:65-66`）；τ7.6 起 Q4 换为 FINAL，之后依次出现横幅。

### 5.6 观众与闪光

- **观众**（`crowd.py:26-89`）：一个「胶囊身体 + 头」原型网格，所有观众是共享该网格的对象（linked duplicate），颜色写入 `ob.color`，材质用 Object Info 读取实现每人不同衣色；主队侧用湘北红/白/黑调色板，客队侧多为白/藏青。8% 空位、随机缩放与朝向。动作不逐人烘焙：14 个空物体作为「组 rig」，每个观众随机父子到其中一组，组 rig 每 2 帧烘焙一次「起立 + 跳动 + 摇摆」（`:41-60`）。所有观众对象 `cycles.use_motion_blur = False`。
- **闪光**（`crowd.py:92-164`）：48 个闪光点（小发光球 + 同步点光源，1500 W），按随机过程触发：间隔服从指数分布，速率 `flash_rate(τ)` 随剧情升高（<4 s：1.2/s；投篮：7/s；入网后：30/s；击掌瞬间额外 +40/s）；每个闪光点触发后 0.4 s 内不可重复使用；持续 0.035 s（至少 1 帧），常量插值关键帧。定格段另有 9 个闪光点以 0.6 亮度「凝固」整段。每次闪光写 `flash` 事件，供快门音效使用。

### 5.7 灯光

`lights.py`：顶棚 5×3 个 1.6 m 面光（各 2400 W）+ 对应的自发光灯板；看台与端墙暖色泛光（1400/700 W，用于让观众呈剪影但有颜色）；篮筐后方冷色轮廓光 2500 W；蜂鸣时篮板 LED 材质发光强度 0→40、红色点光 0→900 W，持续到 τ8.0 后 12 帧淡出（`lights.py:48-61`、`board.py:108-115`）。世界背景 #0c0f16、强度 0.3。

## 6. 声音与字幕

- **配音 / TTS / 字幕：无。** 本片没有台词与字幕。
- **全部音效与音乐由 NumPy/SciPy 合成**（`audio/voices.py` 生成器、`audio/synth.py` 混音），48 kHz 立体声，输出 `build/audio.wav`。不使用任何采样或外部音频。
- **声画同源**：`build_scene.py` 把场景中记录的事件（含 τ、帧、世界坐标）与每帧相机位置 `listener` 一起写入 `events.json`（`build_scene.py:56-60`）；`synth.py` 只读这份 JSON 摆放声音。

### 6.1 事件 → 声音映射（`synth.py:71-94`）

| 事件 | 来源 | 合成器（`voices.py`） | 要点 |
|---|---|---|---|
| dribble | `ball.py:198-201`，每 0.5 s | `ball_bounce` | 95 Hz 正弦体 + 420/640/1150 Hz 壳体共振 + 1.5–6 kHz 噪声咔嗒 |
| squeak | `motion.py:257-260` 变向/急停 | `squeak` | 2.1–3 kHz 扫频 + 38 Hz 颤动 + 2/3 次谐波 |
| release | 出手 | `swoosh` | 700–3200 Hz 带通噪声 |
| land | 落地 | `ball_bounce(0.6)` 降调 | 送低频尾音 |
| swish | 入网 | `swish` | 2.2–7.5 kHz「嘶」+ 26 个随机绳子颗粒 |
| bounce | 球落地弹跳 | `ball_bounce(strength/3)` | 强度来自物理积分 |
| slap | 击掌 | `slap` | 900 Hz 以上噪声瞬态 + 190 Hz 体 |
| flash | 闪光 | `shutter` | 双击高通噪声，最多 160 次，随机声像，音量 0.04–0.12 |

另外固定位置的声音（`synth.py:96-105`）：蜂鸣前 1.6 s 反向镲风格 `riser`；f312 处 70→28 Hz 次低频 `boom` + 233 Hz 锯齿 + 351 Hz 方波的 `buzzer`；片头和慢动作/定格段 52 Hz 双击 `heartbeat`（定格段更响）。

### 6.2 空间与时间处理

- **距离声延迟**（`synth.py:57-62`）：`d = |事件位置 − 当帧相机位置|`，到达时刻 = `frame_of_tau(τ + d/343)`，增益 `1/(1 + d/6)`。注意延迟加在**故事时间**上，慢动作段里延迟会被拉长（推断：这是有意让慢动作中声音也「变慢」）。参考帖的「约 90 ms」固定延迟在本片被改成了按距离计算。
- **慢动作变调**（`:65-66`）：`pitch = 0.5 + 0.5·clip(当帧速度)`，速度 0.2 时音高约 0.6、时长相应拉长；定格段速度 0，音高 0.5。
- **人群床**（`:107-123`）：`crowd_bed` 用多路粉噪声经随机共振峰带通 + 随机音节包络模拟人声嘈杂；入网前「低语」、入网后「欢呼」两层按帧插值包络；速度 < 1 时用 380 Hz 低通做「闷住」效果；左右声道错开 13 ms 形成宽度。
- **球旋转呼啸**（`:125-133`）：飞行段 180–900 Hz 带通噪声，按后旋相位（2 Hz × 速度）调幅，定格段压到 0.35。
- **音乐垫**（`:135-142`）：入网前 A 小调张力和弦（110/164.81/261.63/329.63 Hz），入网后 C 大调明亮和弦（130.81/196/329.63/392/523.25 Hz）；去谐锯齿波 + 900 Hz 低通。
- **混响与尾音**（`:144-149`）：合成 IR（指数衰减噪声 + 4 个早期反射），RT60 2.3 s；另一条 RT60 3.6 s、180 Hz 低通的「低频滚动」IR，由各声音的 `lo` 发送量驱动。最终 `dry + 0.5·wet + 1.4·rumble`。
- **母带**（`:150-155`）：25 Hz 高通 → 以 99.95 百分位归一到 0.75 → `tanh` 软限幅 → 0.3 s 淡入 / 1 s 淡出 → 16-bit。`post.sh` 另对画面做 0.5 s 淡入、末 1 s 淡出。
- **响度：未记录**（未测 LUFS）；听感检查：未记录。

## 7. 迭代、问题与修正

只有 `plan/00-architecture.md`「实测与决定（迭代记录）」可查，以下为其内容整理（摘录 + 对应源码位置为本次核对）：

| 轮次 | 问题 | 修正 | 当前代码中的体现 |
|---|---|---|---|
| 基准 | 单帧耗时决定分辨率 | M3 Pro 实测 1080p / 40 spp / 自适应 / OIDN 约 8–11 s/帧，保留 1080p | `lights.py:95` 1920×1080 |
| 第 1 轮静帧 | 整体过曝、发粉 | 顶灯能量减半、曝光 −0.6、墙/顶棚压暗 | `lights.py:29` 2400 W、`:106` −0.6、墙 #0b0c10 |
| 第 1 轮静帧 | C1 相机进了看台 | 移到场边 | `cameras.py:50` 机位 y≈−8.8（看台从 y 绝对值 10 m 起） |
| 第 2 轮 | 击掌两手够不着 | 改为前举 138°，让樱木停近 | `poses.py:107` `upperarm_R=(138,…)`；`SAK_PATH` 终点 |
| 第 2 轮 | 运动模糊里人物整圈旋转 | 朝向角逐帧解缠绕 | `motion.py:247-249` |
| 第 2 轮 | 防守者挡住击掌 | 改为垂头走开 | `motion.py:20-24`、`:233` `SLUMP` |
| 第 2 轮 | 观众 1686 个对象带运动模糊，渲染约 45 s/帧 | 观众关闭运动模糊，降到约 8 s/帧 | `crowd.py:86` |
| — | 篮球偏粉 | 压深橙色 | `ball.py:28` #d4581f、缝线 #141010 |

- 「1686 个观众对象」仅见于 plan；代码打印 `[crowd] N fans`，实际数量依赖随机种子 7，无日志可核对。
- 渲染流程中提到的 `render_setup.py -- --still F` 在仓库中不存在，实际单帧检查入口是 `stills.py` + `check.sh`（推断：开发中改名）。
- 用户反馈（记录在 `slamdunk-buzzer-anime/prompt.md`，非本目录）：「挺好的，只是人物不够逼真，还有办法做得更逼真吗？」——这是 v1 之后唯一可查的用户意见，直接促成了 v2 改为卡通渲染路线。

## 8. 可复用制作经验

1. **用「故事时间 τ」解耦动作与剪辑节奏。** 所有运动写成 τ 的函数，慢动作、定格、加速只改一张速度段表，再按帧积分得到 τ(f)；同一张表还能驱动记分牌计时、音高与声延迟。依据：`scene/common.py:17-93`、`audio/synth.py:23,65-66`。
2. **解析求解代替物理模拟来保证「必进」。** 已知出手点、出手与入网时刻，直接解抛体初速度，空心入网可被精确控制；只在入网之后才用简单数值积分做网阻与弹跳。依据：`scene/ball.py:105-145`。
3. **软体用「每帧一个形态键」烘焙。** 篮网形变写成「球位置 → 顶点位置」的纯函数，逐帧生成形态键并以 0/1/0 关键帧切换，可控、可重渲、无需布料缓存。依据：`scene/net.py:53-111`。
4. **一张事件表同时驱动灯光与声音（声画同源）。** 闪光、蜂鸣、刷网、弹跳都在生成画面时写进 `events.json`，音频脚本只读它；改动作后重建即可自动对齐声音。依据：`scene/common.py:96-106`、`build_scene.py:56-60`、`audio/synth.py:19-94`。
5. **先测单帧耗时，再查运动模糊这类「隐形成本」。** 上千个观众对象开运动模糊使单帧从约 8 s 涨到约 45 s，关掉后几乎看不出区别。依据：`plan/00-architecture.md`「实测与决定」、`scene/crowd.py:86`。
6. **烘焙朝向角时做逐帧解缠绕，相机四元数做半球对齐。** 否则 ±π 跳变在运动模糊或插值中会表现为整圈旋转。依据：`scene/motion.py:247-249`、`scene/cameras.py:130-133`。
7. **大量重复对象用「共享网格 + Object Info 颜色 + 少量组 rig」。** 1000+ 观众只需 14 个空物体的动画关键帧。依据：`scene/crowd.py:41-89`。
8. **渲染脚本做成可断点续渲。** `use_overwrite=False` + `use_placeholder=True`，启动前删 0 字节帧，可分段 `render.sh S E` 后台跑。依据：`scene/lights.py:108-110`、`render.sh:7-21`。

## 9. 复现入口

记录的命令与顺序（**本轮未执行**）：

```sh
# 1) 构建工程 + 渲染检查静帧（默认 50% 分辨率、24 spp）
cd rukawa-buzzer-beater
./check.sh 0 150 230 312 414 538 700        # 帧号为本次举例；→ build/rukawa.blend、events.json、stills/
#    等价于：blender -b --factory-startup -P build_scene.py [-- --pct 100 --samples 48]
#           blender -b build/rukawa.blend -P stills.py -- --pct 50 --samples 24 F1 F2 ...

# 2) 全片逐帧路径追踪（可分段、可断点续渲）
PCT=100 SPP=48 ./render.sh            # 或 ./render.sh 0 359 与 ./render.sh 360 719 分批

# 3) 合成音轨 + 合成成片
./post.sh                              # audio/synth.py → build/audio.wav → ffmpeg → output.mp4
```

需要先恢复的东西：

- Blender 5.2（Cycles + Metal GPU）在 PATH 中；脚本使用 `#!/bin/zsh`。
- `build/.venv`：含 NumPy、SciPy 的 Python 虚拟环境（已被清理，版本未记录）；`post.sh` 硬编码 `build/.venv/bin/python`。
- ffmpeg（libx264、aac）。
- `build/` 下所有中间产物均需重新生成；`check.sh` 会把日志写到 `build/build.log`、`build/stills.log`，需先存在 `build/` 目录（目录本身仍在）。
- 随机性：观众 `random.Random(7)`、闪光 `Random(11)`、音频 `default_rng(3)`/`(5)`，固定种子，理论上可逐位复现（推断，未验证）。

改参数的入口：节奏 `common.SEGMENTS`；投篮时刻 `motion.TAKEOFF/RELEASE_T/LANDING`、`ball.T_SWISH`；路径 `motion.RUK_PATH/SAK_PATH/DEF_PATH`；镜头 `cameras.SHOTS` 与 `c1…c9`；画质 `PCT/SPP` 环境变量；色调 `lights.render_settings` 中的曝光与 look。

## 10. 参考来源

| 链接 | 平台 / 作者 | 类型 | 借鉴内容 → 本片对应 | 访问状态 |
|---|---|---|---|---|
| https://x.com/guanmo_ai/status/2118258208045494688 | X / 观默（@guanmo_ai） | 创作参考（技术方法） | 《瓶中风暴》14 秒、Cycles 路径追踪、无外部模型、逐帧烘焙关键帧、发光体同步辅助光、NumPy/SciPy 合成声音与约 90 ms 声延迟、保留资产迭代、原生 .blend → 本片第 2 节映射表中的各项 | 2026-11-12 web_fetch 成功读取帖子文字（页面显示发布于 Nov 5, 2026 8:24 AM，时区未标明）；**视频本体未观看** |

- 用户需求中的「review 这个帖子和视频内容」在当时的执行情况未记录；`prompt.md` 中的「参考帖要点」与本次读取的帖子文字一致。
- 《灌篮高手》作为题材背景（角色名、湘北 SHOHOKU、山王 SANNOH、#11/#10），代码中未引用任何原作图像或素材，仅为题材借用。
- 无技术资料或素材来源链接。

## 11. 证据与限制

**实际读取的文件**（项目相对路径，sha256 前 16 位取自盘点指纹）：

| 文件 | 行数 | sha256 前 16 |
|---|---|---|
| `rukawa-buzzer-beater/prompt.md` | 24 | 6df6173e737153ae |
| `rukawa-buzzer-beater/plan/00-architecture.md` | 71 | 2783439d2d4dc9e8 |
| `rukawa-buzzer-beater/plan/01-timeline.md` | 54 | 2c78d36d10f9914a |
| `rukawa-buzzer-beater/build_scene.py` | 67 | ce885909c4d74a6e |
| `rukawa-buzzer-beater/stills.py` | 37 | 22deccf613c2132a |
| `rukawa-buzzer-beater/check.sh` / `render.sh` / `post.sh` | 8 / 23 / 10 | 31a35dcfd92d9e8f / be10b38589451ef8 / 65e14fc52d0f64ca |
| `rukawa-buzzer-beater/scene/common.py` | 223 | eb16d3d7e995ff56 |
| `rukawa-buzzer-beater/scene/motion.py` | 261 | 494dbf5a43da55c0 |
| `rukawa-buzzer-beater/scene/poses.py` | 137 | f641d8f63139e10d |
| `rukawa-buzzer-beater/scene/figure.py` | 224 | b4510e9b16158fef |
| `rukawa-buzzer-beater/scene/ball.py` | 202 | 386f121fd73588e7 |
| `rukawa-buzzer-beater/scene/net.py` | 117 | 3592c0f4bd190768 |
| `rukawa-buzzer-beater/scene/board.py` | 115 | 00425a8e5a702418 |
| `rukawa-buzzer-beater/scene/cameras.py` | 150 | 0f2ae6e5e7d50a65 |
| `rukawa-buzzer-beater/scene/lights.py` | 110 | bf03e4a5c3d7a9de |
| `rukawa-buzzer-beater/scene/crowd.py` | 164 | 33c4516a69b7fd71 |
| `rukawa-buzzer-beater/scene/court.py` | 234 | 150482c68dc2fb84（已读材质、球线、篮架、看台；`ribbon/arc/rect/box` 辅助函数未逐行读） |
| `rukawa-buzzer-beater/audio/synth.py` | 156 | c3fcda29c0480e53 |
| `rukawa-buzzer-beater/audio/voices.py` | 168 | 27aa120374920ed4 |
| `rukawa-buzzer-beater/output.mp4` | — | 仅 ffprobe 媒体头（30,440,883 字节） |
| `slamdunk-buzzer-anime/prompt.md`（仅引用其中 v1 反馈） | — | b1bca3355765875e |

**缺失 / 无法确认**

- 构建与渲染日志、`events.json`、`rukawa.blend`、渲染帧、`audio.wav` 均已清理：事件数、观众实际数量、闪光次数、球出手参数打印值、每帧实际渲染耗时、总渲染时长无法核对。
- Blender 精确版本、NumPy/SciPy 版本、ffmpeg 版本未记录。
- 采样数：代码默认 48 spp，plan 实测记载 40 spp，最终渲染实际使用哪个未记录。
- 未观看成片画面，未做逐帧视觉检查；未测响度。
- 未找回制作对话；用户对 v1 的意见仅有 `slamdunk-buzzer-anime/prompt.md` 中转述的一句。
