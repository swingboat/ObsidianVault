---
type: video-production-dossier
project: geo-story-studio-opus
video_id: moonlight-origin
title: 月亮那么亮，它的光从哪里来？
curriculum: script.json 未记录（README：月球自身不发光，月光是月面反射的太阳光）
captured: '2026-11-12'
status: 成片存在（竖屏 1080×1920，61.3 s，含角标 logo 与片尾品牌卡）；技术检查/视觉检查/用户验收未记录
source_fingerprint: 4e432879cb9d1a2112b9476927e8f19ab6ab98da34a4c31c6922c68766b341cb
tags:
- video-creation
- production-capture
- geography-teaching
- canvas-frame-render
- edge-tts
---

# 月亮那么亮，它的光从哪里来？ · 三步光路 + 台灯皮球类比，带角标 logo 与片尾品牌卡的竖屏短片

<!-- capture-source-sha256:4e432879cb9d1a2112b9476927e8f19ab6ab98da34a4c31c6922c68766b341cb -->

本文是**根据项目文件做的材料回溯**（源码、`script.json`、`build/timeline.json`、`build/tts_cache` 文件名、`out/` 与 `assets/` 清单、证据包 ffprobe 与 sha256）。证据包 `host_session` 为空：**未发现制作会话**。本次未运行任何项目脚本，未打开视频、音频或 logo 图片。

## 1. 当前结论与制作路线

**状态：成片存在**——`out/月亮的光从哪里来.mp4` + 同名 `.srt`。只有 ffprobe 媒体头检查；画面、品牌卡的视觉检查与用户验收**未记录**。AGENTS.md 第 4 节把本片的 `brand()` 与 `LOGO_BOX` 指定为 logo 实现参考。

| 项 | 值 | 来源 |
| --- | --- | --- |
| 分辨率 / 画幅 | 1080×1920，9:16 竖屏 | ffprobe；`lib.mjs:7` |
| 帧率 / 帧数 | 30 fps；`round(61.319 × 30)` = 1840 帧 | ffprobe；`render.mjs:163`（推算） |
| 时长 | 61.333 s（timeline 61.319 s） | ffprobe；`timeline.json` |
| 视频 | H.264，yuv420p，CRF 18，preset medium，faststart | ffprobe；`render.mjs:160-161` |
| 音频 | AAC 48 kHz 立体声 192 kbps | ffprobe；`render.mjs:161` |
| 文件 | 10,730,017 字节（约 10.7 MB） | 证据包 |
| 字幕 | `.srt` 33 条 + 画面烧录 | 文件；`render.mjs:78-105` |
| 品牌素材 | `assets/logo-transparent.png`（115,977 字节）、`assets/logo.png`（106,123 字节），10-18 22:01 复制 | `assets/`；证据包 |
| 其他版本 | 无 | `out/` |

文件时间：logo 与字体 10-18 22:01，TTS 产物 22:03，配乐音效与成片 22:12。

一句话管线：

> `script.json`（14 段：旁白 + 小朋友 + 月亮）→ `tts.py`（`TARGET = 62`，原速超出后一次提到 +9%）→ 时间轴 + 混音 + SRT → `render.mjs`：3 个场景模块共 7 个场景，`chars.mjs` 绘制会说话的月亮 / 戴墨镜的太阳 / 小朋友 / 眼睛 / 台灯皮球；左上角 logo 淡入，最后一句后接品牌卡 → ffmpeg 管道编码。

## 2. 教学目标与课标对齐

- **curriculum 字段：缺失**（键为 `title`、`voice`、`voices`、`sources`、`segments`）。对应人教版必修第一册第一章（天体类型：卫星不发光、反射太阳光）属**推断**。
- **核心知识点**：月球自身不发可见光，看到的月光主要是月面反射的太阳光；月球是地球的天然卫星。
- **讲解结构（按实际文案）**：
  1. 导入：小朋友“它是自己在发光吗？”→ 旁白追问 → 月亮“我的亮光是借来的”（`hook`、`ask`、`confess`）。
  2. 原理拆解：三步光路——太阳发光照到月面 → 月面反射、散向很多方向 → 一部分到达地球进入眼睛（`sun`、`step1`–`step3`）。
  3. 类比：黑屋子里台灯照皮球，“球自己不发光，我们照样能看见它”（`lamp`）。
  4. 结论：月球绕地球运行，是天然卫星；“很亮”说的是月面反射来的可见月光（`kidQ`、`sat`、`visible`）。
  5. 总结：光路口诀“太阳发出光，月面反射光，我们看见月光”；小朋友与月亮收尾（`recap`、`kidEnd`、`moonEnd`）。
