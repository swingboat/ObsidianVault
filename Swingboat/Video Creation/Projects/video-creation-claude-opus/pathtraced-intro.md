---
type: video-production-dossier
project: video-creation-claude-opus
video_id: pathtraced-intro
captured: '2026-11-12'
status: 生成脚本与渲染/后期脚本齐全；项目内无 intro.tsrct、无 build 产物、无 output.mp4，成片是否导出过无法确认
source_fingerprint: 96325b8715828f4448e199bd21edf1a9c9e577378117e009d533dfce57b40181
tags:
- video-creation
- production-capture
- tesseract
- path-tracing
- procedural-music
---

# 浑天仪机械地图片头复刻 · Tesseract CLI 路径追踪 + Node 程序化建模与配乐

<!-- capture-source-sha256:96325b8715828f4448e199bd21edf1a9c9e577378117e009d533dfce57b40181 -->

> [!note] 资料来源
> 未找回制作对话，结论来自项目文件（`prompt.md`、`plan/`、`gen/`、`music/`、四个 shell 脚本）以及本次对参考片 `reference/ref.mp4` 的 ffprobe 和两条 X 帖子的联网访问。项目 git 只有两次提交（初次整体提交 + .gitignore 修正），无法据此还原迭代过程。

## 1. 当前结论与制作路线

### 1.1 状态（按证据等级）

| 证据等级 | 结论 |
|---|---|
| 代码/配置中存在 | 是。10 个镜头模块 `gen/shots/s01…s10`、共享库 `gen/lib/*`、字幕 `gen/credits.mjs`、配乐 `music/compose.mjs`，以及 `build.sh` / `dev.sh` / `render.sh` / `post.sh` 已完整写出 |
| 日志/缓存显示执行过 | 只有间接证据：`plan/0b-armillary.md` 第 4 行写有标定结论「64spp 1080p 约 8s/帧；灯光功率要远小于 60kW（会过曝）」，说明做过标定渲染；`gen/shots/s02-sun-core.mjs:28` 等注释显示踩过 Tesseract 行为的坑。`.tesseract-work/`（导出日志）、`build/`（纹理、music.wav、actions、segments）都不在项目内，`build/` 是空目录 |
| 最终采用 | 无法确认。`intro.tsrct`（可编辑工程）与 `output.mp4` 在架构文档中列为交付物，但两者都被 `.gitignore`（`*.tsrct`、`*.mp4`）排除，当前工作区不存在 |
| 技术检查通过 | 未记录。`post.sh` 末尾有 ffprobe 自检命令，但没有它的输出记录 |
| 视觉检查通过 | 未记录。架构文档计划「抽帧对比原片」（`plan/00-architecture.md:81`），未找到结果 |
| 用户验收 | 未记录 |

> [!warning] 成片状态
> 项目内**没有成片 MP4**。唯一的 MP4 是参考片 `reference/ref.mp4`（原片，非本项目产物）。缺失的中间产物不代表从未生成，但本档案无法证明全片曾被渲染或导出。

### 1.2 规格

| 项目 | 目标值（代码） | 参考原片（本次 ffprobe `reference/ref.mp4`） |
|---|---|---|
| 分辨率 | 1920×1080（`build.sh:16` → `gen/setdoc.mjs`） | 1920×1080，H.264 High，yuv420p |
| 帧率 | 30 fps（`post.sh:15` `-r 30`；`prompt.md:10`） | 30/1，1145 帧 |
| 时长 | 38.2 s（`setdoc` 参数、`render.sh` 镜头表合计 38 200 ms） | 视频流 38.167 s；容器 38.193 s |
| 画幅 | 16:9，无黑边（代码未加 letterbox） | 16:9 |
| 音频 | 48 kHz 立体声；合成 WAV 16-bit（`music/compose.mjs:1`），最终 AAC 256k，loudnorm I=−16 LUFS（`post.sh:16`） | AAC LC 48 kHz 立体声 |
| 编码 | H.264 libx264 preset slow CRF 16，yuv420p，faststart（`post.sh:15-16`） | 码率约 4.46 Mb/s，文件 21 270 655 字节，encoder Lavf62.12.102 |

### 1.3 一句话管线

需求（复刻 X 帖子里的 Mirage 宣传片）→ 抽帧拆镜头表（`plan/00-architecture.md` §1）→ Node 生成器把每个镜头写成 Tesseract action JSON（几何全部程序化：齿轮/星盘/车削/批量方块；纹理用自写 PNG 编码器生成）→ `tsrct project apply` 组装成单一 composition `main` 的可编辑工程 `intro.tsrct` → 关键帧动画（相机 aim 约束、升起、旋转）→ 程序合成原创弦乐 `build/music.wav` 作为 Audio 图层 → `tsrct export` 按镜头分段导出 ProRes（路径追踪，无降噪）→ ffmpeg `hqdn3d` 时域降噪 + concat + 混音 + loudnorm → `output.mp4`。

## 2. 创作目标与需求

### 2.1 原始需求（摘录 `prompt.md`）

> 请先review下： https://x.com/cfenghub/status/2118212495991361715 这个帖子和里面的视频，然后分析下它使用到的技术，最后制作一个一摸一样的视频出来。

追问后的决定（摘录 `prompt.md:5-11`，原文要点）：

- 渲染引擎：和帖子中的视频保持一样的技术（Mirage Tesseract CLI + 原生 GPU 路径追踪，不用 Blender、不用视频模型）。
- 文字与品牌：原样照抄（TESSERACT 3D / MIRAGE + 原演职员名单）。
- 音乐：程序合成原创弦乐，暗色大提琴 6/8 固定音型、节奏点和情绪一致，旋律原创（不复制《权力的游戏》主题）。
- 规格：1920x1080 30fps，和原片一致。
- 降噪器：公司网络拦截了 captions-cdn.xyz，无法下载 OIDN 权重 → 用 `--no-denoise` + 更高采样，再用 ffmpeg 做时域降噪。

### 2.2 目标风格（本次整理，依据 `plan/00-architecture.md:3,23-25`）

