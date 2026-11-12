---
type: video-production-dossier
project: geo-story-studio-opus
video_id: air-composition
title: 一口空气的配料表：除了氧气，还有什么？
curriculum: 人教版必修第一册 第二章第一节 大气的组成和垂直分层（大气的组成：干洁空气、水汽、杂质）；课标1.4
captured: '2026-11-12'
status: 成片存在（竖屏 1080×1920，157.5 s）；技术检查/视觉检查/用户验收未记录
source_fingerprint: e64521a446d8d52a76f70a1cbcaeecf3f95cc3780b3cc561caf233081c81a460
tags:
- video-creation
- production-capture
- geography-teaching
- canvas-frame-render
- edge-tts
---

# 一口空气的配料表 · 「零食袋配料表」拟人气体 + 100 格体积方阵（Canvas 逐帧 + 多角色 edge-tts）

<!-- capture-source-sha256:e64521a446d8d52a76f70a1cbcaeecf3f95cc3780b3cc561caf233081c81a460 -->

本次为材料回溯与归档，结论来自项目文件（`script.json`、`tts.py`、`render.mjs`、`lib.mjs`、`audio.mjs`、`chars.mjs`、`props.mjs`、`scenes-a…e.mjs`、`build/timeline.json`、`out/` 目录清单与 ffprobe 结果）。按时间戳**推断**对应的宿主会话为 `c55dde0a-43c6-4e67-b3b2-0c7ce89d18f4`（创建于 2026-10-31 16:00，早于 `script.json` 修改时间 16:03），只按元数据找到，**对话内容无法读取**（宿主返回空 transcript），因此制作过程中的需求原文、预览与修改记录均未找回。本轮未运行任何项目脚本，未打开视频或音频。

## 1. 当前结论与制作路线

**状态：成片存在**——`out/一口空气的配料表.mp4` 与同名 `.srt`。成片只有 ffprobe 媒体头信息，**视觉检查、试听、用户验收均未记录**。

| 项 | 值 | 来源 |
| --- | --- | --- |
| 分辨率 / 画幅 | 1080×1920，9:16 竖屏 | ffprobe；`lib.mjs:7` `W = 1080, H = 1920, FPS = 30` |
| 帧率 / 帧数 | 30 fps；约 4725 帧 | ffprobe；按 `render.mjs:217` `Math.round(DURATION * FPS)` 与 `timeline.json` 时长 157.509 s 计算 |
| 时长 | 157.500 s（README 记 2:38） | ffprobe；`build/timeline.json` `duration: 157.509` |
| 视频编码 | H.264（libx264，CRF 19，preset medium，yuv420p，faststart） | ffprobe 编解码名；参数取自 `render.mjs:214-215` |
| 音频编码 | AAC，48 kHz 立体声，192 kbps | ffprobe；`render.mjs:215` |
| 文件大小 | 31,327,103 字节（约 31.3 MB，平均约 1.59 Mbps） | 证据包 / ffprobe |
| 字幕 | `out/一口空气的配料表.srt`，70 条；画面同时烧录字幕 | 统计 SRT `-->` 行；`render.mjs:83-108` |
| 其他版本 | 无（没有 `--rec` 推荐版、封面或发布文案） | `out/` 目录清单 |

一句话管线：`script.json`（26 段，5 个声音角色）→ `tts.py` 用 edge-tts 逐句合成并取 WordBoundary 逐词时间 → `build/narration.wav` + `build/timeline.json` + `out/*.srt` → `render.mjs` 动态加载 5 个场景文件（19 个场景），以词时间戳驱动动画，在 Canvas 上逐帧绘制并把 RGBA 原始帧经管道送入 ffmpeg → `audio.mjs` 程序合成配乐与音效 → ffmpeg 混音（配乐对旁白做侧链压缩）→ MP4。文件修改时间显示：`script.json` 16:03 → TTS 产物 16:04 → `render.mjs` 16:08 → `scenes-a…e` 16:10–16:18 → 成片 16:21（同日 2026-10-31）。

