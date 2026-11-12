---
type: video-production-dossier
project: geo-story-studio-opus
video_id: sun-angle
title: 为什么同样晒太阳，有的地方更热？
curriculum: script.json 未记录（README 归入“必修第一册 第一章第二节 太阳对地球的影响”）
captured: '2026-11-12'
status: 成片存在（竖屏 1080×1920，89.7 s）；技术检查/视觉检查/用户验收未记录
source_fingerprint: 3b734b0b033ecba9a8734abca8edf5db3f8a9a8cac9724db482dc8abdd0ce64e
tags:
- video-creation
- production-capture
- geography-teaching
- canvas-frame-render
- edge-tts
---

# 为什么同样晒太阳，有的地方更热？ · 公平实验 + “能量豆”计数：照射角度与单位面积能量（Canvas 逐帧 + 逐词时间戳）

<!-- capture-source-sha256:3b734b0b033ecba9a8734abca8edf5db3f8a9a8cac9724db482dc8abdd0ce64e -->

本文是**根据项目文件做的材料回溯**，用到的材料包括：源码、`script.json`、`build/timeline.json`、`out/` 下的成片与字幕，以及证据包里的 ffprobe 结果和 sha256 清单。证据包中 `host_session` 为空，**未发现制作会话**，制作对话、需求原文和修改过程都无法找回。本次没有运行任何项目脚本（`tts.py`、`render.mjs`、npm、pip 都没跑），也没有打开视频或音频。

## 1. 当前结论与制作路线

**状态：成片存在**：`out/为什么同样晒太阳有的地方更热.mp4` 和同名 `.srt`。目前只有 ffprobe 读到的媒体头信息；视觉检查、试听、响度、用户验收、是否发布，都**未记录**。

| 项 | 值 | 来源 |
| --- | --- | --- |
| 分辨率 / 画幅 | 1080×1920，9:16 竖屏 | ffprobe；`lib.mjs:7` |
| 帧率 / 帧数 | 30 fps；`Math.round(89.69 × 30)` = 2691 帧 | ffprobe；`render.mjs:165`（推算） |
| 时长 | 89.700 s（timeline 89.69 s） | ffprobe；`build/timeline.json` |
| 视频编码 | H.264，yuv420p，CRF 19，preset medium，`+faststart` | ffprobe；`render.mjs:162-163` |
| 音频编码 | AAC 48 kHz 立体声，192 kbps | ffprobe；`render.mjs:163` |
| 文件大小 | 17,470,417 字节（约 17.5 MB，平均约 1.56 Mbps） | 证据包 |
| 字幕 | `.srt` 53 条（2937 字节）；画面另有烧录字幕 | `timeline.json` cues |
| 其他版本 | 无 | `out/` 列表 |

文件时间：`script.json` 10-21 20:29，TTS 产物 20:30，`render.mjs` 20:42，成片 20:44。在本批 5 片中最晚，git 只有初始提交 `4831a07`。

一句话管线：

> `script.json`（23 段：旁白 + 小宇 + 太阳 + 云朵）→ `tts.py`（edge-tts + WordBoundary）→ `timeline.json` / `narration.wav` / SRT → `render.mjs` 动态加载 `scenes-a…e.mjs`（8 个场景），复用 `sun.mjs`、`waves.mjs`，新增 `beam.mjs`（光束几何、能量豆、地砖、手电筒）→ 程序合成音频 → ffmpeg 管道输出 MP4。

## 2. 教学目标与课标对齐

- **没有 curriculum 字段**。README 把本片归入“必修第一册 第一章第二节 太阳对地球的影响”，简介写的是“太阳高度角越小，同样的能量分布面积越大，单位面积获得的太阳辐射越少”。对应的课标条目**未记录**。
- **核心知识点**：其他条件相同时，阳光越倾斜，同一束光的能量摊到的地面面积越大，每平方米得到的能量越少；越接近垂直，能量越集中。实际气温还和日照时间、云量等因素有关，不能只看照射角度。
- **叙事结构**：
  - 导入：小宇问“同样晒太阳，为什么有的地方更热”，太阳喊冤“我对谁都一样大方”。
  - 原理：先做公平比较（光束、照射时长、平地三项锁定，只改照射角度）→ 手电筒实验（正照时光斑小而亮，斜照时光斑大而暗）→ 小宇追问“光又没少，怎么变暗了？”→ 用能量豆计数：8 颗豆，垂直照射时每格 4 颗，倾斜照射时每格 2 颗。
  - 结论：每平方米分到的能量变少。
  - 迁移：小宇用“黄油抹面包”打比方；补充说明角度不能单独决定气温，日照时长和云量也有影响。
  - 总结：`sum` 用“其他条件相同时……”完整表述，太阳和小宇告别。
