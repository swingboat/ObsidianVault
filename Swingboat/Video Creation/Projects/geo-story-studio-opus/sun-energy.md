---
type: video-production-dossier
project: geo-story-studio-opus
video_id: sun-energy
title: 太阳为什么能发光发热？
curriculum: script.json 未记录（README 归入“必修第一册 第一章第二节 太阳对地球的影响”）
captured: '2026-11-12'
status: 成片存在（竖屏 1080×1920，95.2 s）；技术检查/视觉检查/用户验收未记录
source_fingerprint: 41517a8699ebd207bb3fa9d34daa691fc216cc4737aa9190ec5b5582106f50c0
tags:
- video-creation
- production-capture
- geography-teaching
- canvas-frame-render
- edge-tts
---

# 太阳为什么能发光发热？ · 三声线：核聚变“造氦工厂”与能量外送之旅（Canvas 逐帧 + 逐词时间戳）

<!-- capture-source-sha256:41517a8699ebd207bb3fa9d34daa691fc216cc4737aa9190ec5b5582106f50c0 -->

本文是**根据项目文件做的材料回溯**，依据包括：源码、`script.json`、`build/timeline.json`、`out/` 下的成片与字幕，以及证据包里的 ffprobe 结果和 sha256 清单。证据包中 `host_session` 为空，**未发现制作会话**，制作对话、需求原文和修改过程都无法找回。本次没有运行任何项目脚本（没跑 `tts.py`、`render.mjs`、npm、pip），也没有打开视频或音频。

## 1. 当前结论与制作路线

**状态：成片存在**，包括 `out/太阳为什么能发光发热.mp4` 和同名 `.srt`。目前只有证据包里的 ffprobe 媒体头信息。视觉检查、试听、响度测量、用户验收和发布情况都**未记录**。

| 项 | 值 | 来源 |
| --- | --- | --- |
| 分辨率 / 画幅 | 1080×1920，9:16 竖屏 | ffprobe；`lib.mjs:7` |
| 帧率 / 帧数 | 30 fps；`Math.round(95.172 × 30)` = 2855 帧 | ffprobe；按时长推算 |
| 时长 | 95.167 s（timeline 95.172 s） | ffprobe；`build/timeline.json` |
| 视频编码 | H.264，yuv420p，CRF 19，preset medium，`+faststart` | ffprobe；`render.mjs` |
| 音频编码 | AAC 48 kHz 立体声，192 kbps | ffprobe；`render.mjs` |
| 文件大小 | 24,635,391 字节（约 24.6 MB，平均约 2.07 Mbps） | 证据包 |
| 字幕 | `.srt` 49 条（2887 字节），画面另有烧录字幕 | `timeline.json` cues |
| 其他版本 | 无 | `out/` 列表 |

文件时间：TTS 产物 10-20 20:05，成片 20:18。**所有源码（`script.json`、`render.mjs`、`scenes-*.mjs`、`sun.mjs`）的 mtime 都是 20:22，比成片晚 4 分钟**（见 §7）。git 里只有初始提交 `4831a07`。

一句话管线：

> `script.json`（26 段：旁白、小宇、太阳）→ `tts.py` 调 edge-tts，取 WordBoundary 逐词时间戳 → `timeline.json` / `narration.wav` / SRT → `render.mjs` 动态加载 `scenes-a…e.mjs`（9 个场景）、`sun.mjs`（太阳剖面、核子、光波）、`cast.mjs`、`props.mjs` → 程序生成音频 → 通过管道交给 ffmpeg 输出 MP4。

## 2. 教学目标与课标对齐

