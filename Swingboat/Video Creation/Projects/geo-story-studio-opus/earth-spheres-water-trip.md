---
type: video-production-dossier
project: geo-story-studio-opus
video_id: earth-spheres-water-trip
title: 一滴水的串门记：地球的圈层，真的各住一层吗？
curriculum: 人教版必修第一册 第一章第四节 地球的圈层结构（外部圈层：大气圈、水圈、生物圈及其相互联系）；课标 1.3
captured: '2026-11-12'
status: 成片存在（竖屏 1080×1920，198.8 s）；技术检查/视觉检查/用户验收未记录
source_fingerprint: 8a2c2ef8891fae6efc51c9b1c4d0314a3748e5f78d704cfc33ab0f41ca0e3a0e
tags:
- video-creation
- production-capture
- geography-teaching
- canvas-frame-render
- edge-tts
---

# 一滴水的串门记：地球的圈层，真的各住一层吗？ · 水滴导游穿越四个圈层（8 个场景模块 + 土壤孔隙放大镜 + 水汽回声声线）

<!-- capture-source-sha256:8a2c2ef8891fae6efc51c9b1c4d0314a3748e5f78d704cfc33ab0f41ca0e3a0e -->

本文是**根据项目文件做的材料回溯**（源码、`script.json`、`build/timeline.json`、`out/` 下的成片与字幕、证据包里的 ffprobe 与 sha256 清单）。证据包把宿主会话 `da417a9f…`（2026-10-31 15:22 创建，标题「视频制作与内容改编」）映射到本片，但**只凭元数据找到**：宿主返回空 transcript，对话内容无法读取，映射关系是按时间戳**推断**的（会话创建早于 `script.json` 数分钟）。本次未运行任何项目脚本（没有跑 `tts.py`、`render.mjs`、npm、pip），也没有打开视频或音频。

## 1. 当前结论与制作路线

**状态：成片存在**——`out/一滴水的串门记.mp4`（10-31 15:57）+ `out/一滴水的串门记.srt`（15:54）。只有 ffprobe 媒体头信息；视觉检查、试听、用户验收和是否发布都**未记录**。

| 项 | 值 | 来源 |
| --- | --- | --- |
| 分辨率 / 画幅 | 1080×1920，9:16 竖屏 | ffprobe |
| 帧率 / 帧数 | 30 fps；`round(198.824×30)` = 5965 帧 | ffprobe；`render.mjs`（推算） |
| 时长 | 198.833 s（timeline 198.824 s，约 3:19） | ffprobe；timeline |
| 视频 | H.264，yuv420p，CRF 19，preset medium，`+faststart` | ffprobe；`render.mjs` |
| 音频 | AAC 48 kHz 立体声 192 kbps | ffprobe；`render.mjs` |
| 文件大小 | 28,501,050 B（约 28.5 MB） | 证据包 |
| 字幕 | `.srt` 93 条（5783 B）+ 画面烧录 | timeline cue 计数 |
| 其他版本 | 无 | `out/` 列表 |

时间线：会话 15:22 → `script.json`、`timeline.json`、`music.wav` 都在 15:54 → 成片 15:57。`script.json` 与 TTS 同一分钟完成，推断最后一轮是改文案后立即重跑 TTS 和渲染。

一句话管线：

> `script.json`（34 段：旁白晓晓 + 学生小林 + 导游“小滴”的液态 / 水汽两种声线）→ `tts.py` 调 edge-tts，WordBoundary 取逐词时间，水汽声线额外过 `aecho` → timeline + `narration.wav` + SRT → `render.mjs` 加载 `scenes-a…h`（8 个模块、15 个场景），`props.mjs` 画风景窗、圈层大楼、云和小生物，`zoom.mjs` 生成土壤孔隙放大图，`cast.mjs` 画小滴 / 小林 / 圈层标签 → `audio.mjs` 程序配乐与水滴音效 → ffmpeg → MP4。

## 2. 教学目标与课标对齐

