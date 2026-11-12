---
type: video-production-dossier
project: geo-story-studio-opus
video_id: earth-core
title: 给地球做B超：没挖穿，怎么知道有地核？
curriculum: 人教版必修第一册 第一章第四节 地球的圈层结构（内部圈层：地震波、莫霍界面、古登堡界面）；课标 1.3
captured: '2026-11-12'
status: 成片存在（竖屏 1080×1920，199.2 s）；技术检查/视觉检查/用户验收未记录
source_fingerprint: 627d8840e16ba785a365f4a0bc7d5ed39f7ad2ea59b1ed45a01df34f1aec1d85
tags:
- video-creation
- production-capture
- geography-teaching
- canvas-frame-render
- edge-tts
---

# 给地球做B超：没挖穿，怎么知道有地核？ · P 波 / S 波拟人赛跑 + 同步绘制“波速—深度图”的竖屏片

<!-- capture-source-sha256:627d8840e16ba785a365f4a0bc7d5ed39f7ad2ea59b1ed45a01df34f1aec1d85 -->

本文是**根据项目文件做的材料回溯**（源码、`script.json`、`build/timeline.json`、`out/` 下的成片与字幕、证据包里的 ffprobe 与 sha256 清单）。证据包把宿主会话 `9176fd71…`（2026-10-31 14:43 创建，标题「地核地震波视频制作」）映射到本片，但**只凭元数据找到**：宿主返回空 transcript，对话内容无法读取，映射关系是按时间戳**推断**的（会话创建早于 `script.json` 数分钟）。本次未运行任何项目脚本（没有跑 `tts.py`、`render.mjs`、npm、pip），也没有打开视频或音频。

## 1. 当前结论与制作路线

**状态：成片存在**——`out/给地球做B超.mp4`（10-31 15:06）+ `out/给地球做B超.srt`（14:50）。只有 ffprobe 媒体头信息；视觉检查、试听、用户验收和是否发布都**未记录**。

| 项 | 值 | 来源 |
| --- | --- | --- |
| 分辨率 / 画幅 | 1080×1920，9:16 竖屏 | ffprobe |
| 帧率 / 帧数 | 30 fps；`round(199.177×30)` = 5975 帧 | ffprobe；`render.mjs`（推算） |
| 时长 | 199.167 s（timeline 199.177 s，约 3:19） | ffprobe；timeline |
| 视频 | H.264，yuv420p，CRF 19，preset medium，`+faststart` | ffprobe；`render.mjs` |
| 音频 | AAC 48 kHz 立体声 192 kbps | ffprobe；`render.mjs` |
| 文件大小 | 31,375,015 B（约 31.4 MB） | 证据包 |
| 字幕 | `.srt` 104 条（6092 B）+ 画面烧录 | timeline cue 计数 |
| 其他版本 | 无 | `out/` 列表 |

时间线：会话 14:43 → `script.json` 14:48 → TTS 14:50 → `music.wav` 15:03 → 成片 15:06。

一句话管线：

> `script.json`（36 段：旁白晓晓 + 学生小林 + “P波”“S波”两位侦察兵）→ `tts.py` 调 edge-tts，WordBoundary 取逐词时间 → timeline + `narration.wav` + SRT → `render.mjs` 加载 `scenes-a…e`（5 个模块、13 个场景），`props.mjs` 画按比例的剖面地球、台站、钻机、足球、鸡蛋、房子、船，`cast.mjs` 画 P/S 角色 → `audio.mjs` 程序配乐/音效 + 震动事件 → ffmpeg → MP4。

## 2. 教学目标与课标对齐

