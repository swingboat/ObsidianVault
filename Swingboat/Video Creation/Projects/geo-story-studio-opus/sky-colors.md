---
type: video-production-dossier
project: geo-story-studio-opus
video_id: sky-colors
title: 天空为什么是蓝色，晚霞却偏红？
curriculum: 人教版必修第一册 第二章第二节 大气受热过程（大气对太阳辐射的削弱作用：散射；太阳高度与大气削弱）；课标 1.4
captured: '2026-11-12'
status: 成片存在（竖屏 1080×1920，230.3 s）；技术检查/视觉检查/用户验收未记录
source_fingerprint: 58fe905684dea7ca4765c35c36f37e3c1acb2178b350fef2092adaf92da05022
tags:
- video-creation
- production-capture
- geography-teaching
- canvas-frame-render
- edge-tts
---

# 天空为什么是蓝色，晚霞却偏红？ · 拟人光“小红/小蓝”穿越大气弹珠台（Canvas 逐帧 + 四角色 edge-tts）

<!-- capture-source-sha256:58fe905684dea7ca4765c35c36f37e3c1acb2178b350fef2092adaf92da05022 -->

本文是根据项目文件做的材料回溯（`script.json`、`tts.py`、`render.mjs`、`lib.mjs`、`cast.mjs`、`props.mjs`、`audio.mjs`、5 个 `scenes-*.mjs`、`build/timeline.json`、`out/` 文件列表与 ffprobe 结果）。宿主环境中按元数据找到一个可能对应的制作会话 `1b988ab8-2bf3-411f-8276-31a139f8a426`（创建于 2026-10-31 19:55），但**读不到对话内容**（宿主返回空 transcript），对应关系只是根据时间戳**推断**：会话创建后约 5 分钟，`script.json` 于 19:59:51 落盘。本次没有运行任何项目脚本。

## 1. 当前结论与制作路线

**状态**：`out/` 里有成片 MP4 和 SRT。ffprobe 只说明封装和编码参数正常；画面是否检查过、用户是否验收、是否已发布，都**未记录**。

| 项 | 值 | 来源 |
| --- | --- | --- |
| 分辨率 / 帧率 | 1080×1920 竖屏，30 fps | ffprobe；`lib.mjs:7` |
| 帧数 | 6910（`round(230.333×30)`，由代码公式推出） | `render.mjs` 主循环 |
| 时长 | 230.333 s（timeline 230.333） | ffprobe；`build/timeline.json` |
| 编码 | H.264（libx264 preset medium, CRF 19, yuv420p, faststart）+ AAC 48 kHz 立体声 192k | ffprobe；`render.mjs:180-190` |
| 文件 | 26,766,967 B，平均约 0.93 Mbps | evidence pack |
| 字幕 | `out/天空为什么是蓝色，晚霞却偏红.srt`，103 条；画面同时烧录 | 文件；`render.mjs:85` |
| 其他版本 | 无。代码里有 `--cover` 分支，但依赖的 `cover.mjs` 不存在（见 §7） | `render.mjs:163-169` |
| 文件时间 | script 19:59 → timeline/SRT 20:26 → MP4 20:49（2026-10-31） | 文件 mtime |

**一句话管线**：`script.json`（35 段，4 个声音，全局 `rate: 4`）→ `tts.py` 逐句调用 edge-tts，取 WordBoundary 逐词时间并缓存 → `build/timeline.json`、`build/narration.wav`、SRT → `render.mjs` 依次加载 `scenes-a…e.mjs`（共 10 个场景），每帧画背景、场景（0.4 s 交叉淡化）、资料页脚、字幕、片尾卡和角标 logo → RGBA 帧通过管道送入 ffmpeg；`audio.mjs` 程序合成配乐和音效，混音后输出 MP4。

## 2. 教学目标与课标对齐

