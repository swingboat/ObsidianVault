---
type: video-production-dossier
project: geo-story-studio-opus
video_id: earth-life-conditions
title: 宜居星球面试会：地球凭什么能住人？
curriculum: 人教版必修第一册 第一章第一节 地球的宇宙环境（地球上存在生命的条件）；课标1.1
captured: '2026-11-12'
status: 成片存在（竖屏 1080×1920，132.40 s，含片尾品牌卡）；技术检查/视觉检查/用户验收未记录
source_fingerprint: 0a06ad44ab58f9fee19f49bfb7c9301aa9a9e87e636f899c2a273a99008b700d
tags:
- video-creation
- production-capture
- geography-teaching
- canvas-frame-render
- edge-tts
---

# 宜居星球面试会：地球凭什么能住人？ · 五声线行星“面试”群戏 + 条件闯关的竖屏短片

<!-- capture-source-sha256:0a06ad44ab58f9fee19f49bfb7c9301aa9a9e87e636f899c2a273a99008b700d -->

本文是根据项目文件做的材料回溯（源码、`script.json`、`build/timeline.json`、`build/tts_cache` 文件名与时间、`out/` 清单与 ffprobe）。按元数据找到一个可能对应的宿主会话 `5238606a…`（2026-10-25 18:28 创建），但宿主返回空 transcript，**对话内容不可读**；对应关系按时间先后**推断**。本次**未运行任何项目脚本**，未打开音视频。

## 1. 当前结论与制作路线

**状态**：`out/宜居星球面试会.mp4` 与 `.srt` 存在，无封面文件。成片存在 ≠ 视觉/用户验收，均**未记录**。

| 项 | 值 | 来源 |
| --- | --- | --- |
| 分辨率 / 帧率 | 1080×1920，30 fps | ffprobe；`lib.mjs:7` |
| 时长 | 132.400 s（timeline 132.411） | ffprobe；`build/timeline.json` |
| 帧数 | 3972（`Math.round(DURATION*FPS)` 推算） | `render.mjs:203` |
| 视频 / 音频 | H.264 yuv420p CRF 19；AAC 192k 48 kHz 立体声 | ffprobe；`render.mjs:198-201` |
| 文件大小 | 24,547,639 B（约 24.5 MB） | 证据包 |
| 字幕 | SRT 73 条 cue + 画面烧录 | timeline；`render.mjs` `subtitles()` |
| 时间线 | TTS 多轮 19:10–19:13 → `tts.py` 改 19:31:03 → timeline/SRT 19:31:09 → MP4 19:33:27（2026-10-25） | 缓存与文件 mtime |

**管线**：`script.json`（24 句，5 个声音角色：主持人 + 地球/水星/金星/火星）→ `tts.py`（`TARGET = None`，按正常语速 +4% 合成，WordBoundary 逐词时间）→ `timeline.json` / `narration.wav` / SRT → `render.mjs` 加载 `scenes-a…f.mjs`（14 个场景）+ `bodies.mjs`（太阳与带表情的行星）+ `props.mjs`（温度计、气泡、领奖台、抢答器、仪表、分子、微生物、条件图标）→ 逐帧 RGBA 管道送 ffmpeg；`audio.mjs` 程序配乐/音效侧链混音 → MP4。整体骨架（`lib.mjs`、`render.mjs`、`audio.mjs`、`tts.py`）复制自 [[solar-system-scale]]，并新增片尾品牌卡。

## 2. 教学目标与课标对齐

- **curriculum**：人教版必修第一册 第一章第一节 地球的宇宙环境（地球上存在生命的条件）。课标1.1：运用资料，描述地球所处的宇宙环境。
- **based_on**：`geo-story-studio/storage/builds/douyin-earth-life-conditions/v002`——沿用其科学边界：“距离合适不保证生命”“现今大气适合人类呼吸，但不是所有生命都需要氧气”“宜居不等于发现生命”（`script.json` 原字段写的是本机绝对路径）。
- **核心知识点**：地球存在生命的条件——外部（稳定的太阳光照、安全的宇宙环境）+ 自身（适宜的温度、适合生物呼吸的大气、液态水）；以及“冷热不只看距离，还看大气”。
- **叙事结构**（实际顺序）：

