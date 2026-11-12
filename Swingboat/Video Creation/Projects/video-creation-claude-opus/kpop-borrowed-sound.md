---
type: video-production-dossier
project: video-creation-claude-opus
video_id: kpop-borrowed-sound
captured: '2026-11-12'
status: 已有 200.9 秒 1080p30 成片 MP4 + SRT；代码逐帧绘制 + Edge TTS + 程序合成音乐，1992 电视画面用 5 张 AI 生成静帧
source_fingerprint: 229c793d751952709fea45900177d4c21505baa0d9fec210523e32f0108c055e
tags:
- video-creation
- production-capture
- canvas-frame-render
- tts
- explainer
---

# 借来的声音：K-pop 三十年 · Canvas 逐帧绘制 + 词级对时 TTS + 程序合成年代配乐

<!-- capture-source-sha256:229c793d751952709fea45900177d4c21505baa0d9fec210523e32f0108c055e -->

本档案根据项目文件回溯整理。**未找回制作对话，结论来自项目文件**（源码、`script.json`、TTS 缓存、成片媒体头、文件时间戳）。本轮没有运行项目中的任何脚本，没有重新渲染，也没有播放成片做视听检查；只用 `ffprobe` 读取了成片媒体头。项目 git 只有两次提交（整体初次提交 + `.gitignore` 修正），无法从 git 历史还原迭代过程。

## 1. 当前结论与制作路线

**状态：已有成片 `out/借来的声音-Kpop三十年.mp4` 和字幕 `out/借来的声音-Kpop三十年.srt`。** 证据等级：
- 「最终产物存在」：MP4 和 SRT 都在 `out/`，MP4 已用 ffprobe 读取媒体头。
- 「技术检查通过」：仅限 ffprobe 媒体头。MP4 时长与 TTS 缓存重算的时间轴总长一致（见下表）。
- 「视觉检查通过」「用户验收」：**未记录**。项目中没有截图、检查报告或验收记录。

| 规格 | 值 | 来源 |
| --- | --- | --- |
| 分辨率 / 画幅 | 1920×1080，16:9 | ffprobe；`lib.mjs:7` `W = 1920, H = 1080` |
| 帧率 / 帧数 | 30 fps，6026 帧 | ffprobe；`lib.mjs:7` `FPS = 30` |
| 时长 | 200.867 s（约 3 分 21 秒） | ffprobe `duration=200.866667`；按 `tts.py` 算法用 TTS 缓存重算，总长同为 200.867 s |
| 视频编码 | H.264 High，yuv420p，约 9.1 Mbps | ffprobe；`render.mjs:140` `libx264 -preset medium -crf 18` |
| 音频 | AAC-LC，48 kHz 立体声，约 191 kbps | ffprobe；`render.mjs:141` `-b:a 192k` |
| 文件大小 | 234,538,101 字节 | fingerprints.json 的 media 项 |
| 字幕 | 91 条 SRT 字幕，中文 | `out/借来的声音-Kpop三十年.srt` |

**一句话管线**：

```text
script.json（34 段旁白 + 9 个章节 + 字幕断句）
 → tts.py：Edge TTS（晓晓）逐段合成，取词边界 → build/timeline.json + narration.wav + SRT
 → scenes-a…h.mjs：每个镜头是 draw(ctx,t)，入出点和动画都锚定旁白里的词
 → render.mjs：@napi-rs/canvas 逐帧绘制 → RGBA 原始帧通过管道送进 ffmpeg
 → audio.mjs：按年代程序合成 8 种节拍 + 音效（不用采样）
 → ffmpeg：配乐按人声侧链压缩，三轨混音 + 限幅 → H.264/AAC MP4
```

制作路线按标准阶段对应如下：

| 阶段 | 输入 → 处理 → 输出 | 入口 |
| --- | --- | --- |
| 需求 | 原始需求文字未保存（项目里没有 `prompt.md`）；`script.json.inspiration` 写明灵感来源和借鉴边界 | `script.json:3` |
| 分镜 | 旁白分段、章节标记、手工字幕断句都写在同一个 JSON 里 | `script.json` |
| 素材 | 画面全部由代码绘制（人物剪影、磁带、CRT 电视、点阵地球仪等）；唯一的外部位图是 `assets/stage-1992*.png` 5 张静帧 | `props.mjs`、`people.mjs`、`vhs.mjs` |
| 动作 | 关键帧时刻由 `at(段id, 词)` 从 TTS 词边界查出 | `lib.mjs:44-55` |
| 声音 | TTS 旁白 + 程序合成音乐 + 程序合成音效 | `tts.py`、`audio.mjs` |
| 剪辑 | 场景交叉淡化 0.25 s / 硬切，章节切换叠加漏光、磁带故障效果 | `render.mjs:78-114` |
| 导出 | ffmpeg 一次完成编码和混音 | `render.mjs:131-142` |

## 2. 创作目标与需求

原始提示词**未保存**：项目里没有 `prompt.md`、`plan/` 或 README。下面的内容从项目文件中摘录或归纳得到。

- **灵感与边界**（摘录 `script.json:3`）：
  > "https://x.com/ameliax111/status/2110756490575589711 — Manus 生成的《K-pop 声音演变》短片 (2026-10-15)，本片只借用其叙事结构与画面风格，画面与音乐均为代码原创。"
