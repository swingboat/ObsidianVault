---
type: video-production-dossier
project: geo-story-studio-opus
video_id: meteor-annual
title: 流星雨为什么能按年赴约？
curriculum: 人教版必修第一册 第一章第一节 地球的宇宙环境（流星体与流星现象）；关联选择性必修1 第一章第一节（公转周期约一年）
captured: '2026-11-12'
status: 成片存在（竖屏 1080×1920，91.68 s）+ cover.png；script.json 标记 approved；项目自带 npm run check，本次未运行；视觉检查/用户验收未记录
source_fingerprint: 290aeb5e3c16ce9bbba532da1580b043d2b3521d067cf0e4baf5fd0142abf64a
tags:
- video-creation
- production-capture
- geography-teaching
- canvas-frame-render
- edge-tts
---

# 流星雨为什么能按年赴约？ · 伪 3D 轨道几何 + 自带几何/时间轴校验器的竖屏短片

<!-- capture-source-sha256:290aeb5e3c16ce9bbba532da1580b043d2b3521d067cf0e4baf5fd0142abf64a -->

本文是根据项目文件做的材料回溯（源码、`script.json`、`build/timeline.json`、`out/` 文件清单与 ffprobe 结果）。按元数据找到一个可能对应的宿主会话 `bce4315b…`（创建于 2026-10-24 11:19），但宿主返回空 transcript，**无法读取对话内容**；这个对应关系是按时间先后**推断**的（会话创建约 5 分钟后出现本目录最早的文件 `audio.mjs`/`fonts.py`/`package.json`，11:24；`script.json` 当前 mtime 17:13 是最后修改时间）。本次**未运行任何项目脚本**（含 `tts.py`、`render.mjs`、`npm run check`），未打开音视频。

## 1. 当前结论与制作路线

**状态**：`out/流星雨为什么能按年赴约.mp4` 与同名 `.srt`、`out/cover.png` 存在。`script.json:3` 写有 `"status": "approved"`，`validate.mjs:12` 会断言这一点——说明文案曾被标为“已确认”，但谁确认、何时确认**未记录**。`npm run check` 是否在出片后跑过、是否 PASS，**无日志，无法确认**。成片存在 ≠ 视觉或用户验收。

| 项 | 值 | 来源 |
| --- | --- | --- |
| 分辨率 / 帧率 | 1080×1920，30 fps | ffprobe；`lib.mjs:6` |
| 时长 | 91.678 s（timeline 91.678） | ffprobe；`build/timeline.json` |
| 帧数 | 2751（`ceil(DURATION*FPS)` 推算） | `render.mjs:160` |
| 视频编码 | H.264，yuv420p，CRF 18，preset medium，`+faststart` | ffprobe；`render.mjs:153-154` |
| 音频 | AAC 48 kHz 立体声，192 kbps | ffprobe；`render.mjs:154` |
| 文件大小 | 14,276,178 B（约 14.3 MB） | 证据包 |
| 字幕 | `out/流星雨为什么能按年赴约.srt`（46 条 cue）+ 画面烧录 | timeline cues 计数；`render.mjs:69-96` |
| 封面 | `out/cover.png`（921,738 B），取 t=4.0 s 一帧 | `render.mjs:140` |
| 时间线 | SRT 17:32:34 → cover 17:34:11 → MP4 17:34:36（2026-10-24） | 文件 mtime |

**管线一段话**：`script.json`（17 句、3 个角色、9 段 storyboard）→ `tts.py` 用 edge-tts 逐句合成并取 WordBoundary 逐词时间，按 `TARGET = 92` 判断是否需要提速（本片 rate 0 一次满足）→ `build/timeline.json` + `narration.wav` + SRT → `render.mjs` 动态加载 `scenes-a…d.mjs`（11 个场景），每帧画背景星空 → 场景（0.3 s 交叉淡化）→ 字幕 → 资料页脚/进度条 → 角标 logo/片尾品牌卡 → RGBA 经管道送 ffmpeg；`audio.mjs` 程序合成配乐与音效，与旁白做侧链压缩混音 → MP4。另有 `validate.mjs` 做时间轴、版式与科学几何断言。

