---
type: video-production-dossier
project: geo-story-studio-opus
video_id: afternoon-heat
title: 太阳过了正午，为什么下午还会更热？
curriculum: 人教版必修第一册 第二章第二节 大气受热过程（地面是近地面大气主要的直接热源；气温日变化）；课标1.5
captured: '2026-11-12'
status: 成片存在（竖屏 1080×1920，217.7 s）；技术检查/视觉检查/用户验收未记录
source_fingerprint: 850ace933c57dd1761402f01501f0c941d27f1307ed8fa5d6ff96104db6e79d4
tags:
- video-creation
- production-capture
- geography-teaching
- canvas-frame-render
- edge-tts
---

# 太阳过了正午，为什么下午还会更热？ · 热量接力赛 + 「浴缸」能量收支积分模型（Canvas 逐帧 + 多角色 edge-tts）

<!-- capture-source-sha256:850ace933c57dd1761402f01501f0c941d27f1307ed8fa5d6ff96104db6e79d4 -->

本次为材料回溯与归档，结论来自项目文件（`script.json`、`tts.py`、`render.mjs`、`lib.mjs`、`tub.mjs`、`chart.mjs`、`cast.mjs`、`props.mjs`、`audio.mjs`、`scenes-a…e.mjs`、`build/timeline.json`、`build/tts_cache` 文件名、`out/` 清单与 ffprobe）。按时间戳**推断**对应的宿主会话为 `6625ac36-ac9f-4ba1-a8fa-30ded5b69d35`（创建于 2026-10-31 18:59），只按元数据找到，**对话内容无法读取**（宿主返回空 transcript）。本轮未运行任何项目脚本，未打开视频或音频。

## 1. 当前结论与制作路线

**状态：成片存在**——`out/太阳过了正午，为什么下午还会更热.mp4` 与同名 `.srt`；视觉检查、试听、用户验收**未记录**。

| 项 | 值 | 来源 |
| --- | --- | --- |
| 分辨率 / 画幅 | 1080×1920，9:16 竖屏 | ffprobe；`lib.mjs:7` |
| 帧率 / 帧数 | 30 fps；约 6532 帧 | ffprobe；`Math.round(217.746 × 30)`（`render.mjs` 主循环） |
| 时长 | 217.740 s（README 记 3:38） | ffprobe；`timeline.json` `duration: 217.746` |
| 视频 / 音频 | H.264（libx264 CRF 19，preset medium，yuv420p，faststart）/ AAC 48 kHz 立体声 192 kbps | ffprobe；`render.mjs` ffmpeg 参数 |
| 文件大小 | 25,592,277 字节（约 25.6 MB，平均约 0.94 Mbps，五片中码率最低） | 证据包 |
| 字幕 | SRT 99 条 + 画面烧录 | SRT 计数；`render.mjs` `subtitles()` |
| 其他版本 | 无 | `out/` 清单 |

管线属于「B 模板」，与 [[mountain-cold]] 几乎相同（`render.mjs` 只差标题、场景文件数、`OUTRO` 锚点、背景色、说话人表和资料表；`lib.mjs` 只差首行注释；`tts.py` 只差 docstring 与 `TITLE`）：`script.json`（33 段、3 个角色）→ `tts.py` → `narration.wav` / `timeline.json` / SRT → `render.mjs` 并行导入 `scenes-a…e`（10 个场景），共享 `tub.mjs`（浴缸模型）、`chart.mjs`（定性曲线）、`cast.mjs`、`props.mjs` → Canvas 逐帧 → ffmpeg。

文件时间显示**先写了部分场景、后改文案**：`cast.mjs`/`render.mjs` 19:29 → `scenes-a` 19:31 → `scenes-b` 19:33 → `tub.mjs` 19:34 → **`script.json` 19:37** → `scenes-c` 19:38 → TTS 产物 19:39 → `chart.mjs` 19:40 → `scenes-d/e` 19:42–19:45 → 成片 19:52（详见 §7）。

## 2. 教学目标与课标对齐

