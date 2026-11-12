---
type: video-production-dossier
project: geo-story-studio-opus
video_id: solar-eruption-earth
title: 太阳爆发，为什么会影响地球？
curriculum: 人教版必修第一册 第一章第二节 太阳对地球的影响（太阳活动对地球的影响）；课标 1.2
captured: '2026-11-12'
status: 两版成片存在——完整版（竖屏 1080×1920，177.77 s）与推荐版（89.77 s）+ 推荐版封面；均无片尾品牌卡；技术检查/视觉检查/用户验收未记录
source_fingerprint: 2bff9275cb532c205f941239c09e047306942b8491705c3c629a3d743da56ebf
tags:
- video-creation
- production-capture
- geography-teaching
- canvas-frame-render
- edge-tts
---

# 太阳爆发，为什么会影响地球？ · “两份快递”双路径叙事，一套代码出完整版与 `--rec` 推荐版

<!-- capture-source-sha256:2bff9275cb532c205f941239c09e047306942b8491705c3c629a3d743da56ebf -->

本文是根据项目文件做的材料回溯（源码、`script.json` / `script-rec.json`、两份 `timeline.json`、`build/tts_cache` 文件名与时间、`out/` 清单与 ffprobe）。证据包中**未发现制作会话**（`host_session: null`）。本次**未运行任何项目脚本**（含 `build/sheet.sh`），未打开音视频和封面图。

## 1. 当前结论与制作路线

**状态**：`out/` 下有两版成片及字幕、一张推荐版封面。两版是否发布、哪版被采用、视觉与用户验收均**未记录**。

| 项 | 完整版 | 推荐版（`--rec`） | 来源 |
| --- | --- | --- | --- |
| 文件 | `太阳爆发为什么会影响地球.mp4` / `.srt` | `…_推荐版.mp4` / `.srt` / `…_推荐版_封面.png` | `out/` |
| 分辨率 / 帧率 | 1080×1920，30 fps | 同 | ffprobe |
| 时长 | 177.767 s（timeline 177.758） | 89.767 s（timeline 89.776） | ffprobe；timeline |
| 帧数（推算） | 5333 | 2693 | `render.mjs:169` `Math.round` |
| 编码 | H.264 yuv420p CRF 19；AAC 192k 48 kHz 立体声 | 同 | ffprobe；`render.mjs:166-167` |
| 大小 | 38,720,984 B | 22,354,960 B；封面 938,827 B | 证据包 |
| 段落 / cue | 26 句 / 86 条 | 13 句 / 37 条 | timeline |
| 声音角色 | 旁白 + 小宇 + 太阳 | 旁白 + 小宇（无太阳） | 两份 script |
| 生成时间 | TTS 10-26 22:15–22:16 → SRT 22:16 → MP4 22:34 | TTS 10-27 16:46–16:47 → SRT 16:48 → 封面 16:57 → MP4 17:01 | 缓存与文件 mtime |

**管线**：`tts.py [--rec]` 读 `script.json` 或 `script-rec.json`，输出到 `build/` 或 `build/rec/`（缓存共用 `build/tts_cache/`）→ `render.mjs [--rec]` 通过 `lib.mjs` 的 `REC/BUILD_DIR` 选时间轴，并在 `['a'…'e']` 与 `['r1','r2','r3']` 两组场景模块间切换 → 逐帧 RGBA 管道送 ffmpeg，`audio.mjs` 程序配乐/音效 → MP4；`--rec --cover` 用 `cover.mjs` 出封面；`build/sheet.sh` 把多张 `--still` 预览拼成 1920 宽拼图（对应 AGENTS 第 3 节“写一组、验一组”）。

## 2. 教学目标与课标对齐

- **curriculum**：人教版必修第一册 第一章第二节 太阳对地球的影响（太阳活动对地球的影响）；课标 1.2 运用资料，说明太阳对地球的影响。
- **来源**：Geo Story Studio · `douyin-solar-eruption-earth` v002 分镜与科学审校 review-002，即 `geo-story-studio/storage/builds/douyin-solar-eruption-earth/v002`（按命名推断，未打开）；推荐版 `source` 注明“在完整版基础上精简”，NOAA SWPC 资料于 2026-10-27 再次核对。
- **核心知识点**：太阳爆发通过两条路径影响地球——①耀斑电磁辐射（光速约 8 分钟）扰乱日侧电离层、影响无线电短波通信；②CME 等离子体携带磁场（十几小时到几天）在方向与磁场条件合适时引发磁暴、极光。
- **科学边界**：`script.json` 的 `excluded` 字段写明“按审校结论，不使用太阳活动与降水的相关结论，不做实时事件预报，不把磁层画成实体护盾，不画太阳粒子直线撞到地面”。推荐版把“磁场方向对不对得上”进一步写实为“越强、越持久地指向南，即与地球磁场相反”，极光机制改为“磁层里被加速的电子沿磁力线进入高层大气”。
- **叙事结构（完整版）**：