- **片尾署名**（摘录 `scenes-h.mjs:146-147` 中绘制在画面上的文字）：
  > 旁白  微软 Edge 神经语音 · 晓晓 ／ 画面与音乐  代码生成；1992 电视画面为 AI 生成的示意图（ChatGPT） ／ 灵感  @ameliax111 分享的 Manus K-pop 短片
- **叙事结构**（据 `script.json` 归纳）：用「一盘磁带」做比喻。A 面开场 → TRACK 01–06 依次讲 1992、1996、2007、2012、2020、现在 → BONUS 总结方法 → B 面结尾。核心论点是「K-pop 的秘密不是某种曲风，而是一套方法：拿来、拆开、重新组装」。
- **时长目标**：`tts.py:30` 的 `TARGET = 215.0` 是总长上限（秒）。超出时会自动加快语速。实际总长 200.87 s，没有触发加速（见第 6 节）。
- **视觉风格**（据 `lib.mjs:90-101` 归纳）：每个年代一套配色（背景/文字/强调色/卡片色），配合颗粒、暗角、漏光和磁带故障效果，做成「纪录片 + 编辑排版」风格；字幕放在底部居中，字号较小。
- **人物形象原则**：`people.mjs:1` 注释写明 "Stylised performer silhouettes (no real likenesses)"，人物都是几何剪影。唯一的例外是 1992 电视画面使用了照片级 AI 静帧（见第 3 节「图片素材来源与授权」）。

## 3. 技术与依赖

| 项目 | 版本 / 参数 | 证据 |
| --- | --- | --- |
| Node 画布 | `@napi-rs/canvas` 声明 `^1.0.11`，锁定 `1.0.11` | `package.json`、`package-lock.json:14-15` |
| Node.js 版本 | 未记录 | — |
| Python TTS | `edge_tts`、`aiohttp`（`tts.py` import）；**版本未记录**（没有 requirements 或锁文件，`.venv/` 已不存在） | `tts.py:14-16` |
| 字体抽取 | `fontTools.ttLib.TTCollection`；版本未记录 | `fonts.py:9` |
| 编码 / 混音 | 系统 `ffmpeg`；版本未记录 | `render.mjs:131`、`tts.py:181` |
| TTS 声线 | `zh-CN-XiaoxiaoNeural`，语速 +0%、音调 +0Hz | `script.json` `voices`；重算缓存键命中 `rate=0` |
| Edge TTS 端点修补 | 把 `WSS_URL` 改为 `api.msedgeservices.com`。注释说明 2026 年 10 月起旧端点合成返回 403（rany2/edge-tts#490、#491） | `tts.py:18-24` |
| 中文字体 | 宋体 SC Black/Bold（标题）、苹方 SC Semibold/Regular（字幕/正文），从 macOS 系统 `.ttc` 中抽出为单独的 `.ttf` | `fonts.py:14-20`；`build/fonts/` 现存 5 个 ttf |
| 拉丁字体 | Futura Condensed ExtraBold（年份大字）、Georgia Italic、Arial Black、Menlo；回退字体 Arial Unicode、Hiragino Sans GB | `lib.mjs:15-34` |
| 平台 | 依赖 macOS 系统字体路径（`/System/Library/...`），推断在 macOS 上制作；硬件未记录 | `fonts.py:12-13`、`lib.mjs:20-24` |

- 为什么要抽字体：`fonts.py:3` 注释说明 Canvas 只注册 `.ttc` 的第一个字重，所以要把需要的字重单独存成 `.ttf`。`lib.mjs:11-13` 在缺字体时自动调用 `.venv/bin/python fonts.py`。
- 没有使用任何视频生成或图像生成模型的代码调用。片尾署名中的 "ChatGPT" 只说明静帧来源，**具体模型、提示词、参数均未记录**。

### 图片素材来源与授权

| 文件 | 尺寸 | sha256 前 16 位 | 修改时间（文件系统） |
| --- | --- | --- | --- |
| `assets/stage-1992.png` | 1448×1086 RGB | `e178a21bfb71fd1a` | 10-17 15:42 |
| `assets/stage-1992-2.png` | 1448×1086 RGB | `aab4c299425b3a5c` | 10-17 16:05 |
| `assets/stage-1992-3.png` | 1448×1086 RGB | `bb37f368b355d3f5` | 10-17 16:06 |
| `assets/stage-1992-4.png` | 1448×1086 RGB | `4105e039a1ee06bd` | 10-17 16:06 |
| `assets/stage-1992-5.png` | 1448×1086 RGB | `a0a2f6288a02c963` | 10-17 16:06 |

- **来源记录**：只有片尾署名一处，写的是「1992 电视画面为 AI 生成的示意图（ChatGPT）」（`scenes-h.mjs:146`）。`vhs.mjs:2` 注释称其为 "a photoreal still from assets/"。
- **元数据**：本轮解析了 5 个 PNG 的块结构，没有 tEXt/iTXt/zTXt/eXIf 块，**文件内没有生成工具、提示词或 C2PA 信息**，因此无法从文件本身核实来源。
- **授权**：项目中**没有授权、许可证或使用条款记录**。生成平台的条款是否允许此用途，未记录、无法确认。
- **画面内容**（本轮查看了 `stage-1992.png` 一张）：照片级的 90 年代电视综艺舞台，三名身穿宽松外套和牛仔裤、戴棒球帽的男性舞者，前景是观众后脑勺。与旁白「徐太志和孩子们」1992 年选秀登台的情节对应，属于「示意图」。注意：这与 `people.mjs:1` 所写的 "no real likenesses" 原则存在张力。画面人物是否与真实人物相似、是否有肖像风险，本档案**无法确认**，建议发布前人工复核。
- `images/` 是空目录（10-17 16:08 创建），用途未记录。

