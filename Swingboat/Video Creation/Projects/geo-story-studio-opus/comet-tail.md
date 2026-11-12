---
type: video-production-dossier
project: geo-story-studio-opus
video_id: comet-tail
title: 彗星的尾巴，一定拖在身后吗？
curriculum: 人教版必修第一册 第一章第一节 地球的宇宙环境（天体类型：彗星）；关联第一章第二节 太阳对地球的影响（太阳风）；课标 1.1
captured: '2026-11-12'
status: 成片存在（竖屏 1080×1920，91.57 s），无封面；技术检查/视觉检查/用户验收未记录
source_fingerprint: 7c639428104ff8a7cecc681adc660699f849d7ce63f2123089fd1a6dda1cdd1a
tags:
- video-creation
- production-capture
- geography-teaching
- canvas-frame-render
- edge-tts
---

# 彗星的尾巴，一定拖在身后吗？ · 按太阳方位实时计算彗尾方向的竖屏动画

<!-- capture-source-sha256:7c639428104ff8a7cecc681adc660699f849d7ce63f2123089fd1a6dda1cdd1a -->

本文是根据项目文件做的材料回溯（`script.json`、`tts.py`、`render.mjs`、`lib.mjs`、`comet.mjs`、`scenes-a~d.mjs`、`audio.mjs`、`build/timeline.json`、`out/` 清单、证据包 ffprobe）。证据包按元数据找到一个可能对应的宿主会话（`acdb2261…`，本地创建于 2026-10-24 10:01），但**宿主返回空记录，无法读取对话内容**；对应关系是**推断**——会话创建时间比 `script.json`（10:04）早几分钟。本次未运行任何项目脚本，未打开视频、音频和 logo。

代码谱系：`tts.py` 与 [[celestial-judgement]] 只差标题和 `TARGET`，`audio.mjs` 完全相同，`render.mjs`、`lib.mjs` 是在其基础上改写（记分牌换成“彗核 → 彗发 → 彗尾”进度条）。TTS、时间轴和旁白混音的通用机制见 [[atmosphere-insulation]] §5。

## 1. 当前结论与制作路线

**状态**：`out/彗星的尾巴一定拖在身后吗.mp4`、同名 `.srt` 存在；**没有封面**（无 `--cover` 分支）。视觉检查、试听、用户验收**未记录**。

| 项 | 值 | 来源 |
| --- | --- | --- |
| 分辨率 / 画幅 | 1080×1920，9:16 竖屏 | ffprobe；`lib.mjs:7` |
| 帧率 / 帧数 | 30 fps；2747 帧 | ffprobe；91.554×30 四舍五入 |
| 时长 | 91.567 s（timeline 91.554 s） | ffprobe；`build/timeline.json` |
| 视频编码 | H.264 yuv420p，CRF 19，preset medium，faststart | ffprobe；`render.mjs` |
| 音频编码 | AAC 48 kHz 立体声 192 kbps | ffprobe；`render.mjs` |
| 文件大小 | 21,169,204 字节（约 20.2 MiB） | 证据包 |
| 字幕 | 48 条 SRT + 画面烧录（`SUB_Y = 1500`） | `out/*.srt`；`render.mjs` |
| logo | `assets/logo-transparent.png`（sha256 前缀 `3071b98be488`），左上角常驻 | 证据包；`render.mjs` `brand()` |
| git | 由提交 `1aa541f`（2026-10-26，同批加入 5 个视频）纳入仓库 | 证据包 |

**管线**：`script.json`（21 段、旁白 + 会说话的哈雷彗星）→ `tts.py`（`TARGET = 92`；脚本 `rate: 4`，最终 timeline `rate: 15`）→ `timeline.json` / SRT / `narration.wav` → `render.mjs` 按 a–d 顺序导入场景模块（4 个模块、9 个场景），叠加“彗核 › 彗发 › 彗尾”进度条、logo、字幕和资料小字，首尾淡入淡出 → ffmpeg → MP4。

## 2. 教学目标与课标对齐

