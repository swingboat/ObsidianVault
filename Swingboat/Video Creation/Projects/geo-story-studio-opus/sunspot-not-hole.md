---
type: video-production-dossier
project: geo-story-studio-opus
video_id: sunspot-not-hole
title: 太阳黑子，是太阳上的黑洞吗？
curriculum: 人教版必修第一册 第一章第二节 太阳对地球的影响（太阳活动：黑子）；课标 1.2
captured: '2026-11-12'
status: 成片存在（竖屏 1080×1920，109.07 s）；无片尾品牌卡；技术检查/视觉检查/用户验收未记录
source_fingerprint: b0f98a6b1c7269c71ee33d22c086760ad91c2a8cadb2201d52e205fdd77c784e
tags:
- video-creation
- production-capture
- geography-teaching
- canvas-frame-render
- edge-tts
---

# 太阳黑子，是太阳上的黑洞吗？ · 误解驱动的三角色对话 + “换背景”对比实验动画的竖屏短片

<!-- capture-source-sha256:b0f98a6b1c7269c71ee33d22c086760ad91c2a8cadb2201d52e205fdd77c784e -->

本文是根据项目文件做的材料回溯（源码、`script.json`、`build/timeline.json`、`build/tts_cache` 文件名与时间、`out/` 清单与 ffprobe）。按元数据找到一个可能对应的宿主会话 `c30c8708…`（2026-10-26 09:49 创建），但宿主返回空 transcript，**对话内容不可读**；对应关系按时间先后**推断**。本次**未运行任何项目脚本**，未打开音视频。

## 1. 当前结论与制作路线

**状态**：`out/太阳黑子是太阳上的黑洞吗.mp4` 与 `.srt` 存在，无封面。成片存在 ≠ 视觉/用户验收，均**未记录**。

| 项 | 值 | 来源 |
| --- | --- | --- |
| 分辨率 / 帧率 | 1080×1920，30 fps | ffprobe；`lib.mjs` |
| 时长 | 109.067 s（timeline 109.062） | ffprobe；`build/timeline.json` |
| 帧数 | 3272（`Math.round(DURATION*FPS)` 推算） | `render.mjs:159` |
| 视频 / 音频 | H.264 yuv420p CRF 19；AAC 192k 48 kHz 立体声 | ffprobe；`render.mjs:156-157` |
| 文件大小 | 26,166,995 B（约 26.2 MB） | 证据包 |
| 字幕 | SRT 57 条 cue + 画面烧录（y=1530） | timeline；`render.mjs:17, 64-89` |
| 时间线 | TTS 09:57–09:59 → timeline/SRT 09:59:53 → MP4 10:11:35（2026-10-26） | 缓存与文件 mtime |

**管线**：`script.json`（25 句，旁白 + 好奇小孩“小宇” + 拟人太阳）→ `tts.py`（`TARGET = 110` 上限，最终全局 +10%）→ `timeline.json` / `narration.wav` / SRT → `render.mjs` 加载 `scenes-a…e.mjs`（10 个场景）及 `sun.mjs`（太阳角色与内部结构）、`spot.mjs`（黑子、米粒组织、对流、磁力线）、`cast.mjs`（宇航员小宇）、`gear.mjs`（望远镜、黑洞、月亮、遮阳伞、“按住的手”、耀斑）、`props.mjs`（标题、印章、≠ 号）→ 逐帧 RGBA 管道送 ffmpeg，`audio.mjs` 程序配乐/音效（每次场景切换自动加 whoosh）→ MP4。

## 2. 教学目标与课标对齐

