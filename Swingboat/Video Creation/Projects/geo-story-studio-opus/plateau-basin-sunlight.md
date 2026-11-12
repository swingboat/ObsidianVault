---
type: video-production-dossier
project: geo-story-studio-opus
video_id: plateau-basin-sunlight
title: 青藏高原和四川盆地，谁收到的阳光更多？
curriculum: 人教版必修第一册 第一章第二节（我国年太阳辐射总量分布）、第二章第二节 大气受热过程（削弱作用、保温作用）；课标 1.2、1.4
captured: '2026-11-12'
status: 成片存在（竖屏 1080×1920；完整版 218.6 s + 推荐版 113.7 s + 推荐版封面）；推荐版有作者自写抽查记录；技术检查/视觉检查/用户验收未记录
source_fingerprint: d978ada5c33f0251b680c738df0a8b23d51015fbbfa9db10a8b5af50889bb79c
tags:
- video-creation
- production-capture
- geography-teaching
- canvas-frame-render
- edge-tts
---

# 青藏高原和四川盆地，谁收到的阳光更多？ · 四声线“阳光争霸赛”完整版 + `--rec` 结论前置推荐版（同一套代码双剪辑）

<!-- capture-source-sha256:d978ada5c33f0251b680c738df0a8b23d51015fbbfa9db10a8b5af50889bb79c -->

本文是**根据项目文件做的材料回溯**（源码、`script.json` / `script-rec.json`、`build/timeline.json` 与 `build/rec/timeline.json`、`out/` 下的成片、字幕、封面和作者写的《推荐版说明》，以及证据包里的 ffprobe 与 sha256 清单）。证据包把宿主会话 `3a8ac319…`（2026-10-28 10:14 创建）映射到本片，但**只凭元数据找到**：宿主返回空 transcript，对话内容无法读取，映射关系是按时间戳**推断**的（会话创建早于 `script.json` 数分钟）。本次未运行任何项目脚本（没有跑 `tts.py`、`render.mjs`、npm、pip），也没有打开视频或音频。

本片是 AGENTS.md 第 4 节“片尾品牌卡”规则的**参考实现**（`outro()`）。

## 1. 当前结论与制作路线

**状态：两个版本都已出片**。完整版 `out/青藏高原和四川盆地，谁收到的阳光更多.mp4`（10-28 10:37）+ `.srt`；推荐版 `…_推荐版.mp4`（11:04）+ `.srt` + `_推荐版_封面.png` + `_推荐版_说明.md`（11:05）。推荐版说明里有作者写的抽查记录（第 0/2/5/10 秒画面、音画同步、响度约 −15.5 LUFS）；这是项目内自述，本次没有复核。完整版没有任何检查记录；用户验收与是否发布均**未记录**。

| 项 | 完整版 | 推荐版（`--rec`） | 来源 |
| --- | --- | --- | --- |
| 分辨率 / 画幅 | 1080×1920，9:16 | 同左 | ffprobe；`lib.mjs:7` |
| 帧率 / 帧数 | 30 fps；`round(218.612×30)` = 6558 帧 | 30 fps；`round(113.686×30)` = 3411 帧 | ffprobe；`render.mjs:189`（推算） |
| 时长 | 218.603 s（timeline 218.612 s，约 3:39） | 113.700 s（timeline 113.686 s，约 1:54） | ffprobe；timeline |
| 视频 | H.264，yuv420p，CRF 19，preset medium，`+faststart` | 同左 | ffprobe（codec）；`render.mjs:186-187` |
| 音频 | AAC 48 kHz 立体声 192 kbps | 同左 | ffprobe；`render.mjs:187` |
| 文件大小 | 29,632,092 B（约 29.6 MB） | 15,155,583 B（约 15.2 MB） | 证据包 |
| 字幕 | `.srt` 104 条（6503 B）+ 画面烧录 | `.srt` 47 条（3044 B）+ 烧录 | timeline cues 计数；文件 |
| 文案 | `script.json` 39 段 | `script-rec.json` 15 段 | 文件 |
| 片尾品牌卡 | 有（`bye` 结束 +0.3 s） | **无**（以总结卡 + 判断题收尾） | `render.mjs:20-21` |
| 封面 | 无 | `_推荐版_封面.png` 1080×1920（`cover.mjs`） | 文件；说明.md |