- **课标字段**（`script.json` `curriculum` 原文）：“人教版必修第一册 第一章第一节 地球的宇宙环境（天体类型：彗星）；关联第一章第二节 太阳对地球的影响（太阳风）。课标1.1：运用资料，描述地球所处的宇宙环境，说明太阳对地球的影响。”本片是五部中唯一写了 `curriculum` 的。无 `based_on`。
- **核心知识点**：彗星结构（彗核、彗发、彗尾）及成因；彗尾**背向太阳**，可据彗尾判读太阳方位。
- **讲解结构**：
  1. **导入**：`hook`/`q` 古人叫它扫把星——尾巴真的一直拖在身后吗？（画面：扫把星掠过 +“飞行方向”箭头 +“尾巴一定拖在身后？”）
  2. **原理**：`hi`/`core` 远离太阳时的哈雷没有尾巴，彗核是冰和尘埃冻成的“脏雪球”，约 15×8 公里 → `hot`/`subl`/`jet` 受热升华（冰不经液态直接成气体），喷出的气体带出尘埃 → `coma`/`hair` 形成彗发 → `hands`/`dust`/`ion` 太阳的“两只手”：光压推尘埃成宽而弯的尘埃尾，太阳风拽电离气体成细而直的离子尾。
  3. **结论**：`rule` 两条彗尾都背向太阳，离子尾几乎正对太阳反方向 → `turn`/`ahead`/`huh` 绕过太阳往回飞时，尾巴跑到了前面。
  4. **迁移**：`wind` 风中长发的类比 → `exam` 高考判读：彗尾方向看太阳、不看飞行方向，太阳在彗尾反方向（画面给出上北下南的四选一题）。
  5. **总结**：`sum` 一句话链条 → `bye` 哈雷：每 76 年回来一次，2061 年记得抬头找我。
- **科学边界**：`comet.mjs:2` 注释“Tails are placed physically: both point away from the Sun; the dust tail bends toward the comet's past path”；`numbers` 每条都带出处原文（见下表）；画面底部资料小字只在总结段出现，注释写明遵守 `AGENTS.md` §5、不放“示意”声明（`render.mjs` `footer()`）。
- **数字与出处**（`script.json` `numbers`）：

| 数值 | 内容 | 出处（原文摘录） | 片中用法 |
| --- | --- | --- | --- |
| 通常不超过约 16 公里 | 彗核大小 | NASA Space Place《What Is a Comet?》：less than 10 miles (16 km) | 旁白“只有十几公里长” |
| 约 15×8 公里，反射约 3% 的光 | 哈雷彗核 | NASA Science《1P/Halley》 | 画面“哈雷彗核 约 15×8 公里”；3% 未使用 |
| 约 76 年；1986 年来过，2061 年回归 | 哈雷周期 | NASA Science《1P/Halley》 | 结尾“周期 约 76 年”“2061 下次见” |
| 数百万英里 | 彗尾长度 | NASA Space Place | 未在片中出现 |

## 3. 技术与依赖

Node + `@napi-rs/canvas`（`^1.0.11`，lock 1.0.13；`loadImage` 读 logo）；`package.json` 是 `npm init` 模板（`"type": "commonjs"`、占位 `test`），源码为 `.mjs` 不受影响。Python `edge-tts`（运行时替换 `WSS_URL`）、`fonts.py`（fontTools）、系统 ffmpeg，版本均未记录。

| 角色 | voice | 脚本设定 | 实际（叠加全局 +15） | 字幕标签 | 段数 |
| --- | --- | --- | --- | --- | --- |
| 旁白 zh | `zh-CN-XiaoxiaoNeural` | 全局 +4% | +15% | 无 | 15 |
| 哈雷 comet | `zh-CN-YunxiaNeural` | +6% / +6 Hz | +21% / +6 Hz | 青色“哈雷” | 6（hi、hot、hair、cool、huh、bye） |

`tts.py:31-35`：`TARGET = 92.0`、`LEAD_IN = 0.6`、`GAP = 0.45`、`TAIL = 2.8`（注释“room for the closing confetti”），自动分句上限 14 字；21 段中 10 段手写 `captions`。

## 4. 镜头与时间轴

时间来自 `build/timeline.json`（21 段，48 条字幕）。场景在下一段开口前 0.3 s 切入，交叉淡化 `XF = 0.3`，最后一个场景不再淡出（`render.mjs` `last` 判断）。顶部进度条“彗核 › 彗发 › 彗尾”在对应词出现时依次点亮，`rule` 前淡出。

