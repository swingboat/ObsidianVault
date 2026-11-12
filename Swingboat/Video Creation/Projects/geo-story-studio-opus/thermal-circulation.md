---
type: video-production-dossier
project: geo-story-studio-opus
video_id: thermal-circulation
title: 海边的风为什么会掉头？一冷一热，空气转成一个圈
curriculum: 人教版必修第一册 第二章第二节 大气受热过程和大气运动（大气运动：热力环流）；课标 1.4
captured: '2026-11-12'
status: 成片存在（竖屏 1080×1920，177.2 s）；未列入 README 目录；技术检查/视觉检查/用户验收未记录
source_fingerprint: 2962d8a0156fb8fb138acc308fed1a88488fd6d275a15707d75441dd7683cf17
tags:
- video-creation
- production-capture
- geography-teaching
- canvas-frame-render
- edge-tts
---

# 海边的风为什么会掉头？ · 甲 / 乙两柱理想实验：空气“份数”和等压面一起变形（Canvas 逐帧 + 四角色 edge-tts）

<!-- capture-source-sha256:2962d8a0156fb8fb138acc308fed1a88488fd6d275a15707d75441dd7683cf17 -->

本文是根据项目文件做的材料回溯（`script.json`、`tts.py`、`render.mjs`、`lib.mjs`、`diagram.mjs`、`beach.mjs`、`cast.mjs`、`props.mjs`、`audio.mjs`、4 个 `scenes-*.mjs`、`build/timeline.json`，以及 ffprobe 结果）。宿主环境中按元数据找到一个可能对应的制作会话 `8135557c-8484-43c1-8721-497492a075d8`（2026-10-31 21:15 创建），但**读不到对话内容**（宿主返回空 transcript），对应关系只是根据时间戳**推断**：`script.json` 于 21:22:01 落盘。本次没有运行任何项目脚本。

**注意：本片没有列入项目 `README.md` 的视频目录**。README 的“第二章第二节”表格只有 atmosphere-heating、atmosphere-insulation、afternoon-heat、sky-colors、air-pressure、sea-land-breeze、wind-forces，没有 `thermal-circulation/`。

## 1. 当前结论与制作路线

**状态**：`out/` 里有成片 MP4 和 SRT。画面检查、用户验收、发布情况都**未记录**。README 没有收录本片，原因**无法确认**（见 §7）。

| 项 | 值 | 来源 |
| --- | --- | --- |
| 分辨率 / 帧率 | 1080×1920 竖屏，30 fps | ffprobe；`lib.mjs:7` |
| 帧数 | 5315（`round(177.18×30)`，与 ffprobe 的 177.167 s 一致） | `render.mjs` 主循环公式 |
| 时长 | 177.167 s（timeline 177.18） | ffprobe；`build/timeline.json` |
| 编码 | H.264 libx264 medium CRF 19 yuv420p + AAC 48 kHz 立体声 192k，faststart | ffprobe；`render.mjs:173` 附近 |
| 文件 | 26,161,623 B，平均约 1.18 Mbps | evidence pack |
| 字幕 | `out/海边的风为什么会掉头：一冷一热，空气转成一个圈.srt`，92 条；画面同时烧录 | 文件 |
| 其他版本 | 无 | — |
| 文件时间 | script 21:22:01 → timeline 21:24:08 → MP4 21:40:09 | mtime |

**一句话管线**：`render.mjs`、`tts.py`、`audio.mjs` 和 [[air-pressure]] 的基本一样。本片新增了 `diagram.mjs`（一张从头用到尾的甲 / 乙两柱机理图）和 `beach.mjs`（海陆剖面）。4 个场景模块里共有 6 个场景，`render.mjs` 里 `KEYS = ['a','b','c','d']`。

## 2. 教学目标与课标对齐