- **curriculum 字段缺失**。README 把本片归入“必修第一册 第一章第二节 太阳对地球的影响”，简介是“太阳能量来自核心的核聚变，以辐射形式向外传递”。对应的课标条目**未记录**。
- **核心知识点**：太阳是由高温等离子体构成的恒星，它发光发热不是靠燃烧，而是靠核心的核聚变（氢核经过一系列反应形成氦核，减少的质量转化为能量）。能量经辐射区、对流区到达光球，再以太阳辐射的形式射向太空，其中很小一部分到达地球。
- **叙事结构**：
  - 导入：小宇问太阳“烧木头还是烧煤？”，接着用“烧煤几千年就烧光”和“已亮 46 亿年”的矛盾引出问题，太阳回答“我不是在烧火”。
  - 原理：等离子体 → 钻进核心 → 核聚变 → 称质量（质量亏损）→ 每秒约 400 万吨。
  - 能量外送：辐射区里走走停停 → 对流区像煮汤一样翻滚 → 到达光球 → 约 8 分钟传到地球。
  - 总结：三步（产生、向外传递、以辐射形式到达地球）。
  - 收尾：太阳说“还能再亮约 50 亿年”，小宇总结。
- **科学边界**：
  - 用“氢核……经过一系列反应”的说法，没有把过程简化成“4 个氢一步合成”。工厂图示的标题是“氢核 → 氦核 + 能量”，同时配有“一系列反应”的文字。
  - 讲光和热的来源时用“主要来自”。
  - 辐射区的时间写作“上万年甚至更久”。
  - 强调“只有很小一部分到达地球”。
- **数字**（没有 `numbers` 字段，出处没有逐条对应）：

| 数字 | 位置 | 出处 |
| --- | --- | --- |
| 已亮约 46 亿年（数字滚动） | `age`；`scenes-a.mjs:106-115` | source 笼统 |
| 核心约 1500 万 ℃ | `core` | 同上 |
| 每秒约 400 万吨物质变成能量 | `tons`；`scenes-c.mjs:131-140` | 同上 |
| 辐射区“上万年+” | `rad`；`scenes-d.mjs` 印章 | 同上 |
| 约 8 分钟到达地球 | `space`；`scenes-e.mjs:45` | 同上 |
| 还能再亮约 50 亿年 | `sunBye`；`scenes-e.mjs:113` | 同上 |
| **轻了约 0.7%** | 只出现在画面（`scenes-c.mjs:66`），旁白只说“轻了一点点” | `script.json` 中没有这个数字 |
| 烧煤“几千年就烧光” | `coal` | 同上 |

- **based_on（推断）**：`source` 中写的是“douyin-sun-energy v004 分镜”，对应 `geo-story-studio/storage/builds/douyin-sun-energy/v004/`，本次未核对。

## 3. 技术与依赖

| 类别 | 内容 | 证据 |
| --- | --- | --- |
| 渲染 | `@napi-rs/canvas` 声明为 `^1.0.11`，lock 文件锁定 **1.0.11** | `package.json`；`package-lock.json` |
| npm scripts | 无 | `package.json` |
| TTS | `edge-tts` + `aiohttp`，没有 requirements 文件 | `tts.py` |
| 字体 / ffmpeg | 与 [[nebula-nursery]] 相同（`fonts.py` 用 fontTools 抽取系统字体；TTS 和渲染两个阶段都用 ffmpeg） | `fonts.py`；`tts.py:176-190`；`render.mjs:149-158` |
| 本地环境（非声明） | `.venv` 中可以看到 `edge_tts-7.3.1`、`fonttools-4.67.0` | 目录列表 |

**声线**（全局 `rate: 4`）：

| 角色 | voice | 实际 rate / pitch | 字幕胶囊 / 字色 |
| --- | --- | --- | --- |
| 旁白 `zh` | zh-CN-XiaoxiaoNeural | +4% / +0 Hz | 无 / 白 |
| 小宇 `kid` | zh-CN-YunxiaNeural | +10% / +8 Hz | “小宇” / `#c9f1ff` |
| 太阳 `sun` | zh-CN-YunjianNeural | +4% / −4 Hz | “太阳”（金色）/ `#ffe7a8` |

## 4. 镜头与时间轴

