---
type: video-production-dossier
project: geo-story-studio-opus
video_id: atmosphere-insulation
title: 大气怎样给地面保温
curriculum: script.json 未写 curriculum 字段；内容对应“大气受热过程·保温作用”（AGENTS.md 示例“人教版必修第一册 第二章第二节 大气受热过程”，对应关系为推断）
captured: '2026-11-12'
status: 成片存在（竖屏 1080×1920，85.53 s）+ 封面 PNG；技术检查/视觉检查/用户验收未记录
source_fingerprint: 5378dee3bf25d03767d5eb3288eca0cfdd8c802beeaeeab97ea44fede87e7978
tags:
- video-creation
- production-capture
- geography-teaching
- canvas-frame-render
- edge-tts
---

# 大气怎样给地面保温 · 项目参考实现：edge-tts 逐词时间轴驱动的竖屏 Canvas 逐帧动画

<!-- capture-source-sha256:5378dee3bf25d03767d5eb3288eca0cfdd8c802beeaeeab97ea44fede87e7978 -->

本文是根据项目文件做的材料回溯（`script.json`、`tts.py`、`render.mjs`、`lib.mjs`、`props.mjs`、`chars.mjs`、`night.mjs`、`scenes-a~d.mjs`、`audio.mjs`、`build/timeline.json`、`out/` 目录清单与证据包中的 ffprobe 结果）。**未发现制作会话**，文案确认过程、预览轮次与用户反馈均无记录。本次未运行任何项目脚本（未跑 `tts.py`、`render.mjs`、npm、pip），未打开视频、音频与封面图。

`AGENTS.md:3` 把本目录声明为整个项目的**参考实现**，`AGENTS.md` §2 指定 `atmosphere-insulation/tts.py` 为 TTS 标准实现。因此本文对 TTS → 时间轴 → 逐帧渲染 → 混音合成的管线写得最细，其他视频的档案可直接链接到这里。需要注意：本片完成于项目规则成形之前，它**本身不满足**后来 `AGENTS.md` §4（logo、片尾品牌卡）、§5（不放“示意”声明）、§6（`curriculum` 字段）的要求，见 §7。

## 1. 当前结论与制作路线

**状态**：`out/大气怎样给地面保温.mp4`、`out/大气怎样给地面保温.srt`、`out/封面.png` 都在。这只说明“成片存在”，视觉检查、试听、响度测量和用户验收**均未记录**。

| 项 | 值 | 来源 |
| --- | --- | --- |
| 分辨率 / 画幅 | 1080×1920，9:16 竖屏 | ffprobe；`lib.mjs:7` `W = 1080, H = 1920, FPS = 30` |
| 帧率 / 帧数 | 30 fps；2566 帧 | ffprobe；`render.mjs:209` `Math.round(DURATION * FPS)`，85.527×30 → 2566 |
| 时长 | 85.533 s（timeline 85.527 s） | ffprobe；`build/timeline.json` `duration` |
| 视频编码 | H.264，yuv420p，CRF 18，preset medium，`+faststart` | ffprobe（codec）；`render.mjs:206-207` |
| 音频编码 | AAC 48 kHz 立体声，192 kbps | ffprobe；`render.mjs:207` |
| 文件大小 | 17,557,323 字节（约 16.7 MiB） | 证据包 |
| 字幕 | 55 条 SRT（`out/大气怎样给地面保温.srt`，3,067 字节）+ 画面烧录 | 文件；`render.mjs:83-109` |
| 封面 | `out/封面.png`，262,106 字节 | 文件清单；`render.mjs:189-192`（未查看画面） |
| 其他版本 | 无 | `out/` 清单 |

**一句话管线**：