| 环节 | 段落 | 内容 |
| --- | --- | --- |
| 情境导入 | hook, open, e-claim | 扫描太阳系只有地球亮起“生命”；“宜居星球面试会”开场，地球抢答“距离刚刚好” |
| 认知冲突 | quiz … mars-cold | 抢答“最热的行星”：水星抢答失败，金星 467 ℃ 胜出；金星更远却更热→浓厚 CO₂ 温室效应；水星无大气昼夜 430/−180 ℃；火星大气稀薄 −153 ℃ |
| 原理拆解（三关） | e-temp … water-e | 第一关温度（距离适中 + 大气保温约 15 ℃，无大气约 −18 ℃；自转公转周期适中）；第二关大气（质量体积适中、引力吸住气体；金星气压 90 多倍却全是 CO₂；地球现为氮氧为主，也有微生物不需氧）；第三关液态水（火星多为冰；地球海洋约 71%） |
| 外部条件 | outer | 太阳稳定、大小行星各行其道 |
| 结论 + 边界 | e-proud, combo | 条件同时满足；“宜居，不等于一定有生命” |
| 答题 + 迁移 | exam, transfer | 两栏答题框架；听到“发现宜居行星”就逐条核对 |
| 总结 | end | “地球宜居，靠的是条件组合拳！” |

- **数字与出处**（`script.json` `numbers`）：金星约 467 ℃、气压约 93 倍（NASA Venus Facts）；水星昼 430 / 夜 −180 ℃，只有稀薄外逸层（NASA Mercury Facts）；火星约 20 / −153 ℃、水多为冰（NASA Mars Facts）；地球平均约 15 ℃、无温室气体约 −18 ℃（NOAA GML 教学资料）；氮 78% / 氧 21%、海洋约 71%（NASA Facts About Earth）；生命条件取教材。
- **台词简化**：水星台词“我没大气”，`numbers` 注明实际有“稀薄的外逸层”——角色口语化处理，旁白未补充说明。

## 3. 技术与依赖

| 类别 | 内容 | 证据 |
| --- | --- | --- |
| 渲染 | `@napi-rs/canvas` 声明 `^1.0.11`，lock 解析 **1.0.13** | `package.json`；`package-lock.json` |
| npm scripts | npm init 默认 `test`，无实际脚本 | `package.json` |
| Python | edge-tts、aiohttp、fontTools；无 requirements，版本**未记录** | `tts.py`；`fonts.py` |
| 字体 | 缺字体自动跑 `fonts.py`，回退 Arial Rounded / Arial Unicode / Hiragino | `lib.mjs:11-22` |
| ffmpeg | 旁白 adelay/amix/highpass/loudnorm；合成 sidechaincompress/alimiter/libx264/aac | `tts.py`；`render.mjs:191-201` |

**声线**（全局 `rate: 4`，叠加到各角色）：

| 角色 | voice | 角色 rate / pitch | 实际 rate | 句数 |
| --- | --- | --- | --- | --- |
| `zh` 主持/旁白 | zh-CN-XiaoxiaoNeural | 0 / 0 | +4% | 14 |
| `earth` 地球 | zh-CN-YunxiNeural | +6 / +2 Hz | +10% | 4 |
| `mercury` 水星 | zh-CN-YunjianNeural | +14 / +4 Hz | +18% | 2 |
| `venus` 金星 | zh-CN-XiaoyiNeural | +2 / −2 Hz | +6% | 2 |
| `mars` 火星 | zh-CN-YunxiaNeural | 0 / 0 | +4% | 2 |

## 4. 镜头与时间轴

时间取自 `build/timeline.json`；场景统一在下一段落开始前 0.2 s 切换，交叉淡化 0.3 s。

