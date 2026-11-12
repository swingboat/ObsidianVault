---
type: video-production-dossier
project: geo-story-studio-opus
video_id: ship-floats-higher
title: 同一艘船，进海后为什么浮得更高？
curriculum: 人教版必修第一册 第三章第二节 海水的性质（海水的密度及其对航运的影响）；课标 1.6；衔接初中物理阿基米德原理
captured: '2026-11-12'
status: 成片存在（竖屏 1080×1920，212.8 s）；技术检查/视觉检查/用户验收未记录；未收入 README 目录
source_fingerprint: ac276096e759991facee5e86f63624dc3894899c2d1d5a4ee636c997e872c4c1
tags:
- video-creation
- production-capture
- geography-teaching
- canvas-frame-render
- edge-tts
---

# 同一艘船，进海后为什么浮得更高？ · 会说话的货船“大福号” + 按米建模的船体与吃水放大镜 + 方箱船算例

<!-- capture-source-sha256:ac276096e759991facee5e86f63624dc3894899c2d1d5a4ee636c997e872c4c1 -->

本文是**根据项目文件做的材料回溯**（`script.json`、`tts.py`、`render.mjs`、`lib.mjs`、7 个场景模块与 `ship/cast/props/audio.mjs`、`build/timeline.json`、证据包的 ffprobe 与 sha256 清单）。证据包映射到宿主会话 `9ad05fe4…`（创建于 2026-11-01 07:44），只按元数据找到，宿主返回空 transcript，**对话内容不可读**；映射依据是会话创建时间早于 `script.json` 数分钟，属**推断**。本次未运行任何项目脚本，也未打开音视频。

## 1. 当前结论与制作路线

**状态：成片存在**——`out/同一艘船，进海后为什么浮得更高.mp4` + 同名 `.srt`。只有 ffprobe 媒体头信息；画面、试听、响度、用户验收与发布都**未记录**。**项目 README.md 的视频目录里没有这一行**（检索 `ship-floats` 无结果）。

| 项 | 值 | 来源 |
| --- | --- | --- |
| 分辨率 / 画幅 | 1080×1920，9:16 竖屏 | ffprobe；`lib.mjs:7` |
| 帧率 / 帧数 | 30 fps；`round(212.757×30)` = 6383 帧 | ffprobe；`render.mjs:181`（推算） |
| 时长 | 212.767 s（timeline 212.757 s） | ffprobe；`build/timeline.json` |
| 视频编码 | H.264，yuv420p，CRF 19，preset medium，`+faststart` | ffprobe；`render.mjs:178-179` |
| 音频编码 | AAC 48 kHz 立体声，192 kbps | ffprobe；`render.mjs:179` |
| 文件大小 | 29,538,082 字节（约 29.5 MB） | 证据包 |
| 字幕 | `.srt` 110 条，6421 字节；画面烧录字幕 + 角色标签 | 证据包；`render.mjs:82-108` |
| 其他版本 | 无 | `out/` 列表 |

文件时间：`.srt` 2026-11-01 07:58，成片 08:15。git：证据包记录源文件属于 `8cbe673 2026-11-01 feat(videos): add 21 high-school geography lessons`。

一句话管线：

> `script.json`（40 段，3 个声线角色，`display` 数字替换表）→ `tts.py`（edge-tts + WordBoundary）→ `timeline.json` / `narration.wav` / SRT → `render.mjs` 加载 `scenes-a…g.mjs`（9 个场景），船体、水体、放大镜由 `ship.mjs` 按“米”建模 → `audio.mjs` 合成配乐与汽笛等音效 → RGBA 帧管道送 ffmpeg。`render.mjs`、`lib.mjs` 与 [[seawater-salinity]] 同一模板。

## 2. 教学目标与课标对齐