## 2. 教学目标与课标对齐

- **curriculum**（`script.json:4`）：人教版必修第一册 第一章第一节 地球的宇宙环境（天体：流星体与流星现象）；关联选择性必修1 第一章第一节 地球的自转和公转（公转周期约一年）。课标：运用资料，描述地球所处的宇宙环境。
- **核心知识点**：流星雨“按年出现”的原因——地球公转周期约一年，每年相近日期经过轨道上同一位置，穿过母彗星留下的碎屑流。
- **来源说明**（`script.json:5`）：改编自 Geo Story Studio《流星雨为什么能按年赴约？》v002 审阅页；具体 build 路径未写（无 `based_on` 字段），推断对应 `geo-story-studio/storage/builds/…/meteor-annual…`，**未核实**。
- **叙事结构**（storyboard 9 段，`script.json:41-51`）：

| 环节 | 段落 | 内容 |
| --- | --- | --- |
| 情境导入 | A1 hook/tease | “英仙座流星雨怎么年年八月中旬都来？”→“真正准时的，是地球自己” |
| 原理拆解 | A2–B3 | 彗星受热、冰升华带出尘埃 → 碎屑流沿彗星轨道铺开 → 地球公转轨道穿过 → 颗粒冲进大气燃烧发光 → 流星雨与辐射点 |
| 得出规律 | C1 | 日历日期 = 地球在公转轨道上的位置；一年一圈回到交会处 |
| 迁移/辨析 | C2、D1 | 母彗星一百三十多年才回归，流星雨却年年有；碎屑疏密、月光、天气影响每年看到的数量 |
| 总结 | D2 | 答题模板“公转周期约一年 → 每年相近时间 → 穿过同一碎屑流”；地球收尾“是我年年准点路过” |

- **数字与出处**（`script.json:12-17`）：英仙座高峰 8 月 12–13 日（NASA Science）；斯威夫特-塔特尔彗星周期——NASA Perseids 页写 133 年、Facts 页写 135 年，旁白取“一百三十多年”；地球公转约一年。**明确不给**每小时流星数、颗粒速度、发光高度等未核对数字（`numbers.note`）。这是一个有意识的科学边界。
- **科学边界（代码层面）**：`orbit.mjs:12-15` 的彗星轨道（e=0.9、半通径 1.5 AU、倾角 0.95 rad）是为画面构造的教学几何，用于让碎屑流恰好在 1 AU 处穿过地球轨道面；代码注释没有声称它是斯威夫特-塔特尔彗星的实测轨道根数。C2 场景里“地球 133 圈 / 彗星 1 圈”的计数（`scenes-c.mjs:51`）取 NASA Perseids 页的 133。

## 3. 技术与依赖

| 类别 | 内容 | 证据 |
| --- | --- | --- |
| 渲染 | Node ESM，`@napi-rs/canvas` 声明 `^1.0.11`，lock 解析为 **1.0.13** | `package.json`；`package-lock.json` |
| npm scripts | `render` = `node render.mjs`；`check` = `node validate.mjs` | `package.json` |
| Python | `requirements.txt`：`edge-tts>=7.2.3,<8`、`fonttools>=4.60,<5`（本组 5 片中唯一带 requirements 的） | `requirements.txt` |
| TTS | edge-tts + aiohttp（`aiohttp` 未列入 requirements，由 edge-tts 依赖带入——推断），WSS_URL 运行时覆盖 | `tts.py:16-25` |
| 字体 | `fonts.py` 从 macOS 系统字体集合抽取 圆体-简 粗体 / 苹方-简 中粗 到 `build/fonts/` | `fonts.py`；`lib.mjs:8-13` |
| 编码 / 混音 | ffmpeg（libx264、aac、adelay、amix、highpass、loudnorm、sidechaincompress）；版本未记录 | `tts.py`、`render.mjs` |