- **curriculum**：人教版必修第一册 第一章第四节 地球的圈层结构（地球的外部圈层：大气圈、水圈、生物圈及其相互联系）；课标 1.3。
- **核心知识点**：外部圈层的概念，以及它们“相互联系、相互渗透”而不是分层叠放。
- **讲解结构（按文案）**：
  1. 导入（误概念）：小林把圈层想成一栋楼 → 大楼倒塌“不是楼房！”→ 小滴从云里跳下当导游（`a-*`）。
  2. 概念：同一片风景里指出空气、河流、大树、土石，给出大气圈、水圈、生物圈的教材定义，四种颜色在同一位置重叠（`b-*`）。
  3. 过程①：雨水入渗进土壤孔隙——“位置在岩土里，但它是土壤水，仍属水圈”（`c-*`）。
  4. 过程②：根吸水 → 茎 → 叶，“生物体里的水照样属于水圈”；气孔蒸腾变水汽（`d-*`）。
  5. 易错点：水汽看不见，白云白雾是小水滴或冰晶（`d-trap`）。
  6. 过程③：上升冷却、适宜条件下凝结成云滴 / 冰晶；“云算哪个圈层？”用“它是什么、它在哪”两问回答；约 100 万个云滴合成一颗雨滴（`e-*`）。
  7. 迁移：回看路线（只是许多可能路线之一）；一把土里四个圈层都在；生物圈集中在各圈层交界带（`f-*`）。
  8. 总结 + 做题两问 + 告别（`g-*`）。
- **科学边界（`based_on`）**：改编自 `geo-story-studio/storage/builds/douyin-earth-spheres/v003`（N10“地球的几个圈层，真的各住一层吗？”）。边界：圈层相互联系、相互渗透，不画成排他楼层；岩土孔隙中的水仍属水圈，称土壤水，不把所有入渗都当地下水；进入生物体的水仍属水圈；蒸腾只示部分水、相变后改用不同符号；水汽是气态水、不可见，不等于小水滴；凝结需适宜条件，云可含冰晶；云里的水属水圈、云位于大气中；生物圈含生存环境，多数生物集中在各圈层接触带；路线只是许多可能路径之一。文案几乎逐条对应（`c-ans`“是土壤水”、`d-in`、`d-trap`、`e-rise`“在适宜条件下……也可能有冰晶”、`e-ans`、`f-route`、`f-bio`）。
- **知识点与出处**（`numbers` 7 条）：

| 内容 | 出处 |
| --- | --- |
| 大气圈、水圈、生物圈定义；生物圈集中在接触带 | 人教版必修第一册 第一章第四节 |
| 水圈包括海洋、河湖、冰川、地下水、大气水、生物水 | 教材；NOAA *The water cycle* |
| 根吸收的水绝大部分经蒸腾散失（初中常见表述约 99%） | 人教版七年级上册生物；NASA Earth Observatory |
| 水汽不可见，云雾是小水滴或冰晶 | NASA *How Do Clouds Form?* |
| 云滴约 0.02 mm、雨滴约 2 mm，体积比 (2÷0.02)³ = 100 万 | UCAR / NOAA SciJinks 云物理科普；按直径换算 |
| 一把土里有矿物、水、空气、生物 | 人教版必修第一册 第五章第三节 土壤 |

## 3. 技术与依赖

| 类别 | 内容 | 证据 |
| --- | --- | --- |
| 渲染 | Node.js ESM，`@napi-rs/canvas` 声明 `^1.0.11`，lock 解析 **1.1.0** | `package.json`；lock |
| npm scripts | 无 | `package.json` |
| TTS | Python `edge-tts` + `aiohttp`；版本未记录 | `tts.py` |
| 字体 | `fonts.py`（fontTools），同项目通用版本 | sha `a6ac7264e548` |
| 合成 | 系统 ffmpeg（TTS 侧另用 `aecho` 滤镜）；版本未记录 | `tts.py`、`render.mjs` |
| 素材 | `assets/logo-transparent.png`（使用中）、`assets/logo.png`（深蓝底版，代码中无引用） | `lib.mjs:25`；检索 |

**声线**（全局 `"rate": 4` 叠加）：