## 2. 教学目标与课标对齐

- **课标出处**（`script.json:3`）：人教版必修第一册 第二章第一节 大气的组成和垂直分层（大气的组成：干洁空气、水汽、杂质）；课标 1.4「运用示意图等，说明大气的组成和垂直分层，及其与生产和生活的联系」。
- **核心知识点**：低层大气由干洁空气、水汽和杂质组成；干洁空气按体积氮约 78%、氧约 21%、其余约 1%（以氩为主）；各成分作用；水汽与杂质是成云致雨的必要条件。
- **讲解结构**（按 `timeline.json` 段落）：
  - 导入：小宇「深吸一口气——全是氧气吧？」→「错！」给这口空气印配料表（`hook`/`no`）。
  - 原理：先定口径「去掉水汽和杂质、按体积」（`rule`）→ 氮气、氧气拟人自报比例（`n2`/`o2`/`n2role`）→ 78+21=99 抢答，主力是氩（`sum`/`quiz`/`ar`）→ CO₂ 与臭氧的作用（`rest`/`co2`/`o3`）→ 水汽登场，含量因时因地而异、计入后比例口径改变（`h2o`…`h2orole`）→ 杂质/气溶胶、凝结核（`dust`…`nuclei`）。
  - 结论：「必要条件，不是有了就一定下雨」（`must`）。
  - 迁移：人工增雨（先有合适的云再播撒碘化银）、燃烧煤炭石油使 CO₂ 增多（`seed`/`human`）。
  - 总结与考点：一句话总结 + 易错点「那约 1% 不是水汽，也不只是 CO₂，主力是氩」（`recap`/`exam`/`end`）。
- **科学口径边界**：`based_on` 指向 `geo-story-studio/storage/builds/douyin-air-composition/v002`，并附口径说明——近地面干洁空气按体积 78/21/1；约 1% 以氩为主，不是水汽、颗粒或只有 CO₂；水汽计入后比例口径改变；气溶胶有自然和人为来源；只在适宜条件下部分颗粒成为凝结核，不演必然降雨；不用实时 CO₂ 数值和固定臭氧高度。代码中可见对应处理：臭氧场景注释「heights not to scale, so no numbers」，平流层带不标高度（`scenes-b.mjs:144`）；紫外线箭头 5 支中有 1 支半透明穿过（`scenes-b.mjs:166-176`，`const leak = i === 4`）；水汽份额在条形图中持续摆动、比例显示为「？%」（`scenes-c.mjs:144-147`）。
- **数字与出处**（`script.json:14-17`）：
  - 「氮约 78%、氧约 21%、其余约 1%」——整数近似，注明 NOAA 给出氮 78.084%、氧 20.946%、氩 0.934%；出处为教材图 2.2、NOAA JetStream《The Atmosphere》、NASA 文章。
  - 「氩约 0.93%」——其余约 1% 中占比最大的气体。
- **地理素养**：迁移段落涉及人地协调（人类活动改写配料表）和综合思维（必要条件 ≠ 充分条件）；这是对文案的归纳，代码或文件中没有显式标注。

## 3. 技术与依赖

| 类别 | 内容 | 证据 |
| --- | --- | --- |
| 渲染 | Node.js ESM（`"type": "module"`）+ `@napi-rs/canvas`，声明 `^1.0.11`，锁定 **1.1.0**（`@napi-rs/canvas-darwin-arm64` 1.1.0） | `package.json`；`package-lock.json`（lockfileVersion 3） |
| npm scripts | 只有 npm 默认的 `test` 占位脚本，没有渲染命令 | `package.json` |
| TTS | Python `edge-tts` + `aiohttp`（`tts.py:16-18` import）；版本未记录（未读取 `.venv`） | `tts.py` |
| 字体 | `fonts.py` 用 `fontTools.ttLib.TTCollection` 从 macOS 系统字体资产中抽出圆体-简粗体、苹方-简中粗体到 `build/fonts/`；`lib.mjs:11-13` 缺字体时自动调用 `.venv/bin/python fonts.py` | `fonts.py`；`lib.mjs` |
| 编码 / 混音 | 系统 ffmpeg：libx264、aac、`adelay`、`amix`、`highpass`、`loudnorm`、`sidechaincompress`、`alimiter` | `tts.py:177-192`；`render.mjs:205-216`；版本未记录 |
| 平台 | macOS（系统字体路径、darwin-arm64 原生包） | `lib.mjs:17-19`；lock 文件 |