> `script.json`（19 段，旁白 + 豆豆）→ `tts.py`：edge-tts 逐句合成（缓存、429 重试、新端点）并取 `WordBoundary` 逐词时间；总长超 `TARGET = 86` s 时一次算出新语速重合成（最终 +14%）→ 排时间轴，写 `build/timeline.json`、SRT，ffmpeg 混成 `build/narration.wav` → `render.mjs` 加载 4 个场景模块（12 个场景），逐帧绘制 RGBA 经管道送 ffmpeg，`audio.mjs` 程序合成配乐与音效并侧链混音 → MP4；`--still` 抽帧、`--cover` 出封面。

## 2. 教学目标与课标对齐

- **课标字段**：`script.json` 只有 `title / source / numbers / rate / voices / segments` 六个键，**没有 `curriculum`**，也没有 `based_on`。`README.md:64` 的目录说明：“大气吸收地面辐射，并以大气逆辐射把热量还给地面；为什么多云的冬夜不那么冷”。
- **核心知识点**：大气对地面的保温作用——大气吸收地面长波（热红外）辐射，并以大气逆辐射的形式把一部分能量还给地面，使地面净散热减少。
- **改编来源**：`script.json` `source` 写明“Geo Story Studio《大气怎样给地面保温？》v002 分镜（claim-043～046）”，但没有 build 路径。
- **讲解结构**（按段落 id 还原）：
  1. **导入**：`hook` 豆豆问有云的冬夜为啥没那么冷 → `quilt` 比喻“看不见的被子”，画面补“打个比方，不是真有一层布”（`scenes-a.mjs:66`）。
  2. **原理**：`emit` 地面昼夜辐射 → `kidLeak` → `absorb`/`escape` 水汽、二氧化碳和云吸收一部分，一部分溜向太空 → `allDir`/`counter` 大气向四面八方发热红外，向下的叫大气逆辐射 → `notBounce` 不是反弹，是大气自己发出。
  3. **结论**：`ledger`/`ledger2` 地面温度一样时支出一样，收下逆辐射后净散热变少 = 保温。
  4. **迁移**：`cloudy`/`clear` 有云夜降温慢、晴夜降温快易结霜 → `kidStop`/`noStop`/`budget` 保温不等于停止散热，冷暖看能量收支。
  5. **总结**：`recap`/`recap2` 四行清单 → `cta` 晴朗冬夜多穿一件。
- **科学边界**（`script.json` `numbers.note`）：只讲定性机制；箭头、卡片和柱子长短不表示能量占比，不是完整能量收支图；净散热比较的前提是地面温度和性质相同；逆辐射是大气自己发出的，不是地面辐射被反弹。这些边界在画面中分别落实为：账本场景的“前提：地面温度、性质都一样”标签（`scenes-c.mjs:39`），橙色 = 地面发出、青色 = 大气发出的配色约定（`night.mjs:2`），误区对比面板（`scenes-b.mjs` mythScene）。
- **数字与资料**：全片不使用具体数值。资料只列名称：NASA Earth Observatory “Climate and Earth’s Energy Budget”（natural greenhouse effect）、NASA Langley CERES “Global Mean Energy Budget”、NASA Science “The Earth’s Radiation Budget”。

## 3. 技术与依赖

| 类别 | 内容 | 证据 |
| --- | --- | --- |
| 渲染 | Node.js ESM（`"type": "module"`）+ `@napi-rs/canvas`，`package.json` 声明 `^1.0.11`，lock 解析为 **1.0.13** | `package.json`；`package-lock.json` |
| npm scripts | 无 | `package.json` |
| TTS | Python `edge-tts`（导入 `aiohttp`、`edge_tts.communicate`），版本未记录 | `tts.py:15-17` |
| 字体抽取 | `fonts.py` 用 `fontTools.ttLib.TTCollection` 从 macOS 系统字体集合中抽出“圆体-简 粗体”“苹方-简 中粗”；`lib.mjs:11-13` 在缺字体时自动调用 `.venv/bin/python fonts.py` | `fonts.py:9-16`；`lib.mjs:11-20` |
| 回退字体 | Arial Rounded Bold、Arial Unicode、Hiragino Sans GB（系统路径） | `lib.mjs:17-19` |
| 编码 / 混音 | 系统 ffmpeg：`adelay`、`amix`、`highpass`、`loudnorm`、`apad`、`atrim`（TTS）；`sidechaincompress`、`alimiter`、libx264、aac（成片） | `tts.py:176-190`；`render.mjs:197-208` |
| 版本 | Node、Python、ffmpeg 版本均未记录 | — |

