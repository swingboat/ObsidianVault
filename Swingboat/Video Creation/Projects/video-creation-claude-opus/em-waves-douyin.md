---
type: video-production-dossier
project: video-creation-claude-opus
video_id: em-waves-douyin
captured: '2026-11-12'
status: 已有完整竖屏 MP4（约 78 秒）+ 封面 PNG + 发布文案；是否已发布与用户验收未记录
source_fingerprint: 6a5d0d3ff231b4555a87f48c7bd417555ea79de70f12dfafc81c422c8394850e
tags:
- video-creation
- production-capture
- canvas-frame-render
- tts
- vertical-adaptation
---

# 电磁波 · 抖音竖屏版 · 横屏科普片的 9:16 短视频改编（Canvas 逐帧 + TTS 时间轴）

<!-- capture-source-sha256:6a5d0d3ff231b4555a87f48c7bd417555ea79de70f12dfafc81c422c8394850e -->

本次为材料回溯与归档。**未找回制作对话，结论来自项目文件**（源码、`script.json`、`build/tts_cache`、`out/` 中的成片、字幕、封面、发布文案）。本轮未运行任何项目脚本，未重新渲染、未重新合成语音。

## 1. 当前结论与制作路线

**状态：交付包完整**——`out/电磁波-抖音竖屏版.mp4`（成片）、`out/电磁波-抖音竖屏版.srt`、`out/封面.png`、`out/发布文案.txt`。是否实际发布到抖音、播放数据与用户验收均**未记录**。

| 项 | 值 | 来源 |
| --- | --- | --- |
| 分辨率 / 画幅 | **1080×1920，9:16 竖屏** | ffprobe；`lib.mjs:7` `W = 1080, H = 1920` |
| 帧率 / 帧数 | 30 fps，2336 帧 | ffprobe |
| 时长 | 77.870 s | ffprobe；本次用缓存复算 77.87 s，一致 |
| 视频 | H.264 High，yuv420p，CRF 18，preset medium，faststart | ffprobe；`render.mjs` |
| 音频 | AAC LC 48 kHz 立体声 192 kbps | ffprobe；`render.mjs` |
| 文件 | 约 23.9 MB，总码率约 2.46 Mbps | fingerprints.json；ffprobe |
| 字幕 | 39 条 SRT + 画面烧录 | 文件；`render.mjs` `subtitles()` |
| 封面 | `out/封面.png`，1080×1920 RGBA，约 765 KB | `file` 读取 PNG 头 |

一句话管线：

> 母版 `em-waves`（横屏 187 s）→ 复制整套代码到新目录 → 压缩重写口播稿（36 段 → 19 段，800 字 → 351 字）+ 全局语速 +6% → `tts.py`（片长上限 80 s）重新合成并取逐词时间戳 → 按竖屏重写 3 个场景文件（9 个场景）、重写 `render.mjs` 版式（置顶标题、章节进度条、大字幕、抖音安全区、封面模式）→ Canvas 逐帧渲染经管道送 ffmpeg → 程序合成配乐（加快到 124 BPM）与音效混音 → MP4 → `--cover` 渲染封面 → 手写发布文案。

## 2. 与 `em-waves` 的关系（文件对比确认）

**结论：本片是 `em-waves` 的抖音竖屏改编版**，不是独立选题。依据：

1. `script.json:3` 的 `source` 字段与 `em-waves/script.json:3` **逐字相同**（同一 ELI5 标题、同四份资料）；`numbers` 字段是母版的子集。
2. 声线配置完全相同（姐姐 `zh-CN-XiaoxiaoNeural`；豆豆 `zh-CN-YunxiaNeural` +4% / +6 Hz）。
3. 代码文件逐一对比：