声音角色（`script.json:7-13`；`script.json:6` 另有全局 `"rate": 4`，`tts.py:94` 把它加到每个角色的 rate 上）：

| 角色 key | 标签 | 声音 | rate（角色 + 全局） | pitch |
| --- | --- | --- | --- | --- |
| `zh` | 旁白 | zh-CN-XiaoxiaoNeural | 0 + 4 = +4% | 0 |
| `kid` | 小宇 | zh-CN-YunxiNeural | 6 + 4 = +10% | +2 Hz |
| `n2` | 氮气 | zh-CN-YunjianNeural | 0 + 4 = +4% | −6 Hz |
| `o2` | 氧气 | zh-CN-XiaoyiNeural | 6 + 4 = +10% | +2 Hz |
| `h2o` | 水汽 | zh-CN-YunxiaNeural | 10 + 4 = +14% | +4 Hz |

`build/tts_cache/` 有 52 个文件，正好对应 26 段 × (mp3 + json)，即只缓存了一批语速；`timeline.json` 记录 `rate: 4`。`TARGET = None`（`tts.py:33`），所以 +4% 是 `script.json` 预设值，不是时长上限触发的自动提速；为何预设 +4% **未记录**。

## 4. 镜头与时间轴

时间取自 `build/timeline.json`（段落 start/end，秒）；场景边界由各场景文件 `scenes` 数组按段落起点 `S(id)` 或词时间 `at(id, 词)` 再减 0.15–0.3 s 定义，相邻场景交叉淡化 `XF = 0.3`（`render.mjs:22`）。

