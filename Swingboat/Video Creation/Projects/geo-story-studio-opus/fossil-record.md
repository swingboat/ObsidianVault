---
type: video-production-dossier
project: geo-story-studio-opus
video_id: fossil-record
title: 化石侦探社：石头里的证人会说话
curriculum: 人教版必修第一册 第一章第三节 地球的历史（化石和地层）；课标 1.2
captured: '2026-11-12'
status: 成片存在（竖屏 1080×1920，221.1 s）；技术检查/视觉检查/用户验收未记录
source_fingerprint: f5fd50f48798f755e1438773608abb84b53fc0a36b589f134e714ed58eb652d9
tags:
- video-creation
- production-capture
- geography-teaching
- canvas-frame-render
- edge-tts
---

# 化石侦探社：石头里的证人会说话 · “三问破案”四声线竖屏片（逐词时间戳驱动 12 场景）

<!-- capture-source-sha256:f5fd50f48798f755e1438773608abb84b53fc0a36b589f134e714ed58eb652d9 -->

本文是**根据项目文件做的材料回溯**（源码、`script.json`、`build/timeline.json`、`out/` 下的成片与字幕、证据包里的 ffprobe 与 sha256 清单）。证据包把宿主会话 `03478701…`（2026-10-29 14:27 创建）映射到本片，但**只凭元数据找到**：宿主返回空 transcript，对话内容无法读取，映射关系是按时间戳**推断**的（会话创建早于 `script.json` 数分钟）。本次未运行任何项目脚本（没有跑 `tts.py`、`render.mjs`、npm、pip），也没有打开视频或音频。

## 1. 当前结论与制作路线

**状态：成片存在**——`out/化石侦探社.mp4`（10-29 15:04）+ `out/化石侦探社.srt`（14:47）。只有证据包里的 ffprobe 媒体头信息；视觉检查、试听、响度测量、用户验收和是否发布都**未记录**。

| 项 | 值 | 来源 |
| --- | --- | --- |
| 分辨率 / 画幅 | 1080×1920，9:16 竖屏 | ffprobe；`lib.mjs` `W/H` |
| 帧率 / 帧数 | 30 fps；`round(221.096×30)` = 6633 帧 | ffprobe；`render.mjs:227`（推算） |
| 时长 | 221.100 s（timeline 221.096 s，约 3:41） | ffprobe；`build/timeline.json` |
| 视频 | H.264，yuv420p，CRF 19，preset medium，`+faststart` | ffprobe（codec）；`render.mjs:224-225` |
| 音频 | AAC 48 kHz 立体声 192 kbps | ffprobe；`render.mjs:225` |
| 文件大小 | 32,812,667 B（约 32.8 MB） | 证据包 |
| 字幕 | `out/化石侦探社.srt` 127 条（7259 B）+ 画面烧录 | timeline cue 计数；文件 |
| 其他版本 | 无（无推荐版、封面） | `out/` 列表 |

时间线：会话 14:27 → `script.json` 最后修改 14:40 → TTS（timeline/SRT）14:47 → `music.wav` 15:01 → 成片 15:04。

一句话管线：

> `script.json`（36 段：旁白“侦探社”晓晓 + 学生小林 + 贝壳化石“老壳”+ 再搬运化石“穿越壳”）→ `tts.py` 调 edge-tts，WordBoundary 取逐词时间 → `build/timeline.json` + `narration.wav`（adelay/amix/loudnorm）+ SRT → `render.mjs` 加载 `scenes-a…d`（12 个场景），`cast.mjs` 画角色与地层道具，`audio.mjs` 程序合成配乐/音效 → RGBA 帧经 stdin 管道送 ffmpeg → MP4。

## 2. 教学目标与课标对齐