- **curriculum**：人教版必修第一册 第二章第二节 大气受热过程和大气运动（大气运动：热力环流）；课标 1.4。
- **核心知识点**：热力环流是“由于地面冷热不均而形成的空气环流”。因果链是：受热膨胀 → **高空先出现**气压差 → 空气在高空搬走 → 近地面气压才改变 → 近地面回流，转成一个圈。另外讲等压面“凸高凹低”，以及近地面和高空的气压类型相反。
- **叙事结构**：
  - 导入：小林在海边放风筝，风中午吹向岸、晚上掉头（`ask/hook/topic`）。
  - 原理：用理想实验讲 6 个小节（`lab…bend2`，画面顶部依次标 1～6）。
  - 易错点：高空的“高压”比近地面的气压低得多（`q2/a2/a2b`）。
  - 迁移：海陆风（`beach/day/night/aha`），城市热岛环流和山谷风（`more`）。
  - 总结：因果链（`sum1-3`）、做题三句话（`tip`）、阿暖道别（`bye`）。
- **科学边界**（`source` 里的审校要点，用旁白和画面直接说出）：“光是膨胀，甲地上方空气的总量没变，地面受的压力也就没变”（`a1`）；高低压只在同一水平面比较；“只要冷热差还在，我们就会一直转下去”，即温差持续，环流才能维持。`source` 还写明这是“局地、忽略旋转的理想模型”，所以画面里**不画**地转偏向。
- **facts**（7 条定性结论，没有数值）：热力环流定义、等压面上凸 / 下凹（教材）；膨胀时地面气压不立即降低（Stull《Practical Meteorology》Ch.11 p.341–342；Birner CSU General Circulation 讲义幻灯 4–11）；高空先流后近地面气压才变（Stull Fig.11.17）；高空的“高压”仍低于其下方近地面气压（静力平衡 dp/dz = −ρg，NASA Glenn Equation of State）；海陆风（教材）；温差维持环流（Stull Ch.11）。
- `script.json` **没有 `based_on`**，`source` 写的是“改编自 Geo Story Studio · douyin-thermal-circulation v002”，按命名惯例推断源 build 为 `geo-story-studio/storage/builds/douyin-thermal-circulation/v002/`（**推断**）。

## 3. 技术与依赖

| 类别 | 内容 | 依据 |
| --- | --- | --- |
| 渲染 | `@napi-rs/canvas` 声明 `^1.0.11`，lock 解析为 **1.1.0**；没有 npm scripts | `package.json`、lock |
| TTS / 字体 / 合成 | edge-tts + aiohttp（版本未记录）；fontTools（`fonts.py`）；ffmpeg | `tts.py`、`fonts.py` |

| 角色 | 声音 | rate（+全局 4） | pitch |
| --- | --- | --- | --- |
| 旁白老师 `zh` | zh-CN-XiaoxiaoNeural | +4% | 0 |
| 学生小林 `stu` | zh-CN-YunxiNeural | +10% | +2 Hz |
| 暖空气阿暖 `warm` | zh-CN-XiaoyiNeural | +16% | +6 Hz |
| 冷空气阿凉 `cool` | zh-CN-YunxiaNeural | +2%（−2+4） | −2 Hz |

暖空气快、音调高，冷空气慢、音调低，用声音区分了两种空气的“性格”。

## 4. 镜头与时间轴

