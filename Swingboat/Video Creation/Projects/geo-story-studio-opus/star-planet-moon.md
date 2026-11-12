---
type: video-production-dossier
project: geo-story-studio-opus
video_id: star-planet-moon
title: 恒星、行星、卫星：宇宙身份登记处
curriculum: script.json 未记录（README：区分太阳恒星、地球行星、月球天然卫星）
captured: '2026-11-12'
status: 成片存在（竖屏 1080×1920，97.3 s）；技术检查/视觉检查/用户验收未记录
source_fingerprint: 1ba6192e336a11e3f0bfaeff32242c9ddc5f4a400bb0360dc3e2b2768615b20f
tags:
- video-creation
- production-capture
- geography-teaching
- canvas-frame-render
- edge-tts
---

# 恒星、行星、卫星：宇宙身份登记处 · 五声线拟人问答 + “两项检查”盖章（竖屏 8 场景）

<!-- capture-source-sha256:1ba6192e336a11e3f0bfaeff32242c9ddc5f4a400bb0360dc3e2b2768615b20f -->

本文是**根据项目文件做的材料回溯**（源码、`script.json`、`build/timeline.json`、`build/tts_cache` 文件名、`out/` 清单、证据包 ffprobe 与 sha256）。证据包 `host_session` 为空：**未发现制作会话**。本次未运行任何项目脚本，未打开视频或音频。标题取自 README 与 `render.mjs` 的 `TITLE`（`script.json` 没有 `title` 字段）。

## 1. 当前结论与制作路线

**状态：成片存在**——`out/恒星行星卫星-宇宙身份登记处.mp4` + 同名 `.srt`。只有 ffprobe 媒体头检查，视觉检查、试听与用户验收**未记录**。

| 项 | 值 | 来源 |
| --- | --- | --- |
| 分辨率 / 画幅 | 1080×1920，9:16 竖屏 | ffprobe；`lib.mjs:7` |
| 帧率 / 帧数 | 30 fps；`round(97.303 × 30)` = 2919 帧 | ffprobe；`render.mjs`（推算） |
| 时长 | 97.300 s（timeline 97.303 s） | ffprobe；`timeline.json` |
| 视频 | H.264，yuv420p，CRF 19，preset medium，faststart | ffprobe；`render.mjs` |
| 音频 | AAC 48 kHz 立体声 192 kbps | ffprobe；`render.mjs` |
| 文件 | 22,059,369 字节（约 22.1 MB） | 证据包 |
| 字幕 | `.srt` 48 条 + 画面烧录 | 文件；`render.mjs` `subtitles()` |
| 其他版本 | 无 | `out/` |

文件时间：字体抽取 10-17 22:16，TTS 产物 22:18，配乐音效 22:28，成片 22:29。

一句话管线：

> `script.json`（30 段，5 个声线：旁白 + 太阳 / 地球 / 月球 / 人造卫星）→ `tts.py`（`TARGET = 98`，从脚本预设 +6% 自动提到 +13%）→ 逐词时间轴 + 混音 + SRT → `render.mjs`：顶部“宇宙身份登记处”招牌 + 两项检查芯片常驻，4 个场景模块共 8 个场景，`props.mjs` 提供身份证、印章、灯泡等道具 → ffmpeg 管道编码。

代码来源：`lib.mjs` 与 [[geo-celestial-bodies]] 版只差 1 行（`W/H` 对调为竖屏），`bodies.mjs`、`audio.mjs`、`fonts.py` 与之哈希相同；`tts.py` 只改 `TITLE`、`TARGET`、`LEAD_IN`、`TAIL`。**推断**本片由横屏的 geo-celestial-bodies 复制改成竖屏。

## 2. 教学目标与课标对齐

- **curriculum 字段：缺失**；`script.json` 只有 `rate`、`voices`、`segments`，也没有 `source`/`numbers`。对应人教版必修第一册第一章第一节“地球的宇宙环境”属**推断**。
- **核心知识点**：用两个判据区分三类天体——①光从哪来（自身发光 / 反射）；②谁绕谁转（绕恒星 / 绕行星）。太阳 = 恒星，地球 = 行星，月球 = 天然卫星。
- **讲解结构（按实际文案）**：
  1. 导入：三张熟脸“身份完全不一样”，登记处开张，只查两件事（`hook`、`open`、`rules`）。
  2. 原理拆解：太阳自述“高温等离子体、核心核聚变”→ 盖章恒星；地球“绕太阳转一年一圈”、可见光是反射的，但红外下自己也有热辐射 → 盖章行星；月球自称恒星 → 假设实验“关掉照向你的太阳光”→ 变暗 → 月光主要是反射太阳光，绕地球 → 盖章天然卫星（`sunHi`…`moonStamp`）。
  3. 边界：人造卫星来插队——“你是人造卫星，先排队。今天说的是天然卫星”（`satHi`、`satNo`）。
  4. 结论 + 总结：三者并排，“先看光从哪来，再看谁绕谁转”（`sum`、`sumRule`）。
  5. 迁移 / 检测：选择题“月球这么亮属于哪一类”，2.6 s 停顿倒计时后揭晓 C（`quiz`、`answer`、`moonEnd`、`cta`）。
