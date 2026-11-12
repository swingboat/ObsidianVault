---
type: video-production-dossier
project: geo-story-studio-opus
video_id: seawater-temperature
title: 海水越深越冷，是真的吗？
curriculum: 人教版必修第一册 第三章第二节 海水的性质（海水温度的垂直变化，图3.8）；课标 1.6
captured: '2026-11-12'
status: 成片存在（竖屏 1080×1920，282.8 s）；技术检查/视觉检查/用户验收未记录
source_fingerprint: 0c1ac6e17d6b314355128fa2cf341ee4421a649cf6cac54402a9ad04ae514d60
tags:
- video-creation
- production-capture
- geography-teaching
- canvas-frame-render
- edge-tts
---

# 海水越深越冷，是真的吗？ · “探温号”下潜 + 剖面与温度—深度曲线同步绘制的解析模型动画

<!-- capture-source-sha256:0c1ac6e17d6b314355128fa2cf341ee4421a649cf6cac54402a9ad04ae514d60 -->

本文是**根据项目文件做的材料回溯**（`script.json`、`tts.py`、`render.mjs`、`lib.mjs`、7 个场景模块与 `dive/chars/cast/props/audio.mjs`、`build/timeline.json`、证据包的 ffprobe 与 sha256 清单）。证据包映射到宿主会话 `d4f63b22…`（创建于 2026-11-01 06:29），只按元数据找到，宿主返回空 transcript，**对话内容不可读**；映射依据是会话创建时间早于 `script.json` 数分钟，属**推断**。本次未运行任何项目脚本，也未打开音视频。

## 1. 当前结论与制作路线

**状态：成片存在**——`out/海水越深越冷，是真的吗.mp4` + 同名 `.srt`。只有 ffprobe 媒体头信息；画面、试听、响度、用户验收与发布都**未记录**。

| 项 | 值 | 来源 |
| --- | --- | --- |
| 分辨率 / 画幅 | 1080×1920，9:16 竖屏 | ffprobe；`lib.mjs:7` |
| 帧率 / 帧数 | 30 fps；`round(282.782×30)` = 8483 帧 | ffprobe；`render.mjs:181`（推算） |
| 时长 | 282.767 s（timeline 282.782 s） | ffprobe；`build/timeline.json` |
| 视频编码 | H.264，yuv420p，CRF 19，preset medium，`+faststart` | ffprobe；`render.mjs:178-179` |
| 音频编码 | AAC 48 kHz 立体声，192 kbps | ffprobe；`render.mjs:179` |
| 文件大小 | 31,243,646 字节（约 31.2 MB） | 证据包 |
| 字幕 | `.srt` 147 条，8820 字节；画面烧录字幕 + 角色标签 | 证据包；`tts.py`；`render.mjs:82-108` |
| 其他版本 | 无 | `out/` 列表 |

文件时间：`.srt` 2026-11-01 06:53，成片 07:14。git 只记录 `8cbe673 2026-11-01 feat(videos): add 21 high-school geography lessons`。README 目录第 81 行收录（竖屏，4:43）。

一句话管线：

> `script.json`（47 段，4 个声线角色）→ `tts.py`（edge-tts + WordBoundary）→ `timeline.json` / `narration.wav` / SRT → `render.mjs` 并行加载 `scenes-a…g.mjs`（9 个场景，`scenes-c` 只提供叠加层），共享 `dive.mjs` 的“海水柱 + 温度曲线”画板 → `audio.mjs` 合成配乐音效 → RGBA 帧管道送 ffmpeg。

## 2. 教学目标与课标对齐