| 场景 | 约时间 (s) | 段落 | 画面要点 | 模块 |
| --- | --- | --- | --- | --- |
| hook | 0–4.5 | hook | 太阳系一排行星，绿色扫描束在说到“生命”时扫到地球并点亮 | `scenes-a.mjs:10` |
| stage | 4.5–12.4 | open, e-claim | “宜居星球面试会”横幅、聚光灯、四位选手站领奖台 | `scenes-a.mjs:100` |
| quiz | 12.4–25.7 | quiz, mer-guess, ven-win | 抢答卡落下、水星按抢答器、金星“错！”夺冠（buzz/sizzle 音效） | `scenes-b.mjs:8` |
| greenhouse | 25.7–32.4 | ven-why | 太阳–水星–金星按 0.39/0.72 AU 比例排开（1 AU=1000 px），“更远”括号与 430/467 ℃ 标签，CO₂ 温室 | `scenes-b.mjs:60` |
| swing | 32.4–42.2 | mer-swing, mars-cold | 水星昼夜温度计摆动；火星发抖打喷嚏 | `scenes-b.mjs:124` |
| earthTemp | 42.2–51.4 | e-temp | 阳光箭头 + 大气“毯子”与返回的红外波浪箭头 → 15 ℃；撤掉大气后冻成 −18 ℃ | `scenes-c.mjs:9` |
| periods | 51.4–58.6 | e-period | 自转、公转适中 → 昼夜/季节温差小；“第一关 √” | `scenes-c.mjs:80` |
| gravity | 58.6–64.7 | atm | 引力把气体聚成大气层 | `scenes-d.mjs:9` |
| breathe | 64.7–81.2 | ven-brag, e-reply, atm-mix | 金星气压仪表爆表、但全是 CO₂ 分子；地球氮氧分子 + 不需氧的微生物 | `scenes-d.mjs:36` |
| water | 81.2–93.2 | water, mars-ice, water-e | 火星冰晶 vs 地球液态海洋约 71% | `scenes-e.mjs:25` |
| outer | 93.2–101.7 | outer | 稳定的太阳、各行其道的行星轨道 | `scenes-e.mjs:105` |
| combo | 101.7–110.5 | e-proud, combo | 五个条件图标锁到地球周围；“宜居 ≠ 一定有生命” | `scenes-f.mjs:10` |
| exam | 110.5–120.6 | exam | 两栏答题卡：外部 / 自身 | `scenes-f.mjs:56` |
| transfer | 120.6–125.4 | transfer | “发现宜居行星”新闻标题逐项打勾核对 | `scenes-f.mjs:103` |
| finale + 品牌卡 | 125.4–132.4 | end | 地球“组合拳”；`CARD_T = E('end') + 0.5 ≈ 129.6 s` 起片尾卡 | `scenes-f.mjs:140`；`render.mjs:50, 106-126` |

6 个场景模块共 14 个场景（另有 `bodies.mjs`、`props.mjs` 两个绘图库，`props.mjs` 294 行是本组最大的道具库之一）。

## 5. 关键算法与实现

> 视频**讲的知识**是地球存在生命的条件与温室效应；以下是**制作算法**，不是气候或辐射传输模拟。

### 5.1 词级时间驱动“面试”节奏（`lib.mjs:36-45`、各场景）

所有出场、抢答、温度计数值、打勾都锚在词上，例如扫描束 `lerp(150, 1120, prog(t, at('hook','人类'), at('hook','生命') + 0.9))`（`scenes-a.mjs:11-17`）让光束在说到“生命”时正好扫到地球；`earthTemp` 用 `at('e-temp','大气保温')` 把“毯子”盖上、`at('e-temp','没有大气')` 再撤掉并结冰（`scenes-c.mjs:9-14`）：

```js
const on = easeOut(prog(t, tB - 0.2, tB + 0.4)) * (1 - easeIO(prog(t, tOff - 0.1, tOff + 0.5)));
const frozen = easeIO(prog(t, tOff, t18 + 0.3));
```

### 5.2 群戏角色系统：声线 + 表情 + 字幕标签