- **科学限定**：用“主要来自太阳光”“可见月光”限定，没有说月球完全不辐射（与 [[star-planet-moon]] 中地球红外热辐射的处理一致）。
- **数字与出处**：文案无具体数值。`sources` 列 4 个 NASA 页面（§9），未逐句对应。

## 3. 技术与依赖

| 类别 | 内容 | 证据 |
| --- | --- | --- |
| 渲染 | Node ESM + `@napi-rs/canvas` `^1.0.11`；`package.json` 为精简版（`"private": true, "type": "module"`，无 scripts） | `package.json` |
| 图片 | `loadImage` 顶层 `await` 读取 logo | `lib.mjs:23-24` |
| TTS | Python `edge-tts` + `aiohttp`，版本未记录 | `tts.py` |
| 字体 | 圆体 / 苹方（`fonts.py`）；去掉 Arial Unicode 回退，新增 `PF` 苹方字体栈 | `lib.mjs` diff |
| 编码 | 系统 ffmpeg，版本未记录 | `render.mjs` |

声线（`script.json:4-8`；全局 `rate: 9`）：

| 角色 | voice | 角色 rate | 实际 rate | pitch | 段数 |
| --- | --- | --- | --- | --- | --- |
| `zh` 旁白 | `zh-CN-XiaoxiaoNeural` | 0 | +9% | 0 | 9 |
| `kid` 小朋友 | `zh-CN-YunxiaNeural` | +4 | +13% | +6 Hz | 3 |
| `moon` 月亮 | `zh-CN-XiaoyiNeural` | +2 | +11% | +3 Hz | 2 |

## 4. 镜头与时间轴

| # | 场景 | 约时间 (s) | 口播要点 | 画面 | 模块 |
| --- | --- | --- | --- | --- | --- |
| 1 | `night` | 0–9.8 | 好亮！自己发光吗？ | 小山坡上的小朋友仰望满月，问号弹出 | `scenes-a.mjs:23-45` |
| 2 | `secret` | 9.8–13.7 | 月亮：光是借来的 | 月亮醒来“嘘”，“借来的”印章 | `scenes-a.mjs:46-79` |
| 3 | `lightPath` | 13.7–30.6 | 三步光路 | 太阳 → 月面三支金色箭头 + 光子；月面被照亮；五支虚线反射箭头扇形散开；一支到达地面“观察者”的眼睛；①②③标签 | `scenes-b.mjs:26-80` |
| 4 | `lampRoom` | 30.6–36.6 | 台灯照皮球 | 暗室台灯“啪”地打开，皮球被照亮，反射光到角落的眼睛，“台灯 ≈ 太阳 / 皮球 ≈ 月亮” | `scenes-b.mjs:86-111` |
| 5 | `orbit` | 36.6–46.6 | 天然卫星；可见月光 | 地球与椭圆轨道上的月球 | `scenes-c.mjs:19-73` |
| 6 | `recap` | 46.6–51.8 | 光路口诀 | 三步卡片 | `scenes-c.mjs:74-103` |
| 7 | `thanks` | 51.8–61.3 | 谢谢太阳 | 月亮向太阳道谢、爱心；58.4 s 起品牌卡 | `scenes-c.mjs:104-124` |

场景在下一段开始前 0.25–0.3 s 切换，`XF = 0.35`，进入从 0.95 倍、离开到 1.04 倍缩放；开头 0.5 s 淡入、结尾 0.5 s 淡出（`render.mjs:111-133`）。

## 5. 关键算法与实现

> 区分：视频**讲解的知识**是月光来源（反射太阳光）与天然卫星；本节是**制作所用的算法**——光路示意几何与卡通绘制，不是光度学或相位计算。

### 5.1 角标 logo + 片尾品牌卡（`render.mjs:43-75`、`lib.mjs:23-24`）

```js
const OUTRO = E('moonEnd') + 0.45;
function brand(ctx, t) {
  const corner = easeOut(prog(t, 0.3, 1.0)) * (1 - prog(t, OUTRO - 0.2, OUTRO + 0.2));
  if (corner > 0) {  // soft radial dark plate, not a box
    const g = ctx.createRadialGradient(170, 110, 20, 170, 110, 260);
    g.addColorStop(0, `rgba(6,16,31,${0.45 * corner})`); g.addColorStop(1, 'rgba(6,16,31,0)');
    ctx.fillStyle = g; ctx.fillRect(0, 0, 460, 240);
  }
  drawLogo(ctx, 36, 76, 290, corner * 0.95);
  const p = prog(t, OUTRO, OUTRO + 0.6);
  ...
  const w = 860 * lerp(0.7, 1, easeBack(p));
```