| # | 场景 | 约时间 (s) | 口播段 | 画面要点 | 文件 |
| --- | --- | --- | --- | --- | --- |
| 1 | `hook` | 0–6.64 | hook, q | “古人叫它 扫把星”，扫帚形彗星掠过并标“飞行方向”；“尾巴一定拖在身后？” | `scenes-a.mjs:20-50` |
| 2 | `far` | 6.64–18.58 | hi, core | 远处的太阳“很远很远”，冷冰冰的彗核“尾巴：0”；剖开显示冰与尘埃，“脏雪球”印章，“哈雷彗核 约 15×8 公里” | `scenes-a.mjs:52-…` |
| 3 | `heat` | 18.58–30.39 | hot, subl, jet | 热浪逼近，彗核冒气；“固态冰 → 液态 → 气体”卡片：念到“液态”时打红叉，金色弧线从冰跳过液态直达气体，“升华”印章；喷流带出“气体 + 尘埃” | `scenes-b.mjs:47-113` |
| 4 | `comaScene` | 30.39–35.95 | coma, hair | 朦胧彗发（空心圆 = 气体、实心点 = 尘埃），哈雷“我长头发啦” | `scenes-b.mjs:138-…` |
| 5 | `hands` | 35.95–51.13 | hands, dust, ion, cool | 太阳伸出“① 光压”“② 太阳风”两只手：金色光线推出宽而弯的尘埃尾，蓝色粒子流拉出细而直的离子尾 | `scenes-c.mjs:31-97` |
| 6 | `orbit` | 51.13–64.23 | rule, turn, ahead, huh | 椭圆轨道（太阳在焦点），彗星绕日一周，两条彗尾始终背日，留下半透明“残影”；“背日”印章、“尾巴跑到前面！” | `scenes-c.mjs:99-…` |
| 7 | `windScene` | 64.23–69.40 | wind | 风中走路的人，往哪走头发都朝下风；“风 ≈ 光压 + 太阳风　头发 ≈ 彗尾” | `scenes-d.mjs:42-76` |
| 8 | `exam` | 69.40–76.85 | exam | “高考常见判读：太阳在哪个方位？”上北指向标，彗尾指东北、飞行方向指西北（干扰项被划掉）；四个候选方位中“B 西南”亮出太阳；“✓ 彗尾看太阳”“✗ 不看飞行方向” | `scenes-d.mjs:80-126` |
| 9 | `finale` | 76.85–91.55 | sum, bye | “一句话记住”步骤链；“周期 约 76 年”“2061 下次见”+ 彩纸 | `scenes-d.mjs:136-…` |

结尾 0.6 s 淡出到黑；**没有片尾品牌卡**，左上角 logo 一直显示到最后。

## 5. 关键算法与实现

> 区分：视频**讲的知识**是彗星结构与彗尾方向；下面是**制作用的几何算法**——用太阳位置和运动方向实时计算彗尾朝向，属于定性示意，不做光压 / 太阳风的动力学计算，尺度也不按比例。

### 5.1 彗尾方向由太阳位置决定，尘埃尾向轨迹后方弯曲（`comet.mjs:203-288`）

```js
const anti = Math.atan2(y - sy, x - sx);              // 太阳 → 彗核 的方向 = 背日方向
let diff = vel + Math.PI - anti;                        // 运动反方向与背日方向的夹角
diff = Math.atan2(Math.sin(diff), Math.cos(diff));     // 归一到 (−π, π]
const bend = clamp(diff, -0.9, 0.9) * 0.7;
dustTail(ctx, t, x, y, anti, len * 0.85, bend, dust);  // 尘埃尾：背日 + 向后方弯
ionTail(ctx, t, x, y, anti, len, ion);                  // 离子尾：严格背日、直
```

`tailPoint()` 沿尾巴参数 `s∈[0,1]` 取点，方向 `ang + bend·s·0.5`、切向 `ang + bend·s²`，`ribbon()` 用 31 个采样点左右偏移出逐渐变宽的带状多边形并填线性渐变。于是任何场景只要给出彗核位置 `(x, y)`、太阳位置 `(sx, sy)` 和速度方向 `vel`，两条尾巴就自动画对——这是“绕过太阳后尾巴跑到前面”能自然出现的原因。

