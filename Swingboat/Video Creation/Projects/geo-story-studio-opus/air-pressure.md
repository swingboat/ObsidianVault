---
type: video-production-dossier
project: geo-story-studio-opus
video_id: air-pressure
title: 薯片袋在拉萨鼓成了球：看不见的空气，为什么有气压？
curriculum: 人教版必修第一册 第二章第二节 大气受热过程和大气运动（大气运动：气压、高气压与低气压）；课标 1.4
captured: '2026-11-12'
status: 成片存在（竖屏 1080×1920，201.6 s）；技术检查/视觉检查/用户验收未记录
source_fingerprint: 9f68c9244101ab5ac2559d81a10a490006fabbb6041209674ea5ec5f5020e0fb
tags:
- video-creation
- production-capture
- geography-teaching
- canvas-frame-render
- edge-tts
---

# 薯片袋在拉萨鼓成了球 · “气压破案记”：分子碰撞 → 空气柱 → 海拔 → 同一水平面（Canvas 逐帧 + 四角色 edge-tts）

<!-- capture-source-sha256:9f68c9244101ab5ac2559d81a10a490006fabbb6041209674ea5ec5f5020e0fb -->

本文是根据项目文件做的材料回溯（`script.json`、`tts.py`、`render.mjs`、`lib.mjs`、`cast.mjs`、`props.mjs`、`audio.mjs`、5 个 `scenes-*.mjs`、`build/timeline.json`，以及 evidence pack 里的 ffprobe 结果）。宿主环境中按元数据找到一个可能对应的制作会话 `f8cce535-5299-4fd1-a91b-8750248a1af1`（2026-10-31 20:53 创建，标题“视频制作基于气压内容”），但**读不到对话内容**（宿主返回空 transcript），对应关系只是根据时间戳**推断**：会话创建约 8 分钟后，`script.json` 于 21:01:51 落盘。本次没有运行任何项目脚本。

## 1. 当前结论与制作路线

**状态**：`out/` 里有成片 MP4 和 SRT。画面检查、用户验收、发布情况都**未记录**。

| 项 | 值 | 来源 |
| --- | --- | --- |
| 分辨率 / 帧率 | 1080×1920 竖屏，30 fps | ffprobe；`lib.mjs:7` |
| 帧数 | 6049（`round(201.629×30)`，与 ffprobe 的 201.633 s 一致） | `render.mjs` 主循环公式 |
| 时长 | 201.633 s（timeline 201.629） | ffprobe；`build/timeline.json` |
| 编码 | H.264 libx264 medium CRF 19 yuv420p + AAC 48 kHz 立体声 192k，faststart | ffprobe；`render.mjs:166-178` |
| 文件 | 28,043,560 B，平均约 1.11 Mbps | evidence pack |
| 字幕 | `out/薯片袋在拉萨鼓成了球：看不见的空气，为什么有气压.srt`，88 条；画面同时烧录 | 文件 |
| 其他版本 | 无（`render.mjs` 不再有 `--cover` 分支） | `render.mjs:158` |
| 文件时间 | script 21:01:51 → timeline 21:01:59 → MP4 21:14:20 | mtime |

**一句话管线**：与 [[sky-colors]] 用同一套管线。`script.json`（38 段，4 个声音，全局 `rate: 4`）→ `tts.py`（edge-tts 逐词时间戳，带缓存）→ `timeline.json` / `narration.wav` / SRT → `render.mjs` 按 `scenes-a…e.mjs` 绘制 9 个场景 → 管道送入 ffmpeg，与程序合成的配乐、音效混音。

## 2. 教学目标与课标对齐