- **curriculum**：人教版必修第一册 第一章第二节 太阳对地球的影响（太阳活动：黑子）；课标 1.2 运用资料，说明太阳对地球的影响。
- **来源**（`script.json` `source`）：Geo Story Studio · `douyin-sunspot-darkness` v004 分镜，即 `geo-story-studio/storage/builds/douyin-sunspot-darkness/v004`（路径为按命名规律推断，未打开）；事实依据 NSO、NOAA SWPC、NASA/ESA SOHO、NASA Sun Facts、NASA Black Holes、人教版必修第一册。无 `based_on` 字段。
- **核心知识点**：黑子是光球上温度较低、显得较暗的区域，仍在发光；强磁场抑制对流使其变冷；黑子多少和大小是太阳活动强弱的标志，周期约 11 年。
- **叙事结构**：

| 环节 | 段落 | 内容 |
| --- | --- | --- |
| 情境 + 误解 | spot, hole, sunNo, safe | 小宇看到黑点以为“太阳破了个黑洞”，太阳抗议；先插入安全提醒（专业滤光镜） |
| 原理①位置与本质 | photo, gas | 黑子长在光球上；光球是炽热气体，不是硬壳 |
| 原理②为什么暗 | why … bh | 比周围冷一两千度（光球约 5500 ℃）；“去黑子里乘凉？”还有几千度；它在发光，只是被衬暗；换黑背景立刻变亮；单独放夜空比满月亮得多；黑洞连光都逃不出 |
| 原理③为什么冷 | whyCool, soup, mag, term | 对流像沸腾的汤把热量送上来；强磁场像“看不见的手”按住对流 → 教材表述“强磁场抑制了对流” |
| 规律 | cycle, years, mark | 有时多有时少，平均约 11 年、每轮长短不一；黑子多时耀斑往往更频繁 → 太阳活动强弱的标志 |
| 考点 + 总结 | exam, sum, kidBye, sunBye | 黑子—光球层、耀斑—色球层、周期约 11 年；一句话定义；结尾再提醒戴滤光镜 |

- **数字与出处**（`script.json` `numbers`）：光球约 5500 ℃（NASA Sun Facts）；黑子低 1000–2000 K、仍 3000–4000 K（NSO）；“单独看比满月还亮”——NSO 定性说法 + 自带估算（本影单位面积亮度约为光球的两三成，满月约为太阳的四十万分之一）；平均约 11 年（NOAA SWPC、NSO）；层位对应取教材。
- **科学处理**：`spot.mjs:2` 注释“The spot is always drawn in the same warm brown/orange — never black — because it still glows”——画面配色本身就服务于“黑子不黑”的结论；“每轮长短不一”补足了“约 11 年”的不确定性。

## 3. 技术与依赖

| 类别 | 内容 | 证据 |
| --- | --- | --- |
| 渲染 | `@napi-rs/canvas` 声明 `^1.0.11`，lock 解析 **1.0.13** | `package.json`；`package-lock.json` |
| npm scripts | 无 `scripts` 字段 | `package.json` |
| Python | edge-tts、aiohttp、fontTools；无 requirements，版本**未记录**；目录里残留 `__pycache__/tts.cpython-313.pyc`（说明曾用 Python 3.13 导入过 tts.py） | 文件清单 |
| 字体 | 缺字体自动跑 `fonts.py`，回退 Arial Rounded / Arial Unicode / Hiragino | `lib.mjs:11-22` |
| ffmpeg | 旁白 adelay/amix/highpass/loudnorm；合成 sidechaincompress/alimiter/libx264/aac | `tts.py`；`render.mjs:148-157` |
| 复用来源 | `sun.mjs` 头注释“from sun-energy”；`cast.mjs` 含 nebula-nursery 的气体/尘埃/星云函数 | 头注释 |

**声线**（全局 `rate: 4`，timeline 最终 `rate: 10`）：

| 角色 | voice | 角色 rate / pitch | 实际 rate | 句数 |
| --- | --- | --- | --- | --- |
| `zh` 旁白 | zh-CN-XiaoxiaoNeural | 0 / 0 | +10% | 15 |
| `kid` 小宇 | zh-CN-YunxiaNeural | +6 / +8 Hz | +16% | 6 |
| `sun` 太阳 | zh-CN-YunjianNeural | 0 / −4 Hz | +10% | 4 |