- **科学限定**（文案已有）：地球“看得见的光是反射太阳的”+“换上红外眼镜，地球自己也在发出热辐射”，避免“行星完全不发光”的绝对化；月光用“主要是”限定；用“天然卫星”与人造卫星区分。
- **数字与出处**：文案中无具体数值；无 `sources`，**出处未记录**。

## 3. 技术与依赖

| 类别 | 内容 | 证据 |
| --- | --- | --- |
| 渲染 | Node ESM + `@napi-rs/canvas` `^1.0.11`（`scripts` 仅默认 `test`） | `package.json` |
| TTS | Python `edge-tts` + `aiohttp`，版本未记录 | `tts.py` |
| 字体 | `fonts.py`（fontTools），圆体 / 苹方 | `lib.mjs:11-20` |
| 编码 | 系统 ffmpeg，版本未记录 | `render.mjs` |

声线（`script.json:3-9`；`timeline.json` 的全局 `rate: 13` 取代脚本预设的 `"rate": 6`，各角色在其上叠加自身 rate）：

| 角色 | voice | 角色 rate | 实际 rate | pitch | 段数 |
| --- | --- | --- | --- | --- | --- |
| `zh` 旁白 / 登记员 | `zh-CN-XiaoxiaoNeural` | 0 | +13% | 0 | 19 |
| `sun` 太阳 | `zh-CN-YunjianNeural` | +4 | +17% | −2 Hz | 2 |
| `earth` 地球 | `zh-CN-YunxiNeural` | +6 | +19% | +2 Hz | 4 |
| `moon` 月球 | `zh-CN-XiaoyiNeural` | +4 | +17% | +6 Hz | 4 |
| `sat` 人造卫星 | `zh-CN-YunxiaNeural` | +12 | +25% | +10 Hz | 1 |

AGENTS.md 第 2 节把本片 `tts.py` 列为“多个角色”配音参考。

## 4. 镜头与时间轴

时间来自 `timeline.json` 与场景 `from/to`（均在下一段开始前 0.35 s 切换，`XF = 0.35`；缩放中心改为 `(W/2, 800)`）。

| # | 场景 id | 约时间 (s) | 口播要点 | 画面 | 模块 |
| --- | --- | --- | --- | --- | --- |
| 1 | `hook` | 0–6.1 | 三张熟脸身份不同 | 太阳、地球、月球带表情登场 | `scenes-a.mjs:7-49` |
| 2 | `office` | 6.1–14.2 | 登记处开张，两项检查 | 悬挂招牌、两枚检查芯片出现 | `scenes-a.mjs:51-80` |
| 3 | `sun` | 14.2–27.6 | 自己造光，核聚变 → 恒星 | 太阳在柜台前，舞台上移缩小、身份证从下方升起，红色“恒星”章 | `scenes-b.mjs:21-73` |
| 4 | `earth` | 27.6–46.7 | 绕日一年；反射光 + 红外热辐射 → 行星 | 地球绕日、红外眼镜下的热辐射波纹，蓝色“行星”章 | `scenes-b.mjs:75-160` |
| 5 | `moonLight` | 46.7–59.2 | 月球自称恒星 → 关光实验 | 太阳光束照月；拨开关后光束消失、月球变暗变沮丧 | `scenes-c.mjs:11-86` |
| 6 | `moonOrbit` | 59.2–72.7 | 绕地球 → 天然卫星；人造卫星插队 | 月绕地轨道、紫色“天然卫星”章、人造卫星客串 | `scenes-c.mjs:88-125` |
| 7 | `summary` | 72.7–81.5 | 三者并排记 | 三行名称 + 类别印章 | `scenes-d.mjs:13-40` |
| 8 | `quiz` | 81.5–97.3 | 考考你 → C | “考考你”、A/B/C 选项、3 拍倒计时、答案揭晓彩纸、月球戴墨镜 | `scenes-d.mjs:44-89` |

