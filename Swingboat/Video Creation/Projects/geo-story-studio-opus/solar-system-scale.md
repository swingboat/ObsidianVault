---
type: video-production-dossier
project: geo-story-studio-opus
video_id: solar-system-scale
title: 把太阳系装进操场，会有多空？
curriculum: 人教版必修第一册 第一章第一节 地球的宇宙环境（太阳系组成、距日顺序与分类、地球存在生命的条件）；课标1.1
captured: '2026-11-12'
status: 成片存在（竖屏 1080×1920，113.80 s）；无片尾品牌卡；技术检查/视觉检查/用户验收未记录
source_fingerprint: 3cca43fd8610d1d6736e9b3ce1964436ed69284fc806be1c558d197e3e8b14b7
tags:
- video-creation
- production-capture
- geography-teaching
- canvas-frame-render
- edge-tts
---

# 把太阳系装进操场，会有多空？ · 地球=1 厘米统一比例 + 对数缩放“暗色地图”镜头的竖屏短片

<!-- capture-source-sha256:3cca43fd8610d1d6736e9b3ce1964436ed69284fc806be1c558d197e3e8b14b7 -->

本文是根据项目文件做的材料回溯（源码、`script.json`、`build/timeline.json`、`build/tts_cache` 文件名与时间、`out/` 清单与 ffprobe）。按元数据找到一个可能对应的宿主会话 `5984ce06…`（2026-10-24 22:41 创建），但宿主返回空 transcript，**对话内容不可读**；对应关系按时间先后**推断**。本次**未运行任何项目脚本**，未打开音视频。

## 1. 当前结论与制作路线

**状态**：`out/把太阳系装进操场会有多空.mp4` 与 `.srt` 存在；无封面文件。成片存在不等于视觉/用户验收——两者均**未记录**。

| 项 | 值 | 来源 |
| --- | --- | --- |
| 分辨率 / 帧率 | 1080×1920，30 fps | ffprobe；`lib.mjs:7` |
| 时长 | 113.800 s（timeline 113.796） | ffprobe；`build/timeline.json` |
| 帧数 | 3414（`Math.round(DURATION*FPS)` 推算） | `render.mjs:176` |
| 视频 / 音频 | H.264 yuv420p CRF 19 medium faststart；AAC 192k 48 kHz 立体声 | ffprobe；`render.mjs:173-174` |
| 文件大小 | 21,649,691 B（约 21.6 MB） | 证据包 |
| 字幕 | SRT 61 条 cue + 画面烧录（y=1500） | timeline；`render.mjs:23, 66-91` |
| 时间线 | TTS 22:49–22:51 → timeline/SRT 22:51:55 → MP4 23:13:25（2026-10-24） | 缓存与文件 mtime |

**管线**：`script.json`（26 句、4 个声音角色，带 `scale` 比例表）→ `tts.py`（edge-tts 逐句缓存 + WordBoundary；`TARGET = 114` 触发提速，最终 rate +18%；海王星声线额外加 `aecho`）→ `timeline.json` / `narration.wav` / SRT → `render.mjs` 加载 `scenes-a…e.mjs`（9 个场景，`scenes-d` 只提供叠加层）、`map.mjs`（俯视地图与相机）、`bodies.mjs`（太阳、八大行星、学生、图钉）→ 逐帧 RGBA 管道送 ffmpeg，配乐/音效由 `audio.mjs` 程序合成并侧链压缩 → MP4。

## 2. 教学目标与课标对齐

