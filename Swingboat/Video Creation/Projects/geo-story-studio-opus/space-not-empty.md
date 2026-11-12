---
type: video-production-dossier
project: geo-story-studio-opus
video_id: space-not-empty
title: 太空里真的空无一物吗？
curriculum: script.json 未记录（README 归入“必修第一册 第一章第一节 地球的宇宙环境”）
captured: '2026-11-12'
status: 成片存在（竖屏 1080×1920，82.0 s）；技术检查/视觉检查/用户验收未记录
source_fingerprint: 6be7aa6a75a8ed2dc887984e3af9b0761ad77fd3096a8631e07f6c8d3788d053
tags:
- video-creation
- production-capture
- geography-teaching
- canvas-frame-render
- edge-tts
---

# 太空里真的空无一物吗？ · 小宇 + 旁白问答：方糖密度对比与星际介质（Canvas 逐帧 + 逐词时间戳）

<!-- capture-source-sha256:6be7aa6a75a8ed2dc887984e3af9b0761ad77fd3096a8631e07f6c8d3788d053 -->

本文是**根据项目文件做的材料回溯**，依据包括：源码、`script.json`、`build/timeline.json`、`out/` 下的成片与字幕，以及证据包里的 ffprobe 结果和 sha256 清单。证据包中 `host_session` 为空，**未发现制作会话**，所以制作对话、用户需求原文和修改过程都无法找回。本次没有运行任何项目脚本（没跑 `tts.py`、`render.mjs`、npm、pip），也没有打开视频或音频。

## 1. 当前结论与制作路线

**状态：成片存在**，即 `out/太空里真的空无一物吗.mp4` 和同名 `.srt`。目前只有证据包里的 ffprobe 媒体头信息；视觉检查、试听、响度测量、用户验收和是否发布都**未记录**。

| 项 | 值 | 来源 |
| --- | --- | --- |
| 分辨率 / 画幅 | 1080×1920，9:16 竖屏 | ffprobe；`lib.mjs:7` |
| 帧率 / 帧数 | 30 fps；`Math.round(82.029 × 30)` = 2461 帧 | ffprobe；按时长推算 |
| 时长 | 82.033 s（timeline 82.029 s） | ffprobe；`build/timeline.json` |
| 视频编码 | H.264，yuv420p，CRF 19，preset medium，`+faststart` | ffprobe；`render.mjs` ffmpeg 参数 |
| 音频编码 | AAC 48 kHz 立体声，192 kbps | ffprobe；`render.mjs` |
| 文件大小 | 17,986,569 字节（约 18.0 MB，平均约 1.75 Mbps） | 证据包 |
| 字幕 | `.srt` 41 条（2457 字节）；画面另有烧录字幕 | `timeline.json` cues；`render.mjs:64-89` |
| 其他版本 | 无 | `out/` 列表 |

文件时间：`script.json` 10-19 23:06，TTS 产物 23:07，`render.mjs` 23:11，成片 23:17。比 [[nebula-nursery]] 晚约 2 小时。git 里只有初始提交 `4831a07`（2026-10-23）。

一句话管线：

> `script.json`（21 段，旁白 + 小宇）→ `tts.py`：edge-tts 合成 + WordBoundary → `timeline.json` / `narration.wav` / SRT → `render.mjs` 按存在与否动态加载 `scenes-a/b/c.mjs`（7 个场景）+ 角色库 `cast.mjs` + 道具库 `props.mjs` → 程序音频 → ffmpeg 管道合成 MP4。

## 2. 教学目标与课标对齐

- **curriculum 字段：缺失**。`script.json` 的键只有 `title/source/rate/voices/segments`。README 把本片归到“必修第一册 第一章第一节 地球的宇宙环境”，简介是“星际物质：恒星之间飘着稀薄的气体和尘埃”。对应哪条课标**未记录**。
- **核心知识点**：恒星之间并非真空，有稀薄的气体（主要是氢和氦）和尘埃，合称“星际介质”；这些物质分布不均匀，在引力作用下聚成星云，有些星云里能诞生新恒星；星云本身也是星际介质的一部分。“看起来空旷 ≠ 空无一物”。
- **叙事结构**：
  - 导入：小宇问“星星之间黑漆漆的，是不是啥都没有？”
  - 原理：放大镜 → 气体 + 尘埃 → 命名；用方糖做密度对比；“少，也是有”。
  - 结论与迁移：分布不均 → 引力聚成星云 → 拉远看，星云也是星际介质的一部分。
  - 总结：`sum`、`end`，小宇说“原来太空，一点也不空！”
- **科学边界**：
  - 文案用的是“星际介质**最稀的地方**”，没有把它说成平均密度。
  - 标题副字是“在合适的致密区域”（`scenes-c.mjs` `cloudScene`）。
  - `neq` 句强调“非常稀薄 ≠ 完全没有”。
