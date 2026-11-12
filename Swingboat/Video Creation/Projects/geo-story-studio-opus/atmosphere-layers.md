---
type: video-production-dossier
project: geo-story-studio-opus
video_id: atmosphere-layers
title: 坐一趟大气电梯：越往上，为什么先变冷又变暖？
curriculum: 人教版必修第一册 第二章第一节 大气的组成和垂直分层（对流层、平流层的气温垂直变化及其原因）；课标1.4
captured: '2026-11-12'
status: 成片存在（竖屏 1080×1920，211.7 s）；技术检查/视觉检查/用户验收未记录
source_fingerprint: e2c4137ae39f21ff1dcce213967c856d7895e6d173f5361831a594cee323bd29
tags:
- video-creation
- production-capture
- geography-teaching
- canvas-frame-render
- edge-tts
---

# 坐一趟大气电梯 · 可缩放的「高度—温度」定性曲线 + 电梯广播声线（Canvas 逐帧 + 多角色 edge-tts）

<!-- capture-source-sha256:e2c4137ae39f21ff1dcce213967c856d7895e6d173f5361831a594cee323bd29 -->

本次为材料回溯与归档，结论来自项目文件（`script.json`、`tts.py`、`render.mjs`、`lib.mjs`、`lift.mjs`、`audio.mjs`、`chars.mjs`、`props.mjs`、`scenes-a…e.mjs`、`build/timeline.json`、`out/` 清单与 ffprobe）。按时间戳**推断**对应的宿主会话为 `51611128-e0a5-4ca4-b452-9b9f751a2c0c`（创建于 2026-10-31 16:25，早于 `script.json` 修改时间 16:27），只按元数据找到，**对话内容无法读取**（宿主返回空 transcript）。本轮未运行任何项目脚本，未打开视频或音频。

## 1. 当前结论与制作路线

**状态：成片存在**——`out/坐一趟大气电梯.mp4` 与同名 `.srt`；视觉检查、试听、用户验收**未记录**。

| 项 | 值 | 来源 |
| --- | --- | --- |
| 分辨率 / 画幅 | 1080×1920，9:16 竖屏 | ffprobe；`lib.mjs:7` |
| 帧率 / 帧数 | 30 fps；约 6352 帧 | ffprobe；`Math.round(211.718 × 30)`（`render.mjs:217`） |
| 时长 | 211.733 s（README 记 3:32） | ffprobe；`timeline.json` `duration: 211.718` |
| 视频 / 音频 | H.264（libx264 CRF 19，preset medium，yuv420p，faststart）/ AAC 48 kHz 立体声 192 kbps | ffprobe；`render.mjs:214-215` |
| 文件大小 | 41,979,054 字节（约 42.0 MB，平均约 1.59 Mbps） | 证据包 |
| 字幕 | `out/坐一趟大气电梯.srt` 105 条 + 画面烧录 | SRT 计数；`render.mjs:83-108` |
| 其他版本 | 无 | `out/` 清单 |

管线与 [[air-composition]] 是同一套模板（`tts.py` 只有 docstring 和 `TITLE` 不同；`render.mjs` 只有注释、背景微粒配色、说话人颜色、资料页脚和品牌卡标语不同）：`script.json`（33 段、4 个角色）→ `tts.py`（edge-tts + WordBoundary）→ `narration.wav` / `timeline.json` / SRT → `render.mjs` 加载 `scenes-a…e`（13 个场景），共享 `lift.mjs` 的电梯—曲线—温度计套件 → Canvas 逐帧 → ffmpeg。文件时间：`script.json` 16:27 → TTS 产物 16:41 → `render.mjs` 16:45 → 场景文件 16:47–16:57（`lift.mjs` 16:52）→ 成片 17:12。

## 2. 教学目标与课标对齐

