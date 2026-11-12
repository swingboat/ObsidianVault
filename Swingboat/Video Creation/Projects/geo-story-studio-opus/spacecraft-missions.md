---
type: video-production-dossier
project: geo-story-studio-opus
video_id: spacecraft-missions
title: 太空打工人：空间站、北斗、嫦娥五号各干啥
curriculum: script.json 未写 curriculum 字段；README 归为“拓展：认识人造天体”（与必修第一册“地球的宇宙环境”的对应关系为推断）
captured: '2026-11-12'
status: 成片存在（竖屏 1080×1920，91.90 s）+ 封面 PNG；技术检查/视觉检查/用户验收未记录
source_fingerprint: 3b327b2c6a9d0a1108c150aa04ae96f2b5d2eab0d710cec39c8cb3b63e5e897d
tags:
- video-creation
- production-capture
- geography-teaching
- canvas-frame-render
- edge-tts
---

# 太空打工人：空间站、北斗、嫦娥五号各干啥 · 五声线“工牌访谈”式竖屏动画

<!-- capture-source-sha256:3b327b2c6a9d0a1108c150aa04ae96f2b5d2eab0d710cec39c8cb3b63e5e897d -->

本文是根据项目文件做的材料回溯（`script.json`、`tts.py`、`render.mjs`、`lib.mjs`、`craft.mjs`、`chars.mjs`、`props.mjs`、`scenes-a~d.mjs`、`build/timeline.json`、`out/` 清单、证据包 ffprobe）。**未发现制作会话**。本次未运行任何项目脚本，未打开视频、音频、封面和 logo 图片。

本片沿用 [[atmosphere-insulation]] 的管线：`tts.py` 与保温片只差标题和 `TARGET`，`audio.mjs`、`fonts.py` sha256 相同，`render.mjs` 在保温片基础上加了左上角 logo、多角色字幕标签和太空背景。TTS、时间轴、逐帧管道和混音的通用说明见 [[atmosphere-insulation]] §5–§6。

## 1. 当前结论与制作路线

**状态**：`out/太空打工人：空间站、北斗、嫦娥五号各干啥.mp4`、同名 `.srt`、`out/封面.png` 都在；视觉检查、试听与用户验收**未记录**。

| 项 | 值 | 来源 |
| --- | --- | --- |
| 分辨率 / 画幅 | 1080×1920，9:16 竖屏 | ffprobe；`lib.mjs` `W, H` |
| 帧率 / 帧数 | 30 fps；2757 帧 | ffprobe；91.896×30 四舍五入 |
| 时长 | 91.900 s（timeline 91.896 s） | ffprobe；`build/timeline.json` |
| 编码 | H.264 yuv420p CRF 18 + AAC 48 kHz 立体声 192 kbps，faststart | ffprobe；`render.mjs`（与保温片相同） |
| 文件大小 | 20,174,471 字节（约 19.2 MiB） | 证据包 |
| 字幕 | 53 条 SRT + 画面烧录（带角色标签） | `out/*.srt`；`render.mjs` `subtitles()` |
| 封面 | `out/封面.png`，595,279 字节；取 `E('intro') − 0.2` 时刻，胶囊“亮出工牌，看看任务！” | 文件清单；`render.mjs`（未查看画面） |
| logo | `assets/logo-transparent.png`（115,977 字节，sha256 前缀 `3071b98be488`） | 证据包 |

**管线**：`script.json`（22 段、5 个声音）→ `tts.py`（`TARGET = 92`；脚本 `rate: 4`，最终 timeline `rate: 16`）→ `timeline.json` / SRT / `narration.wav` → `render.mjs` 加载 4 个场景模块（15 个场景），叠加 logo、章节条、带角色标签的字幕，逐帧管道送 ffmpeg → MP4。

## 2. 教学目标与课标对齐