- **curriculum**：人教版必修第一册 第三章第二节 海水的性质（海水的密度：概念、影响因素，对航运的影响）；课标 1.6；浮力原理衔接初中物理（阿基米德原理）。
- **核心知识点**：海水密度大于淡水 → 同一艘船从江入海，排水体积变小、吃水变浅；浮力不变。
- **讲解结构（按实际文案）**：
  1. 导入：小林看到大福号从长江开进东海“往上浮了一截”；大福号：“一吨货都没卸，怎么像变轻了？”→“船没有变轻，变的是它脚下的水。”
  2. 原理：吃水定义 → 漂浮平衡（浮力 = 总重）→ 阿基米德原理 → 船有多重就排开多重的水 → 密度：同样 1025 吨水，淡水占 1025 m³，海水只占 1000 m³。
  3. 辨析易错点：小林“海水浮力更大，把船托得更高”→“停！”——刚入海瞬间浮力大于重力，船上浮后浮力回落，漂稳后江海里浮力一样大，变小的是排水体积和吃水。
  4. 定量：方箱船算例，吃水 = 排水体积 ÷ 水线面积：1025 ÷ 250 = 4.1 m，1000 ÷ 250 = 4.0 m，浮高 0.1 m（10 cm）；“真实的船还要看船形和水的实际密度”。
  5. 迁移：反向从海入江吃水变深；载重线标志 F（淡水）高于 S（夏季）；海水密度还受温度、盐度、压力影响。
  6. 总结与考法：“海水密度大，排水体积小，吃水变浅；浮力不变，不是变大”。
- **科学边界**：`source` 写明改编自 Geo Story Studio `douyin-ship-draft v002` 分镜与科学审校（推断路径 `geo-story-studio/storage/builds/douyin-ship-draft/v002`，本次未读取；目录名没有沿用源 build 名 `ship-draft`，与 AGENTS.md 第 1 节的命名规则不一致）。`facts` 引用审校 claim-004、005、006、008。`notes` 明确排除：死海漂浮、潜艇掉深、直布罗陀密度流（审校标为证据不足的旁支）；载重线只讲 F 与 S 的相对位置，不解释 T、W、WNA；长江口用“江面变海面”的场景表现，**不画地图**。
- **事实与数字**：

| 表述 | 出处（script.json 写法） |
| --- | --- |
| 吃水 = 水面到船底的垂直距离 | v002 审校 claim-005；MMA MT-2141 |
| 漂浮平衡：浮力 = 总重；阿基米德原理 | OpenStax University Physics Vol.1 §14.4；UTK Buoyancy |
| 海水溶解盐类，表层海水密度一般大于淡水 | 人教版必修一 3.2；Univ. of Hawai‘i CRDG |
| 淡水 1000、海水 1025 kg/m³ | 自设算例取值（表层海水一般约 1020～1030 kg/m³），非通用常数 |
| 总质量 1025 t，水线面积 250 m²（25 m × 10 m 直壁方箱船） | v002 ship-draft-model.json 自设算例 |
| 1025 → 1000 m³；4.1 → 4.0 m；浮高 0.1 m | V = m ÷ ρ，d = V ÷ A 复算 |
| 载重线；F 在 S 上方，间距为淡水宽限 FWA | IMO《International Convention on Load Lines, 1966》附则 I 第 6 条；MMA MT-2141 |

以上出处本次均未核对。

## 3. 技术与依赖

| 类别 | 内容 | 证据 |
| --- | --- | --- |
| 渲染 | Node.js ESM + `@napi-rs/canvas` `^1.0.11`（lock 解析 1.1.0）；无 npm scripts | `package.json`；`package-lock.json` |
| TTS | Python `edge-tts` + `aiohttp`；版本未记录 | `tts.py` |
| 字体 | `fonts.py`（fontTools）；`lib.mjs` 缺字体时自动调用 | `fonts.py`；`lib.mjs` |
| 编码 / 混音 | 系统 ffmpeg；版本未记录 | `tts.py`；`render.mjs:170-180` |

声线（全局 `rate: 4` 叠加；`timeline.json` 记录 `rate: 4`）：

