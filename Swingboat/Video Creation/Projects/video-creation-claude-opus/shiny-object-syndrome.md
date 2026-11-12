---
type: video-production-dossier
project: video-creation-claude-opus
video_id: shiny-object-syndrome
captured: '2026-11-12'
status: 已有 152.2 秒 1080p30 成片 MP4 + SRT；纯代码绘制卡通讲解片，三种 Edge TTS 声线分角色配音
source_fingerprint: 9e79c789fd75d7cc379eddc958eea196b6ddb819973d4fc4e30b47d85d7be88f
tags:
- video-creation
- production-capture
- canvas-frame-render
- tts
- explainer
---

# 闪亮物体综合征：挖井的阿亮 · Canvas 卡通角色动画 + 三声线 TTS 讲解

<!-- capture-source-sha256:9e79c789fd75d7cc379eddc958eea196b6ddb819973d4fc4e30b47d85d7be88f -->

本档案根据项目文件回溯整理，**未找回制作对话，结论来自项目文件**（源码、`script.json`、TTS 缓存、成片媒体头、文件时间戳）。本轮没有运行项目里的任何脚本，没有重新渲染，也没有播放成片做视听检查；只用 `ffprobe` 读取了成片媒体头。项目 git 只有两次整体提交，无法从 git 历史还原迭代过程。

## 1. 当前结论与制作路线

**状态：已有成片 `out/闪亮物体综合征-挖井的阿亮.mp4` 和字幕 `out/闪亮物体综合征-挖井的阿亮.srt`。** 各证据等级的情况：
- 「最终产物存在」：MP4 和 SRT 都已找到，MP4 已用 ffprobe 读取媒体头。
- 「技术检查通过」：仅限 ffprobe 读取的媒体头。帧数与按缓存重算的时间轴一致（见下表）。
- 「视觉检查通过」「用户验收」：**未记录**。

| 规格 | 值 | 来源 |
| --- | --- | --- |
| 分辨率 / 画幅 | 1920×1080，16:9 | ffprobe；`lib.mjs:7` |
| 帧率 / 帧数 | 30 fps，4567 帧 | ffprobe；`render.mjs:171` 中帧数 = `round(DURATION·FPS)` |
| 时长 | 152.233 s（约 2 分 32 秒） | ffprobe。按 `tts.py` 算法用缓存重算出 `DURATION = 152.239 s`，×30 取整为 4567 帧，与成片一致 |
| 视频编码 | H.264 High，yuv420p，约 0.95 Mbps | ffprobe；`render.mjs:168`（`libx264 -preset medium -crf 18`） |
| 音频 | AAC-LC，48 kHz 立体声，约 191 kbps | ffprobe；`render.mjs:169` |
| 文件大小 | 21,941,921 字节 | fingerprints.json 的 media 项 |
| 字幕 | 69 条，中文，角色台词前面带「阿亮：」「闪闪：」 | SRT；`tts.py:192-196` |

> 同样是 CRF 18，本片码率约 0.95 Mbps，远低于 `kpop-borrowed-sound` 的 9.1 Mbps。原因是本片为扁平色块卡通，没有颗粒、VHS 噪声这类高熵叠加层（**推断**，未做对比测试）。

**一句话管线**：

```text
script.json（22 段：旁白/阿亮/闪闪三种声线 + 段间停顿 + 手写字幕断句）
 → tts.py：Edge TTS 按声线逐段合成，记录词边界 → build/timeline.json + narration.wav + 带角色名的 SRT
 → scenes-a…e.mjs：故事 → 概念 → 循环与算账 → 何时换井/三个问题/停车票 → 小试验与出水结局
   （people.mjs 角色 IK 骨骼，world.mjs 土层剖面，icons.mjs 扁平图标）
 → render.mjs：@napi-rs/canvas 逐帧绘制，把 RGBA 原始帧通过管道送进 ffmpeg
 → audio.mjs：Karplus-Strong 拨弦配乐 + 卡通音效
 → ffmpeg：配乐按人声做侧链闪避，三轨混合后限幅，输出 H.264/AAC MP4
```