## 4. 镜头与时间轴

时间取自 `build/timeline.json`；场景在下一段落前 0.25 s 切换，交叉淡化 `XF = 0.4`，每次切换自动叠一个 whoosh（`render.mjs:128-131`）。

| 场景 | 约时间 (s) | 段落 | 画面要点 | 模块 |
| --- | --- | --- | --- | --- |
| hook | 0–9.9 | spot, hole, sunNo | 宇航员小宇用望远镜看太阳，想象气泡里出现黑洞；太阳抗议，标题换成“那是我的黑子！” | `scenes-a.mjs` |
| safe | 9.9–14.7 | safe | “安全第一！”：望远镜套上太阳滤光镜 | `scenes-a.mjs` |
| photo | 14.7–22.9 | photo, gas | 放大到光球米粒组织：不是硬壳，是炽热气体 | `scenes-b.mjs` |
| cool | 22.9–36.3 | why … hot | 温度对比（约 5500 ℃ vs 低一两千度）；小宇撑遮阳伞去“乘凉”被烤冒烟 | `scenes-b.mjs` |
| contrast | 36.3–44.2 | glow, swap | 同一块黑子：亮背景被从左到右“揭掉”，换成黑背景后黑子发光变亮 | `scenes-c.mjs:30` |
| moonbh | 44.2–53.4 | moon, bh | 黑子 vs 满月亮度；黑洞吞光，“≠”号 | `scenes-c.mjs` |
| soup | 53.4–71.2 | whyCool, soup, mag, term | 光球下对流环；巨大半透明手（强磁场）按住黑子下方，对流变慢、热量箭头缩短，磁力线拱出表面 | `scenes-d.mjs`；`spot.mjs:91, 130` |
| cycle | 71.2–87.3 | cycle, years, mark | 太阳上黑子增减；“平均约 11 年”；黑子多时耀斑闪现 | `scenes-e.mjs:20` |
| exam | 87.3–94.4 | exam | 三行考点：黑子—光球层、耀斑—色球层、周期约 11 年；分层示意 | `scenes-e.mjs` |
| outro | 94.4–109.1 | sum, kidBye, sunBye | 一句话定义；小宇与太阳道别，太阳戴墨镜；页脚资料行 | `scenes-e.mjs` |

5 个场景模块共 10 个场景，另有 5 个绘图库（`sun/spot/cast/gear/props.mjs`）。

## 5. 关键算法与实现

> 视频**讲的知识**是黑子成因与太阳活动；以下是**制作算法**，不是磁流体或辐射模拟。

### 5.1 “换背景”对比实验（`scenes-c.mjs:30-63`）

```js
const peel = easeIO(prog(t, tBlack - 0.2, tBlack + 0.9));
ctx.fillStyle = '#05061a'; ctx.beginPath(); ctx.roundRect(x, y, w, h, 30); ctx.fill();
if (peel < 1) {
  ctx.save(); ctx.beginPath(); ctx.rect(x + w * peel, y - 10, w * (1 - peel) + 10, h + 20); ctx.clip();
  granules(ctx, t, x, y, w, h, { cell: 64 }); ctx.restore();
}
sunspot(ctx, cx, cy, 150, { seed: 3 });     // 黑子本身的颜色始终不变
```

底层先铺黑，再用随时间右移的裁剪矩形只保留未“揭掉”部分的米粒组织；黑子用同一参数绘制，只在说到“亮了起来”时叠加暖色光晕。画面把“同色在不同背景下明暗感不同”（同时对比）直接演示出来，揭开时刻锚在 `at('swap', '黑背景')`。

### 5.2 “强磁场抑制对流”的可视化（`spot.mjs:89-127`）

```js
const near = clamp(1 - Math.abs(cx - spotX) / (cw * 0.9));
const k = 1 - 0.85 * calm * near;            // 越靠近黑子、calm 越大，对流越慢
ctx.lineDashOffset = -t * 60 * k * dir;       // 对流环流动速度
const al = (h * 0.42) * k;                    // 向上的热量箭头长度
```

