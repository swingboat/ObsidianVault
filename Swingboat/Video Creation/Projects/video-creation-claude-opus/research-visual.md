---
type: video-production-dossier
project: video-creation-claude-opus
video_id: research-visual
captured: '2026-11-12'
status: 已有 30 秒带旁白与字幕的完整 MP4（无配乐）；用户验收未记录
source_fingerprint: 30bd5f8acfa6300d72cf2fe4913661747fb489cee87f0a302cba759d4fa5511b
tags:
- video-creation
- production-capture
- canvas-frame-render
- isometric
- tts
---

# 科研视觉怎么做 · 2.5D 等轴积木小世界的 Canvas 逐帧科普动画

<!-- capture-source-sha256:30bd5f8acfa6300d72cf2fe4913661747fb489cee87f0a302cba759d4fa5511b -->

本次为源项目的材料回溯与归档。**未找回制作对话，结论来自项目文件**；本轮没有重新运行 TTS 或渲染，也没有观看成片或做用户验收。项目 git 只有两次整体提交，无法还原迭代；`build/` 中的 `timeline.json`、`narration.wav`、`seg*.mp3` 与预览帧已被清理，仅剩 `build/tts_cache`。

## 1. 当前结论与制作路线

**状态：已有完整成片** `out/科研视觉怎么做.mp4`（技术检查：ffprobe 媒体头可读，时长恰为 30.000 s；视觉检查、用户验收：未记录）。

| 规格 | 值 | 来源 |
| --- | --- | --- |
| 分辨率 / 画幅 | 1920×1080，16:9 横屏 | ffprobe；`render.mjs` L10 |
| 帧率 / 帧数 | 30 fps，900 帧 | ffprobe；`render.mjs` L10、L1052 |
| 时长 | 30.000 s | ffprobe；`tts.py` L30 `TARGET = 30.0` |
| 视频编码 | H.264 High，yuv420p，CRF 18，preset medium | ffprobe；`render.mjs` L1049 |
| 音频 | AAC LC，48 kHz，双声道，192 kbps；只有旁白 | ffprobe；`render.mjs` L1044–1050 |
| 字幕 | 外挂 `out/科研视觉怎么做.srt`（14 条）+ 画面内烧录字幕 | 文件；`render.mjs` L932–953 |
| 文件大小 | 4,488,670 字节，总码率约 1.20 Mbps | ffprobe |

一句话管线：

> 需求文稿 `科普和教育的新革命.md`（30 秒、问题开头 + 四要点 + 收尾、每点一个数字和论文出处、2.5D 等轴积木风）→ 改写为 6 段旁白 `script.json`（每段带 `number` 与 `cite`）→ `tts.py` 用 Edge 神经 TTS 合成并取词边界，自动调语速让全片落在 30 s，余量平摊进停顿 → `timeline.json` + `narration.wav` + SRT → `render.mjs` 单文件实现等轴投影、Lambert 光照、投影阴影与扁平科学家小人，按词边界卡点逐帧绘制 → ffmpeg 编码 → MP4。

没有分镜文件、图片素材或 AI 视频模型，画面全部由 Canvas 2D 代码绘制；按需求不配音乐、不配音效。

## 2. 创作目标与需求

### 2.1 源文稿（需求原文，摘录自 `科普和教育的新革命.md`，全文 5 行）

> 做一个 30 秒的科普动画，主题是「科研视觉怎么做：物体、连线、空间、光」。
> 结构：开头先抛出一个问题，再拆成四个要点。每个要点都落到一个具体数字上，并标出出自哪篇论文，最后用一句话收住。
> 风格：2.5D 等轴科技插画，天蓝色背景，用蓝紫色积木搭一个小世界，让穿白大褂的扁平小人在里面演示。全部用代码画，道理要靠积木和小人的动作演出来，画面上别堆文字。
> 声音：清亮的女声旁白，配字幕，不要配乐。
> 其他可以自由发挥。

说明：