- **curriculum**：人教版必修第一册 第一章第三节 地球的历史（化石和地层）；课标 1.2“运用地质年代表等资料，简要描述地球的演化过程”。
- **核心知识点**：化石与地层——化石是什么、怎么保存，怎样用地层和化石判断先后、对比地层、推断古环境。
- **讲解结构（按文案）**：
  1. 导入：山崖岩层像“一摞旧书”，夹着一枚贝壳；小林问“石头里怎么会有贝壳”（`hook`、`stu-q`）。
  2. 立案：三个问题——留下了什么？什么时候？什么环境？（`case`）
  3. 案一·留下了什么：教材定义（遗体、遗物、遗迹），脚印虫穴也算；倒带到海底——软体分解、快速埋藏只提高机会、化石记录不完整；压实胶结成岩 vs 矿物填充/替代保存是两件事（`def`…`rock-3`）。
  4. 案二·什么时候：地层定义；正常层序先下后上（“交作业”比喻）只给先后、厚度不代表时长；标准化石“限时邮戳”对比两地地层、限定年代范围；排查再搬运的“穿越壳”；化石不能直接报年龄，要靠测年（`stu-when`…`age`）。
  5. 案三·什么环境：原地造礁珊瑚 + 围岩 → 温暖浅海；易错点“深海冷水里也有珊瑚”（`env-0`…`env-trap`）。
  6. 迁移：去地质公园当侦探的三问（`trip`）。
  7. 答题三句 + 一句话总结“化石是证人，岩层是档案”+ 结案（`exam`、`sum`、`end`）。
- **科学边界（`based_on`）**：改编自 `geo-story-studio/storage/builds/douyin-fossil-record/v002`（N06“石头里的化石，怎样记录过去？”）。审校边界：埋藏只提高保存机会；成岩与化石保存分开；层序判断需正常层序；标准化石限定年代范围并排查再搬运；相对年代不等于数值年龄；深海也有珊瑚；化石记录不完整；**不使用**喜马拉雅、恐龙判年代、生物演化阶梯等未通过审校的例子。文案逐条落实了这些边界（如 `bury-2`、`rock-3`、`layer-limit`、`drift-check`、`age`、`env-trap`）。
- **知识点与出处**（`numbers` 为 9 条定性表述，无具体数值）：地层定义、化石定义、同时代地层含相同或相似化石（教材）；快速埋藏、硬部易保存、只有少部分生物成化石（NPS）；矿化与替代、无统一时间门槛（NPS）；层序律（教材、NPS）；标准化石（NHM、NPS）；相对年代与数值年龄（NPS）；珊瑚分布（NOAA）。

## 3. 技术与依赖

| 类别 | 内容 | 证据 |
| --- | --- | --- |
| 渲染 | Node.js ESM，`@napi-rs/canvas` 声明 `^1.0.11`，lock 解析为 **1.0.13** | `package.json`；`package-lock.json` |
| npm scripts | 仅 npm init 默认的 `test`（`echo "Error: no test specified" && exit 1`），无实际脚本 | `package.json` |
| TTS | Python `edge-tts` + `aiohttp`；版本未记录（无 requirements） | `tts.py` import |
| 字体 | `fonts.py`（fontTools）抽取圆体-简粗体、苹方-简中粗到 `build/fonts/`；与其他视频同一份文件 | `fonts.py`（sha `a6ac7264e548`） |
| 合成 | 系统 ffmpeg；版本未记录 | `render.mjs:215-226`；`tts.py` |

**声线**（全局 `"rate": 4` 叠加；`script.json` 另有顶层 `"voice": "zh-CN-XiaoxiaoNeural"`，`tts.py` 实际只读 `voices`）：

| 角色 | key | 声音 | 角色 rate / pitch | 实际 rate |
| --- | --- | --- | --- | --- |
| 旁白（侦探社） | `zh` | zh-CN-XiaoxiaoNeural | 0 / 0 | +4% |
| 小林（学生） | `stu` | zh-CN-YunxiNeural | +6 / +2 Hz | +10% |
| 老壳（贝壳化石证人） | `shell` | zh-CN-YunyangNeural | −4 / −8 Hz | 0% |
| 穿越壳（再搬运化石） | `drift` | zh-CN-YunxiaNeural | +10 / +4 Hz | +14% |

`tts.py` 支持按声线加 ffmpeg 滤镜 `fx`，但本片四个声线都没有配置 `fx`。

## 4. 镜头与时间轴

场景边界写成“下一段开始前 0.2 s”，交叉淡化 `XF = 0.3 s`，进场 0.95→1 放大、离场 1→1.04 放大（`render.mjs:23, 173-186`）。时间取自 `build/timeline.json`。