- 透明 logo 只取有效区域 `LOGO_BOX = {sx:150, sy:96, sw:1998, sh:528}`，绘制宽 290 px、透明度 0.95，0.3–1.0 s 淡入，后面垫一层径向暗晕——这些数值后来被写进 AGENTS.md 第 4 节。
- 品牌卡：最后一句结束 0.45 s 后，背景压暗到 0.9，logo 以 `easeBack` 从 0.7 倍弹到 860 px 宽居中；0.5 s 后出现标语“认识宇宙，从抬头看月亮开始”和小字“资料来源：NASA Moon / Moonlight”；同时角标 logo 淡出，避免同屏两个 logo；配一声 `tada`（`render.mjs:18`）。`TAIL = 3.2`（“room for the closing brand card”）。

### 5.2 三步光路的几何（`scenes-b.mjs:5-80`）

- 太阳 `(230, 690)`、月球 `(760, 930)`、观察者眼睛 `(330, 1235)` 固定；`TOWARD_SUN = atan2(SY−MY, SX−MX)` 同时用于入射箭头方向和月球明暗面方向（`moon(..., {light: TOWARD_SUN})`），保证“被照亮的一侧朝向太阳”。
- ① 入射：沿日月连线的法向偏移 −50/0/+50 画三支箭头，箭头生长进度 `prog(t, at('step1','发出'), at('step1','照到'))`；到达后月面 `lit` 从 0.12 升到 1。
- ② 反射：从受光面按 `FAN = [-1.15, -0.6, 0, 0.6, 1.15]` 弧度扇形画 5 支虚线箭头，逐支错开 0.08 s，对应“散向很多方向”。
- ③ 到达眼睛：只有一支实线箭头指向地面观察者，眼睛在“看见”一词时眨眼并发光——画面上强调“其中一部分”。

### 5.3 明暗面与“未照亮”状态（`chars.mjs:16-29`）

```js
function shade(ctx, x, y, r, light, lit, dark = 'rgba(8,16,30,') {
  ...
  const g = ctx.createLinearGradient(x + dx, y + dy, x - dx, y - dy);
  g.addColorStop(0, dark + '0)'); g.addColorStop(0.46, dark + '0)');
  g.addColorStop(0.58, dark + '0.82)'); g.addColorStop(1, dark + '0.9)');
  ...
  if (lit < 1) { ctx.fillStyle = dark + `${(1 - lit) * 0.82})`; ctx.fillRect(...); }
}
```

沿光照方向的线性渐变在 46%–58% 处形成明暗交界（晨昏线），`lit` 参数再整体压暗，用来表现“没有阳光时月亮是暗的”。`light: 'full'` 时表示从地球看到的满月。台灯皮球场景用同一套 `ball(..., lit, light)`（`chars.mjs:217`）。

### 5.4 逐词驱动 + 结束词时刻

与其他视频相同用 `at(id, word)`；本片 `lib.mjs` 增加 `atEnd`（词结束时刻，如“① 太阳光照到月面”标签在 `atEnd('step1','月面')` 弹出）和 `bob()` 漂浮函数。

## 6. 声音与字幕

| 项 | 内容 | 来源 |
| --- | --- | --- |
| 留白 | `LEAD_IN 1.0`（“let the night sky fade in”）、默认 `GAP 0.45`、`TAIL 3.2` | `tts.py:33-35` |
| 自动提速 | `TARGET = 62.0`（“upper bound for a Douyin short”）；缓存中 14 段都有全局 +0 与 +9 两版 → 原速超长，一次提到 +9% | `tts.py:32`；缓存哈希反查 |
| 旁白混音 | `adelay` → `amix` → `highpass 80` → `loudnorm I=-16:TP=-1.5:LRA=11` | `tts.py` |
| 总混音 | 配乐 **×0.6** 侧链压缩，音效 ×0.5，`alimiter 0.93` | `render.mjs:157-158` |
| 配乐 / 音效 | `audio.mjs` 与 [[geo-celestial-bodies]] 完全相同（112 BPM 拨弦 bed） | diff 无差异 |
| SRT | 33 条 | `out/*.srt` |
| 烧录字幕 | y = 1390 深色胶囊，54 px；“小朋友”天蓝名牌、“月亮”金色名牌 | `render.mjs:78-105` |

## 7. 迭代与问题