- **curriculum**：人教版必修第一册 第二章第二节 大气受热过程和大气运动（大气运动：气压、高气压与低气压，为热力环流打基础）；课标 1.4。
- **核心知识点**：气压的本质（分子碰撞 → 单位面积上的大气压力，单位百帕）；气压约等于上方空气柱的重量；海拔越高气压越低；高压和低压要在**同一水平面**上比较。
- **叙事结构**：
  - 导入：薯片袋到拉萨鼓起来（`ask/bagHi/case/caseQ`），定调为“破案”。
  - 原理：放大空气（`zoom/molHi/hit/force/def`）→ 压力来自四面八方，用覆杯实验演示（`down/allDir/cup`）→ 空气柱重量，约 1013 hPa、每平方米约 10 吨，人体里外平衡（`how…balance`）→ 沿空气柱往上爬（`climb/less/nums`）。
  - 结论：破案（`solve/solve2/bagFat`）。
  - 易错点和迁移：天气预报的高压、低压，要在同一水平面比较；拉萨不是天气图上的低压区；同一水平面有气压差，就有风的起点（`tv…wind`）。高原缺氧不是因为氧气比例变了，而是空气稀薄、氧分子数量少了（`o2Q…o2Lhasa`）。
  - 总结：三句话（`sum1-3`）加薯片袋道别（`bye`）。
- **科学边界**（来自 `source` 里 review-001 列出的审校要点，旁白直接说出）：分子碰撞对各个方向都有作用；“在静止的大气里……约等于”空气柱重量（近静止条件）；比较高低压要同高；氧气比例约 21% 不变，变的是数量。
- **numbers**（7 条，`script.json`）：
  - 分子平均速率约四五百米每秒：√(8RT/πM)，20℃ 时 N₂ 约 470 m/s、O₂ 约 440 m/s；高铁 350 km/h ≈ 97 m/s（NASA Glenn Kinetic Theory）。画面标为“约450 m/s”对比“约97 m/s”。
  - 1013.25 hPa（NOAA JetStream）；101325 N ÷ 9.8 ≈ 1.03×10⁴ kg（约 10 吨）。
  - 拉萨约 650～660 hPa，650/1013 ≈ 0.64（拉萨国家基本气象站多年平均本站气压）。
  - 珠峰顶约 253 mmHg ≈ 337 hPa（West J.B. et al. 1983, J Appl Physiol 54:1188）。
  - 干洁空气中 O₂ 约 21%（教材；NOAA）；拉萨一口气的氧约为海平面的 2/3（同温同体积下分子数与气压成正比，NASA Glenn Equation of State）。
- `script.json` **没有 `based_on`**。`source` 写的是“改编自 Geo Story Studio · douyin-air-pressure v002”，按 AGENTS §1 推断源 build 为 `geo-story-studio/storage/builds/douyin-air-pressure/v002/`（**推断**，未打开）。

## 3. 技术与依赖

| 类别 | 内容 | 依据 |
| --- | --- | --- |
| 渲染 | `@napi-rs/canvas` 声明 `^1.0.11`，lock 解析为 **1.1.0**；没有 npm scripts | `package.json`、lock |
| TTS | `edge-tts` + `aiohttp`，版本未记录；目录里有 `__pycache__/tts.cpython-313.pyc`（21:01），说明运行过的环境是 **CPython 3.13**（其余 4 部片没有这个目录） | 文件名 |
| 字体 | `fonts.py`（依赖 fontTools）抽取圆体-简粗体、苹方-简中粗 | `fonts.py`；`lib.mjs:11-13` |
| 合成 | ffmpeg，参数与 [[sky-colors]] 相同 | `tts.py:181-185`；`render.mjs:173` |

| 角色 | 声音 | rate（+全局 4） | pitch |
| --- | --- | --- | --- |
| 旁白老师 `zh` | zh-CN-XiaoxiaoNeural | +4% | 0 |
| 学生小林 `stu` | zh-CN-YunxiNeural | +10% | +2 Hz |
| 空气分子小氧 `mol` | zh-CN-XiaoyiNeural | +14% | +6 Hz |
| 薯片袋 `bag` | zh-CN-YunxiaNeural | +8% | 0 |

## 4. 镜头与时间轴