- **课标出处**（`script.json:3`）：必修第一册 第二章第二节 大气受热过程（地面是近地面大气主要的直接热源；气温日变化）；课标 1.5「运用示意图等，说明大气受热过程与热力环流原理，并解释相关现象」。
- **核心知识点**：太阳辐射→地面→近地面大气的传递顺序使三个峰值错开；气温升降取决于近地面大气获得与失去热量的差，所以获得开始减少时气温仍会上升，直到收支相等。
- **讲解结构**（`timeline.json`）：
  - 导入：小林以为下午两点半的体育课会凉快，结果跑道「好烫」；太阳「我的照射早就开始减弱」→ 提出问题（`plan`…`q`）。
  - 原理一·热量接力：近地面空气主要不是被太阳直接晒热 → 太阳辐射大部分到达地面 → 地面通过地面辐射、传导和湍流把热交给空气 → 教材原话 → 小林「接力赛」→ 太阳辐射、地面温度、气温峰值依次错开（`who`…`lag`）。
  - 原理二·浴缸：「不看这一刻进来多少，而看攒下多少」→ 龙头＝获得、下水口＝失去、水位＝气温 → 上午、正午、午后、峰值、下降 → 规范表述 → 小林的零花钱类比（`why-long`…`yes`）。
  - 迁移：最低气温在日出前后；北半球大陆最热月通常是 7 月而非夏至所在的 6 月（`trans`…`stu-year`）。
  - 边界：冷空气过境、乌云会让气温提前下降；风和地表状况也会改变峰值时刻，最高气温没有固定钟点（`edge1`/`edge2`）。
  - 总结：考试常说「晴天午后两点左右（地方时）」+ 三个答题关键词 → 一句话总结 → 太阳「下午热可不能全怪我」（`exam1`…`sunend`）。
- **科学口径来源**：无 `based_on` 字段，改用 `source`（`script.json:4`）：「Geo Story Studio · douyin-afternoon-heat v003 页面、分镜与科学审校（review-001，15 项结论的建议表述）」，另列人教版必修第一册 P34–36、NOAA CPC UV Index: Diurnal Variability、FAA/NWS AC 00-6B 4.6、NASA Earth Observatory、UCAR Conduction、NOAA JetStream、NWS Raleigh。源 build 路径**推断**为 `geo-story-studio/storage/builds/douyin-afternoon-heat/v003`，本次未读取。
- **数字与表述出处**（`script.json:5-14`，键值对）：如「太阳辐射大部分穿过大气……对流层大气直接吸收的很少」（P34；审校 claim-007「不说大气完全透明」）；「照射减弱不等于立即降温」（claim-013）；「最高气温一般在午后两点左右（地方时），不是固定钟点」（claim-012/014）；「北半球大陆最热月通常 7 月、海洋 8 月」；「浴缸比喻……定性简化，不代表实测速率」（审校 heat-budget-model）。
- **地理素养**：综合思维（多因素决定峰值时刻）与地理实践力（体育课情境、零花钱类比）贯穿全片——这是对文案的归纳。

## 3. 技术与依赖

| 类别 | 内容 | 证据 |
| --- | --- | --- |
| 渲染 | Node.js ESM + `@napi-rs/canvas` 声明 `^1.0.11`，锁定 1.1.0（darwin-arm64 1.1.0） | `package.json`（`private: true`，**无 scripts**）；`package-lock.json` |
| TTS | Python `edge-tts` + `aiohttp`；版本未记录 | `tts.py:16-18` |
| 字体 | `fonts.py`（fontTools，同哈希 `a6ac7264e548`） | 证据包 |
| ffmpeg | 旁白 `adelay`/`amix`/`highpass`/`loudnorm`（B 模板不支持角色 `fx`）；总混音 `sidechaincompress`/`amix`/`alimiter` | `tts.py`；`render.mjs` |

| 角色 key | 标签 | 声音 | rate（角色 + 全局 4） | pitch |
| --- | --- | --- | --- | --- |
| `zh` | 旁白 | zh-CN-XiaoxiaoNeural | +4% | 0 |
| `stu` | 小林 | zh-CN-YunxiNeural | +10% | +2 Hz |
| `sun` | 太阳 | zh-CN-YunjianNeural | 0%（−4 + 4） | −4 Hz |

`build/tts_cache/` 68 个文件 = 34 对 mp3 + json，比 33 段多 1 对，即一句旧文案的缓存（见 §7）。`TARGET = None`；`timeline.json` `rate: 4`。

## 4. 镜头与时间轴

时间来自 `build/timeline.json`；场景边界为下一场景首段 `S(id) − 0.3`，交叉淡化 `XF = 0.4`（`render.mjs:17`），只做透明度淡化；每次切场景自动加 whoosh（`render.mjs:157`）。