- **curriculum**：人教版必修第一册 第一章第一节 地球的宇宙环境（太阳系的组成、八大行星距日顺序与分类、地球存在生命的条件）。课标1.1：运用资料，描述地球所处的宇宙环境。
- **based_on**：`geo-story-studio/storage/builds/douyin-solar-system-scale/v006`——沿用其“两把尺”的核心和已核验的 NASA/JPL 数据（`script.json` 原字段写的是本机绝对路径，本文按项目相对写法记录）。
- **核心知识点**：太阳系“行星很小、距离很大”；比大小与比距离必须用两把不同的尺，所以课本插图是压缩过的。
- **叙事结构**（按 26 句实际顺序）：
  - 导入：课本插图里八大行星挤满 → “真实的太阳系也这么挤吗？”
  - 原理①比大小：同一比例，地球 1 cm、太阳 1.1 m 齐腰高；八大行星同尺并排，教材分类（类地 / 巨行星 / 远日行星）。
  - 原理②比距离：从操场一头出发，水星 45 m、金星 85 m、地球 117 m（> 足球场）、1 AU≈1.5 亿 km、火星 179 m 与小行星带、木星 611 m 出校门、土星 1.1 km、天王星 2.3 km、海王星 3.5 km；步行四十多分钟 vs 光行 4 小时多。
  - 结论：太阳系大部分是空的；插图只能“二选一”。
  - 迁移：地球日地距离适中 → 温度适宜、液态水 → 生命存在的重要条件。
  - 总结：考点“水金地火木土天海，小行星带在火星和木星轨道之间”+“一句话”三卡片。
- **数字与出处**（`script.json` `scale` / `numbers`）：直径比取 NASA/JPL SSD 平均半径；太阳直径约 109 倍（NASA Sun Facts，IAU 名义半径 695700 km）；1 AU = 149597870.7 km → 模型 117.4 m；半长轴 J2000（JPL Approximate Positions Table 1）；光行时 499.005 s×30.07；步行 5 km/h 算出约 42 分钟；足球场 100～110 m（IFAB 规则）；生命条件取教材。
- **小不一致**：海王星直径比在 `scale.planets` 写 3.87，在 `numbers` 与 `bodies.mjs:15` 写 3.86；旁白只说“约4倍”，不影响成片。

## 3. 技术与依赖

| 类别 | 内容 | 证据 |
| --- | --- | --- |
| 渲染 | `@napi-rs/canvas` 声明 `^1.0.11`，lock 解析 **1.0.13** | `package.json`；`package-lock.json` |
| npm scripts | 只有 npm init 默认的 `test`（报错退出），无 render/check 脚本 | `package.json` |
| Python | edge-tts、aiohttp、fontTools（`fonts.py`）；无 requirements 文件，版本**未记录** | `tts.py`；`fonts.py` |
| 字体 | 缺 `build/fonts/YuantiSC-Bold.ttf` 时自动调用 `.venv/bin/python fonts.py`；回退 Arial Rounded / Arial Unicode / Hiragino Sans GB | `lib.mjs:11-22` |
| ffmpeg | adelay/amix/highpass/loudnorm（旁白）；aecho（海王星）；sidechaincompress/alimiter/libx264/aac（合成） | `tts.py`；`render.mjs` |

**声线**（全局 `rate: 4`，timeline 最终 `rate: 18` 叠加到每个角色）：

| 角色 | voice | 角色 rate / pitch | 实际合成 rate | 句数 |
| --- | --- | --- | --- | --- |
| `zh` 旁白 | zh-CN-XiaoxiaoNeural | 0 / 0 | +18% | 20 |
| `earth` 地球 | zh-CN-YunxiNeural | +6 / +2 Hz | +24% | 4 |
| `jupiter` 木星 | zh-CN-YunjianNeural | 0 / −6 Hz | +18% | 1 |
| `neptune` 海王星 | zh-CN-XiaoyiNeural | 0 / +2 Hz，`fx: aecho=0.8:0.6:220|420:0.35|0.2` | +18% | 1 |

## 4. 镜头与时间轴

时间取自 `build/timeline.json`；场景边界多数比段落开始提前 0.25–0.5 s，交叉淡化 0.3 s（带 0.95→1 / 1→1.04 的轻微缩放）。