声音配置（`script.json:7-10`，`build/timeline.json` 最终 `rate = 14`）：

| 角色 | voice | 脚本设定 | 实际合成（全局 `rate` 叠加后） | 段数 |
| --- | --- | --- | --- | --- |
| 旁白（zh） | `zh-CN-XiaoxiaoNeural` | 全局 +6% | **+14%** | 15 |
| 豆豆（kid） | `zh-CN-YunxiaNeural` | 角色 +4% / +6 Hz | **+18% / +6 Hz** | 4（hook、kidLeak、kidStop、cta） |

## 4. 镜头与时间轴

时间来自 `build/timeline.json`（`rate 14`，19 段，55 条字幕）。场景一般在下一段开口前 0.3 s 切入（第 2 场为 0.25 s），交叉淡化 `XF = 0.3`（`render.mjs:21`）。顶部章节条 6 格：疑问 / 辐射 / 吸收 / 逆辐射 / 记账 / 收支（`render.mjs:26`）。

| # | 场景函数 | 约时间 (s) | 口播段 | 画面要点 | 文件 |
| --- | --- | --- | --- | --- | --- |
| 1 | `hookScene` | 0–4.05 | hook | 晴夜 / 有云夜左右分屏：晴夜结霜、豆豆发抖、温度计下降；云朵飘入右屏、温度计回升；中间“?” | `scenes-a.mjs:16-55` |
| 2 | `quiltScene` | 4.05–7.36 | quilt | 半透明拼布“被子”从天而降罩住“地面君”，横幅“大气 ≈ 看不见的被子”+ 小字比喻说明 | `scenes-a.mjs:59-67` |
| 3 | `emitScene` | 7.36–13.96 | emit, kidLeak | 昼夜转盘（日↔月），地面持续冒橙色热红外；豆豆“全跑光了?!” | `scenes-a.mjs:71-110` |
| 4 | `absorbScene` | 13.96–22.90 | absorb, escape | 水汽、二氧化碳、云三个角色逐词弹出，橙色波终止在气体内（被吸收）；一条虚线波穿过空隙飞向太空 | `scenes-b.mjs:28-53` |
| 5 | `counterScene` | 22.90–31.55 | allDir, counter | 每个气体向 7 个方向发青色波；讲到“向下”时其余方向变暗，三条青色波射向地面，横幅“向下的 = 大气逆辐射” | `scenes-b.mjs:57-79` |
| 6 | `mythScene` | 31.55–36.51 | notBounce | “误区警报！”左“反弹回来”✗ / 右“大气自己发出”✓ | `scenes-b.mjs:82-…` |
| 7 | `ledgerScene` | 36.51–45.72 | ledger, ledger2 | “地面的长波账本”：两列（假如没有逆辐射 / 有大气逆辐射），支出相等、收入 0 vs 青条，青条滑入净散热行“吃掉”一段，盖章“保温！” | `scenes-c.mjs:23-82` |
| 8 | `nightScene` | 45.72–54.56 | cloudy, clear | 再次分屏：有云夜 3 条粗青波（逆辐射强）、温度计慢降；晴夜 1 条细虚线、快降、结霜 | `scenes-c.mjs:90-…` |
| 9 | `kidScene` | 54.56–60.84 | kidStop, noStop 前半 | 豆豆“那地球就不散热啦？”→“当然不是” | `scenes-d.mjs:14-35` |
| 10 | `earthScene` | 60.84–70.53 | noStop 后半, budget | 地球四周放出粉色热红外（支出）；太阳金色箭头（收入）；温度计上下摆动“↑↓” | `scenes-d.mjs:38-80` |
| 11 | `recapScene` | 70.53–79.60 | recap, recap2 | 四行清单逐词打勾：吸收 / 逆辐射 / 减少净散热 = 保温 / 地球仍在散热，冷暖看收支 | `scenes-d.mjs:83-112` |
| 12 | `finaleScene` | 79.60–85.53 | cta | 豆豆穿上紫色羽绒服跳起，“懂了!”，横幅“晴朗冬夜，记得多穿一件！”，彩纸 | `scenes-d.mjs:114-130` |