| # | 场景 | 约时间 (s) | 旁白要点 | 画面 | 模块 |
| --- | --- | --- | --- | --- | --- |
| 1 | hook | 0–15.7 | 风会掉头？引出热力环流 | 海陆剖面（海在左、陆在右）；小林的风筝在“掉头”时从右侧飘到左侧，风纹换方向；画面压暗出现大“？”，环形箭头转起来，盖章“热力环流” | `scenes-a.mjs:23` |
| 2 | 理想实验 | 15.7–105.5 | `lab…bend2` | 同一张甲 / 乙两柱图，顶部依次标出 6 个小标题：一冷一热 → 地面气压先不变 → 高空先出现气压差 → 空气搬家后地面气压才变 → 转成一个圈 → 凸高凹低 | `scenes-b.mjs:95` + `diagram.mjs` |
| 3 | C1 易错点 | 105.5–118.9 | 高空“高压”比近地面低 | 甲柱的分子近地面密、高空稀；两条“实际气压”横条（高空短、近地面长，不标数值），出现“高空‘高压’＜近地面‘低压’”，盖章“只在同一水平面比” | `scenes-c.mjs:9` |
| 4 | C2 海陆风 | 118.9–145.0 | 白天海风，夜晚陆风 | 剖面上标温度；白天的金色环流沿“陆地上升 → 高空流向海 → 海上下沉 → 近地面吹向陆”，标“海风 →”；夜里换成蓝色、方向相反，标“← 陆风” | `scenes-c.mjs:64` |
| 5 | C3 举一反三 | 145.0–151.0 | 城郊、山谷 | 两张卡片：城市热岛环流（两个对称的圈）；谷风（白天沿山坡上升） | `scenes-c.mjs:140` |
| 6 | D 总结 | 151.0–177.2 | 因果链，三句口诀，道别 | 3 张因果卡加 3 条口诀；阿暖和阿凉沿一个圆环一起转；约 174.6 s 起切到片尾品牌卡 | `scenes-d.mjs:24` |

## 5. 关键算法与实现

> 视频**讲的知识**是热力环流原理；下面说的是**做视频用的算法**。`diagram.mjs` 用“等质量空气份数”和等压面偏移量做示意计算，不是数值大气模型。

**5.1 空气份数：按等质量分层**（`diagram.mjs:12-13, 59-71`）：两柱各 40 个点（8 层 × 5 个），每个点代表一份等量的空气。第 k 层的高度取指数大气累积质量达到 (k+0.5)/9 时对应的高度，所以点越往上越稀，和气压随高度递减的指数规律一致：

```js
const HS = 335;
const zRow = (k) => -HS * Math.log(1 - (k + 0.5) / 9);
...
const y = G - zRow(k) * sc + (rnd(i * 7 + c) - 0.5) * 10;   // sc：甲柱受热膨胀 ×1.3，乙柱冷缩 ×0.94
if (c === 0 && k === 7 && j < 4) { ... lerp(x, tx, m) ... }  // 甲顶层 4 份在高空搬到乙
```

只膨胀时，甲柱只是被整体拉高，每柱仍是“空气 40 份”，两个地面气压表之间显示“＝”（`diagram.mjs:154-178`）。旁白讲到“从甲地”时 `moved()` 从 0 走到 1，甲柱顶层的 4 份沿抛物线（`−sin(πm)·40`）移到乙柱上方，份数标签变成 36 和 44，地面气压表随之一低一高。这样就把“先膨胀、后搬家”拆成了画面上可以看到的两步。

**5.2 等压面偏移：两个阶段插值**（`diagram.mjs:46-57, 132-152`）：三条等压面的基准高度是 110/300/520 px。每条线在甲、乙两柱处各叠加一个高斯形偏移 `exp(-((x−CX)/135)²)`：

```js
const expandOnly = [[25, 80, 150], [-5, -15, -30]][c][level] * e;   // 只膨胀：甲整体上抬
const final = [[-45, 20, 80], [40, -10, -60]][c][level];            // 搬家后
return lerp(expandOnly, final, m);
```

搬家后：甲近地面那条下凹 45 px（低压），高空那条上凸 80 px（高压）；乙近地面上凸 40 px（高压），高空下凹 60 px（低压）。这正是教材的“高压处上凸、低压处下凹”“近地面与高空气压类型相反”，和后面出现的“高 / 低”标记（`pressureMarks`，`diagram.mjs:181-200`）一致。高空金色箭头从甲指向乙，近地面蓝色箭头从乙指回甲。