| 环节 | 段落 | 内容 |
| --- | --- | --- |
| 情境导入 | static, sunSorry, far, title | 小宇的短波电台只剩沙沙声；太阳“爆发了一下”；“离 1.5 亿千米关我什么事？”→“两种完全不同的快递” |
| 路径① 光 | flare … hf | 耀斑增亮、X 射线紫外线猛增 → 约 8 分钟到达 → 日侧低层电离层电离猛增吸收短波 → 教材表述“耀斑扰乱电离层，影响无线电短波通信” |
| 路径② 物质 | second … cond | 太阳“甩出一大团自己”= CME；比光慢；不是每次都砸中：看方向、看磁场方向 |
| 影响 | storm, grid, good, aurora, lowLat | 磁暴（指南针、导航、电网、空间天气预报）；极光（高纬，强磁暴时低纬也可能见到） |
| 总结与考点 | map, exam, sum | 两条路径一张图；易错点：受影响的是无线电短波而非有线电话，极光在高纬高空；一句话 |
| 收尾 | kidBye, sunBye | “干扰电台的是八分钟前出发的光”；“看到极光替我说：到货啦！” |

- **推荐版结构**：开门见山提问“太阳爆发，地球一定会受影响吗？”→ 分清两条路径 → 耀斑 / 电离层 / 短波 → CME / 方向与南向磁场 → 磁暴 → 极光 → 一句话复述 → 选择题“耀斑辐射先影响白天还是夜晚一侧？”（画面在 `E('quiz') + 0.5` 揭晓 A 白天一侧，`scenes-r3.mjs:82-119`）。
- **数字与出处**（`numbers`）：日地距离约 1.5 亿 km（NASA Sun Facts、教材）；约 8 分钟（NOAA SWPC Solar Flares；1.5 亿 km ÷ 光速 ≈ 8.3 分钟）；D 层电离、HF 3–30 MHz 被吸收（NOAA）；CME 最快 15–18 小时、慢的几天（NOAA）；南向 IMF 利于磁暴、导航与电网影响（NOAA）；极光椭圆约 60–75°、强磁暴向赤道扩展（NOAA Aurora）；教材第 10—12 页。

## 3. 技术与依赖

| 类别 | 内容 | 证据 |
| --- | --- | --- |
| 渲染 | `@napi-rs/canvas` 声明 `^1.0.11`，lock 解析 **1.0.13** | `package.json`；`package-lock.json` |
| npm scripts | 无 `scripts` 字段 | `package.json` |
| Python | edge-tts、aiohttp、fontTools；无 requirements，版本**未记录** | `tts.py`；`fonts.py` |
| 字体 | 缺字体自动跑 `fonts.py`，回退 Hiragino 等 | `lib.mjs` |
| ffmpeg | 旁白 adelay/amix/highpass/loudnorm；合成 sidechaincompress/alimiter/libx264/aac；`sheet.sh` 用 hstack/vstack/scale 拼预览 | `tts.py`；`render.mjs`；`build/sheet.sh` |
| 复用 | `cast.mjs`、`sun.mjs`、`audio.mjs`、`fonts.py` 与 [[sunspot-not-hole]] 同 hash；`lib.mjs` 仅多出 `REC/BUILD_DIR` 4 行 | 证据包 hash；diff |

**声线**（两版全局 `rate: 4`，`TARGET = None` 未提速）：

| 角色 | voice | 角色 rate / pitch | 实际 rate | 完整版句数 | 推荐版句数 |
| --- | --- | --- | --- | --- | --- |
| `zh` 旁白 | zh-CN-XiaoxiaoNeural | 0 / 0 | +4% | 15 | 11 |
| `kid` 小宇 | zh-CN-YunxiaNeural | +6 / +8 Hz | +10% | 7 | 2 |
| `sun` 太阳 | zh-CN-YunjianNeural | 0 / −4 Hz | +4% | 4 | — |

## 4. 镜头与时间轴

