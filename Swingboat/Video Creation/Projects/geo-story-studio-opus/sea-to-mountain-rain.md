---
type: video-production-dossier
project: geo-story-studio-opus
video_id: sea-to-mountain-rain
title: 一滴海水，怎样变成山里的雨？
curriculum: 人教版必修第一册 第三章第一节 水循环；课标 1.5 运用示意图，说明水循环的过程及其地理意义
captured: '2026-11-12'
status: 成片存在（竖屏 1080×1920，249.3 s）；技术检查/视觉检查/用户验收未记录
source_fingerprint: 5393f9df0e1a62444e4ffffc681f9b3679c6a79ead0741668c6c6d9a15dc83b4
tags:
- video-creation
- production-capture
- geography-teaching
- canvas-frame-render
- edge-tts
---

# 一滴海水，怎样变成山里的雨？ · 五角色“闯关”叙事 + 同一幅海—山剖面贯穿的水循环逐帧动画

<!-- capture-source-sha256:5393f9df0e1a62444e4ffffc681f9b3679c6a79ead0741668c6c6d9a15dc83b4 -->

本文是**根据项目文件做的材料回溯**（`script.json`、`tts.py`、`render.mjs`、`lib.mjs`、6 个场景模块与 `cast/props/audio.mjs`、`build/timeline.json`、证据包中的 ffprobe 与 sha256 清单）。证据包映射到一个宿主会话 `87b56def…`（创建于 2026-10-31 23:05，标题“视频制作与水循环内容”），但只按元数据找到——宿主返回空 transcript，**对话内容不可读**；映射依据是会话创建时间早于 `script.json` 数分钟，属**推断**。本次未运行任何项目脚本（`tts.py`、`render.mjs`、npm、pip 均未运行），也未打开视频或音频。

## 1. 当前结论与制作路线

**状态：成片存在**——`out/海水变山雨.mp4` + 同名 `.srt`。只有证据包的 ffprobe 媒体头信息；画面检查、试听、响度测量、用户验收与发布情况都**未记录**。

| 项 | 值 | 来源 |
| --- | --- | --- |
| 分辨率 / 画幅 | 1080×1920，9:16 竖屏 | ffprobe；`lib.mjs:7` |
| 帧率 / 帧数 | 30 fps；`round(249.256×30)` = 7478 帧 | ffprobe；`render.mjs:221`（推算） |
| 时长 | 249.267 s（timeline 249.256 s） | ffprobe；`build/timeline.json` |
| 视频编码 | H.264，yuv420p，CRF 19，preset medium，`+faststart` | ffprobe（codec）；`render.mjs:218-219` |
| 音频编码 | AAC 48 kHz 立体声，192 kbps | ffprobe；`render.mjs:219` |
| 文件大小 | 35,895,934 字节（约 35.9 MB） | 证据包 |
| 字幕 | `out/海水变山雨.srt`，128 条，7642 字节；画面另有烧录字幕 + 角色标签 | 证据包；`tts.py:202-206`；`render.mjs:69-94` |
| 其他版本 | 无（无横屏版、推荐版或封面模式） | `out/` 列表 |

文件时间：`.srt` 2026-10-31 23:13，成片 23:30（证据包 mtime 换算）。git：证据包只记录 `8cbe673 2026-11-01 feat(videos): add 21 high-school geography lessons` 一条，单片修改历史不可见。README 目录第 75 行收录本片（竖屏，4:09）。

一句话管线：

> `script.json`（43 段，5 个声线角色）→ `tts.py` 调 edge-tts 逐句合成，`WordBoundary` 拿逐词时间 → `build/timeline.json` + `narration.wav`（adelay/amix/loudnorm）+ SRT → `render.mjs` 动态加载 `scenes-a…f.mjs`（11 个场景），按“词”触发动画；`audio.mjs` 程序合成配乐与 70 余个音效事件 → 每帧 RGBA 经 stdin 管道送 ffmpeg，侧链压缩混音后输出 H.264/AAC MP4。