一句话管线：

> `script.json`（旁白晓晓 + 学生小林 + “青藏高原”“四川盆地”两个拟人角色）→ `tts.py` 调 edge-tts 逐句合成，WordBoundary 取逐词时间 → `build/timeline.json` + `narration.wav`（adelay/amix/loudnorm）+ SRT → `render.mjs` 动态加载 `scenes-a…d`（9 个场景），`cast/props/lib` 提供角色与道具，`audio.mjs` 程序合成配乐和音效 → RGBA 帧经 stdin 送 ffmpeg（音乐侧链压缩避让旁白）→ MP4。加 `--rec` 时，同一套 `tts.py` / `render.mjs` 改读 `script-rec.json`、写 `build/rec/`、加载 `scenes-r1…r3` + `stage.mjs`，输出 `_推荐版`。

## 2. 教学目标与课标对齐

- **curriculum**（`script.json`）：人教版必修第一册 第一章第二节 太阳对地球的影响（我国年太阳辐射总量分布）、第二章第二节 大气受热过程（大气对太阳辐射的削弱作用、保温作用）；课标 1.2、1.4。推荐版把“保温作用”从 curriculum 中去掉（`script-rec.json`），与它删掉气温机制的取舍一致。
- **核心知识点**：比较两地“年太阳辐射总量”，用“大气对太阳辐射的削弱作用”解释差异。完整版另用较长篇幅讲“辐射强 ≠ 气温高”（地面是近地面大气的主要直接热源、保温作用差、垂直递减率），实际跨了两个小节——AGENTS.md 第 6 节要求“一个视频只讲透一个核心知识点”，这里属于主线 + 易错点延伸，是否算超范围由读者判断。
- **完整版结构（按文案）**：
  1. 导入：小林提问 → “阳光争霸赛”，红方高原自称“日光城”，蓝方盆地讲“蜀犬吠日”（`ask`…`penHi`）。
  2. 定规则：同样大的水平地面、一整年、直射 + 散射都算 → “阳光存钱罐” → 教材术语“年太阳辐射总量”（`rule`…`term`）。
  3. 比分：高原 >1750、部分 >2000 kWh/㎡；盆地部分 <1000；“整体格局，不是每地每天”（`score`…`notEvery`）。
  4. 原理一：削弱作用（吸收、反射、散射）；高原海拔高、空气稀薄洁净；辨析“离太阳更近”（`atmo`…`notCloser`）。
  5. 原理二：天气——高原晴天多，盆地水汽难散、多云雾，云层反射；散射光仍能到达（`weather`…`penDiffuse`）。
  6. 易错点：日照时数 ≠ 能量；辐射强 ≠ 气温高（`hours`…`gaoCold`）。
  7. 迁移：高原农作物积累养分、光伏；盆地晒被子（`use`…`usePen`）。
  8. 总结 + 两个“不等于”+ 小林收尾（`sum`…`bye`）。
- **推荐版结构**：首帧即给结论“青藏高原总体更多”+ 比较口径，4.7 s 问“为什么” → 接收板 → 光路（散射分“回太空 / 到地面”两路）→ 高原空气少约四成 → 盆地厚云 → 2022 年资源等级 → 四个量辨析（年总量、辐照度、日照时数、气温）→ 总结卡 + 判断题（说明.md 结构表）。
- **科学边界**：`source` 字段写明依据 “Geo Story Studio · douyin-plateau-basin-sunlight v002 分镜与科学审校（review-001）”，即 `geo-story-studio/storage/builds/douyin-plateau-basin-sunlight/v002`；但 `script.json` **没有 `based_on` 字段**，审校边界原文未记录在本项目中。文案自身的限定：“这是全年总量的整体格局，并不是每个地方、每一天都这样”；“不是离太阳更近”；“阴天也有散射光”。
- **数字与出处**（`script.json` `numbers`）：