- **curriculum**：人教版必修第一册 第三章第二节 海水的性质（海水的温度：垂直变化，图 3.8 海水温度随深度的变化）；课标 1.6 运用图表等资料，说明海水性质和运动对人类活动的影响。
- **核心知识点**：海水温度的垂直变化规律及成因；“越深越冷”不是铁律。
- **讲解结构（按实际文案）**：
  1. 导入：小林潜水“往下一扎，突然一阵凉”，追问“一直冷到海底结冰？”（`ask1`、`ask2`），老师定调“大体越深越冷，但冷得有快有慢，还藏着例外”。
  2. 原理拆解：探温号在低纬太平洋下潜——表层被阳光加热 → 混合层（风浪搅匀）→ 温跃层（读数“坐滑梯”，暖轻水在上、冷重水在下）→ 1000 米以下几乎不动、海底不结冰。
  3. 结论：教材规律“一般随深度增加而降低；1000 米以内变化大，以下变化小”，并补一句“1000 米是这条低纬曲线的大致位置”（`ruleNote`）。
  4. 成因深化：深层水“冰冰”自述高纬冷却、结冰析盐、下沉、几百年到上千年流到热带深层。
  5. 迁移：低纬 / 中纬（夏、冬）/ 极地三条曲线；反转——北极冷淡表层下有 0 ℃ 以上、更咸更重的大西洋水“阿特”，得出“密度由温度和盐度共同决定”。
  6. 总结：考场答题模板（规律、原因、易错点）、Argo 浮标、一句话总结。
- **科学边界**：`source` 写明改编自 Geo Story Studio `douyin-seawater-temperature v002` 的分镜与科学审校（推断路径 `geo-story-studio/storage/builds/douyin-seawater-temperature/v002`，本次未读取）。文案中的限定：混合层厚度“要看海区和季节”；1000 米不是全球统一分界；“阳光照不到只是一半原因”；北极只说“部分海域”。深层水来源、北极大西洋水属教材外拓展，文案以教材规律为主线——与 AGENTS.md 第 6 节“以教材为主线、必要时补一句说明”一致。
- **事实与数字**（`facts` 9 条、`numbers` 4 条，出处为文字引用，无 URL）：

| 表述 | 出处（script.json 写法） |
| --- | --- |
| 随深度降低；1000 米以内变化大（图 3.8 为太平洋 170°W 低纬某站） | 人教版必修一 P50—51 图 3.8 |
| 混合层、温跃层、深层缓变；分层稳定 | NOAA What is a thermocline?；CSIRO Report 221；Univ. of Hawaii |
| 高纬冷却结冰、变咸下沉沿深层流动 | NOAA Thermohaline Circulation 等 |
| 低纬表层二十多 ℃、深层几 ℃ | 教材图 3.8 曲线形态 |
| 输送带一圈约 1000 年（文案“几百年甚至上千年”） | NOAA The Global Conveyor Belt |
| 北极表层约 −1.8 ℃，大西洋水层 > 0 ℃，核心约 300—500 米 | NOAA Arctic Report Card 2025；Polyakov 等；Ocean Science 19, 1453 (2023) |
| 全球约 4000 个 Argo 浮标，测到约 2000 米 | Argo Program；NOAA GOMO |

以上出处本次均未核对。

## 3. 技术与依赖

| 类别 | 内容 | 证据 |
| --- | --- | --- |
| 渲染 | Node.js ESM + `@napi-rs/canvas` `^1.0.11`（lock 解析 1.1.0）；无 npm scripts | `package.json`；`package-lock.json` |
| TTS | Python `edge-tts` + `aiohttp`；版本未记录 | `tts.py` |
| 字体 | `fonts.py`（fontTools，抽取圆体 / 苹方）；`lib.mjs:12` 缺字体时自动调用 | `fonts.py`；`lib.mjs` |
| 编码 / 混音 | 系统 ffmpeg；版本未记录 | `tts.py:185`；`render.mjs:170-180` |

声线（全局 `rate: 4` 叠加，`timeline.json` 记录 `rate: 4`）：

| key | 标签 | voice | 角色 rate / pitch | 实际 rate | 段数 |
| --- | --- | --- | --- | --- | --- |
| `zh` 旁白 | — | `zh-CN-XiaoxiaoNeural` | 0 / 0 | +4% | 31 |
| `stu` | 小林 | `zh-CN-YunxiNeural` | +6 / +2 Hz | +10% | 10 |
| `cold` | 深层水 冰冰 | `zh-CN-XiaoyiNeural` | +4 / +3 Hz | +8% | 4 |
| `atl` | 大西洋水 阿特 | `zh-CN-YunjianNeural` | −4 / −2 Hz | 0% | 2 |

## 4. 镜头与时间轴