| key | 标签 | voice | 角色 rate / pitch | 实际 rate | 段数 |
| --- | --- | --- | --- | --- | --- |
| `zh` 旁白 | — | `zh-CN-XiaoxiaoNeural` | 0 / 0 | +4% | 29 |
| `stu` | 小林 | `zh-CN-YunxiNeural` | +6 / +2 Hz | +10% | 5 |
| `ship` | 大福号 | `zh-CN-YunjianNeural` | +2 / −2 Hz | +6% | 6 |

## 4. 镜头与时间轴

时间来自 `timeline.json`；`XF = 0.4`。7 个模块、9 个场景。

| # | 场景 id | 约时间 (s) | 口播要点 | 画面 | 模块 |
| --- | --- | --- | --- | --- | --- |
| 1 | `voyage` | 0–18.6 | 从长江开进东海，船自己浮高 | 视差岸线 + 路牌，江水黄褐渐变为海蓝，船头放大镜里金色漆线露出水面 | `scenes-a.mjs` |
| 2 | `draftDef` | 18.6–29.6 | 吃水定义；江→海吃水变浅 | 船侧尺寸线“吃水” | `scenes-b.mjs:7-65` |
| 3 | `buoyancy` | 29.6–54.9 | 浮力 = 总重；阿基米德原理；1025 吨 | 受力箭头、排开的水块 | `scenes-b.mjs:66-169` |
| 4 | `density` | 54.9–86.1 | 密度；1025 吨水：1025 m³ vs 1000 m³ | 两块水体体积对比 | `scenes-c.mjs` |
| 5 | `mistake` | 86.1–119.6 | 误区“浮力更大”；入海瞬间→上浮→再平衡；前后对照表 | 吃水先停在 4.1 m，再阻尼回弹到 4.0 m；放大镜；对照表 | `scenes-d.mjs` |
| 6 | `calc` | 119.6–147.8 | 方箱船：d = V ÷ A；4.1 / 4.0 / 0.1 m | 25 × 10 × 6 m 箱船与算式 | `scenes-e.mjs` |
| 7 | `reverse` | 147.8–155.1 | 从海入江吃水变深 | 海蓝→江黄，放大镜水面上升 | `scenes-f.mjs:7-33` |
| 8 | `loadLine` | 155.1–187.6 | 载重线；F 高于 S；看实际密度 | 船舷钢板上的圆盘 + 横杠 + TF/F/T/S/W/WNA 线，F、S 高亮 | `scenes-f.mjs:34-162` |
| 9 | `summary` | 187.6–212.8 | 一句话总结；考法；大福号谢幕 | 总结卡；210.1 s 起品牌卡“用地理的眼光，读懂船身上的水线” | `scenes-g.mjs`；`render.mjs:63-77` |

## 5. 关键算法与实现

**视频讲的原理**：漂浮平衡、阿基米德原理、ρ = m / V、方箱船 d = V / A、载重线。**做视频用的算法**如下。

### 5.1 以“米”为单位的船体与放大镜（`ship.mjs:1-8, 62-80, 153-196`）

船体局部坐标用米：`HULL_L = 25, HULL_D = 6`，船底 = 水面 y + `draft × k`，`X(u)`、`Y(v)` 把米换成像素，所以同一个 `draft` 数值同时驱动整船和尺寸线：

```js
export function cargoShip(ctx, t, x, wl, k, draft, o = {}) {
  const keel = wl + draft * k, left = x - (HULL_L / 2) * k;
  const X = (u) => left + u * k, Y = (v) => keel - v * k;
```

0.1 m 的差在整船尺度上只有几个像素，所以加了 `draftLens` 放大镜：镜内每米 560–1100 px，画 3.7–4.4 m 刻度和一条 4.1 m 的金色虚线漆线（淡水水位），看海水水面从漆线处退到 4.0 m。