- **课标出处**（`script.json:3`）：必修第一册 第二章第一节，对流层、平流层的气温垂直变化及其原因；课标 1.4。
- **核心知识点**：对流层气温随高度增加而降低（地面是对流层大气主要的直接热源 + 上升空气膨胀降温）；平流层下部气温变化小、约 30 千米以上明显升温（臭氧吸收太阳紫外线）；两层的运动特征。
- **讲解结构**（`timeline.json`）：
  - 导入：小宇「山顶离太阳更近，怎么反而更冷？」→ 1.5 亿千米 vs 几千米（`hook`/`far`）→ 坐大气电梯到约 50 千米（`ride`）。
  - 原理一·对流层：广播「第一站」→ 温度计下降、平均每 100 米约降 0.6℃ → 地面是主要直接热源 → 膨胀冷却 → 对流显著、天气复杂（`ding1`…`weather`）。
  - 对流层顶：高度随纬度变化（低纬 17–18、中纬 10–12、高纬 8–9 千米），低纬受热多对流旺（`ding2`…`hot`）。
  - 原理二·平流层：「温度计怎么又涨了」→ 臭氧自述吸收紫外线 → 越往上越先迎上更强紫外线 → 上暖下冷、水平运动、利于高空飞行 → 臭氧顺带保护生物（`ding3`…`shield`）；高层大气只预告（`beyond`）。
  - 迁移：夜间逆温；课本曲线是平均状况（`back`/`invert`/`avg`）；山顶气温计算题 20℃、高 3000 米 → 约 2℃（`quiz`/`ans`/`ok`）。
  - 总结：一句话总结 + 易错点「对流层冷不是因为离太阳远、平流层暖也不是因为离太阳近」+ 回扣开场（`recap`/`exam`/`end`）。
- **科学口径边界**：`based_on` 指向 `geo-story-studio/storage/builds/douyin-atmosphere-layers/v002`，口径包括：温度轴只定性、不画℃刻度；对流层顶高度可变；平流层下部气温变化小、约 30 千米以上明显升温；臭氧吸收而非反射，也不表示紫外线全部消失；膨胀冷却只在忽略热交换时成立；平均结构之外有逆温；约 50 千米以上不画成一直升温。代码中的对应处理见 §5.2，以及：温度计「Only words, no degree ticks」（`lift.mjs:134`）；画面上对流层顶标「对流层顶（高度可变）」（`lift.mjs:33`）；紫外线箭头「each ends on an ozone molecule (absorbed, not bounced)」（`scenes-c.mjs:156`）；「a thin leftover ray still gets through」（`scenes-d.mjs:69`）。
- **数字与出处**（`script.json:13-19`）：0.6℃/100 m（教材；ICAO 标准大气 6.5℃/km，注明是平均状况）；对流层顶平均高度三档（教材、UCAR）；平流层顶约 50 千米（教材写 50–55 千米）；约 30 千米以上明显升温（教材图示、UCAR 虚拟探空标准大气表）；日地平均距离约 1.5 亿千米（NASA《Astronomical Unit》）。

## 3. 技术与依赖

| 类别 | 内容 | 证据 |
| --- | --- | --- |
| 渲染 | Node.js ESM + `@napi-rs/canvas` 声明 `^1.0.11`，锁定 1.1.0（darwin-arm64 1.1.0） | `package.json`；`package-lock.json` |
| npm scripts | 仅 npm 默认 `test` 占位 | `package.json` |
| TTS | Python `edge-tts` + `aiohttp`；版本未记录 | `tts.py:16-18` |
| 字体 | `fonts.py`（fontTools）抽取圆体、苹方；与其他视频的 `fonts.py` 哈希相同（`a6ac7264e548`） | 证据包 |
| ffmpeg | 旁白：`adelay`/`amix`/`highpass`/`loudnorm`，并支持每个声音的 `fx` 滤镜；总混音：`sidechaincompress`/`amix`/`alimiter` | `tts.py:177-192`；`render.mjs:205-216` |

| 角色 key | 标签 | 声音 | rate（角色 + 全局 4） | pitch | fx |
| --- | --- | --- | --- | --- | --- |
| `zh` | 旁白 | zh-CN-XiaoxiaoNeural | +4% | 0 | — |
| `kid` | 小宇 | zh-CN-YunxiNeural | +10% | +2 Hz | — |
| `lift` | 电梯广播 | zh-CN-YunyangNeural | +4% | 0 | `highpass=f=260,lowpass=f=3800` |
| `o3` | 臭氧 | zh-CN-XiaoyiNeural | +12% | +4 Hz | — |

`build/tts_cache/` 66 个文件 = 33 段 × (mp3 + json)，只有一批语速；`TARGET = None`，`timeline.json` `rate: 4`。

## 4. 镜头与时间轴

时间来自 `build/timeline.json`；场景边界为下一场景首段 `S(id) − 0.2`（首个切点 −0.15），交叉淡化 `XF = 0.3`。

