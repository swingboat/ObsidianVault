---
type: video-production-dossier
project: video-creation-claude-opus
video_id: em-waves
captured: '2026-11-12'
status: 已有完整 MP4 成片（约 187 秒横屏 ELI5 物理科普动画）；用户验收未记录
source_fingerprint: 695f76ce0bc799cc5f279c6f217259eed6784d7dfa38681b43527189fd9095c3
tags:
- video-creation
- production-capture
- canvas-frame-render
- tts
- science-explainer
---

# 电磁波 · 看不见的大家族 · Canvas 逐帧物理图解 + Edge TTS 驱动时间轴

<!-- capture-source-sha256:695f76ce0bc799cc5f279c6f217259eed6784d7dfa38681b43527189fd9095c3 -->

本次为材料回溯与归档。**未找回制作对话，结论来自项目文件**（源码、`script.json`、`build/tts_cache`、`out/` 成片与字幕的媒体头）。本轮未运行任何项目脚本，未重新渲染、未重新合成语音。同一项目的 `em-waves-douyin` 是本片的抖音竖屏改编版，见该档案。

## 1. 当前结论与制作路线

**状态：`out/电磁波-看不见的大家族.mp4` 是完整成片**（ffprobe 可读；视觉检查、用户验收未记录）。它也是 `em-waves-douyin` 改编的母版。

| 项 | 值 | 来源 |
| --- | --- | --- |
| 分辨率 / 画幅 | 1920×1080，16:9 横屏 | ffprobe；`lib.mjs` `W = 1920, H = 1080` |
| 帧率 / 帧数 | 30 fps，5608 帧 | ffprobe |
| 时长 | 186.933 s | ffprobe；本次用缓存复算 186.927 s，`round(186.927×30)=5608` 帧 |
| 视频 | H.264 High，yuv420p，CRF 18，preset medium，faststart | ffprobe；`render.mjs` ffmpeg 参数 |
| 音频 | AAC LC 48 kHz 立体声 192 kbps | ffprobe；`render.mjs` |
| 文件 | 约 60.0 MB，总码率约 2.57 Mbps | fingerprints.json；ffprobe |
| 字幕 | `out/电磁波-看不见的大家族.srt`，82 条；画面内也烧录 | 文件；`render.mjs` `subtitles()` |

一句话管线：

> 自定 ELI5 物理选题 + 权威资料核数 → 手写对话式口播稿 `script.json`（36 段：「姐姐」讲解 + 「豆豆」提问）→ `tts.py` 用 Edge TTS 合成并取逐词时间戳 → `build/timeline.json` + `narration.wav` + SRT → `scenes-a..d.mjs` 用词锚点编排 12 个场景、程序化物理图形与音效事件 → `render.mjs` 用 `@napi-rs/canvas` 逐帧画 RGBA 并经管道送 ffmpeg → `audio.mjs` 合成配乐/音效，与人声侧链闪避混音 → H.264 + AAC MP4。

全片没有任何位图素材或生成模型画面：太阳、地球、光子吉祥物、小朋友、收音机、发射塔、路由器、气球、磁铁、指南针等全部由 Canvas 路径代码绘制；电磁波、人浪、频谱等由数学公式逐帧计算。

## 2. 创作目标与需求

- 项目内**没有 `prompt.md`、`plan/` 或制作记录**；原始需求原文未记录。
- `script.json:2-3` 摘录：

  > `"title": "电磁波-看不见的大家族"`
  > `"source": "ELI5 图解《电磁波 · 光与无线信号的共同语言》；资料：OpenStax Plane Electromagnetic Waves、NASA Anatomy of an Electromagnetic Wave、NASA The Electromagnetic Spectrum、NIST SI 定义常数（c = 299 792 458 m/s）"`

  即：面向小朋友/零基础的「像给 5 岁孩子讲」（ELI5）物理图解，标题（推断为暂定副题）「光与无线信号的共同语言」，并点名了四份事实依据。
