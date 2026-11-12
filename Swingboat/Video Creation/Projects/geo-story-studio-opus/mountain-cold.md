---
type: video-production-dossier
project: geo-story-studio-opus
video_id: mountain-cold
title: 离太阳更近，山上为什么反而更冷？
curriculum: 人教版必修第一册 第二章第一节（对流层气温随高度增加而降低）、第二章第二节 大气受热过程；课标1.4 及大气受热过程
captured: '2026-11-12'
status: 成片存在（竖屏 1080×1920，221.5 s）；未列入 README 目录；技术检查/视觉检查/用户验收未记录
source_fingerprint: 6cb453967e22428f6aa89d6a00172507bb6c35c4434a8e08f7ae0007e041393b
tags:
- video-creation
- production-capture
- geography-teaching
- canvas-frame-render
- edge-tts
---

# 离太阳更近，山上为什么反而更冷？ · 距离算账 + 「暖气片在脚下」+ 气团团膨胀冷却（Canvas 逐帧 + 多角色 edge-tts）

<!-- capture-source-sha256:6cb453967e22428f6aa89d6a00172507bb6c35c4434a8e08f7ae0007e041393b -->

本次为材料回溯与归档，结论来自项目文件（`script.json`、`tts.py`、`render.mjs`、`lib.mjs`、`cast.mjs`、`terrain.mjs`、`props.mjs`、`audio.mjs`、`scenes-a…f.mjs`、`build/timeline.json`、`out/` 清单与 ffprobe）。按时间戳**推断**对应的宿主会话为 `d40275d8-1f20-4995-9a8b-1531257f6550`（创建于 2026-10-31 17:20，早于 `script.json` 修改时间 17:27），只按元数据找到，**对话内容无法读取**（宿主返回空 transcript）。本轮未运行任何项目脚本，未打开视频或音频。

## 1. 当前结论与制作路线

**状态：成片存在**——`out/离太阳更近，山上为什么反而更冷.mp4` 与同名 `.srt`；视觉检查、试听、用户验收**未记录**。**本片没有列入项目 `README.md` 的视频目录**（检索 README 无 `mountain-cold`）。

| 项 | 值 | 来源 |
| --- | --- | --- |
| 分辨率 / 画幅 | 1080×1920，9:16 竖屏 | ffprobe；`lib.mjs:7` |
| 帧率 / 帧数 | 30 fps；约 6644 帧 | ffprobe；`Math.round(221.481 × 30)`（`render.mjs:188`） |
| 时长 | 221.467 s | ffprobe；`timeline.json` `duration: 221.481` |
| 视频 / 音频 | H.264（libx264 CRF 19，preset medium，yuv420p，faststart）/ AAC 48 kHz 立体声 192 kbps | ffprobe；`render.mjs:185-186` |
| 文件大小 | 31,542,430 字节（约 31.5 MB，平均约 1.14 Mbps） | 证据包 |
| 字幕 | SRT 103 条 + 画面烧录 | SRT 计数；`render.mjs:80-106` |
| 其他版本 | 无 | `out/` 清单 |

管线：`script.json`（35 段、4 个角色）→ `tts.py`（edge-tts + WordBoundary，`LEAD_IN 1.0`、`TAIL 2.8`）→ `narration.wav` / `timeline.json` / SRT → `render.mjs` 用 `Promise.all` 并行导入 `scenes-a…f`（10 个场景），共享 `cast.mjs`（人物与道具）、`terrain.mjs`（山体与山谷）、`props.mjs` → Canvas 逐帧 → ffmpeg。本片与 [[afternoon-heat]] 是另一套模板（「B 模板」：`render.mjs`、`lib.mjs`、`tts.py` 与 afternoon-heat 只差标题、背景色和资料表），和 [[air-composition]] / [[atmosphere-layers]] 的「A 模板」不同（见 §7）。文件时间：`script.json` 17:27 → TTS 17:30 → `cast.mjs`/`terrain.mjs` 17:32–17:33 → `render.mjs` 17:34 → 场景 17:35–17:51 → `scenes-b.mjs` 17:55 → 成片 17:59。

## 2. 教学目标与课标对齐