| 场景（draw 函数） | 约时间 | 旁白要点 | 画面 | 模块 |
| --- | --- | --- | --- | --- |
| `hook` | 0–9.6 s | 岩层像一摞旧书，夹着贝壳；小林发问 | 书页状地层崖壁、闪光的贝壳 | `scenes-a.mjs:9` |
| `caseFile` | 9.6–17.4 s | 侦探社开案，三个问题 | 案卷、三个编号标签 | `scenes-a.mjs:57` |
| `witness` | 17.4–39.2 s | 老壳出场；教材定义；遗体 / 遗迹举例 | 贝壳证人、骨骼、脚印、虫穴 | `scenes-a.mjs:94` |
| `seabed` + `rewind` | 39.2–67.1 s | 倒带回海底；软体分解、硬壳留下；快速埋藏；记录不完整 | 倒带特效、海底、泥沙覆盖、微生物 | `scenes-b.mjs:20, 9` |
| `rock` | 67.1–90.2 s | 压实、胶结成岩；矿物填充或替代；两件事、无统一门槛 | 颗粒压实图 + “壳体放大”圆窗 | `scenes-b.mjs:179` |
| `layers` | 90.2–117.1 s | 地层定义；正常层序先下后上；只给先后、厚度≠时长 | 6 层地层逐层亮起编号，老壳在第 3 层 | `scenes-c.mjs:12` |
| `index` | 117.1–148.0 s | 相隔几百公里的两地；标准化石三条件；穿越壳 | 甲地 / 乙地两座山的剖面、金色对比连线、“限时邮戳” | `scenes-c.mjs:95` |
| `age` | 148.0–160.0 s | 化石不能直接说几岁；先后 + 范围 vs 测年 | “不能直接读年份”印章、两张对比卡 | `scenes-c.mjs:175` |
| `env` | 160.0–189.1 s | 原地造礁珊瑚 + 围岩 → 温暖浅海；深海冷水也有珊瑚 | 岩石面板、活珊瑚与鱼 | `scenes-d.mjs:32` |
| `trip` | 189.1–199.6 s | 地质公园 / 博物馆当侦探 | 三问清单 | `scenes-d.mjs:92` |
| `exam` | 199.6–210.3 s | 答题记三句 | 三行答题卡；资料来源行出现 | `scenes-d.mjs:123` |
| `finale` | 210.3 s–结束 | 化石是证人，岩层是档案；结案 | 总结 + 彩纸 | `scenes-d.mjs:153` |
| 品牌卡 | 约 218.3–221.1 s | 无旁白 | 重绘背景 + 中央 logo + “化石是证人，岩层是档案 / 层序定先后 · 标准化石定范围 · 多证据推环境” | `render.mjs:128-149` |

4 个模块共 12 个场景（a:3、b:2、c:3、d:4），每个模块 176–250 行，符合 AGENTS.md 第 3 节“每个文件 2～7 个场景”。

## 5. 关键算法与实现

> 区分：视频**讲的科学**是化石定义、保存条件、地层层序律、标准化石和古环境推断；下面是**做视频用的算法**。

**5.1 逐词时间戳 + 词尾时刻（`lib.mjs:36-46`）**。与项目标准实现相同，`at(id, 词)` 取词首时刻；本片多了 `atEnd(id, 词)` 取词尾。地层逐层“沉积”的节奏就是把 6 层均匀分布在“先沉积”到“在上”这两个词之间（`scenes-c.mjs:18`）：

```js
const tBuild = (i) => lerp(at('layer-rule', '先沉积') - 0.2, atEnd('layer-rule', '在上') + 0.2, (i + 0.8) / 6);
```

于是旁白念到“先沉积的在下，后沉积的在上”时，1→6 号层按自下而上的顺序依次闪亮、弹出编号。

**5.2 程序化地层 `strata()`（`cast.mjs:165-196`）**。按相对厚度数组 `hs` 自下而上堆叠色带，每层顶面用 `sin(px·0.018 + i·1.7 + seed)` 画轻微起伏，层内按种子撒深浅颗粒，`pages` 选项画“书页线”；`reveal` 可以是小数，实现逐层长出。函数返回每层 `{top, bottom, mid}`，供角色（老壳在第 3 层）、标准化石和对比连线定位。同一个函数既画开场“旧书”崖壁，也画甲乙两地剖面（`scenes-c.mjs:101-102` 用不同 `seed`）。

