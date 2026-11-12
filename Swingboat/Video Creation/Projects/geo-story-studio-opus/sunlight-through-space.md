---
type: video-production-dossier
project: geo-story-studio-opus
video_id: sunlight-through-space
title: 阳光为什么能穿过太空，温暖地球？
curriculum: script.json 未记录（README 归入“必修第一册 第一章第二节 太阳对地球的影响”）
captured: '2026-11-12'
status: 成片存在（竖屏 1080×1920，85.7 s）；技术检查/视觉检查/用户验收未记录
source_fingerprint: ef2d9f371fab8c1d25288c58dcbf8476e40ea2bdd07d35f5d64210826696fe3c
tags:
- video-creation
- production-capture
- geography-teaching
- canvas-frame-render
- edge-tts
---

# 阳光为什么能穿过太空，温暖地球？ · 四声线“电磁波快递”：介质对比 + 地表吸收与反射（Canvas 逐帧 + 逐词时间戳）

<!-- capture-source-sha256:ef2d9f371fab8c1d25288c58dcbf8476e40ea2bdd07d35f5d64210826696fe3c -->

本文是**根据项目文件做的材料回溯**，依据包括：源码、`script.json`、`build/timeline.json`、`out/` 下的成片与字幕，以及证据包里的 ffprobe 结果和 sha256 清单。证据包中 `host_session` 为空，**未发现制作会话**，制作对话、需求原文和修改过程都无法找回。本次没有运行任何项目脚本（`tts.py`、`render.mjs`、npm、pip 都没跑），也没有打开视频或音频。

## 1. 当前结论与制作路线

**状态：成片存在**，即 `out/阳光为什么能穿过太空温暖地球.mp4` 和同名 `.srt`。目前只有 ffprobe 读到的媒体头信息；视觉检查、试听、响度、用户验收和发布情况都**未记录**。

| 项 | 值 | 来源 |
| --- | --- | --- |
| 分辨率 / 画幅 | 1080×1920，9:16 竖屏 | ffprobe；`lib.mjs:7` |
| 帧率 / 帧数 | 30 fps；`Math.round(85.718 × 30)` = 2572 帧 | ffprobe；按时长推算 |
| 时长 | 85.733 s（timeline 85.718 s） | ffprobe；`build/timeline.json` |
| 视频编码 | H.264，yuv420p，CRF 19，preset medium，`+faststart` | ffprobe；`render.mjs` |
| 音频编码 | AAC 48 kHz 立体声，192 kbps | ffprobe；`render.mjs` |
| 文件大小 | 23,337,322 字节（约 23.3 MB，平均约 2.18 Mbps） | 证据包 |
| 字幕 | `.srt` 47 条（2706 字节）；画面另有烧录字幕 | `timeline.json` cues |
| 其他版本 | 无 | `out/` 列表 |

文件时间：`script.json` 10-21 16:57，TTS 产物和 `render.mjs` 16:59，成片 17:10。时间上排在 [[sun-energy]]（10-20）之后、[[sun-angle]]（10-21 20:44）之前。git 只有初始提交 `4831a07`。

一句话管线：

> `script.json`（25 段：旁白 + 小宇 + 太阳 + 地球）→ `tts.py`（edge-tts + WordBoundary）→ `timeline.json` / `narration.wav` / SRT → `render.mjs` 动态加载 `scenes-a…d.mjs`（7 个场景）+ `sun.mjs`（沿用 [[sun-energy]]）+ 新增 `waves.mjs` → 程序音频 → 用 ffmpeg 管道合成 MP4。

## 2. 教学目标与课标对齐

- **curriculum 字段：缺失**。README 归类为“必修第一册 第一章第二节 太阳对地球的影响”，简介是“太阳辐射以电磁波形式传播，不需要介质就能穿过太空”。课标条目**未记录**。
- **核心知识点**：太阳辐射以电磁波的形式传递能量，电磁波传播不需要介质，所以能穿过真空；声音则要靠空气振动，在真空中传不出去。到达地球的太阳辐射有一部分穿过大气照到地面，被地表吸收后地表升温；同时也有一部分被云或地表反射回太空，还有一部分在途中被大气吸收。
- **叙事结构**：
  - 导入：小宇提出“热空气大桥”的误解，旁白回应“没有这样的桥”，太阳说“我用的是电磁波快递”。
  - 原理：先回顾上一期讲的太阳辐射，再说明它是电磁波；用声音对比介质（空气中能传、真空中不能传）；电磁波穿过真空。
  - 迁移：地面吸收太阳辐射后升温（晒过的沙滩），但并非全部被吸收（反射、大气吸收）。
  - 总结：“太阳辐射 → 穿过太空 → 部分被地表吸收”，“这是太阳温暖地球的一个重要过程”。