| 阶段 | 输入 → 处理 → 输出 | 入口 |
| --- | --- | --- |
| 需求 | 原始提示词未保存；`script.json.source` / `note` 记录了内容来源和教学模型说明 | `script.json:3-4` |
| 分镜 | 22 段台词，每段指定声线、停顿，部分手写断句 | `script.json` |
| 素材 | 无外部图片，角色、场景、图标全部用代码绘制 | `people.mjs`、`world.mjs`、`icons.mjs` |
| 动作 | 用 `at(段, 词)` 锚定动作时刻；嘴型由词边界驱动 | `lib.mjs:30-40`、`lib.mjs:207-215` |
| 声音 | TTS 三声线 + 程序合成配乐与音效 | `tts.py`、`audio.mjs` |
| 剪辑 | 场景之间交叉淡化 0.35 s，配合缩放；事件触发全屏震动 | `render.mjs:102-141` |
| 导出 | ffmpeg 一次完成编码和混音 | `render.mjs:156-180` |

## 2. 创作目标与需求

原始提示词**未保存**：项目里没有 `prompt.md`、`plan/` 或 README。以下内容摘录自 `script.json`。

- **内容来源**（摘录 `script.json:3`）：
  > "ELI5 · 闪亮物体综合征（单文件互动笔记）；参考 APA《Multitasking: Switching costs》、UCF Business Incubation Program、Richard Dancsi《Shiny Objects Ruin Startups》"

  其中「ELI5 单文件互动笔记」**不在本项目中**，本轮在项目内也没有找到这份文件；三份参考资料只有标题，没有 URL。
- **教学模型声明**（摘录 `script.json:4`）：
  > "挖井模型为人工教学规则（每口井 4 格出水、共 8 单位时间、换井多花 1 单位），不是研究测得的生产率数字。"
- **标题不一致**：`script.json.title` 是「闪亮物体综合征：为什么新想法很多，完成的事很少」，但 `tts.py:31` 和 `render.mjs:17` 都把输出名硬编码为「闪亮物体综合征-挖井的阿亮」，成片和 SRT 也用这个名字。`script.json.title` 没有被代码读取（`tts.py` 不读它，`timeline.json` 里也不写入 title）。
- **叙事结构**（据 `script.json` 归纳）：
  1. 寓言：阿亮挖井时三次被「闪闪」（会说话的宝石）引诱换地方，最后留下三个半截坑；
  2. 命名：闪亮物体综合征，并声明这**不是医学诊断**；
  3. 四个现实例子：新课、笔记软件、热门项目、相机；
  4. 四步循环；
  5. 算账：专注与频繁换井对比；
  6. 换井不一定错，要看证据，并提醒沉没成本；
  7. 三个自问；
  8. 想法停车票；
  9. 有时间预算的小试验；
  10. A 井出水，收尾。
- **风格**（据 `lib.mjs:58-63`、`render.mjs:22-56` 归纳）：明亮的儿童科普卡通风。蓝天渐变背景，太阳旋转、云朵漂移，主体字体是圆体（Yuanti SC Bold），角色和卡片使用粗描边、硬阴影。

## 3. 技术与依赖

| 项目 | 版本 / 参数 | 证据 |
| --- | --- | --- |
| Node 画布 | `@napi-rs/canvas` 声明 `^1.0.11`，锁定 `1.0.11` | `package.json`、`package-lock.json:16` |
| package.json | `"type": "commonjs"` 是 npm init 生成的默认值；源码都用 `.mjs`，按 ESM 运行，不受这一项影响 | `package.json` |
| Node.js / ffmpeg 版本 | 未记录 | — |
| Python | `edge_tts`、`aiohttp`、`fontTools`；**版本未记录**（没有 requirements 文件，`.venv/` 也已不存在） | `tts.py:15-17`、`fonts.py:9` |
| 旁白声线 `narr` | `zh-CN-XiaoxiaoNeural`，rate +0%，pitch +0Hz | `script.json` `voices` |
| 阿亮 `liang` | `zh-CN-YunxiNeural`，rate +4% | 同上 |
| 闪闪 `shiny` | `zh-CN-XiaoyiNeural`，rate +6%，pitch +12Hz | 同上 |
| Edge TTS 端点修补 | 同 kpop 项目：把 `WSS_URL` 改成 `api.msedgeservices.com`（rany2/edge-tts#490、#491） | `tts.py:19-25` |
| 中文字体 | 圆体 SC Bold（`ZH`/`EN` 主字体）、苹方 SC Semibold，从 macOS MobileAsset 字体包中抽出 | `fonts.py:13-16`；`build/fonts/` 现存 2 个 ttf |
| 其他字体 | Arial Rounded Bold（数字和 `+1` 等），回退 Arial Unicode、Hiragino Sans GB | `lib.mjs:17-21` |
| 平台 / 硬件 | 依赖 macOS 系统字体路径，推断在 macOS 上制作；硬件未记录 | `fonts.py:12` |