时间取自两份 `timeline.json`；场景在下一段落前约 0.25 s 切换，交叉淡化 `XF = 0.4`，每次切换自动叠 whoosh（与 [[sunspot-not-hole]] 相同的 `render.mjs` 骨架）。

**完整版（`scenes-a…e.mjs`，5 个模块 11 个场景）**

| 场景 | 约时间 (s) | 段落 | 画面要点 | 模块 |
| --- | --- | --- | --- | --- |
| radio | 0–13.3 | static, sunSorry, far | 小宇抱着短波收音机出沙沙声，太阳道歉 | `scenes-a.mjs` |
| title | 13.3–21.0 | title | 题目 + “两份快递” | `scenes-a.mjs` |
| flare | 21.0–36.1 | flare, light | 耀斑增亮、辐射以光速约 8 分钟到地球 | `scenes-b.mjs` |
| iono | 36.1–61.6 | whatDo, iono, hf | 日侧电离层剖面：短波被低层吸收、通信中断；教材表述卡 | `scenes-b.mjs` |
| cme | 61.6–83.5 | second, sunThrow, cme, slow | 太阳“甩出一大团自己”，CME 等离子体 + 磁场慢速飞行 | `scenes-c.mjs` |
| cond | 83.5–99.0 | hit, notAim, cond | 方向偏离擦肩而过；磁场方向是否“对得上” | `scenes-c.mjs` |
| storm | 99.0–115.6 | storm, grid | 磁暴：指南针乱指、导航、电网、空间天气监测 | `scenes-d.mjs` |
| aurora | 115.6–135.6 | good, aurora, lowLat | 粒子沿磁场进入两极高空发光；低纬可见；背景整体泛绿紫 | `scenes-d.mjs`；`render.mjs:31-34` |
| map | 135.6–148.6 | map | 两条路径总结图 | `scenes-e.mjs` |
| exam | 148.6–158.6 | exam | 易错点：短波 ≠ 有线电话；极光在高纬高空 | `scenes-e.mjs` |
| end | 158.6–177.8 | sum, kidBye, sunBye | 一句话；“到货啦”收尾；页脚资料行 | `scenes-e.mjs` |

**推荐版（`scenes-r1…r3.mjs` + `diagrams.mjs`，3 个模块 7 个场景）**

| 场景 | 约时间 (s) | 段落 | 画面要点 |
| --- | --- | --- | --- |
| open | 0–10.2 | q, two | 双路径总览图 + 问题（首帧即内容，无黑场淡入） |
| flare | 10.2–21.0 | flare, kid | 耀斑辐射到地球，小宇“难怪全是沙沙声” |
| iono | 21.0–36.1 | iono, hf | 平时 vs 耀斑时的日侧电离层剖面 |
| cme | 36.1–56.6 | cme, ask, cond | CME 速度、方向、南向磁场 |
| storm | 56.6–63.0 | storm | 磁层受扰 → 磁暴（磁层标注为“地球磁场控制的空间”） |
| aurora | 63.0–74.6 | aurora | 极光地球仪 + 夜空光帘 |
| map | 74.6–89.8 | map, quiz | 两路径总结 + A/B 选择题揭晓 |

## 5. 关键算法与实现

> 视频**讲的知识**是太阳活动对地球的影响；以下是**制作算法**，不是电离层或磁层物理模拟。

### 5.1 一套代码两个版本：`--rec` 开关（`tts.py:32-36, 150-151`、`lib.mjs:27-30`、`render.mjs:14-15`）

```python
REC = "--rec" in sys.argv
SCRIPT = ROOT / ("script-rec.json" if REC else "script.json")
if REC:
    BUILD = BUILD / "rec"
...
lead_in, tail = script.get("lead_in", LEAD_IN), script.get("tail", TAIL)
title = script.get("output_title", TITLE)
```

```js
const TITLE = REC ? '太阳爆发为什么会影响地球_推荐版' : '太阳爆发为什么会影响地球';
const KEYS = REC ? ['r1', 'r2', 'r3'] : ['a', 'b', 'c', 'd', 'e'];
```

推荐版在 JSON 里自带 `lead_in: 0.15`、`tail: 2.6`（完整版 1.0 / 2.8），并去掉开场黑场淡入（`render.mjs:124` `REC ? 0 : …`），首帧即双路径总览——针对信息流“首帧即封面”。TTS 缓存目录不随 `--rec` 改变，两版共享缓存。

### 5.2 弯曲电离层剖面（`diagrams.mjs:66-123`）