| 文件 | 与 `em-waves` 的关系 | 具体差异 |
| --- | --- | --- |
| `chars.mjs` | **完全相同**（sha256 前缀均为 `afba16a6a4cecb24`） | 角色（豆豆、光子、太阳、地球）原样复用 |
| `fonts.py` | **完全相同**（`a6ac7264e548bd3d`） | — |
| `lib.mjs` | 仅 1 行不同 | `W = 1920, H = 1080` → `W = 1080, H = 1920` |
| `tts.py` | 6 行不同 | docstring 注明 Douyin vertical cut；`TITLE`；`TARGET 200 → 80`（"keep it around a minute"）；`LEAD_IN 1.6 → 0.35`（"the hook must start almost immediately"）；`GAP 0.45 → 0.3`；`TAIL 3.0 → 2.2`（"room for the closing call-to-action"）；自动分句上限 14 → 12 字 |
| `audio.mjs` | 2 行不同 | 配乐 `bpm 112 → 124`；淡入 `1.2 s → 0.2 s` |
| `package.json` | 仅 `name` | `em-waves` → `em-waves-douyin` |
| `render.mjs` | 大幅改写（167 → 211 行） | 见 §5 |
| `props.mjs` | 追加约 45 行 | 把母版 `scenes-c.mjs` 里的 `BANDS`、`RAINBOW`、`rainbowFill`、`chirp`、`xrayHand` 提到公共道具库，`chirp` 改为可沿任意方向绘制（竖排频谱要用） |
| `scenes-*.mjs` | 重写：4 个文件 12 个场景 → 3 个文件（168/192/223 行）9 个场景 | 见 §5、§6 |
| `script.json` | 重写 | 19 段，**没有一段文字与母版相同**；新增 `"rate": 6` |

4. 时间先后（文件修改时间）：母版 TTS 2026-10-21 20:55、成片 21:10；竖屏版 TTS 22:56–22:57、成片 23:05、封面 23:07、发布文案 23:08。竖屏版在母版完成约 1 小时 45 分钟后开始。

## 3. 创作目标与需求

- 无 `prompt.md`、`plan/`；改编需求原文**未记录**。可从代码注释和交付物还原的目标：
  - 「Douyin: keep it around a minute」（`tts.py:32`）——一分钟左右；
  - 「the hook must start almost immediately on Douyin」（`tts.py:33`）——开口要快；
  - 「no fade-in: the very first frame is what viewers see while scrolling」（`render.mjs` renderFrame 注释）——首帧即封面级画面；
  - 「the cover is shown 3:4 centre-cropped on profile pages」（`render.mjs`）——封面要考虑主页 3:4 裁切；
  - 结尾引导评论（`cta` 段「你还知道哪些电磁波？评论区告诉我！」）。
- `out/发布文案.txt` 全文（项目自有材料，摘录）：

  ```text
  【标题】
  阳光、WiFi、X光，竟然是一家人？1分钟看懂电磁波

  【作品描述】
  晒太阳、连WiFi、拍X光片……它们其实都是电磁波，只是频率不同！
  电场和磁场一个带一个往外跑，不用空气，一秒能绕地球七圈半。
  你还知道哪些电磁波？评论区告诉我👇

  #电磁波 #物理 #科普 #涨知识 #光速 #电磁波谱 #ELI5

  【封面】
  out/封面.png（内容已放在画面中部，3:4 裁切后仍完整）

  【资料来源】
  OpenStax《Plane Electromagnetic Waves》、NASA《Anatomy of an Electromagnetic Wave》《The Electromagnetic Spectrum》、NIST SI 定义常数（c = 299 792 458 m/s）
  ```

## 4. 技术与依赖

| 类别 | 内容 | 证据等级 |
| --- | --- | --- |
| 渲染 | Node.js ESM + `@napi-rs/canvas` **1.0.13**（darwin-arm64 同版本） | `package-lock.json`；`render.mjs` import |
| 编码 / 混音 | ffmpeg（libx264、aac、sidechaincompress、alimiter、loudnorm） | 代码中调用；版本未记录 |
| TTS | Python `edge-tts` + `aiohttp`，端点覆盖 `api.msedgeservices.com` | `tts.py`；版本未记录 |
| 声线 / 语速 | 姐姐 Xiaoxiao **+6%**；豆豆 Yunxia **+10%**（+4 自带 +6 全局）/ +6 Hz | `script.json:9-13`；缓存只有 rate 6 与 rate 10 两种（16 + 3 段） |
| 字体 | 圆体-简 粗体、苹方-简 中粗（`build/fonts/`，22:59 抽取），回退 Arial Rounded / Arial Unicode / Hiragino | `lib.mjs`；文件 |
| 硬件 | 未记录；macOS（系统字体路径） | 推断 |

## 5. 横屏 → 竖屏：改了什么

### 5.1 内容与节奏

