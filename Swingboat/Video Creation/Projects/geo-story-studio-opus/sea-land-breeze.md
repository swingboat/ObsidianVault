---
type: video-production-dossier
project: geo-story-studio-opus
video_id: sea-land-breeze
title: 海边的风，为什么白天晚上会掉头？
curriculum: 人教版必修第一册 第二章第二节 大气受热过程和大气运动（热力环流 · 实例：海陆风）；课标 1.4
captured: '2026-11-12'
status: 成片存在（竖屏 1080×1920，233.9 s）；技术检查/视觉检查/用户验收未记录
source_fingerprint: b60b8088c01bb065521947bdfbae385e7a4870e5962de30b000a21b7c5dd27cb
tags:
- video-creation
- production-capture
- geography-teaching
- canvas-frame-render
- edge-tts
---

# 海边的风，为什么白天晚上会掉头？ · 小陆与海伯：海陆热力性质差异 → 昼夜环流反转 → 过渡与叠加（Canvas 逐帧 + 四角色 edge-tts）

<!-- capture-source-sha256:b60b8088c01bb065521947bdfbae385e7a4870e5962de30b000a21b7c5dd27cb -->

本文是根据项目文件做的材料回溯（`script.json`、`tts.py`、`render.mjs`、`lib.mjs`、`circ.mjs`、`beach.mjs`、`chars.mjs`、`cast.mjs`、`props.mjs`、`audio.mjs`、6 个 `scenes-*.mjs`、`build/timeline.json`，以及 ffprobe 结果）。宿主环境中按元数据找到一个可能对应的制作会话 `35b35754-b1de-450e-a118-73a47d0deb30`（2026-10-31 21:45 创建，标题“Video creation for sea and land breezes”），但**读不到对话内容**（宿主返回空 transcript），对应关系只是根据时间戳**推断**：`script.json` 于 21:50:05 落盘。本次没有运行任何项目脚本。

## 1. 当前结论与制作路线

**状态**：`out/` 里有成片 MP4 和 SRT。画面检查、用户验收、发布情况都**未记录**。

| 项 | 值 | 来源 |
| --- | --- | --- |
| 分辨率 / 帧率 | 1080×1920 竖屏，30 fps | ffprobe；`lib.mjs:7` |
| 帧数 | 7018（`round(233.939×30)`，与 ffprobe 的 233.934 s 一致） | `render.mjs` 主循环公式 |
| 时长 | 233.934 s（timeline 233.939），是这 5 部片里最长的 | ffprobe；timeline |
| 编码 | H.264 libx264 medium CRF 19 yuv420p + AAC 48 kHz 立体声 192k，faststart | ffprobe；`render.mjs:173` 附近 |
| 文件 | 37,253,239 B，平均约 1.27 Mbps | evidence pack |
| 字幕 | `out/海边的风，为什么白天晚上会掉头.srt`，118 条；画面同时烧录 | 文件 |
| 其他版本 | 无 | — |
| 文件时间 | script 21:50:05 → timeline 21:55:55 → MP4 22:15:47 | mtime |

**一句话管线**：`render.mjs` 和 [[thermal-circulation]] 的只差标题、`KEYS`（这里是 a～f 共 6 个模块）、角色表、标语和页脚。新增了 `circ.mjs`（白天和夜晚共用的海陆环流图，由一个状态对象驱动）和 `chars.mjs`（小陆、海伯、小旗、帆船）。6 个场景模块共 14 个场景。

## 2. 教学目标与课标对齐

- **curriculum**：人教版必修第一册 第二章第二节 大气受热过程和大气运动（热力环流 · 实例：海陆风）；课标 1.4。
- **核心知识点**：海陆热力性质差异（比热容、透光与混合、蒸发）→ 白天陆地热，近地面陆低海高，吹海风 → 夜晚陆地比海凉，环流反转，吹陆风，且陆风通常较弱 → 日落后风向不会立刻反转，晴朗、背景风弱时最典型 → 迁移：帆船早出晚归、湖陆风。
- **叙事结构**：
  - 导入：小林的两件怪事——沙子和海水的冷热，小旗的朝向（`ask1-3/hook/topic`）。
  - 角色登场：急性子的小陆和慢性子的海伯（`meet/landHi/seaHi`）。
  - 原理：三条原因（`why/r1-r3/rSum`）→ 白天（`day0…dayCool`）→ 夜晚（`night0…weak`）。
  - 深化：换向有滞后（`q2/a2/lull`）；背景风叠加和阴天的影响（`bg/best`）。
  - 迁移：帆船、湖陆风（`use0/boat/lake`）。
  - 总结：一句话总结、做题三步、易错点“风按来向命名”（`sum…trap`），最后小陆、海伯道别。