- 没有使用任何图像生成或视频生成模型，也没有外部图片素材；`assets/` 和 `images/` 目录都不存在。
- `script.json` 有一个顶层字段 `"voice": "zh-CN-XiaoxiaoNeural"`，代码没有读取它（`voice_of` 只读 `voices[...]`），属于残留或冗余字段。

## 4. 镜头与时间轴

`build/timeline.json` 已被清理。下表时间的来源：本轮按 `tts.py` 的排布规则，用 `build/tts_cache/` 中 22 份词边界 JSON 离线重算。缓存键是 sha1(`声线|语速|音调|文本`) 的前 16 位，按各声线自身参数计算、额外语速 0，全部命中。场景入点公式取自各 `scenes-*.mjs` 末尾的数组，`S(id)` 表示段落开始时间。

| 镜头 id | 约起点 s | 段落 | 内容 | 实现入口 | 关键参数 |
| --- | --- | --- | --- | --- | --- |
| story | 0.00 | intro → result | 地面剖面上有 A/B/C 三块牌子。阿亮扛铁锹走进画面（0.1–1.5 s），7.08 s 开始挖 A；11.9 s 闪闪出现并引诱；17.60 s 走到 B；20.58 s 挖到硬土「当」一声，伴随震屏；22.78 s 第二颗宝石出现；28.87 s 走到 C；32.53 s 盖章「一滴水也没有」 | `scenes-a.mjs:6-123` | `still: true`；挖掘速度 `SPEED 1.6` 次/秒；三口井的最终深度为 A 2 格、B 2 格、C 1.5 格 |
| name | 34.25 | name | 「闪亮物体综合征」逐字亮起，同时 7 个马林巴音阶上行；「挤到一边」时震屏 | `scenes-b.mjs:10-55` | 音阶 `[72,74,76,79,81,84,88]` |
| notdx | 43.03 | notdx | 「先说明」：两张卡「通俗说法 ✓ / 医学诊断 ✗」；一张写着「我有病？」的标签飞向阿亮，被打叉后掉落 | `scenes-b.mjs:56-94` | buzz + stamp |
| real | 50.79 | real | 四张现实卡片：书、App、火箭、相机，每张附两行反差文案（例如「启动了三次 / 验证了零次」） | `scenes-b.mjs:96-127` | 卡片在关键词前 0.3 s 弹出 |
| loop | 62.36 | loop | 环形四步：开始新东西 → 落地做事 → 另一个选择 → 重新开始，箭头逐段画出 | `scenes-c.mjs:7-60` | 圆心 (960,530)，R 300 |
| math | 74.70 | math / focus / hop | 两条 8 格时间轴：专注 `AAAA`，第 4 格出水，剩 4 单位；换井 `AASBBSCC`，换井显示 `+1`，结果是「三口都只到 2 格 · 0 口出水」 | `scenes-c.mjs:64-155` | FOCUS 每格 0.4 s；HOP 每格 0.68 s |
| butok | 95.30 | butok | 换井也可能是对的：「新证据：勘测 A 下面根本没有水」→「硬挖 = 固执」；「已经花掉的时间 ≠ 非坚持不可的理由」 | `scenes-d.mjs:8-56` | — |
| ask | 107.33 | ask | 三个问题卡：靶心「哪个目标」、扳手「什么卡点」、放大镜「什么证据」 | `scenes-d.mjs:58-83` | 每张卡带马林巴音 76/79/82 |
| ticket | 118.11 | ticket / shinyOk | 「新想法停车票」，手写三栏：是什么 / 为什么有用 / 什么时候再看（「A 井出水以后」）；盖章「晚点再看」后，闪闪沿弧线飘进右侧的「新想法停车场」 | `scenes-d.mjs:85-138` | 每栏一个 scribble 音效 |
| trial | 129.83 | trial | 小试验卡：沙漏（时间预算）→ 只验证一个问题 → 检查点旗帜 → 继续 / 改向 | `scenes-e.mjs:8-56` | — |
| finale | 140.10 | water / outro | 回到 A 井，从 3 格挖到 4 格，141.99 s 出水（水花、震屏、tada）；之后依次出现「好奇心可以留着」「主线别弄丢」「好想法，可以晚一点开始」，闪闪在停车场 `P` 牌下挥手；149.22 s 彩带 | `scenes-e.mjs:58-134` | 最后挖掘节拍 1.8 次/秒；`splash shake 10` |