- **数字（没有 `numbers` 字段，出处没有逐条对应）**：
  - 方糖大小的地球空气里“大约一千亿亿个分子”，画面写“约 10¹⁹ 个分子”“= 一千亿亿个！”（`scenes-b.mjs:8-21, 76`）。
  - 星际介质最稀处“十块方糖那么大，才大约一个原子”。
  - 气体“主要是氢和氦”。
  - 这些数字都只能归到 `source` 字段里那几份笼统的资料。
- **术语**：文案、画面一律用“星际介质”，README 简介用的是“星际物质”。两者以哪个为准，需要对照人教版教材确认（**待核对**，AGENTS.md §6 要求术语以教材为准）。
- **based_on（推断）**：`source` 写的是“douyin-empty-space v002 分镜”，按命名规则，路径应为 `geo-story-studio/storage/builds/douyin-empty-space/v002/`。本次未核对。

## 3. 技术与依赖

| 类别 | 内容 | 证据 |
| --- | --- | --- |
| 渲染 | `@napi-rs/canvas` 声明 `^1.0.11`，lock 锁定 **1.0.11** | `package.json`；`package-lock.json` |
| npm scripts | 无 | `package.json` |
| TTS | Python `edge-tts` + `aiohttp`；无 requirements 文件 | `tts.py` |
| 字体 | `fonts.py`（fontTools）抽取圆体/苹方 | `fonts.py`（5 片相同，`a6ac7264e548`） |
| ffmpeg | TTS 混音 + 渲染编码/混音（同 [[nebula-nursery]]） | `tts.py:176-190`；`render.mjs:149-158` |
| 本地环境（非声明） | `.venv` 中有 `edge_tts-7.3.1`、`fonttools-4.67.0` 的 dist-info | 目录列表 |

**声线**（全局 `rate: 4`，叠加在各角色自己的 rate 上）：

| 角色 | voice | 实际 rate / pitch | 字幕胶囊 |
| --- | --- | --- | --- |
| 旁白 `zh` | zh-CN-XiaoxiaoNeural | +4% / +0 Hz | 无 |
| 小宇 `kid` | zh-CN-YunxiaNeural | +10% / +8 Hz | “小宇”（天蓝色），字幕文字为淡青色 `#c9f1ff` |

## 4. 镜头与时间轴

| 场景 id | 时间（约） | 旁白要点 | 画面 | 模块 |
| --- | --- | --- | --- | --- |
| `hook` | 0–4.4 s | 小宇：星星之间啥都没有？ | 两颗拟人星星夹着一段黑色空隙，宇航员小宇 | `scenes-a.mjs` |
| `lens` | 4.4–16.6 s | 放大镜；气体与尘埃；氢、氦；星际介质 | 放大镜镜片内裁切显示粒子，盖章“星际介质” | `scenes-a.mjs` |
| `density` | 16.6–32.5 s | 有多稀？方糖对比：空气约 10¹⁹ vs 十块方糖约一个原子 | 方糖 → 玻璃立方体塞满 260 个分子点；10 个立方体之间有 1 个原子在游走 | `scenes-b.mjs` |
| `thin` | 32.5–44.0 s | “孤单的原子”；少也是有；非常稀薄 ≠ 完全没有 | 拟人原子 `atomBuddy`，红色“≠”印章 | `scenes-b.mjs` |
| `cloud` | 44.0–60.1 s | 分布不均；引力聚成星云；新恒星诞生 | 左上角稀疏粒子；64 个粒子向右下方汇聚成星云，出现婴儿星 | `scenes-c.mjs` |
| `zoom` | 60.1–67.3 s | 拉远：星云和周围物质连成一片 | 镜头缩放到 0.4×，115 个远景粒子和 4 团远处星云 | `scenes-c.mjs` |
| `end` | 67.3–82.0 s | 看起来空旷 ≠ 空无一物；太空一点也不空 | 回到两颗星之间，总结卡片，小宇挥手，彩纸 | `scenes-c.mjs` |

## 5. 关键算法与实现

> **视频讲的科学**：星际介质的组成、密度对比，以及引力让物质聚集成云。**做视频用的算法**：等轴测投影、确定性粒子场、镜头缩放，以及为保护 logo 角落而做的粒子透明度遮罩。画面里的密度只是示意性的对比，粒子数量并不按比例。

### 5.1 动态加载场景模块（`render.mjs:14-15`）

```js
const MODS = await Promise.all(['a', 'b', 'c'].filter((k) => existsSync(here(`./scenes-${k}.mjs`)))
  .map((k) => import(`./scenes-${k}.mjs`)));
const SCENES = MODS.flatMap((m) => m.scenes);
```