- **curriculum**（`script.json`）：人教版必修第一册 第二章第二节 大气受热过程（大气对太阳辐射的削弱作用：散射；太阳高度与大气削弱）；课标 1.4（说明大气的组成和垂直分层……说明大气受热过程与热力环流原理）。
- **核心知识点**：大气对太阳辐射的**散射作用**——波长较短的蓝紫光最容易被散射，所以晴天天空呈蔚蓝色；太阳高度越小，阳光穿过大气的路程越长，所以夕阳偏红橙，晚霞是被这种光照亮的云。
- **实际叙事结构**（按段落 id）：
  - 导入：`ask1/ask2`（小林：同一个太阳，为什么一会儿蓝、一会儿红）→ `hook`。
  - 原理：`white` 白光由七色光混合 → `cast/redHi/blueHi` 两位代表出场 → `enter/term` 引出散射作用这一术语 → `bounce/steady/rule` 总结规律 → `sky` 蓝天的成因 → `violetQ/violetA/violetEnd` 解释天空为什么不是紫色 → `duskQ/noon/dusk/blueLost/redWin/sunset` 太阳高度与路程长短 → `cloudQ…cond` 云为什么是白的、晚霞怎么形成。
  - 迁移：`apply/moon/dawn/haze`（月球白天天空是黑的、日出前天已亮、雾霾天灰白）。
  - 总结：`sum`，再用 `exam` 给出三个答题关键词。
- **科学边界**（写成旁白内容本身，不是免责声明）：`notDye`“空气并没有把阳光染成蓝色，只是改变了蓝光前进的方向”；`notGone`“蓝光没有变成红光，也没有全部消失”；`cond`“晚霞美不美，还要看云的位置、厚薄和颗粒物”；紫光给出两条原因（阳光里紫光较少、人眼对紫光不敏感）。
- **审校来源**：`source` 写的是“Geo Story Studio · douyin-sky-colors v002 分镜与科学审校（claim-007、019～024）”。`script.json` **没有 `based_on` 字段**。按 AGENTS §1 的命名惯例，源 build 应为 `geo-story-studio/storage/builds/douyin-sky-colors/v002/`（**推断**，未打开）。
- **numbers 字段**（7 条，均为定性结论加出处）：白光为多色光混合（NASA Science Wave Behaviors；教材）；蓝紫光最易被散射（Met Office；OSA/FSU）；天空不呈紫色（UCAR；Met Office）；太阳低时路程长、直射光偏红橙（Met Office；教材）；云滴大、各色光散射相近（NOAA JetStream The Color of Clouds）；月球白天天空黑（NASA）；晨昏蒙影与雾霾灰白（教材）。全片**没有**具体数值。

## 3. 技术与依赖

| 类别 | 内容 | 依据 |
| --- | --- | --- |
| 渲染 | Node ESM + `@napi-rs/canvas`：`package.json` 声明 `^1.0.11`，lock 解析为 **1.1.0**；没有 `scripts` | `package.json`、`package-lock.json` |
| TTS | Python `edge-tts`，代码中 import 了 `aiohttp`，版本**未记录**（没有 requirements 文件） | `tts.py:16-18` |
| 字体 | `fonts.py` 用 `fontTools` 从 macOS 系统字体集中抽取圆体-简粗体、苹方-简中粗；`lib.mjs` 发现字体缺失时会自动调用 `.venv/bin/python fonts.py` | `fonts.py`；`lib.mjs:11-13` |
| 合成 | ffmpeg：TTS 混音（adelay/amix/highpass/loudnorm）和成片编码（sidechaincompress/amix/alimiter/libx264/aac） | `tts.py:181-185`；`render.mjs:185` |

声音配置（`script.json` voices；实际语速 = 角色 rate + 全局 `rate: 4`，见 `tts.py` `synth()`）：

| 角色 | 声音 | rate（实际） | pitch |
| --- | --- | --- | --- |
| 旁白老师 `zh` | zh-CN-XiaoxiaoNeural | 0（+4%） | 0 |
| 学生小林 `stu` | zh-CN-YunxiNeural | +6（+10%） | +2 Hz |
| 小蓝 `blue`（蓝光） | zh-CN-XiaoyiNeural | +8（+12%） | +6 Hz |
| 小红 `red`（红光） | zh-CN-YunjianNeural | 0（+4%） | −4 Hz |

## 4. 镜头与时间轴

时间来自 `build/timeline.json`（场景切换点 = 对应段的 `start` 减 0.25～0.3 s）。