## 4. 镜头与时间轴

`build/timeline.json` 已被清理。下表的时间是本轮按 `tts.py` 的排布算法，用 `build/tts_cache/` 中 34 份词边界 JSON（缓存键 = sha1(`声线|语速|音调|文本`) 前 16 位，全部命中 `rate=0`）离线重算得到的。重算总长 200.867 s，与成片完全一致。场景入点公式取自各 `scenes-*.mjs` 末尾的 `scenes` 数组。章节起点 = 该章第一段旁白开始时间 − 1.1 s（`lib.mjs:102-104`）。

| 章节（配色/标签） | 镜头 id | 约起点 s | 内容 | 实现入口 | 关键参数 |
| --- | --- | --- | --- | --- | --- |
| intro（A 面） | deck | 0.00 | 磁带放进卡座，「咔哒」按下播放 | `scenes-a.mjs:12` | `still: true`，不做推镜 |
|  | tape | 5.22 | 「三十年录进一盘磁带」 | `scenes-a.mjs:32` | 入点 `S('q') − 0.25` |
|  | cards | 10.26 | 5 张曲风闪卡，逐词硬切（说唱/新杰克摇摆/电子舞曲/迪斯科/泽西俱乐部） | `scenes-a.mjs:48-72` | 每张卡在 `at('borrow', 词) − 0.06` 出现 |
|  | collage | 15.60 | 10 个曲风贴纸飞散后排成两行，盖章「借来的」 | `scenes-a.mjs:74-120` | `TAGS[i].d = i·0.06` 错峰 |
|  | title | 23.04 | 片名卡 | `scenes-a.mjs:123` | riser 音效 + boom |
| c92（TRACK 01 · 1992） | year92 | 26.04 | 年份计数器从 2026 滚到 1992 | `scenes-b.mjs:63-71` | `lerp(2026, 1992, easeOut)`，用时 1 s |
|  | room | 28.75 | 90 年代客厅里的 CRT 电视播放选秀舞台（**AI 静帧 + VHS 退化效果**），推镜 1→1.55，叠加节目字幕条「徐太志和孩子们／《我知道》난 알아요」 | `scenes-b.mjs:100-122`，`vhs.mjs` | BPM 104，每拍换一张静帧 |
|  | recipe | 36.94 | 配方卡：美国说唱 + 新杰克摇摆 + 韩语流行 | `scenes-b.mjs:125-` | 三个贴纸跟词弹出 |
|  | score | 42.87 | 评委打出最低分 | `scenes-b.mjs` | buzz + stamp 音效，落在「最低分」 |
|  | chart | 46.03 | 冲上榜首，连续霸榜 | `scenes-b.mjs` | 18 个 tick，间隔 0.13 s |
|  | rule1 | 51.76 | 第一条规律 | `scenes-b.mjs` | — |
| c96（TRACK 02） | factory | 57.21 | 造星工厂蓝图，「复制」二字像复印一样叠出 | `scenes-c.mjs:22` | — |
|  | conveyor | 63.99 | 传送带四个工位：选人/训练/写歌/编舞 | `scenes-c.mjs:62-99` | 每个工位一个 click |
|  | trainee | 72.63 | 练习生档案，唱歌/跳舞/外语进度条 | `scenes-c.mjs:102-139` | 进度条数值 0.86/0.93/0.71 |
|  | hallyu | 79.72 | 点阵地球仪，从首尔向亚洲城市画弧线 | `scenes-c.mjs:141-182`，`props.mjs:128-183` | 球面插值大圆弧 |
| c07（TRACK 03） | hook | 86.90 | 新武器：洗脑副歌 | `scenes-d.mjs:21` | BPM 128 |
|  | repeat | 93.66 | 一个词 + 一个动作反复出现 | `scenes-d.mjs:47` | — |
|  | singles | 99.49 | 三张单曲封面：Tell Me / Gee / Sorry, Sorry | `scenes-d.mjs:87-119` | 说到 Gee 时闪白（peak 0.35） |
|  | writers | 105.26 | 欧洲、美国的 Demo 送到首尔重新组装 | `scenes-d.mjs:122-` | — |
| c12（TRACK 04） | upload | 112.55 | 视频播放器窗口里，骑马舞剪影随节拍跳动；进度条显示「上传中 → 已发布 ✓」 | `scenes-e.mjs:18-43` | 上传进度 1.0 s，从「传上」开始 |
|  | billion | 117.92 | 播放量冲破十亿 | `scenes-e.mjs:47` | riser 精确结束在「十亿」，boom + 闪白 |
|  | notv | 123.67 | 旧电视彩条信号关闭、缩成一条亮线后滑出画面；点阵地球仪自转，冒出 24 个播放图标，最后出现一个大播放键 | `scenes-e.mjs:64-105` | 第二处 `crtTV`；图标间隔 0.09 s |
| c20（TRACK 05） | chart20 | 129.84 | 单曲榜第一名 + 迪斯科球 | `scenes-f.mjs:12-59` | BPM 114 |
|  | first | 137.35 | 韩国歌手第一次拿下冠军 | `scenes-f.mjs:62` | 合成欢呼声 2.5 s |
|  | english | 141.27 | 英文歌词 vs 舞台/粉丝/打榜三项打勾 | `scenes-f.mjs:92-119` | — |
|  | fans | 148.29 | 260 根应援棒组成的光海 | `scenes-f.mjs:122-` | 欢呼声 4.5 s |
| cnow（TRACK 06） | song | 153.34 | 一首歌分成三段：嘻哈 → 流行 → 换节拍 | `scenes-g.mjs:37-76` | 配乐同步换风格（第 6 节） |
|  | retro | 164.38 | 泽西俱乐部步进音序器网格 | `scenes-g.mjs:79-` | 四行：KICK/CLAP/HAT/BELL |
| method（BONUS） | secret | 171.15 | 六个年代曲风标签闪过 | `scenes-h.mjs:13` | — |
|  | method | 177.21 | 拿来 → 拆开 → 重新组装 | `scenes-h.mjs:31-60` | 「重新组装」处配乐切到 house |
|  | machine | 181.56 | 齿轮机器：说唱/迪斯科/电子/R&B 进去，K-pop 出来 | `scenes-h.mjs:63-109` | boom + 闪白 |
| outro（B 面） | flip | 188.35 | 磁带翻面，标签从 A 换成 B | `scenes-h.mjs:112-121` | 用 `cos(π·p)` 横向缩放模拟翻转 |
|  | end | 191.27 | 「我的播放列表」+ 196.35 s 起显示片尾署名 | `scenes-h.mjs:124-150` | 最后 0.8 s 渐黑 |