[[nebula-nursery]] 是逐个静态 import（`scenesA/eventsA`…）。从本片开始，每个场景文件统一导出 `scenes` 和 `events`，`render.mjs` 自动收集，音效列表也用 `MODS.flatMap((m) => m.events)` 汇总。这样做符合 AGENTS.md §3“分文件、写一组验一组”的要求：哪个场景文件还没写，就自动跳过。

### 5.2 等轴测透明立方体 + 内容投影（`props.mjs:83-121`）

```js
export function cube(ctx, x, y, s, o = {}, inside) {
  const k = s * 0.8;
  const p = (u, v, w) => [x + (u - w) * k * 0.87, y + (u + w - 1) * k * 0.5 - (v - 0.5) * k + k * 0.0];
  ...
  poly([[0, 0, 0], [1, 0, 0], [1, 1, 0], [0, 1, 0]], tint);   // back faces
  ...
  if (inside) inside(p);
```

- 投影函数 `p(u,v,w)` 会传给回调。`densityScene` 在单位立方体坐标里放 260 个分子点（`CROWD`），点数按“挤”这个词的时间点逐渐增加，并带 23/19 Hz 的微抖动（`scenes-b.mjs:24, 52-60`）。
- 立方体先画后面的面，再画内容，最后画前面的棱，所以看起来像透明玻璃盒。

### 5.3 “十块方糖一个原子”的游走路径（`scenes-b.mjs:25-27, 80-96`）

10 个小立方体排成 2×5 的网格（`GRID`）。唯一的原子按 `PATH = [0,1,6,7,2,3,8,9,4]` 依次穿过各格。时间从“一个原子”开始，到 `kidLonely` 结束后 2 s 为止；`easeIO` 分段插值，外圈虚线环随时间旋转。用一个会动的原子，代替“0.1 个/立方厘米”这种不直观的写法。

### 5.4 引力聚集与拉远镜头（`scenes-c.mjs:10-100`）

- 64 个 `CLUMP` 粒子：起点在画面下部随机分布；左上角的稀疏区会被避开（`home[0] += 480`）。目标点在星云中心的椭圆内，半径按 `sqrt(rnd)` 取值，使分布均匀；每个粒子另有随机延迟。
- `zoomScene`：`z = lerp(1, 0.4, …)`，画布先 `translate/scale/translate` 再绘制 115 个远景粒子。`roof()` 按粒子在屏幕上的 y 坐标设透明度：<230 px 时为 0，<480 px 时为 0.3。代码注释写着“keep the logo corner clear and the title band calm (AGENTS.md §4)”（`scenes-c.mjs:84-88`）。

### 5.5 共用机制（与 [[nebula-nursery]] 相同）

- `lib.mjs` 与 [[nebula-nursery]] **完全相同**（sha256 `12567ee32a91`），提供 `S/E/at/word`、`talkLevel`、`pop/fadeIn/easeIO`。
- `audio.mjs` 在 5 片中完全相同：Karplus–Strong 配乐，15 种程序合成音效。
- `tts.py` 与 [[nebula-nursery]] 相比只改了 3 处：`TITLE`；`TARGET 80 → 85`；`TAIL 2.4 → 2.8`，并把注释改为“room for the closing brand card”。缓存键、429 重试、WSS_URL 替换、adelay/amix/loudnorm 都没变。

## 6. 声音与字幕

- 旁白：Xiaoxiao +4%；小宇：Yunxia +10% / +8 Hz。`LEAD_IN 1.0`，`GAP` 默认 0.45，每段另有 `gap`（0.15–0.6 s），`TAIL 2.8`。
- TTS 混音：`adelay` → `amix normalize=0` → `highpass=80` → `loudnorm=I=-16:TP=-1.5:LRA=11`（`tts.py:176-190`）。成片阶段加入配乐（112 BPM，音量 0.55，由旁白 sidechain 压低）、音效（音量 0.5），最后经 `alimiter 0.93`。
- 时间轴记录 `rate: 4`，说明没有因为 `TARGET = 85` 而提速（82.0 s 小于 85 s）。
- 缓存：`build/tts_cache/` 有 42 个文件，等于 21 段 × 2，没有残留。
- 字幕：段内断句按 `captions` 手动指定，起止时间取自逐词时间戳。小宇的台词左侧有“小宇”胶囊。从 `sum` 开始，页脚淡入“资料：NASA Hubble · NASA Space Place · NASA Science”（`render.mjs:91-98`）。
- 音效与台词对位示例：“一千亿亿”处为 `slam` + `sparkle`，“也是有”处为 `stamp` + `tada`，最后一句结束前 0.2 s 为 `confetti`（`scenes-b.mjs:158-172`、`scenes-c.mjs:145-161`）。

## 7. 迭代与问题