| 场景 id | 时间（约） | 旁白要点 | 画面 | 模块 |
| --- | --- | --- | --- | --- |
| `hook` | 0–14.2 s | 烧木头还是烧煤？几千年烧光 vs 46 亿年；太阳：我不是在烧火 | 燃料条被耗尽，出现“几千年就烧光！”，46 亿年数字滚动，红色大叉划掉“燃烧”猜想 | `scenes-a.mjs` |
| `plasma` | 14.2–17.9 s | 太阳是恒星，由高温等离子体构成 | 标签“恒星”和“高温等离子体：热到原子都被拆成了带电粒子” | `scenes-a.mjs` |
| `dive` | 17.9–21.7 s | 能量源头在核心；小宇喊“冲呀！” | 太阳右半边被切开，露出核心、辐射区、对流区 | `scenes-b.mjs` |
| `core` | 21.7–34.7 s | 核心约 1500 万 ℃；氢核 → 氦核 + 能量；核聚变；造氦工厂 | 22 个拥挤晃动的氢核，温度计，带齿轮和烟囱的“工厂”机器，印章“核聚变！” | `scenes-b.mjs` |
| `mass` | 34.7–41.7 s | 称一称：氦核更轻；少掉的质量 → 能量 | 天平：左边 4 个氢核，右边 1 个氦核，指针偏转 | `scenes-c.mjs` |
| `tons` | 41.7–46.1 s | 每秒约 400 万吨 | 秒表转一圈，吨砝码落进太阳后化为光，数字滚动 | `scenes-c.mjs` |
| `trip` | 46.1–66.5 s | 辐射区走走停停（上万年+）；对流区像一锅汤；到达光球 | 能量包在剖面里随机游走，沿对流环上升，出现煮汤锅插画 | `scenes-d.mjs` |
| `space` | 66.5–76.6 s | 以太阳辐射形式奔向太空，约 8 分钟到地球，只有一小部分到达 | 多条光波向外传播，一条射向地球，配时钟和“约 8 分钟”，地球戴上墨镜 | `scenes-e.mjs` |
| `sum` | 76.6–95.2 s | 三步总结；还能亮约 50 亿年；不是烧火而是核聚变 | 答案卡，印章“还能亮约 50 亿年”，告别 | `scenes-e.mjs` |

## 5. 关键算法与实现

> **视频讲的科学**：核聚变、质量亏损、辐射区与对流区的能量传递、太阳辐射。**做视频用的算法**：确定性随机游走、分区剖面绘制、天平阻尼摆动、数字滚动。随机游走路径只是“反复吸收、放出”的直观示意，不是光子扩散的真实模拟，步数和时间尺度都不对应。

### 5.1 辐射区：带偏置的确定性随机游走（`scenes-d.mjs:10-30`）

```js
const WALK = (() => {
  const pts = [[Math.cos(A0) * ZONE.core * R * 1.05, Math.sin(A0) * ZONE.core * R * 1.05]];
  for (let i = 0; i < 260; i++) {
    const [x, y] = pts[pts.length - 1];
    const r0 = rnd(i * 7 + 3) * TAU, bias = 0.12;
    const a = Math.atan2(Math.sin(r0) + bias * Math.sin(A0), Math.cos(r0) + bias * Math.cos(A0));
    let nx = x + Math.cos(a) * R * 0.05, ny = y + Math.sin(a) * R * 0.05;
    const r = Math.hypot(nx, ny);
    if (r < ZONE.core * R * 1.05) { nx *= (ZONE.core * R * 1.1) / r; ny *= (ZONE.core * R * 1.1) / r; }
    pts.push([nx, ny]);
    if (r >= ZONE.rad * R * 0.97) break;
  }
```

- 每一步方向随机，同时朝出口方向 `A0` 有 12% 的偏置，步长为 0.05R。走进核心就被推回核心边缘外，走到辐射区外沿（0.97 × 0.7R）就停止。
- `walkAt(p)` 在折线上线性插值。进入对流区后改用 `convAt(p)`：半径从 0.7R 升到 0.985R，同时叠加两周期的正弦摆动，模拟“翻滚上升”（`scenes-d.mjs:32-37`）。
- 分区比例在 `sun.mjs:33` 中定义为 `ZONE = { core: 0.25, rad: 0.7 }`。`interior()` 画出 18 个对流小环，配交替方向的旋转粒子、辐射区渐变和随时间脉动的白色核心（`sun.mjs:34-66`）。