## 2. 教学目标与课标对齐

- **curriculum**：人教版必修第一册 第三章第一节 水循环（蒸发、水汽输送、降水、地表径流、下渗、地下径流、植物蒸腾；海陆间 / 海上内 / 陆地内循环）；课标 1.5（`script.json:3`）。
- **核心知识点**：水循环的过程与环节，以“一滴海水”的旅程串起，最后归纳三类循环与地理意义（淡水资源更新）。
- **讲解结构（按实际文案）**：
  1. 导入：小林问“海水那么咸，雨水可一点都不咸呀”（`a-stu`），请“一滴海水当导游”（`a-hook`）。
  2. 原理拆解，分四关：①蒸发（水汽不可见、盐分留在海里）；②水汽输送（例：夏季暖湿气流给我国东部带来降水）；③爬山——抬升、气压降低、膨胀降温、凝结核上成云；云内碰并（0.02 mm vs 2 mm，约 100 万颗）、冷云冰晶；④落地分岔——地表径流、下渗（土壤水 / 补给地下水）、地下径流；再加植物蒸腾与陆面蒸发。
  3. 结论：把路线连成水循环，三类循环依次高亮；海陆间循环“让淡水资源得以更新”。
  4. 迁移与辨析：小林问“能一直更新，是不是用不完”→“能更新 ≠ 用不完”（降水、补给速度、水质）。
  5. 总结：三个考试易错点（水汽看不见、云不是水汽；有云不一定下雨；下渗不一定都变成地下水）+ 小海“下一站说不定又是一场雨”。
- **科学审校边界**（`based_on`，源自 `geo-story-studio/storage/builds/douyin-water-cycle/v003`，N21）：水汽是气态不可见，不画成小水滴；蒸发时盐分基本留在海水里，不据此说雨水可直接饮用；遇山抬升只是一条可能路线，有山不保证下雨；云由小水滴或冰晶组成，不是水汽本身；只演液态碰并、提一句冰相过程；落地后分支，同一份水不复制；下渗 ≠ 进入地下水；并非所有河流入海；陆地水可经蒸发、蒸腾回大气；可更新 ≠ 用不完。代码落实情况见 §5。
- **数字与出处**（`script.json` `numbers`，8 条）：

| 数字 / 表述 | 出处（script.json 写法） |
| --- | --- |
| 液态 → 气态；水汽肉眼不可见 | 人教版必修一 3.1；USGS Evaporation / Glossary |
| 盐分基本留在海水里（不排除浪花飞沫带出的海盐颗粒） | USGS evaporative salt ponds 段 |
| 上升 → 气压降低 → 膨胀 → 冷却 → 凝结核成云 | NOAA JetStream How Clouds Form；NAV CANADA Onshore Flow |
| 云滴约 0.02 mm、雨滴约 2 mm、体积差 100³ = 100 万倍 | NOAA SciJinks / UCAR 科普；按直径换算 |
| 上升气流托不住才降落；冷云冰晶增长也能降水 | USGS Precipitation |
| 地表径流 / 下渗 / 地下径流 | 教材图 3.2；USGS Infiltration / Groundwater Flow |
| 三类循环；海陆间循环使水资源更新 | 人教版必修一 3.1 |
| 可更新 ≠ 用不完 | 人教版必修一 3.1；NASA Water Cycle |

这些出处本次均未访问核对。

## 3. 技术与依赖