- 文件名「科普和教育的新革命」与正文主题不同；文件名的来历（例如是否摘自某篇文章或系列）**未记录**，不作推断。该文件实际承担项目规则中 `prompt.md` 的角色；目录内没有 `prompt.md` 和 `plan/`。
- 文稿只规定了主题、结构、风格和声音；**四个数字与论文出处不在文稿中**，是在写 `script.json` 时补充的（由谁、依据什么检索选定：未记录）。

### 2.2 文稿 → `script.json`：逐条落实

| 文稿要求 | `script.json` 的落实 | 代码中的落实 |
| --- | --- | --- |
| 30 秒 | 6 段短句，总字数约 100 | `tts.py` `TARGET = 30.0`，超时自动加速，不足则把余量平摊到段间停顿，使片长恰为 30 s |
| 开头先抛问题 | `hook`：「为什么有的科研图一眼就懂，有的却越看越乱？」 | hook 场景：13 个积木乱落 + 乱连线 + 小人挠头冒问号 |
| 拆成四个要点，「物体、连线、空间、光」 | `object` / `line` / `space` / `light` 四段，每段以「第一，物体。」这类序号开头 | 右上 4 个进度点（`drawProgress`）标示当前要点 |
| 每点落到具体数字 | 字段 `number`：`4`、`90°`、`45°`、`26°`，旁白里同时念出「四个 / 九十度 / 四十五度 / 二十六度」 | 动画的「到位」时刻精确对齐这些数字词的发音（`keyTime`） |
| 标出论文出处 | 字段 `cite`：Luck & Vogel, Nature, 1997；Huang, Hong & Eades, PacificVis, 2008；Cleveland, McGill & McGill, JASA, 1988；Mamassian & Goutcher, Cognition, 2001 | 左上数字徽章：大号数字 + 小字引用（`drawBadge`） |
| 一句话收住 | `end`：「好图，就是让眼睛少费力。」 | 结尾四个数字胶囊回顾（`drawRecap`）+ 四方块与直角交叉线 |
| 女声、清亮 | `"voice": "zh-CN-XiaoxiaoNeural"` | 全片单声线 |
| 配字幕、不要配乐 | — | 字幕由词边界对时；ffmpeg 只映射旁白一条音轨 |
| 画面上别堆文字 | 文本只放在旁白和字幕中 | 画面文字限于：数字徽章、进度点、结尾数字回顾、字幕、问号气泡 |

### 2.3 旁白脚本（`script.json` 全部 6 段）

| id | 文本 | number | cite |
| --- | --- | --- | --- |
| hook | 为什么有的科研图一眼就懂，有的却越看越乱？ | — | — |
| object | 第一，物体。人一眼大约只记得住四个，主角别超过四个。 | 4 | Luck & Vogel, Nature, 1997 |
| line | 第二，连线。线非交叉不可，就让夹角接近九十度。 | 90° | Huang, Hong & Eades, PacificVis, 2008 |
| space | 第三，空间。把曲线的平均坡度调到四十五度，趋势最好读。 | 45° | Cleveland, McGill & McGill, JASA, 1988 |
| light | 第四，光。大脑默认光从左上方约二十六度照下来。 | 26° | Mamassian & Goutcher, Cognition, 2001 |
| end | 好图，就是让眼睛少费力。 | — | — |

## 3. 技术与依赖

| 类别 | 内容 | 证据等级 / 来源 |
| --- | --- | --- |
| 运行时 | Node.js（ESM，`render.mjs` 使用顶层 `await`），版本未记录 | 代码中存在 |
| 画布 | `@napi-rs/canvas` 声明 `^1.0.11`，锁定 `1.0.11` | `package.json`、`package-lock.json` |
| Python | 项目 `.venv`（已清理），版本未记录 | `tts.py` 文档字符串 |
| TTS 库 | `edge_tts` + `aiohttp`，**版本未记录** | `tts.py` L14–16 |
| TTS 端点补丁 | 把 `edge_tts.communicate.WSS_URL` 改为 `api.msedgeservices.com` 合成端点 | `tts.py` L18–24 |
| 编码 | 系统 `ffmpeg`（libx264、AAC、`loudnorm`），版本未记录 | `tts.py` L162–176、`render.mjs` L1044–1051 |
| 字体 | 仅注册 `/System/Library/Fonts/Hiragino Sans GB.ttc`（冬青黑体简体），字体栈 `"Hiragino Sans GB", "Heiti TC", sans-serif` | `render.mjs` L11–12 |
| 硬件 | 未记录（系统字体路径表明为 macOS，推断） | — |

