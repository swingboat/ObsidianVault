---
type: video-production-dossier
project: video-creation-claude-opus
video_id: word-imperative
captured: '2026-11-12'
status: 已有 68.3 秒带配音/字幕/音乐/音效的完整 MP4；用户验收未记录
source_fingerprint: 1673fbfda19d75b9691e1c3c59bd4d399765ba1a2e6968ee8241acb8ec94d192
tags:
- video-creation
- production-capture
- canvas-frame-render
- tts
- explainer
---

# imperative · 藏着皇帝的单词 · Canvas 逐帧卡通讲解 + 多声线 TTS 驱动时间轴

<!-- capture-source-sha256:1673fbfda19d75b9691e1c3c59bd4d399765ba1a2e6968ee8241acb8ec94d192 -->

本次为源项目的材料回溯与归档。**未找回制作对话，结论来自项目文件**；本轮没有重新运行 TTS、渲染或混音，也没有进行视觉/用户验收。项目 git 只有两次整体提交，无法还原迭代过程；`build/` 中的帧、`timeline.json`、`narration.wav`、`music.wav`、`sfx.wav` 已被清理，仅剩 `build/fonts` 与 `build/tts_cache`。

## 1. 当前结论与制作路线

**状态：已有完整成片** `out/imperative-藏着皇帝的单词.mp4`（技术检查：ffprobe 媒体头可读；视觉检查、用户验收：未记录）。

| 规格 | 值 | 来源 |
| --- | --- | --- |
| 分辨率 / 画幅 | 1920×1080，16:9 横屏 | ffprobe；`lib.mjs` L7 `W = 1920, H = 1080` |
| 帧率 / 帧数 | 30 fps，2050 帧 | ffprobe；`lib.mjs` L7 `FPS = 30` |
| 时长 | 68.333 s | ffprobe；与本次由缓存重算的时间轴 68.333 s 一致（见 §6） |
| 视频编码 | H.264 High，yuv420p，CRF 18，preset medium | ffprobe；`render.mjs` L153 |
| 音频 | AAC LC，48 kHz，双声道，192 kbps | ffprobe；`render.mjs` L154 |
| 字幕 | 外挂 `out/imperative-藏着皇帝的单词.srt`（32 条），画面内另有烧录字幕 | 文件；`render.mjs` L59–81 |
| 文件大小 | 16,788,454 字节，总码率约 1.97 Mbps | ffprobe / 文件清单 |

一句话管线：

> 旁白脚本 `script.json`（分句、声线、停顿、字幕切分）→ `tts.py` 用 Edge 神经 TTS 逐句合成并取词级时间戳 → 生成 `build/timeline.json` + `narration.wav` + SRT → `render.mjs` 读取时间轴，用 `@napi-rs/canvas` 按 30 fps 逐帧画 10 个卡通场景（所有动画卡点都取自 TTS 词边界）→ `audio.mjs` 程序合成音乐床与音效 → ffmpeg 侧链压缩混音 + libx264 编码 → MP4。

没有分镜文档、素材图片或 AI 视频模型：画面全部是 Canvas 2D 矢量绘制（文字、卡片、皇帝角色、门、卷轴、彩带），声音除旁白外全部由代码合成。

## 2. 创作目标与需求

- **原始需求原文：未记录。** 该目录没有 `prompt.md`、`plan/` 或需求说明；项目规则要求的 `prompt.md` 缺失。
- 以下目标由脚本和代码**反推（推断）**：
  - 题材：英语「每日一词」短讲解（画面左上角徽章 `每日一词 imperative`，`render.mjs` L47–56）。
  - 记忆钩子：把 *imperative* 的开头 `imper` 与 *emperor*（皇帝）类比，讲同源拉丁词 *imperāre*「下令」，由此串起三个义项：语法「祈使（句）」、形容词「必须的」、名词「必须做的事」。
  - 易错点：开头 `im-` 在这里不是否定前缀「不」。
  - 风格：紫色放射背景、高饱和卡通、弹跳/回弹缓动、Q 版皇帝角色、大量「pop / boing / 盖章」音效，面向初学者（推断为少儿或入门向）。
  - 时长：`tts.py` L32 `TARGET = 70.0  # upper bound; the piece is as long as the narration needs`，即以 70 秒为上限、按旁白自然长度定片长。