| # | 场景 id | 约时间 (s) | 旁白要点 | 画面 | 模块 |
| --- | --- | --- | --- | --- | --- |
| 1 | `hook` | 0–23.9 | 以为下午凉快 → 跑道好烫 → 太阳辩解 → 提问 | 晴天操场与红色跑道（`heat` 参数让天空变暖）、指针钟 12:00 → 14:30、小林冒汗，开场哨声 | `scenes-a.mjs:10-136` |
| 2 | `path` | 23.9–48.4 | 太阳辐射大部分到达地面；地面辐射、传导、湍流；教材原话 | 太阳光束 → 地面 → 波浪形长波箭头上传；翻开的教材卡 | `scenes-b.mjs:10-98` |
| 3 | `relay` | 48.4–66.4 | 接力赛；三个峰值错开 | 太阳→地面→空气三棒接力；三条定性曲线（太阳辐射、地面温度、气温）依次登顶 | `scenes-b.mjs:99-168`；`chart.mjs:89-91` |
| 4 | `tub` | 66.4–116.1 | 攒下多少；龙头/下水口/水位；上午→正午→午后→峰值→下降 | 浴缸 + 龙头把手 + 下水口；获得/失去对比条；时间指针跟随旁白；「涨到最高」爆点 | `scenes-c.mjs:10-73`；`tub.mjs` |
| 5 | `formal` | 116.1–128.2 | 规范表述 | 教材式卡片，标题「规范表述 · 气温为什么升降」 | `scenes-c.mjs:74-92` |
| 6 | `money` | 128.2–135.6 | 零花钱类比；完全正确 | 粉色存钱罐与硬币 | `scenes-c.mjs:94-160` |
| 7 | `transfer` | 135.6–165.1 | 最低气温在日出前后；7 月最热 | 一天的气温曲线（日出最低、午后最高）+ 北半球一年中正午太阳高度与大陆气温曲线（6 月 vs 7 月） | `scenes-d.mjs:13-78`；`chart.mjs:93-101` |
| 8 | `weather` | 165.1–181.8 | 冷空气、乌云、风、地表打乱账本 | 冷锋符号、云、风等图标；盖章「没有固定钟点」 | `scenes-e.mjs:14-78` |
| 9 | `exam` | 181.8–203.4 | 午后两点左右（地方时）；三个关键词 | 答题卡；「午后两点」处 ding | `scenes-e.mjs:79-112` |
| 10 | `summary` | 203.4–217.7 | 小林总结、一句话、太阳收尾 | 总结画面 | `scenes-e.mjs:113-142` |
| — | 品牌卡（`outro`） | 215.1–217.7 | 无旁白 | 背景压暗，居中 760 px logo，「用地理的眼光，看懂一天的冷暖」，sparkle | `render.mjs:61-75, 158` |

## 5. 关键算法与实现

> 区分：视频讲的是教材的「大气受热过程 + 热量收支决定气温升降」；下面是**制作算法**。浴缸模型与曲线都是定性形状（`chart.mjs:87` 注释「no measured values」，`script.json` 注明「不代表实测速率」），不是气象观测或数值模式。

### 5.1 浴缸模型：对「获得 − 失去」做数值积分（`tub.mjs:1-21`）

```js
export const gain = (h) => Math.max(0, Math.sin((Math.PI * (h - 6)) / 12));
export const loss = (h) => (h <= 14 ? 0.35 + (gain(14) - 0.35) * ss(8, 14, h) : gain(14));
const STEP = 0.01, H0 = 8, H1 = 19;
const CUM = [0];
for (let h = H0; h < H1; h += STEP) CUM.push(CUM[CUM.length - 1] + (gain(h + STEP / 2) - loss(h + STEP / 2)) * STEP);
const PEAK = CUM[Math.round((14 - H0) / STEP)];
export function level(h) { /* 线性插值 CUM，并归一化：8 h → 0.22，14 h 峰值 → 0.86 */ }
```

- 获得（龙头）取 6–18 时的正弦，正午最大；失去（下水口）用 smoothstep 从 0.35 平滑升到 `gain(14)`，14 时以后保持不变。于是 14 时之前 `gain > loss`、14 时两者相等、之后 `loss > gain`。
- 水位 `level(h)` 是 `gain − loss` 的中点法积分（步长 0.01 h），水位峰值**由积分自然落在 14 时**，正好把「龙头变小 ≠ 水位下降」画成可验证的形状，而不是手绘一条曲线。
- 文件头注释直接说明意图：「A qualitative model in "hour of the day" keeps the picture honest」（`tub.mjs:2-3`）。

### 5.2 旁白驱动的「时钟」：词时间 → 一天中的小时（`scenes-c.mjs:10-22`）

`hourAt(t)` 用关键帧表把旁白位置映射到钟点：`am` 段 8→11.4 时、「最大」后到 12 时、`no` 段 13→13.75 时、「一样多」时正好 14 时、`fall` 段 14→16.5 时，段间 `easeIO` 插值；再由 `phase(h)` 显示「上午 / 正午 / 午后 / 傍晚」。浴缸、对比条和指针都读同一个 `h`，所以「水位最高」的画面与旁白「这就是一天中气温最高的时候」同步。