**声线**（`script.json:19-23`，全局 `rate: 0`）：

| 角色 | voice | rate / pitch | 用途 |
| --- | --- | --- | --- |
| `zh` 旁白 | zh-CN-XiaoxiaoNeural | 0 / 0 | 讲解（10 句） |
| `earth` 地球 | zh-CN-YunxiaNeural | 0 / +4 Hz | 拟人地球（4 句） |
| `grain` 小尘埃 | zh-CN-XiaoyiNeural | +6% / +4 Hz | 彗星尘埃颗粒（3 句） |

## 4. 镜头与时间轴

时间取自 `build/timeline.json`（段落 start/end）；场景边界即下一段落的 `S(id)`，最后一个场景延续到片尾 91.68 s。

| 场景 | 约时间 (s) | 段落 | 画面要点 | 模块 |
| --- | --- | --- | --- | --- |
| nightHook | 0–9.8 | hook, tease | 山野夜空、英仙座放射流星；台历一年年翻回“8月”；地球挠头/得意 | `scenes-a.mjs:44` |
| cometScene | 9.8–19.8 | hello, comet | 小尘埃从彗核蹦出打招呼；彗核受热，冰升华喷出气体带出尘埃 | `scenes-a.mjs:94` |
| streamScene | 19.8–30.6 | stream, wait | 伪 3D 太阳系俯视，颗粒沿彗星轨道越散越开成碎屑流 | `scenes-b.mjs:12` |
| crossScene | 30.6–34.1 | cross | 地球轨道（蓝）× 碎屑流（金），交会点闪烁 | `scenes-b.mjs:44` |
| atmosphereScene | 34.1–42.6 | entry, shine | 大气截面：流星体在太空中暗，越过大气上界才亮；小尘埃“我亮啦”化作光迹 | `scenes-b.mjs:86` |
| showerScene | 42.6–48.4 | shower | 地面视角，64 颗流星自辐射点四散 | `scenes-b.mjs:114` |
| calendarOrbit | 48.4–62.4 | key, lap, again | 轨道外圈标 12 个月；地球绕满一圈回到“8月中旬 · 交会处”，盖“年度打卡”章 | `scenes-c.mjs:25` |
| cometYears | 62.4–71.2 | comet133 | 地球圈数计数器跳到 133，彗星只走 1 圈；碎屑流常亮 | `scenes-c.mjs:52` |
| varyScene | 71.2–78.1 | vary | 碎屑疏密 / 月光 / 天气 三张卡 + 迷你夜空流星数变化 | `scenes-d.mjs:32` |
| examScene | 78.1–84.8 | exam | 答题卡三行荧光笔划重点 | `scenes-d.mjs:59` |
| outroScene + 品牌卡 | 84.8–91.7 | outro | 地球“胜利巡游”+ 彩纸；`OUTRO = E('outro')+0.45 ≈ 88.6 s` 起品牌卡 | `scenes-d.mjs:88`；`render.mjs:20, 51-66` |

共 4 个场景模块、11 个场景；每屏顶部有“01 / … 10 /”编号的章节 header（`lib.mjs:81-89`）。

## 5. 关键算法与实现

> 区分：视频**讲的知识**是流星、流星雨与地球公转；以下是**制作用的算法**（Canvas 2D 绘图、伪 3D 投影、TTS 时间轴、断言脚本），不是天体力学仿真。

### 5.1 伪 3D 轨道几何：让“交会点 = 8 月中旬”成为代码不变量（`orbit.mjs:6-29`）

```js
export const VIEW = { cx: 540, cy: 990, au: 390, ky: 0.55, kz: 0.84 };
export const project = ([X, Y, Z]) => ({ x: VIEW.cx + X * VIEW.au,
  y: VIEW.cy - Y * VIEW.au * VIEW.ky - Z * VIEW.au * VIEW.kz });
const ECC = 0.9, SLR = 1.5, INC = 0.95;
export const NU0 = Math.acos((SLR - 1) / ECC);   // r(NU0) = 1 AU
export const NODE = -0.5;
export const PEAK = (31 + 28 + 31 + 30 + 31 + 30 + 31 + 12) / 365;
export const dateTheta = (frac) => NODE + 2 * Math.PI * (frac - PEAK);
```