### 旁白脚本（摘录自 `script.json`，完整 17 段）

| id | 声线 | 文本 | 停顿 gap(s) | 备注 |
| --- | --- | --- | --- | --- |
| hook | zh | 有个英文单词，里面偷偷藏着一位皇帝！ | 0.6 | |
| say1 | en | Imperative. | 0.7 | 第一次读词 |
| stress | zh | 重音落在第二个音节，跟我再读一遍。 | 0.45 | |
| say2 | en | Imperative. | 1.2 | 第二次读词（与 say1 同一条缓存音频） |
| reveal | zh | 把开头这几个字母圈出来，像不像另一个词？ | 0.5 | |
| emperor | en | Emperor. | 0.5 | `zh: 皇帝` |
| family | zh | 对，皇帝！它们是一家人，都来自拉丁语，意思是：下令。 | 1.1 | |
| cmdLead | zh | 皇帝一开口： | 0.35 | |
| cmd | king | Close the door! | 1.3 | `zh: 关上门！` |
| cmdExplain | zh | 这种直接叫人去做的句子，就叫祈使句。 | 1.0 | |
| must | zh | 皇帝的命令，谁敢不做？所以它还表示：必须的，非做不可！ | 1.15 | 手工 `captions` 3 段 |
| exAdj | en | It is imperative to save your work. | 1.1 | `zh: 务必保存好你的工作。` |
| noun | zh | 当名词用，它就是一件必须做的事。 | 0.45 | |
| exNoun | en | Honesty is a moral imperative. | 1.2 | `zh: 诚实，是道义上必须做到的事。` |
| trap | zh | 小心！开头的 I M 在这里不是“不”。它可不是不重要，而是超级重要！ | 1.1 | 手工 `captions` 5 段 |
| recap | zh | 记住这位皇帝：语法里，是请你去做；生活里，是必须去做。 | 0.6 | 手工 `captions` 3 段 |
| outro | king | Imperative! | — | `zh: 必须做！`，最后一段无 gap |

脚本设计要点（本次整理）：

- 每段只承载一个信息点，便于每段绑定一个场景或一个动作卡点。
- 英文句子用独立声线，并给出 `zh` 字段，字幕上下两行「英文 + 中文释义」。
- 「I M」写成分开的两个字母，让中文声线逐字母读出（推断：避免被读成「im」音节）；`trap` 段里的“不”用全角引号，代码里也按 `'“不”'` 去找词边界（`scenes-c.mjs` L56）。
- 长句用 `captions` 手工切字幕，避免自动切分把语义拆坏。

## 3. 技术与依赖

| 类别 | 内容 | 证据等级 / 来源 |
| --- | --- | --- |
| 运行时 | Node.js（ESM `.mjs`，用到顶层 `await`），版本未记录 | 代码中存在 |
| 画布 | `@napi-rs/canvas` 声明 `^1.0.11`，锁定 `1.0.11`（含 `darwin-arm64` 等平台包） | `package.json`、`package-lock.json` |
| Python | 项目 `.venv`（已清理），Python 版本未记录 | `tts.py` L4、`fonts.py` 文档字符串 |
| TTS 库 | `edge_tts`（Microsoft Edge 在线神经语音）+ `aiohttp`；**版本未记录**（无 requirements/锁文件） | `tts.py` L15–17 |
| TTS 端点补丁 | 覆盖 `edge_tts.communicate.WSS_URL` 为 `wss://api.msedgeservices.com/tts/cognitiveservices/websocket/v1` | `tts.py` L19–25 |
| 字体工具 | `fontTools`（`TTCollection`），从 macOS 系统字体资产中抽取单个字重 | `fonts.py` |
| 编码 / 混音 | 系统 `ffmpeg`（libx264、AAC、`sidechaincompress`、`alimiter`、`loudnorm`），版本未记录 | `tts.py` L176–190、`render.mjs` L144–155 |
| 硬件 | 未记录（字体路径与 `.venv/bin/python` 表明是 macOS，推断） | — |