- **课标出处**（`script.json:3`）：必修第一册 第二章第一节（对流层气温随高度增加而降低）、第二章第二节 大气受热过程（地面是近地面大气主要的直接热源、大气的保温作用）；课标 1.4 及「说明大气的受热过程与原理」。
- **核心知识点**：山上冷与「离太阳远近」几乎无关；地面是近地面大气主要的直接热源 + 高处空气稀薄保温差 + 上升膨胀冷却 → 对流层气温随高度增加而降低（平均 0.6℃/100 m）。
- **讲解结构**（`timeline.json`）：
  - 导入：小林穿短袖爬 3000 米山，「阿嚏！说好的离太阳更近呢？」（`hike`…`q`）。
  - 先排除错误解释：日地约 1.5 亿千米 vs 3 千米；缩小一百万倍是 150 千米对 3 毫米；距离只变约五千万分之一；太阳「根本察觉不到」（`dist`…`conc1`）。
  - 原理：大气受热过程 → 「地面是近地面大气主要的直接热源」（教材原话卡）→ 暖气片比喻；小林反问「山顶也有地面」→ 高空一小块地面、空气稀薄水汽少、大气逆辐射弱（`where`…`obj3`）；气团团演示上升 → 气压低 → 膨胀 → 降温（`tuan0`…`tuan4`）。
  - 结论：对流层气温随高度增加而降低；0.6℃/100 m；小林算 3000 米降约 18℃，山脚 25℃ 山顶约 7℃，并提醒「实际还要看当天天气和地形」（`rule`…`calc2`）。
  - 迁移：晴朗风弱夜晚冷空气沿坡流入谷底形成逆温，果园建在山坡中部避霜冻；乞力马扎罗山（约南纬 3°、约 5895 米）山顶有冰川积雪（`always`…`kili2`）。
  - 总结：答题关键词三条 → 小林「怪暖气片在脚下」→ 一句话总结 →「羽绒服我带上了」（`exam`…`pack`）。
- **科学口径来源**：本片没有 `based_on` 字段，改用 `source`（`script.json:4`）：「Geo Story Studio · douyin-mountain-cold v003 分镜与科学审校（review-002，15 项结论的建议表述）」，以及教材、NASA Science Sun Facts、Britannica Kilimanjaro、科普中国《逆温层》。源 build 路径未写明，按项目惯例应为 `geo-story-studio/storage/builds/douyin-mountain-cold/v003`（**推断**，本次未读取）。
- **数字与出处**（`script.json:5-15`，键值对形式）：日地平均距离约 1.5 亿千米（NASA）；3 ÷ 150 000 000 = 1/50 000 000（注明「理想最大几何靠近，其他条件不变」，同源 build 的 `distance-calculation.json`）；缩小 10⁶ 倍 150 km 对 3 mm；0.6℃/100 m 只作平均估算（审校 claim-003/014）；3000 米约降 18℃；夜间谷底逆温（审校 claim-015、科普中国）；乞力马扎罗约 5895 米、3°04′S（Britannica）。

## 3. 技术与依赖

| 类别 | 内容 | 证据 |
| --- | --- | --- |
| 渲染 | Node.js ESM + `@napi-rs/canvas` 声明 `^1.0.11`，锁定 1.1.0（darwin-arm64 1.1.0） | `package.json`（`private: true`，**无 scripts 字段**）；`package-lock.json` |
| TTS | Python `edge-tts` + `aiohttp`；版本未记录 | `tts.py:16-18` |
| 字体 | `fonts.py`（fontTools，与其他视频同哈希 `a6ac7264e548`） | 证据包 |
| ffmpeg | 旁白 `adelay`/`amix`/`highpass`/`loudnorm`（本模板**不支持**角色 `fx`）；总混音 `sidechaincompress`/`amix`/`alimiter` | `tts.py:177-190`；`render.mjs:177-187` |

| 角色 key | 标签 | 声音 | rate（角色 + 全局 4） | pitch |
| --- | --- | --- | --- | --- |
| `zh` | 旁白 | zh-CN-XiaoxiaoNeural | +4% | 0 |
| `stu` | 小林 | zh-CN-YunxiNeural | +10% | +2 Hz |
| `sun` | 太阳 | zh-CN-YunjianNeural | 0%（−4 + 4） | −4 Hz |
| `tuan` | 气团团 | zh-CN-XiaoyiNeural | +12% | +6 Hz |

`build/tts_cache/` 70 个文件 = 35 段 × (mp3 + json)，只有一批语速；`TARGET = None`；`timeline.json` `rate: 4`。

## 4. 镜头与时间轴

时间来自 `build/timeline.json`；场景边界为下一场景首段 `S(id) − 0.3`，交叉淡化 `XF = 0.4`（`render.mjs:17`），本模板只做透明度淡化、不做缩放（`render.mjs:128-137`）。每次切场景自动加一声 whoosh（`render.mjs:158`）。