| # | 场景 id | 约时间 (s) | 旁白要点 | 画面 | 模块 |
| --- | --- | --- | --- | --- | --- |
| 1 | `intro` | 0–17.1 | 薯片袋鼓了，两条线索 | 雪山和“拉萨 · 海拔约3650米”路牌，鼓起的薯片袋，标题“气压破案记” | `scenes-a.mjs:28` |
| 2 | `zoom` | 17.1–43.0 | 分子撞击表面，气压的定义 | 放大镜从袋面展开；分子来回撞墙并闪光；小氧和高铁赛跑；定义卡“单位：百帕（hPa）” | `scenes-a.mjs:67` |
| 3 | `allDir` | 43.0–57.2 | 四面八方，覆杯实验 | 方盒四周都受推力；水杯倒转，标出“空气向上托住纸片”“水不洒！” | `scenes-b.mjs:26` |
| 4 | `column` | 57.2–90.1 | 空气柱重量，10 吨，里外平衡 | 虚线空气柱（越往上越稀薄），1 平方米上压着约 10 吨，小林“压扁？！”，内外推力平衡 | `scenes-b.mjs:82` |
| 5 | `climb` | 90.1–111.9 | 海拔越高，气压越低 | 金色比较线沿山体上升，气压表跟着读数，标出海平面 / 拉萨 / 珠峰顶三个点 | `scenes-c.mjs:26` |
| 6 | `solve` | 111.9–129.8 | 破案 | 工厂到高原：袋外推力箭头缩短，袋里推力不变，袋子鼓起 | `scenes-c.mjs:62` |
| 7 | `hiLo` | 129.8–158.4 | 同一水平面比较，拉萨不是低压区，风的起点 | 侧视的山顶与山脚 → 平面等压线图（高 / 低）→ 拉萨卡片 → 由高压流向低压的箭头 | `scenes-d.mjs:56` |
| 8 | `o2` | 158.4–182.2 | 氧气比例不变，数量变少 | 两个 21% 饼图；两只“一口气”罐子，分子 42 个对 28 个 | `scenes-e.mjs:42` |
| 9 | `sum` | 182.2–201.6 | 三句话，道别 | 三行要点，角色挥手；约 199 s 起切到片尾品牌卡 | `scenes-e.mjs:78` |

## 5. 关键算法与实现

> 视频**讲的知识**是气压的成因和随高度的变化；下面说的是**做视频用的算法**。画面里的气压读数是按经验公式算出来的，不是实测数据。

**5.1 单一标高的指数气压模型驱动气压表**（`scenes-c.mjs:8-11, 30-46`）：

```js
const yOf = (z) => GROUND - (z / ZMAX) * 720;
// single scale-height model fitted to the quoted values: 3650 m → ~650 hPa, 8848 m → ~337 hPa
const pOf = (z) => 1013 * Math.exp(-z / 8000);
...
z = lerp(z, 3650, easeIO(prog(t, tLh - 0.2, tLh + 1.2)));
z = lerp(z, ZMAX, easeIO(prog(t, tEv - 0.2, tEv + 1.3)));
gauge(ctx, t, 590, y, 92, pOf(z), { a: fadeIn(t, t0 + 0.3), color: C.gold });
```

比较线的海拔 `z` 由几段 lerp 叠加而成：先升到 5200 m，回到海平面，再到拉萨，最后到珠峰顶。每一段的起点都挂在“海拔越高”“海平面”“拉萨”“珠峰顶”这些词的时间戳上。本次按代码复算：`pOf(3650)≈642 hPa`、`pOf(8848)≈335 hPa`，和旁白的“约650”“约1/3”相差在 2% 以内。山体旁的标签直接写“约650百帕”“约1/3”，不显示公式算出的值。

**5.2 分子撞墙与分子群**（`scenes-a.mjs:58-66`；`cast.mjs:139-147`）：
- `bouncer(i,t)`：每个分子有自己的周期（1.6～2.4 s），用三角波在起点和墙之间往返；在相位 0.5（撞墙那一刻）附近 `flash = 1 − |ph−0.5|·12`，墙上相应位置会闪一下。
- `swarmPos(i,t,box,v)`：每个分子按 `rnd` 给出的初速度方向匀速运动，x、y 两个方向各自用三角波 `tri()` 折回盒内，相当于在盒壁上做镜面反射。这种写法是确定性的，不用逐帧积分，任意时刻 t 都能直接求出位置，所以 `--still` 预览帧和正式渲染的画面一致。