| 维度 | 横屏母版 `em-waves` | 竖屏版 | 改编意图（代码注释或推断） |
| --- | --- | --- | --- |
| 时长 | 186.9 s | 77.9 s（−58%） | `TARGET = 80`「keep it around a minute」 |
| 口播 | 36 段、约 800 字 | 19 段、约 351 字；**每段都重写** | 压缩为短句、口语化 |
| 语速 | +0% | 全局 +6%（豆豆 +10%） | 一分钟内说完；推断 |
| 开口 | `LEAD_IN 1.6 s`，有淡入 | `LEAD_IN 0.35 s`，**无淡入**，首帧即三个「?」占位卡 | 「the hook must start almost immediately」「the very first frame is what viewers see while scrolling」 |
| 钩子 | 豆豆：「姐姐，阳光、收音机、还有WiFi，它们有啥共同点？」 | 豆豆：「阳光、WiFi、医院的X光，居然是一家人？」+ 立即回答「没错！它们都是电磁波，只是频率不同。」 | 用反差更大的 X 光替换收音机；第 4 秒就给出答案（推断） |
| 段间停顿 | 默认 0.45 s，章节末 0.8–1.0 s | 默认 0.3 s，章节末 0.5 s | 更紧凑 |
| 场景交叉淡化 | `XF = 0.35`，场景提前 0.4 s | `XF = 0.3`「snappier than the landscape cut」，提前 0.3 s | — |
| 删去 | 「场」的定义（气球、磁铁指南针）、太空中声音传不出去、人浪类比、豆豆「频率翻倍会更快吗」、550 nm 绿光、收音机调幅全过程 | — | 只保留主线：是什么 → 怎么产生 → 跑多快 → 频率与波长 → 家族 → 能量 |
| 合并 | `freq`/`lambda`/`formula`/`demo`/`kidFast`/`noFast` 两个场景 | 一个 `bench` 场景 + 新句「所以频率翻倍，速度不变，波长减半！」 | 用一句话吸收母版中豆豆的提问 |
| 结尾 | 「原来，我每天都泡在电磁波里呀！」+ 彩纸 | 「所以此刻，你就泡在电磁波里！」+ CTA「你还知道哪些电磁波？评论区告诉我！」，气泡指向右侧评论按钮 | 引导互动；`TAIL 2.2`「room for the closing call-to-action」 |
| 豆豆台词 | 5 句 | 3 句（hook、kidAir、kidEnergy） | 保留最关键的两个误解提问 |
| 配乐 | 112 BPM，淡入 1.2 s | 124 BPM，淡入 0.2 s | 节奏更快、首帧就有声音 |
| 混音 | 音乐 0.75、音效 0.5 | 音乐 0.8、音效 0.55 | 手机外放更响亮（推断） |

### 5.2 版式与平台适配（`render.mjs`）

- **画布对调**：只改 `lib.mjs:7` 的 `W/H`，所有场景按 1080 宽重排；场景缩放中心从画面正中 `(W/2, H/2)` 改到 `(W/2, 880)`——舞台带的中心。
- **分区**（`scenes-a.mjs:2` 注释 + `render.mjs:21-24`）：
  - 顶部：置顶钩子标题「阳光 · WiFi · X光 / 竟然是一家人？」（64 px + 104 px 金字），开场从 1.15 倍落定，讲到「没错」时弹一下（`render.mjs:54-60`）；
  - y≈426–476：**章节进度条** 5 格「1 是什么 / 2 跑多快 / 3 频率·波长 / 4 家族 / 5 能量」，当前章金色放大、已完成半透明金（`render.mjs:22-23, 61-71`）；
  - y≈520–1260：舞台带，`x ≈ 60–920`；
  - `SUB_Y = 1330`：字幕带；
  - 1500 以下留给抖音账号名与描述。