| # | 场景 | 约时间 (s) | 旁白要点 | 画面 | 模块 |
| --- | --- | --- | --- | --- | --- |
| 1 | `hook`（含 `scale`） | 0–11.7 | 山顶更冷？1.5 亿千米 vs 几千米 | 小宇在雪山顶发抖 → 距离比例笑点 | `scenes-a.mjs:9-86` |
| 2 | `ride` | 11.7–19.2 | 坐大气电梯到约 50 千米 | 小宇登上电梯、盯紧温度计 | `scenes-a.mjs:88-121` |
| 3 | `trop` | 19.2–37.0 | 第一站对流层；0.6℃/100 m；为什么 | 电梯上升、曲线弯向冷端、0.6℃ 数字卡 | `scenes-a.mjs:123-156` |
| 4 | `heatScene` | 37.0–54.8 | 太阳晒地面、地面再加热空气 | 短波穿过大气、地面热环、长波上传 | `scenes-b.mjs:22-92` |
| 5 | `expandScene` | 54.8–64.1 | 上升 → 气压低 → 膨胀 → 降温 | 气团上升变大、受压箭头变小 | `scenes-b.mjs:93-137` |
| 6 | `convectScene` | 64.1–78.6 | 对流显著、天气复杂 | 对流环流 + 云雨雪雷电图标 | `scenes-b.mjs:138-202` |
| 7 | `topScene` | 78.6–98.1 | 对流层顶高度随纬度变化 | 抵达楼顶 → 纬度剖面三档高度 | `scenes-c.mjs:9-95` |
| 8 | `stratScene` | 98.1–113.4 | 第二站平流层；先平后升 | 曲线由 15 km 缩放到 52 km，电梯继续上行，区间括注 | `scenes-c.mjs:96-139` |
| 9 | `ozoneScene` | 113.4–131.6 | 臭氧吸收紫外线变成热 | 紫外线波形箭头止于臭氧分子，平流层上部变暖色 | `scenes-c.mjs:140-197` |
| 10 | `flatScene`（含保护生命） | 131.6–151.9 | 上暖下冷、水平运动、飞机；臭氧保护生物；高层大气预告 | 对流层云顶起伏、垂直翻滚被划掉、水平流线、飞机；一缕残余紫外线 | `scenes-d.mjs:9-97` |
| 11 | `invertScene` | 151.9–170.6 | 晴朗无风夜晚逆温；曲线是平均状况 | 贴地冷空气、烟囱烟雾在暖盖下平铺；虚线（通常）vs 今夜曲线 | `scenes-d.mjs:98-212` |
| 12 | `quizScene` | 170.6–183.7 | 20℃，高 3000 米 → 约 2℃ | 计算题与作答 | `scenes-e.mjs:9-60` |
| 13 | `recapScene` | 183.7–211.7 | 总结 + 易错点 + 回扣山顶 | 两行总结卡 → 「地面 = 大暖炉」回扣画面 | `scenes-e.mjs:61-135` |
| — | 品牌卡 | 208.7–211.7 | 无旁白 | 居中 logo + 「用地理的眼光，看懂头顶的天空 / 对流层越高越冷 · 平流层越高越暖」 | `render.mjs:119-140` |

## 5. 关键算法与实现

> 区分：视频讲的是大气垂直分层的**教材结论**；下面是制作用的**绘图与时间轴算法**，`temp(z)` 只是定性形状，不是大气模式计算。

### 5.1 共用管线（与 [[air-composition]] 相同）

- WordBoundary 逐词时间 → `lib.mjs:36-46` `word()/at()` 查词锚点；SRT 与烧录字幕用同一批时间。
- `tts.py:47-63`：`sha1(voice|rate|pitch|text)[:16]` 缓存 + 429 退避重试（8/16/24/32 s）；`tts.py:20-26` 替换 `WSS_URL`。
- `render.mjs:159-188`：场景交叉淡化 + 推拉缩放，`shakeAt()` 盖章震屏，角标 logo（64, 76, 宽 290）与 `CARD_T = E('end') + 0.3` 品牌卡。

### 5.2 定性温度廓线与可缩放图表（`lift.mjs:6-29`）

```js
export const TROPO = 12;            // Representative tropopause height for drawing only — on screen the top is always called "可变".
export function temp(z) {
  if (z <= TROPO) return 15 - 6 * z;
  if (z <= 20) return -57;
  if (z <= 32) return -57 + (z - 20);
  return Math.min(-3, -45 + 2.8 * (z - 32));
}
export const warmth = (z) => clamp((temp(z) + 62) / 82);
export const yOf = (z, zmax) => G.bot - (z / zmax) * (G.bot - G.top);
export const xOf = (z) => lerp(G.x0 + 50, G.x1 - 40, warmth(z));
```