- 《权力的游戏》片头式：浑天仪环带 + 太阳核心 + 羊皮纸地图上从齿轮机构里升起的城市地标（纽约、伦敦、迪拜、东京）。
- 暖棕/金铜色调、强逆光、景深、辉光（bloom）、暗角、轻微颗粒。
- 字体：Trajan 风格衬线大写 + 小型大写 → 用 Cinzel（OFL）替代。
- 字幕：职务小号大字距、姓名大号；每组首行左侧一个金色「方框套方框」图标。

### 2.3 完成范围与未完成事项

- 已覆盖：11 段时间轴中的 S01–S10（3D 场景）+ 5 组演职员字幕 + 终版字卡（S11 由 2D 文本图层实现，没有单独的 shot 模块）+ 配乐。
- 计划中但代码未实现：**暗角**与**颗粒**（`plan/00-architecture.md:25,58` 提到；在 `gen/`、`post.sh` 中检索不到 vignette/grain/noise 后期处理）。bloom 用每个 Scene3D 图层的 `glow` 效果实现。
- 计划中命名但未落地的文件：`gen/lib/ids.mjs`（实际合并进 `gen/lib/act.mjs` 的 `idSpace`）；`tex.mjs` 里计划的 `skylineHeight / forestHeight / parchmentNoise` 实际为 `bandNYC / bandGold / sunCorona / creditIcon`，羊皮纸噪声纹理没有实现。

## 3. 技术与依赖

| 组件 | 版本 / 参数 | 用途 | 来源 |
|---|---|---|---|
| Tesseract CLI（`tsrct`，Mirage） | 0.5.0；action wire format `fxSchemaVersion 86` | 创建/检出/提交工程、导入字体与资产、apply action、`preview`、`export`；原生 GPU 路径追踪渲染器 `--renderer path-tracer`，2D 段用 `--renderer raster` | `plan/00-architecture.md:29`；`gen/lib/act.mjs:1`；脚本中 CLI 路径为用户级应用支持目录下的 `Tesseract/bin/tsrct` |
| Tesseract skill 文档 | `tesseract-3d`（用户级 agent skills 目录） | 写 action 时对照 schema | `plan/00-architecture.md:29`（只记录了位置，未记录版本） |
| Node.js（ESM `.mjs`） | 版本未记录；无 `package.json`、无第三方依赖 | 全部生成脚本；PNG 用 `node:zlib` 自写编码 | `plan/00-architecture.md:30`；`gen/lib/png.mjs` |
| ffmpeg / ffprobe | 版本未记录 | 时域降噪 `hqdn3d`、ProRes 中转、concat、loudnorm、H.264/AAC 封装 | `post.sh` |
| 降噪器 | 未使用。OIDN 权重下载被公司网络拦截 | 全部渲染 `--no-denoise`，靠提高采样 + ffmpeg 时域降噪 | `prompt.md:11`；`render.sh:3` |
| 字体 | Cinzel（`assets/fonts/Cinzel.ttf`，sha256 前 16 位 `f4d83d34d1f6c741`；OFL 1.1，Copyright 2020 The Cinzel Project Authors） | 3D 刻字（地图标签、钟面罗马数字、Logo）与 2D 字幕；需要 Regular 与 Bold 两个 style | `assets/fonts/OFL.txt:1`；`plan/0b-credits-music.md:4`。字体内部版本号本次未读取（缺 fontTools） |
| 渲染采样 | 地图镜头 96 spp；浑天仪/Logo 镜头 128 spp；S11 raster | 终渲 | `render.sh:11-21` |
| 预览采样 | 16 spp | 单镜头开发循环 | `dev.sh:7,24` |
| 硬件 | 未记录（GPU 型号、内存均无） | — | — |
| 模型 | 无生成式图像/视频/语音模型；无 TTS、无配音 | — | `prompt.md:7`、代码中无相关调用 |

> 帖子与本项目都把制作说成「Opus 5.5 + Tesseract」。本项目文件未记录制作时所用的模型名称与推理参数。

### 3.1 工程文档约定（`plan/00-architecture.md` §4 + `gen/lib/act.mjs`）

- 单一 composition `main`，1920×1080，38.2 s，黑底（`gen/setdoc.mjs`）。
- **每个镜头一个独立 Scene3D 图层**（layerId = 100+镜头号），`activeRange` = 镜头窗口；场景小、互不干扰，关键帧 `layerTime` 用镜头内毫秒。
- `pixelsPerMeter = 100`；坐标 X 右、**Y 向下**、Z 向屏幕内；材质颜色用线性 BT.709（`mat.mjs` 把 sRGB hex 转线性），灯光/2D 颜色用 sRGB。
- ID 分配：场景内 item/data/material ID = 镜头号×10000 + 递增序号，全 composition 唯一；字幕图层 500+；媒体图层 900+、Audio 990。
- 图层顺序：后创建的图层插在上面，所以 `gen/main.mjs:14-16` 把镜头**倒序**生成，让 S01 在最底层；字幕最后生成，压在所有 Scene3D 之上。
- 每个镜头一个 action 批次文件 `build/actions/NN-<key>.json`，`apply` 原子执行，失败即停（`build.sh:25-28`）。

## 4. 镜头与时间轴

镜头时间与内容描述来自 `plan/00-architecture.md` §1（原片拆解，时间取自 `reference/t2_*.jpg` 联系表）；「实现参数」来自对应 shot 文件。时间为项目时间（秒），镜头内关键帧为毫秒。

