---
type: video-production-dossier
project: geo-story-studio-opus
video_id: seawater-salinity
title: 最咸的海水，为什么不在赤道？
curriculum: 人教版必修第一册 第三章第二节 海水的性质（海水的盐度）；关联选择性必修1 气压带和风带；课标 1.6
captured: '2026-11-12'
status: 成片存在（竖屏 1080×1920，249.8 s）；技术检查/视觉检查/用户验收未记录
source_fingerprint: d6b88a0b517428e5ef91222d34a7ac46945dd95761b58a2f7e9e8c23c86c9f40
tags:
- video-creation
- production-capture
- geography-teaching
- canvas-frame-render
- edge-tts
---

# 最咸的海水，为什么不在赤道？ · “淡水账”主线 + 两杯算例 + 经纬度描摹定位图的盐度讲解动画

<!-- capture-source-sha256:d6b88a0b517428e5ef91222d34a7ac46945dd95761b58a2f7e9e8c23c86c9f40 -->

本文是**根据项目文件做的材料回溯**（`script.json`、`tts.py`、`render.mjs`、`lib.mjs`、7 个场景模块与 `seas/chars/cast/props/audio.mjs`、`build/timeline.json`、证据包的 ffprobe 与 sha256 清单）。证据包映射到宿主会话 `3ce2196d…`（创建于 2026-11-01 07:16），只按元数据找到，宿主返回空 transcript，**对话内容不可读**；映射依据是会话创建时间早于 `script.json` 数分钟，属**推断**。本次未运行任何项目脚本，也未打开音视频。

## 1. 当前结论与制作路线

**状态：成片存在**——`out/最咸的海水，为什么不在赤道.mp4` + 同名 `.srt`。只有 ffprobe 媒体头信息；画面、试听、响度、用户验收与发布都**未记录**。

| 项 | 值 | 来源 |
| --- | --- | --- |
| 分辨率 / 画幅 | 1080×1920，9:16 竖屏 | ffprobe；`lib.mjs:7` |
| 帧率 / 帧数 | 30 fps；`round(249.82×30)` = 7495 帧 | ffprobe；`render.mjs:181`（推算） |
| 时长 | 249.833 s（timeline 249.820 s） | ffprobe；`build/timeline.json` |
| 视频编码 | H.264，yuv420p，CRF 19，preset medium，`+faststart` | ffprobe；`render.mjs:178-179` |
| 音频编码 | AAC 48 kHz 立体声，192 kbps | ffprobe；`render.mjs:179` |
| 文件大小 | 32,260,393 字节（约 32.3 MB） | 证据包 |
| 字幕 | `.srt` 129 条，7700 字节；画面烧录字幕 + 角色标签 | 证据包；`render.mjs:82-108` |
| 其他版本 | 无 | `out/` 列表 |

文件时间：`.srt` 2026-11-01 07:23，成片 07:42。git 只记录 `8cbe673 2026-11-01 feat(videos): add 21 high-school geography lessons`。README 目录第 82 行收录（竖屏，4:10）。

一句话管线：

> `script.json`（42 段，4 个声线角色）→ `tts.py`（edge-tts + WordBoundary）→ `timeline.json` / `narration.wav` / SRT → `render.mjs` 加载 `scenes-a…g.mjs`（10 个场景）→ `audio.mjs` 合成配乐音效 → RGBA 帧管道送 ffmpeg。`render.mjs`、`tts.py`、`lib.mjs` 与 [[seawater-temperature]] 是同一模板，只改了标题、背景色、角色标签、标语、资料行与 `OUTRO` 锚点。

## 2. 教学目标与课标对齐

- **curriculum**：人教版必修第一册 第三章第二节 海水的性质（盐度概念、世界表层盐度分布规律、影响因素，红海与波罗的海活动）；关联选择性必修 1 第三章第一节 气压带和风带（赤道低压上升气流、副热带高压下沉气流）；课标 1.6。
- **核心知识点**：表层盐度的纬度分布规律及其成因——“蒸发与降水的淡水收支”。
- **讲解结构（按实际文案）**：
  1. 导入：小林“汤越熬越咸，所以赤道最咸”（`ask1`、`ask2`）→ 揭晓“不在赤道，在副热带”（`hook2`）。
  2. 概念：盐度 = 溶解盐类质量 ÷ 海水总质量；盐粒“阿咸”：溶在水里、不是海底一堆盐；平均约 35‰。
  3. 原理：两杯实验——晒走 125 克水 → 35 ÷ 875 = 40‰；加 250 克淡水 → 35 ÷ 1250 = 28‰；“盐没变，变的是水”。
  4. 应用到全球：大海的淡水账；赤道上升气流、对流雨多，降水 > 蒸发被稀释；副热带下沉气流、蒸发 > 降水变咸。
  5. 结论：纬度—盐度曲线成“双峰骆驼”，回扣教材规律“从副热带海区向南北两侧递减”。
  6. 迁移：红海（沙漠、几无河流、强蒸发，经曼德海峡补水、下层更咸水流出）与波罗的海（河流多、蒸发弱，约 7‰，经丹麦附近海峡交换）——“窄海峡不决定咸淡，交换少只放大结果”。
  7. 总结：一句话总结 + 答题关键词（降水量与蒸发量、河流注入、洋流、海区封闭程度）+ 易错点 + 煲汤加水彩蛋。