- **抖音 UI 安全区**：`--still --safe` 时叠加半透明红色区域——顶部 150 px（状态栏）、右侧 x>930 且 y 760–1520（点赞/评论/分享按钮列）、底部 y>1500（账号与文案）（`render.mjs:108-114`）。用于抽帧检查画面是否被遮挡；是否实际执行过检查未记录。
- **字幕**（`render.mjs:75-101`）：字号 `min(66, (860 − 标签宽) / 每像素字宽)` 自动缩小，保证整行不超过 860 px、避开右侧按钮；胶囊高 100 px；文字加 6 px 深色描边；每条出现时有 1.05→1 的「踢入」缩放。母版是固定 52 px。SRT 自动分句上限也从 14 字降到 12 字。
- **结尾只淡出 0.5 s**，开头不淡入（母版首尾 0.4/0.6 s 淡入淡出）。
- **封面模式** `node render.mjs --cover`（`render.mjs:181-184`）：渲染 `at('yes','电磁波') + 0.7` 时刻（复算约 6.86 s，「电磁波」大字刚弹出、三件「家人」已就位）的一帧；整体下移 230 px（「the cover is shown 3:4 centre-cropped on profile pages, so its content is pulled toward the middle」），不画章节条和字幕，改画金色胶囊「1 分钟看懂电磁波」（`coverStrap`，y≈1390–1510）。`发布文案.txt` 记录「内容已放在画面中部，3:4 裁切后仍完整」——这是作者陈述，本次未打开图片做视觉核对。

### 5.3 画面重绘（同一物理，竖向构图）

- **电磁波**：保留母版的「波前 + E 竖直实线 + B 斜投影虚线」算法，参数按窄画面缩小：`AX=150, AY=880, X1=910, LAM=300, V=280, AE=150, AB=130`（母版 `LAM=380, V=300, AE=170, AB=150`），波前渐入区 120→100 px，高亮倍数 1.5→1.6（`scenes-a.mjs:60-…`）。
- **频谱**：母版是横向啁啾 + 横排七色块；竖屏改为**纵向**啁啾（`chirp(ctx, t, 120, 540, 120, 1230, …)`，从上方低频到下方高频）+ 七个色块竖排（`ROW(i) = 570 + 100i`），右侧对应用途图标（`scenes-c.mjs:13-43`）。为此 `chirp` 被重写成沿任意方向的版本：沿 (ux, uy) 前进 d，法向偏移 `(−uy·off, ux·off)`（`props.mjs:202-…`）。
- **钩子**：首帧就有三个「?」虚位卡，太阳 / 路由器 / X 光手随词替换掉问号，再框成「全家福」，接着横向啁啾演示「只是频率不同」（`scenes-a.mjs:13-57`）。注释：「the opening frame already shows three mystery slots, so the scroll-by thumbnail is never empty」。
- **频率 / 波长标尺** `bench`（`scenes-b.mjs:92-…`）：3 米标尺每米 280 px（母版 400 px），见 §7.2 的相位修正。

## 6. 镜头与时间轴

时间来源：`build/timeline.json` 已清理；下表按 `tts.py` 公式（`LEAD_IN=0.35`、逐段 `gap`、末词结束 +0.12 s、`TAIL=2.2`）与 `build/tts_cache`（姐姐 +6%、豆豆 +10%）**离线复算**，总长 77.87 s 与成片一致。场景提前 0.3 s 进入，交叉淡化 0.3 s。