**5.3 “填充”与“替代”分开演示（`scenes-b.mjs:145-178`）**。圆形放大窗里，念到“填进”时孔隙从中心逐个被矿物色填满（每个孔延迟 0.15 s），念到“替代”时壳体底色在 1.6 s 内从贝壳色混合到矿物灰绿并撒出晶体；配合 `rock-3` 旁白“成岩和保存是两件事”，用画面把两种保存方式区分开。

**5.4 学生小窗与镜头震动（`render.mjs:102-117, 155-166`）**。小林每次发言时从左下角升起（`lerp(1560, 1330, a)`），嘴型由 `talkLevel` 按该句词时间开合；`shakeAt()` 会读取事件里的 `shake` 字段做衰减震动，但本片的 `events` 都没有设置 `shake`（检索 `scenes-*.mjs` 无结果），这段机制在本片中不起作用。

**5.5 片尾品牌卡 `endCard()`（`render.mjs:53-69, 128-149`）**。`CARD_T = E('end') + 0.5`；角标 logo 在 `CARD_T` 前 0.3 s 内淡出，品牌卡先以透明度 p 重绘整幅背景盖住场景，再画 760 宽 logo（0.9→1 放大）和两行标语；最后 0.6 s 淡出到黑（`render.mjs:193`）。`TAIL = 3.2 s`（`tts.py`），品牌卡约 2.8 s。与 [[plateau-basin-sunlight]] 的 `outro()` 不同，这里品牌卡出现后字幕没有显式停止，但最后一句 `end` 在 `CARD_T` 前 0.5 s 已结束，实际不会重叠。

## 6. 声音与字幕

- **TTS**：缓存键 `sha1(voice|rate|pitch|text)[:16]`，`build/tts_cache/` 现有 72 个文件（36 段 × mp3/json）；429 退避重试 5 次；WSS 地址改用 `api.msedgeservices.com`；`LEAD_IN = 0.6 s`（注释“Douyin hook: speak almost immediately”），默认段间 0.45 s，`TARGET = None`。
- **旁白混音**：`aresample → [fx] → adelay` → `amix normalize=0` → `highpass=80` → `loudnorm=I=-16:TP=-1.5:LRA=11`（`tts.py:178-191`）。
- **成片混音**：音乐 0.75 经旁白侧链压缩避让，音效 0.5，`amix` + `alimiter=0.93`（`render.mjs:219-222`，注释“music ducks under the voice”）。
- **配乐**：`audio.mjs` 程序合成，112 BPM，C–G–Am–F 拨弦 + 低音 + 沙锤 + 隔小节钟琴，种子固定。
- **音效**：场景事件共约 66 个，常用 pop 11、stamp 9、note 8、whoosh 7、boing 6、ding 5、sparkle 5；也用了 `fizz`、`thud`、`wind`（这几个来自共用的 `audio.mjs`，注释原本写的是火花熄灭、小石块落地）。
- **字幕**：127 条，起止取自所覆盖词的首尾时间；画面字幕 `SUB_Y = 1500`（注释“above Douyin's bottom caption/UI area”），最大 58 号自动缩小，小林 / 老壳 / 穿越壳带彩色名牌（`render.mjs:71-100`）。
- **资料来源行**：从 `exam` 起显示“资料：人教版地理必修第一册 · NPS · NHM · NOAA”（`render.mjs:119-126`），没有“示意”类声明。

## 7. 迭代与问题

