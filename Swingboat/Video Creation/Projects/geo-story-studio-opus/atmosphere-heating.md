---
type: video-production-dossier
project: geo-story-studio-opus
video_id: atmosphere-heating
title: 地面怎样把大气暖起来
curriculum: script.json 未写 curriculum 字段；内容对应“大气受热过程·地面是对流层大气的主要直接热源”（推断）
captured: '2026-11-12'
status: 成片存在（竖屏 1080×1920，75.07 s）+ 封面 PNG；技术检查/视觉检查/用户验收未记录
source_fingerprint: 9852b29b50f13bf120b5fcf4cf159f7ba78c8b83ced9258c93ae7180d3ce8e8f
tags:
- video-creation
- production-capture
- geography-teaching
- canvas-frame-render
- edge-tts
---

# 地面怎样把大气暖起来 · “三条送热路线”竖屏动画（大气受热双片中的第一部）

<!-- capture-source-sha256:9852b29b50f13bf120b5fcf4cf159f7ba78c8b83ced9258c93ae7180d3ce8e8f -->

本文是根据项目文件做的材料回溯（`script.json`、`tts.py`、`render.mjs`、`lib.mjs`、`props.mjs`、`chars.mjs`、`scenes-a~d.mjs`、`audio.mjs`、`build/timeline.json`、`out/` 清单和证据包里的 ffprobe 结果）。**未发现制作会话**。本次未运行任何项目脚本，未打开视频、音频和封面。

本片与 [[atmosphere-insulation]] 是同一套代码的两部：本片先做（成片 10-22 20:26），保温片约 50 分钟后由本片复制改写（成片 21:14）。两片的 `lib.mjs`、`props.mjs`、`audio.mjs`、`fonts.py` sha256 完全相同，`tts.py` 只差标题和 `TARGET`。**TTS、时间轴、渲染循环、混音的详细说明见 [[atmosphere-insulation]] §5–§6**，本文只写本片特有的部分。

## 1. 当前结论与制作路线

**状态**：`out/地面怎样把大气暖起来.mp4`、`.srt`、`out/封面.png` 都在；视觉检查、试听和用户验收**未记录**。

| 项 | 值 | 来源 |
| --- | --- | --- |
| 分辨率 / 画幅 | 1080×1920，9:16 竖屏 | ffprobe；`lib.mjs:7` |
| 帧率 / 帧数 | 30 fps；2252 帧 | ffprobe；75.066×30 四舍五入 |
| 时长 | 75.067 s（timeline 75.066 s） | ffprobe；`build/timeline.json` |
| 编码 | H.264 yuv420p CRF 18 preset medium + AAC 48 kHz 立体声 192 kbps，faststart | ffprobe；`render.mjs`（同保温片） |
| 文件大小 | 15,040,014 字节（约 14.3 MiB） | 证据包 |
| 字幕 | 41 条 SRT + 画面烧录 | `out/地面怎样把大气暖起来.srt` |
| 封面 | `out/封面.png`，480,491 字节；取 `at('stove','地面') + 0.6` 时刻，胶囊文字“地面才是大气的暖炉” | 文件清单；`render.mjs` cover 分支（未查看画面） |
| 其他版本 | 无 | `out/` 清单 |

**管线**：与 [[atmosphere-insulation]] 相同——`script.json`（16 段）→ `tts.py`（edge-tts + WordBoundary + 缓存 + `TARGET = 76`）→ `timeline.json` / SRT / `narration.wav` → `render.mjs` 加载 4 个场景模块（8 个场景）逐帧绘制并管道送 ffmpeg，`audio.mjs` 合成配乐与音效 → MP4。本片 `rate` 停在脚本设定的 +6%（timeline `rate: 6`），首轮总长 75.066 s 未超 76 s 上限。

## 2. 教学目标与课标对齐

- **课标字段**：`script.json` 无 `curriculum`、无 `based_on`。`README.md:63`：“地面是对流层大气的主要直接热源：热红外辐射、对流、水的相变三条传热路线”。
- **改编来源**：`source` 写“Geo Story Studio《地面怎样把大气暖起来？》v003 分镜”，无 build 路径。
- **核心知识点**：太阳辐射先加热地面（大气也直接吸收一部分），地面再通过**辐射、对流（含传导）、水的相变（潜热）**把热量传给大气，所以对流层通常越高越冷。
- **讲解结构**：
  1. **导入**：`hook` 豆豆问“爬到山顶离太阳更近了，怎么反而更冷？”→ `far` 日地约 1.5 亿千米，多爬几千米几乎等于没靠近 → `stove` 给空气“烧暖气”的主要是脚下的地面。
  2. **原理**：`sunIn`/`airSun` 阳光一部分被地面吸收、地面先热；大气也直接吸收一部分 → `kidHow`/`three` 三条送热路线 → ① `ir1`/`ir2` 热红外“隐形快递”，水汽、二氧化碳接住一部分，也有一部分溜向太空 → ② `touch`/`rise` 紧贴地面的空气被焐暖，“条件合适时”暖空气上浮形成对流 → ③ `evap`/`cond` 蒸发从地面带走热量，水汽在高空凝结成云再放热。
  3. **结论**：`recap` 辐射、对流、水的相变，从下往上把大气烘暖。
  4. **迁移**：`cold` 对流层里离地面这个“暖炉”越远，通常越冷（回到山顶情境）。
  5. **总结**：`cta` 下次在山顶冻得发抖，就想想这三条路线。