- **科学边界**：
  - 结论用的是“**一个**重要过程”“部分被地表吸收”。
  - 讲反射和大气吸收的分流场景用了四条等宽通道。代码注释写明：“four equal lanes — the number of lanes does not represent shares of energy”（`scenes-c.mjs:103`），也就是通道数量不代表能量占比。
  - 文案没有提到大气的**散射**作用，也没有给出反射率或吸收比例的数字（这是内容取舍，不是错误）。
- **数字**：文案中没有具体数值。没有 `numbers` 字段。
- **based_on（推断）**：`source` 中写的是“douyin-sunlight-through-space v002 分镜”，对应路径 `geo-story-studio/storage/builds/douyin-sunlight-through-space/v002/`，本次未核对。

## 3. 技术与依赖

| 类别 | 内容 | 证据 |
| --- | --- | --- |
| 渲染 | `@napi-rs/canvas`，声明 `^1.0.11`，**lock 锁定 1.0.13**（darwin-arm64 同版本）。前三片锁定的是 1.0.11 | `package.json`；`package-lock.json` |
| npm scripts | 无 | `package.json` |
| TTS | `edge-tts` + `aiohttp`，没有 requirements 文件 | `tts.py` |
| 字体 / ffmpeg | 与 [[nebula-nursery]] 相同 | `fonts.py`；`tts.py:176-190`；`render.mjs:149-158` |
| 本地环境（非声明） | `.venv` 中可见 `edge_tts-7.3.1`、`fonttools-4.67.0` | 目录列表 |

**声线**（全局 `rate: 4`）：

| 角色 | voice | 实际 rate / pitch | 字幕胶囊 / 字色 | 句数 |
| --- | --- | --- | --- | --- |
| 旁白 `zh` | zh-CN-XiaoxiaoNeural | +4% / +0 Hz | 无 / 白 | 15 |
| 小宇 `kid` | zh-CN-YunxiaNeural | +10% / +8 Hz | “小宇” / `#c9f1ff` | 7（开场 3 句都是小宇说的） |
| 太阳 `sun` | zh-CN-YunjianNeural | +4% / −4 Hz | “太阳” / `#ffe7a8` | 2 |
| 地球 `earth` | zh-CN-XiaoyiNeural | +8% / +4 Hz | “地球”（薄荷绿）/ `#c8f7e1` | 1（“叮咚，快递到啦！”） |

## 4. 镜头与时间轴

| 场景 id | 时间（约） | 旁白要点 | 画面 | 模块 |
| --- | --- | --- | --- | --- |
| `hook` | 0–13.8 s | 隔着太空，热怎么送来？热空气大桥？没有这座桥；电磁波快递 | 太阳和地球之间搭起一座由热气块组成的拱桥，随后坍塌，盖章“没有这座桥” | `scenes-a.mjs` |
| `em` | 13.8–20.9 s | 上一期讲过太阳辐射；以电磁波形式带走能量 | 标签“上一期：太阳为什么能发光发热”，波纹环 `ringTrain` 持续向外扩散 | `scenes-a.mjs` |
| `medium` | 20.9–34.3 s | 声音靠空气振动；真空中传不出去；电磁波不需要介质 | 上下两块面板对比“空气 / 真空”，喇叭发出的弧形声波在真空中被打叉 | `scenes-b.mjs` |
| `cross` | 34.3–42.6 s | 穿过真空送向地球；小宇冲浪；地球说快递到啦 | 太阳发出扇形光波，其中一条射向地球，能量点沿波前进，小宇在波上冲浪 | `scenes-b.mjs` |
| `ground` | 42.6–54.3 s | 部分穿过大气照到地面；地表吸收升温；沙滩暖乎乎 | 从太空到大气层顶（虚线）再到天空和沙地，沙子颜色随 `warm` 变暖，出现“获得能量 +”“升温”标签 | `scenes-c.mjs` |
| `split` | 54.3–67.1 s | 并非全部吸收：云和地表反射，大气吸收；地表只吸收一部分 | 四条通道：云反射、大气吸收、地面吸收、冰雪地面反射；最后其余三条变暗，盖章“地表吸收” | `scenes-c.mjs` |
| `sum` | 67.1–85.7 s | 记住这条路线；不用修路照样送到；阳光就是电磁波 | 三步路线卡，盖章“照样送到！”，告别 | `scenes-d.mjs` |

## 5. 关键算法与实现

> 先区分两件事：**视频讲的科学**是“电磁波不需要介质、声音需要介质”以及“地表吸收、反射和大气吸收”；**做视频用的算法**是行进正弦波、波上取点、二次贝塞尔拱桥和通道分流。画面中的波长、振幅只为好看，没有对应真实频率。