- **课标字段**：无 `curriculum`、无 `based_on`。`README.md:27` 归入“拓展：中国空间站、北斗卫星导航系统、嫦娥五号的任务，认识人造天体”。
- **改编来源**：`source`“Geo Story Studio《中国空间站、北斗卫星、嫦娥探测器，分别在做什么？》v002 分镜（claim-047～049）”。
- **核心知识点**：认识航天器不能只看外形，要看“被送到哪里、完成什么任务”；三个任务关键词：**在轨实验、导航授时、探月采样**。
- **讲解结构**：
  1. **导入**：`hook` 豆豆看火箭升空：“航天器都在忙些什么？”→ `intro` 三位“太空打工人”亮出工牌（地点、任务两栏都是“？？？”）。
  2. **原理 / 案例**：天宫（`tgHi`~`tgLab`）绕地球飞、航天员长期在轨生活、微重力实验 → 北斗（`kidNav`~`bdTime`）三支队伍（卫星、地面站、终端），终端接收多颗卫星信号自行解算位置，定位 / 导航 / 授时 → 嫦娥五号（`kidMoon`~`ceSci`）2020 年月面采样 → 上升器起飞 → 环月轨道交会对接、样品转移 → 返回器回地球 → 交给科学家。
  3. **结论**：`recap` 工牌被填满：天宫“绕地球飞 / 在轨实验”，北斗“天+地+你 / 导航授时”，嫦娥“月球往返 / 探月采样”。
  4. **迁移**：`kidGet`/`outro` 认识航天器要问“它被送到哪里？要完成什么任务？”。
  5. **收尾**：`cta`“你还想认识哪位太空打工人？评论区告诉我！”。
- **科学边界**（`numbers.note`，原文要点）：不引用乘员数量、设计寿命、北斗精度、卫星数量、样品质量和精确日期；北斗定位由终端接收多颗卫星信号**自行解算**，**不画卫星拍摄用户**；嫦娥五号**不画着陆器整机飞回地球**，样品经“上升器 → 对接 → 返回器”转移。画面对应：`scenes-b.mjs:174` 横幅“终端自己算位置”；`scenes-c.mjs:101-112` 上升器对接、样品罐沿弧线转入返回器。
- **数字**：文案中唯一的数字是“2020 年”（`ceHi`）。资料名称：中国载人航天工程办公室《空间站系统介绍》；国家航天局 / 北斗网《北斗卫星导航系统》；国家航天局《嫦娥五号任务月球样品交接仪式在京举行》。

## 3. 技术与依赖

Node ESM + `@napi-rs/canvas`（`^1.0.11`，lock 1.0.13；本片额外用 `loadImage` 读 logo），无 npm scripts；Python `edge-tts`（运行时替换 `WSS_URL`）；`fonts.py` 依赖 fontTools；系统 ffmpeg。版本均未记录。

| 角色（voice key） | voice | 脚本设定 | 实际（叠加全局 +16） | 字幕标签 / 颜色 | 段数 |
| --- | --- | --- | --- | --- | --- |
| 旁白 zh | `zh-CN-XiaoxiaoNeural` | 0 | +16% | 无 | 12 |
| 豆豆 kid | `zh-CN-YunxiaNeural` | +4% / +6 Hz | +20% / +6 Hz | “豆豆” / mint | 4 |
| 天宫 tg | `zh-CN-YunjianNeural` | +2% | +18% | “天宫” / `#4cc9f0` | 2 |
| 北斗 bd | `zh-CN-YunxiNeural` | +6% | +22% | “北斗” / `#ffc93c` | 2 |
| 嫦娥 ce | `zh-CN-XiaoyiNeural` | +4% | +20% | “嫦娥” / `#ff8fab` | 2 |

`tts.py:32-35`：`TARGET = 92.0`（“about a minute and a half”）、`LEAD_IN = 0.35`、`GAP = 0.3`、`TAIL = 2.2`。

## 4. 镜头与时间轴

时间来自 `build/timeline.json`（22 段，53 条字幕）。章节条 5 格、每格宽度按文字实测：开工 / 天宫 / 北斗 / 嫦娥五号 / 下班总结，当前章用角色色高亮。置顶标题“空间站 · 北斗 · 嫦娥五号 / 太空打工人，各干啥？”。