- `script.json:4-10` 还写了一个 `numbers` 字段，作为口播中数字的**核算底稿**（摘录）：

  ```json
  "c": "299 792 458 m/s ≈ 每秒 30 万千米；一秒绕地球约 7.5 圈（赤道周长 40 075 km）",
  "300MHz": "λ = c / f ≈ 0.999 m",
  "600MHz": "λ ≈ 0.50 m",
  "green": "λ ≈ 550 nm → f ≈ 5.45 × 10¹⁴ Hz（约 545 万亿次/秒）",
  "photon": "E光子 = hf"
  ```

  `tts.py` 不读取这个字段，它只是写给人（和模型）看的事实核对表；本次复核这些数值正确（299 792.458 km ÷ 40 075 km ≈ 7.48 圈；299.792458 / 300 ≈ 0.999 m；c / 550 nm ≈ 5.45×10¹⁴ Hz）。
- 叙事结构（来自 `script.json` 段落分组）：钩子（阳光、收音机、WiFi 是一家人）→ 什么是「场」→ 电荷晃动产生电磁波、E⊥B⊥传播方向 → 不需要介质 / 光速 → 人浪类比频率与波长 → c = λf 与 300/600 MHz 演示 → 电磁波谱家族 → 可见光只是窄窄一段（550 nm 绿光）→ 单个光子能量 vs 一束波总能量 → 收音机如何把歌声送到 → 结尾回扣。
- 风格（来自代码）：深夜蓝「实验室」背景 + 缓慢平移的网格 + 半透明正弦飘带（`render.mjs` `background()`），左上角「物理 · ELI5 | 电磁波」角标；字幕中小朋友台词带粉色「豆豆」标签。

## 3. 技术与依赖

| 类别 | 内容 | 证据等级 |
| --- | --- | --- |
| 渲染 | Node.js ESM（`package.json` `"type": "module"`）+ `@napi-rs/canvas` **1.0.13**（darwin-arm64 同版本） | `package-lock.json` 锁定；`render.mjs` import |
| 编码 / 混音 | ffmpeg（libx264、aac、`sidechaincompress`、`alimiter`、`loudnorm`、`amix`、`adelay`） | 代码中调用；版本未记录 |
| TTS | Python + `edge-tts` + `aiohttp`；端点覆盖到 `api.msedgeservices.com` | `tts.py:15-25`；版本未记录 |
| 字体 | 圆体-简 粗体（`YuanB`）、苹方-简 中粗（`PingB`）、Arial Rounded Bold（`RoundB`，用于 E/B/λ/f 等符号）、Arial Unicode、Hiragino Sans GB | `lib.mjs` 字体注册；`build/fonts/` 两个 TTF 存在（2026-10-21 20:56 抽取） |
| 旁白「姐姐」 | `zh-CN-XiaoxiaoNeural`，rate 0 / pitch 0 | `script.json:12`；缓存命中 rate=0 |
| 提问「豆豆」 | `zh-CN-YunxiaNeural`，rate +4% / pitch +6 Hz | `script.json:13`；缓存命中 rate=4 |
| 全局语速 | +0%（一次通过，未触发自动加速） | 由缓存复算（§6） |
| 配乐 / 音效 | `audio.mjs` 程序合成；比前作新增 `zap`、`wobble`、`static` 三种音效 | 代码中存在；成片有音轨（本轮未试听） |
| 硬件 | 未记录；依赖 macOS 系统字体路径 | 推断为 macOS |

与同项目 `ai-3d-game`（2026-10-16）对比，本片（2026-10-21）沿用同一套骨架，`tts.py` 只改了 4 个常量和一行注释（`TITLE`、`TARGET 240→200`、`LEAD_IN 1.2→1.6`、`TAIL 2.6→3.0`），`fonts.py` 完全相同；`render.mjs` 改为 `Promise.all` 动态导入 `scenes-a..d.mjs` 并统一导出名 `scenes` / `events`，去掉了角落叙述者角色；`lib.mjs` 换成电磁波配色（`eF` 电场黄 `#ffd23f`、`bF` 磁场青 `#4cc9f0`、`grid` 网格色）并新增 `elastic` 缓动、`shakeOf`、`sineLine`。`lib.mjs` 的配色表中还有 `sun/earth/land/moon` 等天体颜色，与 `ai-3d-game/package.json` 里遗留的包名 `geo-celestial-bodies` 呼应——**推断**这套模板最早来自一个天体主题的作品（不在本仓库中）。

## 4. 镜头与时间轴

