---
type: video-production-dossier
project: geo-story-studio-opus
video_id: nebula-nursery
title: 星云都是“恒星的摇篮”吗？
curriculum: script.json 未记录（README 归入“必修第一册 第一章第一节 地球的宇宙环境”）
captured: '2026-11-12'
status: 成片存在（竖屏 1080×1920，77.6 s）；技术检查/视觉检查/用户验收未记录
source_fingerprint: 5ffdfdb34543628e7aa0f4cf03ca14302f887da060446af1d73f3b3a8a29dae8
tags:
- video-creation
- production-capture
- geography-teaching
- canvas-frame-render
- edge-tts
---

# 星云都是“恒星的摇篮”吗？ · 四声线拟人短片：恒星形成区 vs 行星状星云（Canvas 逐帧 + 逐词时间戳）

<!-- capture-source-sha256:5ffdfdb34543628e7aa0f4cf03ca14302f887da060446af1d73f3b3a8a29dae8 -->

本文是**根据项目文件做的材料回溯**：源码、`script.json`、`build/timeline.json`、`out/` 下的成片与字幕，以及证据包中的 ffprobe 结果和 sha256 清单。证据包里 `host_session` 为空，**未发现制作会话**，制作对话、用户需求原文和修改过程都无法找回。本次没有运行任何项目脚本（`tts.py`、`render.mjs`、npm、pip 都没跑），也没有打开视频或音频。

## 1. 当前结论与制作路线

**状态：成片存在**：`out/星云都是恒星的摇篮吗.mp4` 和同名 `.srt`。目前只有证据包里的 ffprobe 媒体头信息；视觉检查、试听、响度测量、用户验收、是否发布，都**未记录**。

| 项 | 值 | 来源 |
| --- | --- | --- |
| 分辨率 / 画幅 | 1080×1920，9:16 竖屏 | ffprobe；`lib.mjs:7` `W = 1080, H = 1920, FPS = 30` |
| 帧率 / 帧数 | 30 fps；`Math.round(77.603 × 30)` = 2328 帧 | ffprobe；`render.mjs:160`（按时长推算） |
| 时长 | 77.600 s（timeline 77.603 s） | ffprobe；`build/timeline.json` |
| 视频编码 | H.264，yuv420p，CRF 19，preset medium，`+faststart` | ffprobe（codec）；`render.mjs:157-158`（参数） |
| 音频编码 | AAC 48 kHz 立体声，192 kbps | ffprobe；`render.mjs:158` |
| 文件大小 | 15,755,453 字节（约 15.8 MB，平均约 1.62 Mbps） | 证据包 |
| 字幕 | `.srt` 共 39 条（2285 字节），画面另有烧录字幕 | `timeline.json` 的 cues；`render.mjs:67-91` |
| 其他版本 | 没有（无推荐版、横屏版或封面） | `out/` 列表 |

文件时间：`script.json` 10-19 21:03，TTS 产物（`narration.wav`、`timeline.json`、`.srt`）21:05，`render.mjs` 21:07，成片 21:20。在本批 5 个视频（星云 → 太空 → 太阳系列）中，这一条最早。git 中只有初始提交 `4831a07`（2026-10-23）一条相关记录。

一句话管线：

> `script.json`（18 段，旁白 + 三个拟人角色）→ `tts.py` 调 edge-tts 逐句合成，用 WordBoundary 取得逐词时间 → `build/timeline.json` + `narration.wav`（adelay/amix/loudnorm）+ SRT → `render.mjs` 读取时间轴，用 4 个场景模块（6 个场景）按“某个词说出的时刻”触发动画；`audio.mjs` 用程序合成配乐和音效 → 每帧 RGBA 经 stdin 管道送入 ffmpeg → 输出 H.264/AAC MP4。

## 2. 教学目标与课标对齐

