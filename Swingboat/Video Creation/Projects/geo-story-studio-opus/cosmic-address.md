---
type: video-production-dossier
project: geo-story-studio-opus
video_id: cosmic-address
title: 我们在宇宙中的什么位置？
curriculum: script.json 未记录（README：天体系统的层次——地月系、太阳系、银河系、可观测宇宙）
captured: '2026-11-12'
status: 成片存在（竖屏 1080×1920，91.2 s，有角标 logo、无片尾品牌卡）；技术检查/视觉检查/用户验收未记录
source_fingerprint: e3b20741ccadf33c7b080bdca43f4e1bf8d9484155c07fc60a460e6ffdcf783c
tags:
- video-creation
- production-capture
- geography-teaching
- canvas-frame-render
- edge-tts
---

# 我们在宇宙中的什么位置？ · 外星快递员写“宇宙地址”，Powers-of-Ten 式连续拉远（竖屏 6 场景）

<!-- capture-source-sha256:e3b20741ccadf33c7b080bdca43f4e1bf8d9484155c07fc60a460e6ffdcf783c -->

本文是**根据项目文件做的材料回溯**（源码、`script.json`、`build/timeline.json`、`build/tts_cache` 文件名、`out/` 与 `assets/` 清单、证据包 ffprobe 与 sha256）。证据包 `host_session` 为空：**未发现制作会话**。本次未运行任何项目脚本，未打开视频、音频或图片。

## 1. 当前结论与制作路线

**状态：成片存在**——`out/宇宙地址-外星快递送到地球.mp4` + 同名 `.srt`。只有 ffprobe 媒体头检查；视觉检查、试听、用户验收与是否发布**未记录**。

| 项 | 值 | 来源 |
| --- | --- | --- |
| 分辨率 / 画幅 | 1080×1920，9:16 竖屏 | ffprobe；`lib.mjs:7` |
| 帧率 / 帧数 | 30 fps；`round(91.16 × 30)` = 2735 帧 | ffprobe；`render.mjs:288`（推算） |
| 时长 | 91.167 s（timeline 91.16 s） | ffprobe；`timeline.json` |
| 视频 | H.264，yuv420p，CRF 19，preset medium，faststart | ffprobe；`render.mjs:285-286` |
| 音频 | AAC 48 kHz 立体声 192 kbps | ffprobe；`render.mjs:286` |
| 文件 | 18,592,226 字节（约 18.6 MB） | 证据包 |
| 字幕 | `.srt` 44 条 + 画面烧录 | 文件；`render.mjs:166-191` |
| 品牌素材 | 仅 `assets/logo-transparent.png`（115,977 字节，与 [[moonlight-origin]] 同哈希 `3071b98be488`） | `assets/`；证据包 |
| 其他版本 | 无 | `out/` |

文件时间（10-18）：TTS 产物 23:04 → 字体 23:09 → `out/` 目录 23:16 有变动 → logo 复制 23:21 → 配乐音效 23:22 → 成片 23:23。**推断**：先出过一版没有 logo 的成片，之后按 AGENTS.md 第 4 节补 logo 重渲染（`render.mjs:112` 注释写“(AGENTS.md §4)”）。

一句话管线：

> `script.json`（20 段：旁白 + 外星快递员 + 地球 + 月球）→ `tts.py`（`TARGET = 92`，从预设 +4% 提到 +10%）→ 时间轴 + 混音 + SRT → `render.mjs`：每个层级场景带 `zoom` 参数，上一层的焦点缩成下一层里的一个点，镜头连续拉远 5 级；顶部“收件地址”标签逐级填写；快递员 UFO 常驻角落；`cosmos.mjs` 程序生成银河系与星系场 → ffmpeg 管道编码。

## 2. 教学目标与课标对齐