| # | 场景 id | 约时间 (s) | 章节条 | 口播段 | 画面 | 实现入口 | 对应母版 |
| --- | --- | --- | --- | --- | --- | --- | --- |
| 1 | `hook` | 0.0–8.4 | 1 是什么 | hook(豆豆), yes | 三个「?」虚位 → 太阳 / 路由器 / X 光手 → 「全家福」→ 170 px「电磁波」+ 星爆 → 横向啁啾「频率低 / 频率高」 | `scenes-a.mjs:12-57` | intro（重写） |
| 2 | `wave` | 8.4–20.1 | 1 | make, perp | 天线电荷振荡，E 实线 / B 虚线随波前向右，高亮上下 / 左右、同起同落、垂直 | `scenes-a.mjs:59-…` | wave（参数缩小） |
| 3 | `vacuum` | 20.1–26.2 | 2 跑多快 | kidAir(豆豆), vacuum | 豆豆气泡「要靠空气？」→ 上方太阳、下方地球，阳光穿过真空 | `scenes-b.mjs:17-50` | vacuum（去掉太空喊话） |
| 4 | `speed` | 26.2–30.9 | 2 | speed | 光子自下而上冲刺，绕地球（R=205）计圈到 7.5 | `scenes-b.mjs:52-90` | speed（方向改为竖向） |
| 5 | `bench` | 30.9–42.9 | 3 频率·波长 | freq, lambda, formula, double | 3 米标尺上的波；粉点原地振动并计数「振了 n 次」；λ 括号；频率 / 波长读数卡；c = λ × f；300→600 MHz、波长减半 | `scenes-b.mjs:92-…` | stadium + formula 合并（去掉人浪） |
| 6 | `spectrum` | 42.9–57.0 | 4 家族 | lineup, members, uses | 左侧纵向啁啾（上低频下高频），七色块竖排逐词弹出，右侧用途图标跳动，「全在干活！」 | `scenes-c.mjs:13-43` | spectrum（横排→竖排） |
| 7 | `visible` | 57.0–61.3 | 4 | visible | 七色块横排，非可见光变暗、可见光块放大 | `scenes-c.mjs:45-81` | visible（去掉 550 nm） |
| 8 | `energy` | 61.3–69.2 | 5 能量 | kidEnergy(豆豆), photon | 豆豆「能量更大？」→ 单光子能量 vs 一束波（强度、时间） | `scenes-c.mjs:83-138` | energy（压缩） |
| 9 | `outro` | 69.2–77.9 | 5 | outro, cta | 七色交叉波浪「浸泡」豆豆，太阳 / 收音机 / 路由器 / X 光手环绕；CTA 气泡「你还知道…评论区告诉我」尾巴指向右侧评论按钮；彩纸 | `scenes-c.mjs:140-…` | outro（加 CTA） |

母版的 `field`（场）、`stadium`（人浪）、`radio`（收音机）三个场景在竖屏版中没有对应。

## 7. 关键算法与实现

> 区分：视频**讲解的知识**是电磁波基础物理；本节是**制作所用的算法**——Canvas 2D 绘图、示意性公式动画和 TTS 时间轴，不是电磁场仿真。

### 7.1 管线：「代码逐帧渲染 + TTS 驱动时间轴」（与母版相同，独立说明）

```text
script.json(rate 6) ─tts.py─▶ build/tts_cache/<sha1(voice|rate|pitch|text)[:16]>.mp3/.json  （逐句缓存 + WordBoundary 逐词时间）
                     ├────▶ build/narration.wav（adelay 定位 → amix → highpass 80 → loudnorm −16 LUFS）
                     ├────▶ build/timeline.json（段 start/end/words/cues，总时长）
                     └────▶ out/电磁波-抖音竖屏版.srt
lib.mjs: S(段) / E(段) / at(段, 词) —— 所有动画、音效、章节条切换都以「词」为锚点
render.mjs: 帧 i → renderFrame(i/30) → RGBA → ffmpeg stdin（drain 背压）；audio.mjs 合成 music.wav / sfx.wav
ffmpeg: 旁白 + 音乐(0.8，旁白侧链压缩) + 音效(0.55) → alimiter 0.93 → H.264 CRF18 / AAC 192k
另：--still [--safe] t… 抽帧（可叠加安全区）；--cover 出封面
```

- 段落时刻 `t_i = LEAD_IN + Σ(末词结束 + 0.12 + gap)`；以末词结束而非 mp3 长度计，规避 mp3 尾部静音。
- 片长上限 `TARGET = 80`：超出时按比例一步提高语速。本片在 `script.json` 中**预设** `"rate": 6`，首轮总长 77.87 s 即满足上限；缓存中只有 +6% / +10% 这一批（22:56:38–22:57:04），没有 +0% 的记录——+6% 是怎么定下来的（手算、先前试跑后清理缓存、或沿用经验）**无法确认**。
- 改编时代码几乎不用动：只要重写 `script.json` 和场景文件，时间轴、字幕、口型、音效会全部按新口播自动对齐——这正是这条管线适合「一稿多版」的原因。

### 7.2 频率渐变时的相位连续（`scenes-b.mjs:95-102`，竖屏版新增）

```js
// frequency ramps 300 → 600 MHz; the phase is the integral of ω so the wave never jumps
const r0 = tD - 0.1, r1 = tHalf - 0.05, k = prog(t, r0, r1);
const kInt = t < r0 ? 0 : t < r1 ? (t - r0) ** 2 / (2 * (r1 - r0)) : (r1 - r0) / 2 + (t - r1);
const f = 300 * (1 + k), lam = (299.792458 / f) * PXM, phi = 7 * (t + kInt);
```