- **curriculum 字段：缺失**。`script.json` 的键只有 `title`、`source`、`rate`、`voices`、`segments`，没有 `curriculum`、`based_on`、`numbers`（不符合 AGENTS.md §6，见 §7）。README 目录把它归在“必修第一册 第一章第一节 地球的宇宙环境”，简介写的是“星云的不同来源：有的孕育新恒星，有的是恒星演化晚期的产物”。具体对应哪条课标，**未记录**。
- **核心知识点**：星云是宇宙中的气体和尘埃云。“恒星摇篮”只适用于其中的恒星形成区（例：猎户座大星云）；另一些星云来自恒星晚年抛出的外层物质（例：环状星云，也叫行星状星云，和行星无关）。判断时要看星云的“来源和内部过程”。
- **叙事结构**（按实际文案）：
  - 导入：`hook`/`stick` 把“恒星摇篮”贴纸贴满所有星云，`ringNo` 环状星云喊“等等！我可不是摇篮！”
  - 原理：`what1-2` 讲定义；`orion1`–`baby` 讲致密区在引力下收缩，形成原恒星；`ring1`–`name` 讲恒星晚年抛出外层物质，以及“行星状星云”这个名字的来历。
  - 结论：`sum1-2`“有些见证诞生，有些记录晚年”。
  - 迁移：`end1-2` 给出两问清单：“它从哪里来？它正在发生什么？”
  - 收尾：`cta` 引导到评论区。
- **科学边界**：
  - 文案用了“在适合的致密区域”“逐渐形成原恒星”这类限定。
  - 坍缩动画旁边有标签“快进示意 · 真实过程非常漫长”（`scenes-b.mjs:21`）。
  - 文案明确说“行星状星云……和行星没关系”。
- **数字与出处**：文案里没有具体数值。`source` 字段：“Geo Story Studio · douyin-nebula-nursery v002 分镜；事实依据 NASA Hubble’s Nebulae、NASA Space Place What Is a Nebula?、NASA/ESA Webb Ring Nebula (NIRCam Image)”。只写了资料名称，没有 URL，也没有逐条对应。
- **based_on（推断）**：按 AGENTS.md §1 的命名规则，源 build 应为 `geo-story-studio/storage/builds/douyin-nebula-nursery/v002/`。`script.json` 只写了“douyin-nebula-nursery v002 分镜”，没有写路径，本次也没有核对源 build。

## 3. 技术与依赖

| 类别 | 内容 | 证据 |
| --- | --- | --- |
| 渲染 | Node ESM + `@napi-rs/canvas`，声明 `^1.0.11`，lock 锁定 **1.0.11**（darwin-arm64 同版本） | `package.json`；`package-lock.json` |
| npm scripts | 无 | `package.json` |
| TTS | Python `edge-tts`（`aiohttp` 用于捕获 429）；**没有** requirements 文件，版本未声明 | `tts.py:15-17` |
| 字体抽取 | `fonts.py` 用 `fontTools.ttLib.TTCollection`，从 macOS 系统字体集合中抽出圆体-简 粗体、苹方-简 中粗；回退顺序 Arial Rounded / Arial Unicode / Hiragino | `fonts.py`；`lib.mjs:10-21` |
| 编码 / 混音 | ffmpeg：TTS 阶段 `aresample/adelay/amix/highpass/loudnorm/apad/atrim`；渲染阶段 `asplit/sidechaincompress/amix/alimiter` + libx264/aac | `tts.py:176-190`；`render.mjs:150-159` |
| 本地环境（非声明） | `.venv` 里能看到 `edge_tts-7.3.1`、`fonttools-4.67.0`、`aiohttp-3.14.5` 的 dist-info | 目录列表；不代表当时制作用的就是这些版本 |

**声线**：全局 `rate: 4`，在每个角色自己的 rate 上再加 4（`tts.py:93`）。

| 角色 | voice | 实际 rate / pitch | 字幕胶囊 |
| --- | --- | --- | --- |
| 旁白 `zh` | zh-CN-XiaoxiaoNeural | +4% / +0 Hz | 无 |
| 猎户座大星云 `orion` | zh-CN-XiaoyiNeural | +8% / +4 Hz | “猎户座大星云” |
| 原恒星 `baby` | zh-CN-YunxiaNeural | +10% / +12 Hz | “原恒星” |
| 环状星云 `ring` | zh-CN-YunyangNeural | +0% / −6 Hz | “环状星云”；`ringHi` 那一句改成“中心星” |

## 4. 镜头与时间轴

时间来自 `build/timeline.json`；场景边界取 `S(id)` 减去 0.25–0.35 s，相邻场景交叉淡化 `XF = 0.4`。