### 5.1 行进光波 `lightWave()`（`sun.mjs:181-195`，与 [[sun-energy]] 共用）

```js
export function lightWave(ctx, t, x1, y1, x2, y2, o = {}) {
  const { p = 1, amp = 22, wl = 70, color = C.energy, w = 8, a = 1 } = o;
  const L = Math.hypot(x2 - x1, y2 - y1), ang = Math.atan2(y2 - y1, x2 - x1);
  ctx.translate(x1, y1); ctx.rotate(ang);
  ...
  for (let s = 0; s <= L * p; s += 6) ctx.lineTo(s, Math.sin((s / wl) * TAU - t * 9) * amp);
```

- 先旋转坐标系，再沿 x 轴画 `sin(2πs/λ − 9t)`，波就会沿传播方向前进。`p` 控制波头已经走到哪里，由“穿过”“地球”这些词的时间戳驱动（`scenes-b.mjs:111-112`）。
- `waves.mjs:46-50` 的 `wavePoint()` 使用相同的相位公式，算出波上任意比例处的坐标。4 个能量点和冲浪的小宇都“骑”在这条波上（`scenes-b.mjs:117-122`）。

### 5.2 热空气拱桥的搭建与坍塌（`scenes-a.mjs:11-40`）

```js
const pt = (f) => [(1 - f) ** 2 * x1 + 2 * (1 - f) * f * cx + f * f * x2,
                   (1 - f) ** 2 * y1 + 2 * (1 - f) * f * cy + f * f * y2];
...
const drop = Math.max(0, fall - rnd(i) * 0.4) ** 2 * 1400;
y += drop; x += (rnd(i + 7) - 0.5) * drop * 0.4;
```

20 块“热气砖”沿二次贝塞尔曲线排成拱桥。`fall` 增大时，每块砖按各自的随机延迟以平方关系下落，同时横向偏移并旋转，看起来像先后崩落。这是在用画面否定“热要靠空气传过来”这个误解。

### 5.3 介质对比与真空场景（`scenes-b.mjs:10-96`）

- `soundArcs(reach)` 让 4 道弧形声波循环向外扩散，`reach` 决定能传多远。“真空”面板的传播距离被限制住，同时喇叭被打上红叉（`bigCross`，来自 `waves.mjs:107`）。两块面板分别位于 `AIR_Y=470` 和 `VAC_Y=990`，上下对照。
- `cross` 场景里，太阳先向 5 个方向（`FAN`）发出淡色光波，表示“向四面八方辐射”；随后只跟随射向地球的那一条。代码注释写道：“radiation leaves in all directions; we follow the part headed for Earth”。

### 5.4 地面世界与能量分流（`scenes-c.mjs:11-155`）

- `world(warm, snow)`：竖向渐变依次为太空、大气层顶（虚线，随时间流动）、天空，下方是波浪形沙地。沙子的 RGB 随 `warm` 插值变暖，用来表现“升温”。
- `splitScene`：4 条等宽通道。云和冰雪地面两条通道用 `lightWave` 向右上方“反弹回太空”，并配 `burst` 粒子效果。说到 `part` 时，地面以外的通道透明度降到 35%，突出“地表只吸收其中一部分”。

### 5.5 共用机制

- `render.mjs` 结构与 [[sun-energy]] 相同（动态加载、字幕、logo、交叉淡化）。背景在 `atmo` 到 `butNot` 之间变暖（`render.mjs:29-32`）；从 `sum` 起，页脚淡入“资料：NASA Science · NASA Earth Observatory”。
- `lib.mjs`、`cast.mjs`、`props.mjs`、`sun.mjs` 都和 [[sun-energy]] 完全相同。`waves.mjs`（`dca3d3afaeac`，包括 `waveRing/ringTrain/energyDot/wavePoint/cloudBuddy/bubble/thought/bigCross`）在本片首次出现，[[sun-angle]] 原样复用。
- `tts.py` 与 [[sun-energy]] 相比只改了 `TITLE` 和 `TARGET`（95.0）。

## 6. 声音与字幕

- 四种声线的参数见 §3。`LEAD_IN 1.0`，各段 `gap` 为 0.2–0.45 s，`TAIL 2.8`。时间轴中 `rate: 4`，片长 85.7 s，没有超过 95 s 的 TARGET，**未提速**。
- 混音：TTS 阶段依次是 `adelay → amix → highpass 80 → loudnorm I=-16/TP=-1.5/LRA=11`；成片阶段加入配乐（0.55，按旁白 sidechain 压低）和音效（0.5），再过 `alimiter 0.93`。实测响度**未记录**。
- 缓存：`build/tts_cache/` 里有 50 个文件，正好是 25 段 × 2。
- 字幕：时间取自逐词时间戳，四个角色的字幕胶囊和字色各不相同（`render.mjs:62, 86`）。