常驻层（`render.mjs`）：`header()` 在“宇宙…开张”时挂出招牌，两枚芯片“1. 光从哪来？”“2. 谁绕谁转？”在对应问答期间（`LIGHT_ON`/`ORBIT_ON` 段落区间）变金色并轻微脉动，总结前淡出；字幕在 `SUB_Y = 1480`；`footer()` 在 y = 1572 放一行声明（见 §7）。

## 5. 关键算法与实现

> 区分：视频**讲解的知识**是“光源 + 绕转关系”两项判据；本节是**制作所用的算法**——拟人化卡通与道具绘制、TTS 时间轴驱动，轨道与光束均为示意几何。

### 5.1 段落区间驱动的常驻检查芯片（`render.mjs:47-71`）

```js
const LIGHT_ON = [['sunQ', 'sunA'], ['earthL', 'earthIR2'], ['moonHi', 'moonLight']];
const ORBIT_ON = [['earthQ', 'earthA'], ['moonQ', 'moonA']];
const during = (t, list) => list.some(([a, b]) => t >= S(a) - 0.2 && t <= E(b) + 0.3);
```

两枚芯片在讲哪项判据时就“点亮”（金底、放大 1.08 倍并以 8 rad/s 脉动），其余时间半透明。区间直接用段落 id 的开始 / 结束时刻，所以改文案后高亮自动跟着走。

### 5.2 “舞台上移 + 身份证升起”转场（`scenes-b.mjs:8-19`）

```js
export function withCard(ctx, t, tIn, stage, cardOpts) {
  const k = easeIO(prog(t, tIn - 0.2, tIn + 0.5));
  ctx.save();
  ctx.translate(540, lerp(820, 680, k)); ctx.scale(lerp(1, 0.6, k), lerp(1, 0.6, k)); ctx.translate(-540, -820);
  stage();
  ctx.restore();
  if (k <= 0) return;
  const y = lerp(1700, 1140, easeOut(prog(t, tIn - 0.1, tIn + 0.5)));
  idCard(ctx, t, 540, y, 1.0, cardOpts);
}
```

太阳、地球、月球三段共用：角色舞台缩到 0.6 倍上移，`props.mjs` 的 `idCard` 从画面下方升起，再由 `stamp()` 从 2.6 倍砸到 1 倍盖上“恒星 / 行星 / 天然卫星”章（三色定义在 `props.mjs:6`），章面加 18 个伪随机墨点模拟真实印章（`props.mjs:9-31`）。

### 5.3 “关掉太阳光”的假设实验（`scenes-c.mjs:11-40`）

光束是从太阳到月球、宽度随距离张开的梯形渐变；`off` 在“关掉”一词处 0.3 s 内升到 1，在“月光”前再降回；`beam = 1 - off` 同时控制光束透明度、月球外发光和表情（`off > 0.5` 时 `mood: 'sad'`）。月球的明暗面由通用 `moon()` 的 `light` 参数指向太阳方向（`ang + π`）。这是把“月球不发光”做成可观察的反事实对照。

### 5.4 考题倒计时自适应停顿（`scenes-d.mjs:95`）

```js
const cdStart = E('quiz') + 0.2, cdStep = Math.max(0.5, (at('answer', 'C') - 0.3 - cdStart) / 3);
```

`quiz` 段在 `script.json` 中设 `gap: 2.6`，留出作答时间；倒计时 3 声 `tick` 音效按这段空白平均分配，揭晓“C”时 `tada` + 彩纸。

### 5.5 程序合成音频

`audio.mjs` 与 [[geo-celestial-bodies]] 相同（112 BPM Karplus–Strong 拨弦 bed + 15 种音效）；本片配乐音量从 0.8 降到 **0.7**（`render.mjs` filter_complex）。

## 6. 声音与字幕

| 项 | 内容 | 来源 |
| --- | --- | --- |
| 留白 | `LEAD_IN 1.0`、默认 `GAP 0.45`、`TAIL 2.4`；`quiz` 后 `gap 2.6` 作答时间 | `tts.py:33-35`；`script.json` |
| 自动提速 | `TARGET = 98.0`；缓存中 30 段都有全局 +6 与 +13 两版，**无原速版** → 从脚本预设 +6% 起跑，一次跳到 +13% 达标 | `tts.py:32`；缓存哈希反查 |
| 旁白混音 | `adelay` → `amix` → `highpass 80` → `loudnorm I=-16:TP=-1.5:LRA=11` | `tts.py` |
| 总混音 | 配乐 ×0.7 侧链压缩，音效 ×0.5，`alimiter 0.93` | `render.mjs` |
| SRT | 48 条 | `out/*.srt` |
| 烧录字幕 | `SUB_Y = 1480` 深色胶囊；字号从 56 起每次减 2 直到整行 ≤ `W − 60`；角色台词前加彩色名牌（太阳金、地球天蓝、月球银白、人造卫星黄），字色暖白 | `render.mjs` `SPEAKER` 与 `subtitles()` |