> 「约起点」由缓存重算得出，精确到约 0.01 s。表中只填了能直接算出入点的镜头；其余参数来自对应源码，本轮没有逐行核对每个场景的全部绘制细节。

### 全局叠加层（`render.mjs:85-114`，绘制顺序）

1. 年代底色：章节切换后 0.5 s 内从上一章颜色渐变过来（`eraBg`）。
2. 场景：`sceneAlpha` 默认交叉淡化 `XF = 0.25 s`。`cut` 入点硬切，`cutOut` 出点硬切。非 `still` 镜头统一做 Ken Burns 推镜，缩放 1 → 1.035。
3. 章节漏光：在每章起点前 0.3 s 开始，持续 1.3 s。c92 与 outro 用暖橙色，其余用粉色。
4. 事件闪白：`EVENTS` 中带 `flash` 的事件，持续 0.2 s。
5. HUD：左上角「TRACK 0x · 年份」，右上角片名，右下角磁带计数器 `floor(t·7.3)`。
6. 暗角 0.5；胶片颗粒：c92 强度 0.11，其他章节 0.07。
7. 章节磁带故障效果：每章起点前后共 0.4 s。
8. 字幕：画在故障效果之后，所以不受故障影响。
9. 片头淡入 0.5 s，片尾 0.8 s 渐黑。

## 5. 关键算法与实现

**视频讲的知识**：K-pop 音乐史，即「借来外国曲风 → 用韩语和工业化流程重组」。片中出现的年份、榜单和播放量等事实都写在 `script.json` 中；本档案**没有核实这些史实**，也没有记录项目做过事实核查。
**制作视频用的算法**：下面 6 项。片中用合成节拍「演示」各年代曲风，这是制作手段，不代表片子讲解了作曲算法。

### 5.1 TTS 词边界 → 全片时间轴（`tts.py:133-195`）

- 逐段调用 Edge TTS，打开 `boundary="WordBoundary"` 收集每个词的 `offset/duration`（单位 100 ns，`TICKS = 1e7`）。用 `text.find(词, cursor)` 记下每个词在原文中的字符位置 `idx`（`tts.py:67-83`）。
- 每段的占用时长 = 最后一个词的结束时间 + 0.12 s。不用 mp3 实际长度，因为 mp3 尾部约有 0.6 s 静音（`tts.py:146-147` 注释）。
- 总长 = `LEAD_IN 2.4` + Σ段长 + Σgap + `TAIL 5.0`。每段的 `gap` 可以在 JSON 里单独设，默认 0.45。章节末尾设 1.0，片名后设 1.2。
- **自动语速**：如果总长 > `TARGET 215`，按超出比例直接跳到所需语速 `rate = min(30, max(rate+2, ceil(rate + (Σspeech/available − 1)·100) + 1))` 后重新合成（`tts.py:144-153`）。本片在 `rate=0` 时总长 200.87 s，不需要加速。缓存中只有 `rate=0` 的 34 段，也印证了这一点。
- 混音：ffmpeg 用 `adelay` 把各段放到对应时间点，`amix normalize=0` → `highpass 80 Hz` → `loudnorm I=-16:TP=-1.5:LRA=11`，输出 `build/narration.wav`（48 kHz 立体声）。

### 5.2 用词锚定动画（`lib.mjs:44-55`）