### 全局层（`render.mjs:115-141`）

1. 背景：天空渐变，旋转的太阳（12 道光芒，`t·0.15` rad/s），5 朵云以不同速度循环漂移。
2. 场景：所有场景共用一个震屏位移 `shakeAt(t)`。事件带 `shake` 字段时，0.4 s 内按 `k=shake·(1−p)²` 衰减，x、y 方向分别用 `sin(t·95)`、`cos(t·81)` 抖动。
3. 场景转场：交叉淡化 `XF = 0.35 s`。入场时缩放从 0.96 放大到 1，离场时从 1 放大到 1.04；`still` 场景不缩放。
4. 左上角徽章「闪亮物体综合征」，带一颗旋转的星星，0.3–0.9 s 淡入。
5. 字幕。
6. 片头 0.4 s、片尾 0.6 s，用深蓝 `rgba(16,20,40)` 渐变进出。

## 5. 关键算法与实现

**视频讲解的知识**：闪亮物体综合征（行为的通俗说法）、任务切换成本、沉没成本、用证据决定是否转向、「停车票」和有时间预算的小试验。挖井算账是**人工设定的教学规则**，不是研究数据，`script.json:4` 和旁白都有说明。本档案**没有核实** APA 等参考资料的具体内容。

**制作视频所用的算法**，共以下 6 项。

### 5.1 TTS 词边界 → 全片时间轴（`tts.py:135-199`）

做法与 `kpop-borrowed-sound` 相同。差异如下：
- 每段可以指定 `voice`（narr/liang/shiny），各声线有自己的 rate/pitch，再叠加全局 `extra_rate`（`tts.py:39-41`、`synth`）。
- `LEAD_IN 1.2 s`，`TAIL 3.0 s`，`TARGET 160 s`（注释说明它只是上限，片长以旁白实际需要为准）。本片总长 152.24 s，没有触发自动提速。
- 每条字幕带上说话人 `who`。写 SRT 时，给阿亮、闪闪的台词加「阿亮：」「闪闪：」前缀（`tts.py:131`、`tts.py:192-196`）。
- `split_caption` 去掉尾部标点时不去掉「、」（kpop 版会去掉），是一处细微差异。

### 5.2 用词锚定动作（`lib.mjs:30-40`）

`at(段id, 词, 第几次出现)` 从 TTS 词边界查出时刻，用法与 kpop 版相同。`scenes-a.mjs:10-18` 是一个典型例子：整段寓言的走位、挖掘和情绪变化，全部由十来个词锚点推导出来。

```js
const T_DIG_A = at('meet', '开始挖');
const T_STOP_A = S('tempt1') + 0.1;
const T_WALK1 = E('swap1') + 0.05, T_ARR_B = T_WALK1 + 0.9;
const T_HARD = at('again', '硬');           // 挖到硬土：clang + 震屏
const depthA = (t) => 2 * easeIO(prog(t, T_DIG_A + 0.3, T_STOP_A - 0.2));
```

井深随时间增长，单位是格，`CELL = 60 px`。水层固定在第 4 格 `WATER = GY + 4·CELL`（`world.mjs:4-6`），画面和教学规则使用同一套坐标。

### 5.3 嘴型与台词同步（`lib.mjs:207-215`）

