---
type: video-production-dossier
project: geo-story-studio-opus
video_id: meteor-journey
title: 看见流星，地上就有陨石吗？
curriculum: script.json 未记录（README：流星体、流星、陨石的区别）
captured: '2026-11-12'
status: 成片存在（竖屏 1080×1920，87.9 s）+ 4 张封面 PNG；技术检查/视觉检查/用户验收未记录
source_fingerprint: d6bd637acd387b270967f8e98fadce6770e08a6507c2f1970f3680ddb3629820
tags:
- video-creation
- production-capture
- geography-teaching
- canvas-frame-render
- edge-tts
---

# 看见流星，地上就有陨石吗？ · 会说话的小石头一路改名（竖屏 11 场景 + 自动提速 + 多比例封面）

<!-- capture-source-sha256:d6bd637acd387b270967f8e98fadce6770e08a6507c2f1970f3680ddb3629820 -->

本文是**根据项目文件做的材料回溯**（源码、`script.json`、`build/timeline.json`、`build/tts_cache` 的文件名与时间、`out/` 文件清单、证据包中的 ffprobe 与 sha256）。证据包 `host_session` 为空：**未发现制作会话**。本次未运行任何项目脚本，也没有打开视频、音频或封面图片。

## 1. 当前结论与制作路线

**状态：成片存在**——`out/看见流星地上就有陨石吗.mp4`、同名 `.srt`，以及 4 张封面 PNG。只有 ffprobe 媒体头检查；画面、声音、封面的视觉检查与用户验收、是否发布均**未记录**。

| 项 | 值 | 来源 |
| --- | --- | --- |
| 分辨率 / 画幅 | 1080×1920，9:16 竖屏 | ffprobe；`lib.mjs:7` |
| 帧率 / 帧数 | 30 fps；`round(87.868 × 30)` = 2636 帧 | ffprobe；`render.mjs:178`（推算） |
| 时长 | 87.867 s（timeline 87.868 s） | ffprobe；`build/timeline.json` |
| 视频 | H.264，yuv420p，**CRF 19**，preset medium，faststart | ffprobe；`render.mjs:175-176` |
| 音频 | AAC 48 kHz 立体声 192 kbps | ffprobe；`render.mjs:176` |
| 文件 | 15,791,339 字节（约 15.8 MB） | 证据包 |
| 字幕 | `.srt` 49 条 + 画面烧录 | 文件；`render.mjs:78-102` |
| 封面 | `封面-竖3比4.png`（1200×1600）、`封面-横4比3.png`（1440×1080）、`封面-横16比9.png`（1920×1080），均由 `cover.mjs` 生成；另有较早的 `封面.png`（432,298 字节），**当前代码中找不到生成它的入口** | `cover.mjs:94-97`；文件时间 |

文件时间：TTS 缓存 10-16 21:07–21:13，`narration.wav`/`timeline.json`/`.srt` 21:13，配乐音效 21:24，成片与 `封面.png` 21:25；三张比例封面 10-17 16:44（次日）。

一句话管线：

> `script.json`（25 段：旁白 + “小石头”第一人称）→ `tts.py`（`TARGET = 88` 秒上限，超出自动一次性提速）→ 逐词时间轴 + 旁白混音 + SRT → `render.mjs` 拼接 4 个场景模块共 11 个场景，顶部“一块石头的 3 个名字”进度条随词点亮 → 程序合成配乐 / 音效 → ffmpeg 管道编码；`cover.mjs` 另出三种比例的抖音封面。

## 2. 教学目标与课标对齐