### 声线（`script.json` → `voices`）

| 角色 key | Edge 语音 | rate | pitch | 用途 |
| --- | --- | --- | --- | --- |
| zh | `zh-CN-XiaoyiNeural` | 0 | 0 | 中文讲解（女声） |
| en | `en-US-EmmaNeural` | −12% | 0 | 英语单词与例句，放慢 12% |
| king | `en-GB-RyanNeural` | −10% | −12 Hz | 「皇帝」发号施令，英音、放慢、降调 |

### 字体（`lib.mjs` L10–21、`fonts.py`）

- `fonts.py` 在 `/System/Library/AssetsV2/com_apple_MobileAsset_Font*/*.asset/AssetData/` 中找 `Yuanti.ttc`、`PingFang.ttc`，按 name 表 ID 4 取 `Yuanti SC Bold`、`PingFang SC Semibold`，另存为 `build/fonts/*.ttf`。原因写在文档字符串里：**Canvas 只注册 .ttc 的第一个字面**，所以要把粗体字面拆出来。
- `lib.mjs` 在字体缺失时自动调用 `.venv/bin/python fonts.py`（L11–13）。
- 字体栈：中文 `ZH = 'YuanB, RoundB, UniB, HiraGB'`（圆体粗）；英文 `EN = 'RoundB, YuanB, UniB, HiraGB'`（Arial Rounded Bold 优先）。
- `PingB`（苹方中粗）被注册但**不在任何字体栈中**，代码中存在、实际未使用（本次读码结论）。
- `build/fonts/` 现存两份抽取结果：`YuantiSC-Bold.ttf`（27.4 MB）、`PingFangSC-Semibold.ttf`（13.0 MB）。系统字体有版权，不应随作品分发（整理者提示）。

## 4. 镜头与时间轴

场景表在 `scenes-a/b/c.mjs` 末尾以 `{id, from, to, draw}` 声明，边界全部用旁白段落起点 `S(id)` 减去 0.3–0.55 s 计算（画面略早于声音进场）。下表时间由本次用 `script.json` + `build/tts_cache` 按 `tts.py` 算法**离线重算**（未运行项目脚本），总长 68.333 s 与 MP4 一致；与 SRT 时间码可逐条对上。