## 7. 迭代与问题

| 事项 | 证据 | 说明 |
| --- | --- | --- |
| 画面有“示意”类声明 | `render.mjs:106` `'科普示意 · 大小、距离不按比例 · 卫星指天然卫星'`（y = 1572） | 与 AGENTS.md 第 5 节冲突（该节说明已做好的视频不必回改）；其中“卫星指天然卫星”是内容限定，可改写进画面内容；场景内另有“剖面示意”（`scenes-b.mjs:57`）、“假设实验 · 示意”（`scenes-c.mjs:64`）标签 |
| 无账号 logo、无片尾品牌卡 | `render.mjs` 无 logo；无 `assets/`；以“评论区扣个C！”收尾 | 与第 4 节不符；**推断**早于该规则 |
| 结尾号召“答对的，评论区扣个C！” | `script.json` `cta` | 第 6 节要求收尾不做成广告式口号，属判断问题 |
| 为压时长提速到 +13% | `tts.py:32`；`timeline.json` | 与第 2 节“`TARGET = None`、不为压时长提速”不符；**推断**该规则晚于本片（git `35ad4da`） |
| 缺 `curriculum`、`title`、`source` | `script.json` | 与第 6 节不符 |
| 横屏版代码直接改竖屏 | `lib.mjs` 仅 1 行 diff；`bodies.mjs`/`audio.mjs` 同哈希 | 复用模式，非问题 |

README 已收录（第 16 行，竖屏 1:37）。没有 TODO、备用版本或注释掉的旧实现。

## 8. 复现入口

命令来自 AGENTS.md“常用命令”与 `render.mjs:2-3` 注释，**本次均未执行**：

```bash
cd star-planet-moon
python3 -m venv .venv && .venv/bin/pip install edge-tts   # fonts.py 另需 fontTools
npm install
.venv/bin/python tts.py            # → build/narration.wav, build/timeline.json, out/恒星行星卫星-宇宙身份登记处.srt
node render.mjs --still 20 52 85   # 预览帧 → build/still_<t>.png
node render.mjs                    # → out/恒星行星卫星-宇宙身份登记处.mp4
```

`lib.mjs` 加载即读 `build/timeline.json`，须先跑 TTS；缓存含 +13% 全部 30 段，**推断**可离线重建。

## 9. 参考来源

- 项目文件中**没有 URL**，`script.json` 无 `source` 字段；无 `based_on`。
- 相关本项目视频：[[geo-celestial-bodies]]（代码母版、天体类型）、[[moonlight-origin]]（月光来源单独成片）、[[cosmic-address]]（共用 `bodies.mjs`，地球 / 月球声线相同）。

## 10. 证据与限制

| 文件 | 行数 | sha256[:12] | 读取程度 |
| --- | --- | --- | --- |
| `star-planet-moon/script.json` | 43 | `8bc36db72372` | 全文 |
| `star-planet-moon/tts.py` | 208 | `29b6a4d17bf9` | 与 geo-celestial-bodies 版 diff |
| `star-planet-moon/render.mjs` | 192 | `3a35a4771fbf` | 与 geo-celestial-bodies 版全文 diff |
| `star-planet-moon/lib.mjs` | 210 | `a4bdc6ce1a90` | diff（仅 `W/H`） |
| `star-planet-moon/props.mjs` | 186 | `27e95e6225d9` | 大纲 + `stamp` |
| `star-planet-moon/bodies.mjs` / `audio.mjs` / `fonts.py` | 273 / 204 / 37 | `18431acd3a06` / `f9e04b2cfd54` / `a6ac7264e548` | 与 geo-celestial-bodies 同哈希 |
| `star-planet-moon/scenes-a.mjs` … `scenes-d.mjs` | 99/186/149/111 | `6628333e1d52` / `7845781fd717` / `52b9047af6c8` / `a3b5f8e498d3` | 大纲、场景表、`withCard`、`moonLightScene` 开头、quiz 与 events |
| `star-planet-moon/package.json` | 17 | `0410c4a9d206` | 证据包摘要 |
| `star-planet-moon/build/timeline.json` | — | — | 段落 start/end、rate |
| `star-planet-moon/build/tts_cache/` | 60 mp3 | — | 文件名哈希反查 |
| `star-planet-moon/out/*.mp4` | — | 22,059,369 字节 | 仅证据包 ffprobe |

限制：无制作对话；为何从横屏母版改出竖屏、角色声线如何选定、预览与修改过程均**未记录**；未做视觉检查与试听；工具版本未记录。