- 分段函数按标准大气形状：对流层每千米降 6（与 0.6℃/100 m 一致）、12–20 km 等温、20–32 km 缓升、32 km 以上明显升温并以 `Math.min(-3, …)` 封顶——对应口径「约 50 千米以上不画成一直升温」。
- 数值只在内部用来换算横坐标 `warmth ∈ [0,1]` 与颜色 `heat(k) = mix(C.cold, C.warm, k)`，画面不标℃（「The scale is internal and never labelled」，`lift.mjs:10`）；纵轴高度刻度按 `zmax ≤ 20 ? 5 : 10` 千米显示（`lift.mjs:53-60`）。
- 到平流层时 `zmaxAt(t) = lerp(15, 52, easeIO(…))` 在 1.5 s 内把图表从 15 km 拉远到 52 km；电梯高度 `zStrat(t)` 分三段，分别锚在「哎」「30千米」等词上，曲线追踪点与电梯同步（`scenes-c.mjs:97-103`）。

### 5.3 电梯广播声线：每个声音可带 ffmpeg 滤镜

`script.json:10` 给 `lift` 角色设 `"fx": "highpass=f=260,lowpass=f=3800"`；`tts.py:181-182` 在 `adelay` 前插入：

```python
fx = script["voices"][seg["voice"]].get("fx")
filters.append(f"[{i}:a]aresample=48000,{fx + ',' if fx else ''}adelay={ms}|{ms}[a{i}]")
```

带通处理让「叮！第一站，对流层」听起来像轿厢喇叭。配合 `audio.mjs:160-174` 新增的两种音效：`chime`（钟琴 + 马林巴 C6→G5「叮咚」）和 `hum`（70→100 Hz 低频，随轿厢上行升调），由 `scenes-a.mjs:176-177`、`scenes-c.mjs:205-216`、`scenes-d.mjs:225` 按词触发。

### 5.4 电梯井与轿厢（`lift.mjs:98-133`）

井壁刻度按 `((z / zmax) * 900) % 26` 循环滚动，制造「轿厢在动」的视差；轿厢纵坐标与图表共用 `yOf(z, zmax)`，所以电梯、曲线追踪点、右侧温度计读数始终指向同一高度。

## 6. 声音与字幕

| 项 | 内容 | 来源 |
| --- | --- | --- |
| 声音 | 旁白 / 小宇 / 电梯广播（带通滤镜）/ 臭氧，见 §3 | `script.json:7-12` |
| 旁白混音 | `aresample 48k`（+ 角色 `fx`）→ `adelay` → `amix normalize=0` → `highpass=f=80` → `loudnorm=I=-16:TP=-1.5:LRA=11` | `tts.py:177-192` |
| 时间参数 | `LEAD_IN 0.6`、`GAP 0.45`（逐段 0.3–0.6）、`TAIL 3.2` | `tts.py:33-36` |
| 配乐 | 程序合成，**106 BPM**（air-composition 为 112），和弦改为 C–Am–F–G（I–vi–IV–V），种子 42，淡入 1.2 s / 淡出 2.5 s | `audio.mjs:189-225` |
| 音效 | 22 种（在 air-composition 的 20 种基础上加 `chime`、`hum`） | `audio.mjs:78-180` |
| 总混音 | 配乐 ×0.75 侧链压缩、音效 ×0.5、`alimiter 0.93` | `render.mjs:209-212` |
| 字幕 | SRT 105 条；烧录 `SUB_Y = 1500`，说话人标签：小宇蓝、电梯广播橙红、臭氧青 | `render.mjs:23, 81` |

未试听、未测响度。

## 7. 迭代与问题

| 事项 | 证据 | 说明 |
| --- | --- | --- |
| 与 [[mountain-cold]] 选题重叠 | 本片导入「山顶离太阳更近，怎么反而更冷」、`quiz` 20℃→2℃、结尾「离地面这个大暖炉远了」；mountain-cold 整片讲同一问题（25℃→约 7℃，「暖气片在脚下」） | 两片同日相隔约 1 小时制作（本片 `script.json` 16:27，mountain-cold 17:27）；是否有意做成系列**无法确认** |
| 与 [[stratosphere-ozone]] 共享代码 | `audio.mjs`、`chars.mjs` 两片 sha256 完全相同（`50c6a7a5dc02`、`ccf861832fc8`），`tts.py` 只差 `TITLE` | stratosphere-ozone 很可能以本片为模板复制（**推断**） |
| `based_on` 写成本机绝对路径 | `script.json:4` | 本档案改写为 `geo-story-studio/storage/builds/douyin-atmosphere-layers/v002`；源 build 未读取 |
| 图表的高度刻度 vs「温度轴只定性」 | `lift.mjs:52-60` 画千米刻度；温度轴无刻度 | 与口径一致：只禁止℃刻度 |
| 规则符合情况（代码中存在） | 角标 logo、品牌卡、资料页脚「人教版地理必修第一册 · UCAR · NOAA」（`render.mjs:115`）；未检出「示意」类声明 | 画面效果未核对 |
| TTS 与渲染间隔 | `script.json` 16:27 → `timeline.json` 16:41 | 中间 14 分钟的过程未记录（可能是文案修改后重跑 TTS，**推断**） |