| # | 场景 id | 时间 (s) | 对应旁白 | 画面内容 | 实现入口 | 关键参数 / 卡点 |
| --- | --- | --- | --- | --- | --- | --- |
| 1 | word | 0 – 17.88 | hook, say1, stress, say2, reveal, emperor | 问号卡片里探出王冠 → 10 个彩色字母依次落下 → 拆成 `im·PER·a·tive`，PER 放大变金并插「重音！」旗 → 第二遍朗读时音节逐个脉冲 → 红笔圈出 `imper` → 下方弹出 `emperor` + 「皇帝」标签 + 招手的皇帝 | `scenes-a.mjs` L35 `wordScene`、L130 `drawEmperorWord`、L176 `say2Pulse` | 字母间隔 0.055 s；`GROUPS=[[0,2],[2,5],[5,6],[6,10]]`；重音组缩放 1.32；音节脉冲切点 `[0,0.18,0.52,0.66,1]` |
| 2 | family | 17.88 – 24.63 | family | 左侧皇帝；三张词卡 imperative/emperor/empire 弹出；顶部金卡「拉丁语 imperāre = 下令！」，箭头连向三卡；说到「下令」时皇帝指向、表情变严肃并画冲击线 | `scenes-a.mjs` L196 | `stamp` 音效 + 屏幕抖动 10 |
| 3 | command | 24.63 – 28.65 | cmdLead, cmd | 打开的门外刮风飘叶；皇帝喊 "Close the door!"，气泡里单词按词边界逐个弹出；门在 "door" 结束时砰地关上，BAM! 爆炸框 | `scenes-b.mjs` L87 `commandScene`、L33 `doorAngle`、L40 `door` | `T_SLAM = word('cmd','door').end`；关门曲线 0.3 s、指数 2.2；`slam` 音效 + 抖动 22 |
| 4 | grammar | 28.65 – 33.01 | cmdExplain | 句卡 `(You) Close the door.`，(You) 被划掉并标「“你”省略了」；Close 被红底高亮并标「一开口就是动作」；顶部「祈使句 / imperative sentence」 | `scenes-b.mjs` L124 | 高亮卡点「直接」「叫」「祈使」 |
| 5 | must | 33.01 – 39.52 | must | 严肃皇帝 + 展开的 MUST DO 卷轴，三条待办依次打勾；说「必须」时红章从 2.6 倍砸下；底部横幅 `imperative = 必须的 · 非做不可`；「谁敢不做」时冒汗滴 | `scenes-b.mjs` L171 | 打勾间隔 0.2 s；盖章 0.18 s；`stamp` + 抖动 16 |
| 6 | adj | 39.52 – 43.26 | exAdj | 例句卡逐词弹出，`imperative` 金底高亮；标签「adj. 形容词 · 必须的」；下方保存进度条 0→100% 后打勾「已保存」 | `scenes-b.mjs` L229 `exampleCard`、L262 `adjScene` | 进度条从 "save" 起到句末 +0.4 s |
| 7 | noun | 43.26 – 50.00 | noun, exNoun | `an imperative` + 「n. 名词」标签 + 「= 一件必须做的事」；随后例句卡 `Honesty is a moral imperative.` 高亮后三词，跳动的心 + 「诚实 = 必须做到」 | `scenes-c.mjs` L21 | 例句卡复用 `exampleCard` |
| 8 | trap | 50.00 – 57.81 | trap | 摇晃警示牌；大号 `im` + 灰色 `perative` ≠ 圆圈「不」被红线划掉；「不重要」被打叉 → 箭头 → 「超级重要！」+ 环绕星星 | `scenes-c.mjs` L54 | `alarm`、两次 `buzz`、`tada` |
| 9 | recap | 57.81 – 64.14 | recap | 顶部 `imperative` 戴王冠；左面板「语法里 / 请你去做 / 祈使句」，右面板「生活里 / 必须去做」+ 卷轴盖章 | `scenes-c.mjs` L111 | 盖章于「生活里」+0.45 s |
| 10 | outro | 64.14 – 68.33 | outro | 字母随皇帝喊 "Imperative!" 逐个弹出并波浪跳动；「必须做！」印章；三义项胶囊「祈使的 / 必须的 / 必须做的事」；两侧彩带喷发；末 0.6 s 淡出 | `scenes-c.mjs` L154 | `confetti` 160 片；`tada` |

全局层（`render.mjs`）：放射旋转背景 + 28 个漂浮光点（L21–45）、左上「每日一词」徽章（L47）、底部烧录字幕（L59）、场景交叉淡化 `XF = 0.35 s` 且进场 0.94→1、出场 1→1.05 缩放（L18、L105–117）、事件驱动屏幕抖动（L87–98）、首 0.4 s 淡入与尾 0.6 s 淡出（L121–125）。

## 5. 关键算法与实现

> 区分：视频**讲解**的是英语词源与词义（imperative ← 拉丁 imperāre「下令」，与 emperor 同源；祈使句；形容词/名词义），这部分是内容知识，本次未做外部词源核验。下文是**制作视频所用**的算法。

### 5.1 TTS 词边界驱动的时间轴（核心机制）

`tts.py` 对每段调用 `edge_tts.Communicate(..., boundary="WordBoundary")`，同时收音频块和 `WordBoundary` 事件；事件的 `offset/duration` 单位是 100 ns（`TICKS = 1e7`）。每个词在原文中的字符位置用游标顺序 `text.find` 回填为 `idx`（L69–85）：

```python
idx = text.find(chunk["text"], cursor)   # 顺序匹配，找不到时退回游标
cursor = idx + len(chunk["text"])
words.append({"text": ..., "idx": idx, "start": offset/1e7, "end": start + duration/1e7})
```

排轨规则（L148–169）：

```text
speech_i = 最后一个词的 end + 0.12     # 不用 mp3 实长：mp3 尾部约 0.6 s 静音会吃掉停顿
start_0 = LEAD_IN(1.2)
start_{i+1} = start_i + speech_i + gap_i（script.json 每段 gap，缺省 0.45）
总长 = 最后一段结束 + TAIL(2.6)         # 给结尾彩带留时间
```