- 圆锥曲线 `r = p/(1+e·cosν)`，取 `ν = NU0` 使 r = 1 AU，并把该点旋到升交点，于是彗星轨道恰在地球轨道上穿过黄道面；降交点落在 3 AU 处（屏外），画面上只有一个交会点。
- 日期→黄经是线性映射，8 月 12 日（PEAK）正好落在 `NODE`。碎屑流按 z 正负分成“面前实线 / 面后虚线”两段绘制（`streamPath`，`orbit.mjs:39-55`），制造穿过平面的立体感。
- 颗粒 `streamGrains` 用确定性伪随机 `rand(i)` 分布在 ν 上，`+ t*0.05` 让颗粒始终前移，`spread` 参数让碎屑从一团逐渐铺开（`orbit.mjs:58-73`）。

### 5.2 TTS 词级时间驱动全部动作（`lib.mjs:17-30`、`scenes-c.mjs:11-13`）

`word(id, phrase)` 用段落文本里短语的字符下标，与 WordBoundary 的 `idx` 求交，取第一个词的 start 和最后一个词的 end；`at()`/`endAt()` 是它的简写。完整公转一圈的动画直接锚在语音上：

```js
export const LAP_START = at('lap', '公转');
export const LAP_END = endAt('lap', '交会');
export const lapAngle = (t) => NODE + TAU * ease(prog(t, LAP_START, LAP_END));
```

流星入大气（`ENTRIES`）、计数器（`SPIN_A/SPIN_B`）、台历翻页、音效 `events` 也都按词取时。口型 `talk()` 在该角色的词区间内以 `|sin(21t)|` 开合（`lib.mjs:103-106`）。

### 5.3 自带校验器 `validate.mjs`（`npm run check`）

从代码读出的检查项（本次未运行）：

| 类别 | 断言 | 行 |
| --- | --- | --- |
| 文案 | `script.status === 'approved'`；timeline 段落 id 与 script 一致；storyboard 段落覆盖全部 segments | 12-14 |
| 字幕 | 每段 `captions` 拼接 = 原文；词时间单调；每条 cue 落在段内；字幕字号 `min(56, 900/宽)` ≥ 40 px | 18-28 |
| 场景/音效 | 场景从 0 开始、首尾相接无空隙、终于 DURATION；所有音效事件时刻在片内 | 31-34 |
| 科学几何 | 升交点在 1 AU 且 z=0；另一交点 > 2.5 AU；8 月 12 日的地球位置与交会点重合（误差 1e-9） | 37-42 |
| 公转动画 | `lapAngle` 首尾正好差 2π，200 个采样单调前进且单步 < 0.12 rad | 45-52 |
| 安全区 | monkey-patch `fillText/strokeText`，在全片均匀采样 150 帧，凡无旋转、alpha>0.05 的文字超出左右 24 px 边距即报错 | 55-76 |
| 规则 | 源码中不得出现“科普示意/非观测照片/仅作示意/不按比例” | 78-80 |
| 交付 | SRT 中 ` --> ` 数量 = cue 数；`--video` 时 ffprobe 断言 h264/yuv420p/尺寸/30 fps/AAC 立体声 48 kHz、音视频时长与 DURATION 相差 < 0.06 s | 81-95 |

README 把它描述为“检查时间轴、字幕安全区、完整公转和**大气内发光**的几何约束”。代码里**没有**针对“大气内发光”的显式断言；“越过大气上界才亮”是由 `atmosphereScene` 的 `streak()` 在 `t ≥ b`（到达 `topY(x)` 抛物线上界）后才 `lit` 实现的（`scenes-b.mjs:59, 77-85`），属于绘制逻辑而非校验项——README 与代码有出入。

### 5.4 品牌角标与片尾卡（`render.mjs:44-66, 115-116`）