- **科学边界**（旁白直接说出）：`nightNote`“海水夜里并没有升温，它只是降温慢”；`weak` 陆风通常较弱；`a2/lull` 日落不会马上换向，中间常有一段风平浪静；`bg` 强背景风会盖过海陆风，阴天时海陆风也不明显；`trap` 风按来向命名。
- **facts**（9 条，出处为教材 P36–37、星球地图出版社图册 P19、NOAA JetStream《The Sea Breeze》、UBC ATSC113《Local Winds》、Stull Ch.11/Ch.17、新加坡气象署《Local Winds》《Winds》）。**numbers** 只有 1 条：水的比热容约为干沙的 5 倍（4.18 vs 0.83 kJ/(kg·K)，Engineering ToolBox）。
- `script.json` **没有 `based_on`**。`source` 写的是“改编自 Geo Story Studio · douyin-sea-land-breeze v002”，按命名惯例推断源 build 为 `geo-story-studio/storage/builds/douyin-sea-land-breeze/v002/`（**推断**）。

## 3. 技术与依赖

| 类别 | 内容 | 依据 |
| --- | --- | --- |
| 渲染 | `@napi-rs/canvas` 声明 `^1.0.11`，lock 解析为 **1.1.0**；没有 npm scripts | `package.json`、lock |
| TTS / 字体 / 合成 | edge-tts + aiohttp（版本未记录）；fontTools；ffmpeg | `tts.py`、`fonts.py` |
| 公共库新增 | `lib.mjs:218-224` 加了 `track(t, ks, d)`，用于分段缓动取值（每个关键帧从上一个值 ease 到新值），`scenes-a.mjs` 用到了它 | `lib.mjs` diff |

| 角色 | 声音 | rate（+全局 4） | pitch |
| --- | --- | --- | --- |
| 旁白老师 `zh` | zh-CN-XiaoxiaoNeural | +4% | 0 |
| 学生小林 `stu` | zh-CN-YunxiNeural | +10% | +2 Hz |
| 陆地小陆 `land` | zh-CN-YunxiaNeural | +18% | +4 Hz |
| 海洋海伯 `sea` | zh-CN-YunjianNeural | −2%（−6+4） | −2 Hz |

声音设定和角色性格对应：小陆“来得快、去得快”，语速最快；海伯“只暖一点点”，语速最慢、音调偏低。

## 4. 镜头与时间轴