README 目录（`README.md:57`）已收录，记 3:32，与 ffprobe 一致。

## 8. 复现入口

命令来自 AGENTS.md「常用命令」与 `render.mjs:2-3`，**本次均未执行**：

```bash
cd atmosphere-layers
npm install
python3 -m venv .venv && .venv/bin/pip install edge-tts   # fonts.py 另需 fontTools
.venv/bin/python tts.py              # → build/narration.wav、build/timeline.json、out/坐一趟大气电梯.srt
node render.mjs --still 30 120       # 预览帧
node render.mjs                      # → out/坐一趟大气电梯.mp4
```

修改入口：温度廓线形状 → `lift.mjs:7-16`；图表缩放与电梯路径 → `scenes-c.mjs:97-103`；广播滤镜 → `script.json:10`；音效 → `audio.mjs:160-174`。

## 9. 参考来源

| 来源 | 用途 | 状态 |
| --- | --- | --- |
| 人教版必修第一册 第二章第一节 | 分层依据、对流层与平流层特征 | `script.json:21`，无 URL |
| <https://scied.ucar.edu/learning-zone/atmosphere/troposphere> | 温度向上降低、层顶随纬度季节变化 | 项目文件中提及，本次未访问 |
| <https://scied.ucar.edu/learning-zone/atmosphere/stratosphere> | 顶部约 50 千米；臭氧吸收紫外使平流层增温 | 项目文件中提及，本次未访问 |
| <https://www.noaa.gov/jetstream/clouds/how-clouds-form> | 上升膨胀冷却（不与外界交换热量） | 项目文件中提及，本次未访问 |
| <https://www.weather.gov/lmk/inversion> | 逆温与夜间近地逆温 | 项目文件中提及，本次未访问 |
| <https://www.antarctica.gov.au/about-antarctica/ice-and-atmosphere/atmosphere/from-the-ground-to-space/atmospheric-regions/> | 平流层顶以上温度又降低 | 项目文件中提及，本次未访问 |
| ICAO 国际标准大气；NASA《Astronomical Unit》；UCAR 虚拟探空活动 | 数字出处 | `script.json:14-18` 仅列名称 |
| `geo-story-studio/storage/builds/douyin-atmosphere-layers/v002` | 改编来源与口径 | 本次未读取 |

## 10. 证据与限制

| 文件 | 行数 | sha256[:12] | 读取程度 |
| --- | --- | --- | --- |
| `atmosphere-layers/script.json` | 64 | `13b422d285c6` | 全部字段（经 timeline 读出全部 33 段文字） |
| `atmosphere-layers/tts.py` | 210 | `b79a0e0d6652` | 与 air-composition 全文 diff（仅 3 行不同） |
| `atmosphere-layers/render.mjs` | 228 | `9d5bdc5306a9` | 与 air-composition 全文 diff |
| `atmosphere-layers/lib.mjs` | 222 | `7d14af2a9491` | 与 air-composition diff（仅注释与配色） |
| `atmosphere-layers/lift.mjs` | 184 | `eb6971a685b7` | 1-60、98-115 行与导出表 |
| `atmosphere-layers/audio.mjs` | 244 | `50c6a7a5dc02` | 与 air-composition diff |
| `atmosphere-layers/chars.mjs` / `props.mjs` | 187 / 163 | `ccf861832fc8` / `aa166a45bcb3` | 导出表与注释 |
| `atmosphere-layers/scenes-a…e.mjs` | 181/230/224/232/159 | `d2cc5f0d67c8` / `1a79d0f2ddfe` / `b92cb819d0fd` / `249c84665d69` / `3c59fd1d7b7d` | 头注释、`scenes`/`events` 表；`stratScene`、`ozoneScene` 开头 |
| `atmosphere-layers/build/timeline.json` | — | — | 全部段落时间与文字 |
| `atmosphere-layers/out/坐一趟大气电梯.mp4` | — | 41,979,054 字节 | 仅 ffprobe |

限制：宿主会话不可读；需求、预览、用户反馈未记录；未运行脚本、未看画面、未试听；依赖版本（Python、Node、ffmpeg）未记录；git 仅有批量提交 `8cbe673`（2026-11-01）。
