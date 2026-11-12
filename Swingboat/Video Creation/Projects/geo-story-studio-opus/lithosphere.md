---
type: video-production-dossier
project: geo-story-studio-opus
video_id: lithosphere
title: 岩石圈，就是地壳吗？
curriculum: 人教版必修第一册 第一章第四节 地球的圈层结构（地壳、地幔、莫霍界面、软流层、岩石圈）；课标 1.3；迁移衔接选择性必修1 第二章 板块运动
captured: '2026-11-12'
status: 成片存在（竖屏 1080×1920，198.3 s）；技术检查/视觉检查/用户验收未记录；未收入 README 目录
source_fingerprint: cb685104dea5a5c9180b07f60fa8208dd8d33649d7dd4e06b35365494d91f9e1
tags:
- video-creation
- production-capture
- geography-teaching
- canvas-frame-render
- edge-tts
---

# 岩石圈，就是地壳吗？ · 审校 claim 约束下的“同一剖面、两种分法”，可平移 / 剪切的分层剖面动画

<!-- capture-source-sha256:cb685104dea5a5c9180b07f60fa8208dd8d33649d7dd4e06b35365494d91f9e1 -->

本文是**根据项目文件做的材料回溯**（`script.json`、`tts.py`、`render.mjs`、`lib.mjs`、5 个场景模块与 `section/cast/audio.mjs`、`build/timeline.json`、证据包的 ffprobe 与 sha256 清单）。证据包映射到宿主会话 `25f14748…`（创建于 2026-11-01 17:11，标题“视频制作和审校流程”），只按元数据找到，宿主返回空 transcript，**对话内容不可读**；映射依据是会话创建时间早于 `script.json` 数分钟，属**推断**。本次未运行任何项目脚本，也未打开音视频。按成片时间，这是本项目**最新**的一部视频（2026-11-01 17:34）。

## 1. 当前结论与制作路线

**状态：成片存在**——`out/岩石圈就是地壳吗.mp4` + 同名 `.srt`。只有 ffprobe 媒体头信息；画面、试听、响度、用户验收与发布都**未记录**。**README.md 的视频目录里没有这一行**（检索 `lithosphere` 无结果）。

| 项 | 值 | 来源 |
| --- | --- | --- |
| 分辨率 / 画幅 | 1080×1920，9:16 竖屏 | ffprobe；`lib.mjs:7` |
| 帧率 / 帧数 | 30 fps；`round(198.303×30)` = 5949 帧 | ffprobe；`render.mjs:228`（推算） |
| 时长 | 198.300 s（timeline 198.303 s） | ffprobe；`build/timeline.json` |
| 视频编码 | H.264，yuv420p，CRF 19，preset medium，`+faststart` | ffprobe；`render.mjs:225-226`（与 [[sea-to-mountain-rain]] 同参数） |
| 音频编码 | AAC 48 kHz 立体声，192 kbps | ffprobe；`render.mjs:226` |
| 文件大小 | 29,161,979 字节（约 29.2 MB） | 证据包 |
| 字幕 | `.srt` 106 条，6191 字节；画面烧录字幕 + 角色标签 | 证据包；`render.mjs:76-103` |
| 其他版本 | 无 | `out/` 列表 |

文件时间：`.srt` 2026-11-01 17:17，成片 17:34。git：证据包记录源文件属于 `8cbe673 2026-11-01 feat(videos): add 21 high-school geography lessons`。

一句话管线：

> `script.json`（36 段，4 个声线角色，`based_on` 带审校 claim 编号）→ `tts.py`（edge-tts + WordBoundary）→ `timeline.json` / `narration.wav` / SRT → `render.mjs` 加载 `scenes-a…e.mjs`（8 个场景），全部场景共用 `section.mjs` 的分层剖面 → `audio.mjs` 合成配乐音效 → RGBA 帧管道送 ffmpeg。`render.mjs`、`tts.py` 由 [[sea-to-mountain-rain]] 复制改写（diff 只涉及标题、背景、角色、资料行与品牌卡文字）。

## 2. 教学目标与课标对齐