角标 logo 0.3–1.0 s 淡入（`(64, 76)`、宽 290、径向暗晕）；`OUTRO` 起全屏压暗，中央 logo 宽 820 带回弹，标语“抬头看流星，低头懂地球”+ 资料行；此后字幕停止、角标淡出；首 0.3 s 由黑淡入、末 0.6 s 淡出。

## 6. 声音与字幕

| 项 | 内容 | 来源 |
| --- | --- | --- |
| 合成 | edge-tts，`boundary="WordBoundary"`；`TARGET=92`、`LEAD_IN=0.35`、`GAP=0.3`（逐句 `gap` 覆盖）、`TAIL=3.4` | `tts.py:31-35` |
| 提速逻辑 | 超出 TARGET 时按超出比例一次算出 rate（上限 +30%）；本片 timeline `rate: 0`，未提速 | `tts.py:149-158`；timeline |
| 缓存 / 限流 | key = `sha1(voice|rate|pitch|text)[:16]` → `build/tts_cache/*.mp3/.json`；429 时退避 8/16/24/32 s，最多 5 次 | `tts.py:44-64` |
| 旁白混音 | 逐句 `adelay` → `amix normalize=0` → `highpass 80` → `loudnorm I=-16:TP=-1.5:LRA=11` → `narration.wav` | `tts.py:176-190` |
| 配乐 | 程序合成：112 BPM，C–G–Am–F，Karplus–Strong 拨弦 + 低音 + 沙锤 + 隔小节钟琴，种子 42，淡入 1.2 s / 淡出 2.5 s | `audio.mjs:150-185` |
| 音效 | 18 种程序音效（tick、whoosh、ding、boing、stamp、scribble、confetti、tada…），由各场景 `events` 按词触发 | `audio.mjs:78-147`；`scenes-*.mjs` |
| 总混音 | 音乐 0.45 经旁白侧链压缩（0.035 / 6:1 / 15 / 350 ms），音效 0.35，三路 amix 后再 `loudnorm -16` | `render.mjs:147-151` |
| 字幕切分 | 句内按标点切块，每块 ≤12 字；本片 17 句都在 `script.json` 里手写了 `captions`，优先使用 | `tts.py:106-136`；`script.json` |
| 烧录字幕 | y=1585，字号 `min(56, 900/宽)`，9 px 深色描边；地球（蓝）/小尘埃（金）带角色标签 | `render.mjs:68-96` |

本次未试听、未测响度，以上均为代码设定。

## 7. 迭代与问题

| 事项 | 证据 | 说明 |
| --- | --- | --- |
| README 说 check 含“大气内发光”约束 | README.md:121 vs `validate.mjs` | 代码无对应断言（见 §5.3） |
| `fonts.py` docstring 称“render.mjs 会自动调用”“缺字体回退 Hiragino” | `fonts.py:4, 26` | 本片 `lib.mjs:8-13` 不调用 `fonts.py`，缺字体直接抛错；该 docstring 与另外 4 片共用（同 hash `a6ac7264e548`），后者的 `lib.mjs` 确有自动调用与回退。README 要求首次渲染前手动跑 `fonts.py`，与本片代码一致 |
| 片长上限 | `tts.py:32` `TARGET = 92.0`；git `35ad4da` | 现行 AGENTS.md 第 2 节默认 `TARGET = None`，但该条在 2026-10-26 21:03 才加入；本片制作时（10-24）旧规则是“超过目标时长时一次算出 rate”，不算违规。92 s 的来源**未记录**；rate 0 即满足，未实际提速 |
| 品牌卡参数 | `render.mjs:20, 60` | 品牌卡在末句后 0.45 s 出现、logo 宽 820；现行 AGENTS（“片尾品牌卡（必需）”条款 2026-11-01 才写入）建议约 0.3 s、约 760 px，属轻微偏差 |
| 示意免责声明 | `validate.mjs:78-80` | 有自动拦截；页脚改放“资料：NASA Science · NASA/JPL”（`render.mjs:101`），符合 AGENTS 第 5 节 |