| # | 场景 | 约时间 (s) | 口播段 | 画面要点 | 文件 |
| --- | --- | --- | --- | --- | --- |
| 1 | `hookScene` | 0–3.33 | hook | 火箭升空，豆豆仰望 | `scenes-a.mjs:20-50` |
| 2 | `introScene` | 3.33–7.60 | intro | 三张工牌（挂绳摆动）弹出，地点 / 任务栏“？？？” | `scenes-a.mjs:53-69` |
| 3 | `orbitScene` | 7.60–14.15 | tgHi, tgWhat | T 字形天宫自我介绍并绕地球飞 | `scenes-a.mjs:72-98` |
| 4 | `lifeScene` | 14.15–19.39 | tgLive | 舱内吃饭 / 睡觉 / 锻炼三格 | `scenes-a.mjs:100-…` |
| 5 | `labScene` | 19.39–25.52 | tgLab | “科学实验”：地面 vs 在轨对比——水流下落 vs 漂浮水球，泪滴火焰 vs 球形蓝焰；“微重力：地面很难长时间做到” | `scenes-b.mjs:8-89` |
| 6 | `helloBeidou` | 25.52–32.81 | kidNav, bdHi | 豆豆举手机问导航，北斗卫星和三颗伙伴登场 | `scenes-b.mjs:92-114` |
| 7 | `teamScene` | 32.81–38.48 | bdTeam | 三支队伍：卫星 / 地面站 / 终端 | `scenes-b.mjs:117-150` |
| 8 | `signalScene` | 38.48–44.58 | bdSig | 4 颗卫星发信号环，虚线汇聚到手机，手机“计算”后落定位针，“我在这儿！”“终端自己算位置” | `scenes-b.mjs:153-175` |
| 9 | `threeScene` | 44.58–51.27 | bdThree, bdTime | 定位 / 导航 / 授时三枚印章；三只乱转的钟在“对得”处同步 | `scenes-b.mjs:178-200` |
| 10 | `meetScene` | 51.27–58.06 | kidMoon, ceHi | 月面着陆的嫦娥五号组合体自我介绍 | `scenes-c.mjs:28-60` |
| 11 | `digScene` | 58.06–62.13 | ceDig | 机械臂铲取 + 钻取，上升器点火离开 | `scenes-c.mjs:62-89` |
| 12 | `dockScene` | 62.13–66.10 | ceDock | 环月虚线轨道，上升器与轨道器对接，样品罐沿弧线移入返回器 | `scenes-c.mjs:91-114` |
| 13 | `homeScene` | 66.10–72.93 | ceHome, ceSci | 返回器分离、再入发热、开伞落地；“交给科学家研究”+ 彩纸 | `scenes-c.mjs:117-…` |
| 14 | `recapScene` | 72.93–78.02 | recap | 三张工牌按词填入地点与任务 | `scenes-d.mjs:10-24` |
| 15 | `outroScene` | 78.02–91.90 | kidGet, outro, cta | “只看长相？”被划掉 →“去哪里？（轨道 · 月球 …）”“做什么？（实验 · 导航 · 采样 …）”两张问题卡；评论气泡“评论区告诉我！”+ 彩纸 | `scenes-d.mjs:27-…` |

最后 0.5 s 淡出；**无片尾品牌卡**。

## 5. 关键算法与实现

> 区分：视频**讲的知识**是三类航天器的任务；下面是**制作用的画法**。轨道、信号、对接都是卡通示意，不按真实尺度或轨道力学计算。通用机制见 [[atmosphere-insulation]] §5。

### 5.1 多角色字幕标签（`render.mjs` `subtitles()`，相对保温片的改动）

```js
const tag = TL.voices[seg.voice]?.tag ?? '';          // script.json voices.*.tag 原样写进 timeline.json
const tcol = TAG_COLOR[seg.voice];                     // { kid: mint, tg, bd, ce }
const tw = tag ? measure(ctx, tag, 36, ZH) + 34 : 0;   // 标签宽度参与字号反推
```

保温片里写死的 `isKid` 判断被改成读 `voices.*.tag`，所以只要在 `script.json` 给角色加 `tag`，字幕就会自动带“天宫 / 北斗 / 嫦娥 / 豆豆”彩色标签。章节条也改成按文字实测宽度排布，并用角色色高亮当前章。

### 5.2 “工牌”贯穿全片：占位 → 按词填写（`props.mjs:31-66`；`scenes-a.mjs:53-69`；`scenes-d.mjs:10-24`）

`badge()` 接收 `rows = [[label, value, tIn], …]`，每个值在 `tIn` 时刻“盖章”出现。开场三张工牌的值全部设为 `tIn = 1e9`（永不出现），另画“？？？”；结尾 `recapScene` 复用同一组件，把“在轨实验 / 导航授时 / 探月采样”的 `tIn` 绑定到 `at('recap', job)`——旁白念到哪个关键词，哪张工牌就填上。前后呼应完全由时间轴驱动。