渲染端 `lib.mjs` L30–40 提供 `word(id, w, nth)` / `at(id, w)`：在段落文本里找子串位置，再映射到覆盖该位置的 TTS 词，返回其起止时间——注释写明 *taken from the TTS timestamps, never estimated*。所有场景动画与音效都写成 `at('family','下令')`、`word('cmd','door').end` 这类相对卡点，所以改台词或语速后重跑 TTS，画面会自动重新对齐，无需手调关键帧。

### 5.2 自动语速收敛

若总长超过 `TARGET`，按超出比例一次性跳到所需语速，而不是逐步试探（L155–158）：

```python
available = TARGET - LEAD_IN - sum(gaps) - TAIL
rate = min(30, max(rate + 2, ceil(rate + (sum(speech)/available - 1) * 100) + 1))
```

全局 `rate` 叠加到每个声线自身的 rate 上，上限 +30%。最终版本 rate = 0（证据见 §7）。

### 5.3 嘴型与表情

`talkLevel(t, ids)`（`lib.mjs` L278–285）：当前时刻落在某个词的起止区间内时，嘴张开度 `0.45 + 0.55·|sin(22t)|`，词间闭嘴——用词边界近似口型，不做音素分析。皇帝角色 `emperor()`（L195–263）参数化：`talk` 张嘴、`point` 右臂抬起指向、`wave` 左臂挥手、`mood` 切换 happy / stern / shock 的眉形和嘴形；每 3.3 s 眨眼一次，身体 `sin(4t)` 上下浮动。

### 5.4 缓动与弹出

`lib.mjs` L42–56：`easeIO`（三次）、`easeOut`、`easeBack`（回弹系数 c1 = 2.2，比常见 1.70158 更夸张）、`elastic`；统一封装 `pop(t, t0, d=0.45)` 作为「卡点弹出」原语，`around()` 以某点为中心同时施加缩放、透明度、旋转。

### 5.5 其他画面算法

- **音节重排** `layout(stressMix)`（`scenes-a.mjs` L16–27）：用离屏 canvas 预量每个字母宽度，按音节组插入 56 px 间隙，重音组放大 1.32、其余缩到 0.92，整体居中；`stressMix` 在 0↔1 间插值，实现「拆开—合拢」。
- **手绘圈** `scribbleEllipse`：1.15 圈椭圆，半径加入 `0.04·sin(23u) + 0.05u` 抖动，按进度截断路径。
- **关门** `doorAngle`：开 72° → 0.3 s 内按 `1 - p^2.2` 关上 → 关后 `0.09·sin(30Δt)·e^(−9Δt)` 衰减回弹；门板用 `cos θ` 压缩宽度、`sin θ·46` 做透视错位。
- **彩带** `confetti`：160 片左右对喷，水平速度带 `e^(−0.6d)` 阻尼，竖直方向 `900·d²` 重力，`|sin(9d+i)|` 纵向缩放模拟翻面；伪随机 `fract(sin(i·127.1+311.7)·43758.5453)`，帧间确定。
- **屏幕抖动**：事件带 `shake` 幅度时，在 0.4 s 内按 `(1-p)²` 衰减，x/y 用不同频率正弦（95、81 rad/s）。

### 5.6 程序音频（`audio.mjs`）

- 采样率 48 kHz，单声道生成、写 WAV 时复制为双声道，峰值超过 0.98 自动归一（L187–203）。
- 伪随机 `rng(seed)`（mulberry32 式），音乐种子 42、音效种子 7，结果可复现。
- 乐器：Karplus–Strong 拨弦（`pluck`，阻尼 0.9965）、正弦 + 3.93 倍泛音的马林巴、4 个非整数泛音的钟（1 / 2.76 / 5.4 / 8.93 倍）、可滑动中心频率的 RBJ 带通噪声（每 32 样本更新系数）。
- 音乐床 `buildMusic`：112 BPM，C–G–Am–F（I–V–vi–IV）循环，每小节 8 个八分音符拨弦琶音（模式 `[0,2,1,3,2,1,3,2]`）+ 第 1、3.5 拍低音 + 16 分音符高频沙锤 + 隔小节钟琴应答；首 1.2 s 淡入、尾 2.5 s 淡出。
- 音效 `FX`：pop、note、ding、sparkle、whoosh、boing、slam、stamp、tick、scribble、buzz、alarm、tada、confetti、wind 共 15 种，全部参数化合成；事件表与画面卡点写在同一个场景文件中（`eventsA/B/C`）。