**5.3 “一口气”罐子的分子计数**（`scenes-e.mjs:26-41, 63-64`）：海平面罐子 42 个分子，其中 O₂ 9 个；拉萨罐子 28 个，其中 O₂ 6 个。两者 O₂ 占比都是 21.4%，总数之比 28/42 = 2/3，O₂ 之比 6/9 = 2/3。画面上的分子个数就是旁白里的数量关系。

**5.4 同一水平面的等压线面板**（`scenes-d.mjs:20-54`）：平面图上有两个闭合中心，每个中心画 3 圈椭圆等压线。高压中心从内到外标 1020/1015/1010，低压中心从内到外标 1000/1005/1010，即向中心升高或降低，最外圈都是 1010，符合等值线标注习惯。旁白讲到“从高压流向低压”时，5 支金色箭头从“高”水平指向“低”，再加上沿箭头漂移的蓝点。这里**没有画地转偏向**，旁白也只说“风的起点”；偏向放到 [[wind-forces]] 里讲。面板底部写“同一水平面上的等压线（单位：百帕）”，没有指北针。

**5.5 破案的推力箭头**（`scenes-c.mjs:83-91`）：袋子周围 8 个方向，袋外的金色向内箭头长度随 `k` 从 150 缩到 60，线宽从 22 降到 13；袋内的薄荷色向外箭头保持不变；袋子的 `puff` 从 0.08 增加到 1。用“外面推得少、里面推得多”的长度对比，代替数值计算。

**5.6 公共管线**：WSS_URL 替换、sha1 缓存键、429 退避重试、WordBoundary → `word()/at()`、`adelay/amix/loudnorm` 等，都与 [[sky-colors]] 相同（`tts.py:19-25, 47, 53-62, 71, 153, 178-190`）。

## 6. 声音与字幕

- `LEAD_IN=0.8`、`GAP=0.45`（逐段 `gap` 覆盖）、`TAIL=2.8`、`TARGET=None`；混音链为 `aresample 48k → adelay → amix(normalize=0) → highpass 80 → loudnorm I=-16:TP=-1.5:LRA=11`。
- 配乐和音效来自 `audio.mjs`（与其余 4 部片哈希相同 `f9e04b2cfd54`）：112 BPM，C–G–Am–F；音乐音量 0.55 并以旁白侧链压缩，音效 0.5，`alimiter 0.93`；每次切场景自动加 whoosh，片尾卡配 sparkle。
- 字幕：每块不超过 14 字，或使用 `captions` 数组；SRT 88 条；烧录位置 `SUB_Y=1560`；角色标签为小林 / 小氧（`#ff8a8d`）/ 薯片袋（`C.bag`）（`render.mjs:78`）。
- 品牌：左上角 logo（x=64, y=76, w=290）；片尾卡从 `E('bye')+0.3` 开始，标语“用地理的眼光，看懂看不见的空气”；从 `S('sum')` 起页脚显示“资料：NASA Glenn · NOAA JetStream · 人教版地理必修第一册”（`render.mjs:46-76, 109`）。

## 7. 迭代与问题

| 事项 | 证据 | 说明 |
| --- | --- | --- |
| 缺少 `based_on` | `script.json` keys | 不符合 AGENTS §1 的要求，只在 `source` 文字里写了 build 名 |
| 去掉了 `--cover` 分支 | 与 [[sky-colors]] 的 `render.mjs` diff：少了 `--cover` 和 `cover.mjs` 引用，也少了随剧情变化的背景 | **推断**：先做的 sky-colors 有一段失效的 cover 代码，这一部把它删掉了；之后 3 部片的 `render.mjs` 都和本片几乎一样，只有标题、场景模块列表、角色表、标语和页脚不同；[[wind-forces]] 的背景另外改成了风纹 |
| 调色板被后续视频继承 | thermal-circulation、sea-land-breeze 的 `lib.mjs` 里仍有 `bag/bagDark/n2/o2` 等只有本片才用的颜色 | **推断**：后续视频是复制本片目录后改出来的 |
| `__pycache__/` | `tts.cpython-313.pyc`（21:01） | 说明 `tts.py` 被当作模块 import 过一次（例如复用其中的函数）；具体原因**未记录** |
| 规则符合度（代码中存在） | `brand()`/`outro()`/页脚只写资料来源 | 符合 AGENTS §4/§5。片尾卡约 2.6 s（≈ 201.63 − (198.7+0.3)，**推算**），最后 0.6 s 淡出到黑 |
| README | 目录中列为 3:22 | 与 ffprobe 的 201.6 s 一致 |