**5.3 环流箭头按弧长展开**（`diagram.mjs:35-40`；`props.mjs:90-134`）：环路 `LOOP` 是一个闭合折线，依次为甲上升 → 高空 → 乙下沉 → 近地面。各段长度是 695/460/695/460 px，累计到 0/695/1155/1850/2310。`loopReveal` 把旁白里“上升 / 高空流出 / 下沉 / 近地面回流”4 个词的时间戳分别映射到这 4 个弧长区间，箭头说到哪段就画到哪段。`loopFlow` 先画半透明的粗轨道，再让 V 形箭头按 `(t·speed) % gap` 沿 `pathPoint` 移动，并在折线拐角处转向。

**5.4 海陆风和其他迁移实例的环流方向**（`scenes-c.mjs:59-61, 90-107, 140-160`）：剖面里海在左（x=270）、陆在右（x=820）。白天环路 `DAY_LOOP` 从陆地底部出发，依次为陆地上升 → 高空流向海 → 海上下沉 → 近地面吹向陆，标“海风 →”。夜间 `NIGHT_LOOP` 方向反过来，用蓝色画。气压标记：白天陆地近地面为低压、高空为高压，海面相反；夜间对调。城市热岛的两个圈都是“城市上升、郊区下沉、近地面风吹向城市”；谷风的两个圈是“沿山坡上升、山谷中心下沉”。这些方向本次都核对过，和教材的规律一致。剖面图没有指北针，但它是剖面而不是平面图，AGENTS §6“上北下南”针对的是地图和平面示意图（这一点是**本次判断**）。

**5.5 易错点用相对长度表示**（`scenes-c.mjs:28-39`）：两条“实际气压”横条，高空长 200 px，近地面长 560 px，故意不标数值，只表达“高空的高压 ＜ 近地面的低压”。这和 facts 里引用的静力平衡相对应。

**5.6 公共管线**：与 [[sky-colors]] 相同（WSS_URL 替换、sha1 缓存、429 退避、WordBoundary → `at()`、`adelay/amix/loudnorm`）。`LEAD_IN=0.8`，`TAIL=2.8`。

## 6. 声音与字幕

- 旁白混音：`highpass 80 → loudnorm I=-16:TP=-1.5:LRA=11`。配乐 112 BPM（C–G–Am–F），音量 0.55，以旁白侧链压缩；音效 0.5；`alimiter 0.93`（`audio.mjs` 和 `render.mjs:173` 与其他几部片相同）。
- 音效由场景 `events` 按词触发；切场景时自动加 whoosh，片尾卡配 sparkle。
- 字幕：SRT 92 条，每块不超过 14 字，或用 `captions` 数组；烧录位置 `SUB_Y=1560`；角色标签为小林 / 阿暖（`#ffa94d`）/ 阿凉（`#8fd3ff`）（`render.mjs:78`）。
- 品牌：左上角 logo；片尾卡从 `E('bye')+0.3` 开始（约 174.6 s，持续约 2.6 s），标语“用地理的眼光，读懂身边的每一阵风”；页脚“资料：人教版地理必修第一册 · Stull《Practical Meteorology》· NASA Glenn”（`render.mjs:61-76, 109`）。

## 7. 迭代与问题

| 事项 | 证据 | 说明 |
| --- | --- | --- |
| **README 没有收录** | `README.md` 第二章第二节表格里没有 `thermal-circulation/` | 原因**无法确认**。可能的推断：标题和 [[sea-land-breeze]]（“海边的风，为什么白天晚上会掉头？”）的开场几乎一样，海陆风那段也有重叠，后来可能用海陆风专题替代了本片的位置；或者只是漏登。两片都有成片 |
| 与 [[sea-land-breeze]] 内容重叠 | 两片共用 `beach.mjs` 的写法（diff 只差参数化的太阳、月亮、棕榈和遮阳伞位置），`props.mjs` 几乎相同 | 本片在 21:22 先做，sea-land-breeze 在 21:50 开始（mtime）。**推断**：sea-land-breeze 是在本片目录的基础上扩展出来的 |
| 缺少 `based_on` | `script.json` keys | 不符合 AGENTS §1，只在 `source` 文字里写了 build 名 |
| 调色板沿用 air-pressure 的 | `lib.mjs` 里有 `bag/bagDark/n2/o2` | **推断**：本片是复制 [[air-pressure]] 后改出来的 |
| 规则符合度（代码中存在） | `brand()`、`outro()`、页脚只写资料来源 | 符合 AGENTS §4/§5；画面文字“人教版 必修第一册 · 大气运动”放在开场（`scenes-a.mjs:50`） |