- **科学边界**：
  - 总结句前加了“**其他条件相同时**”。
  - `butNot` 到 `together` 几句专门交代多因素：“照射角度不能单独决定实际气温”。
  - “能量豆”用的是比喻，画面标题写着“同样一束光 = 同样多的能量”。
- **术语缺口**：旁白和画面都只说“照射角度”“接近垂直 / 更倾斜”，**没有出现教材用语“太阳高度角”**（本次全文检索 `sun-angle/*.mjs` 和 `script.json`，没有找到“高度角”）。README 简介里用了这个术语。按 AGENTS.md §6“比喻之后要给出教材里的正式说法”，这里有缺口，见 §7。
- **数字**：8 颗豆、每格 4 颗或 2 颗，都是画面上的教学比例，几何关系见 §5.1，不来自外部数据，旁白里也没有其他数值。
- **based_on（推断）**：`source` 中写的是“douyin-sun-angle v002 分镜”，对应 `geo-story-studio/storage/builds/douyin-sun-angle/v002/`，本次未核对。

## 3. 技术与依赖

| 类别 | 内容 | 证据 |
| --- | --- | --- |
| 渲染 | `@napi-rs/canvas` 声明为 `^1.0.11`，**lock 锁定 1.0.13**（与 [[sunlight-through-space]] 相同） | `package.json`；`package-lock.json` |
| npm scripts | 无 | `package.json` |
| TTS | `edge-tts` + `aiohttp`，没有 requirements 文件 | `tts.py` |
| 字体 / ffmpeg | 与 [[nebula-nursery]] 相同 | `fonts.py`；`tts.py:176-190`；`render.mjs:155-164` |
| 本地环境（不是声明的依赖） | `.venv` 中能看到 `edge_tts-7.3.1`、`fonttools-4.67.0` | 目录列表 |

**声线**（全局 `rate: 4`）：

| 角色 | voice | 实际 rate / pitch | 字幕胶囊 / 字色 | 句数 |
| --- | --- | --- | --- | --- |
| 旁白 `zh` | zh-CN-XiaoxiaoNeural | +4% / +0 Hz | 无 / 白 | 15 |
| 小宇 `kid` | zh-CN-YunxiaNeural | +10% / +8 Hz | “小宇” / `#c9f1ff` | 5 |
| 太阳 `sun` | zh-CN-YunjianNeural | +4% / −4 Hz | “太阳” / `#ffe7a8` | 2 |
| 云朵 `cloud` | zh-CN-XiaoyiNeural | +8% / +4 Hz | “云朵”（白）/ `#e6ecff` | 1 |

## 4. 镜头与时间轴

| 场景 id | 时间（约） | 旁白要点 | 画面 | 模块 |
| --- | --- | --- | --- | --- |
| `globe` | 0–10.3 s | 为什么有的地方更热？太阳：我很公平；秘密在照射角度 | 地球仪，左侧射来两条“一样宽的阳光”：赤道一条照得正、受照范围小，高处一条照得斜、受照范围大 | `scenes-a.mjs` |
| `fair` | 10.3–19.6 s | 公平比较：同光束、同时长、同平地，只改照射角度 | 三行条件依次加锁（时长一行配转动的时钟），“照射角度”一行打开锁；背景切到网格“实验室” | `scenes-a.mjs` |
| `torch` | 19.6–31.8 s | 小宇用手电筒：正照小而亮，斜照大而暗；“光又没少，怎么变暗了？” | 透视地板，手电筒倾斜时光斑沿一个方向拉长，右侧亮度表下降 | `scenes-b.mjs` |
| `direct` | 31.8–42.2 s | 能量豆；接近垂直时 8 颗豆挤在较小面积上，每格 4 颗 | 垂直光束覆盖 2 块地砖，豆子正好在说“挤在”时落地，每格显示 4 | `scenes-c.mjs` |
| `slant` | 42.2–52.1 s | 更倾斜时摊在更大面积上，每格 2 颗；每平方米能量更少 | 30° 光束覆盖 4 块地砖，配角度标记和“面积大”括号 | `scenes-c.mjs` |
| `butter` | 52.1–56.9 s | 小宇：一小块黄油抹在大面包上，只剩薄薄一层 | 黄油 = 能量，面包 = 地面，盖章“薄薄一层！” | `scenes-d.mjs` |
| `factors` | 56.9–72.4 s | 角度不能单独决定气温；日照时长；云量；放在一起看 | 时钟图标、云朵角色出场说话，“实际有多热？这些条件一起看” | `scenes-d.mjs` |
| `recap` | 72.4–89.7 s | 其他条件相同时，越倾斜能量越摊开；太阳：是角度在分能量；小宇：被摊薄啦 | 左右对比：每格 4 颗 vs 每格 2 颗，右侧地砖的发热程度是左侧的一半，告别 | `scenes-e.mjs` |