| # | 场景 id | 约时间 (s) | 旁白要点 | 画面 | 模块 |
| --- | --- | --- | --- | --- | --- |
| 1 | `hook` | 0–21.5 | 短袖爬山 → 山顶打喷嚏 → 提问 | 小林沿山路上行、穿短袖到发抖，羽绒服 | `scenes-a.mjs:23` |
| 2 | `dist` | 21.5–52.3 | 1.5 亿千米 vs 3 千米；缩小百万倍；五千万分之一；太阳耸肩 | 三行算账：地日距离 → 150 千米公路 → 放大镜下 3 mm vs 米粒；1/50 000 000 卡片 | `scenes-b.mjs:38` |
| 3 | `heat` | 52.3–78.8 | 大气受热过程；教材原话；暖气片比喻 | 太阳辐射 → 地面 → 地面辐射；翻开的教材卡「地面是近地面大气主要的直接热源」；暖气片 | `scenes-c.mjs:11`（`bookCard` 在 56 行） |
| 4 | `summit` | 78.8–99.5 | 山顶也有地面？稀薄、水汽少、逆辐射弱 | 高空一小块地面被冷空气包围；高低处逆辐射对比标签 | `scenes-c.mjs:86` |
| 5 | `parcel` | 99.5–125.8 | 气团团上升、变胖、变冷；膨胀冷却 | 高度/气压轴，气团随词变大、由橙变蓝、打喷嚏震动；「上升→膨胀→消耗能量→降温」因果链 | `scenes-d.mjs:22` |
| 6 | `rule` | 125.8–153.2 | 气温随高度增加而降低；0.6℃/100 m；3000 米降 18℃ | 山体标尺（暖→冷渐变），25/19/13/7℃ 标签；山脚短袖、山顶羽绒服两个小林 | `scenes-d.mjs:76-138` |
| 7 | `inversion` | 153.2–178.0 | 越高越冷一定吗？谷底逆温；果园在山坡中部 | 夜间山谷：冷空气沿两坡下滑、在谷底积成冷湖；坡腰果园；谷底与坡上温度计 | `scenes-e.mjs:21` |
| 8 | `kili` | 178.0–193.2 | 乞力马扎罗：近赤道、约 5895 米、有冰川积雪 | 小地球仪标赤道与南纬约 3°；山脚草原长颈鹿、热浪，山顶「白帽子」 | `scenes-e.mjs:115` |
| 9 | `exam` | 193.2–205.4 | 答题关键词三条 | 答题卡逐条打勾 | `scenes-f.mjs:17` |
| 10 | `wrap` | 205.4–221.5 | 小林总结、一句话、带羽绒服 | 小林穿羽绒服站在暖气片上 | `scenes-f.mjs:52` |
| — | 品牌卡（`outro`） | 218.9–221.5 | 无旁白 | 背景压暗 0.82，居中 760 px logo，标语「用地理的眼光，看懂山上的冷暖」，`sparkle` 音效 | `render.mjs:60-75, 159` |

## 5. 关键算法与实现

> 区分：视频讲解的是大气受热过程与对流层气温垂直递减；下面是**制作算法**。山体、气团、冷湖都是卡通绘制，不是热力学或地形模拟。

### 5.1 共用 TTS 时间轴（B 模板）

- WordBoundary 逐词时间 → `lib.mjs:36-45` `word()/at()`；SRT 与字幕同源。
- 缓存键 `sha1(voice|rate|pitch|text)[:16]`、429 退避重试、`WSS_URL` 替换与 A 模板逐行相同；B 模板把 `available` 的计算挪到循环外并兼容 `TARGET = None`（`tts.py:147`），去掉了 `fx` 滤镜支持，`LEAD_IN` 1.0、`TAIL` 2.8（`tts.py:32-36`）。

### 5.2 距离算账：用同一套画面换尺度（`scenes-b.mjs:38-147`）

三行依次出现：「地球 ←约1.5亿千米→ 太阳」→ 盖章「全部缩小 100 万倍」后画出 150 千米公路（宽度 `lerp(0, 860, roadP)` 随词展开）→ 放大镜看起点，3 mm 括注与米粒并排；随后 1/50 000 000 卡片弹出。数字全部来自 `script.json` `numbers` 的算式，画面只做比例对照，不按真实像素比例画日地距离。

### 5.3 气团团：上升、膨胀、变冷三条进度曲线（`scenes-d.mjs:37-43`）

```js
const rise = easeIO(prog(t, tUp, tTop + 0.2));
const swell = easeOut(prog(t, at('tuan2', '越往上'), tFat + 0.6));
const cool = easeIO(prog(t, at('tuan2', '气压越低'), tCold + 0.5));
const [sx, sy] = shakeOf(t, tSn, 22, 0.6);
const px = 560 + sx, py = lerp(1240, 760, rise) + sy;
const r = lerp(105, 175, swell);
```