| # | 场景（draw 函数） | 约时间 (s) | 旁白要点 | 画面 | 模块 |
| --- | --- | --- | --- | --- | --- |
| 1 | `hook` | 0–4.1 | 小宇：全是氧气吧？ | 小宇深吸气，混色气体微粒卷入口中 | `scenes-a.mjs:7-38` |
| 2 | `bagScene` | 4.1–16.5 | 错！配料表；定规矩 | 「空气」零食袋翻到背面配料表，划掉水汽与杂质 → 干洁空气 | `scenes-a.mjs:40-83` |
| 3 | `gridScene` | 16.5–32.7 | 氮气 78%、氧气 21%、各自作用 | 10×10 体积方阵逐格弹出；N₂ 老大戴皇冠、O₂ 弹入；角色卡 | `scenes-a.mjs:85-117`；`props.mjs:93` |
| 4 | `oneScene` | 32.7–约 43 | 78+21=99，抢答 | 加法 + 三选项抢答（水汽 / 二氧化碳 / 氩气） | `scenes-b.mjs:7-60` |
| 5 | `argonScene` | 约 43–51.7 | 氩气是稀有气体；CO₂、O₃ 量少作用大 | 放大镜里的 1%，氩气打瞌睡（Zzz） | `scenes-b.mjs:62-99` |
| 6 | `co2Scene` | 51.7–58.0 | CO₂ 光合作用原料、保温 | 进入叶片 → 地面上的保温毯 | `scenes-b.mjs:101-139` |
| 7 | `o3Scene` | 58.0–63.8 | 臭氧吸收紫外线、保护伞 | 地球边缘 + 平流层带，紫外线波形箭头被吸收 | `scenes-b.mjs:141-189` |
| 8 | `h2oScene` | 63.8–67.8 | 水汽：配料表上怎么没有我 | 水汽精灵闯入 | `scenes-c.mjs:13-33` |
| 9 | `varyScene` | 67.8–75.7 | 因时因地而异 | 雨林 vs 沙漠、晴天 vs 雨天 | `scenes-c.mjs:35-124` |
| 10 | `ratioScene` | 75.7–80.4 | 计入水汽不能照搬比例 | 两条比例条，第二条水汽份额摆动、百分比变「？%」，盖章「比例不能照搬」 | `scenes-c.mjs:126-153` |
| 11 | `roleScene` | 80.4–85.6 | 云雨雪、吸收地面长波辐射 | 水汽 → 云、雨、雪；长波箭头 | `scenes-c.mjs:155-200` |
| 12 | `dustScene` | 85.6–99.0 | 杂质 / 气溶胶及来源 | 放大镜下固体颗粒与小液滴；沙尘、海盐、烟粒 | `scenes-d.mjs:9-65` |
| 13 | `nucleiScene` | 99.0–110.4 | 小颗粒有用吗；凝结核 | 水汽附着在部分颗粒上凝成小水滴 | `scenes-d.mjs:67-129` |
| 14 | `mustScene` | 110.4–119.1 | 必要条件 ≠ 一定下雨 | 条件对比 | `scenes-d.mjs:131-160` |
| 15 | `seedScene` | 119.1–128.4 | 人工增雨 | 先有合适的云，再播撒催化剂，水滴冰晶长大 | `scenes-e.mjs:7-67` |
| 16 | `humanScene` | 128.4–136.4 | 燃烧煤炭石油 CO₂ 增多 | 人类活动改写配料表 | `scenes-e.mjs:69-118` |
| 17 | `recapScene` | 136.4–144.9 | 一句话总结 | 领奖台：氮第一、氧第二 | `scenes-e.mjs:120-155` |
| 18 | `examScene` | 144.9–151.3 | 考试易错点 | 易错卡 | `scenes-e.mjs:157-183` |
| 19 | `endScene` | 151.3–155.2 | 小宇：原来藏着一整张配料表 | 全部成分绕着零食袋谢幕 | `scenes-e.mjs:185-209` |
| — | 品牌卡（`endCard`） | 154.5–157.5 | 无旁白 | 居中大 logo + 标语「一口空气，一张配料表 / 干洁空气 · 水汽 · 杂质」 | `render.mjs:119-140` |

## 5. 关键算法与实现

> 区分：视频**讲解的地理知识**是大气组成；本节是**制作所用的算法**（TTS 时间轴、Canvas 动画、程序音频），不是大气成分的物理模拟。

### 5.1 WordBoundary 逐词时间轴 → 动画与字幕锚点

`tts.py:70-86` 用 `boundary="WordBoundary"` 收集每个词的 `offset/duration`（单位 100 ns，`TICKS = 1e7`），并在原文中定位 `idx`；`lib.mjs:36-46` 再按文字查回时间：

```js
export function word(id, w, nth = 0) {
  const s = SEG[id];
  let i = -1;
  for (let k = 0; k <= nth; k++) i = s.text.indexOf(w, i + 1);
  if (i < 0) throw new Error(`"${w}" not in ${id}`);
  const hit = s.words.find((x) => x.idx <= i && i < x.idx + x.text.length) ?? s.words.find((x) => x.idx >= i);
  const last = [...s.words].reverse().find((x) => x.idx < i + w.length) ?? hit;
  return { start: hit.start, end: last.end };
}
export const at = (id, w, nth) => word(id, w, nth).start;
```

所有弹出与音效都写成 `at('n2', '78%')`、`at('o3', '保护伞')` 这类词锚点；找不到词时直接抛错，避免静默错位。字幕条 `caption_cues()`（`tts.py:123-137`）也用同一批词时间确定起止。段落间距以末词结束 +0.12 s 计，不用 mp3 长度（`tts.py:151-152` 注释：mp3 尾部约 0.6 s 静音）。

### 5.2 TTS 缓存、429 重试与端点替换