- 母版 `formulaScene` 的时间相位固定为 `t * 5`：屏幕上的波长随频率缩短，但每个点上下振动的快慢不变——画面上「频率翻倍」只体现为波长变短。
- 竖屏版令角频率 `ω(t) = 7·(1 + k(t))`，相位取其积分 `φ = 7·(t + ∫k dt)`，`k` 在 [r0, r1] 线性从 0 到 1，所以 `∫k = (t−r0)²/(2(r1−r0))`，之后每秒加 1。这样频率翻倍时**振动也真的变快一倍**，且相位连续、波形不跳变。
- 「振了 n 次」计数 `n = floor((φ − 7·t_F) / 2π)` 直接由相位得到，与画面严格一致。
- 波长读数仍由真实光速 `299.792458 / f` 计算（300 MHz → 1.00 米，600 MHz → 0.50 米），与 `numbers` 底稿「λ ≈ 1.0 m；600 MHz → 0.50 m」一致。

### 7.3 任意方向的啁啾波（`props.mjs:202-…`）

相位 `ph(u) = (2πL / (l0·k))·(e^{k·u} − 1)`（瞬时波长 `l0·e^{−k·u}`，k = 3.4），沿单位向量 (ux, uy) 前进距离 d，在法向 `(−uy, ux)` 上偏移 `amp·sin(ph − 4t)`。同一函数既画钩子里的横向「频率低→高」，也画频谱场景里的纵向波谱。

### 7.4 字幕自适应字号（`render.mjs:85-88`）

`size = min(66, (860 − tagWidth) / (measure(text, 100px) / 100))`：先按 100 px 测宽求出每像素字宽，再反推能塞进 860 px 的最大字号，上限 66 px。12 字以内的句子基本都能用 60 px 以上的字号显示。

## 8. 声音与字幕

| 项 | 内容 | 来源 |
| --- | --- | --- |
| 旁白「姐姐」 | `zh-CN-XiaoxiaoNeural`，+6%，16 段 | `script.json`；缓存 |
| 提问「豆豆」 | `zh-CN-YunxiaNeural`，+10% / +6 Hz，3 段 | `script.json`；缓存 |
| 旁白处理 | 48 kHz、`highpass=f=80`、`loudnorm=I=-16:TP=-1.5:LRA=11` | `tts.py`（与母版相同） |
| 配乐 | 程序合成：124 BPM、C–G–Am–F 拨弦琶音 + 根音 + 沙锤 + 隔小节钟琴；淡入 0.2 s、淡出 2.5 s；种子 42 | `audio.mjs` |
| 音效 | 18 种程序合成音效（含母版新增的 `zap`/`wobble`/`static`），由各场景 `events` 按词触发；如讲频谱时 `wobble` 200 Hz→700 Hz、七个色块配七声音阶 | `audio.mjs`；`scenes-c.mjs:201-…` |
| 总混音 | 音乐 0.8 经旁白侧链压缩（0.05 / 5:1 / 20 ms / 400 ms），音效 0.55，`alimiter 0.93` | `render.mjs:193-196` |
| SRT | 39 条；首条 `00:00:00,395 --> 00:00:02,884 阳光、WiFi、医院的X光`（= LEAD_IN 0.35 + 首词 0.045），末条 `00:01:14,430 --> 00:01:15,550 评论区告诉我！` | `out/电磁波-抖音竖屏版.srt` |
| 烧录字幕 | y=1330，自适应字号 ≤66 px，6 px 描边，豆豆台词粉色「豆豆」标签 | `render.mjs:75-101` |

检查范围：仅 ffprobe 确认 48 kHz 立体声 AAC；本轮未试听、未测响度。

## 9. 迭代、问题与修正