### 5.2 江海交界的颜色扫过与吃水跟随（`scenes-a.mjs:8-12, 49`；`ship.mjs:8-9`）

`RIVER = [150,118,62]`（黄褐江水）与 `SEA = [28,110,190]`，`seaAt(xw)` 在世界坐标 `FRONT = 1030 ± 200` 的锋面带里 0→1，摄像机 `cam(t) = t × SPEED` 平移，水色按像素列 `mix(RIVER, SEA, …)` 插值；注释：“draft follows the water under the bow with a little lag (1025 t box ship: 4.1 m fresh → 4.0 m sea)”。反向一幕（`scenes-f.mjs:7-33`）把 `SEA` 与 `RIVER` 对调。

### 5.3 “浮力先变大、再回到等于重力”的阻尼回弹（`scenes-d.mjs:10-16`）

```js
// draft after entering the sea: held at 4.1 m, then a damped bob that settles at 4.0 m
function draftAt(t) {
  const tR = at('mis2', '往上浮');
  if (t < tR) return 4.1;
  const u = t - tR;
  return 4.0 + 0.1 * Math.exp(-1.1 * u) * Math.cos(2.3 * u);
}
```

上浮起点卡在“往上浮”一词；衰减振荡表现“上浮—浮力变小—重新漂稳”，而不是一步跳到 4.0 m。随后的对照表在“排水体积”等词上逐行出现（`1025 m³ → 1000 m³ 变小` 等）。

### 5.4 字幕数字替换表 `display`（`tts.py:123-127, 181`）

旁白保留汉字读法（“一千零二十五”“四点一”“除以”），字幕用 `script.json` 的 `display` 表替换成 `1025`、`4.1`、`÷`：

```python
def show(text: str, display) -> str:
    """Spoken Chinese numerals stay in the TTS text; subtitles show digits/symbols (script.json "display")."""
    for a, b in display:
        text = text.replace(a, b)
    return text
```

这是本项目里第三种“读汉字、显示数字”的写法：[[sea-to-mountain-rain]] 用逐条 `["显示","口播"]` 对；[[seawater-temperature]] / [[seawater-salinity]] 没有。全局替换顺序敏感（“一千零二十五”必须排在“一千”之前，表里确实如此）。

### 5.5 载重线标志（`scenes-f.mjs:34-50`）

`LINES` 以 S 为 0 写出 TF −110、F −70、T −40、S 0、W +40、WNA +80（像素偏移，负为上方），圆盘中心 `CX = 340, CY = 960`；按 `notes` 只高亮 F 与 S。偏移量是造型用值，不是公约尺寸。

## 6. 声音与字幕

- **旁白**：40 段约 1003 字；`LEAD_IN 0.8`、`TAIL 2.8`；混音链与模板相同（`adelay → amix → highpass 80 Hz → loudnorm I=-16:TP=-1.5:LRA=11`）。
- **配乐**（`audio.mjs:183-`）：程序合成，**104 BPM，G–Em–C–D**——与海水温度 / 盐度两片的 112 BPM C–G–Am–F 不同。
- **音效**：在复制来的 FX 表上新增汽笛（两路失谐锯齿波 + 缓起，`audio.mjs:164`），滑哨可上行（`o.up`）；7 个模块约 95 个事件 + 自动 whoosh。
- **总混音**：配乐 ×0.5 侧链闪避，音效 ×0.5，`alimiter=0.93`（`render.mjs:175-176`）。
- **字幕**：`SUB_Y = 1560`；标签小林（天蓝）/ 大福号（金）；数字经 `display` 替换；品牌卡后停止。资料行“资料：人教版地理必修第一册 · OpenStax · IMO《国际载重线公约》”从 `sum` 段显示。

## 7. 迭代与问题