## 7. 迭代与问题

1. **没有片尾品牌卡**：`tts.py:35` 的注释写着“room for the closing brand card”，但没有 `outro()`。`sum` 场景 `to: Infinity`，结尾是“照样送到！”印章，最后淡出到黑。这不符合现行 AGENTS.md §4。
2. **`scenes-d.mjs` 注释和实际不一致**：文件头写的是“Scenes 7–8: the three-step route, then the Sun's farewell…”，但只导出了一个 `sum` 场景（`scenes-d.mjs:76-78`），路线和告别是在同一场景里分时段完成的。
3. **lock 版本变了**：本片和 [[sun-angle]] 的 canvas 锁定为 1.0.13，前三片是 1.0.11，而 `package.json` 的声明相同（`^1.0.11`）。原因**未记录**（**推断**：在新目录重新执行了 `npm install`）。
4. **`script.json` 缺少字段**：`curriculum`、`based_on`、`numbers`（§6）。
5. **`TARGET = 95.0`**，和现行规则（默认 None）不同，但实际没有提速。
6. **系列衔接**：文案里有“上一期讲到”，画面上有“上一期：太阳为什么能发光发热”标签（`scenes-a.mjs:104-108`），说明前一集是 [[sun-energy]]。单独发布时，观众可能没看过“上一期”（**推断**，没有记录说明如何发布）。
7. **符合 §5**：画面上没有“示意”字样。
8. README 目录已收录（第 34 行，1:26）。

## 8. 复现入口

本次**没有运行**。以下顺序来自 AGENTS.md 的“常用命令”和文件头注释：

```bash
python3 -m venv .venv && .venv/bin/pip install edge-tts   # fonts.py 还需要 fonttools（未声明）
npm install                                               # @napi-rs/canvas（lock 1.0.13）
.venv/bin/python tts.py            # → build/seg*.mp3, narration.wav, timeline.json；out/阳光为什么能穿过太空温暖地球.srt
node render.mjs --still 28 60      # → build/still_<t>.png
node render.mjs                    # → build/music.wav, sfx.wav；out/阳光为什么能穿过太空温暖地球.mp4
```

字体依赖 macOS 系统资源。

## 9. 参考来源

`script.json` 和代码里**没有 URL**（证据包里 `urls` 为空）。资料名来自 `source` 字段：

- NASA Science: Sun: Facts，太阳辐射（项目文件中提及，本次未访问）
- NASA Science: Anatomy of an Electromagnetic Wave，电磁波不需要介质（项目文件中提及，本次未访问）
- NASA Science: Basics of Space Flight Ch.6，电磁辐射（项目文件中提及，本次未访问）
- NASA Science: The Earth's Radiation Budget，地球辐射收支（项目文件中提及，本次未访问）
- NASA Earth Observatory: Climate and Earth's Energy Budget，反射与吸收（项目文件中提及，本次未访问）
- 分镜来源：`geo-story-studio/storage/builds/douyin-sunlight-through-space/v002/`（路径为推断，本次未访问）

## 10. 证据与限制

| 文件 | sha256 前 12 位 | 说明 |
| --- | --- | --- |
| `script.json` | `70ae499143b0` | 25 段，四种声线 |
| `tts.py` | `32f7a411ea3a` | TARGET 95 |
| `render.mjs` | `19de3cedf34b` | 四种字幕胶囊，暖色背景 |
| `sun.mjs` / `waves.mjs` | `210e265385b4` / `dca3d3afaeac` | 光波、太阳、地球；波纹、云朵、气泡 |
| `lib.mjs` / `cast.mjs` / `props.mjs` | `7d757aa5fc33` / `e7f29e66c6d0` / `4debdbd1b652` | 与 [[sun-energy]] 相同 |
| `scenes-a…d.mjs` | `88b929bd0e10` / `5321b6bda932` / `216dcf230aad` / `1319fe5fb23c` | 7 个场景 |
| `audio.mjs` / `fonts.py` | `f9e04b2cfd54` / `a6ac7264e548` | 5 片相同 |

- **已读**：`script.json` 全文，`tts.py` 和 `render.mjs` 的 diff，`sun.mjs`/`waves.mjs` 的关键函数，各场景文件的结构和关键段落，`timeline.json` 的段落时间。
- **未读、未做**：媒体文件、依赖目录、缓存内容；没有核对源 build 和 NASA 资料。
- **缺失**：制作会话、需求原文、审片记录、发布状态、模型信息、ffmpeg 版本、实测响度。