预览帧（`build/still_*`）已经没有了，返工过程未记录。

## 8. 复现入口

以下命令来自 README 和 `render.mjs` 头部注释，本次**均未执行**：

```bash
cd air-pressure
npm install
python3 -m venv .venv && .venv/bin/pip install edge-tts   # fonts.py 另需 fontTools
.venv/bin/python tts.py            # → build/narration.wav, build/timeline.json, out/<TITLE>.srt
node render.mjs --still 95 120     # → build/still_<t>.png
node render.mjs                    # → out/<TITLE>.mp4
```

改文案和数字在 `script.json`；改气压模型在 `scenes-c.mjs:11`；改等压线数值在 `scenes-d.mjs:28`；改罐子里的分子数在 `scenes-e.mjs:63-64`。

## 9. 参考来源

项目文件里**没有 URL**（`urls: []`）。下表是文件中提到的资料名称，本次**均未访问**：

| 资料 | 用途 |
| --- | --- |
| NASA Glenn《Gas Pressure》《Equation of State》《Kinetic Theory of Gases》 | 分子碰撞、分子速率、分子数与气压成正比 |
| NOAA JetStream《Air Pressure》 | 1013.25 hPa、氧比例 |
| 香港天文台《Introduction to Air Pressure》 | 气压概念 |
| West J.B. et al. 1983, J Appl Physiol 54:1188 | 珠峰顶气压约 337 hPa |
| 西藏拉萨国家基本气象站多年平均本站气压 | 拉萨约 650～660 hPa |
| 人教版地理必修第一册 第二章 | 术语、干洁空气成分 |
| `geo-story-studio/storage/builds/douyin-air-pressure/v002/`（推断路径） | 分镜与 review-001 审校 |

## 10. 证据与限制

| 文件 | 行数 | sha256[:12] | 读取程度 |
| --- | --- | --- | --- |
| `air-pressure/script.json` | 69 | `57089e7c7249` | 全文 |
| `air-pressure/tts.py` | 209 | `b2096837c6aa` | 与 sky-colors 版 diff（只改了标题、LEAD_IN、TAIL 和 docstring） |
| `air-pressure/render.mjs` | 190 | `cfaac57ad559` | 与 sky-colors 版 diff，并读了品牌和字幕部分 |
| `air-pressure/lib.mjs` | 218 | `317dbbaddb31` | 与 sky-colors 版 diff（只改了调色板） |
| `air-pressure/scenes-a…e.mjs` | 178/202/121/147/116 | `8ce1efd9cb87`/`6aebd63d3b56`/`b18d61a72a8e`/`ba44798c116f`/`4aeb511fd10e` | 结构、注释、画面文字全部；scenes-c、d、e 读了关键段落 |
| `air-pressure/cast.mjs` / `props.mjs` | 195/135 | `9c0af96d6ed5`/`eb2f6b7e2d96` | 导出与注释；读了 `swarmPos` |
| `air-pressure/audio.mjs` / `fonts.py` | 204/37 | `f9e04b2cfd54`/`a6ac7264e548` | 与 sky-colors 相同 |
| `air-pressure/build/timeline.json` | — | — | 读取各段 start |
| `air-pressure/out/*.mp4` / `*.srt` | — | 28,043,560 B / 5,732 B | 只看 ffprobe 结果和 SRT 条数 |

限制：没有播放视频，也没有试听；ffprobe 只能说明封装正常。依赖版本（Python 3.13 是从 pyc 文件名推断的）、ffmpeg 版本、渲染耗时都未记录。