```js
const G = [540, 3400, 2200];               // 地心在屏幕下方 3400 px，地表半径 2200 px
const LOW = 330, HIGH = 700;
export const yOn = (x, h) => G[1] - Math.sqrt((G[2] + h) ** 2 - (x - G[0]) ** 2);
```

地表与两层电离层画成同心圆弧，`yOn(x, h)` 给出任意 x 处高度 h 的屏幕 y。短波射线从发射台到较高层反射点再到收音机；参数 `boost` 加厚并染暖低层，`eaten` 让射线在发射台—反射点中点 `IONO.CUT` 处消失并迸出火花，同时收音机切到“沙沙声”状态。三个 `wavePacket` 从太阳飞向低层，表示耀斑辐射只照到日侧。

### 5.3 双路径总览与颜色编码（`diagrams.mjs:1-65`、`cover.mjs`）

头注释规定全片配色：金 = 电磁辐射、青 = 等离子体、淡紫 = 磁场。路径 A（辐射）是直线、虚线流速快（`lineDashOffset = -t*80`）；路径 B（CME）是二次贝塞尔曲线、流速慢（`-t*20`），`onB(s)` 按贝塞尔参数放置 CME 团块。同一 `twoPaths()` 被推荐版开场、总结和封面复用。

### 5.4 极光地球仪（`diagrams.mjs:125-165`）

`proj(lat, lon)` 把经纬度投到北极朝观众倾斜 0.42 rad 的球面，`z < 0` 的背面点断笔；完整可见的纬线圈必须 `closePath()`，代码注释说明否则叠加发光会出现亮缝。`globe(ctx, t, ovalLat, …)` 用可变的极光椭圆纬度表现“强磁暴时向低纬扩展”。

### 5.5 预览拼图脚本（`build/sheet.sh`）

`bash build/sheet.sh [--rec] 0.8 3.0 …` → 先 `node render.mjs --still`，再用 ffmpeg `hstack`（每行 3 张）+ `vstack` + `scale=1920:-1` 生成 `build/sheet.png`，成功后删除单帧 PNG。这是本组 5 片里唯一把“拼图检查”落成脚本的。

## 6. 声音与字幕

| 项 | 内容 | 来源 |
| --- | --- | --- |
| 合成参数 | `TARGET=None`（“no cap: the piece is as long as the narration needs”）、`LEAD_IN=1.0`、`GAP=0.45`、`TAIL=2.8`；推荐版 JSON 覆盖 lead_in/tail；字幕块 ≤14 字 | `tts.py:38-41, 150` |
| 缓存 | 49 组：完整版 26 句 + 推荐版 13 句全部命中 +4%，另 10 组为旧文案 | 缓存键复算 |
| 旁白混音 | adelay → amix → highpass 80 → loudnorm −16 LUFS / −1.5 dBTP / LRA 11 | `tts.py` |
| 配乐 / 音效 | 与 [[meteor-annual]]、[[sunspot-not-hole]] 同一 `audio.mjs`（112 BPM 拨弦）；推荐版 map 段五个关键词各配一个 `note`，选题出现 `ding`、揭晓 `tada` | `scenes-r3.mjs:138-140` |
| 总混音 | 音乐 0.55 侧链压缩，音效 0.5，`alimiter 0.93`；总线无 loudnorm | `render.mjs:163-164` |
| 烧录字幕 | y=1530，字号从 56 逐级缩小；小宇/太阳角色标签 | `render.mjs:64-92` |

本次未试听、未测响度。

## 7. 迭代与问题

| 事项 | 证据 | 说明 |
| --- | --- | --- |
| 完整版 → 推荐版 | `script-rec.json` `source`；mtime 10-27 16:45–16:57 | 次日在完整版基础上精简：26 → 13 句、178 → 90 s，去掉太阳角色，强化“一定会受影响吗？”的问题导向，结尾改成选择题。推荐版的具体需求原文**未记录** |
| 无片尾品牌卡（两版） | `render.mjs:49-61` 无 outro；`scenes-e.mjs` end 与 `scenes-r3.mjs` map 都延续到 `DURATION + 1` | `tts.py` 的 `TAIL` 注释写“room for the closing brand card”，但实现没有；现行 AGENTS 第 4 节要求“推荐版、精简版等其他版本也要加”（2026-11-01 写入），按现行规则两版都缺 |
| README 目录未收录 | README.md 中无 `solar-eruption-earth` / “太阳爆发” | 其他 4 片在 README 目录中都有条目 |
| 无制作会话 | 证据包 `host_session: null` | 过程只能从文件推断 |
| 复用库未用函数 | `cast.mjs` 星云/尘埃函数在场景中无引用 | 与 sunspot-not-hole 同一复制来源 |
| “示意”声明 | 无 | 页脚为资料来源；`excluded` 把“不画实体护盾/粒子直线撞地”写成内容约束 |