## 6. 声音与字幕

### 配音

- 引擎：Microsoft Edge 在线神经 TTS（经 `edge_tts` 库），三条声线见 §3；逐段合成，按 `sha1("voice|rate|pitch|text")[:16]` 缓存到 `build/tts_cache/<key>.mp3/.json`（`tts.py` L44–66）。
- 429 限流时指数式退避重试（8、16、24、32 s，最多 5 次，L52–62）。
- 旁白总线：每段 `aresample=48000` + `adelay` 放到时间轴位置 → `amix(normalize=0)` → `highpass 80 Hz` → `loudnorm I=-16:TP=-1.5:LRA=11` → `apad` + `atrim` 到总长 → `build/narration.wav`（L176–190）。−16 LUFS 是对旁白总线的**目标参数**；成片响度本次未测量。

### 音乐、音效与混音（`render.mjs` L141–155）

```text
[旁白] ─asplit─┬────────────────────────────────┐
               └→ sidechain 键控                  │
[音乐 ×0.8] → sidechaincompress(threshold 0.05, ratio 5, attack 20 ms, release 400 ms)
[音效 ×0.5] ──────────────────────────────────────┤
amix(3 路, normalize=0, duration=first) → alimiter(limit 0.93) → AAC 192k
```

即音乐在人声出现时被压低（ducking），音效叠在上方，最后限幅防爆音。混音结果没有单独的检查报告；未记录是否试听。

### 字幕

- 切分：中文段默认按 `，。？！：；` 切块、每块 ≤14 字、遇句末标点强制断开，去掉块尾逗号句号（`split_caption`，L106–119）；`script.json` 里有 `captions` 的段落用手工切分。英文段整句一条，下方附 `zh` 释义。
- 对时：每块覆盖的字符区间 `[a, b)` 内第一个词的 start 到最后一个词的 end（`caption_cues`，L122–136）——**字幕时间来自 TTS 词边界，不是估算**。
- 输出两份：外挂 SRT（`out/…srt`，32 条，英文条为两行）；画面内烧录字幕（`render.mjs` L59–81）——同一句内各块无缝切换，只有整句首块淡入、末块淡出（各 ≈0.1–0.2 s），深紫圆角底 `rgba(16,6,48,0.78)`，英文金色、中文白色。
- 观察：SRT 第 25/26 条「开头的 I M 在这里」「不是“不”」首尾相接、第 26 条仅 0.5 s；第 24 条「小心！」0.5 s。短条在画面内由下一条起点接续显示，影响较小（本次读 SRT 所得，未看画面）。

## 7. 迭代、问题与修正

未找回制作对话，以下只来自代码注释、缓存与文件时间戳。