| 角色 | key | 声音 | 角色 rate / pitch | 实际 rate | 滤镜 |
| --- | --- | --- | --- | --- | --- |
| 旁白 | `zh` | zh-CN-XiaoxiaoNeural | 0 / 0 | +4% | — |
| 小林（学生） | `stu` | zh-CN-YunxiNeural | +6 / +2 Hz | +10% | — |
| 小滴（液态水滴导游） | `drop` | zh-CN-XiaoyiNeural | +10 / +6 Hz | +14% | — |
| 小滴（水汽，“隐身”） | `vapor` | zh-CN-XiaoyiNeural | +4 / +3 Hz | +8% | `aecho=0.8:0.6:60\|120:0.35\|0.2` |

同一个角色用同一声音的两套参数加回声，表现相变后的“隐身”状态（只用在 `d-vapor`“我隐身啦！”一句）。这是本批 5 个视频中唯一真正用到 `tts.py` `fx` 机制的。

## 4. 镜头与时间轴

场景边界多为“下一句开始前 0.3 s”，交叉淡化 `XF = 0.3 s`，进出缩放同 [[fossil-record]] 框架。画面顶部有章节条 `chapter()`：1 同一块地方 → 2 钻进土里 → 3 爬上大树 → ! 易错点 → 4 飞上云端 → 5 回头看一看 / 抓一把土 → 6 生物圈在哪？→ ★ 一句话总结 → ✎ 做题两问。

| 场景 | 约时间 | 旁白要点 | 画面 | 模块 |
| --- | --- | --- | --- | --- |
| 开场 | 0–19.1 s | “圈层大楼”；不是这么分房；小滴登场 | 四层楼逐层弹出 → 裂开倒塌 → “不是楼房！”印章 → 小滴举导游旗 | `scenes-a.mjs:148` |
| 同一块地方 | 19.1–51.7 s | 三个圈层定义；挤在同一块地方 | 风景窗逐点提示，四种颜色带向彼此重叠 | `scenes-b.mjs:115` |
| 钻进土里 | 51.7–74.1 s | 入渗；“变成岩石圈了吗？”；土壤水仍属水圈 | 孔隙放大镜里小滴沿通道下渗，身份证卡片 | `scenes-c.mjs:95` |
| `d1` / `d2` | 74.1–96.3 s | 根吸水、沿茎上升；体内水仍属水圈；叶片气孔蒸腾 | 植物剖面 → 大叶片与气孔，小滴变水汽符号 | `scenes-d.mjs:144-145` |
| `e1` / `e2` | 96.3–119.0 s | 易错点：水汽 ≠ 云雾；上升冷却凝结 | 两栏“看得见 / 看不见”对比 → 温度计、凝结成云滴 | `scenes-e.mjs:140-141` |
| `f1` / `f2` | 119.0–146.1 s | 云属哪个圈层（两问）；约 100 万个云滴合成雨滴 | 两张问答卡 → 云滴合并、计数器、按比例对比 | `scenes-f.mjs:139-140` |
| `g1` / `g2` | 146.1–166.6 s | 路线回顾（只是其中一条）；一把土四圈层 | 风景上的路线动画 + 备选虚线 → 土壤放大（颗粒、水膜、气泡、蚯蚓、微生物） | `scenes-g.mjs:99-100` |
| `h1`–`h4` | 166.6 s–结束 | 生物圈集中在交界带；总结；做题两问；告别 | 剖面 + 生物带 → “不是楼房”对比 → 两问卡 → 告别 | `scenes-h.mjs:166-169` |
| 品牌卡 | 约 195.8–198.8 s | 无旁白 | logo + “跟着一滴水，看懂地球表层 / 圈层不是楼层 · 相互联系、相互渗透” | `render.mjs` `endCard` |

8 个模块共 15 个场景（a/b/c 各 1、d/e/f/g 各 2、h 4），模块 118–189 行。

## 5. 关键算法与实现

> 区分：视频**讲的科学**是外部圈层的定义与相互渗透；下面是**做视频用的算法**。

**5.1 不挡路的土壤颗粒 `makeGrains()`（`zoom.mjs:6-31`）**。在交错网格上撒矿物颗粒（位置、半径随机抖动），再用“点到折线段的最短距离”剔除会挡住水滴路径的颗粒，保证小滴下渗时始终走在孔隙里：