```js
export function word(id, w, nth = 0) {           // 找到第 nth 次出现的词
  const s = SEG[id]; let i = -1;
  for (let k = 0; k <= nth; k++) i = s.text.indexOf(w, i + 1);
  const hit  = s.words.find(x => x.idx <= i && i < x.idx + x.text.length) ?? s.words.find(x => x.idx >= i);
  const last = [...s.words].reverse().find(x => x.idx < i + w.length) ?? hit;
  return { start: hit.start, end: last.end };
}
export const at = (id, w, nth) => word(id, w, nth).start;
```

所有镜头入点、贴纸弹出和音效时刻都写成 `at('段id','词') ± 偏移`，例如 `at('low','最低分') + 0.05` 处盖章。注释强调 "from the TTS timestamps, never estimated"。这样改旁白文字或语速后，画面和音效会自动重新对齐；词不存在时直接抛错。

### 5.3 VHS 电视画面（`vhs.mjs`）

素材优先级（`vhs.mjs:12-31`）：`assets/stage-1992.mp4|mov|webm` 视频片段（用 ffmpeg 抽成 30 fps、720 宽的帧序列）> 多张 `stage-1992*.png|jpg` 静帧 > 都没有时用代码画的舞台（`scenes-b.mjs:17-43` 的 `drawnStage`）。**本片实际只有 5 张 PNG，走的是「多张静帧」分支。**

多张静帧的「导播切镜」（`stageShot`）：
- 每拍（BPM 104，约 0.577 s）换一张静帧：`img = frames[floor(bt) % 5]`。
- 构图从 3 种里轮换：`STILL_CAMS = [[0.5,0.5,1.02],[0.5,0.45,1.25],[0.5,0.48,1.1]]`（中心 x、中心 y、缩放）。注释："3 framings vs 5 poses so repeats never look the same"。3 和 5 互质，15 拍才会重复同一组合。
- 手持晃动：`sin(d·0.9)·0.008`；每拍额外推近 `exp(−(bt%1)·5)·0.02`。

退化流程（`vhs()`）：
1. 先把画面画到 360×270 的低分辨率画布上（约等于 VHS 亮度分辨率），再放大，让细节变软。
2. 色度渗色：低清图再 `blur(2px) saturate(1.6)`，右移 5 px，用 screen 模式按 0.35 叠加。
3. 主画面 `blur(0.6px) contrast(1.08) saturate(0.9)`；有 8% 的帧随机上下抖动 ±3 px。
4. 隔行扫描线：每 4 px 一条，奇偶帧交替。
5. 跟踪带：14 条横向撕裂亮线，以 `t·0.18` 的速度缓慢下移。
6. 磁头切换噪声：底部 6 条彩色线。
7. 雪花点：160 个。

外层再加 CRT 扫描线和滚动亮带（`fx.mjs:38-49`），然后装进代码画的 CRT 电视外壳（`props.mjs:99`）。

### 5.4 转场与胶片质感（`fx.mjs`）

- **颗粒**：预先生成 6 张半分辨率 LCG 噪声图，按 `floor(t·24) % 6` 循环，用 overlay 模式叠加。不用每帧重新生成噪声，节省时间。
- **磁带故障转场**（`glitch`）：先把已画好的整帧拷到临时画布，再取最多 14 条横条随机横移（最大 ±130 px·k），叠加 RGB 错位（screen 模式 0.35·k）和细扫描线。`k = sin(π·p)`，在章节点达到峰值。随机数用 `rand(帧号·常数 + i)` 生成，可以复现。
- **漏光**：径向渐变光斑从左扫到右，screen 模式，透明度 `sin(π·p)·0.55`。
- **确定性随机**：`rand(i) = fract(sin(i·127.1 + 311.7)·43758.5453)`（`lib.mjs:71-74`）。音频用 mulberry32 风格的 `rng(seed)`，音乐 seed 42，音效 seed 7。因此同样的输入每次渲染结果相同。

### 5.5 点阵地球仪与大圆弧（`props.mjs:128-183`）

- 陆地用 22 个经纬度椭圆粗略表示（`LAND`）。按 1.5° 网格采样，落在陆地内的点进入 `DOTS`。
- 投影：先按经度旋转 `lon0`，再按 `tilt` 绕 x 轴倾斜，`z ≤ 0` 的点在背面，不画。点的透明度和大小随 `z` 变化，营造立体感。
- 弧线：两地坐标转成单位向量后做球面线性插值（slerp），弧高 `1 + 0.18·sin(πk)`。按进度 `p` 只画前一部分点；转到背面时断笔。
- 效果是轻量的「3D 地球」，不需要贴图。限制：海岸线非常粗略，只适合示意。

### 5.6 程序合成配乐（`audio.mjs`）

- 基本单元：`tone`（相位累加振荡器 + 包络）；`bandNoise`（RBJ 带通双二阶滤波器，中心频率按指数从 fFrom 扫到 fTo，每 32 个采样更新一次系数）；`synth`（多声部失谐锯齿波/方波 + 一阶低通）。
- 鼓：kick 是正弦波，频率从 158 Hz 指数下滑到 48 Hz；snare = 190 Hz 音调 + 带通噪声；clap 是三次短促噪声加一段尾音；hat 是 8.5–9.5 kHz 噪声。
- 每个年代一种 `STYLES`（每小节一个函数），用 `lay()` 铺满指定时间段，带淡入淡出：