三条曲线分别锚在「往上飞」「越往上」「气压越低 / 变冷」等词上，所以上升、变大、变色和旁白同步。`cast.mjs:196-233` 的 `parcel()` 把 `warmth` 从 1 到 0 线性插值为橙→冰蓝（RGB 三通道各自 `lerp`），`squeeze` 画 8 支向内的压力箭头，长度随周围气压减小而缩短。

### 5.4 山谷冷湖（`scenes-e.mjs:47-71`）

冷空气小椭圆沿两坡 `slopePt(side, ph)` 循环下滑，透明度 `sin(π·ph)` 两端淡入淡出；谷底冷湖高度 `210·pool` 逐渐上涨，湖面左右端点按坡面线性插值，湖面加 `sin(2t)` 轻微起伏。「冷空气像水一样流进谷底」的比喻直接对应到画面。

### 5.5 山体轮廓与登山路径（`terrain.mjs:5-20`）

山脊用 11 个归一化折点 `RIDGE` 描述，`ridgeY(fx)` 分段线性插值；`trail(p)` 把登山进度 p∈[0,1] 映射到左坡上的点，小林在 hook 场景中沿它上山，标尺场景也复用同一套山体。

### 5.6 分段资料页脚与品牌层（`render.mjs:45-75, 108-125`）

- `FOOT` 表按段落区间显示不同来源：距离段「NASA Science · 平均日地距离约1.5亿千米」、受热段「人教版…大气受热过程」、逆温段「科普中国 · 逆温层」、乞力马扎罗段「Britannica」等，替代「示意」类声明。
- 角标 logo 64/76/290，0.3–1.0 s 淡入，`OUTRO = E('pack') + 0.3` 起 0.4 s 淡出；字幕在 `t > OUTRO` 后停止（`render.mjs:81`）。

## 6. 声音与字幕

| 项 | 内容 | 来源 |
| --- | --- | --- |
| 声音 | 旁白 / 小林 / 太阳（低沉放慢）/ 气团团（高音调），见 §3 | `script.json` `voices` |
| 旁白混音 | `aresample 48k` → `adelay` → `amix normalize=0` → `highpass=f=80` → `loudnorm=I=-16:TP=-1.5:LRA=11` | `tts.py:177-190` |
| 时间参数 | `LEAD_IN 1.0`、`GAP 0.45`（逐段另设）、`TAIL 2.8` | `tts.py:33-36` |
| 配乐 | 程序合成 112 BPM，C–G–Am–F 拨弦 + 低音 + 沙锤 + 钟琴，种子 42，淡入 1.2 s / 淡出 2.5 s | `audio.mjs:150-185` |
| 音效 | 15 种（pop、note、ding、sparkle、whoosh、boing、slam、stamp、tick、scribble、buzz、alarm、tada、confetti、wind）；场景切换自动 whoosh（400→1800 Hz）、品牌卡 sparkle | `audio.mjs:78-140`；`render.mjs:156-160` |
| 总混音 | 配乐 **×0.55**（A 模板为 0.75）经旁白侧链压缩，音效 ×0.5，`alimiter 0.93` | `render.mjs:182-183` |
| 字幕 | SRT 103 条；烧录 `SUB_Y = 1560`，字号 56 起按宽度递减；说话人标签小林（天蓝）、太阳（金）、气团团（橙）；品牌卡出现后停止 | `render.mjs:18, 78-106` |

未试听、未测响度。

## 7. 迭代与问题