| # | 时间 (s) | 内容 | 实现入口 | 关键参数（代码） |
|---|---|---|---|---|
| S01 | 0.0–3.3 | 浑天仪主环带特写：暗红底 + 银色纽约天际线/祥云浮雕，环带缓慢滑动，背后太阳；末尾暗色横梁扫过转场 | `gen/shots/s01-band-nyc.mjs:12-31` | 相机 (−60,150,−1500)→(−40,140,−1440)，看 (0,−10,−620)，50 mm f/2.8 roll −16°；主环 R620 W150 T16 纹理 `band_nyc` reps 4，rotationY 0→−9°；内环 rotationY 0→14°；太阳 power 2500；前景横梁 box 3200×70×70 在 2600–3300 ms 从 (−1400,700) 扫到 (−200,−80)（ease `in`）；glow 75/60/0.9 |
| S02 | 3.3–6.8 | 浑天仪内部：太阳核心，多层环在前景交错转动 | `gen/shots/s02-sun-core.mjs:11-33` | 相机 (−60,110,−820)→(40,70,−700)，26 mm f/4 roll −10°；4 条 band + 3 条细环，各绕不同轴转 20–45°；太阳 coronaSize 950、coronaStrength 5、coreStrength 30；glow 70/80/1.0；字幕 3D ENGINE / Yusuke Shibata（4.0–6.5 s） |
| S03 | 6.8–11.9 | 纽约地图俯冲；中心星形齿盘升起自由女神像，镜头下压到雕像 | `gen/shots/s03-new-york.mjs:34-81` | 相机 4 个关键点（0/1800/3300/5100 ms），35 mm f/4；三块陆地（曼哈顿 blob 旋 25°、大陆、新泽西）+ 城市方块 900/700/500；72 齿外齿圈 + 两层 8 角星盘反向转（30°/−60°/90°）；女神组 1700–3400 ms 从 +520 升起并 rotationY 160→0；5 个周边齿轮；字幕 2 组（9.9–11.9 s） |
| S04 | 11.9–13.3 | 伦敦：罗马数字钟立在齿轮底座、大本钟、圣保罗穹顶、伦敦眼、议会大厦 | `gen/shots/s04-london.mjs:14-76` | 相机 (−640,−400,−1080)→(−560,−385,−1010)，32 mm f/4.5；北/南两岸 + 泰晤士河；钟环 ring 215/150 + 12 个 Cinzel Bold 数字；时针 300→310°、分针 40→100°；伦敦眼 16 辐条 24 座舱 rotationZ 0→6°；`LONDON` 标签 |
| S05 | 13.3–16.3 | 机械装置正视：齿轮列、金色钟形球在摆臂上摆动、铆钉横梁 | `gen/shots/s05-machinery.mjs:11-61` | 相机 (300,−200,−2050)→(160,−230,−1880)，30 mm f/4；上排 11 个齿轮（大/小交替，角速度比 1:−1.46）、下排 9 个；3 根工字梁 + 铆钉；4 个钟形摆 rotationZ ±14° 往返 + 自转 160°；3 个圆窗框；字幕 3 组（13.5–16.3 s） |
| S06 | 16.3–19.3 | 迪拜：棕榈徽章、齿轮、哈利法塔升起，「DUBAI」；末尾仰看塔尖 | `gen/shots/s06-dubai.mjs:19-65` | 相机 3 关键点，末段目标上移到 (160,−880,220) 实现仰摇；30 mm f/4.5；徽章 = 黑盘 + 28 尖星形金刺 + 环 + 7 片棕榈叶 extrude；哈利法塔 8 段 Y 形三翼（glassBlue）+ 银色芯 + 尖顶 + 发光信标，150–1800 ms 从 +1000 升起并转 90°；3 台吊塔；字幕 3 组（16.6–19.1 s） |
| S07 | 19.3–20.8 | 东京：五重塔逐层升起、鸟居、「TOKYO」 | `gen/shots/s07-tokyo-pagoda.mjs:12-54` | 相机 (−480,−900,−980)→(−420,−840,−900)，32 mm f/4.5；城市方块 2200；5 层塔，每层延迟 170 ms、520 ms 内从 +400 落位并旋入（90°−15°×层）；相轮 920–1400 ms 落下；鸟居 + 5 级石阶 |
| S08 | 20.8–23.7 | 东京正俯视：菊花机构、齿轮、东京塔升起、河道等高线 | `gen/shots/s08-tokyo-top.mjs:24-75` | 相机 (−80,−1650,−60)→(−60,−1500,−50)，32 mm f/5，`up` 向量 (0,0,−1)→(−0.1,0,−0.995) 做缓慢滚转；菊花 32 片椭球花瓣 rotationY 0→40°；东京塔 = 12 段 frustum 腿（橙/白相间）+ 两层平台 + 天线，200–1700 ms 升起；6×46 段等高线；字幕 3 组（21.2–23.6 s） |
| S09 | 23.7–26.3 | 铜金色环带特写（森林/云浮雕），背后强光 + 枝条剪影 | `gen/shots/s09-band-gold.mjs:11-26` | 相机 (−140,210,−1350)→(20,190,−1300)，55 mm f/2.4 roll 9°，aim 目标 x −60→60（沿带面平移）；纹理 `band_gold`；10 条枝条弧；太阳 power 3000、coronaSize 1600 |
| S10 | 26.3–32.2 | Logo：镜头摆到环带正面铭牌「TESSERACT 3D」，后方红叶 X 徽章逆光；末尾淡出 | `gen/shots/s10-logo.mjs:17-62` | 主环 R900 + 10 卡箍；相机 P0 (−760,320,−1650) 经 1900 ms `soft` 摆到 P1 (0,30,−2250)，再线性推到 P2 (0,25,−2150)；roll 通过 aim `up` 从 −18° 归零；铭牌 860×150 + Cinzel Bold 92 号 depth 10 bevel 1.5；徽章 4 片红叶呈 X（45°+90°k）；view exposure 5300→5900 ms 从 0 降到 −9 |
| S11 | 32.2–38.2 | 黑场终版字卡：TESSERACT 3D / NOW AVAILABLE WORLDWIDE / MIRAGE | `gen/credits.mjs:50-54` | End card 组 32.8–37.6 s，淡入/淡出各 900 ms；66/19/26 号字，tracking 200/520/650；`render.sh` 用 raster 渲染 |

### 4.1 演职员字幕表（`gen/credits.mjs:8-14`）

| 组 | 出现 (ms) | 时长 | 位置（列左缘 x, 职务基线 y） | 内容 |
|---|---|---|---|---|
| 1 | 4000 | 2500 | (1270,150) 右上 | 3D ENGINE / Yusuke Shibata |
| 2 | 9900 | 2000 | (540,500) 左中 | MODELING & RIGGING / Michael Vitt；PATH TRACING / Joyce Chen |
| 3 | 13500 | 2800 | (300,620) 左下 | 3D CONCURRENCY / John Roberts；3D BUILD SUPPORT / Jason Silberman；3D TESTING / Alex Du |
| 4 | 16600 | 2500 | (1250,560) 右 | RELEASE PIPELINE / Michael Yang；CRITIC REVIEW / Gaurav Misra；FONT DELIVERY / Ben Fortier |
| 5 | 21200 | 2400 | (300,430) 左 | FX STORYLINE / Sicheng Chen；CAPTION EDITING / Sebastian Griego；CI EFFICIENCY / Christine Dumont |