最后 0.5 s 整体淡出到深色（`render.mjs:169-173`）；**没有片尾品牌卡**。

## 5. 关键算法与实现

> 区分：视频**讲的知识**是“大气受热过程中的保温作用”；本节写的是**制作用的算法**（TTS 时间轴、Canvas 2D 示意动画、程序化音频），不是辐射传输计算。画面中的波长、条长都是定性的。

### 5.1 TTS 端点替换、缓存键与 429 退避（`tts.py:19-25, 44-66`）

以下仅摘录缓存与重试逻辑：

```python
key = hashlib.sha1(f"{voice}|{rate}|{pitch}|{text}".encode()).hexdigest()[:16]
for attempt in range(5):
    try: words, audio = await _stream(text, voice, rate, pitch); break
    except aiohttp.WSServerHandshakeError as e:
        if e.status != 429 or attempt == 4: raise
        await asyncio.sleep(8 * (attempt + 1))
```

- 端点在运行时替换，不改 `.venv` 里的库（注释：2026-10 起旧主机返回 403，待 rany2/edge-tts#490、#491 发布后删除），即 `AGENTS.md` §2 要求的写法。
- 缓存键 = `sha1(voice|rate|pitch|text)[:16]`，同时存 `.mp3` 与逐词时间 `.json`；改一句只重合成一句。429 退避 8/16/24/32 s，最多 5 次。

### 5.2 WordBoundary → 字符下标 → 字幕与动画锚点（`tts.py:69-85, 122-136`；`lib.mjs:30-40`）

```python
comm = edge_tts.Communicate(text, voice, rate=f"{rate:+d}%", pitch=f"{pitch:+d}Hz", boundary="WordBoundary")
idx = text.find(chunk["text"], cursor)        # 每个词映射回原句字符位置
start = chunk["offset"] / TICKS               # 100 ns → 秒
```

- 字幕：19 段中 16 段在 `script.json` 手写 `captions`；`caption_cues()` 在原句中定位分句区间 `[a, b)`，取区间内首词 `start`、末词 `end`。未写的段用 `split_caption()` 按标点切、单条 ≤12 字。
- 动画：`at(id, '词')` 用同一 `idx` 查出该词全片开口时刻。所有弹出、切换、音效都写成 `at('ledger2', '变少')` 这样的锚点而非秒数——改文案或语速后重跑 `tts.py`，画面自动对齐。

### 5.3 时长上限与“一步到位”的语速求解（`tts.py:145-158`）

```python
available = TARGET - LEAD_IN - sum(gaps) - TAIL
speech = [w[-1]["end"] + 0.12 for w in words]   # 以末词结束计，避开 mp3 尾部约 0.6 s 静音
total = LEAD_IN + sum(speech) + sum(gaps) + TAIL
if total <= TARGET or rate >= 30: break
rate = min(30, max(rate + 2, math.ceil(rate + (sum(speech) / available - 1) * 100) + 1))
```

本片 `TARGET = 86.0`、`LEAD_IN = 0.35`、`GAP = 0.3`、`TAIL = 2.2`（`tts.py:32-35`）。脚本 `rate: 6`，timeline 最终 `rate: 14`，总长 85.527 s。`build/tts_cache` 76 个文件 = 19 段 × 2 种语速，时间集中在 21:02–21:05，符合“先 +6%、超时后一次跳到 +14%”（推断）。这与现行 `AGENTS.md` §2 不一致，见 §7。

### 5.4 时间轴与旁白混音（`tts.py:160-190`）

