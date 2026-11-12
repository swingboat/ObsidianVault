---
type: video-production-dossier
project: video-creation-claude-opus
video_id: ai-3d-game
captured: '2026-11-12'
status: 已有完整 MP4 成片（237 秒横屏讲解动画）；用户验收未记录
source_fingerprint: 4fb7283551e984d45be0bca681214efbce4cbf65281c23bfb476c56595ed1ae5
tags:
- video-creation
- production-capture
- canvas-frame-render
- tts
- explainer
---

# Kitty 带你看 · AI 做 3D 游戏 · Canvas 逐帧渲染 + Edge TTS 驱动时间轴的帖子解读动画

<!-- capture-source-sha256:4fb7283551e984d45be0bca681214efbce4cbf65281c23bfb476c56595ed1ae5 -->

本次为材料回溯与归档。**未找回制作对话，结论来自项目文件**（源码、`script.json`、`build/tts_cache`、`out/` 成片与字幕的媒体头）。本轮未运行任何项目脚本，未重新渲染、未重新合成语音。

## 1. 当前结论与制作路线

**状态：`out/Kitty带你看-AI做3D游戏.mp4` 是完整成片**（存在且 ffprobe 可读；视觉检查、用户验收均未记录）。

| 项 | 值 | 来源 |
| --- | --- | --- |
| 分辨率 / 画幅 | 1920×1080，16:9 横屏 | ffprobe；`lib.mjs:7` `W = 1920, H = 1080` |
| 帧率 | 30 fps，共 7110 帧 | ffprobe `r_frame_rate=30/1, nb_frames=7110`；`lib.mjs:7` |
| 时长 | 237.000 s | ffprobe；与本次用缓存复算的时间轴 237.011 s 一致（见 §6） |
| 视频编码 | H.264 High，yuv420p，CRF 18，preset medium，faststart | ffprobe；`render.mjs:158-169` |
| 音频 | AAC LC 48 kHz 立体声 192 kbps | ffprobe；`render.mjs:168` |
| 文件 | 约 34.3 MB，总码率约 1.16 Mbps | fingerprints.json；ffprobe |
| 字幕 | `out/Kitty带你看-AI做3D游戏.srt`，108 条，另外画面内也烧录了字幕 | 文件；`render.mjs:59-84` |

一句话管线：

> X 帖子（Sac 的 AI 3D 游戏教程）→ 手写口播稿 `script.json`（38 段、两种声线）→ `tts.py` 调 Edge TTS 合成并取逐词时间戳 → 生成 `build/timeline.json` + `narration.wav` + SRT → `scenes-a..e.mjs` 用「某段某个词的发音时刻」编排 14 个场景的动画与音效事件 → `render.mjs` 用 `@napi-rs/canvas` 逐帧画 RGBA → 经 stdin 管道送 ffmpeg → `audio.mjs` 程序合成配乐/音效并与人声侧链压缩混音 → H.264 + AAC MP4。

没有任何位图素材、3D 模型或 AI 生成画面：所有角色（Kitty 猫、小王子、AI 机器人）、道具（小行星、玫瑰、火山、路灯、水壶）、图标都是 Canvas 2D 路径代码画出来的；唯一的外部资源是 macOS 系统字体。

## 2. 创作目标与需求

- 项目内**没有 `prompt.md`、`plan/` 或制作记录**，原始需求原文未记录。
- 可确认的意图来自 `script.json`：
  - `script.json:2` 摘录：`"source": "https://x.com/Saccc_c/status/2110694725640474759 — Sac《手把手教你用 Opus 5.5 / Astra + Tripo 制作 3D 游戏》(2026-10-15)"`。
  - 形式：由叙述者「Kitty」（白色小猫、红色蝴蝶结、无嘴，见 `chars.mjs:13`）用第一人称「带你看一个超酷的帖子」，穿插「小王子」角色的 3 句台词。
  - 内容结构与原帖三部分一致：开发流程和准备 → 场景构建 → 人物建模；结尾总结「先画图，再变 3D，让 AI 动手搭，再派 AI 来挑刺」并预告作者会在 GitHub 开源。
- 风格（来自代码，非需求原文）：深蓝紫渐变夜空 + 闪烁视差星星（`render.mjs:24-44`），粉/金/天蓝等糖果色卡片，圆体中文字体，弹出（easeBack）动画，左上角「Kitty 带你看帖」角标（`render.mjs:46-56`）。整体是面向大众的儿童绘本式科普动画（推断）。
- 时长要求：`tts.py:32` `TARGET = 240.0  # upper bound; the piece is as long as the narration needs`——片长上限 4 分钟，实际由口播长度决定。
- 同一作者体系：标题 `Kitty带你看-…` 与 `em-waves`（`电磁波-看不见的大家族`）共享同一套代码骨架（见 §5），推断为同一系列的模板化生产。