## 5. 关键算法与实现

> 先区分两类内容：**视频讲的科学**：同一束平行光的截面宽度为 w，以高度角 h 照到平地上，落地宽度是 w / sin h，所以单位面积得到的能量与 sin h 成正比。**做视频用的算法**：按这个几何关系生成光束、豆子落点和地砖计数，并让豆子落地的时刻对准旁白里的词。下面的几何关系就是视频要讲的原理本身，代码按这个关系来画。

### 5.1 平行光束几何（`beam.mjs:23-32`）

```js
// A point at offset u across the beam lands at x = cx - u / sin(elev).
export function beamGeom(cx, gy, elev, w, len) {
  const d = [Math.cos(elev), Math.sin(elev)], n = [-Math.sin(elev), Math.cos(elev)];
  const sc = [cx - d[0] * len, gy - d[1] * len];
  const half = w / 2 / Math.sin(elev);
  return { d, n, w, len, elev, cx, gy, sc, f1: cx - half, f2: cx + half };
}
export const beamAt = (g, u, s) => [g.sc[0] + g.n[0] * u + g.d[0] * s, g.sc[1] + g.n[1] * u + g.d[1] * s];
export const landS = (g, u) => g.len - (Math.cos(g.elev) * u) / Math.sin(g.elev);
```

- `scenes-c.mjs:7-9` 的参数：地砖边长 `SQ = 160`，光束宽 `BW = 320`。垂直光束 `GV`（elev = π/2）落地宽 320，覆盖 2 块地砖；倾斜光束 `GS`（elev = π/6，即 30°）落地宽 320 / sin 30° = 640，覆盖 4 块地砖。8 颗豆分到每格分别是 4 颗和 2 颗，与 sin 30° = 0.5 的比例**精确一致**。
- `beanSlots()` 把 n 颗豆沿光束截面均匀排开，算出每颗的落点 `x = cx − u / sin(elev)` 和飞行距离 `L`（`beam.mjs:87-93`）。

### 5.2 豆子落地对准关键词（`scenes-c.mjs:11-14`）

```js
// Bean speeds chosen so the batch lands right on the spoken "挤在" / "摊在".
const fallV = (g, t0, t1) => Math.max(...beanSlots(g).map((b) => b.L)) / (t1 - t0);
const T_V0 = at('direct', '同样'), V_V = fallV(GV, T_V0, at('direct', '挤在'));
const T_S0 = at('slant', '同样'), V_S = fallV(GS, T_S0, at('slant', '摊在'));
```

速度不是凭感觉设的。做法是取最远一颗豆的飞行距离，除以“同样”到“挤在”或“摊在”之间的时长，反推出速度，这样这一批豆最晚的那颗正好在关键词说出时落地。落地后的弹跳用 `exp(−7Δt)·|cos 18Δt|` 做挤压回弹（`beam.mjs:96-108`）。

### 5.3 手电筒光斑：拉长与变暗（`scenes-b.mjs:64`）

```js
const rx = R0 / Math.sin(el), ry = R0 * FLAT, bright = Math.sin(el);
```

光斑长轴随 1/sin(el) 拉长，亮度随 sin(el) 降低，两者相乘后总量不变，这正好回答了小宇那句“光又没少，怎么变暗了？”。亮度表 `meter()` 显示同一个 `bright` 值（`scenes-b.mjs:110`）。

### 5.4 地球仪上的两条光带（`scenes-a.mjs:9-46`）

两条宽度都是 `BAND = 80` 的水平平行光带，分别打在地球仪 `y = EY`（正对）和 `y = EY − 0.82R` 处。`hitX(y) = EX − √(R² − (y−EY)²)` 用来裁出光带碰到球面的边缘，`litArc()` 用 `asin` 算出被照亮的弧段：照得斜的那一条，弧更长，也就是“受照范围大”。0.82R 大约对应 55° 纬度，这是**推断**，代码里没有标注纬度。

### 5.5 背景与共用机制

- 背景（`render.mjs:25-50`）：开场是星空。从 `fair` 到 `sum` 之间淡入间距 90 px 的网格“实验室”，星星同时变暗 65%。
- `recap` 场景并排放两组地砖，倾斜一侧的 `heat` 取左侧的 0.5 倍（`scenes-e.mjs:23-24`），和 sin 30° 一致。
- `tts.py` 相比 [[sunlight-through-space]] 只改了 docstring（加了 “a cloud” 角色）、`TITLE` 和 `TARGET`（90.0）。`lib/cast/props/sun/waves/audio/fonts` 与前几片完全相同。

## 6. 声音与字幕