```js
export function makeGrains(box, path, { step = 120, clear = 70, seed = 1 } = {}) {
  ...
      const r = step * (0.36 + rand(k + 3) * 0.12);
      if (path && distToPath(gx, gy, path) < r + clear * 0.5) continue;
      out.push({ x: gx, y: gy, r, rot: rand(k + 5) * TAU, n: 7 + Math.floor(rand(k + 9) * 3), k });
```

`along(path, p)` 按弧长把进度 0–1 映射到折线上的点，`trail()` 画虚线足迹（`zoom.mjs:78-106`）。`scenes-c`（入渗）、`scenes-d`（根毛吸水）、`scenes-g`（一把土）共用这套放大镜，各自传入不同的 `PATH` 和种子。

**5.2 路线进度钉在词上（`scenes-g.mjs:9-18`）**。回顾路线 `ROUTE` 是 10 个点的折线，进度关键帧取 `f-route` 句里“岩土”“植物”“空气”“云”四个词的发音时刻（0.3 / 0.62 / 0.8 / 1），相邻关键帧 `easeIO` 插值，于是小滴说到哪就走到哪。

**5.3 一百万个云滴与按比例对比（`scenes-f.mjs:67-130`）**。计数器 `n = round(p²·10⁶)`，从“一百万”这个词开始、到“雨滴”这个词结束时数满；雨滴半径从 12 px 长到 110 px。对比图里云滴画成半径 1.1 px 的点、外加金色提示圈，雨滴半径 110 px，直径比正好 1:100（对应 0.02 mm : 2 mm），注释“to-scale comparison”。README 也特别注明“按真实比例画”。

**5.4 四色重叠与水汽回声**。`washes()`（`scenes-b.mjs:52-67`）在念到 `b-mix` 时，让大气圈、水圈、生物圈、岩石圈四条半透明色带的上下边界从各自位置向中间移动并相互覆盖，直观表现“挤在同一块地方”。声音上，`tts.py` 对带 `fx` 的声线在 `adelay` 之前插入滤镜：`aresample=48000,aecho=…,adelay=ms|ms`，只作用于水汽状态那一句。

**5.5 背景与品牌卡（`render.mjs`）**。背景是海蓝到天蓝的渐变 + 70 个缓慢上升的气泡（注释“the water cycle never stops”）；品牌卡 `CARD_T = E('g-end') + 0.3`（[[fossil-record]]、[[earth-core]] 用 +0.5），光晕改为浅蓝色。

## 6. 声音与字幕

- **TTS**：缓存键 `sha1(voice|rate|pitch|text)[:16]`，`build/tts_cache/` 68 个文件（34 段 × mp3/json）；429 退避重试；WSS 地址替换；`LEAD_IN = 0.6 s`、`TAIL = 3.2 s`、`TARGET = None`。缓存键不含 `fx`，滤镜在混音时才加，改回声参数不用重新请求。
- **混音**：旁白 `aresample → [aecho] → adelay → amix → highpass=80 → loudnorm I=-16:TP=-1.5:LRA=11`；成片音乐 0.75 经旁白侧链避让，音效 0.5，`alimiter=0.93`。
- **音效**：场景事件约 86 个；`audio.mjs`（sha `9c59fa2860b3`，与 [[sea-to-mountain-rain]] 相同）在通用音效外增加了 `drip`（水滴“叮”，用了 11 次）、`slurp`（根吸水）、`poof`（隐身 / 现身）、`crumble`（大楼倒塌）、`rain`（雨声）。
- **字幕**：93 条，起止取逐词时间；`SUB_Y = 1500`；小林 / 小滴 / 小滴（水汽）带彩色名牌。
- **资料来源行**：从 `g-sum` 起显示“资料：人教版地理必修第一册 · NOAA · NASA”。

## 7. 迭代与问题