## 8. 复现入口

命令来自 `render.mjs:2-4`、`build/sheet.sh:2`、`tts.py`，**本次均未执行**：

```bash
cd solar-eruption-earth
python3 -m venv .venv && .venv/bin/pip install edge-tts fonttools   # 版本未记录
npm install
# 完整版
.venv/bin/python tts.py                   # → build/{narration.wav,timeline.json}, out/太阳爆发为什么会影响地球.srt
bash build/sheet.sh 0.8 3.0 6.5 11.5      # 可选：build/sheet.png 预览拼图
node render.mjs                           # → out/太阳爆发为什么会影响地球.mp4
# 推荐版
.venv/bin/python tts.py --rec             # → build/rec/…, out/太阳爆发为什么会影响地球_推荐版.srt
node render.mjs --rec --cover             # → out/太阳爆发为什么会影响地球_推荐版_封面.png
node render.mjs --rec                     # → out/太阳爆发为什么会影响地球_推荐版.mp4
```

两版 +4% 缓存齐全，**推断**可离线重建旁白。

## 9. 参考来源

`script.json` / `script-rec.json` 中**没有 URL**，只有资料名称（项目文件中提及，本次未访问）：

| 资料 | 用途 |
| --- | --- |
| NOAA SWPC《Solar Flares (Radio Blackouts)》 | 光速约 8 分钟、日侧 D 层电离、HF 吸收 |
| NOAA SWPC《Coronal Mass Ejections》 | CME 速度、方向、IMF |
| NOAA SWPC《Geomagnetic Storms》 | 南向磁场、导航与电网影响 |
| NOAA SWPC《Aurora》 | 极光椭圆纬度、低纬扩展、电子加速 |
| NASA《Sun: Facts》 | 日地距离 |
| 人教版地理必修第一册 第10—12页 | 教材表述 |
| `geo-story-studio/storage/builds/douyin-solar-eruption-earth/v002`（含 review-002） | 分镜与科学审校来源（按命名推断，未打开） |

相关：[[sunspot-not-hole]]（同库前作）、[[sun-energy]]。

## 10. 证据与限制

| 文件 | 行数 | sha256[:12] | 读取程度 |
| --- | --- | --- | --- |
| `solar-eruption-earth/script.json` / `script-rec.json` | 282 / 150 | `e214c3e187bf` / `4d0e8376d8ea` | 全文（字段与全部台词） |
| `solar-eruption-earth/tts.py` | 216 | `9cd544be1055` | 与 meteor-annual 版 diff |
| `solar-eruption-earth/render.mjs` | 180 | `ba1b23475e16` | 大部分 |
| `solar-eruption-earth/lib.mjs` | 221 | `1e2bddb417c3` | 与 sunspot-not-hole 版 diff |
| `solar-eruption-earth/diagrams.mjs` / `cover.mjs` | 180 / 18 | `4838a7992f4d` / `34419ca08a23` | 1-165 行 / 全文 |
| `solar-eruption-earth/scenes-a…e.mjs` | 118/191/123/204/126 | `7787057224d3` / `eae8d6d09e30` / `376cebf05ebd` / `201e577d7c9e` / `2da49e1c6fcf` | 场景表与头注释 |
| `solar-eruption-earth/scenes-r1…r3.mjs` | 93/125/142 | `c62f9a5069e1` / `515760e340e4` / `441316a206b6` | 场景表；r3 quiz 段 |
| `solar-eruption-earth/build/sheet.sh` | 16 | — | 全文 |
| `solar-eruption-earth/cast/sun/audio/props.mjs`、`fonts.py` | — | `c13a62ae1572` / `994d919821d0` / `f9e04b2cfd54` / `bec5e712b9cd` / `a6ac7264e548` | 同 hash 比对 / 未细读 props |
| `out/*.mp4`、封面 PNG | — | 38,720,984 / 22,354,960 / 938,827 B | 仅证据包 ffprobe 与大小 |

限制：无制作会话；源文件 mtime 有多文件同一分钟（如 22:34）的现象，可能被批量写入或复制改写，只作参考；推荐版需求、预览拼图、用户意见**未记录**；Git 中随 `8cbe673`（2026-11-01，“add 21 high-school geography lessons”批量提交）入库。