- **curriculum 字段：缺失**（键为 `title`、`source`、`rate`、`voices`、`segments`）。对应人教版必修第一册第一章第一节“天体系统的层次”属**推断**。
- **改编来源**：`script.json` 的 `source` 写“Geo Story Studio · douyin-cosmic-address v002 分镜”，即 `geo-story-studio/storage/builds/douyin-cosmic-address/v002/`（路径按 AGENTS.md 第 1 节命名惯例推断；没有单独的 `based_on` 字段，源 build 本次未查看）。目录名去掉 `douyin-` 前缀和版本号，符合第 1 节。
- **核心知识点**：天体系统的层次——地月系 → 太阳系 → 银河系 → （河外星系）→ 可观测宇宙。
- **讲解结构（按实际文案）**：
  1. 导入：“叮咚！宇宙快递！收件人：地球”，快递员抱怨只写“地球”送不到 → 给地球写宇宙地址（`hook`、`alienQ`、`plan`）。
  2. 原理拆解：逐层拉远——地月系（月球自报平均距离约 38.4 万千米）→ 太阳系（八颗行星，地球自报“第三颗”，日地平均距离约 1.5 亿千米）→ 银河系（至少千亿颗恒星，“这个数量是估计值”）→ 其他星系（`moon1`…`beyond`）。
  3. 纠错 / 科学边界：快递员想写“宇宙的边界”→ 改为“可观测宇宙，也就是信息能够到达我们的范围”；“虚线圈不是整个宇宙的边界；圈的中心只是我们选取的观测位置”（`alienB`、`obs1`、`obs2`、`alienO`）。
  4. 总结：地址写好——地球、地月系、太阳系、银河系、可观测宇宙（`recap`、`send`）。
  5. 迁移：“换你来写：你的宇宙地址，第一行写什么？评论区见！”（`cta`）。
- **科学边界**：文案主动限定“估计值”“不是宇宙中心，是观测位置”“不是整个宇宙的边界”；`planetPos` 注释说明行星位置只是示意、非任何日期的真实位置（`scenes-b.mjs:13`）。
- **数字与出处**：38.4 万千米、1.5 亿千米、至少千亿颗——`source` 字段笼统列出 NASA Science 六个条目（Moon Facts、Solar System Facts、Our Sun: Facts、Galaxies、What is the Universe?、APOD 2022-03-16），没有 URL，也没有逐个数字对应的 `numbers` 字段。

## 3. 技术与依赖

| 类别 | 内容 | 证据 |
| --- | --- | --- |
| 渲染 | Node ESM + `@napi-rs/canvas` `^1.0.11`（`loadImage` 读 logo）；`scripts` 仅默认 `test` | `package.json`；`lib.mjs:23-25` |
| TTS | Python `edge-tts` + `aiohttp`，版本未记录 | `tts.py` |
| 字体 | 圆体 / 苹方（`fonts.py`） | `lib.mjs` |
| 编码 | 系统 ffmpeg，版本未记录 | `render.mjs` |

`bodies.mjs`、`audio.mjs`、`fonts.py` 与 [[geo-celestial-bodies]]、[[star-planet-moon]] 哈希相同；新增 `cast.mjs`（快递员、UFO、包裹、定位针、信息标签）和 `cosmos.mjs`（八大行星、银河系、星系场、虚线圈、小行星带）。

声线（`script.json:5-10`；全局 `rate: 10` 取代预设 `"rate": 4`）：

| 角色 | voice | 角色 rate | 实际 rate | pitch | 段数 |
| --- | --- | --- | --- | --- | --- |
| `zh` 旁白 | `zh-CN-XiaoxiaoNeural` | 0 | +10% | 0 | 12 |
| `alien` 快递员 | `zh-CN-YunxiaNeural` | +10 | +20% | +8 Hz | 6 |
| `earth` 地球 | `zh-CN-YunxiNeural` | +6 | +16% | +2 Hz | 1 |
| `moon` 月球 | `zh-CN-XiaoyiNeural` | +4 | +14% | +6 Hz | 1 |

地球、月球的声线参数与 [[star-planet-moon]] 完全相同。

## 4. 镜头与时间轴