- **科学边界**：`numbers.note`“只讲定性机制：箭头和卡片大小不表示能量占比，不是完整能量收支图”；文案用“主要是”“通常”“条件合适时”限定（`scenes-c.mjs:53` 画面也单独显示“条件合适时”）；`scenes-c.mjs:72` 横幅副标题“水汽本身看不见 · 图标只是示意”。
- **数字**：`numbers.sunDistance`“日地平均距离约 1.5 亿千米；登山多爬几千米，相比之下可以忽略”。资料名称：NASA Earth Observatory “Climate and Earth’s Energy Budget”（Surface / Atmosphere Energy Budget、Natural Greenhouse Effect）、NASA Langley CERES “Global Mean Energy Budget”。

## 3. 技术与依赖

与 [[atmosphere-insulation]] §3 相同：Node ESM + `@napi-rs/canvas`（`^1.0.11`，lock 1.0.13），无 npm scripts；Python `edge-tts`（运行时替换 `WSS_URL`）；`fonts.py` 依赖 fontTools 抽取圆体 / 苹方；系统 ffmpeg。版本均未记录。

| 角色 | voice | 实际语速 / 音调 | 段数 |
| --- | --- | --- | --- |
| 旁白（zh） | `zh-CN-XiaoxiaoNeural` | +6% | 14 |
| 豆豆（kid） | `zh-CN-YunxiaNeural` | +10% / +6 Hz（角色 +4 叠加全局 +6） | 2（hook、kidHow） |

`tts.py` 常量：`TARGET = 76.0`（注释“keep it around a minute”）、`LEAD_IN = 0.35`、`GAP = 0.3`、`TAIL = 2.2`（`tts.py:32-35`）。

## 4. 镜头与时间轴

时间来自 `build/timeline.json`（16 段，41 条字幕）；章节条 6 格：疑问 / 阳光 / 辐射 / 对流 / 水汽 / 揭晓；置顶标题“山顶离太阳更近 / 为啥反而更冷？”。

| # | 场景 | 约时间 (s) | 口播段 | 画面要点 | 文件 |
| --- | --- | --- | --- | --- | --- |
| 1 | `hookScene` | 0–9.14 | hook, far | 雪山顶发抖的豆豆、呼气白雾；“山顶 冷 / 山脚 暖”两支温度计；距离尺“约 1.5 亿千米”，山的那一端标红一丝“多爬的几千米 ≈ 这一丝”，盖章“没靠近!” | `scenes-a.mjs:22-77` |
| 2 | `stoveScene` | 9.14–20.49 | stove, sunIn, airSun | “地面 = 大气的暖炉”，阳光照到地面、地面先热起来；“大气也吸收一部分” | `scenes-a.mjs:81-115` |
| 3 | `routesScene` | 20.49–25.16 | kidHow, three | 豆豆问“热量怎么送上去?”，三张路线图标卡弹出 | `scenes-b.mjs:29-47` |
| 4 | `irScene` | 25.16–37.09 | ir1, ir2 | “① 隐形快递 · 热红外辐射”：橙色波射向水汽、二氧化碳角色，被接住后角色变暖“暖!”；一束虚线波溜向太空；“肉眼看不见” | `scenes-b.mjs:56-101` |
| 5 | `convectionScene` | 37.09–47.62 | touch, rise | “② 贴贴传热 · 对流”：4 个空气团落地被短箭头焐暖，中间两个沿虚线上浮、放大、带“热量火苗”，“条件合适时”“对流!” | `scenes-c.mjs:22-62` |
| 6 | `waterScene` | 47.62–57.43 | evap, cond | “③ 水的接力”：湖面水汽小人带着火苗跳起（蒸发吸热），在高空汇成云下雨，云把火苗分给周围 4 个空气团（凝结放热!） | `scenes-c.mjs:69-111` |
| 7 | `recapScene` | 57.43–63.35 | recap | 三条路线回顾，“从下往上，把大气烘暖” | `scenes-d.mjs:13-39` |
| 8 | `finaleScene` | 63.35–75.07 | cold, cta | 回到山：山脚橙色暖光、火苗上升渐隐；左侧竖向温度条“暖→冷”；“对流层：离暖炉越远，通常越冷”；“记住这三条送热路线!” | `scenes-d.mjs:43-…` |