## 3. 技术与依赖

| 类别 | 内容 | 证据等级 |
| --- | --- | --- |
| 渲染 | Node.js ESM + `@napi-rs/canvas` **1.0.11**（darwin-arm64 预编译包同版本） | `package-lock.json` 锁定；`render.mjs:4` 实际 import |
| 编码 / 混音 | ffmpeg（libx264、aac、`sidechaincompress`、`alimiter`、`loudnorm`、`amix`） | 代码中调用；ffmpeg 版本未记录 |
| TTS | Python + `edge-tts`（微软 Edge 在线神经语音）+ `aiohttp` | `tts.py:15-17`；版本未记录（无 requirements 文件，`.venv/` 已被 .gitignore 排除） |
| 字体抽取 | Python `fontTools.ttLib.TTCollection` | `fonts.py:9`；版本未记录 |
| 字体 | 圆体-简 粗体 `YuantiSC-Bold.ttf`（主字体 `ZH`）、苹方-简 中粗 `PingFangSC-Semibold.ttf`；回退 Arial Rounded Bold、Arial Unicode、Hiragino Sans GB；代码字体 Menlo | `lib.mjs:11-22`；`build/fonts/` 两个 TTF 仍在（2026-10-16 抽取） |
| 声线 | 旁白 `zh-CN-XiaoyiNeural`（rate 0、pitch 0）；小王子 `zh-CN-YunxiaNeural`（rate +2%、pitch +4Hz） | `script.json:3-6`；缓存命中证明已执行 |
| 全局语速 | 首轮 +0% 超时 → 自动提到 **+3%**（小王子实际 +5%） | 由 `build/tts_cache` 复算（§6） |
| 配乐 / 音效 | 全部在 `audio.mjs` 中用 JS 合成（Karplus–Strong 拨弦、正弦/方波、RBJ 带通噪声），无采样素材 | 代码中存在；最终混入成片（成片有音轨，本轮未试听） |
| 硬件 | 未记录；字体路径 `/System/Library/AssetsV2/...`、`Hiragino Sans GB.ttc` 表明在 macOS 上运行 | 推断 |

注意：