| 数字 | 出处（文件原文） |
| --- | --- |
| 高原最丰富带 >1750 kWh/㎡，部分 >2000 | 国家能源局 2014-08-03《我国太阳能资源是如何分布的？》 |
| 盆地一般带，部分 <1000 | 同上 |
| 高原、干燥地区大，平原、多雨高湿地区小 | 中国气象局风能太阳能中心《2022中国风能太阳能资源年景公报》第 2 章 |
| 日地距离约 1.5 亿千米 | NASA Science, Sun: Facts |
| 对流层每升高 100 m 约降 0.6 ℃ | 人教版必修第一册 第二章第一节 |

推荐版 `numbers` 全部改用 2022 公报（西藏大部、青海中部等 >1750；四川东部、重庆 1050～1400；2022 为近 30 年最高的偏大年景），另加 U.S. Standard Atmosphere 1976（4000 m 气压约为海平面 61%）和 WMO-No. 8 第 8 章（日照时数阈值 120 W/m²）。

## 3. 技术与依赖

| 类别 | 内容 | 证据 |
| --- | --- | --- |
| 渲染 | Node.js ESM，`@napi-rs/canvas` 声明 `^1.0.11`，lock 解析为 **1.0.13** | `package.json`；`package-lock.json` |
| npm scripts | 无 | `package.json` |
| TTS | Python `edge-tts` + `aiohttp`（`tts.py` import）；版本未记录（无 requirements 文件） | `tts.py:16-18` |
| 字体抽取 | `fonts.py` 用 `fontTools` 从 macOS 系统字体集抽出圆体-简粗体、苹方-简中粗；`lib.mjs` 发现缺字体时自动调 `.venv/bin/python fonts.py` | `lib.mjs:11-13`；`fonts.py` |
| 合成 | 系统 ffmpeg（libx264、aac、sidechaincompress、amix、alimiter；TTS 侧 adelay/amix/loudnorm）；版本未记录 | `render.mjs:178-188`；`tts.py:182-196` |

**声线**（`script.json` `voices`；全局 `"rate": 4` 叠加到每个角色上，`tts.py:99`）：

| 角色 | key | 声音 | 角色 rate / pitch | 实际 rate |
| --- | --- | --- | --- | --- |
| 旁白 | `zh` | zh-CN-XiaoxiaoNeural | 0 / 0 | +4% |
| 小林（学生） | `stu` | zh-CN-YunxiNeural | +6 / +2 Hz | +10% |
| 青藏高原 | `gao` | zh-CN-YunjianNeural | +4 / −2 Hz | +8% |
| 四川盆地 | `pen` | zh-CN-XiaoyiNeural | +6 / +4 Hz | +10% |

推荐版声线配置完全相同；说明.md 称“正常语速 +4%，未提速”。`TARGET = None`，未设时长上限（`tts.py:38`），符合 AGENTS.md 第 2 节。

## 4. 镜头与时间轴

场景边界都写成“下一段文案开始前 0.25 s”（如 `to: S('score') - 0.25`），交叉淡化 `XF = 0.4 s`（`render.mjs:18`）。时间取自 `build/timeline.json` 的 segment `start`。

**完整版（4 个模块、9 个场景）**