- **复制残留**：`tts.py:3-4` docstring 写着“a voice may carry an ffmpeg audio filter (`fx`, e.g. Neptune's far-away echo)”，这句与 [[solar-system-scale]] 的 `tts.py` 相同，是从那里继承的说明，本片没有海王星角色。`audio.mjs`（sha `9e3eeae951b5`）与 [[meteor-journey]]、[[earth-core]]、[[geologic-day]] 等另外 9 个视频逐字相同，其中 `boom`、`roar`、`sizzle` 的注释仍是流星主题（“the flash of plunging into the atmosphere”“好烫”）。不影响成片，但说明脚本是整套复制后修改的（推断）。
- **`based_on` 写的是本机绝对路径**：`script.json` 的 `based_on` 以个人目录开头（此处按项目规范改写为 `geo-story-studio/storage/builds/douyin-fossil-record/v002`）。AGENTS.md 只要求“源路径写进 based_on”，没有规定相对 / 绝对路径。
- **`script.json` 顶层 `voice` 字段**未被 `tts.py` 使用（只读 `voices`），属冗余字段。
- **README**：已收录（`README.md:43`，标 3:41）。
- **禁用声明**：源码中没有“示意”类文字（仅 `render.mjs:119` 注释引用规则）。
- 制作过程、返工次数、是否做过 `--still` 预览：均**未记录**（`build/` 中没有 still 图或日志）。git 只有批量提交 `8cbe673`（2026-11-01）。

## 8. 复现入口

依据 AGENTS.md“常用命令”和 `render.mjs:1-3` 头注释（本次**均未运行**）：

```bash
cd fossil-record
python3 -m venv .venv && .venv/bin/pip install edge-tts   # fonts.py 还需要 fontTools（文件中未声明）
npm install
.venv/bin/python tts.py            # → build/seg*.mp3, build/narration.wav, build/timeline.json, out/化石侦探社.srt
node render.mjs --still 3.2 7.6    # → build/still_<t>.png
node render.mjs                    # → build/music.wav, build/sfx.wav, out/化石侦探社.mp4
```

必须先跑 TTS：`lib.mjs` 加载时就读 `build/timeline.json`。

## 9. 参考来源

`script.json` `sources` 列出 10 个 URL（项目文件中提及，本次未访问）：

| 来源 | URL | 用途 |
| --- | --- | --- |
| 人教版 普通高中教科书 地理 必修第一册 | https://jc.pep.com.cn/ | 化石、地层定义；层序 |
| NPS · About Fossils | https://www.nps.gov/subjects/fossils/about-fossils.htm | 保存条件、记录不完整 |
| NPS · How Fossils Form | https://www.nps.gov/subjects/fossils/how-fossils-form.htm | 快速埋藏、硬部保存 |
| NPS · Permineralization and Replacement | https://home.nps.gov/articles/000/permineralization-and-replacement.htm | 矿化与替代 |
| NPS · What is a Fossil? | https://www.nps.gov/subjects/fossils/what-is-a-fossil.htm | 化石定义、保存方式 |
| NPS · Geologic Timescale, Geologic Dating Techniques, and Numeric Ages | https://home.nps.gov/articles/000/grcatime-timescale.htm | 层序律、相对年代 vs 数值年龄 |
| NPS · Fossils in Parks | https://home.nps.gov/subjects/fossils/fossil-types-in-parks.htm | 标准化石 |
| NHM London · What is a fossil? | https://www.nhm.ac.uk/discover/what-is-a-fossil.html | 标准化石 |
| NOAA · Where are corals found? | https://oceanservice.noaa.gov/education/tutorial_corals/coral05_distribution.html | 造礁珊瑚分布 |
| NOAA Ocean Exploration · Do all corals live in warm water? | https://oceanexplorer.noaa.gov/ocean-fact/coral-water/ | 深海冷水珊瑚 |

改编来源：`geo-story-studio/storage/builds/douyin-fossil-record/v002`（N06 科学审校版，本次未读取）。

## 10. 证据与限制

| 文件 | 行数 | sha256 前 12 位 | 本次读取 |
| --- | --- | --- | --- |
| `script.json` | 75 | `29916f3fe1ee` | 全文 |
| `tts.py` | 210 | `307b8807dc0f` | 与 [[plateau-basin-sunlight]] 版逐行 diff |
| `render.mjs` | 238 | `f35c78e2704e` | 全文 |
| `lib.mjs` | 243 | `202e082ccf99` | 导出清单、`word/atEnd` |
| `scenes-a/b/c/d.mjs` | 176/250/235/204 | `d00718b9840c` / `f98695860f16` / `af0f5477498d` / `5fc8063619ac` | 头注释、场景表、`layers/index/age/pores` |
| `cast.mjs` | 359 | `e34169fd2c25` | 导出清单、`strata()` |
| `audio.mjs` | 229 | `9e3eeae951b5` | 全文 |
| `fonts.py` | 37 | `a6ac7264e548` | 全文 |
| `package.json` / lock | 17 / — | `065efb18222c` / `8648f7848d1e` | 全文 / 版本字段 |
| `build/timeline.json` | — | — | 顶层字段、segment 起止、cue 数 |

未读 / 无法确认：成片画面与声音（未打开）；`cast.mjs` 角色绘制细节；制作对话（transcript 为空）；edge-tts、ffmpeg、Node、Python 版本；用户验收与发布情况；源 build 的审校原文。