### 5.3 定性曲线库（`chart.mjs:87-101`）

- 一天：`solar(h)` 正弦、`groundT` 以 13 时为中心、`airT` 以 14 时为中心的高斯「鼓包」，三峰依次错开；`dayT(h)` 用两段余弦拼出「日出前后最低、午后最高」的完整日变化。
- 一年：`sunYear(m)` 峰在 6.2 月、`tempYear(m)` 峰在 7.2 月（余弦），表达「夏至在 6 月、北半球大陆最热月在 7 月」。
- `curve(ctx, b, f, v0, v1, p)` 按进度 p 逐步揭示曲线并返回前端点坐标，供跟随标记使用（`chart.mjs:41`）。

### 5.4 共用部分（B 模板）

- WordBoundary 逐词时间 → `lib.mjs:36-45` `word()/at()`；SRT 与字幕同源。
- `tts.py`：`sha1(voice|rate|pitch|text)[:16]` 缓存、429 退避重试（8/16/24/32 s）、`WSS_URL` 替换；`LEAD_IN 1.0`、`TAIL 2.8`。
- 角标 logo 64/76/290；`OUTRO = E('sunend') + 0.3`（`render.mjs:20`）后字幕停止（`render.mjs:81`）、角标 0.4 s 淡出、品牌卡淡入；资料页脚 `FOOT` 按段落区间切换来源（`render.mjs:109-115`）。

## 6. 声音与字幕

| 项 | 内容 | 来源 |
| --- | --- | --- |
| 声音 | 旁白 / 小林 / 太阳（低沉、不加速），见 §3 | `script.json` `voices` |
| 旁白混音 | `aresample 48k` → `adelay` → `amix normalize=0` → `highpass=f=80` → `loudnorm=I=-16:TP=-1.5:LRA=11` | `tts.py` |
| 时间参数 | `LEAD_IN 1.0`、`GAP 0.45`（逐段另设）、`TAIL 2.8` | `tts.py:33-36` |
| 配乐 | 程序合成 112 BPM，C–G–Am–F 拨弦 + 低音 + 沙锤 + 钟琴，种子 42，淡入 1.2 s / 淡出 2.5 s | `audio.mjs:162-197` |
| 音效 | 18 种，比 mountain-cold 多 `drip`（龙头、下水口）、`water`（上午到傍晚整段水声）、`whistle`（开场体育课哨声）；场景切换自动 whoosh，品牌卡 sparkle | `audio.mjs:78-160`；`scenes-a.mjs:139`；`scenes-c.mjs:170-171` |
| 总混音 | 配乐 ×0.55 经旁白侧链压缩，音效 ×0.5，`alimiter 0.93` | `render.mjs:181-182` |
| 字幕 | SRT 99 条；烧录 `SUB_Y = 1560`，说话人标签小林（天蓝）、太阳（金）；品牌卡后停止 | `render.mjs:18, 78-106` |

未试听、未测响度。

## 7. 迭代与问题

| 事项 | 证据 | 说明 |
| --- | --- | --- |
| **`formal` 段文案改过一次** | `build/tts_cache` 比当前 33 段多 1 对缓存，词时间还原出旧句「换成**教材**的说法：气温的升降，取决于近地面大气获得与失去热量的差……」；当前 `script.json` 为「换成**规范**的说法」；`scenes-c.mjs:76` 卡片标题为「规范表述 · 气温为什么升降」 | `script.json` 修改时间 19:37 晚于 `scenes-a/b`、`tub.mjs`，TTS 19:39 重跑。**推断**：这句不是教材原文，改成「规范的说法」以免把概括写成教材引语；修改原因原文未记录 |
| `outro.tagline` 没有传到时间轴 | `script.json:62` 有 `outro.tagline`；`tts.py` 只写 `duration/rate/voices/segments`；`render.mjs:73` 用 `TL.outro?.tagline ?? '用地理的眼光，看懂一天的冷暖'` | 显示的是硬编码默认值，与 `script.json` 相同，画面无差别；只改 `script.json` 不会生效（与 [[mountain-cold]] 同一问题） |
| 无 `based_on` 字段 | `script.json:4` 用 `source` 描述 v003 页面、分镜与 review-001 | 与 AGENTS.md §1 的约定不一致 |
| 模板残留 | `lib.mjs` 与 mountain-cold 只差首行注释，颜色表含 `neb`、`ring`、`core` 等不用的键 | B 模板继承自前作（**推断**） |
| `package.json` 无 scripts | — | 命令只在注释与 AGENTS.md 中 |
| 规则符合情况（代码中存在） | 角标 logo、品牌卡、分段资料页脚（`render.mjs:109-115`：人教版、NOAA Climate Prediction Center、FAA/NWS Aviation Weather · NWS Raleigh 预报讲义、NASA Earth Observatory、UCAR）；未检出「示意」类声明 | 画面效果未核对 |