| 类别 | 内容 | 证据 |
| --- | --- | --- |
| 渲染 | Node.js ESM（`"type": "module"`）+ `@napi-rs/canvas` `^1.0.11`；lock 解析为 1.1.0 | `package.json`；`package-lock.json` |
| npm scripts | 无 | `package.json` |
| TTS | Python `edge-tts` + `aiohttp`（`.venv`）；版本未记录，无 requirements 文件 | `tts.py:16-19` |
| 字体 | `fonts.py` 用 `fontTools` 从 macOS 系统字体集合抽出圆体-简粗体、苹方-简中粗到 `build/fonts/`；`lib.mjs` 缺字体时自动调用 `.venv/bin/python fonts.py` | `fonts.py`；`lib.mjs:11-19` |
| 编码 / 混音 | 系统 ffmpeg：旁白混音（`tts.py:181-196`）、成片编码与总混音（`render.mjs:209-220`）；版本未记录 | 代码 |

声线（`script.json` `voices`；全局 `rate: 4` 叠加在各角色 rate 之上，`tts.py:94`）：

| 角色 key | 字幕标签 | voice | 角色 rate | pitch | 实际 rate | 段数 |
| --- | --- | --- | --- | --- | --- | --- |
| `zh` 旁白（老师） | — | `zh-CN-XiaoxiaoNeural` | 0 | 0 | +4% | 25 |
| `stu` | 小林 | `zh-CN-YunxiNeural` | +6 | +2 Hz | +10% | 4 |
| `drop` | 小海 | `zh-CN-XiaoyiNeural` | +10 | +6 Hz | +14% | 6 |
| `vapor` | 小海（水汽） | `zh-CN-XiaoyiNeural` + `fx: aecho=0.8:0.6:60\|120:0.35\|0.2` | +4 | +3 Hz | +8% | 6 |
| `salt` | 盐粒 | `zh-CN-YunxiaNeural` | 0 | −4 Hz | +4% | 2 |

## 4. 镜头与时间轴

时间取自 `build/timeline.json` 的段落 start；场景边界多为“下一段开始前 0.3 s”，交叉淡化 `XF = 0.3`（`render.mjs:23`）。6 个模块共 11 个场景。

| # | 场景 | 约时间 (s) | 口播要点 | 画面 | 模块 |
| --- | --- | --- | --- | --- | --- |
| 1 | `hook` | 0–9.0 | 小林：雨为什么不咸？请海水当导游 | 雨中山景，“海水 咸 / 雨水 不咸”两张味道卡 | `scenes-a.mjs:7-53` |
| 2 | `seaScene` | 9.0–50.3 | 小海与盐粒登场；第一关蒸发，小海“隐身”，盐粒飞不起来 | 海面剖面、太阳热射线，小海变虚线水汽符号，“不是小水滴”盖章 | `scenes-a.mjs:54-135` |
| 3 | `transport` | 50.3–66.2 | 第二关水汽输送；夏季暖湿气流 | 海—山剖面上的风带；“海洋 → 暖湿气流 → 我国东部 降水多”方块卡（不画地图） | `scenes-b.mjs:10-57` |
| 4 | `lift` | 66.2–101.0 | 第三关抬升、气压降低、膨胀降温、凝结成云；易错点 | 气团沿山坡折线上升 + 温度计；圆形放大镜里水汽聚到凝结核上；“云≠水汽”盖章 | `scenes-b.mjs:58-154` |
| 5 | `rain` | 101.0–137.4 | 云滴被托住；0.02 mm vs 2 mm；约 100 万颗碰并；降水与冰晶 | 云内上升气流；按比例的云滴/雨滴对比；指数计数器；冷云冰晶小卡 | `scenes-c.mjs` |
| 6 | `split` | 137.4–172.2 | 第四关落地分岔：地表径流、下渗、地下径流；小海钻进土里 | 剖面切开地下层，4 颗雨滴各走一条路线 | `scenes-d.mjs` |
| 7 | `transpire` | 172.2–196.7 | 根吸水→茎→叶片气孔蒸腾；地面直接蒸发 | 植物剖面，叶片背面气孔放大 | `scenes-e.mjs` |
| 8 | `cycleMap` | 196.7–219.7 | 连成水循环；海陆间 / 海上内 / 陆地内依次高亮 | 全景路线图，三条循环折线轮流高亮；底部资料行 | `scenes-f.mjs:7-67` |
| 9 | `limit` | 219.7–233.0 | 能更新 ≠ 用不完 | 水龙头接水、补给细流更慢；盖章 | `scenes-f.mjs:68-101` |
| 10 | `exam` | 233.0–242.3 | 三个考试易错点 | 三条易错点卡 | `scenes-f.mjs:102-123` |
| 11 | `ending` + 品牌卡 | 242.3–249.3 | 小海：下一站又是一场雨 | 小海飘走；246.2 s 起品牌卡“跟着一滴海水，看懂水循环” | `scenes-f.mjs:124-138`；`render.mjs:123-145` |