- **curriculum**：人教版必修第一册 第一章第四节 地球的圈层结构（地球的内部圈层：地震波、莫霍界面、古登堡界面，地壳、地幔、地核）；课标 1.3“运用示意图，说明地球的圈层结构”。
- **核心知识点**：利用地震波波速变化划分地球内部圈层。
- **讲解结构（按文案）**：
  1. 导入：半径 6371 km vs 最深钻孔约 12 km（缩成足球不到两张纸厚）；小林问“地幔、地核是谁看见的？”（`hook`…`stu-q`）。
  2. 类比：医生用超声波 → 地球的“超声波”是地震波；台站记录（`nobody`…`quake`）。
  3. 原理：P 波（纵波，快，固液气）像推弹簧；S 波（横波，慢，只过固体）像抖绳子，液体承受不了横向剪切（`meet`…`s-how`）。
  4. 推理：往地心赛跑——约 33 km 两者都加速 → 莫霍界面、地壳；约 2900 km P 变慢、S 消失 → 古登堡界面，外核液态（`go`…`liquid`）。
  5. 证据加固：全球多台站比对，S 波影区、P 波影区（远处又能收到）（`global`、`shadow`）。
  6. 结论：地壳、地幔、地核；外核液态、内核固态（推断）；鸡蛋类比 + 比例提醒；易错点“地幔不是岩浆”（`layers`…`myth`）。
  7. 迁移：先颠后晃、地震预警；考题“海上船只只感到上下颠簸”（`stu-use`…`right`）。
  8. 总结两个界面 + 收尾（`sum`、`end`）。
- **科学边界（`based_on`）**：改编自 `geo-story-studio/storage/builds/douyin-earth-core/v004`（N08“没挖穿地球，怎么知道里面有地核？”）。边界：人类未钻到地核、靠地震波间接推断；P/S 质点振动与传播差异；P 在核幔边界折射形成影区、更远处再出现；S 不过外核与 P 可穿过共同支持外核液态，需多台站多地震资料；影区只是收不到直达波，不等于完全没有信号；幔核界约 2900 km 即古登堡界面；外核液态与内核固态分开，内核固态需更多证据；地幔总体固态，不是岩浆海；比喻只帮助记名称，不代表比例。文案逐条对应（`global`“这可不是一个台站说了算”、`shadow`“更远的地方又能收到它”、`core`“结合更多地震波证据推断”、`egg`“按比例算，地壳比蛋壳还要薄得多”、`myth`）。
- **数字与出处**（`numbers` 13 条，摘要）：

| 数字 | 出处 |
| --- | --- |
| 地球平均半径约 6371 km | 教材及配套图册 |
| 最深钻孔约 12 km（科拉超深钻孔约 12262 m） | geo-story-studio 源页面 `src-geo-b1-c1-s4-earth-interior`；Kola 公开资料 |
| 足球比例下约 0.2 mm（12 ÷ 6371 × 110 mm） | 按上两项换算 |
| 莫霍界面：大陆地下平均约 33 km | 教材 |
| 古登堡界面：约 2900 km | 教材；BGS |
| 地壳平均厚度约 17 km（≈ 0.27% 半径，比蛋壳薄） | 教材 |
| P/S 速度与通过介质、振动方向 | 教材；USGS *Body waves inside the earth* |
| 外核液态、内核固态；P/S 影区 | 教材；IRIS/EarthScope；BGS |
| 地震预警利用纵波先到 | 中国地震局 国家地震烈度速报与预警工程 |

## 3. 技术与依赖

| 类别 | 内容 | 证据 |
| --- | --- | --- |
| 渲染 | Node.js ESM，`@napi-rs/canvas` 声明 `^1.0.11`，lock 解析 **1.1.0**（比前几片的 1.0.13 新） | `package.json`；lock |
| npm scripts | 无 | `package.json` |
| TTS | Python `edge-tts` + `aiohttp`；版本未记录 | `tts.py` |
| 字体 | `fonts.py`（fontTools），同项目通用版本 | sha `a6ac7264e548` |
| 合成 | 系统 ffmpeg；版本未记录 | `render.mjs`、`tts.py` |

**声线**（全局 `"rate": 4` 叠加；顶层 `voice` 字段未被 `tts.py` 使用）：

| 角色 | key | 声音 | 角色 rate / pitch | 实际 rate |
| --- | --- | --- | --- | --- |
| 旁白 | `zh` | zh-CN-XiaoxiaoNeural | 0 / 0 | +4% |
| 小林（学生） | `stu` | zh-CN-YunxiNeural | +6 / +2 Hz | +10% |
| P 波（纵波，“跑得快”） | `p` | zh-CN-YunxiaNeural | +14 / +4 Hz | +18% |
| S 波（横波，“慢一点”） | `s` | zh-CN-XiaoyiNeural | 0 / 0 | +4% |