| # | 场景 | 约时间 (s) | 口播要点 | 画面 | 模块 / 拉远倍数 |
| --- | --- | --- | --- | --- | --- |
| 1 | `hook` | 0–13.9 | 门铃、送不到、写地址 | UFO 飞入，雷达扫描，地球被插上金色定位针 | `scenes-a.mjs:33-73` |
| 2 | `moon` | 13.9–24.8 | 地月系、38.4 万千米 | 地球缩成左侧小球，月球沿倾斜椭圆轨道；距离标签 | `scenes-a.mjs:84-140`；F = 220/115 |
| 3 | `solar` | 24.8–42.6 | 太阳系、第三颗、1.5 亿千米 | 八大行星倾斜轨道 + 小行星带，地球高亮 | `scenes-b.mjs:28-118`；F = 12 |
| 4 | `galaxy` | 42.6–53.8 | 太阳只是一颗；至少千亿颗 | 粒子银河系，太阳位置标记，数字从 0 滚到“千亿颗” | `scenes-b.mjs:130-159`；F = 22 |
| 5 | `beyond` | 53.8–74.4 | 其他星系 → 可观测宇宙 | 星系分层出现；红色“宇宙的边界？”实线圈被否定；金色虚线圈 + 光信使流向中心；“虚线 ≠ 宇宙边界”“观测位置 ≠ 宇宙中心” | `scenes-c.mjs:21-87`；F = 480/44 |
| 6 | `recap` | 74.4–91.2 | 地址写好、出发、评论区 | 五行地址清单，快递员挂上包裹飞走 | `scenes-c.mjs:110-156` |

常驻层（`render.mjs:237-250`）：背景 → 场景 → 顶部“宇宙快递 · 收件地址”标签（y≈182，位于 logo 区下方，五枚芯片逐级从“？”填成名称）→ 快递员 UFO → 底部声明 → 字幕（`SUB_Y = 1490`）→ 左上角 logo。

## 5. 关键算法与实现

> 区分：视频**讲解的知识**是天体系统层次与可观测宇宙的含义；本节是**制作所用的算法**——示意性的缩放镜头与粒子绘图，尺度与轨道都不是真实比例。

### 5.1 Powers-of-Ten 式连续拉远（`render.mjs:199-232`）

```js
// A scene with `zoom: { focus: [x,y], anchor(t) -> [x,y], F }` swallows the previous scene: the previous scene's
// `focus` point lands on `anchor` in the new scene, `F` times smaller, and slides there as the camera pulls back.
function zoomState(s) {
  return (t) => {
    const e = easeIO(prog(t, s.from, s.from + ZD));
    const [ax, ay] = s.zoom.anchor(s.from + ZD);
    const [px, py] = s.zoom.focus ?? [CX, CY];
    return { z: Math.pow(s.zoom.F, 1 - e), qx: lerp(px, ax, e), qy: lerp(py, ay, e), ax, ay, px, py, F: s.zoom.F };
  };
}
```

- 新场景从 `F` 倍放大缩回 1 倍（缩放按 `F^(1−e)` 指数插值，视觉上匀速“后退”），旧场景同步按 `z/F` 缩小，旧场景的焦点（如地球）正好落到新场景的锚点（如太阳系里的地球位置）上，`ZD = 1.5` s。
- 各级 `F`：地球→地月系 220/115≈1.9；地月系→太阳系 12；太阳系→银河系 22；银河系→星系场 480/44≈10.9（`scenes-a.mjs:145`、`scenes-b.mjs:163,167`、`scenes-c.mjs:160`）。
- 太阳系场景里行星在拉远落定前保持不动（`T0 = S('sun1') - 0.4 + 1.5`，`scenes-b.mjs:10`），保证锚点 `planetPos(2, t)` 与旧场景地球对齐。
- 拉远期间背景星星变成指向画面焦点的“曲速”线（`zoomAmount`，`render.mjs:29-32, 43-55`），每次拉远自动加一个 `whoosh` 音效（`render.mjs:254`）。

### 5.2 粒子银河系（`cosmos.mjs:54-93`）

1600 个旋臂粒子 + 500 个核球粒子，用确定性伪随机 `rnd(i)` 生成，按倾角 0.62、旋转 −0.35 投影；`onDisk(u, v)` 用同一变换求太阳在盘面上的屏幕位置，供下一次拉远做锚点。注释称“two-armed barred-spiral sketch”，但代码 `arm = i % 4`、每臂相隔 π/2，实际画的是 **4 条旋臂**——注释与实现不一致（不影响教学结论，属代码事实）。