```js
export function talkLevel(t, ids) {
  for (const id of ids) {
    const s = SEG[id];
    if (t < s.start || t > s.end) continue;
    if (s.words.some((w) => t >= w.start && t <= w.end)) return 0.45 + 0.55 * Math.abs(Math.sin(t * 22));
  }
  return 0;
}
```

只在词边界区间内张嘴，开合度用 22 rad/s 的正弦摆动。词与词之间的停顿自然闭嘴。这不是音素级口型，只是「在说话 / 没说话」的节奏同步。气泡 `sayBubble` 在段落开始前 0.15 s 弹出（easeBack 回弹），段落结束后保持 0.4 s 再淡出。

### 5.4 角色骨骼（`people.mjs`）

- **两骨 IK**（`people.mjs:6-13`）：已知肩膀和手的位置，用余弦定理求肘部，`k = acos((l1² + d² − l2²)/(2·l1·d))`，`bend` 参数决定弯折方向。所有姿势都只需要给出手的位置。
- **姿势表 `handsFor`**（`people.mjs:43-71`）：dig、carry、cheer、point、write、think、shrug、stand。挖掘是一个周期 `u = (t·digSpeed) % 1`：前 45% 铲头下压（握把高度从 −250 降到 −170，铲柄角度 1.0→1.25 rad），后 55% 甩土（角度回到 −0.15）。铲长 270，右手握在铲柄 1/3 处。
- 走路：双腿按 `sin(walk·2π)·26` 交替摆动，身体上下颠动 10 px。站立时有 3 px 的呼吸起伏。
- 表情 `face`：mood 有 happy/excited/shock/tired，`look` 控制视线方向；闪闪另有 wink 和招手（`wave`）。
- `people.mjs:1` 注释说明角色设定：阿亮是挖井的年轻人，闪闪是不断引诱他离开的宝石。两者都是卡通造型，没有使用任何真实人物形象。

### 5.5 剖面世界（`world.mjs`）

地面 `GY = 600`，土层用渐变填充，70 颗卵石的位置由确定性随机数生成（`rnd(i)`）。水层表面按 `sin(x·0.02 + t·2)·5` 起伏，`waterGlow` 用虚线框高亮。井身是圆底矩形，深度可以取小数；挖掘时土块从井口飞向土堆（`clods`），出水时水柱喷出（`splash`），宝石消失时冒烟（`poof`）。

### 5.6 程序合成配乐与音效（`audio.mjs`）

- **配乐**（`audio.mjs:167-203`）：104 BPM，和弦按 C–G–Am–F（I–V–vi–IV）循环。每小节 8 个八分音符拨弦，音型 `[0,2,1,3,2,1,3,2]`，偶数拍重音 1、奇数拍 0.7。拨弦用 **Karplus-Strong**（`audio.mjs:48-65`）：先用一阶低通噪声填满长度为 `SR/f` 的环形缓冲，再反复做相邻两点平均并乘衰减系数 `damp 0.9965`。贝斯在第 1 拍和第 3.5 拍各一次，基频加八度。shaker 是十六分音符的 7–9 kHz 噪声。每隔一小节有一段钟琴应答（3 个音）。整首淡入 1.2 s、淡出 2.5 s。
- **音效**（`audio.mjs:78-158`）：pop、note（马林巴）、ding、sparkle、whoosh、boing、slam、stamp、tick、scribble、buzz、alarm、tada、confetti、dig、clang、splash、poof、wind，共 19 种。实际用到的类型以各场景 `events` 为准。例如：dig 是 120→70 Hz 下滑的正弦加 900→300 Hz 带通噪声；clang 是 1240/1873/2710 Hz 三个非谐波分音。
- 随机数种子固定：配乐 42，音效 7（mulberry32 风格的 `rng`）。

## 6. 声音与字幕

### 配音

- 引擎：Edge 在线神经语音，经 `edge_tts` 调用，版本未记录。三个角色用三种声线，参数见第 3 节。全片 22 段：旁白 15 段、阿亮 4 段（intro、swap1、swap2、water）、闪闪 3 段（tempt1、tempt2、shinyOk）。
- 角色感主要来自两点：声线本身不同，闪闪另外提高 rate 和 pitch（+6%、+12Hz）。项目里没有记录 SSML 情绪风格（style）参数，代码中也没有使用。
- 缓存：现有 44 个文件，即 22 段 × (mp3 + json)，与当前 `script.json` 一一对应，修改时间集中在 10-14 23:02–23:03。429 退避和 403 端点修补的做法与 kpop 项目相同。