| 风格 | BPM | 用在 | 特征（源码） |
| --- | --- | --- | --- |
| lofi | 78 | intro、method 前半、outro | 慢拍，电钢琴和弦 |
| njs（新杰克摇摆） | 104 | c92 | 十六分音符摇摆：0.25→0.33、0.75→0.83 |
| dance | 118 | c96 | 四拍底鼓，反拍开镲，铺底和弦 |
| electro | 128 | c07、cnow 副歌段 | 同一个音敲 4 次再回应，模拟「洗脑副歌」 |
| house | 128 | c12、method「重新组装」之后 | 低两个八度的 bass，四声部和弦 |
| disco | 114 | c20、method 尾段（gain 0.35） | 八度跳动 bass，rim 声 |
| boombap | 90 | cnow 主歌 | 嘻哈节奏 |
| jersey | 140 | cnow 桥段之后 | 切分底鼓 `[0,.75,1.5,2,2.5,3,3.5]`，加铃声 |

- 和旁白同步换风格：cnow 章节里，在 `at('parts','副歌') − 0.1` 从 boombap 切到 electro，在 `at('parts','桥段') − 0.1` 再切到 jersey（`audio.mjs:167-170`）。旁白说到哪种曲风，配乐就同时换成那种曲风。
- 全片底噪：3 kHz 带通噪声，振幅 0.012，模拟磁带嘶声。最后整体乘 0.42。
- `writeWav`：峰值超过 0.98 时归一化到 0.98，写成 16-bit 立体声，两个声道内容相同。

## 6. 声音与字幕

### 配音

- 引擎：Edge 在线神经语音 TTS，经 Python 包 `edge_tts` 调用（版本未记录）。声线 `zh-CN-XiaoxiaoNeural`（晓晓），语速 +0%，音调 +0Hz。全片 34 段都用这一种声线。
- 稳定性处理：同样的文本、声线、语速、音调会命中缓存（`build/tts_cache/<sha1前16>.mp3|json`），不会重复请求。遇到 429 限流时退避重试最多 5 次，等待 8/16/24/32 s（`tts.py:50-60`）。旧端点返回 403 时改用新的 WSS 地址（`tts.py:18-24`）。
- 缓存现状：68 个文件 = 34 段 × (mp3 + json)，与当前 `script.json` 一一对应。时间戳集中在 10-17 11:31。

### 字幕

- 断句：优先使用 `script.json` 里手写的 `captions` 数组（大部分段落都写了）。没写时用 `split_caption` 自动断句：在中文标点处切分，每块不超过 14 字，句末标点强制断开（`tts.py:104-117`）。
- 对时：字幕块 → 用 `find` 找到它在原文中的字符区间 → 取区间内词边界的第一个开始时间和最后一个结束时间（`tts.py:120-130`）。**不靠估算**。
- 输出：`out/借来的声音-Kpop三十年.srt`，共 91 条，第 1 条 `00:00:02,450 → 00:00:03,000「咔哒」`，最后一条结束于 `00:03:15,747`。
- 烧录到画面：`render.mjs:54-72`。PingFang Semibold 40 px，半透明黑底圆角框（y = 968，高 64），左侧有一条 5 px 的年代强调色竖条。同一段内字幕连续显示，直到下一条开始，中间不留空。段内第一条提前 0.08 s 淡入；段内最后一条在结束后 0.15–0.35 s 淡出。字幕画在故障效果之后，转场时不会被打乱。

### 音效（`audio.mjs:183-208`）

共 13 种合成音效：click/whoosh/pop/stamp/tick/ding/boom/riser/rewind/scratch/cheer/buzz/type。全部由各 `scenes-*.mjs` 的 `events` 数组按 `at(...)` 词边界触发；`flash` 字段同时触发画面闪白。每个章节前 0.35 s 有一个 `rewind`（倒带声），对应画面上的磁带故障转场。

### 混音（`render.mjs:135-138`）

```text
narration ──asplit──┬──────────────────────────────┐
                    └─(sidechain key)              │
music ×0.6 ── sidechaincompress(threshold 0.04, ratio 6, attack 15 ms, release 450 ms) ──┤
sfx ×0.55 ─────────────────────────────────────────┤
                         amix(normalize=0, duration=first) → alimiter(limit 0.93) → AAC 192k
```

- 旁白在混音前已做 `loudnorm I=-16 LUFS, TP −1.5 dB`。**成片整体响度没有测量记录**，本轮也没有测量。
- **未试听**：本轮没有做主观听感检查，项目里也没有听感检查记录。

## 7. 迭代、问题与修正

没有 plan 文档、制作日志或对话。以下内容来自代码注释和文件修改时间（10 月 17 日）。时间戳只反映文件最后一次写入，不能代表完整过程；多数源码的修改时间都是同一时刻 11:57，可能是批量写入或复制造成的，所以只作为弱证据。

| 时间（文件系统） | 文件 | 解读 |
| --- | --- | --- |
| 11:30–11:32 | `package-lock.json`、`build/fonts/*`、`build/tts_cache/*`、SRT | 安装依赖、抽取字体、合成旁白和字幕（推断） |
| 15:42 | `assets/stage-1992.png` | 第一张 AI 舞台静帧放入项目 |
| 15:44 | `scenes-h.mjs` | 片尾署名可能在此时加入「AI 生成的示意图（ChatGPT）」（推断） |
| 15:49 | `scenes-b.mjs` | 电视画面接入 `STAGE`（推断） |
| 16:05–16:06 | `stage-1992-2…5.png` | 又加入 4 张同一场景、不同姿势的静帧 |
| 16:08 | `images/`（空目录） | 用途未记录 |
| 16:09 | `vhs.mjs` | 最后修改的源码：多静帧逐拍切换和 3 种构图轮换（推断） |
| 16:12 | `out/…mp4` | 成片写出时间晚于所有源码，**推断**是用当前代码渲染的 |