配音：`zh-CN-XiaoxiaoNeural`（Edge 神经语音，中文女声），最终全局语速 **+6%**（证据见 §7）；无 pitch 参数。

## 4. 镜头与时间轴

单镜头、单场景：一座 8×8 格浮空等轴平台始终在画面中央，6 个「幕」通过换道具与小人走位衔接（`SC` 表，`render.mjs` L372–379，每幕从对应旁白起点前 0.35 s 开始）。时间由本次用 `script.json` + rate +6% 的 TTS 缓存按 `tts.py` 算法**离线重算**，与 SRT 一致；「到位时刻」是 `keyTime()` 取到的数字词发音起点。

| 幕 | 时间 (s) | 旁白 | 到位时刻 | 积木与小人的演示 | 实现入口 | 关键参数 |
| --- | --- | --- | --- | --- | --- | --- |
| hook | 0 – 3.95 | hook | — | 13 个蓝紫方块依次从 5 单位高处弹跳落下（间隔 0.07 s），1.0 s 起 10 条金/白曲线乱连；小人从左下走入后挠头、表情变困惑，头顶冒「?」；镜头微推近 4% | `propsAt` L544–598、`sciBase` L432–441、L1008–1013 | `bounce` 缓动；`CLUTTER`、`TANGLE` 常量 L383–398 |
| object | 3.95 – 9.98 | object | 「四个」−0.15 s = 7.58 | 乱线消失；小人挥臂横扫，9 个多余方块按序被弹飞出岛；前 4 个方块归位到 2×2，逐个金边发光（间隔 0.25 s），小人逐个指点后变开心；地面出现 4 个脉动金圈；左上徽章「4 / Luck & Vogel…」 | L544–616、`sciBase` L442–460 | `KEEP_POS` ±0.85；弹飞方向 `norm3([x+0.6, y−1.5, 0])` |
| line | 9.98 – 15.41 | line | 「九十度」= 14.68 | 4 个方块缩成节点，连成金、青两条「光束」并有信号点流动；小人走到节点旁推动，交叉角从 16° 平滑转到 90°，地面量角弧在 ≥86° 时变成直角方块；到位后小人蹦跳 | L618–685、`nodePositions` L403、`lineAngle` L402 | `R_LINE = 2.5`；转动区间 [T−2.0, T−0.1] |
| space | 15.41 – 21.53 | space | 「四十五度」= 19.40 | 5 根柱子沿屏幕水平方向排开组成折线图；小人「拉」最后一根柱子把间距拉宽，平均坡度从约 68.8° 降到 45.0°；虚线量角器在误差 <1.5° 时变金 | L687–737、`columns` L417 | `DATA=[0,0.75,1.4,2.15,3.0]`；步长 0.24→0.62 |
| light | 21.53 – 26.90 | light | 「二十六度」= 25.54 | 3 个方块 + 一块带 4 个圆形凹凸的板；太阳沿虚线轨道从 −150° 转到左上 26°，小人像指挥一样指向太阳；全场光照、投影随之变化，圆盘高光方向决定看起来是凸还是凹；到位时画出竖直参考线与 26° 扇形 | L739–818、`sunTheta` L424、`sceneLight` L426 | 太阳轨道半径 390 px；转动区间 [T−2.4, T−0.1] |
| end | 26.90 – 30.00 | end | — | 4 个方块在四角弹出，两条对角光束以直角相交；小人走到右侧挥手蹦跳；顶部依次弹出 4、90°、45°、26° 回顾胶囊；镜头拉远 4%，末 0.3 s 白场淡出 | L820–850、`drawRecap` L911 | 胶囊间隔 0.15 s |

HUD（`render.mjs` L854–953）：左上数字徽章在到位时刻前 0.1 s 回弹出现、幕末淡出；右上 4 个进度点（当前项金色加长、已完成白色）；底部字幕（深靛蓝半透明圆角底，白色 50 px 粗体）。