### 5.3 北斗定位：信号环 + 汇聚线 + 终端自解算（`lib.mjs` `rings()`；`scenes-b.mjs:153-175`）

```js
const p = (t * speed + i / n) % 1;                     // rings(): 3 个扩散弧，越远越淡
ctx.globalAlpha = alpha * (1 - p);
ctx.arc(x, y, 20 + p * r, dir - spread, dir + spread);
// signalScene: 讲到“好几颗”时 4 条虚线从卫星伸向手机，lineDashOffset = -t*80 表示信号流动
ctx.lineTo(lerp(x, 528, lp), lerp(y + 60, 1090, lp));
```

讲到“算出”时手机进入 `calc` 状态并落下定位针，横幅“终端自己算位置”——落实 `numbers.note`“不画卫星拍摄用户”。授时用三只指向不同的钟：`time = lerp(spin, 10.25 + …, sync)`，在“对得”一词处从各自乱转插值到同一时刻并亮绿框（`scenes-b.mjs:189-198`）。

### 5.4 嫦娥五号四器分工（`craft.mjs:106-225`；`scenes-c.mjs:62-131`）

`craft.mjs` 把着陆器、上升器、轨道器、返回器、样品罐分别做成可组合的部件（着陆器带 `arm`/`drill` 进度参数，上升器带 `flame`，返回器带 `heat`/`chute`）。对接场景里上升器沿 `easeIO` 从月面升到轨道器右端，样品罐沿抛物线 `− sin(mv·π)·120` 从上升器移到左端返回器（`scenes-c.mjs:101-110`）；返回场景中再入发热 `heat = sin(π·prog)` 先升后降，随后开伞。顺序上严格按“采样 → 上升 → 对接转移 → 返回”，没有着陆器整机返回的画面。

### 5.5 左上角 logo 与太空背景（`lib.mjs:24-26`；`render.mjs` `brand()`、`background()`）

```js
const a = easeOut(prog(t, 0.3, 1.0));                 // 0.3–1.0 s 淡入
const x = 64, y = 76, w = 290, h = (w * sh) / sw;     // 竖屏规格，裁掉透明留白 sx150 sy96 sw1998 sh528
g = ctx.createRadialGradient(..., 20, ..., 250);      // 淡径向暗晕，不是实心底框
ctx.globalAlpha = 0.95 * a; ctx.drawImage(LOGO, sx, sy, sw, sh, x, y, w, h);
```

参数与 `AGENTS.md` §4 一致；封面帧用 `brand(ctx, 5)` 强制全显。背景 120 颗星按深度 `z` 以 `12·z` px/s 横向视差漂移，叠一团紫色径向“星云”。

## 6. 声音与字幕

- 五个声音见 §3；实际语速是脚本值再叠加全局 +16%。`build/tts_cache/` 88 个文件 = 22 段 × 2 种全局语速（+4% 与 +16%），时间集中在 10-23 15:20–15:23。
- 旁白混音（`highpass` + `loudnorm -16 LUFS`）、配乐（124 BPM 程序合成）、18 种程序音效、侧链压缩 + `alimiter 0.93` 与 [[atmosphere-insulation]] §6 相同。场景 `events` 共 76 条（a 18、b 26、c 19、d 13，按 `type:` 计数）。
- SRT 53 条：首条 `00:00:00,392 → 00:00:01,996 火箭送上天的航天器`，末条 `00:01:28,552 → 00:01:29,576 评论区告诉我！`。烧录字幕带角色彩色标签。
- 本次未试听、未测响度。

## 7. 迭代与问题