## 5. 关键算法与实现

**视频讲的原理**：蒸发、抬升冷却、碰并、径流分岔。**做视频用的算法**如下。

### 5.1 逐词时间戳驱动一切（`lib.mjs:36-46`；`tts.py:123-142`）

`tts.py` 用 `boundary="WordBoundary"` 收集每个词的 offset（100 ns 单位，`TICKS = 1e7`），并记录词在原句里的字符下标 `idx`。场景代码用 `at(id, '关键词')` 取该词的开口时刻：

```js
export function word(id, w, nth = 0) {
  const s = SEG[id];
  let i = -1;
  for (let k = 0; k <= nth; k++) i = s.text.indexOf(w, i + 1);
  const hit = s.words.find((x) => x.idx <= i && i < x.idx + x.text.length) ?? s.words.find((x) => x.idx >= i);
  const last = [...s.words].reverse().find((x) => x.idx < i + w.length) ?? hit;
  return { start: hit.start, end: last.end };
}
```

字幕同理：`captions` 可写成 `["0.02毫米", "零点零二毫米"]` 对，屏幕显示阿拉伯数字，时间按口播汉字匹配（`tts.py:132`）。所以数字卡、盖章、计数器都踩在对应词上，没有按字数估算。

### 5.2 TTS 缓存、429 退避与接口地址替换（`tts.py:23-26, 45-69`）

- 运行时覆盖 `edge_tts.communicate.WSS_URL` 为 `api.msedgeservices.com`（注释引用 rany2/edge-tts#490、#491），未改 `.venv`。
- 缓存键 `sha1(voice|rate|pitch|text)[:16]`，mp3 与词时间 json 成对存于 `build/tts_cache/`。
- 只对握手 429 重试，最多 5 次，等待 8/16/24/32 s。

### 5.3 “水汽不可见”与“按比例的云滴”（`cast.mjs:218-231`；`scenes-c.mjs:95-137`）

审校边界“水汽不画成小水滴”在代码里落成一个专用符号：`vaporSign` 画三个虚线空心圆（注释：“the on-screen symbol for (invisible) water vapour, never a solid drop”）；`droplet` 也有 `vapor` 参数做实心 → 空心过渡。云滴与雨滴按真实直径比 1:100 画成半径 2.3 px 与 230 px，云滴外加金色呼吸圈方便找到；碰并计数器按指数增长：

```js
const k = prog(t, T_MERGE, T_TEAM);
const n = Math.round(Math.pow(10, 6 * easeIO(k)));   // 1 → 1,000,000
```

### 5.4 海—山剖面与“同一份水不复制”的分岔（`props.mjs:88-110`；`scenes-d.mjs:16-45`）

地形 `groundY(x)` 用 9 个关键点做余弦平滑插值，河道 `RIVER`、地下层 `SOIL_D = 110`、`WATER_D = 230` 都由它派生，蒸发、输送、抬升、分岔几幕复用同一幅剖面。落地后用 4 颗**不同**的雨滴（`WALK`：两颗 `run`、一颗 `soil`、一颗 `deep`）各走一条折线，`along(pts, p)` 按进度插值，时间窗取自“沿着地面”“渗进”“继续往深处”“流入海洋”等词——对应审校边界“落地后分支走不同路线，同一份水不复制”“下渗不自动等于进入地下水”。