- **README 目录缺失**：README.md 的视频目录里没有本片（同批的 [[seawater-temperature]]、[[seawater-salinity]] 都已收录）。
- **目录名与源 build 名不一致**：源是 `douyin-ship-draft`，按 AGENTS.md 第 1 节应命名为 `ship-draft/`；实际为 `ship-floats-higher/`。原因**未记录**。
- **没有 `based_on` 字段、没有 URL**：来源写在 `source` 文本里。
- **全局语速 +4%**：`rate: 4`，`TARGET = None`；原因**未记录**。
- **复制遗留**：`audio.mjs` 仍带声呐、“咕咚”、水下咕噜等海水温度片的音效（推断沿用）；`cast.mjs`、`props.mjs` 已精简（头注释分别为“小林 + face() + pill”与“titles, tags, stamps, headings and small badges”）。
- **学科边界**：本片主体是初中物理浮力，地理部分落在“海水密度对航运的影响”。`curriculum` 已写明“衔接初中物理”，`notes` 删掉证据不足的旁支——范围控制有记录；是否偏离 AGENTS.md 第 0 节“只做高中地理”未见讨论。
- **遵守 AGENTS 的证据**：无“示意 / 不按比例”字样（`render.mjs:110` 注释）；长江口不画地图；logo、品牌卡、`TAIL 2.8` 符合第 4 节；`build/` 无残留预览图。
- 宿主会话不可读，修改轮次与用户反馈**无法确认**。

## 8. 复现入口

本次**均未运行**。按 AGENTS.md“常用命令”（在 `ship-floats-higher/` 内）：

```bash
python3 -m venv .venv && .venv/bin/pip install edge-tts   # fonts.py 另需 fontTools（AGENTS 未列出）
npm install                                               # @napi-rs/canvas；无 npm scripts
.venv/bin/python tts.py            # → build/seg*.mp3、tts_cache/、narration.wav、timeline.json；out/<标题>.srt
node render.mjs --still 10 105     # → build/still_<t>.png（用完删除）
node render.mjs                    # → out/同一艘船，进海后为什么浮得更高.mp4
```

## 9. 参考来源

`script.json` **没有 URL**，只有文字出处：人教版地理必修第一册 第三章第二节；OpenStax University Physics Vol.1 §14.4；UTK Buoyancy；University of Hawai‘i CRDG《Density, Temperature, and Salinity》；Massachusetts Maritime Academy MT-2141 Ship's Construction；IMO《International Convention on Load Lines, 1966》附则 I 第 6 条；源 build 的 v002 审校 claim-004/005/006/008 与 `ship-draft-model.json`。均为**项目文件中提及，本次未访问**。

## 10. 证据与限制

| 文件 | 行数 | sha256 前 12 位 | 用途 |
| --- | --- | --- | --- |
| `script.json` | 432 | `54468e2512d0` | 全文已读 |
| `tts.py` | 216 | `ec48aac67bb1` | 与 seawater-temperature 版 diff（新增 `show()`） |
| `render.mjs` | 192 | `c83d760cf730` | 与 seawater-salinity 版 diff |
| `lib.mjs` | 225 | `0365603bacac` | diff（仅首行注释不同） |
| `ship.mjs` | 222 | `d585a1f342f7` | 船体、水体、放大镜 |
| `scenes-a…g.mjs` | 163/194/177/122/154/188/110 | `3521402c925a` / `a51b1e2d46a7` / `6b823c45c546` / `a627153bad3d` / `ac3fc0655a2d` / `3ce16496108c` / `67664925996c` | 头注释、场景表、关键函数 |
| `cast.mjs` / `props.mjs` / `audio.mjs` | 134/174/238 | `743a0393e33b` / `c9dae5c7464a` / `ad11fc13b222` | 导出清单、注释、配乐参数 |

- 已读：上表（大文件按段）、`build/timeline.json`、`package-lock.json`、README.md 目录检索。
- 未读 / 未做：`node_modules/`、`.venv/`、`tts_cache`、音视频；未运行脚本；未访问外部资料；未读取源 build。
- 缺失：会话内容、需求原文、验收记录、工具版本、成片实测响度、未收入 README 的原因。