- **目录名与 AGENTS.md 第 1 节不一致**：规则要求改编视频沿用源 build 名称去掉平台前缀，`based_on` 指向 `douyin-earth-spheres/v003`，按规则应叫 `earth-spheres/`，实际目录是 `earth-spheres-water-trip/`。推断：可能是为了与其他圈层视频区分而加了后缀，文件中没有说明。
- **单场景模块**：`scenes-a/b/c.mjs` 各只有 1 个场景，AGENTS.md 第 3 节建议每个文件 2～7 个场景；但这几个文件都在 133–169 行，满足“单文件约 300 行以内”的主要目的。
- **未使用的素材**：`assets/logo.png`（深蓝底版）被复制进来但代码中没有引用；AGENTS.md 第 4 节说这一版只在背景很浅时使用。
- **数字出处跨学段**：蒸腾“绝大部分”引用了人教版七年级生物（约 99%），旁白只说“绝大部分”，没有念出 99%。
- **`based_on` 写的是本机绝对路径**（此处改写为 `geo-story-studio/...`）。
- **README**：已收录（`README.md:50`，标 3:19）。
- **禁用声明**：源码中没有“示意”类文字。
- 返工过程、预览帧检查：**未记录**（`build/` 无 still 或日志）。git 只有批量提交 `8cbe673`（2026-11-01）。

## 8. 复现入口

依据 AGENTS.md“常用命令”和 `render.mjs` 头注释（本次**均未运行**）：

```bash
cd earth-spheres-water-trip
python3 -m venv .venv && .venv/bin/pip install edge-tts   # fonts.py 另需 fontTools
npm install
.venv/bin/python tts.py            # → build/timeline.json, build/narration.wav, out/一滴水的串门记.srt
node render.mjs --still 3.2 7.6    # 预览帧
node render.mjs                    # → out/一滴水的串门记.mp4
```

## 9. 参考来源

`script.json` `sources` 列出 4 个 URL（项目文件中提及，本次未访问）：

| 来源 | URL | 用途 |
| --- | --- | --- |
| 人教版 地理必修第一册 | https://jc.pep.com.cn/ | 圈层定义、土壤 |
| NOAA · The water cycle | https://www.noaa.gov/education/resource-collections/freshwater/water-cycle | 水圈组成、水循环 |
| NASA Earth Observatory · The Water Cycle | https://science.nasa.gov/earth/earth-observatory/the-water-cycle/ | 蒸腾 |
| NASA · How Do Clouds Form? | https://science.nasa.gov/kids/earth/how-do-clouds-form/ | 水汽与云滴 |

另有无 URL 的出处：人教版七年级上册生物；UCAR / NOAA SciJinks 云物理科普。改编来源：`geo-story-studio/storage/builds/douyin-earth-spheres/v003`（N10 科学审校版，本次未读取）。

## 10. 证据与限制

| 文件 | 行数 | sha256 前 12 位 | 本次读取 |
| --- | --- | --- | --- |
| `script.json` | 71 | `b6da8cc781df` | 全文 |
| `tts.py` | 210 | `8f7c9083f96d` | 与 [[plateau-basin-sunlight]] 版逐行 diff |
| `render.mjs` | 232 | `c5b8e0d2cf24` | 与 [[fossil-record]] 版逐行 diff |
| `scenes-a…h.mjs` | 169/157/133/160/155/156/118/189 | `b32d0efa416e` / `e15cfc724c70` / `ced865e3624e` / `071ec4ae3efd` / `67d6508d572b` / `57d0bb8721ca` / `4497dd788875` / `185efc006b6b` | 头注释、场景表、`washes`、`f2`、`routeP` |
| `zoom.mjs` | 108 | `2a9a0b9b977a` | 1–35、78–108 |
| `props.mjs` / `cast.mjs` | 192 / 272 | `34be8a79962e` / `7e642c942df6` | 导出清单、`sphereTag` |
| `lib.mjs` | 244 | `e416cf463218` | 头部、logo 加载 |
| `audio.mjs` | 258 | `9c59fa2860b3` | 与 [[fossil-record]] 版 diff（新增 5 个音效） |
| `package.json` / lock | 10 / — | `178b0f7b4883` / `ed7d1f5ece9b` | 全文 / 版本字段 |
| `build/timeline.json` | — | — | 顶层字段、segment 起止、cue 数 |

未读 / 无法确认：成片画面与声音；角色与道具绘制细节；制作对话（transcript 为空）；工具版本；用户验收与发布；源 build 审校原文。