时间来源：`build/timeline.json` 已清理；下表时间是本次按 `tts.py` 公式（`LEAD_IN=1.6`、逐段 `gap`、末词结束 +0.12 s、`TAIL=3.0`）用 `build/tts_cache`（+0% 一批）的词时间戳**离线复算**，总长 186.93 s 与成片吻合。场景边界 = 代码 `from/to` 表达式代入复算值，相邻场景交叉淡化 0.35 s（`render.mjs:18`）。

| # | 场景 id | 约时间 (s) | 口播段 | 画面与讲解内容 | 实现入口 | 关键参数 |
| --- | --- | --- | --- | --- | --- | --- |
| 1 | `intro` | 0.0–13.6 | hook(豆豆), family, draw | 豆豆发问，太阳/收音机/路由器依次弹出并合成「全家福」，大字「电磁波」，光子吉祥物飞过，「画出来」配写字音 | `scenes-a.mjs:11-49` | 阳光/收音机/WiFi 三个 boing 按词触发 |
| 2 | `field` | 13.6–31.8 | fieldQ, balloon, magnet, fieldDef | 大字「场」盖章震屏；摩擦气球隔空吸起 9 片彩纸（电场 E）；磁铁靠近指南针自己转（磁场 B）；「隔空作用!」 | `scenes-a.mjs:51-112` | 纸屑 `BITS` 9 片；`stamp shake: 10` |
| 3 | `wave` | 31.8–49.3 | shake, waveDef, perp | 天线中「+」电荷上下振荡，向右发出 E（黄实线，竖直）与 B（青虚线，斜向表示朝观众的第三维）；「电磁波！」；E/B/前进方向三轴示意「两两垂直」；1100 px 处同一点 E、B 箭头「同起同落」 | `scenes-a.mjs:114-191` | `LAM=380, V=300 px/s, AE=170, AB=150, ZX=-0.55, ZY=0.38` |
| 4 | `vacuum` | 49.3–64.7 | kidAir(豆豆), vacuum, sound | 太阳→地球之间「几乎真空：没有空气」，光子照样抵达；太空里喊「啊——!」对方只有「？」 | `scenes-b.mjs:19-79` | — |
| 5 | `speed` | 64.7–71.7 | speed | 光子带速度线横穿；「每秒约 30万 km」；光子绕地球虚线轨道计圈「1 秒绕地球 x.x 圈」到 7.5 | `scenes-b.mjs:80-125` | 8 个 tick 音均分在「一秒钟」到「七圈半」之间 |
| 6 | `stadium` | 71.7–87.3 | stadium, stadium2, freq, lambda | 18 个观众做人浪：每人只在原地起立/坐下，白色虚线正弦与绿色「波浪向前跑」箭头跟着波峰走；第 9 人高亮讲「频率 f · 原地每秒振几次」并计数「站起 × n」，再标「波长 λ」 | `scenes-b.mjs:127-199` | `LAM=540, VW=250 px/s, BASE=800` |
| 7 | `formula` | 87.3–105.7 | formula, demo, kidFast(豆豆), noFast | 公式 c = λ × f 逐符号随词弹出；3 米标尺内正弦波，频率 300→600 MHz 连续变化，读数卡实时显示 MHz 与米；「频率 ×2 → 波长 ÷2」；豆豆担心更快，答「速度永远是 c」 | `scenes-b.mjs:201-…` | `λ_px = (299.792458 / f_MHz) × 400 px/米` |
| 8 | `spectrum` | 105.7–124.9 | lineup, members, uses | 一条从左到右波长指数收缩的「啁啾」波（频率低·波长长 → 频率高·波长短），七个波段逐词点亮；用途图标：发射塔、路由器、遥控器、X 光手 | `scenes-c.mjs:9-81` | 七段音阶 `72 + [0,2,4,5,7,9,11]` |
| 9 | `visible` | 124.9–134.8 | visible, green | 放大可见光窄带（红→紫）「人眼可见的一小段」；550 nm 绿光「每秒振动 ≈ 545 万亿次」 | `scenes-c.mjs:83-138` | — |
| 10 | `energy` | 134.8–150.3 | kidEnergy(豆豆), photon, total | 低频与高频光子对比，「E光子 = h f」；天平：左盘 12 个低频光子、右盘 1 个高频光子，天平倾斜，「强度：光子多不多」「时间：照了多久」「不能只比频率！」 | `scenes-c.mjs:140-…` | 天平倾角 `-0.16·easeIO` |
| 11 | `radio` | 150.3–170.1 | radioQ, radio1, radio2 | 「吹过来的空气？」打叉；麦克风→「电信号」→发射塔，振幅随歌声变化的载波（AM）飞向收音机天线，「信息藏在波的变化里」；收音机「解出信号」，喇叭推动空气、音符飘出 | `scenes-d.mjs:25-104` | 载波包络 `0.35+0.65·|sin((x−220t)/90)|` |
| 12 | `outro` | 170.1–186.9 | outro, outro2, kidEnd(豆豆) | 太阳、收音机、路由器组成「电磁波大家族」，豆豆与光子；彩纸 + tada | `scenes-d.mjs:106-…` | `to: DURATION` |