样式：职务 21 号 tracking 260、颜色 (0.85,0.79,0.68)；姓名 52 号 tracking 90、右移 24 px、低 56 px；多行组行距 135 px；组透明度 0→100（450 ms）…100→0（450 ms）。图标 `credit_icon.png` 由 `gen/media.mjs` 以 Image 图层补进每组，缩放 62%、不透明度 90。名单按需求「原样照抄」原片。

## 5. 关键算法与实现

> 本片是产品宣传片，**不讲解任何知识**；下文全部是「制作视频所用的算法」。路径追踪本身由 Tesseract 引擎完成，项目只负责生成场景描述、选择采样数和后期降噪。

### 5.1 「代码即场景」：action JSON 生成器

- 机制：每个镜头模块导出 `{ no, key, start, dur, build() }`，`build()` 返回 Tesseract action 数组：`createFxScene3DLayer` → `setFxScene3DWorld` → 相机 `addFxScene3DLayer` + `setFxScene3DActiveCamera` → 一个 `addFxScene3DFragment`（data[] + items[] 一次性加入）→ 灯光 → `setFxAnimator` 关键帧 → `addFxLayerEffect glow`。
- 关键封装（`gen/lib/act.mjs`）：`sceneLayer`、`camera`、`light`、`mesh`（= `meshData` + `meshItem`，含 primary/side 两个材质区域）、`group`、`textData/textItem`（Text3D 的 front/back/side 区域）、`anim`、`setVal`、`glow`。
- 关键帧写法：`keys = [[ms, value, ease]]`，ease 指**到达该关键帧的那一段**的缓动；值按数组长度自动判 float / vector2 / vector3。缓动表（`act.mjs:118-125`）：

```js
inOut: cubicBezier(0.42, 0, 0.58, 1)   out:  cubicBezier(0.2, 0.6, 0.35, 1)
in:    cubicBezier(0.55, 0, 0.85, 0.4) soft: cubicBezier(0.3, 0.1, 0.4, 1)
linear / hold
```

- 效果：场景、动画、灯光全部可由参数复现与修改；工程仍是可编辑 `.tsrct`（需求原文强调「fully editable」）。
- 限制：字段名必须严格符合 schema 版本（`fxSchemaVersion 86`）；Image 与 Audio 图层没有走 action，而是由 `gen/media.mjs` 直接改写 checkout 出来的文档 JSON 再 commit（原因未记录）。

### 5.2 相机：aim 约束 + 景深跟随 + 用 up 向量做滚转

`act.mjs:35-50`：相机不直接设旋转，而是挂一个 aim 约束指向「世界坐标 offset」目标点；`up = [sin(roll), cos(roll), 0]` 决定滚转；给了 `fNumber` 时景深 `focusAim` 绑定该约束，焦点自动跟随目标。

```js
constraints: [{ type: 'aim', target: { target: { type: 'world' }, offset: target }, up, forwardAxis: 'positiveZ' }]
lens: { type: 'physical', focalLengthMm: focal, sensorMm: [36, 24], fNumber }
depthOfField: { focusDistance: 1000, focusAim: { constraintId: aimId } }
```

- 运镜 = 同时给相机 `position` 和约束目标 `offset` 打关键帧（S03/S06/S09/S10）。
- 滚转动画 = 给 aim 约束的 `up` 打关键帧（S08 俯视缓慢滚转、S10 从 −18° 归正）。
- 正俯视（S08）时默认 up 与视线平行会退化，所以改用 `up: [0,0,-1]`。

### 5.3 程序化几何（`gen/lib/geo.mjs`）

- **齿轮** `gearPolys`：每齿 4 个点（根圆 0、齿顶 0.22、齿顶 0.48、根圆 0.7 的角度比例）→ 梯形齿；中心孔与辐条窗口作为额外闭合多边形，配合 extrude 的 `fillRule: 'evenOdd'` 挖空。辐条窗口宽度用 `gap = min(0.32, 18/ri)` 保证辐条最小 18 px。
- **星盘** `starPolys`：2N 个点交替取 rOut / rIn；可带中心孔。
- **车削** `lathe`：Y 向下的半轮廓 `[[radius, y], …]` 绕 Y 旋转；用于穹顶、雕像袍身、钟形球、平台、菊花碗。
- **可编辑网格** `MeshBuilder`：累积四边形面，可带 `uv0` 与区域标签；批量城市方块、环带、东京塔锥台腿都用它，一个 mesh 承载上千个方块以减少 item 数。
- **种子随机数** `rng(seed)`：LCG `s = s*1664525 + 1013904223 (mod 2^32)`，布局可复现。
- extrude 沿 Z 挤出，平放到地图上时绕 X 转 90°（`rot: [90,0,0]`）。

### 5.4 浑天仪环带：带 UV 的圆柱带 + 浮雕纹理（`gen/lib/armillary.mjs`）

- `bandGeometry(R, W, T, reps, seg=192)`：外表面四边形 `u = i/seg × reps`、`v = 0..1`，区域 `primary`；内表面与上下侧面为 `side`（bronze）。`smoothAngle 30`。
- 材质 `bandMat`：baseColor / metallic / height 三张纹理，`heightStrength 1.4`，粗糙度常量 0.42。
- 两侧银色 torus「绳纹」边、每圈若干 brass 卡箍 + 金色铆钉球。
- 纹理（`gen/lib/tex.mjs`，2048×240）：在浮点高度平面 `Plane` 上叠加 `rect / disc / dome / line`，再派生三张图：

```js
h    = height.blur(1)
mask = smoothstep(0.12, 0.3, h)                 // 浮雕区域
base = mix(colLow*(0.8+0.4*noise), colHigh*(0.75+0.25*h), mask)
metal = mask * metalHigh
```

  - `band_nyc`：两侧绳纹、底部水波纹、随机楼宇矩形（窗格处高度 ×0.55）、两座阶梯尖塔（帝国大厦式）、7 个螺旋「祥云」、14 个圆鼓包；颜色暗红 (0.2,0.02,0.03) → 银白 (0.93,0.91,0.87)，metalHigh 0.85。
  - `band_gold`：900 个随机 dome（树冠/云团）+ 40 根竖条（光束）+ 底部波纹；暗红褐 → 铜金 (0.95,0.68,0.38)，metalHigh 0.9。