| 场景 | 约时间 (s) | 段落 | 画面要点 | 模块 |
| --- | --- | --- | --- | --- |
| hook | 0–6.4 | hook, q | 课本式插图：太阳 + 八大行星挤成一排 | `scenes-a.mjs:11` |
| bean | 6.4–23.3 | hi, rule, sun, tiny | 拟人地球缩成 1 cm 豆子（带尺）；镜头后拉，1.1 m 太阳球滚入，穿校服的学生显示“齐腰高” | `scenes-a.mjs:76` |
| sizes | 23.3–42.3 | small, jup, big, kind | 同一比例（每地球直径 15 px 半径）三行排出八大行星；木星发言；三组教材分类框 | `scenes-b.mjs:25` |
| turn | 42.3–44.9 | turn | 卷尺展开：“第二把尺：距离” | `scenes-c.mjs:8` |
| walk（+ scenes-d 叠加层） | 44.9–84.9 | walk … empty | 俯视“暗色地图”：操场 → 校园 → 城镇，学生从太阳球出发逐个走到行星；比例尺、指北针；木星出校门、外行星图钉、海王星回声圈、步行 vs 光行时间卡；“大部分是空的”时地图变暗 | `scenes-c.mjs:71`；`scenes-d.mjs:25, 45`；`map.mjs` |
| rulers | 84.9–90.4 | ruler | 两把尺并列：比大小 vs 比距离 | `scenes-e.mjs:19` |
| life | 90.4–97.6 | life | 不远不近 → 温度适宜 → 液态水 → 生命条件链 | `scenes-e.mjs:75` |
| exam | 97.6–104.1 | exam | 距日顺序八格 + 小行星带位置 + “必考”印章 | `scenes-e.mjs:128` |
| finale | 104.1–113.8 | sum, bye | “行星很小 / 距离很大 / 太阳系很空旷”三卡；地球挥手；课本插图被横向压扁并盖“压缩版”章；彩纸 | `scenes-e.mjs:168` |

5 个场景模块（`scenes-d` 导出空 `scenes`，只给 walk 提供 `outerUnder/outerOver` 叠加与音效）共 9 个场景，另有 `map.mjs`、`bodies.mjs` 两个绘图库。

## 5. 关键算法与实现

> 视频**讲的知识**是太阳系尺度与行星分类；下面是**制作算法**（比例换算、相机插值、TTS 时间轴），不是天体运动模拟。

### 5.1 统一比例：所有数字从一张表来（`bodies.mjs:6-18`）

```js
// d = mean diameter (Earth = 1, NASA/JPL mean radii); a = semi-major axis (AU, J2000); m = model distance (m) with Earth = 1 cm
{ name: '木星', key: 'jupiter', d: 10.97, a: 5.2, m: 610.8, kind: 'giant' },
...
export const SUN_D = 109.2; // Sun diameter in Earth diameters
```

`m = a × 117.4`（1 AU = 149597870.7 km ÷ 12742 km × 1 cm）。大小场景用 `K = 15` px/地球直径画所有行星（`scenes-b.mjs:5`），地图场景用同一 `m` 定位，`kind` 决定教材分类框的颜色——画面、旁白、`script.json` 的数字共用一个来源。

### 5.2 俯视地图的“对数缩放”相机（`map.mjs:5-16`、`scenes-c.mjs:29-49`）

```js
export function camera(t) {
  ...
  const p = easeIO(prog(t, t0, t1));
  const k = k0 * Math.pow(k1 / k0, p), fd = lerp(d0, d1, p), ay = lerp(y0, y1, p);
  return { k, X: (x) => W / 2 + x * k, Y: (d) => ay - (d - fd) * k };
}
```

关键帧 `[时间, 焦点距离 d, 每米像素 k, 焦点屏幕 y]` 全部锚在词时间上（如 `at('out', '3.5公里') + 0.4`）。像素比例从 7 px/m 缩到 0.27 px/m（约 26 倍），用几何插值 `k0·(k1/k0)^p` 而不是线性插值，缩放感均匀；`track()` 让学生按同样的词时间从一个行星走到下一个。比例尺 `scaleBar` 从 `NICE = [5, 10, 20 … 2000]` 里挑当前合适的刻度（`map.mjs:166-178`）。

### 5.3 词级时间 + 角色声线特效