### 5.3 “可观测宇宙”的可视化（`scenes-c.mjs:9-60`）

46 个星系按层次淡入；快递员猜“宇宙的边界？”时先出现红色实线圈并被否定；说到“可观测”时金色虚线圈（滚动虚线 `off: -t * 30`）展开，圈外星系降到 0.4 透明度但**仍然保留**，圈内星系向中心发射“光信使”光点（表示信息到达观测者）；再依次弹出“虚线 ≠ 宇宙边界”和“观测位置”标签。用画面直接对应文案中的两个易错点。

### 5.4 收件地址标签与 logo 让位（`render.mjs:58-126`）

`LEVEL_TIMES` 取五个层级词的开口时刻，标签里的芯片从“？”虚框弹成彩色名称，当前层级高亮；每次填写伴随 `stamp` + 递升音 `note`（`render.mjs:255-256`）。标签纵向位置 `y0 = 182`，注释“sits below the corner logo (logo zone ends at y≈153)”。logo 按 AGENTS.md 第 4 节放在 `x = 64, y = 76`，宽 290、透明度 0.95、径向暗晕、0.3–1.0 s 淡入。

### 5.5 快递员的路线（`render.mjs:128-161`）

UFO 在 `hook` 前从右上飞入（大号）→ `moon1` 前 1.2 s 停到右下角（0.4 倍，避开主画面）→ `recap` 时回到中间 → “出发”时沿抛物线加速飞出并拖尾；口型由 `talkLevel` 按快递员 6 段台词的词边界驱动，包裹只在开场和送出时挂着。

## 6. 声音与字幕

| 项 | 内容 | 来源 |
| --- | --- | --- |
| 留白 | `LEAD_IN 1.0`、默认 `GAP 0.45`、`TAIL 2.6`（“room for the closing send-off”） | `tts.py:33-35` |
| 自动提速 | `TARGET = 92.0`；缓存中 20 段都有全局 +4 与 +10 两版 → 预设 +4% 超长，一次提到 +10% | `tts.py:32`；缓存哈希反查 |
| 旁白混音 | `adelay` → `amix` → `highpass 80` → `loudnorm I=-16:TP=-1.5:LRA=11` | `tts.py` |
| 总混音 | 配乐 ×0.6 侧链压缩，音效 ×0.5，`alimiter 0.93` | `render.mjs:282-283` |
| 配乐 / 音效 | 通用 `audio.mjs`（112 BPM 拨弦 bed）；拉远 `whoosh`、层级 `note`（C 大调 72/76/79/84/88）+ `stamp` 由 `render.mjs` 自动生成 | `render.mjs:252-259` |
| SRT | 44 条 | `out/*.srt` |
| 烧录字幕 | `SUB_Y = 1490`，字号 56 起递减至整行 ≤ `W − 60`；快递员绿色、地球天蓝、月球银白名牌 | `render.mjs:164-191` |

## 7. 迭代与问题

| 事项 | 证据 | 说明 |
| --- | --- | --- |
| 有角标 logo，但**没有片尾品牌卡** | `render.mjs:113-126` 只有角标；结尾为快递员飞走 + CTA，`TAIL` 注释“closing send-off” | 与 AGENTS.md 第 4 节“片尾品牌卡（必需）”不符；本片已按 §4 放 logo 且 x=64，**推断**品牌卡要求是之后补进规则的 |
| logo 晚于 TTS 加入 | `assets/` 23:21，成片 23:23，`out/` 23:16 有变动 | **推断**补 logo 后重渲染 |
| 画面有“示意”类声明 | `render.mjs:195` 底部“科普示意 · 大小、距离、轨道与速度均不按比例”，复习段换成“资料：NASA Science · 科普示意，不按比例”；场景内“图中距离不按比例”（`scenes-a.mjs:138`）、“轨道、大小与位置均为示意”（`scenes-b.mjs:114`）、“（位置示意）”（`scenes-b.mjs:146`）、“能接收到信息的范围（示意）”（`scenes-c.mjs:50`） | 与第 5 节冲突（已做视频不必回改）；“资料：NASA Science”部分符合第 5 节建议 |
| 为压时长提速到 +10% | `tts.py:32`；`timeline.json` | 与第 2 节不符；**推断**早于 git `35ad4da` 的规则调整 |
| 银河系注释与代码不符 | `cosmos.mjs:54` vs `arm = i % 4` | 见 §5.2 |
| 结尾“评论区见！” | `script.json` `cta` | 第 6 节要求收尾不做成广告式口号，属判断问题 |
| 缺 `curriculum`、`numbers`，`source` 无 URL | `script.json` | 与第 6 节不符 |