| 场景 id | 时间（约） | 旁白要点 | 画面 | 模块 |
| --- | --- | --- | --- | --- |
| `hook` | 0–11.0 s | 漂亮宇宙云都是摇篮吗？啪啪贴标签；环状星云抗议 | 两团星云被贴上“恒星摇篮”贴纸，配 stamp 音效 | `scenes-a.mjs` |
| `what` | 11.0–19.6 s | 星云 = 气体 + 尘埃云；比喻只适合部分区域 | 放大镜下的气体点和尘埃粒 | `scenes-a.mjs` |
| `orion` | 19.6–37.0 s | 猎户座大星云是恒星形成区；致密区收缩成原恒星 | 拟人星云 → 镜头推进到致密云团 → 引力箭头 → 原恒星“出生” | `scenes-b.mjs` |
| `ring` | 37.0–54.3 s | 恒星晚年抛出外层 → 环状星云；“行星状星云”和行星无关 | 生命进度条“诞生→壮年→晚年”，老恒星外壳散开成环 | `scenes-c.mjs` |
| `compare` | 54.3–63.2 s | 不全是摇篮；见证诞生 / 记录晚年 | 两张“星云身份证”，盖章“见证诞生”“记录晚年” | `scenes-d.mjs` |
| `end` | 63.2–77.6 s | 先别急着叫摇篮；看来源和过程；评论区互动 | 两问打勾清单 → 全员谢幕，CTA 加彩纸 | `scenes-d.mjs` |

## 5. 关键算法与实现

> 先区分两类内容：**视频讲的科学**是星云的来源分类、引力收缩和外层抛射；**做视频用的算法**是时间戳驱动、粒子插值、合成发光和程序音频。坍缩粒子的轨迹只是视觉插值，不是物理模拟。

### 5.1 逐词时间戳 → 动画与音效锚点（`lib.mjs:34-44`）

```js
export function word(id, w, nth = 0) {
  const s = SEG[id];
  let i = -1;
  for (let k = 0; k <= nth; k++) i = s.text.indexOf(w, i + 1);
  if (i < 0) throw new Error(`"${w}" not in ${id}`);
  const hit = s.words.find((x) => x.idx <= i && i < x.idx + x.text.length) ?? s.words.find((x) => x.idx >= i);
  ...
}
export const at = (id, w, nth) => word(id, w, nth).start;
```

所有动作都挂在“某个词说出的时刻”上，例如两次“啪”的盖章音：`at('stick','啪')`、`at('stick','啪',1)`（`scenes-a.mjs:130-131`）。角色嘴型由 `talkLevel()` 控制，只在有词发音的时间段内张合（`lib.mjs:188-195`）。

### 5.2 TTS：缓存键、429 重试、端点替换、TARGET 循环（`tts.py`）

- `tts.py:22-25`：把 `edge_tts.communicate.WSS_URL` 替换为 `api.msedgeservices.com`，和 AGENTS.md §2 一致。
- `tts.py:46`：缓存键为 `sha1(voice|rate|pitch|text)[:16]`，存到 `build/tts_cache/{key}.mp3/.json`。目录里共 36 个文件，正好 18 段 × 2，没有残留的旧缓存。
- `tts.py:52-62`：只有遇到 `WSServerHandshakeError` 且状态码为 429 时才重试，最多 5 次，等待时间依次为 8、16、24、32 s。
- `tts.py:148-158`：如果超过 `TARGET = 80.0`，就按超出比例一次算出新的 rate。时间轴记录 `rate: 4`，等于 `script.json` 的全局值，说明**没有触发提速**。
- `tts.py:152`：句间距离以最后一个词的结束时间 + 0.12 s 为准，不用 mp3 文件长度（注释说明 mp3 尾部约有 0.6 s 静音）。

### 5.3 致密云团坍缩（`scenes-b.mjs:60-88`）

```js
const c = easeIO(prog(t, tGrav - 0.1, tProto + 0.1));   // “引力”→“原恒星”
const shrink = lerp(310, 120, c);
for (let i = 0; i < 44; i++) {
  const d0 = 140 + rnd(i + 11) * 300, a0 = rnd(i + 17) * TAU;
  const ci = clamp(c * (0.8 + 0.4 * rnd(i + 23)));
  const d = lerp(d0, 18, ci), a = a0 + ci * 2.4 + Math.sin(t + i) * 0.03;
  ...
  if (i % 3) gasDot(ctx, px, py, 8, fade); else dustGrain(ctx, px, py, 11, a, fade);
}
```

- 44 个粒子（气体点:尘埃粒 ≈ 2:1）。初始位置由确定性哈希 `rnd()` 生成，每次渲染结果相同。每个粒子的收缩速度略有差异，同时旋入 2.4 rad。
- 6 支金色“引力”箭头沿径向向内脉动。
- 同时显示“快进示意 · 真实过程非常漫长”胶囊，并配一个闪烁的快进图标。