### 5.2 椭圆轨道、太阳在焦点（`scenes-c.mjs:99-128`）

```js
const OX = 540, OY = 1080, A = 560, B = 330, SUN_Y = OY - Math.sqrt(A * A - B * B);   // c = √(a²−b²)
const orbitPt = (th) => [OX + B * Math.sin(th), OY - A * Math.cos(th)];
const orbitVel = (th) => Math.atan2(-A * Math.sin(th), -B * Math.cos(th));        // θ 随时间减小
const len = clamp(400 - dist * 0.3, 170, 300) * (o.k ?? 1);                        // 离太阳越近尾巴越长
```

θ 在 `rule` 到“绕过”之间从 1.9 缓动到 0.55，再到 `ahead` 结束时到 −1.55（`easeIO` 分两段），越过 1.6、0.9、−0.9 三个位置后各留一个 40% 透明的小“残影”彗星，让观众同时看到不同位置的彗尾都背日。

### 5.3 升华与喷流粒子（`scenes-b.mjs:8-27`）

`outgas()` 对 n 个粒子用确定性伪随机 `rand(i)` 分配方向、速度，按 `(rand + (t−t0)·v) mod 1` 循环外飘，透明度 `sin(π·ph)` 两头淡；前 `n·dust` 个画实心橙点（尘埃），其余画空心浅蓝圈（气体）。彗发场景沿用同一套“空心 = 气体、实心 = 尘埃”约定。光压是 8 条沿背日方向流动的金色短线，太阳风是 40 个向外扩散并渐隐的蓝点（`scenes-c.mjs:34-54`）。

### 5.4 判读题的方位几何（`scenes-d.mjs:78-114`）

画面先画“北”指向标（上北下南），彗星放在 `(540, 760)`，太阳虚拟放在左下很远处 `(cx − 2000, cy + 2000)`，于是彗尾自动指向东北；飞行方向箭头指向西北作干扰。四个候选位置按 `(dx, dy) = (±1, ±1)` 摆在 290×280 px 的四角，讲到“反方向”时只有 B（西南）换成太阳。题目方位与 5.1 的彗尾算法共用同一函数，答案与画面一致。

### 5.5 进度条、字幕与资料小字（`render.mjs`）

- 进度条 `CHIPS`：三个胶囊在 `at('core','彗核')`、`at('coma','彗发')`、`at('dust','尘埃尾')` 时刻从“？”弹出为名称。
- 字幕：胶囊底，58 px 起每次减 2 px，直到整行宽度 ≤ `W − 60`；哈雷台词带青色“哈雷”标签。
- 资料小字“资料：NASA Space Place · NASA Science · ESA · DLR”只在 `sum` 段开始后淡入，位于 y=1640。
- logo：`x=64, y=76, w=290`，0.3–1.0 s 淡入，径向暗晕（同 [[celestial-judgement]]，但去掉了片尾淡出）。

## 6. 声音与字幕

- 旁白 Xiaoxiao +15%，哈雷 Yunxia +21% / +6 Hz。`build/tts_cache/` 84 个文件 = 21 段 × 2 种全局语速（+4% 与 +15%），mp3 时间集中在 10-24 10:05–10:07。
- 旁白混音同保温片（`highpass` + `loudnorm -16 LUFS`）；成片混音同 [[celestial-judgement]]：配乐 ×0.75 经侧链压缩、音效 ×0.5、`alimiter 0.93`。`audio.mjs` 与 celestial 相同（含 `fizz/boom/roar/thud/sizzle`）。`events` 共 50 条（a 11、b 11、c 15、d 13）。
- SRT 48 条：首条 `00:00:00,643 → 00:00:01,817 古人叫它扫把星`，末条 `00:01:27,684 → 00:01:28,634 记得抬头找我！`。
- 本次未试听、未测响度。

## 7. 迭代与问题