- **curriculum 字段：缺失**（`script.json` 键为 `title`、`voice`、`voices`、`sources`、`segments`）。对应人教版必修第一册第一章“地球的宇宙环境”中天体与流星现象的内容属**推断**，项目文件未写明。README 第 22 行概括为“流星体、流星、陨石的区别；大多数流星体在大气中烧蚀殆尽”。
- **核心知识点**：同一块物体在三个阶段的名称——太空中是流星体（天体）、进入大气发光的现象叫流星、落地残余叫陨石。只讲这一个点。
- **讲解结构（按实际文案）**：
  1. 导入：“看见流星，地上就一定有陨石吗？”→“不一定！大多数时候，一颗都没有”（`hook`、`no`、`three`）。
  2. 原理拆解：小石头自我介绍 → 流星体（大小从灰尘到小行星）→ 每秒几十公里冲入大气 → **纠正“摩擦着火”**：前方空气被猛烈压缩变得极热而发光 → 这道光叫流星，“是一种现象，不是石头本身”（`hi`…`name2`）。
  3. 结论：绝大多数流星体半路耗尽，每天约 44 吨几乎全部汽化；少数落地才叫陨石，通常不到原来的 5%，带熔壳（`hot`…`alive`）。
  4. 迁移应用：流星雨后别急着捡陨石——流星雨多来自彗星碎屑，太脆，几乎到不了地面（`tip`、`comet`）。
  5. 总结 + 考题：三句口诀；“博物馆里从天而降的那块石头叫什么？评论区告诉我！”（`recap`…`quiz`）。
- **科学边界**：发光机制的旁白采用“前方空气被压缩加热”说法，并明确否定“摩擦着火”。教材或教辅里常见“与大气摩擦燃烧”一类表述（本次未核对人教版原文）；AGENTS.md 第 6 节要求“以教材为主线，必要时补一句说明”，本片是直接纠正，是否与教材口径冲突**需人工核对**。
- **数字与出处**：`script.json` 有 `sources`（3 条 NASA 页面，见 §9），但没有逐条对应的 `numbers` 字段。画面上的数字：“几十公里”每秒（`scenes-b.mjs:45`）、每天约 44 吨（计数动画 `scenes-c.mjs:40`）、落地不到 5%（`scenes-c.mjs:96`）。哪个数字来自哪一页没有记录。

## 3. 技术与依赖

| 类别 | 内容 | 证据 |
| --- | --- | --- |
| 渲染 | Node ESM + `@napi-rs/canvas` `^1.0.11`；`scripts` 只有默认 `test` 占位 | `package.json` |
| TTS | Python `edge-tts` + `aiohttp`，版本未记录 | `tts.py:15-17` |
| 字体 | `fonts.py`（fontTools）抽取圆体 / 苹方；新增 `SANS = 'PingB, UniB, HiraGB'`、`NUM` 数字字体 | `lib.mjs:20-22` |
| 编码 | 系统 ffmpeg，版本未记录 | 代码 |

声线（`script.json` + `timeline.json`）：

| 角色 | voice | 脚本 rate | 实际 rate（+13 全局） | pitch | 段数 |
| --- | --- | --- | --- | --- | --- |
| `zh` 旁白 | `zh-CN-XiaoxiaoNeural` | +6% | **+19%** | 0 | 21 |
| `rock` 小石头 | `zh-CN-YunxiaNeural` | +8% | **+21%** | +8 Hz | 4 |

## 4. 镜头与时间轴

时间来自 `build/timeline.json` 与各模块 `from/to`（多在下一段前 0.2–0.35 s 切换，`XF = 0.3`）。

| # | 场景 | 约时间 (s) | 口播要点 | 画面 | 模块 |
| --- | --- | --- | --- | --- | --- |
| 1 | `wish` | 0–6.6 | 钩子 + “不一定！” | 地面许愿的人、划过的流星、“不一定”印章 | `scenes-a.mjs:18-45` |
| 2 | `journey` | 6.6–10.0 | 三个名字 | 太空→天空→地面路线图，名字先隐藏 | `scenes-a.mjs:46-77` |
| 3 | `meet` | 10.0–18.9 | 小石头登场，流星体 | 带表情的石头，灰尘到小行星尺寸对比 | `scenes-a.mjs:78-123` |
| 4 | `dive` | 18.9–24.8 | “冲呀！”每秒几十公里 | 地球与大气层，石头俯冲 | `scenes-b.mjs:5-50` |
| 5 | `truth` | 24.8–32.7 | 不是摩擦着火 | 空气分子堆积在前方变热、气帽发光，三步说明 | `scenes-b.mjs:51-110` |
| 6 | `meteorView` | 32.7–37.7 | 这道光才叫流星 | 地面视角看流星，“现象”标签 | `scenes-b.mjs:111-140` |
| 7 | `shrink` | 37.7–48.1 | 好烫，变小，44 吨 | 烧蚀碎屑，吨数计数卡 | `scenes-c.mjs:5-51` |
| 8 | `landed` | 48.1–64.3 | 陨石、5%、熔壳 | 落地、熔壳反光、“我活下来啦” | `scenes-c.mjs:52-119` |
| 9 | `shower` | 64.3–72.4 | 别急着捡；彗星碎屑 | 辐射点放射状流星雨、彗星、提篮子的人，“地面：几乎 0” | `scenes-d.mjs:5-60` |
| 10 | `recap` | 72.4–79.7 | 三句口诀 | 三行名称 + 关键词卡 | `scenes-d.mjs:61-89` |
| 11 | `quiz` | 79.7–87.9 | 博物馆考题 | A/B/C 三选项 | `scenes-d.mjs:90-129` |