- **科学边界**：`source` 写明改编自 Geo Story Studio `douyin-seawater-salinity v002` 分镜与科学审校（推断路径 `geo-story-studio/storage/builds/douyin-seawater-salinity/v002`，本次未读取）。`notes` 字段记录两条措辞决定：①中文地图上的“丹麦海峡”指格陵兰岛与冰岛之间的海峡，不是波罗的海出口，所以旁白改说“丹麦附近几条狭窄的海峡”（厄勒、大贝尔特、小贝尔特）；②不给红海、波罗的海做“世界最高 / 最低”的绝对排名。
- **事实与数字**（`facts` 8 条、`numbers` 4 条）：

| 表述 | 出处（script.json 写法） |
| --- | --- |
| 盐度定义（质量分数，千分数） | 人教版必修一 P52；NASA Salinity Explained |
| 大洋平均盐度约 35‰ | 人教版必修一 P52；SYKE Hydrography |
| 1000 克算例 → 40‰ / 28‰ | 按定义自设算例（非实测），沿用 v002 分镜 shot-02 |
| 表层盐度从副热带向南北两侧递减；赤道较淡 | 教材 P53 及图册；ESA Sensing salinity |
| 赤道上升气流多对流雨；副热带下沉气流降水少 | 人教版选择性必修 1 第三章第一节 |
| 红海几无河流、强蒸发；经曼德海峡补水、下层流出 | 教材 P54 活动；WHOI Mysteries of the Red Sea |
| 波罗的海平均约 7‰，丹麦诸海峡表层约 20‰，北部 2‰—0‰ | SYKE Baltic Sea portal Hydrography |
| 红海表层水温可超过 30 ℃ | WHOI（旁白未直接使用） |

以上出处本次均未核对。

## 3. 技术与依赖

| 类别 | 内容 | 证据 |
| --- | --- | --- |
| 渲染 | Node.js ESM + `@napi-rs/canvas` `^1.0.11`（lock 解析 1.1.0）；无 npm scripts | `package.json`；`package-lock.json` |
| TTS | Python `edge-tts` + `aiohttp`；版本未记录 | `tts.py` |
| 字体 | `fonts.py`（fontTools）；`lib.mjs` 缺字体时自动调用 | `fonts.py`；`lib.mjs` |
| 编码 / 混音 | 系统 ffmpeg；版本未记录 | `tts.py:185`；`render.mjs:170-180` |

声线（全局 `rate: 4` 叠加；`timeline.json` 记录 `rate: 4`）：

| key | 标签 | voice | 角色 rate / pitch | 实际 rate | 段数 |
| --- | --- | --- | --- | --- | --- |
| `zh` 旁白 | — | `zh-CN-XiaoxiaoNeural` | 0 / 0 | +4% | 30 |
| `stu` | 小林 | `zh-CN-YunxiNeural` | +6 / +2 Hz | +10% | 8 |
| `salt` | 盐粒 阿咸 | `zh-CN-YunxiaNeural` | +4 / 0 | +8% | 3 |
| `drop` | 水滴 滴滴 | `zh-CN-XiaoyiNeural` | +8 / +4 Hz | +12% | 1 |

## 4. 镜头与时间轴

时间来自 `timeline.json`；`XF = 0.4`。7 个模块、10 个场景。