最后 0.5 s 淡出；**无片尾品牌卡**。

## 5. 关键算法与实现

> 区分：视频**讲的知识**是“地面通过辐射、对流、相变加热大气”；下面是**制作用的画法**，都是定性示意，不是热力学或辐射计算。通用机制（端点替换、缓存、429 重试、WordBoundary 锚点、语速求解、逐帧管道、侧链混音）见 [[atmosphere-insulation]] §5。

### 5.1 距离尺：用“一丝”表现可以忽略（`scenes-a.mjs:47-77`）

```js
const bp = easeOut(prog(t, tYi - 0.3, tYi + 0.6));           // 讲到“一亿”时金线从山画到太阳
ctx.moveTo(150, 1040); ctx.lineTo(lerp(150, 790, bp), 1040);
const mp = pop(t, tPa - 0.1, 0.45);                           // 讲到“多爬”时弹出 3 px 红条
ctx.fillStyle = C.red; ctx.fillRect(150, 1030, 3, 20);
txt(ctx, '多爬的几千米 ≈ 这一丝', 330, 1092, …);
```

640 px 代表约 1.5 亿千米时，几千米按比例只有约 10⁻⁵ px（本次按代码坐标估算），3 px 红条是为了“看得见”而放大的；画面靠“≈ 这一丝”的文字表达，而不是精确比例。

### 5.2 三条路线各用一种运动语言

- **① 辐射**（`scenes-b.mjs:65-85`）：`irWave()` 从地面射向水汽 / 二氧化碳角色，到达后 `buddy({warm})` 从冷蓝渐变为橙并发光；第三束用虚线 `dash: [18, 14]` 穿过去“溜向太空”。气体角色按“水汽”“二氧化碳”的词时依次弹出。
- **② 传导 + 对流**（`scenes-c.mjs:27-46`）：4 个空气团先落到地面，`warm` 期间画两支向上短箭头（接触加热）；只有中间两团上浮，路径为二次贝塞尔虚线，上升时按 `sin(up·2π)·30·up` 左右摆动、放大 25%，并带一枚“热量火苗”。

```js
const up = riser ? easeIO(prog(t, tUp - 0.3 + (i - 1) * 0.35, tSend + 0.8 + (i - 1) * 0.35)) : 0;
const y = lerp(y0, i === 1 ? 760 : 820, up), xx = x + Math.sin(up * Math.PI * 2) * 30 * up;
```

- **③ 相变**（`scenes-c.mjs:76-106`）：热量用“火苗”这个可见代币表示——3 个水汽小人从湖面跳起时各拿一枚火苗（讲到“带走”），升到高空后在“凝结”处 `lerp` 汇合成云并缩小消失，云再把火苗沿直线分发给周围 4 个空气团（讲到“放给”），空气团随之变暖。“蒸发吸热 / 凝结放热”两个标签分别在对应词时弹出。

### 5.3 结尾的垂直温度条（`scenes-d.mjs:51-68`）

山脚放一团呼吸式橙色光晕代表“暖炉”，6 枚火苗按 `(t·0.35 + i/6) mod 1` 循环上升并逐渐变小、变淡；左侧温度条用自下而上的 `hot → gold → cool` 线性渐变，在“越远”到“越冷”两词之间从底部长到顶部，配“暖 / 冷”标签——把“对流层气温通常随高度降低”画成“离暖炉越远越冷”。

## 6. 声音与字幕

- 旁白 `zh-CN-XiaoxiaoNeural` +6%，豆豆 `zh-CN-YunxiaNeural` +10% / +6 Hz；`build/tts_cache/` 52 个文件（26 组），mp3 时间集中在 10-22 20:15–20:17。
- 旁白混音、配乐（124 BPM I–V–vi–IV 程序合成）、18 种程序音效、侧链压缩和 `alimiter 0.93` 与 [[atmosphere-insulation]] §6 完全相同（`audio.mjs` sha256 相同）。四个场景文件共 49 个 `events` 条目（a 13、b 13、c 11、d 12，按 `type:` 计数）。
- SRT 41 条：首条 `00:00:00,395 → 00:00:02,180 爬到山顶离太阳更近了`，末条 `00:01:10,753 → 00:01:12,746 就想想这三条送热路线吧！`。
- 本次未试听、未测响度。