段起点 `t_{i+1} = t_i + (末词结束 + 0.12) + gap_i`，首段从 0.35 s 开口；`gap` 可逐段写（本片 0.15–0.5 s，章节间留长停顿）。ffmpeg：每段 `aresample=48000,adelay` → `amix=inputs=19:normalize=0` → `highpass=f=80` → `loudnorm=I=-16:TP=-1.5:LRA=11` → `apad,atrim` → `build/narration.wav`。

### 5.5 逐帧渲染循环（`render.mjs:17-20, 142-174, 209-215`）

```js
const PARTS = ['a', 'b', 'c', 'd'].filter((k) => existsSync(here(`./scenes-${k}.mjs`)));
const MODS = await Promise.all(PARTS.map((k) => import(`./scenes-${k}.mjs`)));
const enter = s.from <= 0 ? 1 : easeOut(prog(t, s.from, s.from + XF));
const leave = s.to >= DURATION ? 1 : 1 - prog(t, s.to, s.to + XF);
const sc = lerp(0.94, 1, enter) * lerp(1.05, 1, leave);
if (!ff.stdin.write(Buffer.from(buf.buffer, buf.byteOffset, buf.byteLength))) await once(ff.stdin, 'drain');
```

- 场景模块只导出 `scenes`（`from/to/draw`）和 `events`（音效 + 可选 `shake`），`render.mjs` 不认识具体场景——即 `AGENTS.md` §3 “按场景拆模块”的落地形态；缺失的模块自动跳过，便于“写一组、验一组”。
- 版式分带（`render.mjs:23-26`）：置顶标题“同样是冬夜 / 有云反而没那么冷？”+ 章节条（y≈426–476），舞台中心 y=880，字幕 `SUB_Y = 1330`，1500 以下留给平台文案；`--safe` 叠加安全区（顶部 150 px、右侧按钮列、底部 420 px，`render.mjs:116-123`）。字幕字号 `min(66, (860 − 标签宽) / 每像素字宽)`。
- 背景星空、网格、漂云用 `sin(i·k)·43758.5453` 取小数的伪随机生成，逐帧可复现（`render.mjs:29-59`）。

### 5.6 示意图里的“物理语义”（`props.mjs:54-76`；`scenes-b.mjs:28-79`；`scenes-c.mjs:62-69`）

- `irWave()` 沿任意方向画行进正弦波 `amp·sin(2πs/λ − 10t)`，两端收窄，`p` 控制波头位置，`dash` 表示“溜走”。
- **颜色即语义**（`night.mjs:2, 8-9`）：橙 = 地面发出，青 = 大气自己发出，粉 = 地球射向太空。橙色波**终止在气体体内**（注释“it is absorbed there, not passed on”），逆辐射是**从气体出发的另一条青色波**，从画法上排除“反弹”误解；讲到“向下”时其余 6 个方向降到 0.3 透明度。
- 账本净散热：青条在“变少”一词处滑入净散热行，`(OUT − IN·eat)` 缩短右列红条；`OUT = 320`、`IN = 170` 只是像素，`numbers.note` 已声明不代表能量占比。

## 6. 声音与字幕