### 5.4 环状星云的绘制与变形（`cast.mjs:157-189`、`scenes-c.mjs`）

- 环用椭圆画（纵横比 0.86）。外缘 40 个红色发光团以 `globalCompositeOperation = 'lighter'` 叠加，内层 3 道青色描边，中心是高温白星（戴眼镜，与 `oldStar` 的爷爷形象呼应）。
- `ringScene` 用 `m = easeIO(prog(t, tOut-0.2, tLayer+0.8))` 把“大老恒星”变形为外壳扩散、中心小星，时间区间从“向外抛出”说出时开始，到“外层物质”说完后 0.8 s 结束。另有一条生命进度条，指针滑到“晚年”（`scenes-c.mjs:8-27`）。

### 5.5 画面层与混音（`render.mjs`）

- 绘制层顺序（`render.mjs:115-125`）：背景 → 场景（交叉淡化）→ 页脚 → 字幕 → 角标 logo → 首 0.3 s 淡入和末 0.6 s 淡出到黑。
- 背景（`render.mjs:27-46`）：240 颗确定性星点闪烁，并带视差漂移。讲猎户座时整体偏暖，讲到 `ring1` 时转冷。
- 字幕（`render.mjs:67-91`）：在 `SUB_Y = 1530` 画圆角胶囊，角色台词左侧加一个说话人胶囊。字号从 56 开始逐步减 2，直到放得下画面宽度。
- logo（`render.mjs:49-61`）：位置 `x=64, y=76`，宽 290，取 `LOGO_BOX` 有效区域，透明度 0.95，后衬径向暗晕，0.3–1.0 s 淡入。与 AGENTS.md §4 的位置和尺寸一致。
- 混音（`render.mjs:154`）：音乐音量 0.55，并以旁白做 sidechain 压缩（threshold 0.05、ratio 5）；音效音量 0.5；三路 amix 后过 `alimiter=limit=0.93`。

### 5.6 程序化音频（`audio.mjs`）

- 配乐：112 BPM，I–V–vi–IV（C–G–Am–F）。用 Karplus–Strong 拨弦模拟尤克里里，加低音、沙锤（带通噪声），隔小节加一段钟琴，首尾分别淡入 1.2 s、淡出 2.5 s。
- 音效：`FX` 共 15 种（pop、stamp、whoosh、tada、confetti、wind……）。whoosh 用中心频率随时间移动的 RBJ 带通双二阶滤波器。另外，每个场景切换处（从第二个场景起）都会自动加一个 whoosh（`render.mjs:130`）。

## 6. 声音与字幕

- 旁白：4 种 voice，具体 rate/pitch 见 §3。句间停顿用每段自己的 `gap`（0.25–0.7 s），没写时默认 0.45 s；片头留白 `LEAD_IN = 1.0`，片尾留白 `TAIL = 2.4`（注释写“room for the closing card”）。
- 混音链：各句 `aresample=48000,adelay` → `amix normalize=0` → `highpass=80` → `loudnorm=I=-16:TP=-1.5:LRA=11` → `apad,atrim` 到总长（`tts.py:176-190`）。渲染阶段再叠加配乐和音效（§5.5）。成片的实际响度**未测量**。
- 字幕时间：每条字幕的起止时间取自它所覆盖的词的时间戳（`tts.py:122-136`）。`script.json` 的 `captions` 会手动指定断句，没指定的段落按标点断句，每条不超过 14 字。画面上，一条字幕会一直显示到下一条开始；每段最后一条在说完后 0.12–0.3 s 内淡出。

## 7. 迭代与问题

以下各条都有证据；标“推断”的除外。