## 5. 关键算法与实现

> 区分：视频**讲解**的是四条视觉认知/可视化结论（视觉工作记忆约 4 个对象、交叉角接近 90° 易读、折线平均坡度 45° 的 banking、光源先验偏左上约 26°）。这些是内容知识，引用来自 `script.json` 的 `cite`，本次未核验原论文。下文是**制作视频所用**的算法；其中 45°、90°、26° 这几个角度在画面中是被真实计算出来的，而不是贴图。

### 5.1 等轴投影与「相机」

```js
const C30 = Math.cos(Math.PI / 6), S30 = 0.5;
const cam = { cx: W / 2, cy: 455, u: 68 };          // u = 每个世界单位的像素
const P = (x, y, z = 0) => [cam.cx + (x - y) * C30 * cam.u,
                            cam.cy + (x + y) * S30 * cam.u - z * cam.u];
```

世界坐标 x 向右下、y 向左下、z 向上（单位：格）。每帧修改 `cam.u` 实现推拉（hook 推近 4%、end 拉远 4%），`cam.cy = 470 + 4·sin(0.6t)` 做轻微漂浮（L961–966）。注意 L70 的初值 `cy: 455` 每帧都被覆盖为 470 附近。

### 5.2 Lambert 光照与可动光源

```js
function lightVec(theta) {      // theta：屏幕上相对竖直向上的角度，正值偏左
  const up = [0,0,1], left = [-√½, √½, 0], view = norm3([1,1,1]);
  return norm3(c·up + s·left + 0.55·view);   // 0.55 把光往观众方向拉，避免正面全黑
}
const lambert = (n, L) => 0.5 + 0.58 * Math.max(0, dot(n, L));
```

方块只画三个可见面（顶、左、右，法线 `[0,0,1]`、`[0,1,0]`、`[1,0,0]`），每个面颜色 = 基色 × lambert 系数（>1 时向白色插值，`shadeCol`），顶面另加 0.06 提亮，顶边描 35% 白色高光。默认光角 `DEFAULT_THETA = 26°`，即全片默认就用「左上 26°」打光，第四幕只是把它转出来给观众看。

### 5.3 投影阴影

`boxShadow`（L143–157）：把盒子底面 4 角和沿光线方向压到地面的 4 个点（偏移 `−L.xy · (top / L.z)`）取凸包（单调链算法 `convexHull`），填到独立的 `shadowLayer`。所有阴影先画在同一层、裁切到平台顶面，再以 `0.2 × clamp((L.z − 0.12)/0.3)` 的整体透明度合成一次——注释写明 *so overlaps don't double-darken*。小人的阴影是一条从脚底沿光线方向延伸的粗圆头线段。光线接近水平（`L.z < 0.12`）时阴影关闭。

### 5.4 画家算法排序

方块按中心 `x + y`（再加 `0.01·z`）由远到近排序，小人以 `脚位 x + y + 0.35` 插入同一序列（L997–1006），这样小人可以走到方块前后而遮挡正确。带凹凸圆盘的扁平板在被画出后立即叠画 `slabTop` 图层，保证圆盘不被后面的方块盖住。

### 5.5 三个被「算」出来的角度

- **90° 交叉**：4 个节点为 `(±R, 0)` 和 `±R(cos φ, sin φ)`，φ 由 16° 经 `easeIO` 到 90°；量角弧在地面平面上逐点投影绘制，`φ ≥ 86°` 时淡入直角方块（L402–409、L655–681）。
- **45° 平均坡度**：柱子沿世界 `(a, −a)` 排列，这条线投影到屏幕正好水平，相邻柱水平间距 `2·step·C30·u`；令 4 段总水平距离等于总高差，得
  `COL_K = (4 · 2 · STEP_END · C30) / 3.0`（L413，3.0 为数据极差）。步长 0.62 时坡度恰为 45.0°；初始步长 0.24 时约 68.8°（本次按公式验算）。量角器用首末点 `atan2` 实测，`|slope − 45°| < 1.5°` 才变金（L719–733）。
