---
type: video-production-dossier
project: geo-story-studio-opus
video_id: geo-celestial-bodies
title: 天体：宇宙里的“东西”
curriculum: script.json 未记录（画面角标“高中地理 · 必修一”；README 归为天体类型与天体系统）
captured: '2026-11-12'
status: 成片存在（横屏 1920×1080，118.4 s）；技术检查/视觉检查/用户验收未记录
source_fingerprint: 316f0f8940a112932fec24597ccc05ad510c26fb5eb22471f033a6c091d3c14e
tags:
- video-creation
- production-capture
- geography-teaching
- canvas-frame-render
- edge-tts
---

# 天体：宇宙里的“东西” · 横屏双角色问答 + 12 场景 Canvas 逐帧动画（逐词时间戳驱动）

<!-- capture-source-sha256:316f0f8940a112932fec24597ccc05ad510c26fb5eb22471f033a6c091d3c14e -->

本文是**根据项目文件做的材料回溯**（源码、`script.json`、`build/timeline.json`、`out/` 下的成片与字幕、证据包里的 ffprobe 与 sha256 清单）。证据包中 `host_session` 为空：**未发现制作会话**，制作对话、用户需求原文与修改过程均无法找回。本次未运行任何项目脚本（没有跑 `tts.py`、`render.mjs`、npm、pip），也没有打开视频或音频。

## 1. 当前结论与制作路线

**状态：成片存在**——`out/天体-宇宙里的东西.mp4` + 同名 `.srt`。只做了证据包里的 ffprobe 媒体头检查；视觉检查、试听、响度测量、用户验收和是否发布都**未记录**。

| 项 | 值 | 来源 |
| --- | --- | --- |
| 分辨率 / 画幅 | 1920×1080，16:9 横屏 | ffprobe；`lib.mjs:7` `W = 1920, H = 1080` |
| 帧率 / 帧数 | 30 fps；`Math.round(118.379 × 30)` = 3551 帧 | ffprobe；`render.mjs:157`（由时长推算） |
| 时长 | 118.368 s（timeline 118.379 s） | ffprobe；`build/timeline.json` |
| 视频编码 | H.264，yuv420p，CRF 18，preset medium，`+faststart` | ffprobe（codec）；`render.mjs:154-155`（参数） |
| 音频编码 | AAC 48 kHz 立体声，192 kbps | ffprobe；`render.mjs:155` |
| 文件大小 | 19,472,239 字节（约 19.5 MB） | 证据包 |
| 字幕 | `out/天体-宇宙里的东西.srt`，53 条，3222 字节；画面另有烧录字幕 | 文件；`render.mjs:57-82` |
| 其他版本 | 无（无竖屏版、封面或推荐版） | `out/` 列表 |

文件时间：TTS 产物（`narration.wav`、`timeline.json`、`.srt`）10-14 13:21，`music.wav`/`sfx.wav` 13:29，成片 13:30。git 只有初始提交 `4831a07`（2026-10-23）一条相关记录，单个视频的修改历史无法从 git 看出。

一句话管线：

> `script.json`（29 段，旁白“姐姐” + 提问的“小宇”）→ `tts.py` 调 edge-tts 逐句合成，按 WordBoundary 拿逐词时间 → `build/timeline.json` + `narration.wav`（adelay/amix/loudnorm）+ SRT → `render.mjs` 读取时间轴，用 3 个场景模块（12 个场景）按“词”触发动画，`audio.mjs` 程序合成配乐与音效 → 每帧 RGBA 经 stdin 管道送 ffmpeg → H.264/AAC MP4。

## 2. 教学目标与课标对齐