| # | 场景 id | 约时间 (s) | 口播要点 | 画面 | 模块 |
| --- | --- | --- | --- | --- | --- |
| 1 | `kitchen` | 0–14.2 | 煲汤越熬越咸 → 赤道最咸？ | 灶台汤锅、蒸汽 | `scenes-a.mjs` |
| 2 | `arena` | 14.2–25.2 | 揭晓：最咸在副热带 | “咸度擂台”：南副热带 / 赤道附近 / 北副热带三根柱，两侧戴皇冠 | `scenes-a.mjs:84-` |
| 3 | `definition` | 25.2–51.0 | 盐度定义；阿咸；平均约 35‰ | 定义公式卡，阿咸登场 | `scenes-b.mjs` |
| 4 | `lab` | 51.0–95.1 | 两杯实验 40‰ / 28‰；盐没变，变的是水 | 两只烧杯，盐点数恒为 5，克数滚动；滴滴蒸发飞走 | `scenes-c.mjs` |
| 5 | `ledger` | 95.1–105.1 | 大海的淡水账 | 存（降水、河流）/ 取（蒸发）账本 | `scenes-d.mjs:20-65` |
| 6 | `budgets` | 105.1–136.1 | 赤道 vs 副热带收支 | 两块收支面板（云雨 / 晴空、天平） | `scenes-d.mjs:66-153` |
| 7 | `curve` | 136.1–161.7 | 双峰骆驼曲线；高纬递减；教材规律 | 60°S–60°N 盐度曲线 | `scenes-e.mjs` |
| 8 | `seas` | 161.7–216.6 | 红海 vs 波罗的海；窄海峡不决定咸淡 | 两张经纬度描摹的定位图（上北、指北针），河流虚线、海峡水团交换 | `scenes-f.mjs` + `seas.mjs` |
| 9 | `summary` | 216.6–240.0 | 一句话总结、关键词、易错点 | 总结卡与关键词 | `scenes-g.mjs` |
| 10 | `soup` | 240.0–249.8 | 小林：汤咸了加点水 | 汤锅回扣；247.2 s 起品牌卡“用地理的眼光，算清大海的咸淡账” | `scenes-g.mjs`；`render.mjs:63-77` |

## 5. 关键算法与实现

**视频讲的原理**：盐度 = 盐 ÷ 海水总质量；盐量不变、水量变化决定浓淡；气压带控制蒸发与降水收支；河流注入与海区封闭程度放大区域差异。**做视频用的算法**如下。

### 5.1 “盐不变”的两杯算例（`scenes-c.mjs:30-65`；`chars.mjs:118-135`）

克数随口播滚动：

```js
const gL = Math.round(lerp(1000, 875, easeIO(prog(t, tEv + 0.3, tEvD))));
const gR = Math.round(lerp(1000, 1250, easeIO(prog(t, tRain + 0.3, tRainD))));
```

两杯里都画 `saltDots(..., 5, ...)`——注释写明“the count never changes in the lab scenes”，盐点个数始终是 5，水位变化，用画面守住“盐没变，变的是水”。结果卡 `35 ÷ 875 = 40‰`、`35 ÷ 1250 = 28‰` 在“千分之四十”“千分之二十八”一词出现时弹出。

### 5.2 纬度—盐度曲线（`scenes-e.mjs:9-17`）

曲线用高斯项叠加出“双峰骆驼”，不是实测数据：

```js
function sal(lat) {
  const g = (c, w) => Math.exp(-(((lat - c) / w) ** 2));
  return 34.0 + 1.6 * g(22, 18) + 1.5 * g(-20, 18) - 0.15 * g(4, 7) - 0.3 * Math.max(0, (Math.abs(lat) - 35) / 25);
}
```

两峰在约 22°N 与 20°S，赤道附近有小凹陷，|纬度| > 35° 后线性下降；纵轴取 33.5–35.8‰。

### 5.3 按经纬度描摹的定位图（`seas.mjs:1-43`）

红海、波罗的海（含西兰岛、卡特加特 / 斯卡格拉克到北海）用手工 `[lon, lat]` 折线描摹海岸（头注释“traced roughly from real coastlines (lon/lat), north up”），再用两套线性投影：

```js
export const redProj = (x0, y0) => ([lon, lat]) => [x0 + (lon - 32) * 23, y0 + (30.5 - lat) * 23];
export const balProj = (x0, y0) => ([lon, lat]) => [x0 + (lon - 7.5) * 20, y0 + (66.2 - lat) * 34];
```

波罗的海经向 20 px/°、纬向 34 px/°，相当于在约 54° 纬度上做了 cos φ 压缩；红海接近赤道，用 1:1。两张图都画 `compass()` 指北针（“N”），符合 AGENTS.md 第 6 节“上北下南、有指向标”。`riverLine` 画流动的河流虚线，`parcels` 让带盐点的水团沿海峡往返，表现海水交换。

### 5.4 模板与时间轴（`render.mjs`；`tts.py`）

与 [[seawater-temperature]] 共用：逐词时间戳 `at(id, '词')` 驱动所有弹出；`kf` 关键帧；场景透明度交叉淡化 `XF 0.4`；每次切场自动 whoosh；`OUTRO = E('end') + 0.3`（≈247.2 s）起品牌卡，标语与 `tagline` 一致；TTS 缓存、429 退避、WSS_URL 替换、`TARGET = None`、`LEAD_IN 0.8`、`TAIL 2.8`。

## 6. 声音与字幕