| 事项 | 证据 | 说明 |
| --- | --- | --- |
| **未列入 README 目录** | `README.md` 中检索不到 `mountain-cold` | 成片已存在，但项目目录表缺这一行 |
| 与 [[atmosphere-layers]] 选题重叠 | atmosphere-layers 的导入、计算题（20℃→约 2℃）和结尾「离地面这个大暖炉远了」与本片主线相同 | 本片把这个问题单独展开（距离算账、山顶也有地面的反驳、逆温、乞力马扎罗）；两片同日制作，关系**未记录** |
| `outro.tagline` 没有传到时间轴 | `script.json:67` 有 `outro.tagline`；`tts.py:194-197` 只写 `duration/rate/voices/segments`；`render.mjs:73` 用 `TL.outro?.tagline ?? '用地理的眼光，看懂山上的冷暖'` | 实际显示的是硬编码默认值，正好与 `script.json` 相同，所以画面无差别；改标语时只改 `script.json` 不会生效 |
| 无 `based_on` 字段 | `script.json:4` 用 `source` 描述 v003 分镜与 review-002 | 与 AGENTS.md §1「源路径写进 `based_on`」的约定不一致 |
| 模板残留 | `lib.mjs:63-72` 颜色表含 `neb`、`ring`、`core`、`rad`、`photo`、`basinG` 等本片不用的键 | 推断 B 模板继承自天文类或 plateau-basin-sunlight 等前作 |
| `scenes-b.mjs` 最后修改 | 17:55，晚于其他场景文件（17:35–17:51），早于成片 17:59 | 距离算账场景可能在预览后返工（**推断**） |
| `package.json` 无 scripts | `package.json` 只有 name/version/private/type/dependencies | 渲染命令只在注释和 AGENTS.md 中 |
| 规则符合情况（代码中存在） | 角标 logo、品牌卡（字幕停、角标淡出）、分段资料页脚；未检出「示意」类声明 | 画面效果未核对 |

## 8. 复现入口

命令来自 AGENTS.md「常用命令」与 `render.mjs:2-3`，**本次均未执行**：

```bash
cd mountain-cold
npm install
python3 -m venv .venv && .venv/bin/pip install edge-tts   # fonts.py 另需 fontTools
.venv/bin/python tts.py              # → build/narration.wav、build/timeline.json、out/离太阳更近，山上为什么反而更冷.srt
node render.mjs --still 40 110 165   # 预览帧 → build/still_<t>.png
node render.mjs                      # → out/离太阳更近，山上为什么反而更冷.mp4
```

修改入口：资料页脚 → `render.mjs:109-116`；品牌卡标语 → `render.mjs:73`（见 §7）；山体 → `terrain.mjs`；气团 → `cast.mjs:196-233`；标尺温度 → `scenes-d.mjs:77`。

## 9. 参考来源

`script.json` 没有 URL 字段，代码中也没有 URL；只有名称引用：

| 来源 | 用途 | 状态 |
| --- | --- | --- |
| Geo Story Studio · douyin-mountain-cold v003 分镜与科学审校（review-002） | 改编来源与 15 项结论表述 | `script.json:4`；本次未读取 |
| 人教版地理必修第一册 第二章第一、二节 | 受热过程、0.6℃/100 m | `script.json` `numbers` |
| NASA Science「Sun: Facts」 | 日地平均距离约 1.5 亿千米 | 仅名称，无 URL |
| Encyclopaedia Britannica「Kilimanjaro」 | 5895 m、3°04′S、冰川积雪 | 仅名称，无 URL |
| 科普中国《逆温层》 | 谷底逆温、坡腰果园 | 仅名称（注明 kepuchina.cn），无具体 URL |

## 10. 证据与限制

| 文件 | 行数 | sha256[:12] | 读取程度 |
| --- | --- | --- | --- |
| `mountain-cold/script.json` | 69 | `3c6e1a1994b0` | 全部字段（段落经 timeline 读出） |
| `mountain-cold/tts.py` | 209 | `4b8538a860f2` | 与 air-composition diff |
| `mountain-cold/render.mjs` | 199 | `c7238b783bd7` | 全文 |
| `mountain-cold/lib.mjs` | 223 | `3d0d239da565` | 1-72 行与导出表 |
| `mountain-cold/cast.mjs` | 262 | `db3bd8032e54` | 导出表、`parcel()` |
| `mountain-cold/terrain.mjs` | 129 | `56b03c6a77c1` | 导出表、1-21 行 |
| `mountain-cold/props.mjs` | 176 | `e241ead4a636` | 导出表与注释 |
| `mountain-cold/audio.mjs` | 204 | `f9e04b2cfd54` | 音效名、配乐参数 |
| `mountain-cold/scenes-a…f.mjs` | 100/164/159/159/174/92 | `c6029d240640` / `267fdac40f5f` / `20ada58884b7` / `3a008424d1d6` / `79713d9a5bb9` / `04ad93d9c9c0` | 头注释、函数与 `scenes` 表；距离、气团、冷湖片段 |
| `mountain-cold/build/timeline.json` | — | — | 全部段落时间与文字 |
| `mountain-cold/out/离太阳更近，山上为什么反而更冷.mp4` | — | 31,542,430 字节 | 仅 ffprobe |

限制：宿主会话不可读；需求、预览、用户反馈未记录；未运行脚本、未看画面、未试听；依赖版本未记录；git 仅有批量提交 `8cbe673`（2026-11-01）。