- 声线在 `script.json` `voices` 里按角色给 rate/pitch/`tag`，`tts.py` 叠加全局 rate 合成；字幕胶囊按 `TL.voices[seg.voice].tag` 显示“地球/水星/金星/火星”标签，颜色表 `SPEAKER`（`render.mjs:67`）。
- `bodies.mjs` 的 `face()` 新增 `cold` 表情：不说话时嘴巴画成按 `sin(40t)` 抖动的折线（diff vs [[solar-system-scale]] `bodies.mjs:46-50`），配合火星“阿嚏”。
- 口型 `talkLevel(t, ids)` 在角色词区间内开合（`lib.mjs` 末尾）。

### 5.3 片尾品牌卡与双 logo 互斥（`render.mjs:49-56, 98-126, 165-168`）

```js
const CARD_T = E('end') + 0.5;
const a = easeOut(prog(t, 0.3, 1.0)) * (1 - prog(t, CARD_T - 0.3, CARD_T));   // 角标淡出
...
const w = 760, h = (w * sh) / sw, sc = lerp(0.9, 1, p);                        // 中央大 logo
txt(ctx, '宜居，靠的是条件组合', W / 2, 1080 + (1 - q) * 30, { size: 64, color: C.gold });
```

品牌卡先在自身 alpha 下重画背景盖住场景，再画 760 px logo + 标语“宜居，靠的是条件组合”与副标“温度 · 大气 · 液态水 · 光照 · 安全环境”；页脚资料行“资料：NASA · NOAA · 人教版地理必修第一册”从 exam 开始显示、在品牌卡前淡出；最后 0.6 s 淡出到黑。这是本组 5 片中与现行 AGENTS 第 4 节最吻合的实现（760 px、角标同步淡出）。

### 5.4 TTS 提速试探后回到正常语速（缓存证据）

按 `sha1(voice|rate|pitch|text)[:16]` 复算缓存键，`build/tts_cache` 里 24 句各有 **四套**：+4%（19:10）、+26%（19:10–19:11）、+28%（19:11–19:12）、+17%（19:12–19:13）。说明当时 `tts.py` 设过时长上限并多次提速；19:31:03 `tts.py` 被改成 `TARGET = None  # no length cap: keep the natural speaking rate…`，6 秒后 timeline 以 **+4%**（全部命中缓存）生成。**推断**：用户在这一步要求不为压时长提速，次日（10-26 21:03）AGENTS.md 以 `35ad4da` “drop narration length cap and keep normal speech rate” 把它写成项目规则。当时 TARGET 的具体值未保留。

## 6. 声音与字幕

| 项 | 内容 | 来源 |
| --- | --- | --- |
| 合成参数 | `TARGET=None`、`LEAD_IN=0.6`、`GAP=0.45`（逐句覆盖）、`TAIL=3.2`（“room for the closing brand card”）；字幕块 ≤14 字 | `tts.py:32-36` |
| 缓存 / 429 | sha1 前 16 位缓存；429 退避 8/16/24/32 s | `tts.py` |
| 旁白混音 | adelay → amix → highpass 80 → loudnorm −16 LUFS / −1.5 dBTP / LRA 11；支持每角色 `fx`（本片未用） | `tts.py:181-186` |
| 配乐 | 与 [[solar-system-scale]] 同一 `audio.mjs`（hash `9e3eeae951b5`）：112 BPM 拨弦配乐 | `audio.mjs` |
| 音效 | 抢答 `buzz`、高温 `sizzle`、金星气压 `alarm`、打勾 `ding` 等按词触发 | `scenes-b.mjs:171-179`；`scenes-d.mjs:130-132` |
| 总混音 | 音乐 0.75 侧链压缩，音效 0.5，`alimiter 0.93`；总线无 loudnorm | `render.mjs:197-198` |
| 烧录字幕 | 深色胶囊 y=1500，字号从 58 逐级缩小到适宽，角色标签着色 | `render.mjs:69-94` |

本次未试听、未测响度。

## 7. 迭代与问题