全局层（`render.mjs:102-129`）：背景（径向渐变 + 每秒左移 12 px 的 80 px 网格 + 正弦飘带 + 90 个闪烁点）→ 震屏（事件里带 `shake` 的 stamp/slam 触发衰减抖动；本片实际使用了 `shake: 6–10`）→ 场景层（进入 0.94→1、退出 1→1.05 的缩放淡化）→ 角标 → 烧录字幕 → 首尾 0.4/0.6 s 淡入淡出。与 ai-3d-game 不同，本片**没有常驻叙述者形象**，「姐姐」只有声音，豆豆只在提问场景出现。

## 5. 关键算法与实现

> 区分：视频**讲解的知识**是电磁波的物理（场、E⊥B⊥k、无需介质、c = λf、电磁波谱、E光子 = hf、调幅广播）。本节是**制作视频所用的算法**：Canvas 2D 绘图 + 用简单数学公式逐帧生成「示意性」物理动画 + 音频 DSP。画面是教学示意，不是电磁场数值仿真。

### 5.1 管线：「代码逐帧渲染 + TTS 驱动时间轴」

```text
script.json ──tts.py──▶ build/tts_cache/<sha1(voice|rate|pitch|text)[:16]>.mp3/.json   逐句缓存 + 逐词时间戳
            │          build/seg<i>.mp3 ─ffmpeg adelay/amix/highpass/loudnorm─▶ build/narration.wav
            │          build/timeline.json（每段 start/end/words/cues + 总时长）
            └────────▶ out/电磁波-看不见的大家族.srt
lib.mjs 读 timeline.json → S(段)/E(段)/at(段, 词) 词锚点
scenes-a..d.mjs 导出 scenes[{id, from, to, draw(ctx,t)}] 与 events[{type, t, …}]
render.mjs: 帧 i → renderFrame(i/30) → getImageData RGBA → ffmpeg stdin（背压 drain）
            audio.mjs: buildMusic → music.wav；buildSfx(events) → sfx.wav
            ffmpeg: 旁白 + 侧链闪避的音乐 + 音效 → alimiter → libx264 CRF18 + AAC 192k
```

要点（与 ai-3d-game 相同的机制，此处独立说明）：

1. **语音先行**：`tts.py` 用 `edge_tts.Communicate(..., boundary="WordBoundary")` 拿到每个词的 offset/duration（100 ns 单位），并用 `text.find(word, cursor)` 记录词在原文中的字符下标 `idx`。
2. **段落排布**：第 i 段起点 `t_i = LEAD_IN + Σ(末词结束 + 0.12 + gap_j)`；用末词结束而不是 mp3 长度，避开 mp3 尾部约 0.6 s 静音（`tts.py:151` 注释）。
3. **片长上限与自动语速**：`TARGET = 200`。若超出则按 `ceil((Σspeech / available − 1)×100)+1` 一步提高语速后重合成。本片首轮 186.93 s 即通过，语速保持 +0%。
4. **词锚点**：场景里一律写 `at('perp', '垂直') - 0.15` 这类表达式（`lib.mjs:31-40`）；子串找不到时 `word()` 直接抛错。改稿只需重跑 `tts.py`，画面、字幕、音效自动重新对齐。
5. **字幕**：段内有 `captions` 用人工分句，否则按标点切成 ≤14 字；每条字幕起止取所覆盖词的真实时间（`tts.py:106-136`）。
6. **纯函数帧**：`draw(ctx, t)` 只依赖 t，`node render.mjs --still 40 95` 可任意抽帧预览（`render.mjs:3, 135-140`）。
7. 场景文件拆为 4 个（192–300 行），每个文件导出同名 `scenes`/`events`，`render.mjs:14-17` 用 `Promise.all` 动态导入并 `flatMap` 拼接。