### 字幕

- 断句：只有 `name` 一段手写了 `captions`，其余段落都由 `split_caption` 自动断句（遇到标点切开，每块不超过 14 字）。
- 时间：每块字幕的起止时间取所覆盖词边界的首尾，**不靠估算**。
- SRT 共 69 条。第 1 条是 `00:00:01,248 → 00:00:01,801「阿亮：今年」`，最后一条 `00:02:26,944 → 00:02:29,119「好想法，可以晚一点开始」`。
- 画面字幕（`render.mjs:70-96`）：圆体 52 px，白字；底部居中放一个深色胶囊底（`rgba(24,26,48,0.8)`，高 86，下边缘 y = 1046）。角色台词左侧加名牌：阿亮橙色、闪闪天蓝。显示规则：一条持续到下一条开始；段首提前 0.08 s 淡入，段尾在 0.12–0.3 s 内淡出。
- 双重提示：故事段的台词还会以对话气泡出现在角色旁边（`sayBubble`），同时底部字幕照常显示。

### 配乐、音效与混音

- 配乐和音效全部由程序合成，算法见 5.6。音效由各场景的 `events` 按词锚点触发。数字字符串会被转成音效，例如 `digTicks` 把 `AASBBSCC` 中的挖掘格转成 `dig`、换井格 `S` 转成 `whoosh`（`scenes-c.mjs:162-164`），所以画面和声音来自同一份数据。
- 混音链（`render.mjs:165-166`）：

```text
[旁白] asplit → 一路作侧链键控信号，一路进最终混音
[配乐] ×0.7 → sidechaincompress(threshold 0.05, ratio 5, attack 20 ms, release 400 ms)
[音效] ×0.5
amix(normalize=0, duration=first) → alimiter(limit 0.93) → AAC 192k
```

- 旁白在 `tts.py` 中先做 `highpass 80 Hz` 和 `loudnorm I=-16:TP=-1.5:LRA=11`。**成片整体响度没有记录**，本轮也没有测量，也没有试听。

## 7. 迭代、问题与修正

没有 plan、日志或对话。以下内容来自文件修改时间（10-14）和代码注释；时间戳只代表最后一次写入。

| 时间（文件系统） | 文件 | 解读 |
| --- | --- | --- |
| 23:01 | `package.json`、`package-lock.json` | 初始化 npm，安装 canvas。`package.json` 保留了 npm init 的默认字段（`"test"` 脚本、`commonjs`） |
| 23:02 | `fonts.py`、`tts.py` | — |
| 23:02–23:03 | `build/tts_cache/*`、`build/fonts/*` | 22 段一次合成完成；字体也已抽取 |
| 23:06–23:17 | `people.mjs` → `audio.mjs` → `lib.mjs`/`scenes-a` → `world.mjs` → `icons.mjs`/`scenes-b` → `scenes-c` → `render.mjs` → `scenes-d` | 各模块最后一次写入的顺序 |
| 23:18 | `scenes-e.mjs`、`script.json`、SRT | SRT 由 `tts.py` 写出，但缓存时间仍是 23:02–23:03。**推断**：23:18 重跑了 `tts.py`，全部命中缓存；`script.json` 的改动没有影响缓存键，可能只改了 `gap`、`captions` 这类字段 |
| 23:19 | `out/…mp4` | 成片写出，晚于所有源码 |

- 整个项目的文件时间集中在约 18 分钟内，说明制作节奏很快（**推断**；更早的版本可能已被覆盖，无法确认）。
- 从代码可以看出的取舍：
  - 嘴型只做「说话 / 不说话」加正弦开合，不做音素级口型（`lib.mjs:207-215`）。
  - 字幕和对话气泡同时显示。
  - `TARGET` 只作上限，片长跟随旁白。
  - `script.json.title` 和硬编码的 `TITLE` 不一致，这个问题一直没有修正（见第 2 节）。