- **curriculum**：人教版必修第一册 第一章第四节 地球的圈层结构（地壳、地幔、莫霍界面、软流层、岩石圈）；课标 1.3 运用示意图，说明地球的圈层结构；迁移衔接选择性必修 1 第二章 板块运动（岩石圈板块）。
- **核心知识点**：岩石圈的范围 = 地壳 + 上地幔顶部（软流层以上），以及它与“地壳 / 地幔”是两种不同的分层依据。
- **讲解结构（按实际文案）**：
  1. 导入：“考你一题：岩石圈，就是地壳吗？”小林：“那还用问？”→“差了一截”。
  2. 原理：切开局部剖面，逐层认识地壳 → 莫霍界面 → 地幔最上部（坚硬）→ 软流层；“上地幔顶部”自己喊“岩石圈也算我一份”；给出教材原文式定义。
  3. 辨析：小林“莫霍界面明明分开了，怎么又合成一个圈？”→ 按物质组成 vs 按力学性质两种分法；类比“按班级 / 按身高排队的两张名单”；结论“莫霍界面在岩石圈里面，不是底界”。
  4. 误区：“软流层是一锅岩浆吗？”——慢吞吞的“软流层”自述“不是岩浆，是固态的”；冰川类比“固态也能缓慢流动”；补教材说法“可能是岩浆的主要发源地”= 局部少量熔融。
  5. 迁移：板块 = 岩石圈板块，地壳连同坚硬的上地幔顶部一整块移动；喜马拉雅山——碰撞的是两大块岩石圈。
  6. 总结：三道判断题（小林秒答“错！”）+ 一句话记住 + “动的是一整块岩石圈”。
- **科学审校边界**（`based_on`：`geo-story-studio/storage/builds/douyin-lithosphere/v003`，N09“岩石圈，就是地壳吗？”）：

| claim | 边界内容 | 落在本片的位置 |
| --- | --- | --- |
| claim-013、018 | 岩石圈包括地壳和上地幔顶部坚硬部分 | `def`、`sum`；品牌卡副标题 |
| claim-026 | 地壳 / 地幔按物质组成分；岩石圈 / 软流层按力学性质分 | `two`、`comp`、`mech`、`class`（`scenes-b.mjs`） |
| claim-027 | 莫霍界面在岩石圈内部，不是岩石圈底界 | `moho-in`、`q3` / `a3` |
| claim-028 | 软流层总体固态，地质时间尺度缓慢变形流动，不是液态岩浆海 | `asth-hi`、`solid`、`glacier`、`magma`（`scenes-c.mjs`） |
| claim-029 | 板块是岩石圈板块，地壳与坚硬上地幔顶部共同运动 | `plate`、`move`、`top-go`（`scenes-d.mjs`） |

  另有排除项：不用全深度数值、统一厚度、岩浆通道、食品类比；不讲板块驱动力和运动速率；剖面不按比例。代码对应：`section.mjs:8-10` 只用相对比例 `FR = { moho: 0.3, lith: 0.56 }`，画面没有深度数字；类比用“名单”和冰川，没有食品类比。`numbers` 里还引用“源 build review-002 claim-027”。claim 原文本次未读取。
- **数字与出处**（`numbers` 7 条，均为定性表述，无数值）：岩石圈定义、莫霍界面位置、两种分层依据、软流层固态、冰川流动、板块即岩石圈板块、喜马拉雅碰撞；出处为人教版必修一 1.4 / 选择性必修 1 第二章、The Geological Society、NOAA JetStream Max、BGS、NSIDC。

## 3. 技术与依赖

| 类别 | 内容 | 证据 |
| --- | --- | --- |
| 渲染 | Node.js ESM + `@napi-rs/canvas` `^1.0.11`（lock 解析 1.1.0）；无 npm scripts | `package.json`；`package-lock.json` |
| TTS | Python `edge-tts` + `aiohttp`；版本未记录 | `tts.py` |
| 字体 | `fonts.py`（fontTools）；`lib.mjs` 缺字体时自动调用 | `fonts.py`；`lib.mjs` |
| 编码 / 混音 | 系统 ffmpeg；版本未记录 | `tts.py`；`render.mjs` |