### 5.2 电磁波场景：带「波前」的 E/B 斜投影（`scenes-a.mjs:115-191`）

```js
const AX = 330, AY = 520, LAM = 380, V = 300, AE = 170, AB = 150, ZX = -0.55, ZY = 0.38;
const OM = 2π·V / LAM;                               // ω = 2πv/λ，保证波形以 V px/s 向右传播
front   = AX + V·max(0, t − t0)                      // t0 = 「电场」一词前 0.3 s，波前从天线出发
fieldAt = x > front ? 0 : clamp((front − x)/120) · sin(OM(t − t0) − 2π(x − AX)/LAM)
E 点: (x, AY − AE·fieldAt(x))                        // 竖直
B 点: (x + ZX·AB·fieldAt(x), AY + ZY·AB·fieldAt(x))  // 斜向 (−0.55, 0.38) 表示指向观众的 z 轴
```

- 同一个 `fieldAt` 同时驱动 E 和 B，所以二者天然同相（「同起同落」），这正是平面电磁波在真空中的性质；B 用斜向投影 + 虚线表达第三个维度，配合右上角三轴示意「两两垂直」。
- 每隔 28 px 画一根从轴线到曲线的细「场矢量」，再用 4 px 步长画粗曲线；讲到「上下 / 左右」时对应曲线线宽 ×1.5 并加辉光。
- 波前前沿 120 px 内振幅线性渐入，避免波前处出现硬切。
- 天线上的「+」电荷位置 `AY − 150·shake·sin(OM(t − t0))`，与波源同频。

### 5.3 人浪：横波「原地振动、波形前进」（`scenes-b.mjs:127-199`）

`raise(t, x) = (1 + sin(ωt − kx)) / 2`，`k = 2π/540`，`ω = k·250`。18 个人只改变自身高度（起立 0→1），整体形成以 250 px/s 前进的波形；`tt = t·run` 让人浪在「人浪」一词附近平滑启动。绿色箭头位置由当前波峰解析解求出：`xc = (ωt − π/2 − 2πn)/k`。

### 5.4 c = λf 标尺（`scenes-b.mjs:201-…`）

3 米标尺，每米 400 px。频率在「翻」到「半米」两个词之间从 300 平滑插值到 600 MHz：`λ_px = (299.792458 / f) × 400`，读数卡实时显示 `round(f) MHz` 与 `λ.toFixed(2) 米`——画面上的数字是用真实光速**算出来的**，所以 300 MHz 显示 1.00 米、600 MHz 显示 0.50 米，与口播「约 1 米 / 半米」一致。

### 5.5 电磁波谱「啁啾」曲线（`scenes-c.mjs:23-37`）

相位 `ph(u) = (2πL / (l0·k))·(e^{k·u} − 1)`，`u ∈ [0,1]`，`k = 3.4`，`l0 = 340 px`。瞬时波长 `= l0·e^{−k·u}`，从左端 340 px 指数缩短到右端约 11 px，用一条线直观表达「频率低·波长长 → 频率高·波长短」；描边用七个波段颜色的线性渐变，`reveal` 控制从左到右揭开。注意它只是**示意**，真实电磁波谱跨越十几个数量级，不能按比例画。

### 5.6 调幅广播与能量天平（`scenes-d.mjs:56-75`、`scenes-c.mjs:176-200`）

- 电信号与载波都用 `sineLine(..., {env})` 画：`env` 是随 x 和 t 变化的包络，载波 `amp·(0.35 + 0.65·|sin((x − 220t)/90)|)`，包络以 220 px/s 向接收端移动，表示「信息刻在振幅变化里」（代码注释写明是 AM）。
- 能量对比用天平：左盘 12 个低频光子、右盘 1 个高频光子，横梁倾斜 −0.16 rad，表达「总能量还取决于强度（光子数）与时间」。

### 5.7 角色与口型