- PNG 编码自写（`gen/lib/png.mjs`）：逐行 filter 0 + `zlib.deflateSync(level 9)` + 查表 CRC32；`Plane.idx` 在 x 方向环绕，所以纹理在环带上首尾无缝。

### 5.5 太阳：发光球 + 朝向相机的日冕卡片 + 点光源

- 核心球 emissive `#FFE4B0` 强度 25–30，**`castsShadows: false`**，否则会挡住放在球心的点光源（`armillary.mjs:57` 注释）。
- 日冕：`plane` 双面、emissive 纹理 `sun_corona.png`（1024²：高斯核心 + 指数外晕 + 260 条随机长度/宽度/亮度的放射线；R/G/B 分别取 v、v^1.25×0.88、v^1.9×0.65 形成白→橙渐变），放在球心前方 `coronaZ` 处。设计文档写「用 aim 约束朝向相机」，代码里实际是固定朝向的平面，靠相机大致正对它。
- 点光源 power 2500–5000、`sourceRadius = 0.9r`。
- 枝条剪影 `branches`：单位 torus（R=1、tube 0.004）按 R×100 缩放、随机三轴旋转，得到细暗弧。

### 5.6 地图桌面（`gen/lib/map.mjs`）

- `blob(cx, cz, rx, rz, seed)`：极坐标 `k = 1 + wob·(0.5 sin3a + 0.3 sin5a + 0.2 sin9a)` 生成摆动海岸线。
- `mapBase`：水面 box（顶面 y=0）+ 陆地多边形 extrude 12 px（顶面 y=−12，侧面 parchmentDark）。
- `cityBlocks`：包围盒内拒绝采样，`pointInPoly`（射线奇偶法）+ `avoid` 圆形避让机构区；高度 `h = hMin + r^2.2 (hMax−hMin)` 让矮楼占多数。
- `label`：Cinzel Bold Text3D，depth 3，`rot [-90, rotY, 0]` 平躺在陆地上，ink 色。
- `gears`：角度 `deg = dir × speed × (dur/1000) × 100 / r`，即**角速度与半径成反比**（线速度相同，相邻齿轮视觉上咬合）；`dir` 交替正反转。
- `rise`：位置先 hold 在地下 `from` 处，再 `out` 缓动升到位，可附带 rotationY 旋入。
- `mapLights`：暖色 spot key（左上后方，cone 45°，feather 0.6，radius 120）+ 冷色 point fill + 世界环境光 0.05。

### 5.7 曝光淡出与 bloom

- S10 末尾用 Scene3D `view.exposure` 关键帧 0 → −9（ease `in`）自然变黑，而不是叠黑场（`s10-logo.mjs:60`）。
- 每个 Scene3D 图层加 `glow` 效果（阈值 70–80、半径 40–90、强度 0.6–1.0）做 bloom；浑天仪镜头阈值更低、半径更大。

### 5.8 采样与降噪策略

- 无 AI 降噪器 → 地图镜头 96 spp、强光的浑天仪/Logo 镜头 128 spp（`render.sh`），导出 ProRes 中间件，再在 `post.sh` 中逐段 `hqdn3d=2.5:2:5:4`（亮度空间 2.5 / 色度空间 2 / 亮度时域 5 / 色度时域 4）时域降噪；S11 是 raster 文本段，不做降噪（`vf=null`）。
- 标定（`gen/calib.mjs`）：用「真实复杂度」的地图样本（水、陆地、2 齿轮、星盘、穹顶、300 个方块、spot 60 kW + fill 6 kW、glow）测单帧耗时。文档记录的结论是 64 spp 1080p 约 8 s/帧，且 60 kW 功率会过曝；正式镜头的 key 光降到 700–2800。
- 渲染耗时推算（**推断**，按 8 s/64 spp 线性外推，未计入场景复杂度差异）：浑天仪/Logo 段 15.3 s ≈ 459 帧 × 约 16 s，地图段 16.9 s ≈ 507 帧 × 约 12 s，合计约 3.7 小时。项目内没有实际导出耗时记录。

## 6. 声音与字幕

### 6.1 配乐（`music/compose.mjs` → `build/music.wav`）

- 无配音、无 TTS、无采样音源；纯 Node 程序合成，48 kHz、16-bit、立体声、38.2 s。
- 调性与节拍：D 小调 6/8，八分音符 `E = 60/(85.7×3) ≈ 0.233 s`，每小节 6 个八分 ≈ 1.40 s；固定音型从 `T0 = 3.3 s`（S02 开始）进入，与镜头切点对齐。
- 和声：4 小节循环 Dm–Dm–B♭–C（`CHORDS`）；第 17 小节（`LAST`，≈27.1 s）落到 D 大三和弦（皮卡第三度）全奏，持续 4.4 s，覆盖 Logo 段。
- 段落（代码实际）：

| 时间 | 层次 | 代码 |
|---|---|---|
| 0–3.3 s | 低音 D 持续音 + 中音 D 长音；定音鼓滚奏间隔从 0.32 s 缩到 0.06 s、音量按 (t/T0)² 渐强；1.6 s 反向噪声上扬 | `:55-58` |
| 3.3 s 起 | 大提琴固定音型：每小节「根–五–三」×2（八分音符，强拍 ×1.15），加一个低八度全小节低音；滤波截止频率逐小节升高 1500+60b Hz | `:61-66` |
| 第 2 小节第 4 个八分（≈6.8 s）起 | 原创旋律 21 个音（`MEL`），先低八度；第 7 小节起叠加原八度 4 声部 | `:69-78` |
| 第 3 小节起 | 中提琴/圆号式长音铺底，音量 0.05→0.08→0.11 分段增加 | `:81-84` |
| 第 7 小节（≈13.1 s）起 | 大鼓在每小节第 1、4 拍，奇数小节第 6 拍加定音鼓；终止前 2 小节军鼓式噪声滚奏 + 2.4 s 反向噪声上扬 | `:87-93` |
| ≈27.1 s | 终止和弦：8 个音（D2…F#5）+ 重鼓 + 噪声冲击 | `:96-101` |
| 32.8–32.9 s | 终版字卡：轻低鼓 + 高音 D 与低音 D 柔和长音 | `:104-106` |
| 36.6–38.1 s | 线性淡出 | `:125` |