### 5.2 天平：质量亏损的可视化（`scenes-c.mjs:44-72`）

```js
const tilt = easeOut(prog(t, tLight - 0.2, tLight + 0.6));
const wob = Math.sin((t - tLight) * 14) * 0.03 * Math.exp(-Math.max(0, t - tLight) * 3) * (t > tLight ? 1 : 0);
const ang = -0.09 * tilt + wob;
```

- 在旁白说到“轻了”时，横梁倾斜 0.09 rad，并叠加一次指数衰减的 14 rad/s 摆动。
- 氦核一侧弹出“轻了约 0.7%”的标签；随后在说到“能量”时，从氦核秤盘飘出一团光并炸开。

### 5.3 数字滚动与对位

- 46 亿年和每秒 400 万吨两个数字，都用 `Math.round(N * easeOut(prog(t, tNum-0.15, tNum+0.8)))` 在约 1 s 内从 0 滚到目标值，起点是对应数字被说出的时刻（`scenes-a.mjs:106-115`、`scenes-c.mjs:131-140`）。
- 16 个“吨”砝码依次以 0.16 s 的间隔抛进太阳，到达后闪光（`scenes-c.mjs:120-129`）。

### 5.4 共用的渲染与 TTS 机制

- `render.mjs`：动态加载 `scenes-a…e`（`render.mjs:14-15`），字体自适应的说话人字幕，交叉淡化 `XF=0.4`，logo 位于 `x=64,y=76,w=290`（`render.mjs:47-59`）。背景在 `kidGo` 之后转为暖橙色，到 `surface` 之后恢复（`render.mjs:29-32`）。
- `lib.mjs` 与 [[sunlight-through-space]]、[[sun-angle]] 完全相同（`7d757aa5fc33`），只比 [[nebula-nursery]] 多了一行太阳配色（`core/rad/conv/photo/proton/neutron/energy`）。
- `sun.mjs`（`210e265385b4`）首次出现在本片，后面两片原样复用。其中的 `lightWave()` 也用于 [[sunlight-through-space]] 的穿越真空场景。
- `tts.py` 与 [[nebula-nursery]] 相比，只改了 docstring（增加 “the Sun” 角色）、`TITLE`、`TARGET 100.0` 和 `TAIL 2.8`。

## 6. 声音与字幕

- 三种声线的 rate 和 pitch 见 §3。`LEAD_IN 1.0`，各段 `gap` 在 0.2–0.5 s 之间，`TAIL 2.8`。时间轴中 `rate: 4`，95.2 s 短于 `TARGET` 100 s，**没有提速**。
- TTS 混音链：`adelay → amix → highpass 80 → loudnorm I=-16/TP=-1.5/LRA=11`。成片阶段再加入配乐（112 BPM，音量 0.55，sidechain 闪避）和音效（音量 0.5），最后接 `alimiter 0.93`。实际响度**未测量**。
- 缓存：`build/tts_cache/` 共 52 个文件，等于 26 段 × 2。
- 字幕：每条的起止时间都取自逐词时间戳。太阳说话时，字幕胶囊为金色、字色为 `#ffe7a8`；小宇则是天蓝色胶囊。从 `sum` 开始，页脚淡入“资料：NASA Science · NASA STEM · NASA Marshall”。

## 7. 迭代与问题