常驻层（`render.mjs`）：顶部“一块石头的 **3** 个名字”和三枚芯片，说到“流星体 / 流星 / 陨石”时依次点亮，复习段前淡出（`render.mjs:45-75`）；字幕位于 `CAP_Y = 1450`；底部 y = 1860 有一行小字声明（见 §7）。

## 5. 关键算法与实现

> 区分：视频**讲解的知识**是流星体 / 流星 / 陨石的定义与流星发光原理；本节是**制作所用的算法**——示意性的粒子与渐变绘图，不是高超声速气体动力学或烧蚀模拟。

### 5.1 自动提速与它在缓存里留下的痕迹（`tts.py:148-158`）

```python
rate = script.get("rate", 0)
while True:
    words = synth(script, rate)
    speech = [w[-1]["end"] + 0.12 for w in words]
    total = LEAD_IN + sum(speech) + sum(gaps) + TAIL
    if total <= TARGET or rate >= 30:
        break
    rate = min(30, max(rate + 2, math.ceil(rate + (sum(speech) / available - 1) * 100) + 1))
```

`TARGET = 88.0`（注释“upper bound for a Douyin-length explainer”）、`LEAD_IN = 0.6`（“Douyin hook: speak almost immediately”）。这段循环给每个角色的 rate 再加一个全局增量，`timeline.json` 记录最终为 `rate: 13`。

本次用缓存键公式 `sha1(voice|rate|pitch|text)[:16]` 对 `build/tts_cache/` 的 108 个 mp3 反查（只做哈希计算，没有调用 TTS）：

- 25 段当前文案都有全局 +0 和 +13 两版（= 一次运行：原速超长 → 一步算到 +13 → 达标）。
- 其中 17 段还有 +29、+30 两版；另外 24 个 mp3 对不上当前文案，正好是 8 段旧文案 × 3 种语速（+0/+29/+30）。
- 对不上 +29/+30 的 8 段是 `size`、`truth`、`name2`、`tons`、`five`、`crust`、`comet`、`quiz`。

**推断**：初稿这 8 段更长，原速超出 88 s，脚本一路提到 +30%（上限）仍收不住；随后改写了这 8 段（多为科学解释段），重跑后只需 +13%。缓存文件时间 21:07–21:13 都在同一晚，旧稿文本本身已不可见。

### 5.2 “压缩空气发光”的示意（`scenes-b.mjs:60-80`、`bodies.mjs:176-194`）

60 个空气分子沿石头运动方向反向流过；靠近石头正前方（侧向偏移 < 260 px）的分子被“堆”在一个随距离变化的最小距离上，`glow` 从“压缩”到“发出光来”逐渐变 1，前方分子由淡蓝变成橙白；`airCap` 在前端画三层闪烁弧线 + 径向渐变的“热空气帽”，`streak` 拖出由暖白到橙红的尾迹。画面直接对应旁白的三步：压缩 → 极热 → 发光（`STEPS`，`scenes-b.mjs:54-58`）。

### 5.3 流星雨辐射点（`scenes-d.mjs:7, 28-41`）

```js
const RAD = [900, 330]; // radiant the shower seems to come from
for (let i = 0; i < 40; i++) {
  const t0 = S('tip') - 0.3 + i * 0.19 + rand(i) * 0.3, life = 0.7;
  const p = prog(t, t0, t0 + life);
  const ang = 1.95 + (rand(i + 9) - 0.5) * 1.1;
  const d0 = 120 + rand(i + 4) * 160, d1 = d0 + 260 + rand(i + 2) * 260;
  streak(ctx, RAD[0] + Math.cos(ang) * d, RAD[1] + Math.sin(ang) * d, ...);
}
```