- 合成算法：
  - 弦乐 `strings()`：3–4 个相互失谐 ±0.4% 的锯齿波，5.3 Hz 颤音（前 0.4 s 渐入），线性 ADSR 后再平方，加 ±10% 随机幅度抖动，两次一阶低通（cutoff、1.3×cutoff）。
  - 鼓 `drum()`：正弦扫频 `f = f1 + (f0−f1)e^{−18t}` + 衰减噪声，指数包络，2.5 kHz 低通。
  - 混响：Freeverb 式 8 个梳状滤波器（延迟 1116–1617 样本，反馈 0.86，阻尼 0.25）+ 4 个全通；右声道延迟 +23 样本形成立体声；干 0.8 / 湿 0.9。
  - 母带：峰值归一到 0.89 后 `tanh` 软削波，再量化到 ±32000。
  - 声像：等功率 `gl = cos((pan+1)π/4)`、`gr = sin((pan+1)π/4)`。
- 注意：`strings()` 每个声部初相用 `Math.random()`，不是种子随机数，所以每次生成的 WAV 不会逐样本一致（`compose.mjs:23`）；其余噪声用种子 LCG。
- 版权取舍：需求明确「旋律原创，不复制《权力的游戏》主题」，只对齐 6/8 暗色大提琴固定音型、节奏点与情绪（`prompt.md:9`）。

### 6.2 混音与响度

- 配乐既作为工程内 Audio 图层（`gen/media.mjs`，id 990，volume 1.0），又在后期由 ffmpeg 单独混入：Tesseract 分段导出不含音频（`render.sh` 注释、`plan/0b-credits-music.md:36`），`post.sh` 把 `build/music.wav` 作为第二输入，`loudnorm=I=-16:TP=-1.5:LRA=11`，AAC 256 kb/s 48 kHz，`-shortest`。
- 实际响度测量结果：未记录。没有试听记录。

### 6.3 字幕

- 不是对白字幕，而是演职员字幕 + 终版字卡，全部用 Tesseract 2D Text 图层（Cinzel），时间固定写在 `BLOCKS` 中，与镜头窗口手工对齐（见 §4.1）。
- Cinzel 的小写字母本身是小型大写，所以姓名直接写成「Yusuke Shibata」即可得到原片的大写 + 小型大写效果（`plan/0b-credits-music.md:4`）。

## 7. 迭代、问题与修正

> 项目没有制作日志，只能从「设计文档 vs 最终代码」的差异和代码注释反推迭代。下列差异均为**代码/文档中存在**的事实；「原因」除注明来源外都是推断。

### 7.1 已记录的问题与对策

| 问题 | 对策 | 依据 |
|---|---|---|
| 公司网络拦截降噪器权重下载（OIDN） | 全片 `--no-denoise`，提高采样，ffmpeg `hqdn3d` 时域降噪 | `prompt.md:11`；`render.sh:3` |
| 标定时 60 kW spot 过曝 | 正式镜头 key 光降到 700–2800，太阳点光 2500–5000 | `plan/0b-armillary.md:4`；`gen/calib.mjs:11` |
| 关键帧会替换属性的静态值，从 0 开始会让环「跳」回零度 | 旋转动画的首帧从静态旋转值开始：`[[0, base], [D, base+deg]]` | `gen/shots/s02-sun-core.mjs:28-30` 注释 |
| 后创建的图层插在最上层 | 镜头倒序生成 action，S01 在底层，字幕最后 | `gen/main.mjs:14` 注释 |
| 太阳球会挡住球心点光源 | 核心球 `shadows: false` | `gen/lib/armillary.mjs:57` 注释 |
| 大任务一次生成易超时（项目通用规则） | 先写 `plan/00` 架构，再写 4 份 `0b-*` 模块细化，再逐文件写代码（每文件 ≤250 行，最大的 `act.mjs` 142 行） | `plan/00-architecture.md` §7 |

### 7.2 设计文档与最终代码的差异（并列记录）

| 项 | 设计文档 | 代码 | 推断原因 |
|---|---|---|---|
| S01 相机 | (−120,160,−900)，roll −18°，前推 z −900→−860 | (−60,150,−1500)→(−40,140,−1440)，roll −16° | 预览后拉远以容纳环带与太阳（推断） |
| S01 横梁 | 2.7–3.3 s，box 2000×60×60 | 2.6–3.3 s，box 3200×70×70 | — |
| S02 相机 | (0,80,−420)，28 mm，roll 0→25° 动画 | (−60,110,−820)→(40,70,−700)，26 mm，roll 固定 −10° | — |
| S03 外齿圈 | 48 齿 r260 8 辐条 | 72 齿 r290，中心孔 215（环形齿圈） | — |
| S03 城市方块 | 约 600 | 900 + 700 + 500 | 加密以接近原片质感（推断） |
| S03 女神升起 | 1800–3300 ms，自 +420 | 1700–3400 ms，自 +520 | — |
| S05 上排齿轮 | 9 个 | 11 个；相机拉远到 z −2050 | — |
| S06 哈利法塔 | 5 段，高 900 | 8 段 × 105 + 200 尖顶 | — |
| S08 花瓣 | 16 片 | 2 圈 × 16 = 32 片；滚转改用 aim up 向量 | — |
| S10 主环 / 铭牌 | R620；铭牌 760×120；字号 84；exposure 0→−10 | R900；铭牌 860×150；字号 92；exposure 0→−9 | — |
| 字幕样式 | 职务 22 号 tracking 350；姓名 54 号 tracking 180；S06 组 y=700 | 21/260；52/90；S06 组 y=560 | — |
| 终版字卡 | 33.0–33.8 淡入，36.6–37.4 淡出；64/20/24 号 | 组 32.8–37.6 s，各 900 ms 淡入淡出；66/19/26 号 | — |
| 配乐速度 | ♩.=84（每小节 ≈1.43 s） | ♩.=85.7（每小节 ≈1.40 s） | 让 17 小节后的终止和弦落在 Logo 段（推断） |
| 终止 | 26.3 s 起 Dm→B♭→C→D 大三 | 循环 Dm–Dm–B♭–C，≈27.1 s 落 D 大三 | — |
| 混响 | 卷积混响（指数衰减噪声 IR） | Freeverb 式梳状 + 全通 | — |
| 降噪参数 | `hqdn3d=2:1.5:4:3` | `hqdn3d=2.5:2:5:4` | 噪点比预期多（推断） |
| 暗角 / 颗粒 | 计划实现 | 未实现 | — |