| 事项 | 证据 | 说明 |
| --- | --- | --- |
| 没有片尾品牌卡，角标 logo 不淡出 | `scenes-d.mjs` 无品牌卡场景；`brand()` 去掉了 celestial 版的 `outro` 淡出；`TAIL = 2.8` 注释是“room for the closing confetti” | `render.mjs` 注释已引用 `AGENTS.md` §4、§5，但 §4 的“片尾品牌卡（必需）”未实现；规则写入时间与本片先后**无法确认** |
| 语速 +4% → +15% | `tts.py:32` `TARGET = 92`；timeline `rate: 15`；缓存 21×2 组 | 与现行 `AGENTS.md` §2（默认 `TARGET = None`）不一致 |
| 资料小字在 y=1640 | `render.mjs` `footer()` | 落在保温片定义的“1500 以下留给平台文案”区域内，可能被平台文字遮挡（推断，未看画面） |
| 数字有出处但部分未用 | `numbers` 中“反射约 3%”“数百万英里” | 片中未出现，不影响正确性 |
| 缺 `based_on` | `script.json` 键 | `curriculum`、`numbers`、`sources` 均齐全，是五部中最符合 §6 的一部 |
| 制作会话无法读取 | 证据包 `host_session` | 只能按时间推断对应关系 |
| README | `README.md:24` 已收录，标 1:32 | 与 ffprobe 91.57 s 一致 |

## 8. 复现入口

命令来自 `AGENTS.md`“常用命令”与 `render.mjs` 头部注释，**本次未执行**：

```bash
cd comet-tail
python3 -m venv .venv && .venv/bin/pip install edge-tts   # fonts.py 另需 fonttools（推断）
npm install
.venv/bin/python tts.py          # → build/timeline.json、build/narration.wav、out/彗星的尾巴一定拖在身后吗.srt
node render.mjs --still 8.0 14.2 # → build/still_<t>.png（无 --safe、--cover）
node render.mjs                  # → out/彗星的尾巴一定拖在身后吗.mp4
```

先跑 `tts.py`；`lib.mjs` 还需要 `assets/logo-transparent.png`。缓存含两轮全部 21 段，**推断**可离线重建。

## 9. 参考来源

`script.json` `sources` 中的 5 条，均为**项目文件中提及，本次未访问**：

| 来源 | 用途 |
| --- | --- |
| NASA Space Place “What Is a Comet?” — https://spaceplace.nasa.gov/comets/en/ | 彗核大小、彗尾背日、长度 |
| NASA Science “1P/Halley” — https://science.nasa.gov/solar-system/comets/1p-halley/ | 哈雷彗核尺寸、反照率、76 年周期、2061 回归 |
| NASA “Comet Facts” — https://science.nasa.gov/solar-system/comets/facts/ | 彗星基本事实 |
| ESA “Structure of a comet” — https://www.esa.int/ESA_Multimedia/Images/2023/11/Structure_of_a_comet | 彗核 / 彗发 / 尘埃尾 / 离子尾结构 |
| DLR “Comet research（升华、电离与太阳风）” — https://www.dlr.de/en/research-and-transfer/projects-and-missions/rosetta/comet-research | 升华、电离与太阳风作用 |

## 10. 证据与限制

已读文件（`comet-tail/` 下，sha256 前 12 位）：`script.json` 54 行 `ef9a1dfc342c`（全文）；`tts.py` 208 行 `54c10d09fb02`（与 celestial diff）；`render.mjs` 215 行 `22d795567cf8`、`lib.mjs` 240 行 `19e379a6a67a`（与 celestial diff）；`comet.mjs` 295 行 `f58daa931e6f`（函数目录、尾巴与整体彗星函数）；`scenes-a.mjs` 117 行 `4bf605db4e20`、`scenes-b.mjs` 186 行 `7650a88d9eaa`、`scenes-c.mjs` 183 行 `908b2767425c`、`scenes-d.mjs` 197 行 `b89c8e15d607`（场景表、hands / orbit / exam / outgas 代码、画面文字）；`audio.mjs` `9e3eeae951b5`、`fonts.py` `a6ac7264e548`；`package.json` `57466b2f4f08`、`package-lock.json` `0ee4fac19cb2`；`build/timeline.json`；`out/*.srt` 首尾。成片仅有证据包 ffprobe。

限制：宿主会话记录为空，需求原文、语速决策、预览轮次与用户反馈**未记录**；未打开成片和 logo，画面遮挡、资料小字可读性**无法确认**；5 个 URL 本次未访问；Node / Python / ffmpeg 版本未记录。