- 缓存键：`sha1(f"{voice}|{rate}|{pitch}|{text}")[:16]`，mp3 与词时间 json 成对存放于 `build/tts_cache/`（`tts.py:47-51`）。
- 429 重试：最多 5 次，等待 8/16/24/32 s（`tts.py:53-63`）。
- 端点替换：`edge_tts.communicate.WSS_URL` 改为 `api.msedgeservices.com`（`tts.py:20-26`），符合 AGENTS.md §2。

### 5.3 100 格体积方阵（`props.mjs:92-127`）

```js
for (let i = 0; i < 100; i++) {
  const cx = x0 + (i % 10) * (cell + gap), cy = y0 + Math.floor(i / 10) * (cell + gap);
  let col, t0;
  if (i < 78) { col = C.n2; t0 = tN + i * 0.013; }
  else if (i < 99) { col = C.o2; t0 = tO + (i - 78) * 0.03; }
  else { col = C.ar; t0 = tR; }
  ...
  const p = pop(t, t0, 0.3);
```

每格的弹出时刻 = 说到「78%」「21%」的词时间 + 序号 × 13/30 ms，形成「一格一格占地盘」的波浪；第 100 格留给「其余约 1%」，可单独加脉动高亮环（`hiLast`）。`pop()` 是带回弹的 `easeBack`（`lib.mjs:55-59`，c1 = 2.2）。

### 5.4 场景切换、震屏与品牌层（`render.mjs`）

- 场景按 `from` 排序后逐帧遍历；进入 `easeOut(prog(t, from, from+XF))`，离开线性淡出，同时缩放 `lerp(0.95,1,enter)·lerp(1.04,1,leave)` 制造轻微推拉（`render.mjs:159-178`）。
- 音效事件可带 `shake`，`shakeAt()` 用二次衰减的正余弦偏移整幅画面 0.4 s（`render.mjs:146-157`）；盖章类事件 shake 10–16（如 `scenes-a.mjs:129`）。
- 角标 logo：`x=64, y=76, w=290`，取 `LOGO_BOX` 有效区域，背后画半径 250 的径向暗晕，0.3–1.0 s 淡入，品牌卡出现前 0.3 s 淡出（`render.mjs:64-78`）。
- 品牌卡：`CARD_T = E('end') + 0.3`，重绘背景压住全部画面，居中 760 px logo 由 0.9 放大到 1，下面金色标语；最后 0.6 s 整体淡出到黑（`render.mjs:119-140, 183-187`）。
- 资料页脚从 `recap` 起淡入「资料：人教版地理必修第一册 · NOAA · NASA」，替代「示意」类声明（`render.mjs:110-117`）。

### 5.5 「比例不能照搬」的动态条形图（`scenes-c.mjs:126-153`）

第二条比例条加入水汽份额 `vw = ip·(2.5 + 1.8·sin(2.2t))`，宽度持续摆动，氮、氧标签在水汽出现 0.6 s 后变成「？%」——用画面表达「水汽含量不固定，计入后口径就变了」，而不是给出一个湿空气百分比。

## 6. 声音与字幕