## 8. 可复用制作经验

1. **先用真实复杂度的场景标定单帧耗时，再定采样数和全片方案。** 标定场景包含水、陆地、齿轮、星盘、穹顶、300 个方块和真实灯光，而不是空场景；结论（64 spp ≈ 8 s/帧、60 kW 过曝）直接决定了 96/128 spp 的分级采样和灯光功率量级。依据：`gen/calib.mjs`、`plan/0b-armillary.md:4`、`render.sh`。
2. **降噪器不可用时，把降噪移到后期时域：** 路径追踪 `--no-denoise` + 中等采样 → ProRes 中间件 → `hqdn3d` 逐段时域降噪；纯 2D 段不降噪、改用 raster 渲染。依据：`render.sh`、`post.sh`。
3. **一个镜头一个 Scene3D 图层 + 一个 action 批次文件。** 场景互不干扰、可单独 apply/重建/分段导出；配合 `dev.sh <shot> <时间点> [spp]` 只构建一个镜头并在指定时刻出 16 spp 预览，迭代成本低。依据：`gen/main.mjs`、`dev.sh`、`plan/00-architecture.md` §4。
4. **用相机 aim 约束 + 景深跟随 aim 目标来运镜。** 只需给相机位置和目标点打关键帧，焦点自动跟随；滚转用 up 向量关键帧，正俯视时换 up 轴避免退化。依据：`gen/lib/act.mjs:35-50`、`s08`、`s10`。
5. **关键帧会覆盖静态值：动画首帧要从当前静态值出发。** 否则带初始倾角的物体会在第 0 帧跳回 0°。依据：`gen/shots/s02-sun-core.mjs:28`。
6. **齿轮组按「角速度 ∝ 1/半径、方向交替」统一驱动**，不做真实啮合求解也能读成咬合传动。依据：`gen/lib/map.mjs:64-73`。
7. **大批量重复小物体合并成一个可编辑网格**（城市方块上千个），种子随机 + 拒绝采样 + 避让圆，既省 item 数又能复现布局。依据：`gen/lib/map.mjs:37-52`、`gen/lib/geo.mjs:83-100`。
8. **配乐把音乐结构锚到镜头切点：** 固定音型的第一个强拍放在 S02 起点 3.3 s，调速让终止和弦落进 Logo 段，打击乐从机械镜头开始。依据：`music/compose.mjs:6-7,52,87`。

## 9. 复现入口

> 以下命令来自项目脚本；**记录命令不代表本轮执行过**。本次整理没有运行任何项目脚本、没有渲染。

### 9.1 前置条件

- 安装 Tesseract CLI 0.5.0（脚本假定 `tsrct` 在用户级应用支持目录的 `Tesseract/bin/` 下；换机器需改 `build.sh` / `dev.sh` / `render.sh` 第 5–6 行的 `T=`）。
- Node.js（支持 ESM 与顶层 await；具体版本未记录）、ffmpeg/ffprobe（含 `libx264`、`prores_ks`、`hqdn3d`、`loudnorm`）。
- 需要重新生成的内容（都被 `.gitignore` 排除，当前不在项目内）：`build/tex/*.png`、`build/music.wav`、`build/actions/*.json`、`intro.tsrct`、`build/segments/*.mov`、`output.mp4`。
- 能联网下载降噪器权重时，可去掉 `--no-denoise` 并降低采样（本项目未验证）。

### 9.2 执行顺序

```bash
cd pathtraced-intro
./build.sh            # tex.mjs → compose.mjs → main.mjs → 创建 intro.tsrct → 改画布 → 导入 Cinzel/纹理/音乐 → 按序 apply → media.mjs 补图标与 Audio 图层
./dev.sh s03-new-york 8.5,10.5 16   # 可选：只构建一个镜头，在项目时间 8.5 s / 10.5 s 出 16 spp 预览到 .tesseract-work/dev/
./render.sh           # 逐镜头导出 build/segments/sNN.mov（已存在则跳过）；也可 ./render.sh 03 10 只渲指定镜头
./post.sh             # hqdn3d → concat → 混入 music.wav + loudnorm → output.mp4，并 ffprobe 自检
```

- `build.sh` 中的关键 CLI 调用：`project create / checkout / commit / import-font / import-asset --kind image|audio / apply --actions`。
- `render.sh` 的分段导出调用：`tsrct export --project intro.tsrct --format prores --start-ms S --duration-ms D --renderer path-tracer --no-denoise --samples N --json`，日志在 `.tesseract-work/export-sNN.log`，先写 `.part.mov` 再改名，可断点续渲。
- 标定场景：`node gen/calib.mjs <out.json>` 只生成 action 文件，apply 与渲染命令未写成脚本（未记录）。

### 9.3 改参数的入口

| 想改什么 | 改哪里 |
|---|---|
| 镜头时间窗口 | 各 shot 的 `start` / `dur`，同时改 `render.sh` 的镜头表与 `credits.mjs` 的 `BLOCKS.at` |
| 相机运镜 | 各 shot 的 `A.camera(...)` 初值与 `cam.camId position` / `cam.tgtId offset` / `cam.aimId up` 关键帧 |
| 材质色 | `gen/lib/mat.mjs` 的 `M` 表（sRGB hex，自动转线性） |
| 浮雕图案 | `gen/lib/tex.mjs` 的 `bandNYC` / `bandGold`（改完需重新跑 `build.sh`） |
| 辉光 | 各 shot 末尾 `A.glow(L, id, threshold, radius, intensity)` |
| 采样 / 降噪 | `render.sh` 镜头表第 4 列；`post.sh` 的 `hqdn3d` 参数 |
| 配乐速度 / 终止位置 | `music/compose.mjs` 的 `E`、`T0`、`LAST` |
| 字幕名单与位置 | `gen/credits.mjs` 的 `BLOCKS` |

## 10. 参考来源