代码中能看到的取舍和修正：
- **电视画面素材三级降级**（`vhs.mjs:12-31`、`scenes-b.mjs:16`）：注释把「图生视频片段」列为首选，但项目中没有任何 mp4/mov 素材。**推断**没有做图生视频，或者做了但没有采用，具体未记录。代码画的舞台 `drawnStage` 保留为兜底方案，推测是最早的版本（推断）。
- **只有 1 张静帧时用 6 机位轮换**（`CAMS`，每 2 拍切一次）；**多张静帧时改为每拍换姿势 + 3 种构图**。这说明先做了单张图的方案，再补了多张图（推断，依据是两套参数并存，且多张图的文件更晚出现）。
- **Edge TTS 403 修补**：注释写明 2026 年 10 月旧端点开始返回 403，所以直接覆盖 `WSS_URL`，等 `edge-tts` 发布修复版后再移除。
- **段落间隔不用 mp3 长度**：mp3 尾部约 0.6 s 静音会挤占下一个 gap，所以改用最后一个词的结束时间 + 0.12 s。
- **语速一步到位**：按超出比例直接计算目标语速，不逐级试探（`tts.py:152`），减少重复合成。本片没有触发。
- 人物剪影的原则是「no real likenesses」，但 1992 镜头改用了照片级 AI 静帧。这是一次方向调整；原因未记录。

## 8. 可复用制作经验

1. **所有动画和音效都锚定到 TTS 词边界，不手填秒数**。改旁白或语速后，画面会自动重新对齐。依据：`lib.mjs:44-55`，各 `scenes-*.mjs` 的 `events`。
2. **按「最后一个词的结束时间」排段落，不按音频文件长度**，避免 TTS 尾部静音把节奏拖松。依据：`tts.py:146-147`。
3. **字幕和旁白放在同一个 JSON 里，并手写断句**；自动断句只作兜底。字幕时间直接取自词边界。依据：`script.json` 的 `captions`、`tts.py:104-130`。
4. **外部素材分级降级（视频 > 多静帧 > 代码绘制）**：没有素材时管线也能出片，有素材后无需改场景代码。依据：`vhs.mjs:12-31`。
5. **用年代化退化处理统一 AI 照片和矢量画面的质感**：先缩到低分辨率，再加色度渗色、隔行扫描线、跟踪带，既符合 1992 电视的观感，也能弱化 AI 生成图的细节瑕疵。依据：`vhs.mjs:69-117`。
6. **少量静帧做出「导播切镜」**：静帧数和构图数取互质（5 与 3），每拍切一次，再加手持晃动和每拍推近。依据：`vhs.mjs:47-67`。
7. **配乐风格跟着旁白里的词切换，并用侧链压缩避让人声**：让「讲到哪种曲风就听到哪种曲风」。依据：`audio.mjs:167-170`、`render.mjs:137`。
8. **所有随机数都用确定性种子**（`rand(i)`、`rng(42/7)`、预生成颗粒图），重新渲染结果一致，便于逐帧对比修改。依据：`lib.mjs:71`、`audio.mjs:8-17`、`fx.mjs:6-19`。

## 9. 复现入口

以下命令来自源码注释和代码。**本轮没有执行。**

```bash
cd kpop-borrowed-sound
npm install                                   # @napi-rs/canvas 1.0.11（按 package-lock）
python3 -m venv .venv && .venv/bin/pip install edge-tts aiohttp fonttools   # 版本未记录，需要自行锁定
.venv/bin/python tts.py                       # → build/timeline.json、build/narration.wav、out/<标题>.srt
node render.mjs --still 3.2 7.6               # 先出单帧预览 → build/still_<t>.png
node render.mjs                               # 全片 → build/music.wav、build/sfx.wav → out/借来的声音-Kpop三十年.mp4
```

- 执行顺序：`tts.py` 必须先跑。`lib.mjs:37` 启动时会读取 `build/timeline.json`，文件已被清理，不先跑 TTS 就无法渲染。
- `fonts.py` 不用手动跑：`lib.mjs` 发现缺字体时会自动调用（需要 `.venv` 中有 fontTools）。`build/fonts/` 现存 5 个 ttf，可以直接用。
- 现存 `build/tts_cache/` 覆盖全部 34 段（rate 0），只要不改文本、声线、语速，`tts.py` 可以**完全离线**命中缓存重建时间轴和旁白。
- 必须保留 `assets/stage-1992*.png`；删掉后会退回代码绘制的舞台。
- 依赖 macOS 系统字体路径；在其他系统上会退回 Arial Unicode / Hiragino，或者缺字。
- 渲染耗时：**未记录**。文件时间戳显示最后一次源码修改（16:09）到 MP4 写出（16:12）约 3 分钟，但无法确认渲染的开始时间，所以不能当作耗时数据。

常改参数的位置：