`kid()`（豆豆，`chars.mjs:115`）带背包、上下漂浮；`photon()`（`chars.mjs:159`）是带正弦尾巴、可设频率 `freq` 的发光球吉祥物；`talkLevel(t, ids)`（`lib.mjs:178-185`）在词发音区间内返回 `0.45 + 0.55|sin 22t|` 驱动嘴巴开合。

## 6. 声音与字幕

### 6.1 配音

| 角色 | 声线 | 语速 / 音高（最终） | 段数 | 证据 |
| --- | --- | --- | --- | --- |
| 姐姐（旁白） | `zh-CN-XiaoxiaoNeural` | +0% / +0 Hz | 31 | `script.json:12`；缓存命中 rate=0 |
| 豆豆（提问的小朋友） | `zh-CN-YunxiaNeural` | +4% / +6 Hz | 5（hook、kidAir、kidFast、kidEnergy、kidEnd） | `script.json:13`；缓存命中 rate=4 |

- 对话式结构：豆豆的问题都是常见误解（要不要空气？频率翻倍会更快吗？频率越高能量越大吗？），姐姐用「不用！」「不会！」「对单个光子来说，没错」逐个纠正——这是 ELI5 科普稿的核心写法。
- 旁白混音：各段 mp3 → 48 kHz → `adelay` 到绝对起点 → `amix normalize=0` → `highpass=f=80` → `loudnorm=I=-16:TP=-1.5:LRA=11`（`tts.py:176-190`）。−16 LUFS 是代码目标；成片实测响度未记录。
- `LEAD_IN = 1.6 s`、`TAIL = 3.0 s`（比 ai-3d-game 各长 0.4 s，留给开场「?」动画和结尾彩纸）。

### 6.2 字幕

- `out/电磁波-看不见的大家族.srt`：82 条，首条 `00:00:01,648 --> 00:00:04,785 姐姐，阳光、收音机、还有WiFi`（= LEAD_IN 1.6 + 首词 0.048 s），末条 `00:03:01,812 --> 00:03:03,807 我每天都泡在电磁波里呀！`（kidEnd 无 `captions`，由 `split_caption` 自动切成「原来」「我每天都泡在电磁波里呀！」两条）。
- 烧录字幕（`render.mjs:58-84`）：y=960 深色胶囊、52 px 字；豆豆台词带粉色「豆豆」标签、字色 `#ffd6e2`。

### 6.3 配乐与音效（`audio.mjs`）

- 配乐 `buildMusic` 与 ai-3d-game 完全相同：112 BPM、C–G–Am–F、Karplus–Strong 拨弦琶音 + 根音低音 + 7–9 kHz 沙锤 + 隔小节钟琴，种子 42，首 1.2 s 淡入、尾 2.5 s 淡出。
- 音效 18 种：沿用 15 种（pop、note、ding、sparkle、whoosh、boing、slam、stamp、tick、scribble、buzz、alarm、tada、confetti、wind）并为本题新增 3 种（`audio.mjs:135-148`）：
  - `zap`：1800→400 Hz 下滑方波 + 900 rad/s 抖动 + 5000→2000 Hz 带通噪声，用于「电场」「电信号」「五百万亿次」等带电/高频的词；
  - `wobble`：频率 `f·(1 + 0.12·sin(2π·rate·u))` 的颤音正弦，`f`/`rate` 可调，用来让声音「振动」——讲频率翻倍时先 300 Hz/4 Hz，再 600 Hz/8 Hz，**声音本身也翻倍**；
  - `static`：2.5 kHz 带通噪声 + 快速调幅，模拟收音机电流声（「刻」进电磁波时）。
- 总混音（`render.mjs:149-151`）：音乐 `volume=0.75`（ai-3d-game 为 0.7）经旁白侧链压缩（threshold 0.05、ratio 5、attack 20 ms、release 400 ms），音效 0.5，三路 `amix normalize=0` 后 `alimiter=0.93`。
- 检查范围：仅 ffprobe 确认 48 kHz 立体声 AAC；本轮未试听、未测响度。

## 7. 迭代、问题与修正

项目无 `plan/`、日志或对话，以下来自代码与文件痕迹：