| 场景 id | 约时间 | 旁白要点 | 画面 | 模块 |
| --- | --- | --- | --- | --- |
| `intro` | 0–20.6 s | 小林提问；争霸赛；红蓝方登场；“蜀犬吠日” | 拳击擂台、两地拟人角色、小狗叫两声 | `scenes-a.mjs:30` |
| `rule` | 20.6–42.4 s | 比的不是更热、更久；水平地面一整年；存钱罐；年太阳辐射总量 | “≠”盖章、存钱罐、直射/散射光点 | `scenes-a.mjs:76` |
| `score` | 42.4–67.0 s | 1750 / 2000 / 1000；整体格局；盆地“凭什么” | 两只存钱罐按数值装液，阈值虚线 | `scenes-a.mjs:159` |
| `air` | 67.0–101.8 s | 吸收/反射/散射=削弱作用；海拔 4000 m；“空气棉被”；不是离太阳更近 | 单束光分三路；“棉被”层数对比；1.5 亿千米对比 | `scenes-b.mjs:42` |
| `weather` | 101.8–122.4 s | 高原晴天多；盆地四周高山、多云雾阴雨；云层反射；散射光 | 两栏面板、雨、云 | `scenes-b.mjs:138` |
| `hours` | 122.4–135.8 s | 日照时数只记多久不记多强 | 两个水龙头（流量 vs 时长）+ 钟 | `scenes-c.mjs:35` |
| `temp` | 135.8–174.5 s | 辐射强≠气温高；地面是主要直接热源；逆辐射弱；0.6 ℃/100 m；墨镜防晒羽绒服 | 地面辐射波、温度计 | `scenes-c.mjs:102` |
| `use` | 174.5–193.1 s | 高原光合强、呼吸少、光伏；盆地晒被子 | 青稞、光伏板 | `scenes-d.mjs:54` |
| `sum` | 193.1 s–结束 | 三张总结卡 + 两个“不等于” + “背后全是地理” | 总结卡、彩纸 | `scenes-d.mjs:116` |
| 品牌卡 | 约 216.0–218.6 s | 无旁白 | 压暗 + 中央 logo + “用地理的眼光，看懂身边的阳光” | `render.mjs:62-76` |

**推荐版（3 个模块、7 个场景，共用 `stage.mjs` 两栏舞台）**

| 场景 id | 约时间 | 内容 | 模块 |
| --- | --- | --- | --- |
| `hook` | 0–12.3 s | 首帧两栏对照 + 结论 + 比较口径；接收板 | `scenes-r1.mjs:22` |
| `path` | 12.3–22.7 s | 一束光：吸收、反射回太空、散射向上/向下 | `scenes-r2.mjs:12` |
| `high` | 22.7–34.7 s | 高原头顶空气少约四成 | `scenes-r2.mjs:54` |
| `cloud` | 34.7–52.6 s | 盆地云层；散射光仍计入但合计偏少 | `scenes-r2.mjs:76` |
| `data` | 52.6–73.9 s | 2022 资源等级标尺（四档），高原东部/东南部较低 | `scenes-r3.mjs:25` |
| `terms` | 73.9–89.4 s | 年总量 / 辐照度 / 日照时数 / 气温 | `scenes-r3.mjs:89` |
| `sum` | 89.4 s–结束 | 总结卡 + 判断题“日照时数越长，总量一定越多吗？” | `scenes-r3.mjs:113` |

## 5. 关键算法与实现

> 区分：视频**讲的科学**是削弱作用、年辐射总量和大气受热；下面是**做视频用的算法**。