| 事项 | 证据 | 说明 |
| --- | --- | --- |
| 横屏 → 竖屏改编 | 文件对比（§2） | 复制母版目录后：公共库只改尺寸、节奏常量，场景与版式重写 |
| 频率翻倍时振动快慢不变的问题 | `scenes-b.mjs:96` 注释与母版 `formulaScene` 对比 | 竖屏版改为相位积分，振动真实加倍且不跳帧；母版未回改（母版文件仍是 `t * 5`） |
| 首帧空白、被刷走 | `scenes-a.mjs` hook 注释、`render.mjs:160` 注释 | 取消淡入；首帧放三个「?」虚位卡与置顶标题 |
| 画面被抖音 UI 遮挡 | `render.mjs:21-24, 108-114` | 定义舞台带与字幕带；`--safe` 叠加安全区抽帧检查 |
| 主页封面 3:4 裁切 | `render.mjs:138`；`发布文案.txt` | 封面整体下移 230 px，并换成「1 分钟看懂电磁波」胶囊 |
| 频谱相关绘图函数需要两处复用 | `props.mjs:187-231` | 从场景文件提到 `props.mjs`，`chirp` 支持任意方向 |
| Edge TTS 端点 403 | `tts.py:19-25` | 沿用母版补丁 |

文件时间（非日志）：TTS 22:56:38–22:57:04 → SRT 22:57:08 → 字体抽取 22:59 → 成片 23:05:22 → 封面 23:07:37 → 发布文案 23:08:06。**推断**从合成语音到交付包约 12 分钟（含渲染编码约 6 分钟以内）；画面预览次数、`--safe` 是否执行、用户反馈均**未记录**。

## 10. 可复用制作经验

1. **一稿多版：复制母版目录，只换 `script.json` 与场景文件**。公共库（TTS、字体、角色、音频）几乎不改，`lib.mjs` 只改 1 行尺寸，就能得到新画幅版本（依据：§2 文件对比）。
2. **短视频改编先改常量再改内容**：`TARGET`、`LEAD_IN`、`GAP`、`TAIL`、`XF`、BPM、配乐淡入一起收紧，节奏立刻变快（依据：`tts.py:31-35`、`audio.mjs`、`render.mjs:20`）。
3. **首帧即封面**：竖屏信息流里不淡入，开场就放置顶标题和三个悬念「?」，首句在 0.35 s 开口（依据：`render.mjs:160`、`scenes-a.mjs` hook 注释）。
4. **把平台 UI 画成可开关的安全区覆盖层**：顶部 150 px、右侧按钮列、底部 420 px；舞台带与字幕带按此划分，抽帧时 `--safe` 一眼看出遮挡（依据：`render.mjs:108-114`）。
5. **字幕字号按可用宽度反推**：`min(66, 860 / 每像素字宽)`，避免长句压到右侧按钮（依据：`render.mjs:85-88`）。
6. **封面用同一渲染器出图并考虑二次裁切**：选「关键词刚出现」的时刻，整体向中心偏移，替换为一句卖点胶囊（依据：`render.mjs:138, 181-184`）。
7. **改变频率的动画要对 ω 积分求相位**，否则要么振动不变快、要么波形跳变（依据：`scenes-b.mjs:95-102`）。
8. **发布文案与成片同目录交付**：标题、描述、话题、封面说明、资料来源一并落盘，便于复核与复用（依据：`out/发布文案.txt`）。

## 11. 复现入口

命令来自 `render.mjs:1-5`、`tts.py:4`，**本轮均未执行**。

```bash
cd em-waves-douyin
npm install                                 # @napi-rs/canvas 1.0.13
python3 -m venv .venv && .venv/bin/pip install edge-tts aiohttp fonttools   # 版本未记录
.venv/bin/python tts.py                     # → build/narration.wav, build/timeline.json, out/电磁波-抖音竖屏版.srt
node render.mjs --still --safe 3.2 40       # 可选：带抖音安全区的抽帧
node render.mjs                             # → out/电磁波-抖音竖屏版.mp4
node render.mjs --cover                     # → out/封面.png
```

- 先跑 `tts.py`（`lib.mjs` 加载时读取 `build/timeline.json`，当前已清理）。`build/tts_cache/` 保留 19 段全部 mp3 + 词时间戳，`rate=6` 首轮即命中，**推断**可离线重建。
- 系统依赖：ffmpeg（libx264）、macOS 系统字体（`build/fonts/` 已有）。
- 修改入口：口播 → `script.json`；时长与留白 → `tts.py:32-35`；版式分区、章节名 → `render.mjs:21-24`；安全区 → `render.mjs:108-114`；封面 → `render.mjs:103-106, 138, 181-184`；配乐速度 → `audio.mjs` `bpm`。
- 发布文案为手写文本，不由脚本生成。