时间来自 `timeline.json`；场景边界为“某段开始前 0.2–0.3 s”，交叉淡化 `XF = 0.4`（`render.mjs:17`）。7 个模块、9 个场景。

| # | 场景 id | 约时间 (s) | 口播要点 | 画面 | 模块 |
| --- | --- | --- | --- | --- | --- |
| 1 | `beach` | 0–10.1 | 小林潜水“突然一阵凉” | 浮潜小林，“暖洋洋 / 好凉！”标签 | `scenes-a.mjs` |
| 2 | `title` | 10.1–22.5 | 定调 + “坐上探温号” | 标题卡“海水越深越冷 / 是真的吗？”，探温号 | `scenes-a.mjs` |
| 3 | `dive` | 22.5–107.2 | 表层、混合层、温跃层、深层、教材规律、1000 米说明 | 左海水柱随温度变色，右温度—深度曲线随下潜画出；深度轴 300→1200→4000 m 缩放 | `scenes-b.mjs` + `scenes-c.mjs` 叠加 |
| 4 | `dark` | 107.2–119.4 | 深海为什么冷？阳光只是一半原因 | 阳光随深度衰减 | `scenes-d.mjs` |
| 5 | `origin` | 119.4–160.0 | 冰冰：高纬冷却、结冰析盐、“咕咚”下沉、几百年流到热带 | 南北向大洋剖面，盐晶、海冰 | `scenes-d.mjs` |
| 6 | `compare` | 160.0–191.0 | 低纬 / 中纬夏冬 / 极地曲线 | 同一坐标系三类曲线，中纬夏↔冬过渡 | `scenes-e.mjs` |
| 7 | `arctic` | 191.0–229.6 | 反转：北极冷淡表层下的暖咸大西洋水 | 左剖面（河水 + 融冰、暖层虚线带），右 −2…3 ℃ 剖面曲线 | `scenes-f.mjs` |
| 8 | `exam` | 229.6–253.7 | 答题模板：规律 / 原因 / 易错点 | 三栏答题板 | `scenes-g.mjs` |
| 9 | `argo` + `finale` | 253.7–282.8 | Argo 浮标；小林总结；一句话总结 | 浮标上下沉浮画曲线；280.2 s 起品牌卡“用地理的眼光，读懂大海的冷暖” | `scenes-g.mjs`；`render.mjs:63-77` |

## 5. 关键算法与实现

**视频讲的原理**：太阳辐射加热表层、风浪混合、温跃层稳定分层、高纬冷咸水下沉、温度和盐度共同决定密度。**做视频用的算法**如下。

### 5.1 解析温度剖面 + 共享深度轴的“下潜画板”（`dive.mjs:7-34`）

左侧海水柱与右侧曲线共用 `depthY(z, zMax)`，探温号与曲线读数在同一深度对齐。低纬剖面不是数据点，而是按教材图 3.8 形态拟合的解析式：

```js
// ~28 ℃ mixed layer to ~60 m, steep fall through the thermocline, a few ℃ below ~1000 m
export function T(z) {
  if (z <= 60) return 28 - 0.1 * (z / 60) ** 2;
  return 1.4 + 4.0 * Math.exp(-(z - 60) / 1300) + 22.5 * Math.exp(-(z - 60) / 200);
}
```

`tempColor(T)` 把温度映射成水色（< 6 ℃ 深蓝 → 16 ℃ 青 → 28 ℃ 橙），海水柱按深度逐行上色。曲线数值为造型用，非实测。

### 5.2 关键帧缓动的镜头缩放（`scenes-b.mjs:12-22`；`dive.mjs:30-34`）

`kf(t, v0, keys)` 依次对 `[t0, t1, value]` 做 `easeIO` 插值。深度轴 `zMax` 在“温跃层”一词时从 300 m 拉到 1200 m，在 `deep` 段拉到 4000 m；探温号深度 `z` 在“风吹浪打”“穿过”“再往下”等词上推进到 55 → 250 → 1000 → 3800 m。镜头节奏完全由逐词时间戳决定。

### 5.3 对比曲线与北极反转剖面（`scenes-e.mjs:7-11`；`scenes-f.mjs:13-17`）