| 项 | 内容 | 来源 |
| --- | --- | --- |
| 声音角色 | 旁白、小宇、氮气、氧气、水汽 5 个声音（见 §3 表）；本片没有使用 `fx` 滤镜 | `script.json:7-13` |
| 旁白合成 | 逐句 `aresample=48000` → `adelay` 定位 → `amix normalize=0` → `highpass=f=80` → `loudnorm=I=-16:TP=-1.5:LRA=11` → `apad,atrim` 到总长 | `tts.py:177-192` |
| 时间参数 | `LEAD_IN = 0.6`（注释「Douyin hook: speak almost immediately」）、默认 `GAP = 0.45`（各段在 `script.json` 中多设 0.3–0.6）、`TAIL = 3.2`（品牌卡留白） | `tts.py:33-36` |
| 配乐 | `audio.mjs` 程序合成：112 BPM，C–G–Am–F 循环，Karplus–Strong 拨弦琶音 + 低音 + 沙锤噪声 + 隔小节钟琴；随机种子 42；淡入 1.2 s、淡出 2.5 s | `audio.mjs:174-210` |
| 音效 | 20 种程序合成音效（pop、note、ding、sparkle、whoosh、boing、slam、stamp、tick、scribble、buzz、alarm、tada、confetti、wind、fizz、boom、roar、thud、sizzle），由各场景 `events` 按词时间触发；如「78加21」配三声上行音符 | `audio.mjs:78-165`；`scenes-b.mjs:199-201` |
| 总混音 | 配乐 ×0.75，经旁白侧链压缩（threshold 0.05、ratio 5、attack 20 ms、release 400 ms）；音效 ×0.5；三路 `amix` 后 `alimiter=limit=0.93` | `render.mjs:209-212` |
| SRT | 70 条；分句在 `script.json` 的 `captions` 或标点自动切分（≤14 字） | `tts.py:107-137`；`out/*.srt` |
| 烧录字幕 | `SUB_Y = 1500`，胶囊底板，字号从 58 起按宽度递减；角色台词前有彩色说话人标签（小宇/氮气/氧气/水汽） | `render.mjs:23, 81-108` |

响度与听感只能从代码参数推知，本轮未试听、未测量。

## 7. 迭代与问题

| 事项 | 证据 | 说明 |
| --- | --- | --- |
| 渲染脚本注释的输出名不对 | `render.mjs:2` 写 `-> out/宜居星球面试会.mp4`，实际 `TITLE = '一口空气的配料表'`（`render.mjs:13`） | 注释是从其他视频模板复制后未改（**推断**），不影响输出文件名 |
| TTS 说明残留其他视频内容 | `tts.py:4` docstring 举例「Neptune's far-away echo」 | 本片没有任何声音使用 `fx`；同样是模板残留（**推断**） |
| `based_on` 写成本机绝对路径 | `script.json:4` | 本档案改写为 `geo-story-studio/storage/builds/douyin-air-composition/v002`；源 build 本次未读取 |
| 全局语速 +4% | `script.json:6`，`TARGET = None` | 与 AGENTS.md §2「不为压时长调快语速」不冲突（没有时长上限），但预设原因未记录 |
| 规则符合情况（代码中存在） | 角标 logo（`render.mjs:66-78`）、片尾品牌卡（`render.mjs:120-140`）、资料页脚、无「示意」声明（全目录检索「示意/不按比例/非观测」只在注释中出现） | 只说明代码中存在，画面效果未做视觉核对 |
| 片尾留白 | `TAIL = 3.2` 与品牌卡 `CARD_T = E('end') + 0.3` | 品牌卡实际约 3.0 s，在 AGENTS.md §4 建议的 2.5–3 s 区间上沿 |
| 预览帧 | `build/` 无 `still_*.png` | 是否做过 `--still` 拼图检查无法确认（可能已按规则删除） |

README 目录（`README.md:56`）已收录本片，时长记 2:38，与 ffprobe 157.5 s 一致。

## 8. 复现入口

以下命令取自 AGENTS.md「常用命令」、`tts.py:5` 与 `render.mjs:2-3`，**本次均未执行**：

```bash
cd air-composition
npm install                                   # @napi-rs/canvas（lock 1.1.0）
python3 -m venv .venv && .venv/bin/pip install edge-tts   # fonts.py 另需 fontTools（未在文件中声明安装方式）
.venv/bin/python tts.py                       # → build/narration.wav、build/timeline.json、out/一口空气的配料表.srt
node render.mjs --still 8.0 14.2              # 预览帧 → build/still_<t>.png
node render.mjs                               # → build/music.wav、build/sfx.wav、out/一口空气的配料表.mp4
```

- 必须先跑 `tts.py`：`lib.mjs:29` 在加载时读取 `build/timeline.json`。
- `build/tts_cache/` 已有全部 26 段缓存，在文案、声音、语速不变时 `tts.py` 不会再请求网络（**推断**，未验证）。
- 修改入口：文案与角色 → `script.json`；留白 → `tts.py:33-36`；场景 → `scenes-*.mjs`；配乐 → `audio.mjs` `buildMusic`；品牌卡标语 → `render.mjs:137-138`。