声线（全局 `rate: 4` 叠加；`timeline.json` 记录 `rate: 4`）：

| key | 标签 | voice | 角色 rate / pitch | 实际 rate | 段数 |
| --- | --- | --- | --- | --- | --- |
| `zh` 旁白 | — | `zh-CN-XiaoxiaoNeural` | 0 / 0 | +4% | 27 |
| `stu` | 小林 | `zh-CN-YunxiNeural` | +6 / +2 Hz | +10% | 6 |
| `top` | 上地幔顶部 | `zh-CN-YunjianNeural` | +8 / 0 | +12% | 2 |
| `asth` | 软流层 | `zh-CN-XiaoyiNeural` | **−38** / −6 Hz | **−34%** | 1 |

“软流层”的极慢语速配合文案里的省略号（“我……不是岩浆……”），用声音表现“很慢地动”；`section.mjs:222` 的角色脸是“drowsy, slow-talking face with drifting ‘Z’s”。

## 4. 镜头与时间轴

时间来自 `timeline.json`；场景边界为“某段开始前 0.15–0.2 s”，`XF = 0.3`（`render.mjs:23`）。5 个模块、8 个场景。

| # | 场景 | 约时间 (s) | 口播要点 | 画面 | 模块 |
| --- | --- | --- | --- | --- | --- |
| 1 | `hook` | 0–13.9 | 考你一题；小林“那还用问”；差一截 | “岩石圈 ＝ 地壳？”题卡，“差一截！” | `scenes-a.mjs:10-59` |
| 2 | `layersScene` | 13.9–48.4 | 切开地面，逐层：地壳、莫霍界面、坚硬的地幔最上部、软流层；上地幔顶部喊话；教材定义 | `section()` 按 `reveal` 逐层出现，金色竖括号框出岩石圈 | `scenes-a.mjs:60-120` |
| 3 | 两种分法 | 48.4–90.2 | 物质组成 vs 力学性质；两张名单类比；莫霍界面不是底界 | 同一剖面两组标签（“按物质组成”/“按软硬（力学性质）”）；“一班 / 二班”与身高队列；莫霍界面处打 ✗“不是底界”，软流层顶打 ✓ | `scenes-b.mjs` |
| 4 | 软流层 | 90.2–131.0 | 不是岩浆、总体固态；冰川类比；局部少量熔融 | 软流层网格整体剪切 + 快进时钟；冰川旗杆“第 1→30 年”；零星熔融光点 | `scenes-c.mjs` |
| 5 | `plates` | 131.0–151.0 | 板块 = 岩石圈板块；一整块一起动 | 先演错误“只有地壳滑走”，再整块（地壳 + 上地幔顶部）在软流层上平移 | `scenes-d.mjs:29-74` |
| 6 | `himalaya` | 151.0–159.9 | 印度洋板块与亚欧板块碰撞隆起 | 两块岩石圈剖面对撞，山体隆起，三枚标签（不画地图） | `scenes-d.mjs:75-120` |
| 7 | `quiz` | 159.9–184.0 | 三道判断题，小林连答“错！” | 判断卡 + “✗”盖章 + “全对！”盖章 | `scenes-e.mjs` |
| 8 | `summary` | 184.0–198.3 | 一句话记住；动的是一整块岩石圈 | 迷你剖面 + 公式；195.3 s 起品牌卡“用地理的眼光，看懂脚下的大地” | `scenes-e.mjs`；`render.mjs:130-` |

## 5. 关键算法与实现

**视频讲的原理**：按物质组成与按力学性质的两种圈层划分、软流层的固态蠕变、岩石圈板块整体运动。**做视频用的算法**如下。

### 5.1 一份可组合的分层剖面（`section.mjs:8-13, 144-162`）

所有场景调用同一个 `section(ctx, t, o)`：按 `FR` 相对比例切出地壳 / 上地幔顶部 / 软流层三条带和莫霍线，`o.reveal` 控制逐层出现，`o.glow` 高亮某层，`o.shift` 平移岩石圈，`o.shear` 剪切软流层。层厚只用相对比例（注释“relative ranges only; not to scale”），没有深度数值——与 `based_on` 的“不用全深度数值、统一厚度”一致。地壳、上地幔顶部的纹理按周期 `wrapX` 循环，所以整块平移不会露出边缘（`section.mjs:21-24`）。