| # | 场景 id | 约时间 (s) | 旁白要点 | 画面 | 模块 |
| --- | --- | --- | --- | --- | --- |
| 1 | `intro` | 0–18.4 | 小林提问、hook | 中午 / 傍晚两扇天空窗，两个太阳之间画“=”和“?”，出标题，一束阳光出发 | `scenes-a.mjs:10` |
| 2 | `light` | 18.4–40.7 | 白光是七色光；红光、蓝光自我介绍 | 棱镜分出七色光；小红、小蓝从光束里飞出，下方对比长波和短波的波形条 | `scenes-a.mjs:54` |
| 3 | `pinball` | 40.7–70.5 | 进入大气、散射作用、规律 | 16 个 N₂/O₂ 分子组成“弹珠台”：小蓝在分子间折来折去，小红基本直行，只被推偏一次 | `scenes-b.mjs:35` |
| 4 | `skyview` | 70.5–86.6 | 蓝天的成因；不是染色 | 天空窗里布满散射出来的蓝点，从各个方向射向眼睛图标 | `scenes-b.mjs:111` |
| 5 | `violet` | 86.6–106.4 | 为什么不是紫色 | 两条原因卡片，各色散射光混合后进入眼睛 | `scenes-c.mjs:9` |
| 6 | `path` | 106.4–144.9 | 太阳高度与路程；夕阳偏红 | 地球圆弧加大气壳层：太阳从高处滑到低处，标出“路程短/路程长”；光包沿长路程前进，计数“走到终点：红 4 · 蓝 1” | `scenes-c.mjs:85` |
| 7 | `cloudWhite` | 144.9–159.2 | 云是白的 | 分子和云滴大小对比，红、绿、蓝光被云滴散射得差不多 | `scenes-d.mjs:9` |
| 8 | `sunsetCloud` | 159.2–181.0 | 晚霞形成和条件 | 低太阳把红橙光打到云上，云再把光散向观察者；出现条件卡 | `scenes-d.mjs:61` |
| 9 | `apply` | 181.0–206.5 | 三个现象 | 三张卡片从右侧滑入：月球、晨昏、雾霾 | `scenes-e.mjs:43` |
| 10 | `summary` | 206.5–230.3 | 一句话总结，三个关键词 | 小红、小蓝对照卡和“答题关键词”列表；227.3 s 起切到片尾品牌卡 | `scenes-e.mjs:65` |

## 5. 关键算法与实现

> 先区分两件事：视频**讲的知识**是大气散射与太阳高度；下面说的是**做视频用的算法**。画面是 Canvas 2D 的程序动画，不是辐射传输模拟。

**5.1 球壳几何算出“路程短 / 路程长”**（`scenes-c.mjs:61-70`）：地球半径 `RE=1500`、大气壳半径 `RA=1730`、观察点 `P` 在地表最高点。从 P 沿某个方向射线与外圆求交点，得到阳光在大气里走过的距离：

```js
const ELEV = (7 * Math.PI) / 180, D = [-Math.cos(ELEV), -Math.sin(ELEV)];
const exitDist = (d) => {
  const px = P[0] - O[0], py = P[1] - O[1], b = px * d[0] + py * d[1], c = px * px + py * py - RA * RA;
  return -b + Math.sqrt(b * b - c);
};
const LONG = exitDist(D), SHORT = exitDist([0, -1]);
```

本次按代码常量复算：SHORT = 230 px，LONG ≈ 698 px，太阳高度 7° 时路程约为正午的 3.0 倍。大气层厚度为了画面需要被放大，所以这个倍数**不是**真实大气质量比。太阳沿极角插值，从正上方滑到 `SUN_LOW`；颜色参数 `warm` 跟着旁白里“红橙色”这个词变化（`scenes-c.mjs:101-107`）。

**5.2 光包损耗是事先写好的，不是随机的**（`scenes-c.mjs:83`）：10 个光包红蓝交替，每个光包的 `hit` 表示它在路程的哪个比例处被撞离（取值 ≥1 表示能走到终点）。蓝光包的值是 `[0.25,0.45,0.62,2,0.35]`，红光包里只有 i=5 在 0.78 处被撞走，所以画面计数稳定在“红 4 · 蓝 1”，每次渲染结果一致。被撞走的光包沿 `side`（`rnd` 决定左右）斜着飘出画面。