- **curriculum 字段：缺失**。`script.json` 只有 `voices` 和 `segments` 两个键，没有 `title`、`curriculum`、`source`/`numbers`（与 AGENTS.md 第 6 节要求不符，见 §7）。画面左上角角标写“高中地理 · 必修一 / 天体”（`render.mjs:45-54`）；README 目录行把它归为“天体类型；判断天体的三个要点；天体系统的层次”。对应教材章节为**推断**：人教版必修第一册第一章“宇宙中的地球”第一节（地球的宇宙环境）——项目文件中没有写明。
- **核心知识点**：天体的类型与判断方法（在太空、有实体、独立运行），天体系统的层次。严格说这是两个相关知识点放在一片里，以“天体”为主线、“天体系统”收尾。
- **讲解结构（按实际文案）**：
  1. 导入：小宇问“天上亮晶晶的那些，都叫天体吗？”（`hook`），给出通俗定义“宇宙里的东西”（`def`）。
  2. 原理拆解：恒星、行星、卫星、彗星、星云逐个介绍，再补流星体、星际物质、空间站（`star`…`more`）。
  3. 结论 / 规律：“问三句”判断法（`rule`、`q1`–`q3`、`ruleEnd`）。
  4. 迁移应用：月球、着陆的嫦娥四号、太阳黑子三例判断（`quizMoon`…`spot`）；同一块石头的三个名字——流星体 / 流星 / 陨石（`rock`…`rock3`，文案明说“考试最爱考的”）。
  5. 拓展：天体系统四级——地月系 → 太阳系 → 银河系 → 可观测宇宙（`system`、`sys1`–`sys4`）。
  6. 总结：“在太空、有实体、独立运行，就是天体！”（`outro`、`kidEnd`）。
- **科学边界**（文案里已有的限定）：嫦娥四号“停在月面，成了月球的一部分，不再独立运行，不算”；太阳黑子“是太阳表面的一块区域，也不算”；流星“不是天体”、陨石“也不是天体”。这些属于教材常见判断口径。
- **画面数字**（`scenes-c.mjs:63-68`）：地月距离 ≈ 38.4 万千米、日地距离 ≈ 1.5 亿千米、银河系直径 ≈ 10 万光年；旁白“银河系几千亿颗恒星”。**出处在项目文件中均未注明**，本次未核对。

## 3. 技术与依赖

| 类别 | 内容 | 证据 |
| --- | --- | --- |
| 渲染 | Node.js ESM + `@napi-rs/canvas` `^1.0.11`（`package.json` 声明；lock 文件已存在，具体解析版本本次未展开） | `package.json`；`render.mjs:4` |
| 包配置 | `package.json` 写 `"type": "commonjs"`，但脚本都是 `.mjs`，按 ESM 加载；`scripts` 只有 npm 默认 `test` 占位 | `package.json` |
| TTS | Python `edge-tts` + `aiohttp`；版本未记录（无 requirements 文件） | `tts.py:15-17` |
| 字体 | `fonts.py` 用 `fontTools` 从 macOS 系统字体集合抽出圆体-简粗体、苹方-简中粗到 `build/fonts/`；`lib.mjs` 在缺字体时自动调用 | `fonts.py:1-16`；`lib.mjs:11-20` |
| 编码 / 混音 | 系统 ffmpeg：旁白混音（`tts.py`）、成片编码与总混音（`render.mjs`）；版本未记录 | 代码 |

声线（`script.json:2-5`）：

| 角色 | voice | rate | pitch | 段数 |
| --- | --- | --- | --- | --- |
| `zh` 旁白（文案中称“姐姐”） | `zh-CN-XiaoyiNeural` | +0% | +0 Hz | 25 |
| `kid` 提问的小宇（字幕标签“小宇”） | `zh-CN-YunxiaNeural` | +4% | +6 Hz | 4 |

`timeline.json` 记录 `rate: 0`，即没有为压时长而整体提速。

## 4. 镜头与时间轴

时间来自 `build/timeline.json`（段落 start），场景边界按各模块 `from/to` 公式（多为下一段开始前 0.35–0.45 s 切换，交叉淡化 `XF = 0.35`）。

| # | 场景 id | 约时间 (s) | 口播要点 | 画面 | 模块 |
| --- | --- | --- | --- | --- | --- |
| 1 | `intro` | 0–9.2 | 小宇提问 → “天体就是宇宙里的东西” | 宇航员小宇指向闪烁星点，太阳/地球/月球/彗星/空间站/石块组成环 | `scenes-a.mjs:43-83` |
| 2 | `star` | 9.2–13.2 | 恒星自己发光发热 | 右侧信息卡 `infoCard`，标签按词弹出 | `scenes-a.mjs:85-92` |
| 3 | `planet` | 13.2–18.7 | 行星绕恒星转、反射阳光 | 同上 + 地球 | `scenes-a.mjs:94-118` |
| 4 | `moon` | 18.7–22.5 | 卫星绕行星，月球 | 同上 | `scenes-a.mjs:120-131` |
| 5 | `comet` | 22.5–27.4 | 脏雪球、长尾巴 | 彗尾背向太阳 | `scenes-a.mjs:133-146` |
| 6 | `nebula` | 27.4–32.1 | 气体尘埃，恒星出生地 | 星云 | `scenes-a.mjs:148-166` |
| 7 | `more` | 32.1–40.2 | 流星体、星际物质、空间站也是天体 | 三张卡片 + 对勾 | `scenes-a.mjs:168-197` |
| 8 | `rules` | 40.2–55.4 | 三问判断法 | 三问清单 + 小图标 | `scenes-b.mjs:33-66` |
| 9 | `quiz` | 55.4–72.7 | 月球 / 嫦娥四号 / 太阳黑子 | 主体从右滑入，清单逐条打勾或打叉，再给结论 | `scenes-b.mjs:67-163` |
| 10 | `rock` | 72.7–88.0 | 流星体 → 流星 → 陨石 | 太空 / 大气 / 地面三层，石块下落发热，三个名牌 | `scenes-c.mjs:5-61` |
| 11 | `system` | 88.0–109.4 | 天体抱团 → 四级系统 | 地月互相吸引后绕转，再逐级缩放；右侧层级阶梯 | `scenes-c.mjs:62-169` |
| 12 | `outro` | 109.4–118.4 | 总结口诀 | 三张关键词卡 “+” 连成“= 天体！”，彩纸 | `scenes-c.mjs:170-196` |