- 内容层面的谨慎处理：
  - 旁白专门声明「不是医学诊断」；
  - `note` 字段声明挖井数字是教学规则；
  - `butok` 段反过来说明「换井不一定是错的」，避免把内容讲成「永远不要换」。
  - 这些处理由谁要求、出于什么原因，**未记录**。

## 8. 可复用制作经验

1. **让教学模型的参数和画面坐标共用同一套常量**：`CELL = 60`、`WATER = GY + 4·CELL` 既是画面尺寸，也是「第 4 格出水」这条规则，画面不会和讲解对不上。依据：`world.mjs:4-6`、`scenes-c.mjs:64-68`。
2. **用一串字符同时描述动画和音效**：`'AAAA'`、`'AASBBSCC'` 同时生成时间轴格子、井深和 `dig`/`whoosh` 音效，改一处，三处一起变。依据：`scenes-c.mjs:64-66`、`scenes-c.mjs:162-164`。
3. **多角色配音只需在 JSON 里给每段加 `voice`**，并给角色单独设 rate/pitch；SRT 和画面字幕自动加上说话人名牌。依据：`script.json` 的 `voices`、`tts.py:131`、`tts.py:194`、`render.mjs:70`。
4. **用词边界驱动廉价口型**：落在词区间内就张嘴，开合度用正弦摆动，几行代码就能和配音节奏对齐。依据：`lib.mjs:207-215`。
5. **用两骨 IK 加「手部目标表」做角色动画**：新增一个姿势只要写两个手的坐标，肘部自动求解；挖掘这类周期动作用相位 `u` 控制。依据：`people.mjs:6-71`。
6. **震屏写成事件属性**：在 `events` 里加一个 `shake: N` 字段，就能同时触发音效和全局震屏，按二次方衰减，强度可以叠加。依据：`render.mjs:102-113`。
7. **科普内容在脚本层写明边界**：「不是医学诊断」「教学规则不是研究数据」「换井也可能是对的」，避免以偏概全。依据：`script.json:4`，以及 `notdx`、`butok` 段。

## 9. 复现入口

以下命令取自源码注释，**本轮没有执行**。

```bash
cd shiny-object-syndrome
npm install                                   # 按 package-lock 安装 @napi-rs/canvas 1.0.11
python3 -m venv .venv && .venv/bin/pip install edge-tts aiohttp fonttools   # 版本未记录
.venv/bin/python tts.py                       # 生成 build/timeline.json、build/narration.wav、out/闪亮物体综合征-挖井的阿亮.srt
node render.mjs --still 3.2 7.6               # 先出单帧预览，写到 build/still_<t>.png
node render.mjs                               # 生成 build/music.wav、build/sfx.wav，再输出 out/闪亮物体综合征-挖井的阿亮.mp4
```

- 必须先运行 `tts.py`。`lib.mjs:24` 启动时会读取 `build/timeline.json`，该文件已被清理。
- `build/tts_cache/` 覆盖全部 22 段。只要不改文本、声线、语速、音调，可以离线重建时间轴，不需要访问 Edge 服务。
- 字体：`build/fonts/` 里有 YuantiSC-Bold、PingFangSC-Semibold 两个文件。如需重新抽取，`fonts.py` 只搜索 macOS MobileAsset 字体目录；圆体、苹方需要事先由系统下载（**推断**，依据是 `fonts.py:12` 的 `ASSETS` 路径）。
- 渲染耗时**未记录**。SRT（23:18）到 MP4（23:19）的时间戳间隔不能当作渲染耗时。

修改参数的位置：

| 想改什么 | 改哪里 |
| --- | --- |
| 台词、角色、停顿、断句 | `script.json` 的 `segments` |
| 角色声线、语速、音调 | `script.json` 的 `voices` |
| 片长上限、片头片尾留白 | `tts.py:32-35`（`TARGET`/`LEAD_IN`/`TAIL`） |
| 输出文件名 | `tts.py:31` 和 `render.mjs:17`，两处都要改 |
| 配色 | `lib.mjs:58-63` 的 `C` |
| 挖井规则（格数、水层） | `world.mjs:4-6`、`scenes-c.mjs:64-68` |
| 寓言段走位和挖掘节奏 | `scenes-a.mjs:6-18`（`SPEED`、各 `T_*` 锚点） |
| 转场长度、震屏 | `render.mjs:20` 的 `XF`、`render.mjs:102-113` |
| 配乐（BPM、和弦、拨弦衰减） | `audio.mjs:171-196` |
| 混音比例 | `render.mjs:165-166` |