README 已收录（第 18 行，竖屏 1:31）。

## 8. 复现入口

命令来自 AGENTS.md“常用命令”与 `render.mjs:2-3`，**本次均未执行**：

```bash
cd cosmic-address
python3 -m venv .venv && .venv/bin/pip install edge-tts   # fonts.py 另需 fontTools
npm install
.venv/bin/python tts.py            # → build/narration.wav, build/timeline.json, out/宇宙地址-外星快递送到地球.srt
node render.mjs --still 26 44 66   # 检查三次拉远落点与可观测宇宙
node render.mjs                    # → out/宇宙地址-外星快递送到地球.mp4
```

依赖 `assets/logo-transparent.png`（`lib.mjs` 顶层加载）与 `build/timeline.json`。缓存含 +10% 全部 20 段，**推断**可离线重建。

## 9. 参考来源

- `script.json` `source` 字段（项目文件中提及，**无 URL，本次未访问**）：Geo Story Studio `douyin-cosmic-address` v002 分镜（`geo-story-studio/storage/builds/douyin-cosmic-address/v002/`，路径为推断）；NASA Science：Moon Facts、Solar System Facts、Our Sun: Facts、Galaxies、What is the Universe?、APOD 2022-03-16 “The Observable Universe”。
- 代码与证据包中 **URL 数为 0**。
- 相关本项目视频：[[geo-celestial-bodies]]（天体系统四级作为其中一段，用逐级缩放实现）、[[star-planet-moon]]（同款地球 / 月球声线）、[[moonlight-origin]]（logo 实现来源）。

## 10. 证据与限制

| 文件 | 行数 | sha256[:12] | 读取程度 |
| --- | --- | --- | --- |
| `cosmic-address/script.json` | 34 | `e7b9a34ce077` | 全文 |
| `cosmic-address/render.mjs` | 299 | `cdf0361e8e62` | 全文 |
| `cosmic-address/lib.mjs` | 214 | `df199d637f81` | 与 geo-celestial-bodies 版 diff（`W/H`、logo） |
| `cosmic-address/tts.py` | 208 | `6ac1fe1e4437` | diff（`TITLE`、`TARGET`、`LEAD_IN`、`TAIL`） |
| `cosmic-address/cast.mjs` | 141 | `b8fe3b7be50a` | 大纲 |
| `cosmic-address/cosmos.mjs` | 178 | `93efd0e4b46a` | 大纲 + 行星表、银河系 |
| `cosmic-address/scenes-a.mjs` / `-b` / `-c` | 161/185/180 | `6c7548607a85` / `39a98e7c8971` / `cc8be6255f46` | 大纲、场景与 zoom 定义、`beyondScene` 前半 |
| `cosmic-address/bodies.mjs` / `audio.mjs` / `fonts.py` | 273/204/37 | `18431acd3a06` / `f9e04b2cfd54` / `a6ac7264e548` | 与通用版同哈希 |
| `cosmic-address/package.json` | 17 | `9a71a9b55908` | 证据包摘要 |
| `cosmic-address/assets/logo-transparent.png` | — | `3071b98be488` | 仅哈希与时间 |
| `cosmic-address/build/timeline.json` | — | — | 段落 start/end、rate |
| `cosmic-address/out/*.mp4` | — | 18,592,226 字节 | 仅证据包 ffprobe |

限制：无制作对话；源 build（v002 分镜）未查看，改编时做了哪些取舍**无法确认**；“先渲染无 logo 版”只是文件时间推断；未做视觉检查（拉远落点是否对齐、logo 与标签是否重叠未验证）；工具版本未记录。