## 5. 关键算法与实现

> 区分：视频**讲解的知识**是天体的判断与天体系统层次；本节是**制作所用的算法**——Canvas 2D 卡通绘图和 TTS 时间轴驱动，不是天体力学模拟（轨道是椭圆投影 + 匀角速度，非开普勒运动）。

### 5.1 逐词时间戳 → 动画锚点（`lib.mjs:30-40`）

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
export const at = (id, w, nth) => word(id, w, nth).start;
```

`tts.py` 在合成时把每个 WordBoundary 的文字在原句中的字符位置 `idx` 记下来（`tts.py:70-84`）；场景里写 `at('change', '不再')` 就能拿到“不再”开口的时刻，找不到词会直接抛错。三问清单的打勾 / 打叉（`scenes-b.mjs:69-76`）、信息卡标签（`scenes-a.mjs:18`）、系统阶梯（`scenes-c.mjs:159`）、音效（`eventsA/B/C`）都用这个锚点，**没有按字数估算**。

### 5.2 TTS：缓存键、429 重试、端点替换（`tts.py:19-66`）

- 端点替换：`edge_tts.communicate.WSS_URL` 改为 `api.msedgeservices.com`，注释说明旧主机自 2026 年 10 月返回 403（`tts.py:19-25`）。
- 缓存键：`sha1(f"{voice}|{rate}|{pitch}|{text}")[:16]`，命中则直接复制 mp3、读 json（`tts.py:46-50`）。`build/tts_cache/` 现有 58 个文件 = 29 段 × (mp3 + json)。
- 429：最多 5 次，等待 8/16/24/32 s 递增（`tts.py:52-62`）。
- 时长控制：`TARGET = 130.0`（`tts.py:32`），只有总长超出才按比例一次算出提速量；本片首轮 118.4 s 未超，`rate = 0`。
- 段落排布：以每段**末词结束 + 0.12 s** 而非 mp3 长度计（注释：mp3 尾部约 0.6 s 静音），再加每段 `gap`（`tts.py:152-154, 160-168`）。

### 5.3 场景切换与镜头震动（`render.mjs:88-127`）

每个场景是 `{id, from, to, draw}`，渲染时对 `[from, to + XF]` 内的场景做 alpha 交叉淡化，进入时从 0.94 放大到 1、离开时从 1 放大到 1.05；音效事件若带 `shake`（如陨石落地 `slam` `shake: 18`，`scenes-c.mjs:209`），则 0.4 s 内按 `(1-p)²` 衰减的正余弦偏移整幅画面。开头 0.4 s 由深色淡入、结尾 0.6 s 淡出。

### 5.4 天体系统“逐级缩小”（`scenes-c.mjs:145-155`）

```js
LEVELS.forEach((lv, i) => {
  const tin = i === 0 ? tL0 : S(lv.id) - 0.45;
  const tout = i < 3 ? S(LEVELS[i + 1].id) - 0.45 : 1e9;
  if (t < tin) return;
  const enter = easeOut(prog(t, tin, tin + 0.7));
  const leave = easeIO(prog(t, tout, tout + 0.8));
  const sc = (i === 0 ? lerp(0.6, 1, enter) : lerp(3.2, 1, enter)) * lerp(1, 0.12, leave);
  ...
});
```

当前层级缩到 0.12 倍淡出，下一层从 3.2 倍缩入，形成“镜头不断拉远”的效果；右侧四级阶梯（地月系→可观测宇宙）在说到对应词时弹出并高亮当前级。太阳系层用 8 条椭圆轨道（`rx = 120 + 52k`，`ry = 0.42 rx`），角速度随序号递减，地球加金色圈；**轨道比例不是真实比例**（画面上未声明，文案也未涉及比例）。

### 5.5 同一块石头的三个名字（`scenes-c.mjs:9-15`）

石块位置按两个词区间分段插值：`冲 → 道光` 之间沿二次曲线加速冲入大气层并逐渐变红（`heat`），`落 → 地面` 之间落地；落地时触发 `slam` 音效和画面震动，再弹出“陨石 ✗ 不是天体”名牌。

### 5.6 程序合成音频（`audio.mjs`）

全部声音在代码中合成，没有外部音乐素材：配乐为 112 BPM、C–G–Am–F 和弦的 Karplus–Strong 拨弦琶音 + 根音 + 沙锤噪声 + 隔小节钟琴，随机种子 42（`audio.mjs:149-185`）；音效 15 种（`pop`、`whoosh`、`slam`、`buzz`、`tada`、`confetti` 等），由 RBJ 带通 biquad 噪声、正弦扫频、FM 钟声组合（`audio.mjs:19-140`）。

## 6. 声音与字幕

| 项 | 内容 | 来源 |
| --- | --- | --- |
| 旁白 | `zh-CN-XiaoyiNeural` +0%，25 段 | `script.json`；`timeline.json` |
| 提问角色 | `zh-CN-YunxiaNeural` +4% / +6 Hz，4 段（hook、kidQ、kidChange、kidEnd） | 同上 |
| 留白 | `LEAD_IN 1.2`（让标题动画先出来）、默认 `GAP 0.45`、各段 `gap` 0.35–1.0、`TAIL 2.6`（“room for the closing confetti”） | `tts.py:33-35`；`script.json` |
| 旁白混音 | 每段 `aresample=48000,adelay` 定位 → `amix normalize=0` → `highpass=f=80` → `loudnorm=I=-16:TP=-1.5:LRA=11` → `build/narration.wav` | `tts.py:176-190` |
| 总混音 | 配乐 ×0.8 经旁白侧链压缩（threshold 0.05、ratio 5、attack 20、release 400）；音效 ×0.5；三路 `amix` 后 `alimiter=limit=0.93` | `render.mjs:149-152` |
| 字幕切分 | 按中文标点切，每条 ≤14 字；长句用 `captions` 手工断句（`more`、`outro`）；每条起止取自所含词的时间戳 | `tts.py:106-136`；`script.json` |
| SRT | 53 条；首条 `00:00:01,248 → 00:00:03,460 姐姐，天上亮晶晶的那些`，末条 `00:01:54,865 → 00:01:55,659 我记住啦！` | `out/*.srt` |
| 烧录字幕 | y = 952 的深色胶囊，52 px；小宇台词前加天蓝“小宇”标签、字色浅蓝 | `render.mjs:57-82` |

本次未试听、未测成片响度。

## 7. 迭代与问题

| 事项 | 证据 | 说明 |
| --- | --- | --- |
| 未放账号 logo | `render.mjs:45-54` 只有文字角标“高中地理 · 必修一 / 天体”；目录无 `assets/` | 与 AGENTS.md 第 4 节“地理视频左上角必须打 logo”不符。**推断**本片早于该规则（规则以 [[moonlight-origin]] 为参考实现） |
| 无片尾品牌卡 | `scenes-c.mjs:173-196` 以“= 天体！”+ 彩纸收尾；`TAIL` 注释为 confetti | 与第 4 节“片尾品牌卡（必需）”不符 |
| `script.json` 缺 `curriculum`、`title`、`source`/`numbers` | `script.json` 仅两个键 | 与第 6 节不符；画面上的距离、尺度数字无出处 |
| `TARGET = 130.0` 而非 `None` | `tts.py:32` | 与第 2 节“默认 `TARGET = None`”不符；但实际未触发提速（`rate: 0`），对成片无影响 |
| 旁白声音用 Xiaoyi | `script.json:3` | 第 2 节默认 Xiaoxiao、允许换 Xiaoyi，属允许范围 |
| 一片讲两个点 | §2 结构 | 天体判断 + 天体系统层次；第 6 节要求“一个视频只讲透一个核心知识点”，是否算超范围属判断问题 |
| `package.json` `"type": "commonjs"` | `package.json` | 与 `.mjs` 不冲突，仅为 npm init 默认值 |

没有备用版本、代码中的 TODO 或被注释掉的旧实现；修改过程**未记录**。README 已收录本片（第 15 行，横屏 1:58）。AGENTS.md 第 2 节把本片 `tts.py` 列为多角色配音参考。

## 8. 复现入口

以下命令来自 AGENTS.md“常用命令”和 `render.mjs:1-3`、`tts.py:4` 注释，**本次均未执行**：

```bash
cd geo-celestial-bodies
python3 -m venv .venv && .venv/bin/pip install edge-tts   # fonts.py 另需 fontTools；版本未记录
npm install                                              # @napi-rs/canvas
.venv/bin/python tts.py            # → build/seg*.mp3, build/narration.wav, build/timeline.json, out/天体-宇宙里的东西.srt
node render.mjs --still 8.0 60     # → build/still_<t>.png 预览（看完应删）
node render.mjs                    # → build/music.wav, build/sfx.wav, out/天体-宇宙里的东西.mp4
```

顺序：必须先跑 `tts.py`，因为 `lib.mjs:24` 在加载时就读取 `build/timeline.json`。`build/tts_cache/` 保留了全部 29 段缓存，**推断**重跑 TTS 不需要联网（未验证）。系统依赖：ffmpeg（libx264、aac）、macOS 系统字体。

## 9. 参考来源

- 项目文件中**没有任何 URL**，`script.json` 也没有 `source` 字段。
- 无 `based_on`，没有发现源自 geo-story-studio build 的痕迹。
- 相关的本项目视频：[[star-planet-moon]]（恒星/行星/卫星区分）、[[cosmic-address]]（天体系统层次）、[[meteor-journey]]（流星体/流星/陨石）。三者与本片共用同一份 `bodies.mjs` 或同类素材（见各自档案）。

## 10. 证据与限制

| 文件 | 行数 | sha256[:12] | 读取程度 |
| --- | --- | --- | --- |
| `geo-celestial-bodies/script.json` | 38 | `0f3818759b7d` | 全文 |
| `geo-celestial-bodies/tts.py` | 208 | `097b3775a675` | 全文 |
| `geo-celestial-bodies/render.mjs` | 168 | `f0817e6a356e` | 全文 |
| `geo-celestial-bodies/lib.mjs` | 210 | `0c8b267f5545` | 全文 |
| `geo-celestial-bodies/audio.mjs` | 204 | `f9e04b2cfd54` | 全文（与 star-planet-moon、moonlight-origin、cosmic-address 哈希相同） |
| `geo-celestial-bodies/bodies.mjs` | 273 | `18431acd3a06` | 函数大纲 + 光照 / 地球 / 彗星 / 小宇开头（与 star-planet-moon、cosmic-address 哈希相同） |
| `geo-celestial-bodies/scenes-a.mjs` | 233 | `0c5c06f2a748` | 大纲、`infoCard`、intro 开头、`more`、场景表 |
| `geo-celestial-bodies/scenes-b.mjs` | 184 | `9e660e589d8d` | 大纲、QUIZ 定义、`quizScene` 开头 |
| `geo-celestial-bodies/scenes-c.mjs` | 223 | `08e8609c4c60` | rock 开头、system 全文、outro、events |
| `geo-celestial-bodies/fonts.py` | 37 | `a6ac7264e548` | 开头（全项目同一份） |
| `geo-celestial-bodies/package.json` | 17 | `65b576e2988f` | 全文；`package-lock.json`（`5cb2167043d3`）未展开 |
| `geo-celestial-bodies/build/timeline.json` | — | — | 段落 id / voice / start / end / cue 数 |
| `geo-celestial-bodies/out/天体-宇宙里的东西.srt` | 53 条 | — | 首尾条目 |
| `geo-celestial-bodies/out/天体-宇宙里的东西.mp4` | — | 19,472,239 字节 | 仅证据包 ffprobe |

限制：

- 无制作会话、无 prompt / 计划文件：选题来源、为何横屏、预览与修改过程**未记录**。
- 没有打开视频与音频，画面效果（包括轨道、地图性质的图示是否规范）未做视觉检查；时间轴表是按代码公式和 `timeline.json` 推算的。
- Python 包、Node、ffmpeg 版本与硬件均未记录；`build/still_*.png` 预览图不存在（已清理或未生成）。