用语速差表现“纵波快、横波慢”：P 波声线实际 +18%，S 波 +4%（推断为有意设计，文件无注释说明）。

## 4. 镜头与时间轴

交叉淡化 `XF = 0.3 s`，场景进出带 0.95→1 / 1→1.04 缩放（`render.mjs`，与 [[fossil-record]] 同一框架）。

| 场景（draw） | 约时间 | 旁白要点 | 画面 | 模块 |
| --- | --- | --- | --- | --- |
| `hook` | 0–17.6 s | 6371 km vs 12 km；足球不到两张纸厚；小林提问 | 剖面地球 + 放大框、钻机、足球 | `scenes-a.mjs:36` |
| `bscan` | 17.7–30.1 s | 谁也没看见；B 超类比 | 超声扫描画面 | `scenes-a.mjs:91` |
| `quake` | 30.2–38.0 s | 震源发波，全球台站记录 | 剖面地球内扩散的波前、8 个台站 | `scenes-a.mjs:128` |
| `meetP` | 38.0–52.5 s | P 波自我介绍；推弹簧 | P 角色 + 弹簧式质点（顺传播方向） | `scenes-b.mjs:75` |
| `sScene` | 52.5–67.1 s | S 波自我介绍；抖绳子；液体里传不下去 | 绳式质点（垂直传播方向），入水衰减 | `scenes-b.mjs:97` |
| `race` | 67.1–109.5 s | 33 km 都加速→莫霍界面；2900 km P 变慢 S 消失→古登堡界面；外核液态 | 上半屏边跑边画“波速—深度图”，下半屏深度条带赛跑 | `scenes-c.mjs:99` |
| `shadows` | 109.5–126.7 s | 多台站比对；S 影区；P 折射影区、远处再收到 | 地球剖面射线图、彩色影区弧带 | `scenes-d.mjs:55` |
| `model` | 126.7–142.0 s | 三层；外核液态、内核固态 | 分层剖面 + 标签 | `scenes-d.mjs:109` |
| `eggScene` | 142.0–150.2 s | 鸡蛋类比与比例提醒 | 煮鸡蛋剖面 | `scenes-d.mjs:146` |
| `myth` | 150.2–155.8 s | 地幔不是岩浆 | 印章 + 震动 | `scenes-d.mjs:163` |
| `feel` | 155.8–173.8 s | 先上下颠簸、后左右摇晃；地震预警 | 房子先竖向后横向晃动、手机预警 | `scenes-e.mjs:27` |
| `quiz` | 173.8–183.5 s | 海上船只只上下颠簸 | 海面、海床、船 | `scenes-e.mjs:69` |
| `summary` | 183.5 s–结束 | 两个界面对比卡；收尾 | 双行卡片、彩纸；资料来源行 | `scenes-e.mjs:119` |
| 品牌卡 | 约 196.4–199.2 s | 无旁白 | logo + “看不见地心，听得见证据 / 纵波横波当侦察兵 · 两个界面分三层” | `render.mjs` `endCard` |

5 个模块共 13 个场景（a:3、b:2、c:1、d:4、e:3），每个 155–211 行。

## 5. 关键算法与实现

> 区分：视频**讲的科学**是纵波 / 横波性质与两个不连续面；下面是**做视频用的算法**。

**5.1 边跑边画的波速—深度图（`scenes-c.mjs:8-50`）**。上半屏是教材式“地震波波速与深度”图，P、S 两条折线用简化剖面（注释“after the textbook figure / PREM”），在 33 km、2900 km 处有跳变，S 线在 2900 km 截止：

```js
const xd = (d) => (d <= 100 ? X0 + (d / 100) * (XB - X0) : XB + ((d - 100) / 6271) * (X1 - XB));
const PV = [[0, 6.0], [33, 6.8], [33, 8.0], [400, 9.0], [670, 10.3], [2900, 13.7], [2900, 8.0], [5150, 10.3], [5150, 11.0], [6371, 11.3]];
const SV = [[0, 3.5], [33, 3.9], [33, 4.5], [400, 4.9], [670, 5.6], [2900, 7.3]];
```