- `word(id, w, nth)` 支持同一词第 n 次出现（`lib.mjs:36-45`）；所有行星出现、图钉弹出、镜头关键帧都以此取时。
- 海王星一句在 `tts.py:181-182` 的混音阶段插入 `aecho`，制造“3.5 公里外喊话”的回声；画面同步画回声圈（`scenes-d.mjs:57-`）。
- 字幕字号从 58 开始每次减 2，直到整条胶囊（含角色标签）不超过 `W − 60`（`render.mjs:66-91`）。

### 5.4 屏幕震动与品牌角标（`render.mjs:49-61, 106-117`）

音效事件可带 `shake` 强度，`shakeAt()` 在 0.4 s 内按 `(1−p)²` 衰减叠加正余弦位移。角标 logo 0.3–1.0 s 淡入、`(64, 76)` 宽 290、0.95 透明度、径向暗晕，**全程不淡出**——本片没有片尾品牌卡，结尾是 finale 场景 + 彩纸（`TAIL = 2.8` 注释为 “room for the closing confetti”）。

## 6. 声音与字幕

| 项 | 内容 | 来源 |
| --- | --- | --- |
| 合成参数 | `TARGET=114`、`LEAD_IN=0.6`、`GAP=0.45`（逐句 gap 覆盖）、`TAIL=2.8`；字幕块 ≤14 字 | `tts.py:32-36, 112` |
| 提速经过 | 缓存里三整套（各 26 句）：+4%（22:49–22:50）、+30%（22:50）、+18%（22:51），timeline 最终 +18% | 缓存键复算（sha1(voice|rate|pitch|text)）与 mtime |
| 缓存 / 429 | sha1 前 16 位缓存；429 退避重试 5 次 | `tts.py` |
| 旁白混音 | adelay → amix → highpass 80 → loudnorm −16 LUFS / −1.5 dBTP / LRA 11 | `tts.py` |
| 配乐 | 程序合成 112 BPM C–G–Am–F 拨弦 + 低音 + 沙锤 + 钟琴，淡入 1.2 s / 淡出 2.5 s | `audio.mjs:178-208` |
| 音效 | 基础 18 种 + 本片新增 `fizz`、`boom`、`roar`、`thud`、`sizzle` | `audio.mjs:140-164`（与 meteor-annual 版 diff） |
| 总混音 | 音乐 0.75 侧链压缩（0.05 / 5:1 / 20 / 400 ms），音效 0.5，amix 后 `alimiter 0.93`；总线**不再做 loudnorm** | `render.mjs:170-171` |
| 烧录字幕 | 深色胶囊 y=1500；地球/木星/海王星分别用天蓝/琥珀/淡紫角色标签 | `render.mjs:64-91` |

本次未试听、未测响度。

## 7. 迭代与问题

| 事项 | 证据 | 说明 |
| --- | --- | --- |
| 为压时长把语速提到 +18% | `tts.py:33` `TARGET = 114.0`；缓存 +4/+30/+18 三套 | 首轮 +4% 超出；+30% 与 +18% 两套相隔约 1 分钟——**推断**期间改过 TARGET 或文案间隔。制作时（10-24）的旧 AGENTS 规则允许按目标时长一次提速；2026-10-26 `35ad4da` 起改为默认 `TARGET = None`、不为压时长提速，本片按现行规则属不符 |
| 无片尾品牌卡 | `render.mjs:49-61`（无 outro）；`scenes-e.mjs:198-203` finale 到 `1e9` | 现行 AGENTS 第 4 节“片尾品牌卡（必需）”于 2026-11-01 才写入，制作时不是硬性要求；按现行规则缺失 |
| `based_on` 写了本机绝对路径 | `script.json` `based_on` | 可移植性/隐私上不理想，建议改为相对写法（本次未改源文件） |
| 海王星直径 3.87 vs 3.86 | `script.json` `scale` vs `numbers`、`bodies.mjs:15` | 数据表内部小不一致 |
| `package.json` 是 npm init 默认模板 | `package.json` | `main: index.js`、`test` 报错，无实际脚本 |
| “示意”声明 | `render.mjs:93` 注释 | 页脚只在总结时显示“资料：NASA · NASA/JPL SSD · 人教版地理必修第一册”，遵守 AGENTS 第 5 节 |