### 5.2 “固态也能流动”的网格剪切（`section.mjs:90-131`；`scenes-c.mjs:11-30`）

软流层画成连续的网格 + 颗粒，剪切量随深度线性递减，颗粒跟着网格走——“it stays one solid body”：

```js
const off = (yy) => shear * (1 - (yy - y) / h) + wobble * Math.sin(t * 1.3 + yy * 0.02);
// scattered tiny melt pockets (a small local fraction, never a liquid layer)
if (melt > 0 && rand(r * 31 + c * 7) < 0.16) { ... }
```

`scenes-c.mjs` 在“漫长”一词时让 `shear` 缓动到 230 px，同时显示快进时钟；熔融光点只占约 16% 的格子，在“局部少量熔融”处淡入。这是把审校 claim-028（不是液态岩浆海）直接写进了绘图参数。

### 5.3 先演错、再纠正的板块运动（`scenes-d.mjs:30-40`）

```js
// the wrong idea: only the crust slides off
const wrong = easeOut(prog(t, tWrong, tWrong + 0.6)) * (1 - easeIO(prog(t, tFix - 0.3, tFix + 0.3)));
block(ctx, 170 + dx, 900 + dx, { crustDx: 110 * wrong, shift: dx, outline: fadeIn(t, tWhole, 0.4) });
```

在“不是只有”时，中间板块的地壳单独错开 110 px；到“连同”时收回，再在“一整块”“一起移动”时整块平移 150 px，下方软流层按 0.8 倍剪切。喜马拉雅一幕复用 `block()`，在“碰撞”“隆起”等词上推挤和抬升。

### 5.4 时间轴、TTS 与品牌（`tts.py`；`render.mjs:56-150`）

- 与 [[sea-to-mountain-rain]] 同源：WSS_URL 运行时替换、`sha1(voice|rate|pitch|text)` 缓存、429 退避 5 次、`TARGET = None`、`LEAD_IN 0.6`、`TAIL 3.2`；保留角色 `fx` 支持（本片未用），去掉了 `["显示","口播"]` 字幕对。
- 切场：淡入 + 0.95→1 缩放、离场 1→1.04；盖章事件带 `shake` 震屏。
- 小林每次说话时左下角弹出头像（`render.mjs:103-118`，本片包括开场那句），嘴型按词时间开合。
- 角标 logo `x=64, y=76, w=290`，`CARD_T = E('end') + 0.3`（≈195.3 s）前淡出；品牌卡 760 px logo + “用地理的眼光，看懂脚下的大地”+“岩石圈 ＝ 地壳 ＋ 上地幔顶部（软流层以上）”；末 0.6 s 淡到黑。

## 6. 声音与字幕

- **旁白**：36 段约 913 字（五部里最短）；混音链 `aresample → adelay → amix → highpass 80 Hz → loudnorm I=-16:TP=-1.5:LRA=11`。
- **配乐**：`audio.mjs` 程序合成，112 BPM C–G–Am–F 拨弦，与 [[sea-to-mountain-rain]] 相同的配乐函数。
- **音效**：FX 表约 22 种；5 个模块共 67 个事件（pop 9、whoosh 10、ding 7、tick 6、sparkle 6、stamp 5 等）。表中 `sizzle`（注释“two-note sizzle for ‘好烫’”）本片未调用，推断沿用自其他视频。
- **总混音**（`render.mjs:222-223`）：配乐 ×0.75 侧链闪避，音效 ×0.5，`alimiter=0.93`。
- **字幕**：`SUB_Y = 1500`，字号 58 起按宽度递减；标签小林（黄）/ 上地幔顶部（岩褐）/ 软流层（橙红）。资料行“资料：人教版地理必修第一册、选择性必修1 · Geological Society · NOAA · BGS · NSIDC”从 `sum` 段显示（字号 25，比模板小，推断为放下更长的来源列表）。

## 7. 迭代与问题