1. **画面有“示意”声明（违反现行规则 §5）**：在 `sum1` 前 0.3 s（约 54.3 s）之前，页脚一直显示“原创科普示意 · 非观测照片 · 形状、配色和时间均为示意”，之后才换成资料来源（`render.mjs:94-98`）。坍缩场景也有“快进示意 · 真实过程非常漫长”（`scenes-b.mjs:21`）。AGENTS.md §5 写明“已经做好的视频不用回头修改，除非用户要求”。
2. **没有片尾品牌卡**：`render.mjs` 里没有 `outro()`，结尾是全员谢幕加“评论区说说它的身世！”和彩纸（`scenes-d.mjs:79-131`）。现行 AGENTS.md §4 要求片尾品牌卡，并且不要加号召类文字。这一节在 HEAD 的 AGENTS.md 中已存在；初始提交 `4831a07` 中只有“如果有片尾品牌卡……”的条件性写法。也就是说，本片制作时（10-19）这条可能还不是强制要求（**推断**）。
3. **`script.json` 缺字段**：没有 `curriculum`、`based_on`、`numbers`，和 §6 的要求不一致。
4. **TARGET 与现行规则不同**：`TARGET = 80.0`，而现行 §2 默认 `TARGET = None`（commit `35ad4da` 2026-10-26“drop narration length cap”）。不过实际 rate 仍是 +4%，没有提速。
5. **本片早于 §5 规则生效（推断）**：同一天稍晚完成的 [[space-not-empty]]，其 `render.mjs` 注释写着“AGENTS.md §5: no ‘示意’ disclaimers on screen”，并去掉了页脚声明。由此推断，§5 是在本片和 [[space-not-empty]] 之间加入或开始执行的。
6. README 目录**有收录**（第 25 行，时长记为 1:18）。

## 8. 复现入口

AGENTS.md“常用命令”和文件头注释记录的顺序（本次**没有运行**）：

```bash
python3 -m venv .venv && .venv/bin/pip install edge-tts   # fonts.py 另需 fonttools，未在任何清单中声明
npm install                                               # 安装 @napi-rs/canvas（lock 1.0.11）
.venv/bin/python tts.py            # → build/seg*.mp3, narration.wav, timeline.json；out/星云都是恒星的摇篮吗.srt
node render.mjs --still 20 45      # → build/still_<t>.png 预览
node render.mjs                    # → build/music.wav, sfx.wav；out/星云都是恒星的摇篮吗.mp4
```

`lib.mjs:11-13`：如果缺少 `build/fonts/YuantiSC-Bold.ttf`，会自动调用 `.venv/bin/python fonts.py`。字体来自 macOS 系统资源，所以只能在 macOS 上复现。

## 9. 参考来源

`script.json` 和代码中**没有任何 URL**（证据包 `urls` 为空）。只有 `source` 字段里的资料名，页脚资料行为“资料：NASA Hubble · NASA Space Place · NASA/ESA Webb”（`render.mjs:96`）：

- NASA Hubble’s Nebulae：星云类型（项目文件中提及，本次未访问）
- NASA Space Place: What Is a Nebula?：星云定义（项目文件中提及，本次未访问）
- NASA/ESA Webb Ring Nebula (NIRCam Image)：环状星云（项目文件中提及，本次未访问）
- 分镜来源：`geo-story-studio/storage/builds/douyin-nebula-nursery/v002/`（路径为按规则推断，本次未访问）

## 10. 证据与限制

| 文件 | sha256 前 12 位 | 用途 |
| --- | --- | --- |
| `script.json` | `b44ace299207` | 18 段文案、voices、source |
| `tts.py` | `2a56c49b652c` | TTS / 时间轴 / SRT / 混音 |
| `render.mjs` | `ec0651603033` | 主渲染、logo、字幕、页脚、ffmpeg |
| `lib.mjs` | `12567ee32a91` | 时间戳查询、缓动、绘图（与 [[space-not-empty]] 相同） |
| `cast.mjs` | `8cfb6539f06c` | 星云、原恒星、老恒星、环状星云角色 |
| `scenes-a/b/c/d.mjs` | `4820c65c72c9` / `6579a1da5855` / `10017da116a3` / `2b3efa338575` | 6 个场景 |
| `audio.mjs` | `f9e04b2cfd54` | 程序音频（本批 5 片完全相同） |
| `fonts.py` | `a6ac7264e548` | 字体抽取（5 片相同） |

- **已读**：`script.json` 全文、`tts.py` 全文、`render.mjs` 全文、`lib.mjs`、`audio.mjs` 全文，`cast.mjs` 和各场景文件的结构与关键段落；`build/timeline.json`（只用了段落时间和 cues）。
- **未读或未做**：没有打开视频、音频和图片；没有看 `node_modules/`、`.venv/`、`tts_cache` 的内容；没有核对源 build；没有查证 NASA 资料。
- **缺失信息**：制作会话、用户需求、审片意见、发布状态、使用的模型和思考强度、ffmpeg 版本、实际响度。