| 事项 | 证据 | 结论等级 |
| --- | --- | --- |
| Edge TTS 旧主机 `speech.platform.bing.com` 自 2026 年 10 月对合成请求返回 403，改连 `api.msedgeservices.com`；注释要求在 edge-tts 正式发版修复后删除该覆盖 | `tts.py` L19–25 注释 | 代码中存在 |
| 免费端点 429 限流 → 加入退避重试 | `tts.py` L56–62 | 代码中存在；是否实际触发未记录 |
| mp3 尾部约 0.6 s 静音会侵占下一段停顿 → 段长改用「最后一个词 end + 0.12 s」 | `tts.py` L151 注释 | 代码中存在 |
| Canvas 只注册 .ttc 第一个字面 → 写 `fonts.py` 抽取粗体字面 | `fonts.py` 文档字符串 | 代码中存在；`build/fonts` 有产物，说明执行过 |
| **TTS 跑过两轮语速**：缓存中 17 段文本各有两份，全局附加 rate 分别为 0 和 +12（en: −12/0，king: −10/+2，zh: 0/+12） | 本次用 `tts.py` 的 key 公式对 `script.json` 文本反算 34 个缓存键，全部命中 | 日志/缓存显示执行过 |
| **最终采用 rate = 0** | rate 0 的缓存重算总长 68.333 s，与 MP4 的 2050 帧 / 68.333 s 和 SRT 时间码一致；rate +12 版本总长 62.97 s，不符 | 最终采用（交叉印证） |
| 为何先跑过 +12：用当前 gap 推算，只有 `TARGET` 约 63 s 时自动收敛才会从 0 跳到 +12；推测早期目标更短，后来把 `TARGET` 放宽为「70 s 上限」并回到原速 | 计算推导 | **推断，无法确认** |
| 时间顺序：TTS 缓存 2026-10-13 20:40–20:42；`script.json` 与 SRT 21:34（同一时刻，说明改脚本后重跑了 `tts.py`，且文本未变全部命中缓存）；`scenes-*.mjs` 与 `lib.mjs` 10-14 09:46–09:53 修改，`render.mjs` 与 MP4 09:54 | 文件 mtime | 推断（mtime 只反映最后一次写入） |
| 与 `research-visual` 的关系：两者 `tts.py` 主体相同；本片增加了按段声线、pitch、按段 gap、手工 captions、英文双语字幕，并把渲染拆为 `lib.mjs` + 三个场景文件 + `audio.mjs` | 代码 diff；`research-visual` 文件时间更早（10-13 20:11–20:14） | 推断：本片复用并扩展了前一部的管线 |

未发现 `plan/`、失败候选、预览帧或用户反馈记录；`render.mjs --still` 预览功能存在，但 `build/still_*.png` 已不在（被清理或未生成，无法确认）。

## 8. 可复用制作经验

1. **让 TTS 词边界成为唯一时间源。** 所有画面与音效卡点写成 `at(段落, 关键词)`，改台词/语速只需重跑 TTS，画面自动重对齐；避免手写秒数。依据：`lib.mjs` L30–40、`scenes-*.mjs` 事件表。
2. **段落间距按「最后一个词的结束」计算，不按音频文件长度。** Edge TTS 的 mp3 尾部带约 0.6 s 静音，按文件长度排轨会让停顿忽长忽短。依据：`tts.py` L151–152。
3. **多角色用不同声线 + rate/pitch 区分**，并把声线参数纳入缓存键；同文本同参数的句子（say1/say2）自动复用一条音频。依据：`script.json` `voices`、`tts.py` L39–47。
4. **超时先算比例一次跳到目标语速**，再设上限（+30%）；同时允许片长「以旁白为准、只设上限」，比硬卡固定片长更自然。依据：`tts.py` L146–158。
5. **声音与画面在同一处声明。** 每个场景文件导出 `scenes` 和 `events`，事件既触发合成音效也可带 `shake`，保证「看到的砸章」和「听到的砸章」同帧。依据：`scenes-a.mjs` L258–278、`render.mjs` L87–98、L142。
6. **Node Canvas 中文粗体要先把 .ttc 拆成单字面 .ttf 再注册**，否则只拿到第一个字重。依据：`fonts.py`。
7. **侧链压缩做音乐避让**：`sidechaincompress` 以旁白为键，加 `alimiter` 收尾，比手画音量包络省事。依据：`render.mjs` L149–151。
8. **长中文句手工指定字幕切分**（`captions`），自动规则只做兜底。依据：`script.json` must / trap / recap。

## 9. 复现入口

记录命令不代表本轮执行过。`tts.py`、`fonts.py`、`render.mjs` 的调用方式来自各文件头部注释；`npm install` 与 venv 安装两行是整理者按 import 补的（项目没有 Python 依赖清单）。顺序与前置条件：