```js
const summer = (z) => (z <= 30 ? 22 : 3.5 + 11.5 * Math.exp(-(z - 30) / 450) + 7 * Math.exp(-(z - 30) / 40));
const winter = (z) => (z <= 150 ? 14.6 - 0.1 * (z / 150) : 3.5 + 11 * Math.exp(-(z - 150) / 420));
const polar = (z) => 1.3 - 0.5 * (z / ZMAX);
// Arctic: near-freezing surface, > 0 ℃ Atlantic Water core near 400 m, cooler below
export function arcticT(z) {
  const base = -1.8 + 1.0 * clamp((z - 300) / 700);
  return base + 3.3 * Math.exp(-(((z - 420) / 200) ** 2)) * clamp(z / 120);
}
```

低纬曲线直接复用 `dive.mjs` 的 `T`（`import { T as lowT }`）；中纬冬季把混合层加深到 150 m，表现“混合变深、温跃层变弱”。北极用高斯峰表示约 420 m 的暖核，与 `numbers` 的“核心约 300—500 米”对应。

### 5.4 叠加层拆文件（`scenes-c.mjs:59-99`；`scenes-b.mjs:8, 133, 155`）

`dive` 场景跨 22.5–107.2 s，内容很多。`scenes-c.mjs` 不导出场景（`scenes = []`），只导出 `deepOverlay()` 与 `deepEvents`，由 `scenes-b.mjs` 在同一画板上调用——一个长场景拆成两个文件，符合 AGENTS.md 第 3 节“单文件约 300 行以内”（意图为**推断**）。

### 5.5 TTS、切场与品牌（`tts.py`；`render.mjs:20, 48-77, 121-153`）

- `tts.py` 与 [[sea-to-mountain-rain]] 同源：WSS_URL 运行时替换、`sha1(voice|rate|pitch|text)` 缓存、429 退避 5 次、`TARGET = None`。差异：`LEAD_IN 0.8`、`TAIL 2.8`；不支持 `["显示","口播"]` 字幕对和角色 `fx`。
- 切场只做透明度交叉淡化（无缩放、无震屏）；`render.mjs:149-153` 给每个场景切换自动加一个 whoosh，品牌卡加一个 sparkle。
- 角标 logo：`x=64, y=76, w=290`，0.3–1.0 s 淡入；`OUTRO = E('sum') + 0.3`（≈280.2 s）起 0.4 s 淡出。品牌卡压暗背景（0.84），760 px logo 0.9→1 放大，标语“用地理的眼光，读懂大海的冷暖”（与 `script.json` `tagline` 相同），字幕同时停止；末 0.6 s 淡到黑。

## 6. 声音与字幕

- **旁白**：47 段约 1342 字；`LEAD_IN 0.8 s`，默认段间 0.45 s。混音链 `aresample → adelay → amix(normalize=0) → highpass 80 Hz → loudnorm I=-16:TP=-1.5:LRA=11`，输出 `build/narration.wav`。
- **配乐**（`audio.mjs:172-`）：程序合成，112 BPM，C–G–Am–F 拨弦 + 低音 + 沙锤。
- **音效**：在通用 FX 外新增水下“咕噜”上滑音、带两次回声的声呐 ping、“咕咚”下沉、温跃层下滑哨（`audio.mjs:140-160` 注释）。7 个模块约 86 个事件，加自动 whoosh。
- **总混音**（`render.mjs:170-176`）：配乐 ×0.5 + 旁白侧链压缩，音效 ×0.5，`alimiter=0.93`。配乐比 [[sea-to-mountain-rain]]（×0.75）更轻。
- **字幕**：`SUB_Y = 1560`，起始字号 56，按宽度递减；小林 / 冰冰 / 阿特各有颜色标签（`render.mjs:79-80`），品牌卡出现后不再画字幕。底部资料行“资料：人教版地理必修第一册 · NOAA · Met Office · Argo”从 `stuEnd` 开始显示。

## 7. 迭代与问题