README 目录（`README.md:65`）已收录，记 3:38，与 ffprobe 一致。

## 8. 复现入口

命令来自 AGENTS.md「常用命令」与 `render.mjs:2-3`，**本次均未执行**：

```bash
cd afternoon-heat
npm install
python3 -m venv .venv && .venv/bin/pip install edge-tts   # fonts.py 另需 fontTools
.venv/bin/python tts.py              # → build/narration.wav、build/timeline.json、out/太阳过了正午，为什么下午还会更热.srt
node render.mjs --still 50 100 150   # 预览帧
node render.mjs                      # → out/太阳过了正午，为什么下午还会更热.mp4
```

修改入口：浴缸收支曲线 → `tub.mjs:7-21`；旁白—钟点映射 → `scenes-c.mjs:10-22`；日/年曲线 → `chart.mjs:87-101`；资料页脚 → `render.mjs:109-115`；品牌卡标语 → `render.mjs:73`。

## 9. 参考来源

`script.json` 与代码中**没有 URL**；只有 `source` 字段与画面页脚中的名称：

| 来源 | 用途 | 状态 |
| --- | --- | --- |
| Geo Story Studio · douyin-afternoon-heat v003 页面、分镜与科学审校（review-001） | 改编来源与 15 项结论表述 | `script.json:4`；本次未读取 |
| 人教版地理必修第一册 P34–36；人教版地理图册 P18 | 大气受热过程、地面是直接热源 | `script.json` `numbers` |
| NOAA CPC「UV Index: Diurnal Variability」 | 照射减弱不等于立即降温 | 仅名称 |
| FAA/NWS AC 00-6B 4.6 | 气温日变化 | 仅名称 |
| NASA Earth Observatory「Climate and Earth's Energy Budget」；NOAA JetStream「Earth-Atmosphere Energy Balance」 | 能量收支 | 仅名称 |
| UCAR「Conduction」 | 地面通过传导把热交给空气 | 仅名称 |
| NWS Raleigh「Maximum Temperature」 | 最高气温出现时间 | 仅名称 |

## 10. 证据与限制

| 文件 | 行数 | sha256[:12] | 读取程度 |
| --- | --- | --- | --- |
| `afternoon-heat/script.json` | 64 | `ff9b2e0f6ff5` | 全部字段（段落经 timeline 读出） |
| `afternoon-heat/tts.py` | 209 | `e08828e13311` | 与 air-composition diff |
| `afternoon-heat/render.mjs` | 198 | `9cd3338ccf71` | 与 mountain-cold 全文 diff |
| `afternoon-heat/lib.mjs` | 223 | `f89ea571d6c4` | 与 mountain-cold diff（仅首行） |
| `afternoon-heat/tub.mjs` | 135 | `9fb24ee598ec` | 1-22、114-125 行与导出表 |
| `afternoon-heat/chart.mjs` | 102 | `fe8870373778` | 导出表、87-101 行 |
| `afternoon-heat/cast.mjs` / `props.mjs` | 207 / 150 | `a501d67302f8` / `938ba087b021` | 导出表与注释 |
| `afternoon-heat/audio.mjs` | 216 | `3e7d0f11e02f` | 音效名、配乐参数 |
| `afternoon-heat/scenes-a…e.mjs` | 148/185/183/89/157 | `938809a3575b` / `78b1cef0f94a` / `471cb0c5ad50` / `6c53991a77a5` / `e4d2af3e80fa` | 头注释、函数与 `scenes` 表；`hourAt`、`formalScene` 片段 |
| `afternoon-heat/build/tts_cache/` | 68 个文件 | — | 只按文件名比对缓存键，并读取多出的 1 个词时间 json 还原旧句 |
| `afternoon-heat/build/timeline.json` | — | — | 全部段落时间与文字 |
| `afternoon-heat/out/太阳过了正午，为什么下午还会更热.mp4` | — | 25,592,277 字节 | 仅 ffprobe |

限制：宿主会话不可读；需求原文、改句原因、预览与用户反馈未记录；未运行脚本、未看画面、未试听；依赖版本未记录；git 仅有批量提交 `8cbe673`（2026-11-01）。