1. **源码比成片新**：所有 `.mjs` 和 `script.json` 的 mtime 都是 10-20 20:22，成片是 20:18。`script.json` 与 `timeline.json` 的逐段文本、captions 和 gap 都一致（本次比对过），所以文案没有改动。至于渲染代码在成片之后是否改过画面，**无法确认**。所有文件 mtime 相同，更像是批量写回或复制造成的（**推断**）。
2. **没有片尾品牌卡**：`tts.py:35` 注释写着“room for the closing brand card”，但代码里没有 `outro()`。最后的 `sum` 场景 `to: Infinity` 一直画到结束，最后 0.6 s 淡出到黑。这不符合现行 AGENTS.md §4。
3. **画面数字没有出处**：“轻了约 0.7%”只出现在画面上，`script.json` 里没有这个数字，`source` 也没有逐条对应。
4. **`script.json` 缺字段**：缺 `curriculum`、`based_on`、`numbers`（AGENTS.md §6）。
5. **`TARGET = 100.0`**：与现行规则（默认 None）不一致，但实际没有提速。
6. **符合 §5**：页脚没有“示意”字样（`render.mjs:91` 的注释也引用了 §5）。
7. **系列关系**：[[sunlight-through-space]] 的开场明确写着“上一期讲到，太阳会发出太阳辐射”，并贴了“上一期：太阳为什么能发光发热”的标签，说明本片是该系列的上一集。
8. README 目录中有收录（第 33 行，时长 1:35）。

## 8. 复现入口

本次**没有运行**。下面的顺序取自 AGENTS.md 的常用命令和文件头注释：

```bash
python3 -m venv .venv && .venv/bin/pip install edge-tts   # fonts.py 另需 fonttools（未声明）
npm install                                               # @napi-rs/canvas 1.0.11
.venv/bin/python tts.py            # → build/seg*.mp3, narration.wav, timeline.json；out/太阳为什么能发光发热.srt
node render.mjs --still 30 55      # → build/still_<t>.png
node render.mjs                    # → build/music.wav, sfx.wav；out/太阳为什么能发光发热.mp4
```

字体依赖 macOS 系统资源。缺少字体时，`lib.mjs:11-13` 会自动调用 `fonts.py`。

## 9. 参考来源

`script.json` 和代码中**没有 URL**（证据包 `urls` 为空）。资料名来自 `source` 字段：

- NASA Science: Our Sun: Facts（太阳年龄、核心温度、结构；项目文件中提及，本次未访问）
- NASA Stars（恒星与核聚变；项目文件中提及，本次未访问）
- NASA STEM: How Does the Sun Behave?（太阳内部能量传递；项目文件中提及，本次未访问）
- NASA Marshall: Why We Study the Sun（太阳研究背景；项目文件中提及，本次未访问）
- 分镜来源：`geo-story-studio/storage/builds/douyin-sun-energy/v004/`（路径为推断，本次未访问）

## 10. 证据与限制

| 文件 | sha256 前 12 位 | 说明 |
| --- | --- | --- |
| `script.json` | `bcc18df69e53` | 26 段 |
| `tts.py` | `e3e79f4f0953` | TARGET 100、TAIL 2.8 |
| `render.mjs` | `c90c321bf49c` | 动态加载，暖色背景，太阳字幕胶囊 |
| `lib.mjs` | `7d757aa5fc33` | 与后两片相同 |
| `sun.mjs` | `210e265385b4` | 太阳剖面、核子、地球、光波 |
| `cast.mjs` / `props.mjs` | `e7f29e66c6d0` / `4debdbd1b652` | 小宇等角色、通用道具 |
| `scenes-a…e.mjs` | `f9a38509f141` / `c4ad9be3edfc` / `e19130491bb3` / `1367cc0c15e2` / `0b65ac947dc7` | 9 个场景 |
| `audio.mjs` / `fonts.py` | `f9e04b2cfd54` / `a6ac7264e548` | 5 片相同 |

- **已读**：`script.json` 全文，`tts.py` 和 `render.mjs` 的逐行 diff，`sun.mjs`、各场景文件的结构和关键段落，`timeline.json` 的段落时间。
- **未读 / 未做**：媒体文件、`node_modules/`、`.venv/`、缓存内容；没有核对源 build、NASA 资料和各项数字。
- **缺失**：制作会话、需求原文、审片记录、发布状态、模型信息、ffmpeg 版本、实测响度，以及 20:22 这次修改的内容。