**5.1 逐词时间戳驱动一切（`lib.mjs:37-46`）**。`tts.py` 把 edge-tts 的 WordBoundary（100 ns 单位，`TICKS = 1e7`）换算成秒，并用 `text.find` 记下每个词在原句里的字符位置 `idx`（`tts.py:81-88`）。渲染端 `at(id, '1750')` 按字符位置找回该词的发音时刻，所有数字弹出、液位上涨、音效都挂在这里，例如比分场景 `tG = at('gaoNum', '1750')`（`scenes-a.mjs:160`），音效 `{ type: 'tada', t: at('gaoNum', '1750') + 0.4 }`（`scenes-a.mjs:226`）。

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
```

文案里找不到该词会直接抛错，改文案后渲染会失败而不是悄悄错位。

**5.2 同一套代码出两个剪辑（`--rec`）**。`tts.py:33-41` 根据 `--rec` 切换脚本、`build/rec/`、标题后缀、`LEAD_IN`（0.2 s vs 1.0 s）和 `TAIL`（2.0 s vs 2.8 s），TTS 缓存共用；`lib.mjs:28-30` 决定读哪份 timeline；`render.mjs:15` 切换场景模块列表，`render.mjs:143` 让推荐版不从黑场淡入（首帧就是完整画面）。

```js
const KEYS = REC ? ['r1', 'r2', 'r3'] : ['a', 'b', 'c', 'd'];
// the closing brand card takes over the corner logo (AGENTS.md §4); the feed edit ends on its summary card instead
const OUTRO = REC ? DURATION + 60 : E('bye') + 0.3;
```

**5.3 角标 logo 与片尾品牌卡（`render.mjs:47-76`）**。角标：`LOGO_BOX = {sx:150, sy:96, sw:1998, sh:528}` 只取透明 PNG 的有效区域（`lib.mjs:25`），放在 `x=64, y=76, w=290`，后面画半径 250 的径向暗晕，0.3–1.0 s 淡入，`OUTRO` 起 0.4 s 内淡出。品牌卡：最后一句结束 0.3 s 后整屏压暗到 0.82，中央 logo 宽 760、0.9→1 放大，下面金色标语；字幕在 `t > OUTRO` 时停止（`render.mjs:82`），出现时加一声 `sparkle`（`render.mjs:153`），最后 0.6 s 淡出到黑（`render.mjs:143`）。这正是 AGENTS.md 第 4 节引用的写法。

**5.4 两栏舞台按气压画空气柱（`stage.mjs:1-2, 8-9, 62-90`）**。推荐版两栏从同一条顶线 `TOP = 640` 起算，高原地面 `SP = 1120`、盆地地面 `SB = 1385`，于是高原上方空气柱更短；注释写明比例取自气压（4000 m 约 62%，500 m 约 95%）。空气粒子按 `u = ((i+0.5)/n)^0.6` 分布，越靠近地面越密。完整版的比分场景则是“存钱罐液位”：`levelY(v)` 把 kWh/㎡ 线性映射到罐高、满刻度 `MAXV = 2200`（`scenes-a.mjs:156-158`）；推荐版说明明确说**删掉了这种按数值装罐的液位图**，改为四档资源等级标尺 `TIERS`（`scenes-r3.mjs:23`），“不暗示倍数或区域平均值”。

**5.5 程序合成配乐与音效（`audio.mjs`）**。配乐：112 BPM，C–G–Am–F 循环，Karplus–Strong 拨弦 + 低音 + 带通噪声沙锤，隔小节加钟琴（`audio.mjs:149-185`），淡入 1.2 s、淡出 2.5 s。音效表 `FX` 有 pop/ding/sparkle/whoosh/boing/slam/stamp/tick/scribble/buzz/alarm/tada/confetti/wind（`audio.mjs:78-140`），由各场景 `events` 按词时间触发；`render.mjs:152` 另给每次场景切换自动加一声 whoosh。随机数用种子固定的 mulberry32（`rng(7)` / `rng(42)`），重渲染结果一致。

## 6. 声音与字幕

- **TTS**：`synth_one` 按 `sha1(voice|rate|pitch|text)[:16]` 缓存到 `build/tts_cache/`（`tts.py:52`）；当前缓存 108 个文件（mp3 + json 成对）。429 时等待 8/16/24/32 s 重试，最多 5 次（`tts.py:58-68`）。地址改用 `api.msedgeservices.com`（`tts.py:20-26`）。
- **排时**：每句占用“最后一个词结束 + 0.12 s”，再加该句 `gap`（默认 0.45 s）；注释说明 mp3 尾部约 0.6 s 静音会与下一句重叠（`tts.py:157-158`）。
- **旁白混音**：每句 `aresample=48000,adelay=ms|ms` → `amix normalize=0` → `highpass=80` → `loudnorm=I=-16:TP=-1.5:LRA=11` → `apad,atrim` 到总长（`tts.py:182-191`）。
- **成片混音**：旁白分两路，一路作侧链让音乐（0.55）自动避让（`sidechaincompress threshold=0.05 ratio=5`），音效 0.5，三路 `amix` 后 `alimiter=0.93`（`render.mjs:183-184`）。推荐版自述成片响度约 −15.5 LUFS（未复核）。
- **字幕**：`captions` 字段手工断句，无则按标点自动切（≤14 字，`tts.py:112-125`）；每条字幕起止取所覆盖词的首尾时间（`tts.py:128-142`）。画面字幕在 `SUB_Y = 1560`，按宽度自动从 56 号缩小，角色发言带彩色名牌（小林 / 青藏高原 / 四川盆地）（`render.mjs:79-107`）。
- **资料来源行**：完整版从 `sum` 起、推荐版从 `data` 起在 y=1680 显示（`render.mjs:110-118`），代替“示意”声明，符合 AGENTS.md 第 5 节。

## 7. 迭代与问题

- **推荐版是对完整版的二次修订**（时间：完整版成片 10:37 → 推荐版 SRT 10:55、封面 11:03、成片 11:04、说明 11:05）。说明.md“相比完整版的修改”列出：删去红蓝方登场、存钱罐、“离太阳更近”辨析、气温机制与生活应用、多轮总结与片尾品牌卡；不再混用 2014 年国家能源局阈值，统一用 2022 公报；散射光拆成“回太空 / 到地面”；高原改称“大部分地区晴天较多”。这些修改**反过来说明完整版存在的口径问题**：完整版仍用 2014 年阈值（“部分超过 2000”“部分不到 1000”）、仍用按数值装液的存钱罐、仍把高原概括为“空气干燥，晴天多”。完整版没有随之更新（说明.md：“教学完整版的文件和配置保持不变”）。
- **与 AGENTS.md 第 4 节冲突**：规则要求“推荐版、精简版等其他版本也要加”片尾品牌卡，而本片推荐版明确不加（`render.mjs:20-21` 注释“the feed edit ends on its summary card instead”，说明.md 也写“推荐版以总结卡收尾，左上角 logo 全程保留”）。推断：该条规则是在本片之后补写的，本片推荐版未回头修改。
- **`based_on` 缺失**：AGENTS.md 第 1 节要求把源 build 路径写进 `based_on`，本片只在 `source` 字段里写了 “douyin-plateau-basin-sunlight v002 … review-001”。
- **数据混用风险**：完整版 `numbers` 同时引用 2014 国家能源局区划和 2022 公报两种口径，推荐版说明专门指出要避免这种混用。
- **README**：已收录（`README.md:37`，标 3:39，并注明 1:54 推荐版）。
- **禁用声明**：源码中没有“示意 / 非观测 / 不按比例”等文字（仅 `render.mjs:109` 注释引用规则本身）。画面上没有中国地图（说明.md“地图”条）。
- git 只有批量提交 `8cbe673`（2026-11-01，“add 21 high-school geography lessons”），看不出单片修改过程。

## 8. 复现入口

AGENTS.md 常用命令 + `render.mjs` 头注释 + 说明.md（本次**均未运行**）：

```bash
cd plateau-basin-sunlight
python3 -m venv .venv && .venv/bin/pip install edge-tts   # 另需 fontTools 才能跑 fonts.py（未在文件中声明）
npm install                                               # @napi-rs/canvas
.venv/bin/python tts.py            # → build/timeline.json, build/narration.wav, out/<标题>.srt
node render.mjs --still 8.0 14.2   # → build/still_<t>.png
node render.mjs                    # → build/music.wav, build/sfx.wav, out/<标题>.mp4
.venv/bin/python tts.py --rec      # → build/rec/（共用 tts_cache）, out/<标题>_推荐版.srt
node render.mjs --rec              # → out/<标题>_推荐版.mp4
node render.mjs --rec --cover      # → out/<标题>_推荐版_封面.png
```

顺序必须先 TTS 后渲染：`lib.mjs:30` 启动时就读取 timeline.json。

## 9. 参考来源

代码和 `script*.json` 中**没有 URL**（证据包 `urls: []`），只有出处名称（项目文件中提及，本次未访问）：

| 来源 | 用途 |
| --- | --- |
| `geo-story-studio/storage/builds/douyin-plateau-basin-sunlight/v002`（分镜与科学审校 review-001） | 改编来源（写在 `source` 字段） |
| 国家能源局《我国太阳能资源是如何分布的？》（2014-08-03） | 完整版 1750 / 2000 / 1000 阈值 |
| 中国气象局风能太阳能中心《2022中国风能太阳能资源年景公报》第 2 章、图 2.1/2.2 | 分布规律；推荐版全部区域数据 |
| NASA Science, Sun: Facts；NASA Earth Observatory, Climate and Earth's Energy Budget | 日地距离；能量收支背景 |
| U.S. Standard Atmosphere 1976 | 推荐版“4000 m 空气少约四成” |
| WMO-No. 8 第 8 章 Sunshine duration | 推荐版日照时数定义 |
| 人教版地理必修第一册 | 术语、0.6 ℃/100 m、平均海拔 4000 m 以上 |

## 10. 证据与限制

| 文件 | 行数 | sha256 前 12 位 | 本次读取 |
| --- | --- | --- | --- |
| `script.json` | 69 | `dc5fecee4fee` | 全文 |
| `script-rec.json` | 39 | `a1ad1e87c900` | 全文 |
| `tts.py` | 214 | `db1d0f5f959b` | 全文 |
| `render.mjs` | 200 | `414827a4004b` | 全文 |
| `lib.mjs` | 223 | `3de5c0dded3e` | 1–64、185–223 |
| `stage.mjs` | 151 | `9204aa79ebb5` | 1–14、62–90 |
| `scenes-a/b/c/d.mjs` | 232/221/209/155 | `6d7ac65c43d9` / `641707311b42` / `8ca6ec9446bb` / `0af6b4de407d` | 场景表、事件表与部分绘制函数 |
| `scenes-r1/r2/r3.mjs` | 68/115/178 | `16a11f375571` / `428ed057078a` / `47c6a3b4d953` | 场景表、`TIERS`、`dataScene` 开头 |
| `cover.mjs` | 18 | `fb41dbd80728` | 全文 |
| `audio.mjs` | 204 | `f9e04b2cfd54` | 函数清单（与 `fossil-record/audio.mjs` 对比：少了 fizz/boom/roar/thud/sizzle 五个音效） |
| `cast.mjs` / `props.mjs` | 263 / 172 | `90c2d27d0ca7` / `58f30f82779c` | 仅导出函数清单 |
| `fonts.py` | 37 | `a6ac7264e548` | 全文（与其他视频相同） |
| `out/…_推荐版_说明.md` | — | — | 全文（作者自述，不等于验收） |
| `build/timeline.json`、`build/rec/timeline.json` | — | — | 顶层字段、segment 起止、cue 数 |

未读 / 无法确认：成片画面与声音（未打开）；`cast.mjs`、`props.mjs` 绘制细节；制作对话（transcript 为空）；edge-tts、ffmpeg、Node、Python 版本；用户是否验收或发布；`geo-story-studio` 源 build 的审校原文。