| 项 | 内容 | 来源 |
| --- | --- | --- |
| 旁白 | `zh-CN-XiaoxiaoNeural`，实际 +14% | `script.json`；`timeline.json` `rate` |
| 豆豆 | `zh-CN-YunxiaNeural`，实际 +18% / +6 Hz；嘴型由 `talkLevel()` 按逐词时间开合 | `script.json`；`lib.mjs:180-187` |
| TTS 缓存 | `build/tts_cache/` 76 个文件（38 组 mp3+json） | 目录清单 |
| 旁白处理 | `highpass 80 Hz` + `loudnorm I=-16 LUFS, TP=-1.5, LRA=11` | `tts.py:183-184` |
| 配乐 | 程序合成：124 BPM，C–G–Am–F（I–V–vi–IV）Karplus–Strong 拨弦琶音 + 根音贝斯 + 沙锤噪声 + 隔小节钟琴；种子 42；淡入 0.2 s、淡出 2.5 s | `audio.mjs:47-66, 163-199` |
| 音效 | 18 种程序音效（pop、note、ding、sparkle、whoosh、boing、slam、stamp、tick、scribble、buzz、alarm、tada、confetti、zap、wobble、static、wind），由各场景 `events` 按词锚点触发 | `audio.mjs:78-161`；`scenes-*.mjs` |
| 总混音 | 配乐 ×0.8 经旁白侧链压缩（threshold 0.05、ratio 5、attack 20 ms、release 400 ms），音效 ×0.55，三路 `amix` 后 `alimiter=limit=0.93` | `render.mjs:201-204` |
| SRT | 55 条，首条 `00:00:00,392 → 00:00:01,695 同样是冬天的夜里`，末条 `00:01:22,211 → 00:01:23,207 我要多穿一件！` | `out/*.srt` |
| 烧录字幕 | y=1330 胶囊，字号 ≤66 px，6 px 描边；每条出现时 1.05→1 “踢入” | `render.mjs:83-109` |

本次未试听、未测响度，只有 ffprobe 的编码信息。

## 7. 迭代与问题

| 事项 | 证据 | 说明 |
| --- | --- | --- |
| 由 [[atmosphere-heating]] 复制改写而来 | `lib.mjs:1` 注释仍写“Shared helpers for 地面怎样把大气暖起来”；`lib.mjs`、`props.mjs`、`audio.mjs`、`fonts.py` 与 atmosphere-heating 的 sha256 完全相同；`render.mjs` diff 只差标题、章节、星空背景、封面文字和封面时刻；`tts.py` 只差 `TITLE`、`TARGET` | 文件时间：atmosphere-heating 成片 10-22 20:26，本片 `script.json` 21:02、成片 21:14。“参考实现”在时间上是第二部 |
| 为结尾“穿羽绒服”扩展角色 | `chars.mjs:79-103` 新增 `coat` 参数（与 heating 版 diff） | 只服务 `finaleScene` |
| 用 `TARGET` 把语速从 +6% 提到 +14% | `tts.py:32`；`timeline.json` `rate: 14`；缓存 19×2 组 | 与现行 `AGENTS.md` §2“默认 `TARGET = None`，只有用户明确给时长上限才设，不要为压时长提速”不一致；当时是否有用户给的时长要求**未记录** |
| 没有左上角 logo、没有 `assets/`、没有片尾品牌卡 | 目录无 `assets/`；`render.mjs` 无 `brand()`/`outro()`；`TAIL = 2.2` | 不满足 `AGENTS.md` §4（`TAIL` 约 2.8 s、品牌卡 2.5–3 s）。本片早于该规则（推断），规则未要求回改 |
| 画面有“示意”声明 | `props.mjs:131-143` `disclaimer()` 绘制“示意图 · 箭头不代表能量多少”，在 `scenes-a.mjs:109`、`scenes-b.mjs:52, 78`、`scenes-c.mjs:81`、`scenes-d.mjs:79` 调用 | `AGENTS.md` §5 禁止新视频这样做，但写明“已经做好的视频不用回头修改” |
| 缺 `curriculum` 字段 | `script.json` 键列表 | 不满足 `AGENTS.md` §6 |
| 安装说明不全 | `AGENTS.md` §2 只写 `pip install edge-tts`；`fonts.py:9` 需要 `fontTools` | 若 `build/fonts/` 不存在，`lib.mjs:11-13` 会调用 `fonts.py`，没有 fonttools 时会失败（推断）；本机 `build/fonts/` 已存在（10-22 21:07 生成） |
| 模板残留 | `lib.mjs:215-229` `sineLine()` 引用不存在的 `C.eF`、`tts.py:124-127` 英文分支，本片均未用 | 推断来自更早模板，无功能影响 |
| 安全区检查 | `render.mjs:4, 116-123` 提供 `--safe` | 是否执行过、抽帧结果均**未记录**；`build/` 里没有 `still_*.png` |
| README 目录 | `README.md:64` 已收录，时长 1:26 | 与 ffprobe 85.53 s 一致 |