## 7. 迭代与问题

| 事项 | 证据 | 说明 |
| --- | --- | --- |
| 文案有过改动（推断） | 缓存 26 组 vs 16 段，且最终 `rate` 等于脚本起始值 6，说明不是提速重合成 | 多出的 10 组最可能是改写过的句子；改了什么**无法确认** |
| 画面有“示意”类文字 | `props.mjs:131-143` `disclaimer()`“示意图 · 箭头不代表能量多少”（`scenes-a.mjs:114`、`scenes-b.mjs:100`、`scenes-c.mjs:61, 110`、`scenes-d.mjs:38` 调用）；`scenes-b.mjs:98`“橙色只是示意”；`scenes-c.mjs:72`“水汽本身看不见 · 图标只是示意” | 与 `AGENTS.md` §5 冲突；§5 写明已完成的视频不必回改 |
| 无 logo、无 `assets/`、无片尾品牌卡 | 目录清单；`render.mjs` 无 `brand()`；`TAIL = 2.2` | 不满足 `AGENTS.md` §4；本片早于该规则（推断） |
| 设了 `TARGET = 76` | `tts.py:32` | 现行 `AGENTS.md` §2 默认 `TARGET = None`；本片未触发提速，实际影响为零 |
| 缺 `curriculum` / `based_on` | `script.json` 键 | 不满足 `AGENTS.md` §1、§6 |
| 被复制为 [[atmosphere-insulation]] 的底板 | 4 个公共文件 sha256 相同；保温片 `lib.mjs:1` 仍写本片标题 | 保温片在 `chars.mjs` 给 hiker 加了 `coat` 参数，并新增 `night.mjs` |
| README | `README.md:63` 已收录，标 1:15 | 与 ffprobe 75.07 s 一致 |

## 8. 复现入口

命令来自 `AGENTS.md`“常用命令”与 `render.mjs` 头部注释，**本次未执行**：

```bash
cd atmosphere-heating
python3 -m venv .venv && .venv/bin/pip install edge-tts   # fonts.py 另需 fonttools（推断）
npm install
.venv/bin/python tts.py          # → build/timeline.json、build/narration.wav、out/地面怎样把大气暖起来.srt
node render.mjs --still 8.0 14.2 # 可加 --safe
node render.mjs                  # → out/地面怎样把大气暖起来.mp4
node render.mjs --cover          # → out/封面.png
```

先跑 `tts.py`（`lib.mjs` 加载时读 `build/timeline.json`）。缓存已含当前 16 段，**推断**可离线重建。

## 9. 参考来源

| 来源 | 用途 | 状态 |
| --- | --- | --- |
| Geo Story Studio《地面怎样把大气暖起来？》v003 分镜 | 改编底稿 | 只有名称，无 `based_on` 路径；未查看 |
| NASA Earth Observatory — Climate and Earth’s Energy Budget | 地表 / 大气能量收支、自然温室效应 | 项目文件中只提及名称，无 URL；本次未访问 |
| NASA Langley CERES — Global Mean Energy Budget | 全球平均能量收支 | 同上 |

证据包 `urls` 为空。

## 10. 证据与限制

已读文件（`atmosphere-heating/` 下，sha256 前 12 位）：`script.json` 38 行 `16625a6be8c6`（全文）；`tts.py` 208 行 `f53a4949b9d9`（与保温片 diff）；`render.mjs` 211 行 `e21cd528f5c3`（与保温片 diff）；`scenes-a.mjs` 137 行 `d1c3ed22e671`、`scenes-b.mjs` 123 行 `f65a7ae68655`、`scenes-c.mjs` 131 行 `3d82bb25cbc5`、`scenes-d.mjs` 114 行 `a140593068c2`（场景表、主要场景函数、画面文字）；`chars.mjs` 217 行 `876a49e06b9f`（与保温片 diff）；`lib.mjs` `c44f12ed8625`、`props.mjs` `3b6a75ea94a1`、`audio.mjs` `ef790f152784`、`fonts.py` `a6ac7264e548`（与保温片哈希相同，按保温片阅读结果）；`package.json` `bc7b4e468bc6`、`package-lock.json` `c84dbe91fcbd`；`build/timeline.json`；`out/*.srt` 首尾。成片仅有证据包 ffprobe。

限制：未发现制作会话；文案改动内容、预览次数和用户反馈**未记录**；未打开成片、封面和音频，画面遮挡与安全区**无法确认**；Node / Python / ffmpeg 版本未记录；git 只有初始化提交 `4831a07`（2026-10-23）。