| 想改什么 | 改哪里 |
| --- | --- |
| 旁白文字、断句、段间停顿、章节 | `script.json`（`text`/`captions`/`gap`/`chapter`） |
| 声线、语速 | `script.json.voices`、`script.json.rate`；总长上限 `tts.py:30 TARGET` |
| 年代配色和标签 | `lib.mjs:91-101 ERA` |
| 转场长度 | `render.mjs:24 XF`；漏光和故障效果的参数在 `render.mjs:99-107` |
| VHS 强度、电视切镜 | `vhs.mjs:34`（低清分辨率）、`vhs.mjs:48-49`（机位表） |
| 配乐风格、BPM、各段音量 | `audio.mjs:91-146 STYLES`、`audio.mjs:156-180 buildMusic` |
| 混音比例和压缩 | `render.mjs:137-138` |
| 输出质量 | `render.mjs:140`（CRF 18，preset medium） |

## 10. 参考来源

| 链接 | 类型 | 本地出处 | 借鉴内容 | 访问状态 |
| --- | --- | --- | --- | --- |
| https://x.com/ameliax111/status/2110756490575589711 | 创作参考（X 帖子，作者 Ameliayu 余一 @ameliax111） | `script.json:3`；片尾署名 `scenes-h.mjs:147` | 本地记录写的是「只借用其叙事结构与画面风格，画面与音乐均为代码原创」。具体借鉴了哪些镜头，**关系未确认** | 2026-11-12 web_fetch 成功，读到帖子文字：用 Manus 一个提示词生成约 3 分钟的 K-pop 声音演变视频，混合 AI 画面、真实素材和动态图形；发布时间 2026-10-15 15:35（页面显示）。**帖内视频未观看** |
| rany2/edge-tts issue #490、#491 | 技术资料 | `tts.py:19` 注释 | Edge TTS 合成端点从 `speech.platform.bing.com` 迁到 `api.msedgeservices.com` 的修补依据 | 注释中只有 issue 编号，没有完整 URL；本轮**未访问** |
| ChatGPT（图像生成） | 素材来源 | 片尾署名 `scenes-h.mjs:146` | `assets/stage-1992*.png` 5 张 1992 舞台示意图 | 不是链接；具体模型、提示词、生成日期和授权条款**未记录** |

片中的史实（徐太志和孩子们、H.O.T.、江南 Style 十亿播放、Dynamite 登顶等）在项目中**没有列出出处**。

## 11. 证据与限制

**本轮实际读取的关键文件**（sha256 前 16 位取自 fingerprints.json；assets 是本轮计算的）：

| 文件 | sha256 前 16 位 | 读取范围 |
| --- | --- | --- |
| `script.json` | `d3e910ce31ade542` | 全文 |
| `tts.py` | `936e951dfdbe6230` | 全文 |
| `fonts.py` | `82f5dbc915be5869` | 全文 |
| `render.mjs` | `d10572daf690e849` | 全文 |
| `lib.mjs` | `3037a8c0a5fcc23f` | 全文 |
| `fx.mjs` | `b9a13c33fdb0a296` | 全文 |
| `vhs.mjs` | `fc3ecaac3b7aa04e` | 全文 |
| `audio.mjs` | `1a8b40bfaa2bab4b` | 全文 |
| `package.json` / `package-lock.json` | `308e7946859b93e1` / `e8f2b53e2d7fdc7e` | 版本字段 |
| `scenes-a.mjs` | `77eb7ae4e89799f5` | 1–60、74–92、134–155 |
| `scenes-b.mjs` | `3cdf2c2df02d2b92` | 1–130、214–237 |
| `scenes-c/d/f/g.mjs` | `450c4b68…` / `f7160916…` / `e898b836…` / `5983087f…` | 函数和常量索引、`scenes`/`events` 数组 |
| `scenes-e.mjs` | `bd5991b094dbbb6e` | 17–105、107–126 |
| `scenes-h.mjs` | `bf8ffc18e7d5d10d` | 索引、111–175 |
| `people.mjs` | `b9abd1d7989d667c` | 1–60 |
| `props.mjs` | `624aada94c3f3fc5` | 函数索引、128–183 |
| `build/tts_cache/*.json`（34 份） | 见 fingerprints.json | 用于离线重算时间轴 |
| `out/借来的声音-Kpop三十年.srt` | `3b1c43b54e7690ff` | 首尾条目、条数 |
| `out/借来的声音-Kpop三十年.mp4` | —（234,538,101 字节） | 仅 ffprobe 媒体头 |
| `assets/stage-1992*.png`（5 张） | 见第 3 节 | PNG 块结构；只看了 `stage-1992.png` 一张的画面 |

**未读取或无法确认的内容**：
- `scenes-c/d/f/g` 的大部分绘制细节、`props.mjs` 中磁带/卡座/电视/应援棒的绘制细节、`people.mjs` 61 行之后的内容，只看了结构。
- 成片没有播放或抽帧检查，片尾署名是否出现在成片中，只能从时间戳**推断**。另外 4 张静帧的画面没有查看。
- 缺失：原始需求或提示词、制作对话、plan 文档、Python 依赖版本、Node 和 ffmpeg 版本、渲染耗时、成片响度、视觉和听感检查记录、用户验收、AI 静帧的模型、提示词和授权、史实出处。
- `build/` 中的 `timeline.json`、`narration.wav`、`music.wav`、`sfx.wav`、`still_*.png` 都已被清理。缺失不代表从未生成过。