- 四种声线的参数见 §3。`LEAD_IN 1.0`，各段 `gap` 0.25–0.5 s，`TAIL 2.8`。时间轴记录 `rate: 4`，片长 89.7 s，在 TARGET 90 s 以内，**没有提速**，只差约 0.3 s 就会触发提速。
- 混音：TTS 阶段为 `adelay → amix → highpass 80 → loudnorm I=-16/TP=-1.5/LRA=11`；成片阶段加入配乐（0.55，按旁白做 sidechain 闪避）和音效（0.5），最后过 `alimiter 0.93`。实际响度**未记录**。
- 缓存：`build/tts_cache/` 共 46 个文件，正好是 23 段 × 2。
- 字幕：时间取自逐词时间戳；`sum` 句手动分成 5 条 caption。从 `sum` 开始，页脚淡入“资料：NASA Earth Observatory · NOAA NESDIS”。

## 7. 迭代与问题

1. **没有出现“太阳高度角”这个术语**（见 §2）。README 简介用了这个词，成片只说“照射角度”，和 AGENTS.md §6“比喻之后给出教材正式说法”不一致。
2. **没有片尾品牌卡**：`tts.py:35` 的注释写着“room for the closing brand card”，但没有 `outro()`；`recap` 场景 `to: Infinity`，结尾是“摊薄啦！”印章，接着淡出到黑。这不符合现行 AGENTS.md §4。
3. **`script.json` 缺少字段**：`curriculum`、`based_on`、`numbers`（§6）。
4. **`TARGET = 90.0` 和片长贴得很近**：片长 89.69 s，离阈值只有约 0.3 s。如果文案再加一点，就会按旧逻辑自动提速，而现行 §2 规定默认 `TARGET = None`、不靠提速压时长。
5. **地球仪示意**：代码里没有画地轴、纬线或指向标（本次只读了代码，没有看画面，**无法确认**画面是否另有标注）。如果要用于课堂，可以对照 AGENTS.md §6 的“地理图示规范”再核查一遍。
6. **符合 §5**：画面没有“示意”字样。
7. README 目录已收录（第 35 行，1:30）。

## 8. 复现入口

本次**未运行**。以下顺序来自 AGENTS.md 的常用命令和文件头注释：

```bash
python3 -m venv .venv && .venv/bin/pip install edge-tts   # fonts.py 另需 fonttools（未声明）
npm install                                               # @napi-rs/canvas（lock 1.0.13）
.venv/bin/python tts.py            # → build/seg*.mp3, narration.wav, timeline.json；out/为什么同样晒太阳有的地方更热.srt
node render.mjs --still 40 47      # → build/still_<t>.png（直射 / 斜射对比）
node render.mjs                    # → build/music.wav, sfx.wav；out/为什么同样晒太阳有的地方更热.mp4
```

字体依赖 macOS 系统资源。

## 9. 参考来源

`script.json` 和代码里**都没有 URL**（证据包 `urls` 为空）。资料名来自 `source` 字段：

- NASA Earth Observatory: Climate and Earth's Energy Budget，入射角与单位面积能量（项目文件中提及，本次未访问）
- NOAA NESDIS / NASA Space Place: How Do Clouds Affect Solar Energy?，云量对到达地面阳光的影响（项目文件中提及，本次未访问）
- 分镜来源：`geo-story-studio/storage/builds/douyin-sun-angle/v002/`（路径为推断，本次未访问）

## 10. 证据与限制

| 文件 | sha256 前 12 位 | 说明 |
| --- | --- | --- |
| `script.json` | `5ae7e52edfce` | 23 段，四种声线 |
| `tts.py` | `44673df9ab7e` | TARGET 90 |
| `render.mjs` | `559d7eebc1cf` | 实验室网格背景，云朵说话时的字幕胶囊 |
| `beam.mjs` | `22585d012f25` | 光束几何、能量豆、地砖、手电筒、锁、时钟 |
| `scenes-a…e.mjs` | `ced390f24470` / `c9527312f91e` / `c9dcb6021cfe` / `efa8a62a94cf` / `4b00a6cfb226` | 8 个场景 |
| `sun.mjs` / `waves.mjs` | `210e265385b4` / `dca3d3afaeac` | 与 [[sunlight-through-space]] 相同 |
| `lib/cast/props.mjs` | `7d757aa5fc33` / `e7f29e66c6d0` / `4debdbd1b652` | 与 [[sun-energy]] 相同 |
| `audio.mjs` / `fonts.py` | `f9e04b2cfd54` / `a6ac7264e548` | 5 片相同 |

- **已读**：`script.json` 全文，`tts.py` 和 `render.mjs` 的 diff，`beam.mjs` 的关键函数，各场景文件的结构和关键段落，`timeline.json` 中各段的时间。
- **未读 / 未做**：媒体文件、依赖目录、缓存内容；没有核对源 build 和外部资料；没有看过地球仪画面。
- **缺失**：制作会话、需求原文、审片记录、发布状态、模型信息、ffmpeg 版本、实测响度。