### 5.5 场景切换、震屏与品牌（`render.mjs:49-62, 123-181`）

- 场景按 `from` 排序，进场 `easeOut` 淡入 + 0.95→1 缩放，离场 1→1.04 放大淡出。
- 事件带 `shake` 时（3 处“盖章”），`shakeAt` 用衰减平方的正余弦抖动整个场景层。
- 角标 logo：`x=64, y=76, w=290`，0.3–1.0 s 淡入，透明度 0.95，后衬径向暗晕；`CARD_T = E('h-end') + 0.3` 前 0.3 s 淡出。品牌卡重画背景，760 px 大 logo + 标语“跟着一滴海水，看懂水循环”与“蒸发 · 输送 · 降水 · 径流 · 下渗 · 蒸腾”；最后 0.6 s 整体淡到黑。

## 6. 声音与字幕

- **旁白**：43 段，约 1158 字；`LEAD_IN 0.6 s`（注释“Douyin hook: speak almost immediately”），默认段间 0.45 s，`script.json` 按段覆盖 0.25–0.5 s；段长按最后一个词结束 + 0.12 s 计，避开 mp3 尾部静音（`tts.py:154-156`）。
- **旁白混音**：每段 `aresample=48000` →（`vapor` 角色加 `aecho` 回声，表现“隐身的水汽”）→ `adelay` 上时间轴 → `amix normalize=0` → `highpass 80 Hz` → `loudnorm I=-16:TP=-1.5:LRA=11` → `build/narration.wav`。
- **配乐**（`audio.mjs:203-239`）：全程序合成，112 BPM，C–G–Am–F，Karplus–Strong 拨弦 + 低音 + 带通噪声沙锤，隔小节加钟琴；淡入 1.2 s、淡出 2.5 s。
- **音效**：`audio.mjs` 的 `FX` 表约 25 种（pop、ding、whoosh、stamp、rain、drip、slurp、poof、fizz 等），6 个模块共约 71 个事件。
- **总混音**（`render.mjs:215-216`）：配乐 ×0.75 并以旁白为侧链 `sidechaincompress`（threshold 0.05、ratio 5）闪避；音效 ×0.5；`amix` 后 `alimiter=0.93`。最终响度未测量。
- **字幕**：`SUB_Y = 1500`（注释：在抖音底部文案区之上），胶囊底，字号 58 起按宽度递减；非旁白角色有彩色标签（小林 / 小海 / 小海（水汽）/ 盐粒）。每条显示到下一条开始，段末延后 0.3 s 淡出。小林非开场台词时，左下角弹出小林头像（`render.mjs:96-111`），嘴型由 `talkLevel` 按词时间开合。

## 7. 迭代与问题

只列项目文件里有证据的点：

- **`based_on` 写的是本机绝对路径**（`script.json:4`），可移植性差；本文统一写作 `geo-story-studio/storage/builds/douyin-water-cycle/v003`。
- **全局语速 +4%**：`script.json` `rate: 4`，`TARGET = None`，`timeline.json` 记录 `rate: 4`。AGENTS.md 第 2 节要求按正常语速、不为压时长提速；这里不是 TARGET 压缩，但为何整体加速**未记录**。
- **遵守 AGENTS 的证据**：`render.mjs:113` 注释写明“no ‘示意’ disclaimers on screen”，代码中未搜到“示意 / 不按比例”字样；资料行“资料：人教版地理必修第一册 · USGS · NOAA · NASA”只在总结段出现；夏季风例子用方块卡，没有画中国地图（与第 6 节“拿不准不画中国全图”一致，意图为**推断**）。
- **分模块写法**：`render.mjs:16-21` 用 `existsSync` 跳过尚不存在的 `scenes-*.mjs`，支持“写一组、验一组”（AGENTS 第 3 节）；实际按什么顺序写、是否导出过预览拼图**未记录**——`build/` 中没有残留 `still_*.png`。
- **音效库复制遗留**：`audio.mjs:176-177` 有注释为“the ‘圈层大楼’ falling apart”的 `crumble` 音效，本片场景没有调用，推断沿用自本项目一部圈层题材视频。
- **README**：目录第 75 行已收录，描述与代码一致。