- **26° 光源**：太阳屏幕位置 `(ox − sin θ·R, oy − cos θ·R)` 与 `lightVec(θ)` 用同一个 θ，所以太阳位置、方块明暗和投影方向始终一致；θ 从 −150°（右下，接近从下往上打光）转到 26°。凹凸圆盘只有线性渐变（亮 `#fbfaff` → 中 `#a7a0f5` → 暗 `#3f33b8`），渐变方向随 θ 走，演示「明暗方向决定凸/凹」的知觉错觉（L748–766）。

### 5.6 小人的程序化动画

`drawScientist`（L247–350）用圆角矩形、二次曲线画出白大褂、蓝领带、口袋两支笔、圆眼镜、深紫发型；参数 `facing`（左右翻转）、`armF/armB`（手臂角，0 = 下垂、π/2 = 前伸、π = 上举）、`lean`、`hop`、`mood`（`puzzled` 一字嘴 / `happy` 微笑加腮红）。`sciBase(t)`（L432–519）是按幕写死的走位与姿势函数；`sciPose(t)`（L521–536）对比前一帧位置求速度，自动决定行走步幅（`clamp(speed/2.2, 0.35, 0.9)`）与朝向，并记忆上一次朝向。`--still` 模式先预跑 10 帧来「热身」朝向记忆（L1033–1038）。

### 5.7 关键词卡点

```js
function keyTime(id, word) {   // 词边界起点；没有词边界时按字数比例估算
  const i = s.text.indexOf(word);
  const w = s.words?.find(w => w.idx <= i && i < w.idx + w.text.length) ?? s.words?.find(w => w.idx >= i);
  return w ? w.start : s.start + (s.end - s.start) * i / s.text.length;
}
```

每个演示的动作区间都写成「到位时刻前 2.0–2.6 s 开始、前 0.1 s 完成」，让数字被念出的瞬间正好是角度到位、徽章弹出的瞬间。

## 6. 声音与字幕

### 配音

- 引擎：Microsoft Edge 在线神经 TTS（`edge_tts`），声线 `zh-CN-XiaoxiaoNeural`，最终全局语速 +6%，无 pitch 调整。
- 逐段合成，缓存键 `sha1("voice|rate|text")[:16]`（无 pitch 字段，L39）；429 限流时按 8/16/24/32 s 退避重试。
- 排轨（`tts.py` L132–155）：

```text
LEAD_IN = 0.35, GAP = 0.45, TAIL = 0.6, TARGET = 30.0
speech_i = 该段最后一个词的 end（不含 mp3 尾部静音）
若 LEAD_IN + Σspeech + 5·GAP + TAIL > 30 → 按比例一次性提高 rate 重合成
gap = GAP + max(0, 30 − total) / 5      # 剩余时间平摊进 5 个段间停顿
duration = max(30, 末段结束 + TAIL)
```

  本片：rate +6% 时 Σspeech = 26.486 s，total = 29.686 s，平摊后 gap ≈ 0.513 s，片长恰为 30 s（本次重算）。
- 旁白总线：`adelay` 放位 → `amix(normalize=0)` → `highpass 80 Hz` → `loudnorm I=-16:TP=-1.5:LRA=11` → `apad/atrim` → `narration.wav`（48 kHz 双声道）。成片响度本次未测量。
- 无配乐、无音效：`render.mjs` 只把 `narration.wav` 作为第二路输入直接编码（L1044–1050），符合文稿「不要配乐」。

### 字幕

- `split_caption`（L98–111）按 `，。？！` 切块，每块 ≤14 字，句号处强制断开；去掉块尾逗号/句号。
- `caption_cues`（L114–123）以块内首词 start、末词 end 作为字幕起止，**来自 TTS 词边界**。
- 外挂 SRT 14 条（例如「第一，物体」4.342–5.509 s），画面内烧录字幕同源同时；同一句内各块瞬切，只有整句首尾淡入淡出（`drawSubtitle` 注释）。

## 7. 迭代、问题与修正

未找回制作对话，以下只来自代码注释、缓存与文件时间戳。