预览帧、返工次数、用户反馈都**未记录**。

## 8. 复现入口

以下命令来自 README 的通用命令和 `render.mjs` 头部注释，本次**均未执行**：

```bash
cd thermal-circulation
npm install
python3 -m venv .venv && .venv/bin/pip install edge-tts   # fonts.py 另需 fontTools
.venv/bin/python tts.py
node render.mjs --still 60 90      # → build/still_<t>.png
node render.mjs                    # → out/海边的风为什么会掉头：一冷一热，空气转成一个圈.mp4
```

修改入口：机理图的数值（等压面偏移、空气份数）在 `diagram.mjs:12-14, 47-50`；各个动作对应的触发词在 `diagram.mjs:17-28` 的 `T` 表里。

## 9. 参考来源

项目文件里**没有 URL**（`urls: []`）。下表是文件中提到的资料名称，本次**均未访问**：

| 资料 | 用途 |
| --- | --- |
| 人教版地理必修第一册（热力环流定义、等压面、海陆风） | 术语、示意图规范 |
| Roland Stull《Practical Meteorology》Ch.11 Hydrostatic Thermal Circulations（p.341–342, Fig.11.17） | 膨胀与质量转移的先后顺序 |
| Thomas Birner (CSU)《General Circulation》讲义 幻灯 4–11 | 同上 |
| NASA Glenn《Equation of State》 | 静力平衡、高空“高压”仍低于近地面 |
| `geo-story-studio/storage/builds/douyin-thermal-circulation/v002/`（推断路径） | 分镜与科学审校 |

## 10. 证据与限制

| 文件 | 行数 | sha256[:12] | 读取程度 |
| --- | --- | --- | --- |
| `thermal-circulation/script.json` | 64 | `0a64bcb81906` | 全文 |
| `thermal-circulation/diagram.mjs` | 213 | `a96ea6873d60` | 全文 |
| `thermal-circulation/scenes-a…d.mjs` | 65/127/182/81 | `7d8e3574a2c6`/`d1ddde07ea4c`/`b91a1470b16a`/`36404ddd2b5d` | 结构全部；scenes-a 的 draw、scenes-b 的 PH、scenes-c 全部关键段 |
| `thermal-circulation/beach.mjs` / `props.mjs` / `cast.mjs` | 120/174/203 | `024702258471`/`34f8ce246a5a`/`ccc28f56f04a` | 导出与注释；读了 `loopFlow`/`pathPoint`；与 sea-land-breeze 版做了 diff |
| `thermal-circulation/render.mjs` / `lib.mjs` / `tts.py` | 190/218/209 | `431104244ff9`/`80773868fb56`/`158c5b687e2c` | 与 air-pressure / sky-colors 版 diff |
| `thermal-circulation/audio.mjs` / `fonts.py` | 204/37 | `f9e04b2cfd54`/`a6ac7264e548` | 与其他几部片相同 |
| `thermal-circulation/build/timeline.json` | — | — | 读取各段 start |
| `thermal-circulation/out/*.mp4` / `*.srt` | — | 26,161,623 B / 5,469 B | 只看 ffprobe 结果和 SRT 条数 |
| `README.md` | — | — | 搜索视频目录，确认没有本片 |

限制：没有播放视频，也没有试听；机理图的方向是读代码核对的，没有看渲染出来的画面。依赖版本和渲染耗时都未记录。