## 9. 参考来源

| 来源 | 用途 | 状态 |
| --- | --- | --- |
| 人教版《普通高中教科书 地理 必修第一册》第二章第一节 | 低层大气组成、各成分作用、成云致雨必要条件 | `script.json:19`，无 URL |
| <https://www.noaa.gov/jetstream/atmosphere> | 干、湿两种口径的气体组成；水汽含量可变 | 项目文件中提及，本次未访问 |
| <https://www.noaa.gov/jetstream/clouds/how-clouds-form> | 小颗粒可作凝结核，但还需饱和条件 | 项目文件中提及，本次未访问 |
| <https://atmosphere-imager.gsfc.nasa.gov/faqs/aerosol> | 气溶胶定义与自然、人为来源 | 项目文件中提及，本次未访问 |
| <https://scied.ucar.edu/learning-zone/air-quality/whats-in-the-air> | 干空气以氮氧氩为主；平流层臭氧吸收紫外线 | 项目文件中提及，本次未访问 |
| <https://www.cma.gov.cn/>（中国气象局人工影响天气科普） | 人工增雨需合适云层再播撒碘化银 | 项目文件中提及，本次未访问 |
| NASA《The Atmosphere: Getting a Handle on Carbon Dioxide》 | 78/21/1 与氩的比例 | `script.json:15-16` 仅列标题，无 URL |
| `geo-story-studio/storage/builds/douyin-air-composition/v002` | 改编来源与科学口径边界 | `script.json:4`；本次未读取 |

## 10. 证据与限制

| 文件 | 行数 | sha256[:12] | 读取程度 |
| --- | --- | --- | --- |
| `air-composition/script.json` | 55 | `86b23948d04f` | 全文 |
| `air-composition/tts.py` | 210 | `ffd262bfb539` | 全文 |
| `air-composition/render.mjs` | 228 | `8baeb0de3b14` | 全文 |
| `air-composition/lib.mjs` | 222 | `d77bdf12386b` | 1-80 行、导出表、190-221 行 |
| `air-composition/audio.mjs` | 229 | `9e3eeae951b5` | 结构、拨弦、配乐、音效名 |
| `air-composition/props.mjs` | 179 | `7792779bec41` | 导出表、`grid100` 全文 |
| `air-composition/chars.mjs` | 251 | `7e987d365f93` | 导出表与注释 |
| `air-composition/scenes-a…e.mjs` | 144/220/225/186/242 | `bea253fdafe0` / `d39048c07f7e` / `1a176134ab7d` / `57e141cb2290` / `f06c6b8ffac1` | 各文件头注释、`scenes`/`events` 表；`gridScene`、`o3Scene`、`ratioScene` 全文 |
| `air-composition/fonts.py` | 37 | `a6ac7264e548` | 前 20 行 |
| `air-composition/package.json` / `package-lock.json` | 17 / — | `33bf87c1dee9` / `0a239b4762fe` | 全文 / 包版本 |
| `air-composition/build/timeline.json` | — | — | 顶层字段、各段时间与文字、cue 数 |
| `air-composition/out/一口空气的配料表.mp4` | — | 31,327,103 字节 | 仅证据包中的 ffprobe 结果 |
| `air-composition/out/一口空气的配料表.srt` | 70 条 | — | 只统计条数 |

限制：

- 宿主会话对话不可读；需求原文、模型与思考强度、预览轮次、用户反馈**未记录**。
- 未运行任何脚本、未打开视频与音频；画面、配音和响度没有核对。
- Python 包、Node、ffmpeg 版本未记录；源 build（`geo-story-studio`）未读取，口径说明只来自 `script.json`。
- git 只有一次批量提交 `8cbe673 2026-11-01 feat(videos): add 21 high-school geography lessons`，没有本片的单独迭代历史。