| # | 场景 | 约时间 (s) | 旁白要点 | 画面 | 模块 |
| --- | --- | --- | --- | --- | --- |
| 1 | hook | 0–24.0 | 两件怪事 | 中午小林在烫沙上跳，温度计；夜里沙凉水温；小旗白天朝岸、夜里朝海；两件事合成一件，盖章“海陆风” | `scenes-a.mjs:17` |
| 2 | hosts | 24.0–41.6 | 小陆、海伯登场 | 聚光灯下两人各配一根高温度计：小陆的读数大起大落，海伯的几乎不动 | `scenes-a.mjs:71` |
| 3 | specificHeat | 41.6–49.4 | 比热容 | 升高 1℃：沙子 1 个热量币，水要 5 个 | `scenes-b.mjs:37` |
| 4 | mixing | 49.4–63.6 | 透光、混合、蒸发 | 阳光照到陆地表面就停住，在海水里照得很深；热点在陆地挤在表层、在海水里被搅到深处；海面有蒸发的水汽 | `scenes-b.mjs:79` |
| 5 | curves | 63.6–71.9 | 海陆热力性质差异 | 一昼夜气温曲线：陆地振幅大，海洋振幅小 | `scenes-b.mjs:216` |
| 6 | day | 71.9–105.0 | 白天的环流与海风 | `circ.mjs` 环流图：金色环路、高空同高线、高 / 低标记、小旗朝陆，“海风：海 → 陆” | `scenes-c.mjs:16` |
| 7 | coolCoast | 105.0–111.1 | 海风凉爽湿润 | 海风吹到海边城市，往内陆逐渐减弱，带着水滴图标 | `scenes-c.mjs:69` |
| 8 | night | 111.1–152.0 | 夜晚反转、陆风较弱 | 同一张环流图，`rev:true`，蓝色、环路变细；“陆风：陆 → 海”；昼夜强弱对比卡 | `scenes-d.mjs:17` |
| 9 | lag | 152.0–168.2 | 不会马上掉头 | 气温曲线上标出日落线，标记点滑到实际交叉点“陆地比海面凉了”，两个交叉点附近画出无风带 | `scenes-e.mjs:38` |
| 10 | override | 168.2–184.3 | 背景风、阴天 | 风的矢量相加卡；冷空气、台风盖过海陆风；阴天画面变灰、温差缩小；“最典型”条件清单 | `scenes-e.mjs:101` |
| 11 | boats | 184.3–194.4 | 早出晚归 | 清晨陆风送船出海，午后海风送船回家 | `scenes-f.mjs:15` |
| 12 | lake | 194.4–202.9 | 湖陆风 | 湖两侧两个环流，湖面缩小后环流变细、变淡 | `scenes-f.mjs:38` |
| 13 | summary | 202.9–220.7 | 一句话，做题三步 | 白天和夜晚两个小环流，三步列表 | `scenes-f.mjs:81` |
| 14 | finale | 220.7–233.9 | 风按来向命名，道别 | “从海上吹来 = 海风”，小陆、海伯道别；约 231.3 s 起切到片尾品牌卡 | `scenes-f.mjs:106` |

## 5. 关键算法与实现

> 视频**讲的知识**是海陆风；下面说的是**做视频用的算法**。气温曲线是余弦函数画出的示意曲线，不是观测数据。

**5.1 昼夜共用一张环流图，用状态对象驱动**（`circ.mjs:7-61`）。海在左（`SEA_X=270`）、陆在右（`LAND_X=840`），高空线 `UP_Y=560`，近地面线 `LOW_Y=1170`。环路总是从**上升的一侧**开始：

```js
const DAY_PTS = [[LAND_X, LOW_Y], [LAND_X, UP_Y], [SEA_X, UP_Y], [SEA_X, LOW_Y]];
const NIGHT_PTS = [[SEA_X, LOW_Y], [SEA_X, UP_Y], [LAND_X, UP_Y], [LAND_X, LOW_Y]];
export function legProgress(rise, aloft, sink, surf) {
  return (V * clamp(rise) + Hz * clamp(aloft) + V * clamp(sink) + Hz * clamp(surf)) / TOTAL;
}
...
const warmX = rev ? SEA_X : LAND_X, coolX = rev ? LAND_X : SEA_X;
hl(ctx, warmX, UP_Y - 110, true, ...);  hl(ctx, coolX, UP_Y - 110, false, ...);   // 高空：暖侧高压
hl(ctx, warmX, LOW_Y + 100, false, ...); hl(ctx, coolX, LOW_Y + 100, true, ...);  // 近地面：暖侧低压
```

“上升 / 高空 / 下沉 / 近地面”4 段的进度分别挂在旁白关键词上（白天是“往上升”“流向海洋”“下沉”“近地面”，见 `scenes-c.mjs:16-34`），再按各段弧长加权，合成一个 0～1 的进度交给 `loopFlow`。高空和近地面两组高 / 低标记，都和一条“高空 · 同一高度”虚线一起出现，对应“同一水平面比较”。夜间传入 `rev:true`，环路和标记整体对调；`weak` 参数让环路从 36 px 变细到 24 px，箭头流速从 190 降到 120，小旗的风力从 0.85 降到 0.6，表现“陆风通常较弱”（`scenes-d.mjs:17-37`）。小旗的方向 `flagDir`、`windStreaks` 风纹的方向都和近地面的风向一致：白天 +1（海 → 陆，向右），夜里 −1。