| 事项 | 证据 | 结论等级 |
| --- | --- | --- |
| Edge TTS 旧主机自 2026-10 起对合成返回 403 → 改连 `api.msedgeservices.com` | `tts.py` L18–24 注释，引用 edge-tts #490、#491 | 代码中存在 |
| 免费端点 429 限流 → 退避重试 | `tts.py` L49–55 | 代码中存在；是否触发未记录 |
| **自动语速收敛实际发生过一次**：缓存里 6 段各有 rate 0 与 rate +6 两份。rate 0 时 Σspeech = 28.074 s、total = 31.27 s > 30；按公式 `ceil((28.074/26.8 − 1)·100) + 1 = 6` 跳到 +6，total = 29.69 s ≤ 30，停止 | 本次用缓存键公式对 `script.json` 文本反算 12 个键全部命中，并按 `tts.py` 公式验算 | 日志/缓存显示执行过；+6% 为最终采用（与 SRT、30 s 时长一致） |
| 最终 SRT 时间码与 rate +6 缓存重算结果一致（如 hook 首块 0.397 s 起） | SRT 与重算对照 | 最终采用（交叉印证） |
| 时间顺序：文稿 2026-10-13 13:56 → `package.json` 13:59 → `out/` 目录 15:23 创建 → `render.mjs`、`script.json` 20:11 → `tts.py` 20:12 → 缓存 20:12–20:13 → SRT 20:13 → MP4 20:14 | 文件 mtime | 推断：15:23 前后已有一次早期输出，20:11 起又改了脚本/渲染并重新出片；早期版本内容、早期脚本文本均无法确认（缓存中只有当前文本，可能曾清理） |
| 「画面上别堆文字」：画面文字只有数字、引用、问号与字幕；四条结论全部靠方块数量、交叉角、坡度和光照变化演示 | `render.mjs` 全文 | 代码中存在；视觉效果未检查 |
| 与 `word-imperative` 的关系：本片是同一 TTS 管线较早、较简的版本（单声线、统一 gap、自动切字幕、单文件渲染器、无音乐） | 代码对比 + 文件时间 | 推断 |

未发现 `plan/`、失败候选、预览帧或用户反馈；`--still` 预览功能存在，但 `build/still_*.png` 不在（被清理或未生成，无法确认）。

## 8. 可复用制作经验

1. **把「数字」做成动画的终点。** 每个要点的演示动作都在数字被念出的瞬间完成（`keyTime(段, '九十度')` 等），观众听到数字时正好看到角度到位、徽章弹出。依据：`render.mjs` L368–371、L855–860。
2. **讲角度就真的算角度。** 45° 坡度由投影几何反推柱高系数 `COL_K`，26° 光源与太阳位置、明暗、投影共用一个 θ；这样画面经得起截图测量，也方便改数字。依据：`render.mjs` L413、L424、L74–78。
3. **固定片长：先调语速，再把余量平摊进停顿。** 超时按比例一次跳到所需语速，不足则均匀加长停顿，片长精确落在 `TARGET`。依据：`tts.py` L132–155。
4. **需求文稿 → 结构化脚本字段。** 把「每点一个数字 + 出处」直接设计成 `script.json` 的 `number` / `cite` 字段，HUD 从脚本读取，改文案不碰渲染代码。依据：`script.json`、`drawBadge`。
5. **阴影单独一层合成一次**，裁切到地面，避免多个物体阴影重叠处变得更黑。依据：`render.mjs` L974–995。
6. **等轴场景用「中心 x+y」画家排序，并把角色插进同一序列**，就能让角色在积木之间穿行而不写深度缓冲。依据：`render.mjs` L997–1006。
7. **角色朝向与步幅由前后帧位移自动推导**，走位函数只需给出位置和少数姿势参数。依据：`sciPose` L521–536。

## 9. 复现入口

记录命令不代表本轮执行过。`tts.py` 和 `render.mjs` 的命令来自文件头部注释；`npm install` 与 venv 安装两行是整理者按 import 补的（项目没有 Python 依赖清单）。

```bash
cd research-visual
npm install                              # 恢复 @napi-rs/canvas 1.0.11
python3 -m venv .venv && .venv/bin/pip install edge-tts   # 版本未记录
.venv/bin/python tts.py                  # build/seg*.mp3、build/narration.wav、build/timeline.json、out/科研视觉怎么做.srt
node render.mjs --still 3.2 7.6          # 可选：预览单帧 build/still_<t>.png
node render.mjs                          # 逐帧渲染 900 帧并编码 out/科研视觉怎么做.mp4
```