- **旁白**：42 段约 1170 字；混音链 `aresample → adelay → amix → highpass 80 Hz → loudnorm I=-16:TP=-1.5:LRA=11`。
- **配乐 / 音效**：`audio.mjs` 与 [[seawater-temperature]] 字节级相同（sha256 `b101c258f28d`）：112 BPM C–G–Am–F 拨弦配乐 + FX 表；7 个模块约 85 个事件，另有自动 whoosh 与品牌卡 sparkle。
- **总混音**：配乐 ×0.5 侧链闪避，音效 ×0.5，`alimiter=0.93`（`render.mjs:170-176`）。
- **字幕**：`SUB_Y = 1560`，字号 56 起按宽度递减；标签小林（天蓝）/ 阿咸（金）/ 滴滴（浅蓝）；品牌卡后停止。底部资料行“资料：人教版地理必修第一册 · ESA · NASA · WHOI · SYKE”从 `sum` 段开始显示。

## 7. 迭代与问题

- **地名纠错有记录**：`notes` 说明为什么不用“丹麦海峡”——这是本片唯一留下的“改稿理由”类证据。
- **注释与代码不一致**：`render.mjs:22` 背景注释写“sunlit sea with drifting sparkles and salt glints”，但与 [[seawater-temperature]] 的 diff 显示只改了渐变色，气泡绘制代码没变（**推断**：注释先改、实现未跟上）。
- **复制遗留**：`audio.mjs` 原样复制自 [[seawater-temperature]]，含“温跃层下滑哨”“声呐”等本片用不到的音效；`cast.mjs`、`props.mjs` 与 seawater-temperature 也完全相同（sha256 `4eaa4e811cae`、`9a53efe20fc3`）。
- **全局语速 +4%**：`rate: 4`，`TARGET = None`；原因**未记录**。
- **没有 `based_on` 字段、没有 URL**：来源写在 `source` 文本里。
- **遵守 AGENTS 的证据**：无“示意 / 不按比例”字样；地图上北 + 指北针；不画中国地图；logo、品牌卡、`TAIL 2.8` 符合第 4 节；`build/` 无残留 `still_*.png`。
- 宿主会话不可读，修改轮次与用户反馈**无法确认**。

## 8. 复现入口

本次**均未运行**。按 AGENTS.md“常用命令”（在 `seawater-salinity/` 内）：

```bash
python3 -m venv .venv && .venv/bin/pip install edge-tts   # fonts.py 另需 fontTools（AGENTS 未列出）
npm install                                               # @napi-rs/canvas；无 npm scripts
.venv/bin/python tts.py            # → build/seg*.mp3、tts_cache/、narration.wav、timeline.json；out/<标题>.srt
node render.mjs --still 70 190     # → build/still_<t>.png（用完删除）
node render.mjs                    # → out/最咸的海水，为什么不在赤道.mp4
```

## 9. 参考来源

`script.json` **没有 URL**，只有文字出处：人教版地理必修第一册 P52—54 及配套图册 P24—25；人教版选择性必修 1 第三章第一节；ESA《Sensing salinity》；NASA Salinity《Salinity FAQs》《Salinity Explained》；WHOI Oceanus《Mysteries of the Red Sea》；SYKE Baltic Sea portal《Hydrography》。均为**项目文件中提及，本次未访问**。改编来源：Geo Story Studio `douyin-seawater-salinity v002`。

## 10. 证据与限制

| 文件 | 行数 | sha256 前 12 位 | 用途 |
| --- | --- | --- | --- |
| `script.json` | 81 | `1aa20de2459e` | 全文已读 |
| `tts.py` | 209 | `bcd3c5cc6b74` | 与 seawater-temperature 版 diff（仅标题与注释不同） |
| `render.mjs` | 192 | `42ea30f35614` | 与 seawater-temperature 版 diff |
| `lib.mjs` | 225 | `903aad56d128` | diff（仅首行注释不同） |
| `seas.mjs` | 132 | `04b68fb80880` | 海岸折线、投影、指北针 |
| `scenes-a…g.mjs` | 162/91/109/174/136/199/124 | `e385ffb37b1d` / `5cc4e949fbe6` / `50b45dd17d3e` / `f74bc9904489` / `af75272c7c1c` / `2bacf54d9d4a` / `5b97973c1f55` | 头注释、场景表、关键段落 |
| `chars.mjs` / `audio.mjs` | 232/227 | `501c385aa728` / `b101c258f28d` | 导出清单、注释 |

- 已读：上表（大文件按段）、`build/timeline.json`、`package-lock.json`。
- 未读 / 未做：`node_modules/`、`.venv/`、`tts_cache`、音视频；未运行脚本；未访问外部资料；未读取源 build。
- 缺失：会话内容、需求原文、验收记录、工具版本、成片实测响度。