## 12. 参考来源

| 来源 | 类型 | 用途 | 访问状态 |
| --- | --- | --- | --- |
| 母版 `em-waves`（同仓库） | 前作 / 改编来源 | 选题、资料、角色、算法、音频引擎全部继承；本片为其竖屏短视频版 | 本地文件已对比 |
| OpenStax「Plane Electromagnetic Waves」 | 技术资料 | 平面电磁波性质 | `script.json:3`、`发布文案.txt` 仅列名称，无 URL，未访问 |
| NASA「Anatomy of an Electromagnetic Wave」 | 技术资料 | 电磁波结构、无需介质 | 同上 |
| NASA「The Electromagnetic Spectrum」 | 技术资料 | 电磁波谱 | 同上 |
| NIST SI 定义常数（c = 299 792 458 m/s） | 技术资料 | 光速 | 同上 |

抖音平台的安全区尺寸（顶部 150 px、右侧按钮列、底部 420 px）与「主页 3:4 裁切」只出现在代码注释中，未注明来源，**本次未核实**是否符合抖音当前官方规范。未发现外部参考视频或 X 帖子。

## 13. 证据与限制

| 文件 | 行数 | sha256[:16] | 读取程度 |
| --- | --- | --- | --- |
| `em-waves-douyin/script.json` | 41 | `0ca2351d545f37c6` | 全文，并与母版逐段对比 |
| `em-waves-douyin/render.mjs` | 211 | `c7b47a118a3b2f48` | 与母版全文 diff |
| `em-waves-douyin/tts.py` | 207 | `cf7b01f5bea5d46a` | 与母版全文 diff |
| `em-waves-douyin/audio.mjs` | 217 | `ef790f152784aedc` | 与母版全文 diff（2 行） |
| `em-waves-douyin/lib.mjs` | 227 | `fd7e2c87e695d0de` | 与母版全文 diff（1 行） |
| `em-waves-douyin/chars.mjs` / `fonts.py` | 180 / 36 | `afba16a6a4cecb24` / `a6ac7264e548bd3d` | 与母版哈希相同 |
| `em-waves-douyin/props.mjs` | 231 | `7f19378e3d1afc39` | 与母版 diff（新增部分全文） |
| `em-waves-douyin/scenes-a.mjs` | 168 | `c570d59f585c9a8d` | hook 全文、wave 开头、导出表 |
| `em-waves-douyin/scenes-b.mjs` | 192 | `519dc4c2ac30b7e7` | vacuum / speed 开头、bench 大部分、导出表 |
| `em-waves-douyin/scenes-c.mjs` | 223 | `ee94a9f160c14946` | spectrum 全文、visible / energy 开头、outro 大部分、events |
| `em-waves-douyin/package.json` / `package-lock.json` | — | `410eee36992b8b09` / `694a5af27419ebd9` | 全文 / 包版本 |
| `em-waves-douyin/out/电磁波-抖音竖屏版.srt` | 39 条 | `c49bc7e75ea37914` | 首尾条目、条数 |
| `em-waves-douyin/out/发布文案.txt` | — | `e0d746eb42427b3a` | 全文 |
| `em-waves-douyin/out/电磁波-抖音竖屏版.mp4` | — | 23,939,121 字节 | 仅 ffprobe 媒体头 |
| `em-waves-douyin/out/封面.png` | — | 765,153 字节（未列入 fingerprints.json） | 仅 PNG 头（尺寸），未查看画面 |
| `em-waves-douyin/build/tts_cache/*.json` | 19 个 | 见 fingerprints.json | 以哈希键匹配 script 并复算时间轴 |

缺失与限制：

- 无 `prompt.md`、`plan/`、制作对话：改编需求原文、为何选 +6% 语速、预览与修改过程**未记录**。
- `timeline.json`、`narration.wav`、`music.wav`、`sfx.wav`、预览帧（含 `--safe` 抽帧）已清理或从未保存；§6 时间为复算值。
- 依赖版本（Python 包、Node、ffmpeg）与硬件未记录。
- 封面未做视觉检查；「3:4 裁切后仍完整」是发布文案中的作者陈述。
- 成片仅 ffprobe 技术检查；**未做视觉检查、未试听、未测响度；是否发布与用户验收未记录**。