预览帧、校验输出、修改轮次均**未保留/未记录**。

## 8. 复现入口

命令来自 README.md:121、`render.mjs:2-3`、`package.json`，**本次均未执行**：

```bash
cd meteor-annual
python3 -m venv .venv && .venv/bin/pip install -r requirements.txt
npm install                          # @napi-rs/canvas
.venv/bin/python fonts.py            # 首次：抽取字体到 build/fonts/
.venv/bin/python tts.py              # → build/narration.wav, build/timeline.json, out/流星雨为什么能按年赴约.srt
npm run check                        # 时间轴/字幕/几何/安全区断言
node render.mjs --still 3.2 7.6      # 可选：build/still_<t>.png
node render.mjs                      # → out/cover.png + out/流星雨为什么能按年赴约.mp4
npm run check -- --video             # 再查编码与音画时长
```

`build/tts_cache/` 有 28 组缓存：当前 17 句（rate 0，10-24 17:14 生成）全部命中，另 11 组对应已不存在的旧文案——说明合成前后改过台词（具体改动无法还原）。**推断**可离线重建旁白。

## 9. 参考来源

| 来源 | 用途 | 状态 |
| --- | --- | --- |
| https://science.nasa.gov/solar-system/meteors-meteorites/meteor-showers/ | 流星雨机制 | 项目文件中提及，本次未访问 |
| https://ssd.jpl.nasa.gov/sb/meteor_streams.html | 流星群/碎屑流 | 项目文件中提及，本次未访问 |
| https://science.nasa.gov/solar-system/meteors-meteorites/perseids/ | 英仙座高峰日期、母彗星 133 年 | 项目文件中提及，本次未访问 |
| https://science.nasa.gov/solar-system/meteors-meteorites/facts/ | 流星体/流星定义；母彗星 135 年 | 项目文件中提及，本次未访问 |
| Geo Story Studio《流星雨为什么能按年赴约？》v002 审阅页 | 改编来源 | 仅 `script.json:5` 文字提及，无路径 |

同项目相关选题：[[meteor-journey]]、[[comet-tail]]（未在本次比对）。

## 10. 证据与限制

| 文件 | 行数 | sha256[:12] | 读取程度 |
| --- | --- | --- | --- |
| `meteor-annual/script.json` | 60 | `6ce5b14d989e` | 全文 |
| `meteor-annual/tts.py` | 208 | `b190c0b4e8ee` | 全文 |
| `meteor-annual/render.mjs` | 173 | `55efc6c298f6` | 全文 |
| `meteor-annual/lib.mjs` | 124 | `bf9347af1a2d` | 全文 |
| `meteor-annual/validate.mjs` | 96 | `4e273cabd774` | 全文 |
| `meteor-annual/orbit.mjs` | 142 | `c74e0056a46d` | 1-80 行 |
| `meteor-annual/scenes-a…d.mjs` | 139/156/104/129 | `1ac0a081dd5c` / `a37eb065407d` / `3af5b16d83db` / `1e7e926c7190` | 场景表、events、header；scenes-b 大气段、scenes-c 全文 |
| `meteor-annual/chars.mjs` | 172 | `970fe1b24ffb` | 函数清单 |
| `meteor-annual/audio.mjs` | 204 | `f9e04b2cfd54` | 结构与 buildMusic |
| `meteor-annual/fonts.py` / `requirements.txt` / `package.json` | 37 / 3 / 14 | `a6ac7264e548` / `bd834a6a7632` / `ad8486cb1b0f` | 全文 |
| `meteor-annual/build/timeline.json` | — | — | 顶层字段、段落时间、cue 数 |
| `out/*.mp4` / `cover.png` | — | 14,276,178 B / 921,738 B | 仅证据包 ffprobe 与大小 |

限制：制作对话不可读；`npm run check` 结果、预览帧、用户反馈均无记录；Git 中本片随 `1aa541f`（2026-10-26，与另 4 片合并提交）入库，无单独迭代历史。