- 需要 macOS（`Hiragino Sans GB.ttc` 路径写死）和 PATH 中的 `ffmpeg`。
- `build/tts_cache` 中保留了 rate 0 与 +6 两套音频与词边界：不改 `script.json` 文本时，`tts.py` 先命中 rate 0、判定超时后命中 +6，全程无需联网，可复现当前时间轴。
- 修改任意一句会触发联网合成，需要 `tts.py` 里的端点补丁或已修复的 edge-tts。
- 渲染耗时：未记录。

## 10. 参考来源

| 来源 | 类型 | 在本片中的位置 | 借鉴/用途 | 访问状态 |
| --- | --- | --- | --- | --- |
| `科普和教育的新革命.md`（项目内） | 创作需求（用户文稿） | 全片 | 主题、结构、风格、声音要求，见 §2 | 本地全文已读 |
| Luck & Vogel, Nature, 1997 | 内容引用（视频讲解的知识） | object 幕徽章 | 「一眼约记住 4 个」 | 项目只给出作者/期刊/年份，无链接；本次未查阅原文 |
| Huang, Hong & Eades, PacificVis, 2008 | 内容引用 | line 幕徽章 | 「交叉角接近 90°」 | 同上 |
| Cleveland, McGill & McGill, JASA, 1988 | 内容引用 | space 幕徽章 | 「平均坡度 45°」 | 同上 |
| Mamassian & Goutcher, Cognition, 2001 | 内容引用 | light 幕徽章 | 「光从左上约 26°」 | 同上 |
| https://github.com/rany2/edge-tts/issues/490 | 技术资料 | `tts.py` L19 注释 | edge-tts 合成握手 403 问题 | 2026-11-12 访问成功：Closed，2026-10-10 提出 |
| https://github.com/rany2/edge-tts/pull/491 | 技术资料（注释写作 #491，实为 PR） | `tts.py` L19 注释 | 改用 `api.msedgeservices.com` 合成端点的修复思路 | 2026-11-12 访问成功：Closed（未合并） |

- 视觉风格参考（图片、视频、帖子）：**未记录**。
- 四篇论文由谁检索、是否核对过原文与数值：未记录；档案不对其准确性作结论。

## 11. 证据与限制

实际读取的文件（项目相对路径，sha256 前 16 位取自 fingerprints.json）：

| 文件 | sha256 前 16 位 | 读取范围 |
| --- | --- | --- |
| `research-visual/科普和教育的新革命.md` | `bf9c5841ffcce001` | 全文 |
| `research-visual/script.json` | `1e0269f47400f2a5` | 全文 |
| `research-visual/tts.py` | `12e31de9466db77d` | 全文 |
| `research-visual/render.mjs` | `51986182e25731b0` | 全文（1062 行，分段阅读） |
| `research-visual/package.json` / `package-lock.json` | `b42c051b2bb801e3` / `56efadb1d116e74a` | 全文 / 版本字段 |
| `research-visual/out/科研视觉怎么做.srt` | `7df7206eea4b1d9a` | 全文 |
| `research-visual/build/tts_cache/*.json`（12 个） | 见 fingerprints.json | 词边界；用于反算 rate 与时间轴 |
| `research-visual/out/科研视觉怎么做.mp4` | — | 仅 ffprobe 媒体头 |

未读取 / 缺失：

- 未打开 `build/tts_cache/*.mp3` 内容；未观看成片、未抽帧、未测响度，因此「积木演示是否清楚、文字是否足够少」只有代码层面的证据，没有视觉检查。
- 缺失：`prompt.md`（由 `科普和教育的新革命.md` 代替）、设计文档、`build/timeline.json`、`narration.wav`、预览帧、渲染日志与耗时、早期版本（15:23 前后）内容、Python/edge-tts/Node/ffmpeg 版本、用户验收记录。
- 制作对话：未找回，本档案结论来自项目文件。