## 8. 复现入口

命令来自 `render.mjs:2-3`、`tts.py` docstring 与 AGENTS“常用命令”，**本次均未执行**：

```bash
cd solar-system-scale
python3 -m venv .venv && .venv/bin/pip install edge-tts fonttools   # 版本未记录
npm install                                  # @napi-rs/canvas
.venv/bin/python tts.py                      # → build/narration.wav, build/timeline.json, out/把太阳系装进操场会有多空.srt
node render.mjs --still 3.2 7.6              # 可选预览 → build/still_<t>.png
node render.mjs                              # → out/把太阳系装进操场会有多空.mp4（首次会自动跑 fonts.py）
```

缓存三套齐全，**推断**在当前 `TARGET = 114` 下可离线重建（+4 → 计算出的 rate 若与 18 一致才会全部命中，未验证）。

## 9. 参考来源

| 来源 | 用途 | 状态 |
| --- | --- | --- |
| http://www.pep.com.cn | 人教版必修第一册（分类、生命条件） | 项目文件中提及，本次未访问 |
| https://science.nasa.gov/solar-system/planets/ | 行星概况 | 项目文件中提及，本次未访问 |
| https://science.nasa.gov/sun/facts/ | 太阳直径 | 项目文件中提及，本次未访问 |
| https://ssd.jpl.nasa.gov/planets/phys_par.html | 行星平均半径 | 项目文件中提及，本次未访问 |
| https://ssd.jpl.nasa.gov/planets/approx_pos.html | 半长轴（J2000） | 项目文件中提及，本次未访问 |
| https://ssd.jpl.nasa.gov/astro_par.html | 1 AU、光行时 | 项目文件中提及，本次未访问 |
| https://science.nasa.gov/solar-system/asteroids/facts/ | 小行星带 | 项目文件中提及，本次未访问 |
| `geo-story-studio/storage/builds/douyin-solar-system-scale/v006` | 改编来源（“两把尺”） | 本次未打开 |

同项目相关：[[earth-life-conditions]]（同一套 lib/audio 骨架）、[[cosmic-address]]、[[geo-celestial-bodies]]。

## 10. 证据与限制

| 文件 | 行数 | sha256[:12] | 读取程度 |
| --- | --- | --- | --- |
| `solar-system-scale/script.json` | 377 | `3e1629c867b8` | 全文 |
| `solar-system-scale/tts.py` | 210 | `bab845d10980` | 与 meteor-annual 版 diff + 关键行 |
| `solar-system-scale/render.mjs` | 187 | `ab05d84c4853` | 大部分 |
| `solar-system-scale/lib.mjs` | 243 | `6b59dd58594d` | 1-80 行 |
| `solar-system-scale/map.mjs` | 194 | `894437151518` | 相机、函数清单 |
| `solar-system-scale/bodies.mjs` | 217 | `29f3118a6511` | 数据表、函数清单 |
| `solar-system-scale/scenes-a…e.mjs` | 189/115/187/125/221 | `1857238b4e19` / `64f1f1c3ce57` / `e1f935c93e45` / `01c182c38340` / `0bf8688b7922` | 场景表、关键片段 |
| `solar-system-scale/audio.mjs` | 229 | `9e3eeae951b5` | 与 meteor-annual 版 diff |
| `solar-system-scale/package.json` / `fonts.py` | 17 / 37 | `df1cdf7033d8` / `a6ac7264e548` | 全文 |
| `out/把太阳系装进操场会有多空.mp4` | — | 21,649,691 B | 仅证据包 ffprobe |

限制：制作对话不可读；预览帧、用户反馈、提速决策原因均**未记录**；Git 中随 `1aa541f`（2026-10-26，5 片合并提交）入库。