40 条短寿命流星都从同一辐射点向外，且在离地面很高处就淡出（`1 - prog(p, 0.6, 1)`），配合“地面：几乎 0”卡片，表现“彗星碎屑到不了地面”。

### 5.4 逐词驱动的名称进度条与计数（`render.mjs:46-75`、`scenes-c.mjs:40`）

三枚芯片的点亮时刻分别取 `at('name1','流星体')`、`at('name2','流星')`、`at('name3','陨石')`；44 吨计数从 `S('tons') + 0.2` 增长到 `atEnd('tons','吨')`——`atEnd` 是本片在 `lib.mjs` 新增的“词结束时刻”取值函数。

### 5.5 盖章与封面复用（`lib.mjs:167-185`、`cover.mjs`）

`stamp()` 从 2.4 倍以 `easeIn` 砸到 1 倍，再轻微回弹，注释称为抖音“盖章”节拍（“不一定！”“别急着捡！”）；配 `stamp` 音效和 `shake`。`cover.mjs` 直接复用 `rockGuy`、`streak`、`stamp`，按 `s = h/1080` 或 `w/1200` 缩放出 3:4 竖封面与 4:3、16:9 横封面（`cover.mjs:47-97`）。

### 5.6 音效扩展（`audio.mjs`）

在项目通用 `audio.mjs` 的基础上新增 5 种程序音效：`fizz`（空中熄灭）、`boom`（冲入大气）、`roar`（压缩空气的持续轰鸣）、`thud`（落地）、`sizzle`（“好烫”）。配乐仍是 112 BPM 拨弦 bed（与 [[geo-celestial-bodies]] 相同）。

## 6. 声音与字幕

| 项 | 内容 | 来源 |
| --- | --- | --- |
| 留白 | `LEAD_IN 0.6`、默认 `GAP 0.45`、各段 `gap` 0.15–0.7、`TAIL 2.8`（“room for the closing confetti”） | `tts.py:31-35`；`script.json` |
| 旁白混音 | `adelay` → `amix normalize=0` → `highpass 80` → `loudnorm I=-16:TP=-1.5:LRA=11` | `tts.py`（与全项目同一实现） |
| 总混音 | 配乐 **×0.75** 侧链压缩，音效 ×0.5，`alimiter 0.93` | `render.mjs:172-173` |
| SRT | 49 条；首条 `00:00:00,642 看见流星`，末条 `00:01:23,950 → 00:01:24,948 评论区告诉我！` | `out/*.srt` |
| 烧录字幕 | 60 px 白字 14 px 深色描边，无底框；超过 960 px 等比缩小；小石头台词上方加金色“小石头”标签、字色淡金 | `render.mjs:78-102` |
| 安全区 | 字幕 y = 1450，注释“sit above Douyin's bottom caption/UI area” | `render.mjs:20` |

## 7. 迭代与问题

| 事项 | 证据 | 说明 |
| --- | --- | --- |
| 初稿 8 段文案被改写、语速从 +30% 降到 +13% | 缓存反查（§5.1） | **推断**；改写前文本不可见 |
| 画面底部有“示意”类声明 | `render.mjs:142` `'科普示意 · 非实拍 · 大小与距离不按比例'` | 与 AGENTS.md 第 5 节“画面上不要放示意类免责声明”冲突；该节说明已做好的视频不必回改 |
| 无账号 logo、无片尾品牌卡 | `render.mjs` 无 logo 绘制；无 `assets/`；结尾是考题 + 彩纸 | 与第 4 节不符（**推断**本片早于该规则） |
| 语速被自动提到 +19%/+21% | `tts.py:32`；`timeline.json` `rate: 13` | 与第 2 节“`TARGET = None`，不要为压时长提速”不符；git `35ad4da` 提交说明为“drop narration length cap and keep normal speech rate”，**推断**该规则晚于本片 |
| 结尾号召“评论区告诉我！” | `script.json` `quiz` 段 | 第 4 节只禁止品牌卡里放号召文字、第 6 节要求收尾不做成广告式口号；这里以考题形式出现，是否算违规属判断问题 |
| 缺 `curriculum` / `numbers` | `script.json` | 与第 6 节不符；`sources` 只列页面，不对应具体数字 |
| `封面.png` 无生成入口 | 与成片同为 21:25 生成，但 `cover.mjs` 只写三张比例封面 | **推断**是早期封面脚本或渲染模式的产物，后被替换；无法确认 |