- **README 目录缺失**：README.md 的视频目录里没有本片。
- **`based_on` 写的是本机绝对路径**（`script.json`），本文统一写作 `geo-story-studio/storage/builds/douyin-lithosphere/v003`。
- **全局语速 +4%**：`rate: 4`，`TARGET = None`；原因**未记录**。“软流层”−38% 是有意的角色设计。
- **“不按比例”只留在元数据和注释里**：`based_on` 与 `section.mjs:8` 写了“不按比例 / not to scale”，画面上没有——符合 AGENTS.md 第 5 节（`render.mjs:120` 注释也引用了该规则）。
- **遵守 AGENTS 的证据**：`numbers` 每条都写了出处，`sources` 给出 5 个 URL；喜马拉雅不画地图；logo、品牌卡、`TAIL 3.2` 符合第 4 节；`build/` 无残留预览图。
- 宿主会话标题“视频制作和审校流程”提示本片可能伴随审校流程讨论，但内容不可读，**无法确认**。

## 8. 复现入口

本次**均未运行**。按 AGENTS.md“常用命令”（在 `lithosphere/` 内）：

```bash
python3 -m venv .venv && .venv/bin/pip install edge-tts   # fonts.py 另需 fontTools（AGENTS 未列出）
npm install                                               # @napi-rs/canvas；无 npm scripts
.venv/bin/python tts.py            # → build/seg*.mp3、tts_cache/、narration.wav、timeline.json；out/岩石圈就是地壳吗.srt
node render.mjs --still 40 140     # → build/still_<t>.png（用完删除）
node render.mjs                    # → out/岩石圈就是地壳吗.mp4
```

## 9. 参考来源

`script.json` `sources` 列出 5 个 URL，均为**项目文件中提及，本次未访问**：

| URL | 用途 |
| --- | --- |
| https://jc.pep.com.cn/ | 人教版必修第一册、选择性必修 1（教材口径） |
| https://www.geolsoc.org.uk/Plate-Tectonics/Chap2-What-is-a-Plate/Mechanical-properties-lithosphere-and-asthenosphere.html | 岩石圈 / 软流层的力学性质 |
| https://www.noaa.gov/jetstream/tsunamis/tsunami-generation-earthquakes/jetstream-max-plate-tectonics-and-earthquakes | 两种分层依据、板块 |
| https://www.bgs.ac.uk/discovering-geology/earth-hazards/earthquakes/what-causes-earthquakes/ | 板块即岩石圈板块 |
| https://nsidc.org/learn/parts-cryosphere/glaciers | 冰川固态流动类比 |

改编来源：`geo-story-studio/storage/builds/douyin-lithosphere/v003`（N09，claim-013、018、026、027、028、029；review-002），本次未读取。

## 10. 证据与限制

| 文件 | 行数 | sha256 前 12 位 | 用途 |
| --- | --- | --- | --- |
| `script.json` | 73 | `0f30cfc28803` | 全文已读 |
| `tts.py` | 210 | `cbd14567befb` | 与 sea-to-mountain-rain 版 diff |
| `render.mjs` | 239 | `7e33b7578773` | 与 sea-to-mountain-rain 版 diff + 行号核对 |
| `section.mjs` | 242 | `598de978f419` | 剖面、剪切、角色脸 |
| `scenes-a…e.mjs` | 144/161/165/140/104 | `d3c40107172a` / `c14e4161c2cb` / `ce1aa799e483` / `f6e3380f1f61` / `a539d5448503` | 头注释、场景表、关键段落、屏幕文字 |
| `lib.mjs` / `cast.mjs` / `audio.mjs` | 243/150/229 | `eb3caf848f6e` / `6c6f82f71c67` / `9e3eeae951b5` | 颜色、角色、FX 清单 |

- 已读：上表（大文件按段）、`build/timeline.json`、`package-lock.json`、README.md 目录检索。
- 未读 / 未做：`node_modules/`、`.venv/`、`tts_cache`、音视频；未运行脚本；未访问外部 URL；未读取源 build 与审校 claim 原文。
- 缺失：会话内容、需求原文、验收记录、工具版本、成片实测响度、未收入 README 的原因。