## 8. 复现入口

命令来自 `AGENTS.md`“常用命令”、`tts.py:4` 与 `render.mjs:2-5`。**本次均未执行。**

```bash
cd atmosphere-insulation
python3 -m venv .venv && .venv/bin/pip install edge-tts      # AGENTS.md §2；fonts.py 另需 fonttools（推断）
npm install                                                  # @napi-rs/canvas（lock 1.0.13）
.venv/bin/python tts.py            # → build/seg*.mp3、build/tts_cache/、build/narration.wav、build/timeline.json、out/大气怎样给地面保温.srt
node render.mjs --still 8.0 14.2   # → build/still_<t>.png（可加 --safe 叠加安全区）
node render.mjs                    # → build/music.wav、build/sfx.wav、out/大气怎样给地面保温.mp4
node render.mjs --cover            # → out/封面.png（取 hook 段结束后 0.1 s，整体下移 230 px）
```

- 顺序：必须先跑 `tts.py`，因为 `lib.mjs:24` 在加载时就读 `build/timeline.json`。
- 缓存含 +6% 与 +14% 两轮全部 19 段，**推断**可离线重建。修改入口：文案 `script.json`；节奏 `tts.py:32-35`；版式 `render.mjs:23-26`；场景 `scenes-*.mjs`。

## 9. 参考来源

| 来源 | 用途 | 状态 |
| --- | --- | --- |
| Geo Story Studio《大气怎样给地面保温？》v002 分镜（claim-043～046） | 改编底稿 | `script.json` 只写名称，无 `based_on` 路径；本次未查看 |
| NASA Earth Observatory — Climate and Earth’s Energy Budget（natural greenhouse effect） | 保温 / 温室效应原理 | 项目文件中只提及名称，无 URL；本次未访问 |
| NASA Langley CERES — Global Mean Energy Budget | 能量收支 | 同上 |
| NASA Science — The Earth’s Radiation Budget | 辐射收支 | 同上 |
| rany2/edge-tts#490、#491 | TTS 端点替换依据 | `tts.py:20` 注释提及；本次未访问 |

证据包 `urls` 为空：代码与脚本中没有任何 http(s) 链接。

## 10. 证据与限制

已读文件（`atmosphere-insulation/` 下，sha256 前 12 位）：`script.json` 41 行 `2c60927959c5`（全文）；`tts.py` 208 行 `977f0c63af67`（全文）；`render.mjs` 220 行 `c1e904698e0c`（全文 + 与 heating 版 diff）；`lib.mjs` 230 行 `c44f12ed8625`（全文）；`scenes-a.mjs` `cce1242446dd`（全文）；`scenes-b.mjs` `189652e7f5e5`、`scenes-c.mjs` `3ee9320fda5a`、`scenes-d.mjs` `9d52f932c5db`（主要场景函数与场景表）；`props.mjs` `3b6a75ea94a1`、`night.mjs` `02c4eb1e16ec`、`chars.mjs` `63d77c3fb5db`（函数目录与关键函数）；`audio.mjs` `ef790f152784`（音效目录与配乐）；`fonts.py` `a6ac7264e548`；`package.json` `d8ea4e240c0f` / `package-lock.json` `f365fc2b664b`；`build/timeline.json`（段起止、cues、逐词查询）；`out/*.srt`（首尾与条数）。成片 mp4 仅有证据包里的 ffprobe。

限制：

- 未发现制作会话；需求原文、`TARGET = 86` 的来由、文案确认、预览次数都**未记录**。
- 未打开成片、封面、音频；画面是否压到字幕区或安全区**无法确认**。
- Node / Python / edge-tts / ffmpeg 版本未记录，只有 `@napi-rs/canvas` 1.0.13 来自 lock。
- git 只有初始化提交 `4831a07`（2026-10-23），看不到本片修改历史；时间顺序来自文件修改时间。