**5.3 弹珠台**（`scenes-b.mjs:10-31, 69-86`）：16 个分子用 `rnd(i)` 确定位置，再加正弦微动。小蓝按 `ZIG=[4,1,6,11,9,14]` 依次连接这些分子的**当前位置**，逐段 easeIO 插值，被撞的分子会闪一下。小红直线前进，只在 35%～50% 处被 `sin` 脉冲推偏 40 px。两人身后的波形尾巴用 `waveTail` 画，波长分别是 `lam=120` 和 `52` px（`cast.mjs:157-175`），直接对应旁白里的“步子大 / 小又密”。

**5.4 背景随傍晚变暖**（`render.mjs:29-35`）：背景渐变的三个色标在 `S('duskQ')→S('dusk')` 之间从深蓝 `mixRGB` 到暮紫，到 `S('apply')` 前后再退回。这是 5 部片里唯一会随剧情改变的背景（其余几部都是固定深蓝）。

**5.5 公共管线**（5 部片共用，`tts.py` 只改了标题和 `LEAD_IN/TAIL`）：
- 端点替换 `edge_tts.communicate.WSS_URL = "wss://api.msedgeservices.com/..."`（`tts.py:19-25`，带 rany2/edge-tts#490/#491 注释）。
- 缓存键 `sha1(f"{voice}|{rate}|{pitch}|{text}")[:16]`，同一键对应一个 mp3 和一个 json（`tts.py:47`）。遇到 429 最多重试 5 次，第 n 次失败后等 8n 秒（`tts.py:53-62`）。
- 段落间距按**最后一个词的结束时间 +0.12 s** 计算，不用 mp3 时长，避开 mp3 尾部约 0.6 s 的静音（`tts.py:153`）。
- `lib.mjs:36-45` 里的 `word(id, w)` / `at(id, w)` 把“某段里的某个词”换算成秒，所有卡片、音效和角色动作都挂在词上。`talkLevel` 只在逐词区间内让嘴巴开合（`lib.mjs:193`）。

## 6. 声音与字幕

- **TTS 混音**（`tts.py:178-190`）：每句先 `aresample=48000,adelay=ms|ms` 放到时间轴上，再 `amix(normalize=0)` → `highpass=f=80` → `loudnorm=I=-16:TP=-1.5:LRA=11` → `apad,atrim` 到总长。`LEAD_IN=1.0`，默认 `GAP=0.45`（各段用 `gap` 覆盖，常见 0.3/0.6），`TAIL=3.2`；`TARGET=None`，即不设时长上限。
- **配乐**（`audio.mjs:150-185`）：112 BPM，C–G–Am–F 拨弦琶音、低音、沙锤，每隔一小节加一段钟琴；种子 42；淡入 1.2 s、淡出 2.5 s。成片混音时音量 0.55，并以旁白为侧链压缩（threshold 0.05、5:1、20/400 ms）。
- **音效**：`audio.mjs` 程序合成 15 种（pop/note/ding/sparkle/whoosh/boing/slam/stamp/tick/scribble/buzz/alarm/tada/confetti/wind），由各场景的 `events` 按词触发；每次切场景自动加一个 whoosh（`render.mjs:154`），片尾卡配 sparkle；音量 0.5，最后经 `alimiter=0.93`。
- **字幕**：分句时优先用 `captions` 数组，没有就用 `split_caption`，每块不超过 14 字（`tts.py:107-121`）；起止时间取该块覆盖到的首词和末词。烧录位置 `SUB_Y=1560`，角色台词前加名字标签（小林/小红/小蓝），字号从 56 px 起逐步缩小，直到整行放得下（`render.mjs:85-110`）。

## 7. 迭代与问题