## 8. 复现入口

本次**均未运行**。按 AGENTS.md“常用命令”与代码推断的顺序（在 `sea-to-mountain-rain/` 内）：

```bash
python3 -m venv .venv && .venv/bin/pip install edge-tts   # AGENTS 第 2 节；fonts.py 另需 fontTools（AGENTS 未列出）
npm install                                               # 安装 @napi-rs/canvas（package.json 无 scripts）
.venv/bin/python tts.py            # → build/seg*.mp3、tts_cache/、narration.wav、timeline.json；out/海水变山雨.srt
node render.mjs --still 8.0 14.2   # → build/still_<t>.png 预览（用完删除）
node render.mjs                    # → build/music.wav、sfx.wav；out/海水变山雨.mp4
```


## 9. 参考来源

`script.json` `sources` 列出 6 个 URL，均为**项目文件中提及，本次未访问**：

| URL | 用途 |
| --- | --- |
| https://jc.pep.com.cn/ | 人教版普通高中教科书 地理 必修第一册（教材口径） |
| https://www.usgs.gov/water-science-school/science/evaporation-and-water-cycle | 蒸发；盐分留在海水里 |
| https://www.noaa.gov/jetstream/clouds/how-clouds-form | 抬升冷却、凝结成云 |
| https://www.usgs.gov/water-science-school/science/precipitation-and-water-cycle | 碰并成雨、冰晶降水 |
| https://www.usgs.gov/water-science-school/science/infiltration-and-water-cycle | 下渗、土壤水与地下水 |
| https://science.nasa.gov/earth/explore/earth-science-data/water-cycle/ | 水循环总览；可更新 ≠ 用不完 |

`numbers` 里另提到 USGS Glossary / Groundwater Flow、NAV CANADA Onshore Flow、NOAA SciJinks / UCAR，但没有给 URL。改编来源：`geo-story-studio/storage/builds/douyin-water-cycle/v003`（N21 审校边界），本次未读取该目录。

## 10. 证据与限制

| 文件 | 行数 | sha256 前 12 位 | 用途 |
| --- | --- | --- | --- |
| `script.json` | 85 | `e52fd27857e8` | 全文已读 |
| `tts.py` | 214 | `b8d55b02795f` | 全文已读 |
| `render.mjs` | 232 | `99a2083c3f5c` | 全文已读 |
| `lib.mjs` | 245 | `9cdd0392d147` | 字体、时间轴、`word()`、`talkLevel` |
| `scenes-a…f.mjs` | 158/176/154/122/149/159 | `34af1a3b39f9` / `ec86c305f553` / `570122a9640c` / `5939ff695f11` / `24abb8ac48a4` / `45fd775a364c` | 头注释、场景表、关键段落 |
| `props.mjs` / `cast.mjs` / `audio.mjs` | 236/277/258 | `74c72c713b69` / `716c9b49b6fc` / `9c59fa2860b3` | 剖面、角色、合成音频 |
| `out/海水变山雨.mp4` | — | 未计算 | 仅 ffprobe 元数据 |

- 已读：上表源码（大文件按段读取）、`build/timeline.json`（段落时间、cue 数）、`package-lock.json`（解析版本）。
- 未读 / 未做：`node_modules/`、`.venv/`、`build/tts_cache/`、字体、音视频内容；未运行脚本；未访问外部 URL；未读取源 build 目录。
- 缺失：宿主会话内容（修改轮次、用户反馈无法确认）、需求原文、验收记录、edge-tts / ffmpeg / Node 版本、成片实际响度。