1. **改掉了“示意”声明**：相比 [[nebula-nursery]]，页脚不再显示“原创科普示意…”，改为在总结段淡入资料来源。注释明确写着“AGENTS.md §5: no ‘示意’ disclaimers on screen”（`render.mjs:91`）。这说明到 10-19 23:11 时 §5 已在使用（**推断**：规则就是在这两部片子之间加入的）。
2. **TAIL 注释和实现对不上**：`tts.py:35` 写“room for the closing brand card”，但 `render.mjs` 里没有 `outro()`，场景文件里也找不到任何 LOGO 或品牌卡代码。最后的 `end` 场景一直持续到 `DURATION`，结尾是“太空，一点也不空！”印章、彩纸，以及最后 0.6 s 的淡出黑场。现行 AGENTS.md §4 要求的片尾品牌卡（居中 logo + 标语）**没有实现**。
3. **目录名和源 build 名不一致**：`source` 写的是 `douyin-empty-space`。AGENTS.md §1 要求目录名沿用源 build 名（去掉前缀），照此应为 `empty-space/`，实际却是 `space-not-empty/`。这条规则是否早于本片，无法确认。
4. **`script.json` 缺少** `curriculum`、`based_on`、`numbers`（§6）。
5. **`TARGET = 85.0`**，与现行规则“默认 None”不一致（规则在 2026-10-26 的 `35ad4da` 中修改）。不过本片实际没有提速。
6. **术语**：用“星际介质”还是“星际物质”，见 §2，待核对。
7. **代码复用**：`cast.mjs` 注释写有“same visual language as nebula-nursery”和“from nebula-nursery”，`nebulaCloud` 的 `PUFFS/LANES` 由 [[nebula-nursery]] 移植而来。`props.mjs`（`4debdbd1b652`）首次出现在本片，之后被 [[sun-energy]]、[[sunlight-through-space]]、[[sun-angle]] 原样复用。
8. README 目录已收录（第 26 行，1:22）。

## 8. 复现入口

本次**未运行**。以下顺序来自 AGENTS.md 的常用命令和文件头注释：

```bash
python3 -m venv .venv && .venv/bin/pip install edge-tts   # fonts.py 另需 fonttools（未声明）
npm install                                               # @napi-rs/canvas 1.0.11
.venv/bin/python tts.py            # → build/seg*.mp3, narration.wav, timeline.json；out/太空里真的空无一物吗.srt
node render.mjs --still 25 62      # → build/still_<t>.png
node render.mjs                    # → build/music.wav, sfx.wav；out/太空里真的空无一物吗.mp4
```

字体依赖 macOS 系统字体资源（`fonts.py`）。缺字体时，`lib.mjs:11-13` 会自动调用 `fonts.py`。

## 9. 参考来源

`script.json` 和代码中**没有 URL**（证据包 `urls` 为空）。资料名来自 `source` 字段：

- NASA Hubble’s Nebulae：星云（项目文件中提及，本次未访问）
- NASA Space Place: What Is a Nebula?：星云和星际气体尘埃（项目文件中提及，本次未访问）
- NASA Science Heliophysics Big Idea 2.2：星际介质背景（项目文件中提及，本次未访问）
- NASA SVS Our Interstellar Medium：星际介质（项目文件中提及，本次未访问）
- 分镜来源：`geo-story-studio/storage/builds/douyin-empty-space/v002/`（路径为推断，本次未访问）

## 10. 证据与限制

| 文件 | sha256 前 12 位 | 说明 |
| --- | --- | --- |
| `script.json` | `a2d42e2e4c52` | 21 段 |
| `tts.py` | `4dcf16c01d58` | 与 [[nebula-nursery]] 差 3 行 |
| `render.mjs` | `474bb8ef3a5d` | 动态加载、页脚改为资料来源 |
| `lib.mjs` | `12567ee32a91` | 与 [[nebula-nursery]] 相同 |
| `cast.mjs` | `b21495554f76` | 小宇、星星、原子、星云 |
| `props.mjs` | `4debdbd1b652` | 标题、放大镜、立方体、≠、粒子场 |
| `scenes-a/b/c.mjs` | `a08e66ec85db` / `2622d5eed256` / `c6de09b8cbbd` | 7 个场景 |
| `audio.mjs` / `fonts.py` | `f9e04b2cfd54` / `a6ac7264e548` | 5 片相同 |

- **已读**：`script.json` 全文，`tts.py`（逐行 diff），`render.mjs`（逐行 diff），`props.mjs` 与各场景文件的结构和关键段落，`timeline.json` 中的段落时间。
- **未读、未做**：媒体文件、`node_modules/`、`.venv/`、`tts_cache` 内容；没有核对源 build 和 NASA 资料；没有核验数字（10¹⁹、“十块方糖一个原子”）。
- **缺失**：制作会话、需求原文、审片记录、发布状态、模型信息、ffmpeg 版本、实测响度。