| 事项 | 证据 | 说明 |
| --- | --- | --- |
| 事实核对 | `script.json:3-10` | 把资料来源和关键数字写进脚本旁的 `numbers` 字段，口播「约 30 万千米」「七圈半」「约 1 米 / 半米」「五百多万亿次」都可回查 |
| 防误解的措辞 | `script.json` `photon`/`total` 段 | 「对单个光子来说，没错」+「一整束波总共带多少能量，还要看强度和时间」——主动纠正「频率高 = 能量大」的简化说法 |
| Edge TTS 端点 403 | `tts.py:19-25` | 与 ai-3d-game 相同的端点覆盖补丁 |
| TTS 合成一次通过 | 缓存只有 rate=0 一批（36 对 mp3/json，20:55:02–20:55:57），无其他文本版本 | 首次合成后口播稿未再改动，或旧缓存曾被清理——**无法区分** |
| 模板复用 | 与 ai-3d-game 的 diff | 只改常量、配色、场景文件；新增 3 种音效与 `sineLine` 等波形工具 |

文件时间（非日志）：TTS 2026-10-21 20:55；字体抽取 20:56；成片写入 21:10:27。**推断**全片渲染 + 编码在约 14 分钟以内。画面迭代次数、预览帧、用户反馈：**未记录**。

## 8. 可复用制作经验

1. **在脚本里放「数字核对表」**：`numbers` 字段不参与程序，但把每个口播数字的来源与算式固定下来，便于审稿与改编复用（依据：`script.json:4-10`；抖音版直接沿用）。
2. **屏幕上的数值用真实常数现算**：标尺读数由 `299.792458 / f` 计算，口播、字幕、画面三者不会互相矛盾（依据：`scenes-b.mjs` formulaScene）。
3. **同一个场函数驱动 E 和 B**：用一个 `fieldAt(x)` 同时生成两条曲线，物理上的同相关系自动成立，不需要手动对齐（依据：`scenes-a.mjs:121-145`）。
4. **2D 画第三维：斜投影 + 虚线 + 三轴示意**：`(ZX, ZY) = (−0.55, 0.38)` 的斜向位移即可让观众理解「垂直于纸面」（依据：`scenes-a.mjs:115`）。
5. **用「人浪」解释横波**：只改变每个人的高度，就能同时展示「原地振动」（频率）和「波形前进」（波长、波速）（依据：`scenes-b.mjs:127-199`）。
6. **让音效也表达概念**：频率翻倍时 `wobble` 音高与颤音速率同时翻倍，声音成为第二条解释通道（依据：`scenes-b.mjs` events `demo`）。
7. **提问角色专门说出误解**：小朋友的 5 句台词都是常见误区，由旁白当场纠正，结构清晰且适合剪成短视频（依据：`script.json`；抖音版保留了其中 3 句）。
8. **模板化：新片只改常量与场景文件**：`tts.py`、`fonts.py`、`audio.mjs`、`render.mjs` 主干几乎不动（依据：与 ai-3d-game 的 diff）。

## 9. 复现入口

命令来自源码注释（`tts.py:4`、`render.mjs:2-3`、`fonts.py:4`），**本轮均未执行**。

```bash
cd em-waves
npm install                          # @napi-rs/canvas 1.0.13（package-lock 锁定）
python3 -m venv .venv && .venv/bin/pip install edge-tts aiohttp fonttools   # 版本未记录
.venv/bin/python tts.py              # → build/seg*.mp3, narration.wav, timeline.json, out/电磁波-看不见的大家族.srt
node render.mjs --still 3.2 7.6      # 可选：抽帧预览
node render.mjs                      # → build/music.wav, sfx.wav, out/电磁波-看不见的大家族.mp4
```

- 必须先跑 `tts.py`：`lib.mjs:24` 在加载时读取 `build/timeline.json`（当前已清理）。
- `build/tts_cache/` 保留了全部 36 段（+0% / 豆豆 +4%）的 mp3 + 词时间戳；首轮即命中缓存且总长 ≤200 s，**推断**可离线重建时间轴与旁白（仍需安装 `edge-tts` 以通过 import）。
- 系统依赖：ffmpeg（libx264）；macOS 系统字体（`build/fonts/` 已有抽取好的 TTF）。
- 修改入口：口播/停顿 → `script.json`；片长上限与首尾留白 → `tts.py:32-35`；配色 → `lib.mjs:58-63`；物理动画参数 → 各 `scenes-*.mjs` 顶部常量（如 `scenes-a.mjs:115`、`scenes-b.mjs:128`、`scenes-b.mjs:202`）；配乐/音效 → `audio.mjs`；混音比例 → `render.mjs:149-151`。