- **模板沿用痕迹**：`props.mjs` 头注释写“pressure badges, circulation loops, dials and thermometers”，`cast.mjs` 含 `airBlob`、`moon`、`stars`——都是大气类题材的部件，推断从本项目更早的大气视频复制而来；本片实际用到的只是其中一部分（**推断**）。`audio.mjs` 与 [[seawater-salinity]] 完全相同（sha256 `b101c258f28d`），连温跃层下滑哨也一起带过去了。
- **全局语速 +4%**：`rate: 4`、`TARGET = None`；AGENTS.md 第 2 节要求正常语速，原因**未记录**。
- **没有 `based_on` 字段**：改编来源写在 `source` 文本里，而 AGENTS.md 第 1 节要求源路径写进 `based_on`；也没有 URL。
- **遵守 AGENTS 的证据**：`render.mjs:110` 注释“no ‘示意’ disclaimers on screen”，代码中未搜到“示意 / 不按比例”；品牌卡、logo 位置与淡出、`TAIL 2.8` 与第 4 节一致。`build/` 中无残留 `still_*.png`。
- **核心知识点范围**：主线是“温度垂直变化”，但纳入了温盐环流与北极大西洋水两段拓展（约 70 s）；是否超出“一个视频只讲透一个知识点”的尺度未见讨论记录。
- 宿主会话不可读，修改轮次与用户反馈**无法确认**。

## 8. 复现入口

本次**均未运行**。按 AGENTS.md“常用命令”（在 `seawater-temperature/` 内）：

```bash
python3 -m venv .venv && .venv/bin/pip install edge-tts   # fonts.py 另需 fontTools（AGENTS 未列出）
npm install                                               # @napi-rs/canvas；package.json 无 scripts
.venv/bin/python tts.py            # → build/seg*.mp3、tts_cache/、narration.wav、timeline.json；out/<标题>.srt
node render.mjs --still 60 200     # → build/still_<t>.png 预览（用完删除）
node render.mjs                    # → build/music.wav、sfx.wav；out/海水越深越冷，是真的吗.mp4
```

## 9. 参考来源

`script.json` **没有 URL**，只有文字出处：人教版地理必修第一册 P50—52 图 3.8；星球地图出版社配套图册 P23—24；NOAA National Ocean Service《What is a thermocline?》《Why does the ocean get colder at depth?》《Thermohaline Circulation》《The Global Conveyor Belt》；CSIRO Report 221《Quality Control Cookbook for XBT Data》；Met Office《Sea ice: an overview》；NOAA Arctic Report Card 2025《Atlantification of the Arctic Ocean》；Polyakov 等；Ocean Science 19, 1453 (2023)；Argo Program（argo.ucsd.edu）与 NOAA Global Ocean Monitoring。均为**项目文件中提及，本次未访问**。改编来源：Geo Story Studio `douyin-seawater-temperature v002`（分镜与科学审校）。

## 10. 证据与限制

| 文件 | 行数 | sha256 前 12 位 | 用途 |
| --- | --- | --- | --- |
| `script.json` | 87 | `0aca78e8fd6c` | 全文已读 |
| `tts.py` | 209 | `ffc51c2f7747` | 与 sea-to-mountain-rain 版逐行 diff |
| `render.mjs` | 192 | `e9c99c90f874` | 全文已读 |
| `dive.mjs` | 182 | `37cd4b385513` | 剖面模型、画板 |
| `scenes-a…g.mjs` | 142/157/100/211/113/154/164 | `ad8d99ba6f31` / `59137f22b0db` / `e385dc1a3e4a` / `978f008bdc6c` / `c88a399884fc` / `fcb62af2ff9b` / `5c27324e1949` | 头注释、场景表、关键函数 |
| `chars.mjs` / `cast.mjs` / `props.mjs` / `audio.mjs` | 206/204/174/227 | `2b8f74f1f7cc` / `4eaa4e811cae` / `9a53efe20fc3` / `b101c258f28d` | 导出清单、注释 |
| `lib.mjs` | 225 | `747d02f48593` | 字体、`BUILD_DIR`、时间轴 |

- 已读：上表（大文件按段）、`build/timeline.json`、`package-lock.json`。
- 未读 / 未做：`node_modules/`、`.venv/`、`tts_cache`、音视频内容；未运行脚本；未访问外部资料；未读取源 build。
- 缺失：会话内容、需求原文、验收记录、工具版本、成片实测响度。