README 已收录（第 22 行，竖屏 1:28）。

## 8. 复现入口

命令来自 AGENTS.md“常用命令”和 `render.mjs:1-3`、`cover.mjs:2`、`tts.py:4`，**本次均未执行**：

```bash
cd meteor-journey
python3 -m venv .venv && .venv/bin/pip install edge-tts   # fonts.py 另需 fontTools
npm install
.venv/bin/python tts.py          # → build/narration.wav, build/timeline.json, out/看见流星地上就有陨石吗.srt
node render.mjs --still 28 55    # 预览帧 → build/still_<t>.png
node render.mjs                  # → out/看见流星地上就有陨石吗.mp4
node cover.mjs                   # → out/封面-竖3比4.png, 封面-横4比3.png, 封面-横16比9.png（经 lib.mjs 读取 timeline.json）
```

`lib.mjs` 加载时读取 `build/timeline.json`，所以 `render.mjs` 与 `cover.mjs` 都要在 `tts.py` 之后运行。缓存已含当前文案 +13% 版本，**推断**可离线重建。

## 9. 参考来源

`script.json` 的 `sources` 字段（项目文件中提及，**本次未访问**）：

| 标题 | 发布方 | URL | 用途（推断） |
| --- | --- | --- | --- |
| Meteors and Meteorites | NASA | https://science.nasa.gov/solar-system/meteors-meteorites/ | 流星体 / 流星 / 陨石定义 |
| Meteors & Meteorites Facts | NASA | https://science.nasa.gov/solar-system/meteors-meteorites/facts/ | 每天落入地球物质量、发光机制、流星雨来源等数字 |
| Basics of Space Flight, Chapter 1-3 | NASA | https://science.nasa.gov/learn/basics-of-space-flight/chapter1-3/ | 太阳系小天体背景 |

无 `based_on`。相关本项目视频：[[geo-celestial-bodies]]（同一“石头三个名字”知识点作为其中一段）。

## 10. 证据与限制

| 文件 | 行数 | sha256[:12] | 读取程度 |
| --- | --- | --- | --- |
| `meteor-journey/script.json` | 212 | `31eb4ed212d2` | 全文 |
| `meteor-journey/tts.py` | 208 | `9184895dec79` | 与 geo-celestial-bodies 版 diff（仅 docstring、`TITLE`、`TARGET`、`LEAD_IN`、`TAIL`） |
| `meteor-journey/render.mjs` | 189 | `009a733eb404` | 全文 |
| `meteor-journey/lib.mjs` | 234 | `810fe70c5b85` | 与 geo-celestial-bodies 版 diff + `stamp` |
| `meteor-journey/cover.mjs` | 98 | `5bf9c9ab31b3` | 全文 |
| `meteor-journey/bodies.mjs` | 245 | `2d5f67f0c1c1` | 大纲 + `streak`/`airCap`/`person`/`ablate` |
| `meteor-journey/audio.mjs` | 229 | `9e3eeae951b5` | 与通用版 diff（新增 5 种音效） |
| `meteor-journey/scenes-a.mjs` … `scenes-d.mjs` | 146/158/139/146 | `d6e1030d3653` / `e3601e0e66b5` / `0cc5cc515a6f` / `122ec9230eb0` | 大纲、场景表、`truth`、`shower`、数字相关行 |
| `meteor-journey/package.json` | 17 | `eb0f5fb03b07` | 全文 |
| `meteor-journey/build/timeline.json` | — | — | 段落 start/end、rate、voices |
| `meteor-journey/build/tts_cache/` | 108 mp3 | — | 仅文件名哈希反查与修改时间 |
| `meteor-journey/out/*.mp4` | — | 15,791,339 字节 | 仅证据包 ffprobe |
| `meteor-journey/out/封面*.png` | 4 个 | 432–552 KB | 仅文件大小与时间，未查看画面 |

限制：无制作对话与需求原文；8 段旧文案内容、改写原因、预览过程均**未记录**；画面、封面未做视觉检查；工具版本与硬件未记录。