## 10. 参考来源

`script.json:3` 只写了资料名称，**没有链接**；本次未联网查找或访问这些资料，下表仅记录本地声明。

| 来源（按 script.json 原文） | 类型 | 声明用途 | 对应镜头 | 访问状态 |
| --- | --- | --- | --- | --- |
| OpenStax「Plane Electromagnetic Waves」 | 技术资料（教材） | 平面电磁波性质：E⊥B⊥传播方向、同相 | 场景 3 `wave` | 无 URL，未访问 |
| NASA「Anatomy of an Electromagnetic Wave」 | 技术资料（科普） | 电磁波结构、无需介质、波长/频率 | 场景 3–7 | 无 URL，未访问 |
| NASA「The Electromagnetic Spectrum」 | 技术资料（科普） | 电磁波谱家族与用途 | 场景 8–9 | 无 URL，未访问 |
| NIST SI 定义常数（c = 299 792 458 m/s） | 技术资料（标准） | 光速数值 | 场景 5、7 | 无 URL，未访问 |
| 「ELI5 图解《电磁波 · 光与无线信号的共同语言》」 | 选题 / 风格说明 | 讲解定位与标题 | 全片 | 本地文字，未见外部出处，关系未确认 |

未发现外部参考视频或 X 帖子。

## 11. 证据与限制

| 文件 | 行数 | sha256[:16] | 读取程度 |
| --- | --- | --- | --- |
| `em-waves/script.json` | 61 | `59e590fa3b0fb21a` | 全文 |
| `em-waves/tts.py` | 207 | `254920b1b2e4b84e` | 与 ai-3d-game 全文 diff（仅 4 常量 + 注释不同） |
| `em-waves/render.mjs` | 167 | `e697ed10258be402` | 全文 |
| `em-waves/lib.mjs` | 227 | `43d75dc35fd171bd` | 与 ai-3d-game diff + 后半全文 |
| `em-waves/audio.mjs` | 217 | `6629719cde28e60f` | 与 ai-3d-game diff（新增 3 种音效） |
| `em-waves/fonts.py` | 36 | `a6ac7264e548bd3d` | 与 ai-3d-game 相同哈希 |
| `em-waves/scenes-a.mjs` | 227 | `d7de20a7514e5acb` | 场景 3 全文、导出表、函数目录 |
| `em-waves/scenes-b.mjs` | 300 | `2bf6a27bcdb95d99` | 场景 5 开头、场景 6–7 大部分、导出表 |
| `em-waves/scenes-c.mjs` | 236 | `e5d824a71f08e275` | 常量、啁啾函数、天平部分、导出表 |
| `em-waves/scenes-d.mjs` | 192 | `5821fa06f72dac02` | 调制部分、导出表 |
| `em-waves/chars.mjs` / `props.mjs` | 180 / 187 | `afba16a6a4cecb24` / `dc6941e39e50ae61` | 函数目录、kid/photon 开头 |
| `em-waves/package.json` / `package-lock.json` | — | `1f0e220b068d5087` / `35cc6ca4259ce99c` | 全文 / 包版本 |
| `em-waves/out/电磁波-看不见的大家族.srt` | 82 条 | `c450cafae7e70402` | 首尾条目、条数 |
| `em-waves/out/电磁波-看不见的大家族.mp4` | — | 59,976,203 字节 | 仅 ffprobe 媒体头 |
| `em-waves/build/tts_cache/*.json` | 36 个 | 见 fingerprints.json | 以哈希键匹配 script 并复算时间轴 |

缺失与限制：

- 无 `prompt.md`、`plan/`、制作对话：需求原文、画面迭代与反馈**未记录**。
- `timeline.json`、`narration.wav`、`music.wav`、`sfx.wav`、预览帧已清理；§4 时间为复算值。
- Python 依赖、Node、ffmpeg 版本与硬件未记录。
- 参考资料仅有名称、无链接，本次未访问，未核实画面是否逐条对应资料。
- 成片仅 ffprobe 技术检查；**未做视觉检查、未试听、未测响度；用户验收未记录**。