| 链接 | 平台 / 作者 | 类型 | 概述 | 借鉴内容与对应 | 访问状态 |
|---|---|---|---|---|---|
| https://x.com/cfenghub/status/2118212495991361715 | X / CF长风（@cfenghub） | 创作参考（需求入口，转发帖） | 中文转发评论：称 Opus 5.5 + Tesseract 打通 3D 管线、真实光追、无需剪辑或 3D 软件，流程为「大模型写分镜 → Agent 调 Tesseract 光追渲染 → 改文本/机位/打光后压配音出片」；引用 Mirage 原帖视频。页面显示时间 2026-11-05 5:23 AM（时区未标注） | 用户要求 review 此帖并复刻其中的视频；技术路线（不用 Blender/视频模型）依此确定 | 2026-11-12 web_fetch 成功（未登录可见正文与引用卡片；视频本体未通过网页观看） |
| https://x.com/trymirage/status/2118054387209925032 | X / Mirage（@trymirage） | 创作参考（原视频出处） | 英文官方帖：Opus 5.5 或 Astra 配合 Tesseract 可做路径追踪的专业级 3D 输出，视频「without Blender or video models」且「fully editable」。页面显示时间 2026-11-04 6:54 PM（时区未标注） | 全片镜头、文字品牌（TESSERACT 3D / MIRAGE）、演职员名单、30 fps 1080p 规格、可编辑工程交付形式 | 2026-11-12 web_fetch 成功（同上，视频本体未通过网页观看） |
| `reference/ref.mp4`（本地） | 原视频的本地副本（下载方式未记录） | 创作参考 | 38.19 s，1920×1080 H.264，30 fps，AAC 48 kHz 立体声 | 镜头表时间、字幕位置、配乐节奏点 | 本次只做 ffprobe；未播放 |
| `reference/t2_*.jpg`、`hi_*.jpg`、`sheet*.jpg`、`c1–c6.png`、`crops.png`、`spec.png`（本地） | 由 ref.mp4 抽出的联系表、单帧、裁切图（推断，文件名与架构文档引用一致） | 创作参考 | `hi_<秒>.jpg` 对应 1–37 s 的若干时刻；`t2_*` 是镜头表时间来源 | 镜头拆解、字幕版式 | 本次未查看图像内容；原始逐帧目录 `reference/frames/` 已被 `.gitignore` 排除并移出仓库 |
| https://github.com/NDISCOVER/Cinzel | GitHub / The Cinzel Project Authors | 素材来源（字体） | Cinzel 字体项目，SIL OFL 1.1 | 3D 刻字与 2D 字幕字体 | 仅在 `assets/fonts/OFL.txt:1` 中出现，本次未访问 |

- `captions-cdn.xyz` 在 `prompt.md` 中作为被拦截的降噪器权重下载域名出现，不是参考来源，本次未访问。
- 帖子把成片说成「最后压上配音」；Mirage 原帖与本地 ref.mp4 是否含配音，本项目只记录为「AAC 立体声」+ 配乐复刻方案，未记录原片是否有人声。

## 11. 证据与限制

### 11.1 实际读取的文件（sha256 前 16 位取自 fingerprints.json）

| 文件 | sha256 前 16 位 | 读取方式 |
|---|---|---|
| `prompt.md` | `2be9ab6c4086cea7` | 全文 |
| `plan/00-architecture.md` | `2b41655dc503b3b2` | 全文 |
| `plan/0b-lib.md` | `4d2cc169fa9b4c23` | 全文 |
| `plan/0b-map.md` | `28401661c35af7b0` | 全文 |
| `plan/0b-armillary.md` | `12327418278eb81f` | 全文 |
| `plan/0b-credits-music.md` | `54b72ed57afbb188` | 全文 |
| `build.sh` / `dev.sh` / `render.sh` / `post.sh` | `45173176704d5614` / `967fb21b74515071` / `522dc46182f8db7f` / `d268ed9e77dc0b89` | 全文 |
| `gen/main.mjs` / `gen/media.mjs` / `gen/setdoc.mjs` / `gen/calib.mjs` / `gen/credits.mjs` | `f231b16c9da87b74` / `a41eeef5cbee8696` / `65320278bea22614` / `f36eb1b45bf8f425` / `9e6933786bd4002a` | 全文 |
| `gen/lib/act.mjs` / `geo.mjs` / `mat.mjs` / `png.mjs` / `tex.mjs` | `e983b7dbf8de2bfe` / `45af053795895509` / `3029b7bef5f1d69d` / `12e9e9d04a5c793e` / `86b584d43ca0b1f6` | 全文 |
| `gen/lib/armillary.mjs` / `map.mjs` | `ab239b8a8307e447` / `6120a9cf1bc7be23` | 全文 |
| `gen/shots/s01…s10`（10 个文件） | `e0dbf9abd6cb4c4d`、`4c0fa39ad85ca90a`、`74cf10260a21c70d`、`34a3071fda4a6382`、`5aa8019d3edc1c58`、`1910a0f85fc2eeea`、`de9436c86f1d540c`、`a8086ca553eba6f5`、`ca9f2c8063c940c8`、`37c565fc3ad6a33e` | 全文 |
| `music/compose.mjs` | `2c128a6d4eeec782` | 全文 |
| `assets/fonts/OFL.txt` | `f2b3029aba64c378` | 前 3 行 |
| `reference/ref.mp4` | —（21 270 655 字节） | 仅 ffprobe 媒体头 |

### 11.2 未读取 / 缺失

- 未查看：`reference/` 下全部 jpg/png 参考图、`assets/fonts/Cinzel.ttf` 内部元数据（仅计算哈希）。
- 不存在于项目内：`intro.tsrct`、`output.mp4`、`build/` 下全部产物（目录为空）、`.tesseract-work/`（检查图、apply/export 日志）、`reference/frames/`。因此**无法确认**：全片是否 apply 成功、是否终渲、实际渲染耗时、最终降噪效果、响度、视觉对比结果。
- 未记录：Node/ffmpeg 版本、GPU 与硬件、制作所用模型与推理参数、制作耗时、用户反馈与验收。
- 未找回制作对话；git 历史只有两次提交，无法还原迭代顺序。§7.2 的差异只能说明「设计→代码」发生过调整，调整原因均为推断。