横轴分段：0–100 km 拉伸到 150 px 宽，其余 6271 km 压到 660 px，并画坐标轴断开符号，这样 33 km 的莫霍界面也看得见。`depths(t)` 给两位侦察兵的深度设关键帧，时刻全部取旁白词：念到“33千米”到达 33 km，念完“2900千米”到达 2900 km；P 在 `liquid` 句内继续跑到 6371 km，S 停在 2900 km。`curve(ctx, pts, upto)` 只画到当前深度，于是曲线和角色同步前进。

**5.2 纵波 / 横波质点（`scenes-b.mjs:30-55`）**。同一条正弦相位 `ph = bx·0.016 − 6t`：P 模式把位移加在 x 方向（±22 px）并在相邻质点间画弹簧圈；S 模式把位移加在 y 方向（±60 px）连成绳子；每个质点下方保留一个半透明“平衡位置”点，用来对比振动方向和传播方向。S 波还传入包络 `env = (x) => (x < 630 ? 1 : lerp(1, Math.exp(-(x - 630) / 35), wp))`（`scenes-b.mjs:131`）：念到“液体”时右侧出现水体，进入水体的横波振幅按指数迅速衰减。

**5.3 影区射线图（`scenes-d.mjs:7-47`）**。地核半径按真实比例 `RATIO.core = 3480/6371`（`props.mjs:9`）。直达射线按直线弦处理：从顶部震源出发、与地核相切的弦对应角距 `2·acos(3480/6371) ≈ 114°`，所以 S 波影区取 `[114°, 246°]`；P 波影区画在 ±110°～145°，穿核 P 波（PKP）在 150°/160°/170° 用 `pkp()` 画成进出地核时偏折的折线。注释写明这是“straight chords”的简化；画面上没有角度数字（检索 `scenes-d.mjs` 只在注释中出现“°”）。真实地震射线是弯曲的，常见资料给出的影区起点约 103°～105°，这里的 114° 是直线几何的结果——画面只示意影区存在和位置，不应拿来读角度（本次判断，文件未说明）。

**5.4 比例与夸张（`props.mjs:9, 26-40`）**。剖面地球的地核、内核按真实比例，地壳厚度却设为半径的 4.5%（真实约 0.27%），文件头注释说明“layers to scale except the exaggerated crust”；旁白在 `egg` 段补一句“按比例算，地壳比蛋壳还要薄得多”，与审校边界“比喻不代表比例”一致。

**5.5 震动事件与品牌卡**。场景事件可带 `shake` 字段，`render.mjs` 的 `shakeAt()` 把所有事件的二次衰减震动叠加到场景层（字幕、logo 不抖）。本片使用了 8 处：震源 `boom` 14、`feel` 段先 `thud` 10（上下颠簸）后 `slam` 16（左右摇晃）等（`scenes-a.mjs:191, 198`；`scenes-e.mjs:152-154`）。品牌卡 `CARD_T = E('end') + 0.5`，做法同 [[fossil-record]]。

## 6. 声音与字幕

- **TTS**：缓存键 `sha1(voice|rate|pitch|text)[:16]`，`build/tts_cache/` 72 个文件；429 退避重试；WSS 地址替换；`LEAD_IN = 0.6 s`、`TAIL = 3.2 s`、`TARGET = None`。`tts.py` docstring 已改为“narrator / student / P wave / S wave”，没有残留。
- **混音**：旁白 `adelay → amix → highpass=80 → loudnorm I=-16:TP=-1.5:LRA=11`；成片音乐 0.75 经侧链避让，音效 0.5，`alimiter=0.93`。
- **音效**：场景事件约 62 个；除常用 pop/note/ding/whoosh 外，用了共用 `audio.mjs` 里原为流星片写的 `boom` 4、`fizz` 2、`sizzle` 1、`thud` 3 来表现地震和波“消失”。
- **字幕**：104 条；`SUB_Y = 1500`；小林 / P波 / S波带彩色名牌；起止取逐词时间。
- **资料来源行**：从 `sum` 起显示“资料：人教版地理必修第一册 · USGS · BGS · IRIS”。

## 7. 迭代与问题