```bash
cd word-imperative
npm install                      # 恢复 @napi-rs/canvas 1.0.11（node_modules 已被 .gitignore 排除）
python3 -m venv .venv && .venv/bin/pip install edge-tts fontTools   # 版本未记录；aiohttp 随 edge-tts 安装
.venv/bin/python fonts.py        # 可省略：render 时字体缺失会自动调用
.venv/bin/python tts.py          # 生成 build/seg*.mp3、build/narration.wav、build/timeline.json、out/*.srt
node render.mjs --still 3.2 27.6 # 可选：预览单帧到 build/still_<t>.png
node render.mjs                  # 合成 build/music.wav、build/sfx.wav，逐帧渲染并编码 out/*.mp4
```

- 需要 macOS（系统字体路径写死）与 PATH 中的 `ffmpeg`。
- `build/tts_cache` 已保留：只要 `script.json` 文本与声线参数不变，`tts.py` 会全部命中缓存，不需要联网；改了任何一句才会请求 Edge TTS，且需要 `tts.py` 中的端点补丁（或已修复的 edge-tts 版本）。
- 若要复现 68.333 s 版本，`script.json` 不要加 `rate` 字段（默认 0）。
- 耗时、渲染性能：未记录。

## 10. 参考来源

| 链接 | 类型 | 引用位置 | 借鉴内容 | 访问状态 |
| --- | --- | --- | --- | --- |
| https://github.com/rany2/edge-tts/issues/490 | 技术资料（issue） | `tts.py` L20 注释 | 2026-10 起 edge-tts 合成握手返回 403 的问题报告 | 2026-11-12 访问成功：标题 "Edge-tts failed .WSServerHandshakeError: 403…"，状态 Closed，2026-10-10 提出（报告者环境为 edge-tts 7.2.8，非本项目版本） |
| https://github.com/rany2/edge-tts/pull/491 | 技术资料（PR，注释写作 #491） | `tts.py` L20 注释 | 改用 `api.msedgeservices.com` 合成；本项目以猴子补丁方式采用其第 1 项思路 | 2026-11-12 访问成功：标题 "Fix 403s: synthesize through api.msedgeservices.com; parse the server's Date in any locale"，状态 Closed（未合并） |

- 创作参考（视频、帖子、示意图）：**未记录**。
- 词源与例句的内容出处：未记录，本次未核验。

## 11. 证据与限制

实际读取的文件（项目相对路径，sha256 前 16 位取自 fingerprints.json）：

| 文件 | sha256 前 16 位 | 读取范围 |
| --- | --- | --- |
| `word-imperative/script.json` | `9cd21854e2fe60b5` | 全文 |
| `word-imperative/tts.py` | `344f75d58ad85244` | 全文 |
| `word-imperative/render.mjs` | `1e54bf12b845a921` | 全文 |
| `word-imperative/lib.mjs` | `84f7b11b9119b462` | 全文 |
| `word-imperative/audio.mjs` | `f9e04b2cfd54743c` | 全文 |
| `word-imperative/scenes-a.mjs` | `d4dd4491b70dd8c2` | 全文 |
| `word-imperative/scenes-b.mjs` | `84772ad89e95b3d6` | 全文 |
| `word-imperative/scenes-c.mjs` | `01681d6ba7448303` | 全文 |
| `word-imperative/fonts.py` | `a6ac7264e548bd3d` | 全文 |
| `word-imperative/package.json` / `package-lock.json` | `b5a84ef531af2830` / `682541fe33bcc701` | 全文 / 版本字段 |
| `word-imperative/out/imperative-藏着皇帝的单词.srt` | `2c645e239647df0b` | 全文 |
| `word-imperative/build/tts_cache/*.json`（32 个） | 见 fingerprints.json | 词数、文本、末词 end；用于反算 rate |
| `word-imperative/out/imperative-藏着皇帝的单词.mp4` | — | 仅 ffprobe 媒体头 |

未读取 / 缺失：

- 未打开 `build/tts_cache/*.mp3` 与 `build/fonts/*.ttf` 内容；未观看成片、未抽帧、未测响度。
- 缺失：原始需求 `prompt.md`、设计文档、`build/timeline.json`、`narration.wav`、`music.wav`、`sfx.wav`、预览帧、渲染日志与耗时、Python/edge-tts/Node/ffmpeg 版本、视觉检查与用户验收记录。
- 制作对话：未找回，本档案结论来自项目文件。