| 事项 | 证据 | 说明 |
| --- | --- | --- |
| 提速 → 撤回上限 | §5.4 | 本片是项目“时长不设上限、不提速”规则的**推断**源头 |
| `tts.py` docstring 残留 | `tts.py:4` “e.g. Neptune's far-away echo” | 从 solar-system-scale 复制，本片无海王星 |
| `based_on` 写本机绝对路径 | `script.json` | 本文改写为相对路径记录；源文件未改 |
| `package.json` 为 npm init 模板 | `package.json` | 无 render/check 脚本 |
| 水星“没大气”的口语化 | `script.json` `mer-swing` vs `numbers` | 与“稀薄外逸层”略有出入；教材层面可接受，未在旁白补充 |
| “示意”声明 | `render.mjs:96` 注释 | 未放；页脚改为资料来源，符合 AGENTS 第 5 节 |

## 8. 复现入口

**本次均未执行**：

```bash
cd earth-life-conditions
python3 -m venv .venv && .venv/bin/pip install edge-tts fonttools   # 版本未记录
npm install
.venv/bin/python tts.py            # → build/narration.wav, build/timeline.json, out/宜居星球面试会.srt
node render.mjs --still 30 112     # 可选预览 → build/still_<t>.png
node render.mjs                    # → out/宜居星球面试会.mp4
```

当前 +4% 一套缓存齐全，**推断**可离线重建旁白。

## 9. 参考来源

| 来源 | 用途 | 状态 |
| --- | --- | --- |
| http://www.pep.com.cn | 教材：生命存在条件 | 项目文件中提及，本次未访问 |
| https://science.nasa.gov/venus/venus-facts/ | 金星温度、气压 | 项目文件中提及，本次未访问 |
| https://science.nasa.gov/mercury/facts/ | 水星昼夜温差 | 项目文件中提及，本次未访问 |
| https://science.nasa.gov/mars/facts/ | 火星温度、冰 | 项目文件中提及，本次未访问 |
| https://science.nasa.gov/earth/facts/ | 大气组成、海洋比例 | 项目文件中提及，本次未访问 |
| https://science.nasa.gov/exoplanets/habitable-zone/ | 宜居带概念（迁移段） | 项目文件中提及，本次未访问 |
| https://gml.noaa.gov/infodata/lesson_plans/The%20Greenhouse%20Effect-%20Fact%20of%20Theory.pdf | 15 ℃ / −18 ℃ | 项目文件中提及，本次未访问 |
| `geo-story-studio/storage/builds/douyin-earth-life-conditions/v002` | 改编来源与科学边界 | 本次未打开 |

相关：[[solar-system-scale]]（同骨架）、[[atmosphere-insulation]]（大气保温/逆辐射）。

## 10. 证据与限制

| 文件 | 行数 | sha256[:12] | 读取程度 |
| --- | --- | --- | --- |
| `earth-life-conditions/script.json` | 59 | `df37516f9f8c` | 全文 |
| `earth-life-conditions/tts.py` | 210 | `f9ececc1f75b` | 与 meteor-annual 版 diff |
| `earth-life-conditions/render.mjs` | 214 | `10c83ee8bf04` | 与 solar-system-scale 版 diff + 关键行 |
| `earth-life-conditions/lib.mjs` | 243 | `ab9219509664` | 与 solar-system-scale 版 diff（只改注释与颜色） |
| `earth-life-conditions/scenes-a…f.mjs` | 145/185/139/138/171/191 | `77bae7644a0b` / `7bbdfd056226` / `9e81b868cb19` / `f63cbed7fdad` / `09293036921f` / `ac02934a3eb7` | 场景表、头注释、hook/greenhouse/earthTemp 片段、events |
| `earth-life-conditions/props.mjs` / `bodies.mjs` | 294 / 162 | `bd2c921baf0c` / `b1c4b517de95` | 函数清单；bodies 与前作 diff |
| `earth-life-conditions/audio.mjs` / `fonts.py` / `package.json` | 229 / 37 / 17 | `9e3eeae951b5` / `a6ac7264e548` / `855538b3632f` | 与前作同 hash / 全文 |
| `out/宜居星球面试会.mp4` | — | 24,547,639 B | 仅证据包 ffprobe |

限制：制作对话不可读；旧 TARGET 值、预览帧、用户意见**未记录**；Git 中随 `1aa541f`（2026-10-26，5 片合并提交）入库。