| 事项 | 证据 | 说明 |
| --- | --- | --- |
| 最早的带 logo 视频（推断） | 全项目 `lib.mjs` 含 `LOGO_BOX` 的视频中，本片成片时间（10-18 22:12）最早，其次为 [[cosmic-address]]（23:23） | 文件时间可能因重渲染改变，仅作推断 |
| 角标 logo 贴边 | `render.mjs:61` `drawLogo(ctx, 36, 76, 290, …)` | AGENTS.md 第 4 节明确要求新视频“比 moonlight-origin 用的 `x=36` 往右多留一点边距”（x≈64）——即规则以本片为参照并作了修正 |
| 品牌卡细节与现行规则有出入 | `render.mjs:17, 66, 72` | 规则：最后一句后约 0.3 s 出现、logo 宽约 760 px、只放品牌和标语；本片为 0.45 s、860 px，并多一行“资料来源”小字。**推断**规则在本片之后细化 |
| 场景内“示意图”声明 | `scenes-b.mjs:29` “示意图 · 大小、距离不按真实比例”；`scenes-c.mjs:40` “示意图 · 轨道与大小不按真实比例” | 与第 5 节冲突（已做视频不必回改） |
| 为压时长提速到 +9% | `tts.py:32`；`timeline.json` | 与第 2 节不符；**推断**早于 git `35ad4da` 的规则调整 |
| 缺 `curriculum` / `numbers` | `script.json` | 与第 6 节不符 |
| `package.json` 与其他视频不同 | 精简为 `private` + `module` | 无影响 |

README 已收录（第 21 行，竖屏 1:01）。没有 TODO 或备用版本。

## 8. 复现入口

命令来自 AGENTS.md“常用命令”与 `render.mjs:2-3`，**本次均未执行**：

```bash
cd moonlight-origin
python3 -m venv .venv && .venv/bin/pip install edge-tts   # fonts.py 另需 fontTools
npm install
.venv/bin/python tts.py            # → build/narration.wav, build/timeline.json, out/月亮的光从哪里来.srt
node render.mjs --still 22 33 59.5 # 预览光路、台灯、品牌卡
node render.mjs                    # → out/月亮的光从哪里来.mp4
```

需要 `assets/logo-transparent.png`（`lib.mjs` 顶层加载，缺失会直接报错）和 `build/timeline.json`。缓存含 +9% 全部 14 段，**推断**可离线重建。

## 9. 参考来源

`script.json` 的 `sources`（项目文件中提及，**本次未访问**）：

| 名称 | URL | 用途（推断） |
| --- | --- | --- |
| NASA Moonlight | https://science.nasa.gov/moon/moonlight/ | 月光是反射的太阳光 |
| NASA Moon Phases | https://science.nasa.gov/moon/moon-phases/ | 受光面方向、月相 |
| NASA Moon Facts | https://science.nasa.gov/moon/facts/ | 天然卫星、绕地运行 |
| NASA Our Sun: Facts | https://science.nasa.gov/sun/facts/ | 太阳是光源 |

品牌卡小字“资料来源：NASA Moon / Moonlight”与此对应。无 `based_on`。相关本项目视频：[[star-planet-moon]]（月球“关灯实验”）、[[geo-celestial-bodies]]。

## 10. 证据与限制

| 文件 | 行数 | sha256[:12] | 读取程度 |
| --- | --- | --- | --- |
| `moonlight-origin/script.json` | 32 | `f839f7f4ee68` | 全文 |
| `moonlight-origin/render.mjs` | 174 | `cd5edebee2aa` | 全文 |
| `moonlight-origin/lib.mjs` | 222 | `d49d8b195079` | 与 geo-celestial-bodies 版 diff |
| `moonlight-origin/tts.py` | 208 | `c144a5589ee8` | diff（docstring、`TITLE`、`TARGET`、`LEAD_IN`、`TAIL`） |
| `moonlight-origin/chars.mjs` | 230 | `958f89dd8bcc` | 大纲 + `shade`、`moon` |
| `moonlight-origin/scenes-a.mjs` / `-b` / `-c` | 97/133/143 | `4c1e83d0ff19` / `45d9f2172ac2` / `64dd30f75f80` | 大纲、场景表；`lightPath`、`lampRoom` 全文 |
| `moonlight-origin/audio.mjs` / `fonts.py` | 204 / 37 | `f9e04b2cfd54` / `a6ac7264e548` | 与通用版相同 |
| `moonlight-origin/package.json` | 10 | `26db1b193166` | 全文 |
| `moonlight-origin/assets/logo-transparent.png` / `logo.png` | — | `3071b98be488` / `99f946ff382b` | 仅大小与哈希，未查看 |
| `moonlight-origin/build/timeline.json` | — | — | 段落 start/end、rate |
| `moonlight-origin/out/*.mp4` | — | 10,730,017 字节 | 仅证据包 ffprobe |

限制：无制作对话，logo 规则如何从本片演化、品牌卡数值如何选定**未记录**；未查看画面（logo 可读性、品牌卡效果未验证）；工具版本未记录。