| 事项 | 证据 | 说明 |
| --- | --- | --- |
| 语速被 `TARGET` 从 +4% 一次推到 +16% | `tts.py:32`；`timeline.json` `rate: 16`；缓存 22×2 组 | 现行 `AGENTS.md` §2：默认 `TARGET = None`，不为压时长提速，内容太长优先精简文案。当时是否有用户时长要求**未记录** |
| 有 logo，但没有片尾品牌卡 | `render.mjs` `brand()` 注释引用“AGENTS.md §4”；`scenes-d.mjs` 只有 `outroScene`；`TAIL = 2.2` | logo 已按 §4 实现，§4 的“片尾品牌卡、角标同时淡出、`TAIL` 约 2.8 s”未实现（推断当时规则尚未包含品牌卡） |
| 结尾互动号召 | `script.json` `cta`；`scenes-d.mjs:60-66` 评论气泡 | `AGENTS.md` §4 要求品牌卡上不放号召文字；本片没有品牌卡，号召出现在正片旁白里，是否符合当前要求需人工判断 |
| 地球上的中国轮廓 | `chars.mjs:71` 注释“China-ish blob is the first land patch (no map precision intended)” | `AGENTS.md` §6 要求中国地图必须完整正确，拿不准时不画全图；本次未看画面，是否会被识别为中国轮廓**无法确认**，建议人工复核 |
| 缺 `curriculum` / `based_on` | `script.json` 键 | 不满足 `AGENTS.md` §1、§6；README 将其归为“拓展” |
| 复制自保温片 | `tts.py` 与保温片只差 2 处；`render.mjs` diff 见 §5.1、§5.5 | `lib.mjs` 新增 `rnd()`、`rings()`、`TAU`，删去 `sineLine`、`LETTER_COLORS` |
| README | `README.md:27` 已收录，标 1:32 | 与 ffprobe 91.90 s 一致 |

## 8. 复现入口

命令来自 `AGENTS.md`“常用命令”和 `render.mjs` 头部注释，**本次未执行**：

```bash
cd spacecraft-missions
python3 -m venv .venv && .venv/bin/pip install edge-tts   # fonts.py 另需 fonttools（推断）
npm install
.venv/bin/python tts.py          # → build/timeline.json、build/narration.wav、out/<标题>.srt
node render.mjs --still 8.0 14.2 # 可加 --safe
node render.mjs                  # → out/太空打工人：空间站、北斗、嫦娥五号各干啥.mp4
node render.mjs --cover          # → out/封面.png
```

先跑 `tts.py`；`lib.mjs` 加载时还需要 `assets/logo-transparent.png`。缓存含两轮全部 22 段，**推断**可离线重建。

## 9. 参考来源

| 来源 | 用途 | 状态 |
| --- | --- | --- |
| Geo Story Studio《中国空间站、北斗卫星、嫦娥探测器，分别在做什么？》v002 分镜（claim-047～049） | 改编底稿 | 只有名称，无 `based_on` 路径；未查看 |
| 中国载人航天工程办公室《空间站系统介绍》 | 天宫 | 项目文件中只提及名称，无 URL；本次未访问 |
| 国家航天局 / 北斗网《北斗卫星导航系统》 | 北斗三大组成、定位导航授时 | 同上 |
| 国家航天局《嫦娥五号任务月球样品交接仪式在京举行》 | 嫦娥五号采样返回 | 同上 |

证据包 `urls` 为空。

## 10. 证据与限制

已读文件（`spacecraft-missions/` 下，sha256 前 12 位）：`script.json` 44 行 `ae0de7c6e5e2`（全文）；`tts.py` 208 行 `2740b54e107a`（与保温片 diff）；`render.mjs` 228 行 `3a9859aea41e`、`lib.mjs` 236 行 `30aa88668c89`（与保温片 diff）；`scenes-a.mjs` 188 行 `2fbf180c88f6`、`scenes-b.mjs` 238 行 `68d4cf8a5d56`、`scenes-c.mjs` 180 行 `eba1d6fb4f56`、`scenes-d.mjs` 89 行 `65aedaa79845`（场景表与主要场景函数）；`craft.mjs` 267 行 `45ecd45e4f61`、`chars.mjs` 158 行 `db65a2a17785`、`props.mjs` 190 行 `1537eee4bebb`（函数目录与关键函数）；`audio.mjs` `ef790f152784`、`fonts.py` `a6ac7264e548`（与保温片相同）；`package.json` `6003b49eef8d`、`package-lock.json` `052473061455`；`build/timeline.json`；`out/*.srt` 首尾。成片仅有证据包 ffprobe。

限制：未发现制作会话；语速决策、文案确认、预览和用户反馈**未记录**；未打开成片、封面、logo，地球图案、logo 是否被遮挡**无法确认**；Node / Python / ffmpeg 版本未记录；git 只有初始化提交 `4831a07`（2026-10-23）。