**5.2 一昼夜气温曲线，用二分法求交叉点**（`scenes-b.mjs:174-213`；`scenes-e.mjs:13-21`）：

```js
export const landT = (h) => 925 - 230 * Math.cos((TAU * (h - 14)) / 24);   // canvas y，越小越热
export const seaT  = (h) => 925 -  48 * Math.cos((TAU * (h - 15.5)) / 24);
function crossing(h0, h1) { /* 40 次二分 landT−seaT 变号 */ }
const H_EVE = crossing(18, 26), H_MORN = crossing(6, 12);
```

陆地曲线振幅 230，峰值在 14 时；海洋振幅 48，峰值在 15:30。本次按代码复算，傍晚交叉点 H_EVE ≈ 19.62 h（约 19:37），比 18:00 的“日落”线晚约 1.6 h；早晨交叉点约 7:37。`lag` 场景里，标记点从日落线滑到 H_EVE，两个交叉点附近画出“风很小”的区段。两条曲线之间的填色逐 0.25 h 判断：陆地更暖时填橙色，海洋更暖时填蓝色。“日落后不会立刻换向”这一点是由曲线算出来的，不是手工标的。

**5.3 风的矢量叠加**（`scenes-e.mjs:113-124`）：局地海风箭头向右长 200 px，大范围强风箭头向左长 540 px，“实际的风”箭头向左长 340 px，正好满足 200 − 540 = −340。同时小旗反向（`big>0.5 ? -1 : 1`）。阴天时用 `gray` 参数同时压暗画面、缩小海陆温差的着色，并让两支温度计读数靠近。

**5.4 迁移场景的方向**（`scenes-f.mjs:15-31, 54-62, 73-79`）：清晨小旗和风纹向海（−1），帆船驶出；午后向陆（+1），帆船返航。湖陆风在湖两侧各画一个环路，路径是“近地面从湖心吹向岸 → 岸上上升 → 高空回到湖上 → 湖上下沉”，对应白天的湖风；`WEAKEN` 之后环路的透明度从 1 降到 0.35，线宽从 32 降到 14。本次核对过这些环流方向，都和教材一致。剖面图没有指北针，这是剖面，不属于“上北下南”的要求范围（**本次判断**）。

**5.5 公共管线**：与 [[sky-colors]] 相同（WSS_URL 替换、sha1 缓存、429 退避、WordBoundary → `at()`、`adelay/amix/loudnorm`）。`LEAD_IN=0.8`，`TAIL=2.8`。

## 6. 声音与字幕

- 旁白：`highpass 80 → loudnorm I=-16:TP=-1.5:LRA=11`，最后输出 `build/narration.wav`；42 段，SRT 118 条，是这 5 部片中最多的。
- 配乐和音效：共用的 `audio.mjs`（112 BPM，C–G–Am–F；音乐 0.55 并以旁白侧链压缩；音效 0.5；`alimiter 0.93`）；切场景自动加 whoosh，片尾卡配 sparkle。
- 字幕：每块不超过 14 字，或用 `captions` 数组；烧录位置 `SUB_Y=1560`；角色标签为小林 / 小陆（`#ffb347`）/ 海伯（`#5ec8ff`）（`render.mjs:78`）。
- 品牌：左上角 logo；片尾卡从 `E('bye')+0.3` 开始（约 231.3 s，持续约 2.6 s），标语“用地理的眼光，读懂海边的每一阵风”；页脚“资料：人教版地理必修第一册 · NOAA · UBC ATSC113 · Stull”。

## 7. 迭代与问题

| 事项 | 证据 | 说明 |
| --- | --- | --- |
| 与 [[thermal-circulation]] 主题重叠 | 标题几乎相同；`beach.mjs` 和 `props.mjs` 与 thermal-circulation 的只差参数化或一处字体；`cast.mjs` 只多了 `noMouth` 选项 | **推断**：本片是在 thermal-circulation 目录基础上扩展的海陆风专题（21:50 晚于 21:22）。README 只收录了本片 |
| 遗留注释过时 | `tts.py:3` docstring 仍写“(narrator / student / warm air / cool air)”；`cast.mjs:1` 仍写“阿暖 / 阿凉”；`lib.mjs` 还留着 `bag/bagDark` 颜色 | 实际角色是小陆 / 海伯。注释没更新，不影响运行 |
| 缺少 `based_on` | `script.json` keys | 不符合 AGENTS §1，只在 `source` 文字里写了 build 名 |
| 规则符合度（代码中存在） | `brand()`、`outro()`、页脚只写资料来源 | 符合 AGENTS §4/§5 |
| README | 写的是 3:54 | 与 ffprobe 的 233.9 s 一致 |