每个对流单元按与黑子的水平距离得到衰减系数 `k`，同时控制虚线流动速度、热团亮度和向上热量箭头长度；`calm` 随旁白“按住了翻滚的汤”增大。教材结论“强磁场抑制了对流”因此对应到画面上“黑子下方变慢、箭头变短”。

### 5.3 黑子绘制与配色约束（`spot.mjs:1-46`）

`blobPath` 用确定性 `rnd(seed)` 扰动半径并以二次曲线连接，得到不规则的半影/本影；`SPOT` 色板固定为暖棕/橙（`umbra #7a3410`、`pen #b9601f`），代码注释明确“never black”。

### 5.4 词级时间与角色字幕

同前几片：`at()/S()/E()` 取 WordBoundary 时间；字幕胶囊字号从 56 逐级缩小到适宽，小宇（天蓝）/太阳（金色）带角色标签，文字颜色随角色变化（`render.mjs:61-89`）。背景在讲对流段落时整体变暖（`render.mjs:29-32`）。

## 6. 声音与字幕

| 项 | 内容 | 来源 |
| --- | --- | --- |
| 合成参数 | `TARGET=110`（注释“upper bound; the piece is as long as the narration needs”）、`LEAD_IN=1.0`（让标题动画先呼吸）、`GAP=0.45`、`TAIL=2.8`（注释“room for the closing brand card”）；字幕块 ≤14 字 | `tts.py:31-35` |
| 提速经过 | 缓存中当前 25 句有三套：+4%（09:57–09:59）、+30%（09:57–09:58，仅 18/25 句命中，说明当时有 7 句文案不同）、+10%（09:59）；最终 +10% | 缓存键复算与 mtime |
| 旧文案残留 | 82 组缓存中 14 组对应已不存在的文本 | 同上 |
| 旁白混音 | adelay → amix → highpass 80 → loudnorm −16 LUFS / −1.5 dBTP / LRA 11 | `tts.py` |
| 配乐 | 与 [[meteor-annual]] 同一 `audio.mjs`（hash `f9e04b2cfd54`）：112 BPM 拨弦 C–G–Am–F | `audio.mjs:150-185` |
| 音效 | whoosh（换背景从 2600→500 Hz 下滑）、tada（“亮了起来”）、boing/ding（满月）等；另每个场景切换自动 whoosh | `scenes-c.mjs:120-124`；`render.mjs:128-131` |
| 总混音 | 音乐 0.55 侧链压缩（0.05 / 5:1 / 20 / 400 ms），音效 0.5，`alimiter 0.93`；总线无 loudnorm | `render.mjs:153-154` |

本次未试听、未测响度。

## 7. 迭代与问题

| 事项 | 证据 | 说明 |
| --- | --- | --- |
| 无片尾品牌卡 | `render.mjs:47-59, 115-126`；`scenes-e.mjs` outro 到 `Infinity` | 角标 logo 全程不淡出、没有中央大 logo + 标语；而 `tts.py:35` 注释写“room for the closing brand card”——注释与实现不符。“片尾品牌卡（必需）”2026-11-01 才写入 AGENTS，制作时（10-26）非硬性；按现行规则缺失 |
| 为压时长提速到 +10% | `tts.py:32`；缓存 | 制作于 10-26 上午，早于当晚 21:03 的“不设上限”规则（`35ad4da`），当时合规；按现行规则属不符 |
| 目录名与源 build 不一致 | `script.json` `source` 为 `douyin-sunspot-darkness`，目录为 `sunspot-not-hole` | 现行 AGENTS 第 1 节“目录名沿用源 build 名称”于 2026-11-01（`9da7a28`）才加入；按现行规则应为 `sunspot-darkness/` |
| 复用库带未用代码 | `cast.mjs` 中 `gasDot/dustGrain/atomBuddy/nebulaCloud/babyStar/sparkleStar` 在本片场景中无引用 | 从 nebula-nursery / sun-energy 复制的整文件 |
| 参考资料无 URL | `script.json` 只有资料名称 | 证据包 `urls: []` |
| “示意”声明 | `render.mjs:91` 注释 | 未放；页脚只在总结段显示“资料：NSO · NOAA SWPC · NASA · 人教版必修第一册” |
| 安全提醒 | `safe`、`sunBye` 段 | 开头和结尾两次提醒用滤光镜观测，属内容层面的安全处理 |