## 10. 参考来源

项目中**没有任何 URL**。`script.json:3` 只列出资料名称。按规则，这里不补充或推测链接，也没有联网核实这些资料的内容。

| 名称（项目原文） | 类型 | 借鉴内容 | 访问状态 |
| --- | --- | --- | --- |
| ELI5 · 闪亮物体综合征（单文件互动笔记） | 内容底稿（推断为同一作者先前的讲解笔记） | 推断整体叙事和挖井比喻来自这份笔记；具体对应关系**未确认** | 文件不在本项目中，未找到 |
| APA《Multitasking: Switching costs》 | 技术或科学资料 | 推断对应「换井多花一格」的任务切换成本；**关系未确认** | 无链接，未访问 |
| UCF Business Incubation Program | 资料来源 | 具体借鉴了什么**未记录** | 无链接，未访问 |
| Richard Dancsi《Shiny Objects Ruin Startups》 | 资料来源 | 推断对应「更热门的项目：启动了三次，验证了零次」这类创业场景；**关系未确认** | 无链接，未访问 |

技术资料：`tts.py:20` 注释引用了 rany2/edge-tts issue #490、#491，没有完整 URL，本轮未访问。

## 11. 证据与限制

**本轮实际读取的关键文件**（sha256 前 16 位取自 fingerprints.json）：

| 文件 | sha256 前 16 位 | 读取范围 |
| --- | --- | --- |
| `script.json` | `ee2f30a66c22c20d` | 全文 |
| `tts.py` | `4db1ae4d9b275fff` | 全文 |
| `fonts.py` | `a6ac7264e548bd3d` | 1–25 |
| `render.mjs` | `e0bc2be5b9aa8a4c` | 全文 |
| `lib.mjs` | `898713838519f7f5` | 全文 |
| `audio.mjs` | `ead49fc21f848b07` | 1–66、函数索引、135–204 |
| `people.mjs` | `131ffeb05ca8bbee` | 1–72、129–160、函数索引 |
| `world.mjs` | `7df1390660532a0e` | 1–60、函数索引 |
| `icons.mjs` | `b8f355538eed30e3` | 只看了函数索引 |
| `scenes-a.mjs` | `f7a0b9ea9d5367ba` | 1–40、123–139 |
| `scenes-b.mjs` | `0173990def5eac5b` | 10–12、56–100、129–154 |
| `scenes-c.mjs` | `dbb387a40fcb056b` | 1–20、64–68、关键行、157–184 |
| `scenes-d.mjs` | `a2aa43408c747112` | 常量、文字行、140–163 |
| `scenes-e.mjs` | `14c41f7197ed7377` | 1–30、58–134（部分） |
| `package.json` / `package-lock.json` | `62de20bfdb668a96` / `03adc613b15a6bb9` | 版本字段 |
| `build/tts_cache/*.json`（22 份） | 见 fingerprints.json | 用于离线重算时间轴 |
| `out/闪亮物体综合征-挖井的阿亮.srt` | `fd67b64db3f37e21` | 首尾条目、条数 |
| `out/闪亮物体综合征-挖井的阿亮.mp4` | 无（21,941,921 字节） | 只读了 ffprobe 媒体头 |

**未读取或无法确认**：
- `icons.mjs` 的图标细节、`people.mjs` 的表情与闪闪造型、`scenes-a` 中段的角色调度、`scenes-c/d/e` 部分绘制代码：只看了结构，没有逐行核对。
- 成片没有播放，也没有抽帧，画面效果、字幕是否遮挡、声画是否同步，都没有经过视觉或听觉检查。
- 缺失：原始需求或提示词、制作对话、plan 文档、ELI5 笔记原件、参考资料链接、Python/Node/ffmpeg 版本、渲染耗时、成片响度、视觉与听感检查记录、用户验收。
- `build/` 下的 `timeline.json`、`narration.wav`、`music.wav`、`sfx.wav`、`seg*.mp3`、`still_*.png` 都已被清理；这些文件现在不存在，不代表当初没有生成。