预览帧、返工过程、用户反馈都**未记录**。

## 8. 复现入口

以下命令来自 README 和 `render.mjs` 头部注释，本次**均未执行**：

```bash
cd sea-land-breeze
npm install
python3 -m venv .venv && .venv/bin/pip install edge-tts   # fonts.py 另需 fontTools
.venv/bin/python tts.py
node render.mjs --still 90 160     # → build/still_<t>.png
node render.mjs                    # → out/海边的风，为什么白天晚上会掉头.mp4
```

修改入口：环流图的布局在 `circ.mjs:7-12`；气温曲线的振幅和相位在 `scenes-b.mjs:176-177`；矢量叠加卡在 `scenes-e.mjs:113-124`。

## 9. 参考来源

项目文件里**没有 URL**（`urls: []`）。下表是文件中提到的资料名称，本次**均未访问**：

| 资料 | 用途 |
| --- | --- |
| 人教版地理必修第一册 P36–37；星球地图出版社配套图册 P19 | 热力环流、海陆风、风向定义 |
| NOAA JetStream《The Sea Breeze》 | 热响应差异、海风的凉爽效应 |
| UBC ATSC113《Local Winds: Sea and Land Breezes》 | 陆风较弱、过渡期、背景风 |
| Roland Stull《Practical Meteorology》Ch.11 / Ch.17 §17.3.4 | 环流机理、背景风叠加 |
| 新加坡气象署《Local Winds》《Winds》 | 陆风较弱、风向按来向命名 |
| Engineering ToolBox《Specific Heat of Common Substances》 | 比热容 4.18 / 0.83 kJ/(kg·K) |
| `geo-story-studio/storage/builds/douyin-sea-land-breeze/v002/`（推断路径） | 分镜与科学审校 |

## 10. 证据与限制

| 文件 | 行数 | sha256[:12] | 读取程度 |
| --- | --- | --- | --- |
| `sea-land-breeze/script.json` | 77 | `95920f310403` | 全文 |
| `sea-land-breeze/circ.mjs` | 68 | `bd6839b5a027` | 全文 |
| `sea-land-breeze/scenes-a…f.mjs` | 137/239/122/92/168/149 | `b6f8b60ec71c`/`def8db6414b6`/`da8213acc4cb`/`398d40f1d6f3`/`0310e3104daa`/`bb883c3fb57b` | 结构全部；scenes-b 的曲线、c 的白天、d 的夜晚、e 的交叉点和矢量、f 的帆船和湖都读了 |
| `sea-land-breeze/chars.mjs` / `beach.mjs` / `cast.mjs` / `props.mjs` | 192/120/204/174 | `d7326be02277`/`25c38634912c`/`93f5d5547bba`/`9a53efe20fc3` | 导出与注释；`flag` 全文；与 thermal-circulation 版做了 diff |
| `sea-land-breeze/render.mjs` / `lib.mjs` / `tts.py` | 190/225/209 | `3146a06f1b37`/`6d9813fa970a`/`e6a7c337482d` | 与 air-pressure / sky-colors 版 diff |
| `sea-land-breeze/audio.mjs` / `fonts.py` | 204/37 | `f9e04b2cfd54`/`a6ac7264e548` | 与其他几部片相同 |
| `sea-land-breeze/build/timeline.json` | — | — | 读取各段 start |
| `sea-land-breeze/out/*.mp4` / `*.srt` | — | 37,253,239 B / 7,115 B | 只看 ffprobe 结果和 SRT 条数 |

限制：没有播放视频，也没有试听；环流方向和曲线交叉点是读代码、复算得出的，没有看渲染出来的画面。依赖版本和渲染耗时都未记录。