- `package.json` 的 `name` 是 `geo-celestial-bodies`，与本片无关——**推断**是从另一个作品目录复制骨架后未改名；它不影响运行（脚本直接 `node render.mjs`）。
- `tts.py:19-25` 写死了一个 WSS 端点覆盖：
  > Since Oct 2026 the legacy speech.platform.bing.com host answers synthesis with 403; Edge itself now synthesizes via api.msedgeservices.com (see rany2/edge-tts#490, #491). Remove this override once an edge-tts release ships the fix.

  这是对 `edge-tts` 上游故障的临时补丁（代码中存在；从缓存可知当时合成成功）。
- 角色/模型名（Fable 5.1、Image 2.5、Tripo H3.1 / Smart Mesh P2.0、Opus 5.5、Astra、Blender、Auto-Rig Pro、Unity MCP、Mixamo）是**视频讲解的内容**，不是制作本片用到的工具；本片的画面一律由 Canvas 代码绘制。

## 4. 镜头与时间轴

时间来源说明：`build/timeline.json` 已被清理，本表时间是本次用 `tts.py` 的公式（`LEAD_IN=1.2`、每段 `gap`、末词结束 +0.12 s、`TAIL=2.6`）和 `build/tts_cache` 中 +3% 语速那一批缓存的词时间戳**离线复算**得到，总长 237.01 s，与成片 237.000 s 吻合（证据等级：复算一致，非原始日志）。场景边界 = 代码中 `from/to` 表达式代入复算时间；相邻场景有 `XF = 0.35 s` 交叉淡化（`render.mjs:22, 90-94`）。

| # | 场景 id | 约时间 (s) | 口播段 | 画面内容 | 实现入口 | 关键参数 |
| --- | --- | --- | --- | --- | --- | --- |
| 1 | `intro` | 0.0–18.2 | hook, story, prince1 | Kitty 挥手；帖子卡片（Sac @Saccc_c · 2026-10-15）弹出；小王子在小行星上来回走；「近 300 万浏览」数字滚动 + 星爆；小王子气泡「走来走去啦！」 | `scenes-a.mjs:39-79` | `ownKitty: true`（Kitty 由场景自己摆位）；浏览数 `300*easeOut(...)` |
| 2 | `ai` | 18.2–24.3 | ai | 四张卡「场景搭建/人物制作/代码开发/调试」逐词弹出，再各盖一个橙色「AI」章；底部「主要工作，都交给 AI！」 | `scenes-a.mjs:82-108` | 每张卡在 `at('ai', 词) - 0.15` 弹出 |
| 3 | `parts` | 24.3–30.4 | parts | 三张编号卡 1/2/3 | `scenes-a.mjs:110-126` | 每卡配 marimba 音 n=76/79/84 |
| 4 | `relay` | 30.4–51.7 | flow, f1–f5 | 「五棒接力」：五个屏幕头机器人（Fable 5.1 / Image 2.5 / Tripo / Opus 5.5 / Astra）依次亮起，金色星星作接力棒抛物线跳站 | `scenes-b.mjs:9-47` | 工具颜色表 `lib.mjs:67-75`；接力棒高度 `222 - sin(kπ)*40` |
| 5 | `tools` | 51.7–78.1 | blender, unity, mcp | Blender 列（`$ blender --background` 终端条、优化 Tripo 模型、整体建模、Auto-Rig Pro）、Unity 打包、Unity CLI + Pipeline + MCP 让 Claude 操作 | `scenes-b.mjs:49-…` | 每个要点用 `at('blender','命令行版')` 等词点驱动 |
| 6 | `ref` | 78.1–96.3 | sceneT, s1, s1b | 程序绘制的「参考画」从左向右揭开；定画风/配色/位置；缩到左侧后补俯视图、斜侧视图 | `scenes-c.mjs:9-93` | 参考画 760×460 画布坐标，`groundY(x)=900-√(760²-x²)` |
| 7 | `assets` | 96.3–111.9 | s2, s2b | AI 在参考画上给火山×2/玫瑰/路灯打框（Tripo 生成）与 Blender 直接搭的区分，盖「确认」；Tripo H3.1 章 | `scenes-c.mjs:95-147` | 框坐标 `BOXES`（`scenes-c.mjs:18-20`） |
| 8 | `build` | 111.9–132.2 | s3, s3b, s3c | Opus 机器人把白模道具沿抛物线摆上木质舞台，落位后变彩色；三机位相机 + 「审查 agent」盲审，路灯位置不符先打红叉、迭代后变绿勾 | `scenes-c.mjs:149-…` | 道具抛物线 `y = lerp(...) - sin(kπ)*180`；路灯偏移 `lerp(90,0,fix)` |
| 9 | `views` | 132.2–146.5 | charT, c1, prince2 | 人物建模五步链；小王子 T-pose 四视图（正/侧/背/另一侧） | `scenes-d.mjs:11-63` | `TPOSE = {armL: π/2, armR: π/2, legL: .12, legR: .12}` |
| 10 | `tripo` | 146.5–171.3 | c2, c2b, c2c | Smart Mesh P2.0 卡、设置面板（多视图、四图上传、四边形拓扑、精度最高）；白模加四边形网格 → Texture 上色；形状不对时局部重生成 | `scenes-d.mjs:65-158` | 网格叠加见 §5「白模/线框」 |
| 11 | `rig` | 171.3–183.4 | c3, c3b | Auto-Rig Pro 绑骨：骨骼覆盖层；测试抬手、弯腿、转身 | `scenes-d.mjs:160-…` | `prince(..., {bones})` |
| 12 | `mixamo` | 183.4–194.2 | c4 | 搜索框按关键词出三个火柴人动作（走路/挥手/跳跃），FBX 飞入，匹配骨骼打勾 | `scenes-e.mjs:9-77` | 动作表 `ACTIONS` |
| 13 | `water` | 194.2–216.7 | c5, c5b, c5c, prince3 | 白色「最接近的 Mixamo 动作」幽灵 → 手臂 IK 控制器把手伸到水壶把手 → 手指握持特写 → 「微调 #1…#6」→ 三个绿勾（不穿模/不脱手/不悬空）→ 小王子走到玫瑰旁浇水、爱心上飘 | `scenes-e.mjs:79-151` | `REACH = {armR: .75, elbR: -.5}`；握点 `GRIP` 由关节正运动学预先算出 |
| 14 | `outro` | 216.7–237.0 | recap, github, bye | 四步总结图标链；GitHub「下周开源」卡；Kitty 跳跃挥手、小王子在星球上挥手；彩纸 | `scenes-e.mjs:153-…` | `to: 1e9`；彩纸 160 片 |

全局层（每帧都画，`render.mjs:108-141`）：

- 背景：两色线性渐变 `#0b0d2e → #2a2470` + 两团粉/青径向光 + 150 颗视差闪烁星（`render.mjs:25-44`）。
- 场景层：按 `sceneAlpha` 交叉淡化，进入时从 0.94 放大到 1、退出时放大到 1.05（`render.mjs:118-119`）。
- 震屏：`EVENTS` 中带 `shake` 的事件做衰减抖动（`render.mjs:96-106`；本片事件表里未见 `shake` 字段，属于模板能力）。
- 角落 Kitty：当场景未声明 `ownKitty` 时，Kitty 以 0.5 倍站在左下角，口型随旁白逐词开合（`render.mjs:127-133`）。
- 角标、烧录字幕、首尾 0.4/0.6 s 黑场淡入淡出（`render.mjs:136-140`）。

## 5. 关键算法与实现

> 区分：视频**讲解的知识**是 Sac 的 AI 3D 游戏工作流（PRD→参考图→Tripo→Opus 操控 Blender/Unity→审查 agent 盲审→Auto-Rig Pro→Mixamo→IK 微调）。本节写的是**制作本视频所用的算法**——全部是 2D Canvas 与音频 DSP，没有用到任何 3D 引擎或生成模型。

### 5.1 管线总览：「代码逐帧渲染 + TTS 驱动时间轴」

```text
script.json ──tts.py──▶ build/tts_cache/<sha1>.mp3/.json   (每句一个，带逐词时间戳)
            │          build/seg<i>.mp3 → ffmpeg adelay+amix+loudnorm → build/narration.wav
            │          build/timeline.json  (每段 start/end/words[]/cues[]，总时长)
            └────────▶ out/<TITLE>.srt
build/timeline.json ──lib.mjs(TL, S, E, at)──▶ scenes-a..e.mjs  (场景表 + 音效事件表，时间全用「词」表达)
render.mjs: for i in 0..DURATION*FPS: renderFrame(i/FPS) → RGBA → ffmpeg stdin
            audio.mjs: buildMusic(DURATION) → music.wav；buildSfx(EVENTS) → sfx.wav
            ffmpeg: rawvideo + narration + music(侧链闪避) + sfx → limiter → H.264/AAC MP4
```

核心思想：**语音先行，画面跟着语音走**。先把旁白合成出来，拿到每个词真实的发音时刻；所有动画、字幕、口型、音效都写成「某段里某个词开始前 0.15 秒」这种相对时间。改稿或改语速后重跑 `tts.py`，画面会自动重新对齐，不需要手工改任何秒数。

### 5.2 TTS 与逐词时间戳（`tts.py:44-85`）

- 每句缓存键：`sha1(f"{voice}|{rate}|{pitch}|{text}")[:16]`，缓存 `.mp3`（音频）+ `.json`（词表）。改一句只重合成这一句；语速变化则全量重合成（键里含 rate）。
- `edge_tts.Communicate(text, voice, rate="+3%", pitch="+4Hz", boundary="WordBoundary")` 流式返回音频块和 `WordBoundary` 事件；offset/duration 单位是 100 ns（`TICKS = 1e7`）。
- 每个词用 `text.find(chunk["text"], cursor)` 回填它在原文中的字符下标 `idx`，这样后续可以用「原文子串」反查时间。实际缓存（hook 段，+3%，键 `a93db02d06e08797`）前几项：`{"text":"嗨","idx":0,"start":0.049,"end":0.400}`、`{"text":"我","idx":2,…}`、`{"text":"Kitty","idx":4,"start":1.080,"end":1.481}`、`{"text":"今天","idx":10,"start":2.027,…}`——中文按词/字切分，标点不产生词，`idx` 跳过标点。
- 免费端点 429 限流时按 8/16/24/32 s 退避重试，最多 5 次（`tts.py:52-62`）。

### 5.3 自动语速适配（`tts.py:145-158`）

```python
available = TARGET - LEAD_IN - sum(gaps) - TAIL
rate = script.get("rate", 0)
while True:
    words  = synth(script, rate)                      # 每段 rate + 声线自带 rate
    speech = [w[-1]["end"] + 0.12 for w in words]     # 以末词结束为准，丢弃 mp3 尾部约 0.6 s 静音
    total  = LEAD_IN + sum(speech) + sum(gaps) + TAIL
    if total <= TARGET or rate >= 30: break
    rate = min(30, max(rate + 2, ceil(rate + (sum(speech)/available - 1)*100) + 1))
```

本片实际过程（由缓存复算，两批缓存文件的写入时间分别为 10:51:49–10:52:49 与 10:52:50–10:53:52）：

| 轮次 | 语速 | 复算总长 | 结果 |
| --- | --- | --- | --- |
| 1 | +0% | 243.35 s | 超过 `TARGET = 240` |
| 2 | +3% | 237.01 s | 采用；成片 237.000 s |

### 5.4 时间轴与字幕分句（`tts.py:160-174, 106-136`）

- 第 i 段开始时刻 `t_i = LEAD_IN + Σ_{j<i}(speech_j + gap_j)`，各词时间加上 `t_i` 变为全片绝对时间。`gap` 在 `script.json` 中逐段手调：句内 0.3–0.4 s，章节末 0.9–1.0 s 作为「换章呼吸」。
- 字幕：若段落写了 `captions` 数组就按人工分句，否则按中文标点切分、每块 ≤14 字（`split_caption`）。每块的起止 = 该子串覆盖的第一个词 start 到最后一个词 end——字幕时点来自真实发音，不是按字数估算。
- `lang == "en"` 分支（英文整句 + 中文释义）在本片未用到，是模板能力。

### 5.5 「词锚点」API（`lib.mjs:24-42`）

```js
export const S = (id) => SEG[id].start;      // 段开始
export const E = (id) => SEG[id].end;        // 段末词结束
export function word(id, w, nth = 0) { ... } // 段内第 nth 次出现的子串 w → {start, end}
export const at = (id, w, nth) => word(id, w, nth).start;
```

`word()` 先在段文本里找子串下标，再找覆盖该下标的 TTS 词；子串跨多个词时 end 取最后一个词。找不到直接 `throw`，所以改稿删掉某个被引用的词时渲染会立即报错，而不是静默错位。场景写法示例（`scenes-a.mjs:93`）：`const p = pop(t, at('ai', w) - 0.15, 0.45);`——提前 0.15 s 开始弹出，让画面略早于声音到达。

### 5.6 场景系统与逐帧渲染（`render.mjs`）

- `render.mjs:16-21` 依次动态导入存在的 `scenes-a..e.mjs`，拼接每个文件导出的 `scenesX`（场景表）和 `eventsX`（音效事件表）。拆成 5 个文件，每个 137–252 行。拆分原因未记录；它与项目 `.github/copilot-instructions.md` 中「每次写入约 250 行以内、避免 502」的规则一致（该规则写于 ocean-shallows 踩坑之后，与本片的先后关系无法确认）。
- 场景对象 `{ id, from, to, draw(ctx, t), ownKitty? }`，`draw` 是 `t` 的**纯函数**：同一个 t 永远画出同一帧，因此 `--still 3.2 7.6` 可以随意抽帧预览（`render.mjs:148-153`），也不存在累积误差。
- 缓动库：`easeOut` 三次、`easeIO` 三次、`easeBack`（c1 = 2.2 的回弹），`pop(t, t0, d=0.45)` 就是 `easeBack(prog(t, t0, t0+d))`，全片「弹出」手感都来自它（`lib.mjs:44-55`）。
- 伪随机 `rand(i) = fract(sin(i*127.1+311.7)*43758.5453)` 用于星空、彩纸，保证每帧一致（`lib.mjs:56-59`）。
- 输出：`ctx.getImageData` 取 RGBA，写入 ffmpeg stdin；`write()` 返回 false 时 `await once(ff.stdin, 'drain')` 做背压（`render.mjs:170-176`），避免内存堆积。全程单进程串行，无并发。

### 5.7 角色「绑骨」与 IK 演示（`chars.mjs:68-221`、`scenes-e.mjs:79-151`）

- 小王子是一个 2D 两段式肢体正运动学：`princeJoints(pose, view)` 由肩/髋基点、上臂/前臂长度 62/58、大腿/小腿 74/70 和角度 `armR/elbR/legL/kneeL…` 算出肘、手、膝、脚位置；`view` 改变肩宽和左右展开系数，实现正面/侧面/背面/四分之三视图。
- 白模/线框模式：在离屏 canvas 上先画白模，再用 `globalCompositeOperation = 'source-atop'` 只在身体像素上叠 20 px 正弦扰动网格，模拟「四边形拓扑」；离屏层按尺寸缓存复用（`chars.mjs:164-221`）。
- 骨骼覆盖：`drawBones` 在关节处画青色圆点和连线，用于 Auto-Rig Pro 场景。
- 「IK」镜头其实是**预计算的正运动学**：先用 `REACH` 姿态求出手的位置作为水壶握点 `GRIP`，再把手臂角度从默认插值到 `REACH`，最后举起时让水壶跟随 `J.haR`（手关节）移动，从而保证「不脱手、不悬空」。这是演示效果，不是真正的逆运动学求解。

### 5.8 口型（`lib.mjs:216-224`）

`talkLevel(t, ids)`：当前时刻落在某个旁白词的 [start, end] 内时返回 `0.45 + 0.55·|sin(22t)|`，否则 0。Kitty 无嘴，`talk` 只驱动身体上下晃动（`|sin 9t|·7`）、头部轻摆和蝴蝶结抖动（`chars.mjs:15-57`）；机器人则是屏幕上的嘴弧开合（`chars.mjs:264`）。词间停顿自动闭口。

## 6. 声音与字幕

### 6.1 配音

| 角色 | 声线 | 语速 / 音高（最终） | 段数 | 证据 |
| --- | --- | --- | --- | --- |
| Kitty 旁白 | `zh-CN-XiaoyiNeural` | +3% / +0 Hz | 35 | `script.json:4`；缓存命中 rate=3 |
| 小王子 | `zh-CN-YunxiaNeural` | +5%（自带 +2 再加全局 +3）/ +4 Hz | 3（prince1/2/3） | `script.json:5`；缓存命中 rate=5 |

- 旁白混音：每段 mp3 重采样到 48 kHz，`adelay` 到各自绝对起点，`amix normalize=0` 叠加，再 `highpass=f=80, loudnorm=I=-16:TP=-1.5:LRA=11`，`apad + atrim` 补齐到全片时长（`tts.py:176-190`）。目标响度 −16 LUFS 是**代码参数**；成片实测响度未记录（本轮也未测）。
- 首句前留 `LEAD_IN = 1.2 s` 给标题动画，结尾留 `TAIL = 2.6 s` 给彩纸（`tts.py:33-35`）。

### 6.2 字幕

- `tts.py` 输出 `out/Kitty带你看-AI做3D游戏.srt`（108 条，首条 `00:00:01,249 --> 00:00:02,681 嗨，我是Kitty！`，末条 `00:03:53,539 --> 00:03:54,291 我们下次见！`）。首条 1.249 s = LEAD_IN 1.2 + 首词 0.049，印证 SRT 由逐词时间戳生成。
- 同一组 cue 也烧录进画面（`render.mjs:59-84`）：底部 y=958 的深紫半透明胶囊、50 px 圆体白字；小王子台词加金色「小王子」标签、字色 `#ffe9a8`。cue 显示到下一条 cue 开始为止，段末 cue 在末词后 0.12–0.3 s 淡出，避免字幕闪烁。
- 字幕分句大多在 `script.json` 中手写 `captions` 数组（如 `blender` 段拆成 6 行），保证每行意群完整、长度适合 1080p 一行显示。

### 6.3 配乐（`audio.mjs:149-185`，全部程序合成）

- 112 BPM，C–G–Am–F（I–V–vi–IV）循环，每小节 8 个八分音符拨弦（Karplus–Strong，阻尼 0.9965，琶音顺序 `[0,2,1,3,2,1,3,2]`，偶数拍重音）。
- 低音：每小节第 1 拍和第 3.5 拍各一个根音正弦 + 八度泛音。
- 沙锤：16 分音符 7–9 kHz 带通噪声，第 3 个 16 分音最重。
- 奇数小节加三音钟琴「回应」，频率随和弦根音移调。
- 首 1.2 s 淡入、末 2.5 s 淡出；随机种子固定（`rng(42)`），每次渲染一致。

### 6.4 音效（`audio.mjs:77-147`）

15 种卡通音效：`pop`（下滑正弦）、`note`（马林巴）、`ding`/`sparkle`（加法合成钟声，分音比 1 : 2.76 : 5.4 : 8.93）、`whoosh`（中心频率指数扫动的 RBJ 带通噪声）、`boing`、`slam`、`stamp`、`tick`、`scribble`（9 Hz 调幅高频噪声模拟写字）、`buzz`（两路 150/157 Hz 方波拍频，表示「不对」）、`alarm`、`tada`、`confetti`、`wind`。每个音效由场景文件中的事件 `{type, t: at(段, 词) - 偏移, …参数}` 触发，与画面使用同一组词锚点，所以声画天然同步。种子 `rng(7)`。

### 6.5 总混音（`render.mjs:158-169`）

```text
[旁白] asplit → v1(侧链键) / v2(直通)
[音乐] volume 0.7 → sidechaincompress(threshold 0.05, ratio 5, attack 20 ms, release 400 ms, 键=旁白)
[音效] volume 0.5
amix(旁白, 闪避后的音乐, 音效; normalize=0, duration=first) → alimiter limit 0.93 → AAC 192k
```

`writeWav` 把单声道浮点缓冲按峰值归一到 0.98 后复制成双声道 16-bit（`audio.mjs:187-203`），所以音乐和音效轨本身是单声道居中。检查范围：仅 ffprobe 确认存在 48 kHz 立体声 AAC 音轨；本轮未试听、未测响度。

## 7. 迭代、问题与修正

项目没有 `plan/`、制作日志或对话记录，以下仅来自代码注释和缓存痕迹：

| 问题 / 决策 | 证据 | 处理 |
| --- | --- | --- |
| Edge TTS 旧域名 2026-10 起返回 403 | `tts.py:19-25` 注释 | 把 `WSS_URL` 指向 `api.msedgeservices.com`，注明上游修复后删除 |
| 免费端点 429 限流 | `tts.py:56-62` | 线性退避重试 5 次 |
| mp3 尾部约 0.6 s 静音会吃掉段间停顿 | `tts.py:151` 注释 | 段长按末词结束 +0.12 s 计算，而不是 mp3 时长 |
| 首轮 +0% 总长 243.35 s 超过 240 s 上限 | 缓存中存在 rate 0 与 rate 3 两批 | 自动按超出比例一步跳到 +3%，不逐级试探（`tts.py:157` 注释 "jump straight to the speed-up the overshoot implies"） |
| `@napi-rs/canvas` 只能注册 `.ttc` 的第一个字重 | `fonts.py:3` | 用 fontTools 从系统 TTC 中按全名抽出 Bold/Semibold 存成独立 TTF |
| 系统缺圆体时 | `fonts.py:26` | 打印提示，渲染回退到 Hiragino Sans GB |
| 改稿删词导致锚点失效 | `lib.mjs:36` | `word()` 找不到子串时直接抛错，渲染前暴露问题 |

时间线（文件修改时间，非日志）：字体抽取 2026-10-16 10:50；TTS 两批 10:51:49–10:53:52；成片 mp4 写入 11:10:24。据此**推断**最后一次全片渲染加编码在 17 分钟以内（区间内可能还有其他操作，无法确认单帧耗时）。

画面迭代（多少版、改了什么）、用户反馈与验收：**未记录**。

## 8. 可复用制作经验

1. **让语音决定时间轴，动画只引用「词」**：所有 `from/to`、弹出、音效都写成 `at('段', '词') ± 偏移`，改稿或改语速只需重跑 TTS（依据：`lib.mjs:31-42`、`scenes-*.mjs` 事件表）。
2. **按「末词结束」而不是音频文件长度排段**：TTS mp3 尾部有约 0.6 s 静音，直接拼会让停顿忽长忽短（依据：`tts.py:151-152`）。
3. **给片长设上限并自动反推语速**：一次性按超出比例算出语速，避免逐级试探多次付费/限流（依据：`tts.py:145-158`，本片 +0%→+3% 一步到位）。
4. **TTS 缓存键包含声线、语速、音高和全文**：只重合成改动的句子，同时保留历史版本可复查（依据：`tts.py:46`；本片缓存恰好留下两轮语速痕迹）。
5. **`draw(ctx, t)` 写成时间的纯函数 + `--still` 抽帧**：任意时刻可单独预览，不必从头渲染（依据：`render.mjs:148-153`）。
6. **字幕手工分句 + 逐词对时**：`captions` 控制意群，时点仍取 TTS 词边界，既好读又准（依据：`script.json`、`tts.py:122-136`）。
7. **音乐用侧链压缩让位人声**，音效与画面共用同一事件表，保证声画同步且无需手动对齐（依据：`render.mjs:164-165`、`audio.mjs:142-147`）。
8. **讲 3D 流程不一定要 3D**：用 2D 正运动学 + 白模/线框/骨骼三种叠加模式即可表达「建模→拓扑→绑骨→IK」（依据：`chars.mjs:73-221`）。

## 9. 复现入口

以下命令来自源码注释（`tts.py:4`、`render.mjs:2-3`、`fonts.py:4`），**本轮均未执行**。

```bash
cd ai-3d-game
npm install                          # @napi-rs/canvas 1.0.11（package-lock 锁定）
python3 -m venv .venv && .venv/bin/pip install edge-tts aiohttp fonttools   # 版本未记录
.venv/bin/python tts.py              # → build/seg*.mp3, build/narration.wav, build/timeline.json, out/*.srt
node render.mjs --still 3.2 7.6      # 可选：抽帧预览 → build/still_<t>.png
node render.mjs                      # 首次自动调用 .venv/bin/python fonts.py；→ build/music.wav, build/sfx.wav, out/Kitty带你看-AI做3D游戏.mp4
```

- 顺序约束：`lib.mjs:25` 在模块加载时读取 `build/timeline.json`，所以**必须先跑 `tts.py`**；当前仓库中该文件已被清理。
- 离线可复现性：`build/tts_cache/` 完整保留了 38 段 × 2 种语速共 76 对 mp3/json。`tts.py` 首轮（+0%）和第二轮（+3%）都会命中缓存，理论上**无需联网**即可重建 `timeline.json` 与 `narration.wav`（仍需安装 `edge-tts` 以通过 import）。推断，未验证。
- 系统依赖：ffmpeg（含 libx264）；macOS 系统字体资产 `Yuanti.ttc`、`PingFang.ttc`（`fonts.py:12-16`），`build/fonts/` 中两个 TTF 已存在可直接用。
- 修改入口：口播稿与停顿 → `script.json`；片长上限/首尾留白 → `tts.py:32-35`；画面 → 对应 `scenes-*.mjs`；配色与工具色 → `lib.mjs:61-75`；配乐 → `audio.mjs:149-185`；混音比例 → `render.mjs:164-165`。

## 10. 参考来源

| 来源 | 类型 | 作者 / 平台 | 借鉴内容 | 本片对应 | 访问状态 |
| --- | --- | --- | --- | --- | --- |
| https://x.com/Saccc_c/status/2110694725640474759 | 内容来源（被解读的帖子，X 长文 Article） | Sac（@Saccc_c），X | 全片内容：五工具接力流程、Blender CLI + Auto-Rig Pro、Unity CLI + Pipeline + MCP、场景三步（参考图→拆资产/H3.1→Opus 摆放 + 多机位迭代）、人物建模步骤；「近 300w 浏览」 | 全部 14 个场景；帖子卡片画面 `scenes-a.mjs:10-22` | 2026-11-12 web_fetch 成功，读取了正文前约 3000 字（标题、引言、第 1 部分、第 2 部分前 3 步）；人物建模部分未在本次读取，其对应关系以 `script.json` 为准 |

- 本地材料中帖子日期写作 2026-10-15（`script.json:2`、`scenes-a.mjs:16`）；本次抓取的页面未显示发布时间，无法独立核实。
- 本次读到的原帖部分与 `script.json` 一致：标题、「近 300w 浏览」、五工具分工、Blender CLI 与付费 Auto-Rig Pro、Unity CLI + Pipeline 包 + 官方 Unity MCP、俯视图看布局/斜侧视图看高度层次、让 AI 判断哪些资产用 Tripo 生成并在图上标注、推荐 Tripo H3.1、Opus 5.5 按参考图操控 Blender + Unity 并多角度截图迭代。
- 原帖还给出可复用提示词（如「基于上传的场景参考图，帮我生成一张该场景的全面貌参考图片…」），视频未逐字引用提示词。
- 「审查 agent 多机位盲审」「下周 GitHub 开源」两点在本次读取的片段中未出现，可能位于未读取部分，**未核实**。
- `tts.py:20` 提到的 `rany2/edge-tts#490, #491` 是技术资料（edge-tts 上游 issue），本次未访问。

## 11. 证据与限制

实际读取的关键文件（sha256 前 16 位取自 fingerprints.json）：

| 文件 | 行数 | sha256[:16] | 读取程度 |
| --- | --- | --- | --- |
| `ai-3d-game/script.json` | 51 | `cf9e368e468e2c36` | 全文 |
| `ai-3d-game/tts.py` | 207 | `47d5d5064db72589` | 全文 |
| `ai-3d-game/render.mjs` | 180 | `23ce50c6637c7903` | 全文 |
| `ai-3d-game/audio.mjs` | 203 | `f9e04b2cfd54743c` | 全文 |
| `ai-3d-game/lib.mjs` | 246 | `dffe877d575e9be0` | 全文 |
| `ai-3d-game/fonts.py` | 36 | `a6ac7264e548bd3d` | 全文 |
| `ai-3d-game/scenes-a.mjs` | 147 | `9fc8094e860c6bad` | 全文 |
| `ai-3d-game/scenes-b.mjs` | 137 | `3676efa17048ba32` | 场景 4 全文、场景 5 前半、导出表 |
| `ai-3d-game/scenes-c.mjs` | 252 | `9b5cbf7f5017a09b` | 参考画常量、场景 6、场景 8 大部分、导出表 |
| `ai-3d-game/scenes-d.mjs` | 228 | `8b55a171937143e8` | 常量、场景 10 开头、导出表 |
| `ai-3d-game/scenes-e.mjs` | 216 | `e9c4347f354aa855` | 场景 13、14 全文、导出表 |
| `ai-3d-game/chars.mjs` | 290 | `669a318f9eae7f30` | 小王子关节与绘制模式、talk 参数 |
| `ai-3d-game/props.mjs` | 197 | `441b6054d3d2d0e7` | 仅函数目录 |
| `ai-3d-game/package.json` / `package-lock.json` | — | `65b576e2988f7c37` / `5cb2167043d396d3` | 全文 / 包版本 |
| `ai-3d-game/out/Kitty带你看-AI做3D游戏.srt` | 108 条 | `e743c11a78103a09` | 首尾条目与条数 |
| `ai-3d-game/out/Kitty带你看-AI做3D游戏.mp4` | — | 34,308,455 字节 | 仅 ffprobe 媒体头 |
| `ai-3d-game/build/tts_cache/*.json` | 76 个 | 见 fingerprints.json | 以哈希键匹配 script 并复算时间轴 |

缺失与限制：

- 无 `prompt.md`、`plan/`、制作对话：原始需求、画面迭代与用户反馈**未记录**。
- `build/timeline.json`、`narration.wav`、`music.wav`、`sfx.wav`、`seg*.mp3`、预览帧已清理；§4 时间为复算值。
- Python 依赖、Node、ffmpeg 的版本未记录；运行硬件未记录。
- 成片仅做 ffprobe 技术检查；**未做视觉检查、未试听、未测响度；用户验收未记录**。