| 事项 | 证据 | 说明 |
| --- | --- | --- |
| `--cover` 分支跑不通 | `render.mjs:4, 163-169` 会 `import('./cover.mjs')`，但本目录没有这个文件（仓库里只有 geologic-day、meteor-journey、plateau-basin-sunlight、solar-eruption-earth 有） | 执行 `node render.mjs --cover` 会因模块不存在而报错，`out/` 里也没有封面。**推断**：`render.mjs` 是从带封面功能的视频复制来的（[[plateau-basin-sunlight]] 的 `render.mjs` 也有 `cover.mjs`/`--cover` 写法，并多出推荐版 `--rec`） |
| 缺少 `based_on` | `script.json` 的 keys 里没有 | AGENTS §1 要求把源 build 路径写进 `based_on`；本片只在 `source` 文字里写了 build 名和版本 |
| fontTools 不在安装说明里 | `fonts.py` 需要 `fontTools`；README 只写了 `pip install edge-tts` | 新环境首次渲染时，自动抽取字体可能失败，退回 Hiragino（`fonts.py` 打印提示）——**推断** |
| 规则符合度（代码中存在） | `brand()`（x=64, y=76, w=290, α 0.95, 0.3–1.0 s 淡入）、`outro()`（760 px logo + 标语“用地理的眼光，看懂天空的颜色”）、页脚只写资料来源 | 符合 AGENTS §4/§5。片尾卡从 `E('exam')+0.3` = 227.3 s 开始，持续约 3.0 s；最后 0.6 s 淡出到黑 |
| README 时长一致 | README 写 3:50 | 与 ffprobe 的 230.3 s 一致 |

制作对话读不到，所以预览帧（`build/` 里已经没有 `still_*.png`）、返工次数和用户反馈都**未记录**。

## 8. 复现入口

以下命令来自 README 和 `render.mjs` 头部注释，本次**均未执行**：

```bash
cd sky-colors
npm install
python3 -m venv .venv && .venv/bin/pip install edge-tts   # 另需 fontTools 供 fonts.py 使用
.venv/bin/python tts.py            # → build/narration.wav, build/timeline.json, out/<TITLE>.srt
node render.mjs --still 60 120     # → build/still_<t>.png
node render.mjs                    # → build/music.wav, build/sfx.wav, out/<TITLE>.mp4
```

`lib.mjs` 加载时就会读取 `build/timeline.json`，所以必须先跑 `tts.py`。`build/tts_cache/` 还在，`rate` 不变时**推断**可以不联网重建。

## 9. 参考来源

`script.json` 和代码里**没有 URL**（evidence pack `urls: []`）。下表列出的是项目文件中提到的资料名称，本次**都没有访问**：

| 资料 | 用途 |
| --- | --- |
| NASA Science《Wave Behaviors》、Space Place《Why Is the Sky Blue》 | 白光组成、月球天空 |
| Met Office《Why is the sky blue》 | 散射规律、紫光、太阳高度 |
| UCAR《The Colors of the Sky》 | 天空不呈紫色 |
| NOAA JetStream《The Color of Clouds》 | 云为什么是白的 |
| OSA/FSU《Why is the sky blue》 | 散射规律 |
| 人教版地理必修第一册 | 术语与结论 |
| `geo-story-studio/storage/builds/douyin-sky-colors/v002/`（推断路径） | 分镜与科学审校 claim-007、019～024 |

## 10. 证据与限制

| 文件 | 行数 | sha256[:12] | 读取程度 |
| --- | --- | --- | --- |
| `sky-colors/script.json` | 66 | `7af2e90ad134` | 全文 |
| `sky-colors/tts.py` | 209 | `2bc0b097c1b1` | 全文 |
| `sky-colors/render.mjs` | 202 | `337c237b9c32` | 全文 |
| `sky-colors/lib.mjs` | 223 | `c5d1ee06d19f` | 全文 |
| `sky-colors/audio.mjs` | 204 | `f9e04b2cfd54` | 音效与配乐部分（与另外 4 部片哈希相同） |
| `sky-colors/scenes-a…e.mjs` | 138/181/176/136/101 | `2e0eb8559c50`/`9188f52b1dcd`/`8d21a8a34691`/`e2c450c76a84`/`13072ebd6a18` | 结构与注释全部；scenes-b、c、e 读了关键段落 |
| `sky-colors/cast.mjs` / `props.mjs` | 246/182 | `fa96985157d0`/`1e636b87c6ad` | 导出与注释；读了 `sun`、`waveTail`、`LIGHT` |
| `sky-colors/fonts.py` | 37 | `a6ac7264e548` | 全文 |
| `sky-colors/build/timeline.json` | — | — | 读取各段的 start/end/cues |
| `sky-colors/out/*.mp4` / `*.srt` | — | 26,766,967 B / 6,620 B | 只看 ffprobe 结果和 SRT 条数，没有播放 |

限制：没有打开视频和音频，没有试听，也没有测响度；ffprobe 是技术层面的信息，不代表画面检查或验收。Python 和 ffmpeg 的版本、硬件、渲染耗时都未记录。