## 8. 复现入口

**本次均未执行**：

```bash
cd sunspot-not-hole
python3 -m venv .venv && .venv/bin/pip install edge-tts fonttools   # 版本未记录
npm install
.venv/bin/python tts.py            # → build/narration.wav, build/timeline.json, out/太阳黑子是太阳上的黑洞吗.srt
node render.mjs --still 38 60      # 可选预览 → build/still_<t>.png
node render.mjs                    # → out/太阳黑子是太阳上的黑洞吗.mp4
```

+4% 与 +10% 两套缓存齐全，**推断**在 `TARGET = 110` 下可离线重建。

## 9. 参考来源

`script.json` 中**没有 URL**，只有资料名称（项目文件中提及，本次未访问）：

| 资料 | 用途 |
| --- | --- |
| NSO《Sunspots and Active Regions》 | 黑子温差、仍很亮、约 11 年 |
| NOAA SWPC《Sunspots / Solar Cycle》 | 太阳活动周期 |
| NASA/ESA SOHO Sunspots lesson | 黑子教学资料 |
| NASA《Sun: Facts》 | 光球约 5500 ℃ |
| NASA《Black Holes》 | 黑洞连光都逃不出 |
| 人教版地理必修第一册 | 黑子—光球、耀斑—色球 |
| `geo-story-studio/storage/builds/douyin-sunspot-darkness/v004` | 分镜来源（路径按命名推断，未打开） |

相关：[[solar-eruption-earth]]（同一套 `sun/cast/audio` 库，承接“太阳活动对地球的影响”）、[[sun-energy]]、[[nebula-nursery]]。

## 10. 证据与限制

| 文件 | 行数 | sha256[:12] | 读取程度 |
| --- | --- | --- | --- |
| `sunspot-not-hole/script.json` | 252 | `1f38582434c6` | 全文（字段与全部台词） |
| `sunspot-not-hole/tts.py` | 208 | `19dbc7d5a621` | 与 meteor-annual 版 diff |
| `sunspot-not-hole/render.mjs` | 170 | `50abc88537dc` | 全文 |
| `sunspot-not-hole/spot.mjs` | 174 | `9b133ceef519` | 1-46、89-127 行 |
| `sunspot-not-hole/scenes-a…e.mjs` | 149/167/128/96/167 | `86b19409b10e` / `bd6c1cb3d5a9` / `658fc87f5c4a` / `871c95e793ab` / `f732a1f808a7` | 场景表；scenes-c contrast 全文；scenes-e cycle/exam 片段 |
| `sunspot-not-hole/sun/cast/gear/props.mjs` | 124/243/218/153 | `994d919821d0` / `c13a62ae1572` / `30e1c6bf2b7e` / `4debdbd1b652` | 函数清单 |
| `sunspot-not-hole/lib.mjs` / `audio.mjs` / `fonts.py` | 219/204/37 | `7d757aa5fc33` / `f9e04b2cfd54` / `a6ac7264e548` | 关键段 / 同 hash 前作 |
| `out/太阳黑子是太阳上的黑洞吗.mp4` | — | 26,166,995 B | 仅证据包 ffprobe |

限制：制作对话不可读；源文件 mtime 集中在 09:52–10:10、成片 10:11，可能被后续编辑改写，不能当作精确工时；预览帧、用户意见**未记录**；Git 中随 `1aa541f`（2026-10-26，5 片合并提交）入库。