- **影区角度简化**：见 5.3，代码按直线弦取 114°，与常见资料的约 103° 不同；画面没有标角度，风险有限，但截图用于讲题时需注意（本次判断）。
- **数字出处引用了源项目页面**：`numbers` 中“约 12 千米”的出处写作“geo-story-studio 源页面 src-geo-b1-c1-s4-earth-interior；Kola Superdeep Borehole 公开资料”，没有给出 Kola 的具体 URL。
- **`based_on` 写的是本机绝对路径**（此处改写为 `geo-story-studio/...`）。
- **共用音频文件的注释残留**：`audio.mjs`（sha `9e3eeae951b5`）与 [[meteor-journey]]、[[fossil-record]] 等相同，`boom` 注释仍为“the flash of plunging into the atmosphere”。
- **依赖版本漂移**：本片与 [[earth-spheres-water-trip]] 的 lock 为 `@napi-rs/canvas` 1.1.0，前几天的片子为 1.0.13；声明都是 `^1.0.11`。没有记录这是否影响渲染。
- **README**：已收录（`README.md:49`，标 3:19）。
- **禁用声明**：源码中没有“示意”类文字；画面上没有中国地图。
- 返工过程、预览帧检查：**未记录**（`build/` 无 still 或日志）。git 只有批量提交 `8cbe673`（2026-11-01）。

## 8. 复现入口

依据 AGENTS.md“常用命令”和 `render.mjs` 头注释（本次**均未运行**）：

```bash
cd earth-core
python3 -m venv .venv && .venv/bin/pip install edge-tts   # fonts.py 另需 fontTools
npm install
.venv/bin/python tts.py            # → build/timeline.json, build/narration.wav, out/给地球做B超.srt
node render.mjs --still 3.2 7.6    # 预览帧 → build/still_<t>.png
node render.mjs                    # → out/给地球做B超.mp4
```

## 9. 参考来源

`script.json` `sources` 列出 4 个 URL（项目文件中提及，本次未访问）：

| 来源 | URL | 用途 |
| --- | --- | --- |
| 人教版 地理必修第一册 | https://jc.pep.com.cn/ | 两个界面、深度、圈层名称、易错点 |
| USGS · Body waves inside the earth | https://earthquake.usgs.gov/earthquakes/events/1906calif/18april/earthwaves.php | P/S 波性质与振动方向 |
| BGS · What causes earthquakes? | https://www.bgs.ac.uk/discovering-geology/earth-hazards/earthquakes/what-causes-earthquakes/ | 古登堡界面、影区、地幔固态 |
| IRIS / EarthScope · Exploring the Earth Using Seismology | https://www.iris.edu/hq/inclass/fact-sheet/12 | 外核液态、内核固态证据 |

另有无 URL 的出处：Kola Superdeep Borehole 公开资料；中国地震局 国家地震烈度速报与预警工程。改编来源：`geo-story-studio/storage/builds/douyin-earth-core/v004`（N08 科学审校版，本次未读取）。

## 10. 证据与限制

| 文件 | 行数 | sha256 前 12 位 | 本次读取 |
| --- | --- | --- | --- |
| `script.json` | 73 | `717b4ebdf2ea` | 全文 |
| `tts.py` | 210 | `5bc0a71cb654` | 与 [[plateau-basin-sunlight]] 版逐行 diff |
| `render.mjs` | 236 | `3a9f79260cf1` | 与 [[fossil-record]] 版逐行 diff |
| `scenes-a…e.mjs` | 201/160/155/211/167 | `0cc6c53ad9c3` / `98a9b2c954ac` / `608ee04e349f` / `9265233a58e7` / `f7e757f9ad03` | 头注释、场景表；`race`、`particles`、影区几何片段 |
| `props.mjs` | 215 | `75c8ba37d11d` | 导出清单、`RATIO`、`layers` 开头 |
| `cast.mjs` | 250 | `0df1660b0556` | 导出清单 |
| `lib.mjs` | 244 | `13cd639fab66` | 未细读（与 [[fossil-record]] 同结构，推断） |
| `audio.mjs` | 229 | `9e3eeae951b5` | 与 [[fossil-record]] 相同 |
| `package.json` / lock | 10 / — | `67750a0f5309` / `857ee7d00aa7` | 全文 / 版本字段 |
| `build/timeline.json` | — | — | 顶层字段、segment 起止、cue 数 |

未读 / 无法确认：成片画面与声音；`cast.mjs` 角色细节；制作对话（transcript 为空）；工具版本；用户验收与发布；源 build 审校原文。
