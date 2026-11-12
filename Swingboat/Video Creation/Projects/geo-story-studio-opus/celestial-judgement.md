---
type: video-production-dossier
project: geo-story-studio-opus
video_id: celestial-judgement
title: 哪些算天体？天体认证处开张了
curriculum: script.json 未写 curriculum 字段；sources 指向“人教版《地理 必修第一册》：天体定义、脚注、太阳活动”
captured: '2026-11-12'
status: 成片存在（竖屏 1080×1920，85.37 s），无封面；技术检查/视觉检查/用户验收未记录
source_fingerprint: 803b2e8f79932eac7d5e1660dd85b0e74e805c529e3874fe0af08caf5a968e1b
tags:
- video-creation
- production-capture
- geography-teaching
- canvas-frame-render
- edge-tts
---

# 哪些算天体？天体认证处开张了 · 六声线“认证处审核”竖屏动画 + 记分牌

<!-- capture-source-sha256:803b2e8f79932eac7d5e1660dd85b0e74e805c529e3874fe0af08caf5a968e1b -->

本文是根据项目文件做的材料回溯（`script.json`、`tts.py`、`render.mjs`、`lib.mjs`、`bodies.mjs`、`props.mjs`、`scenes-a~d.mjs`、`audio.mjs`、`build/timeline.json`、`out/` 清单、证据包 ffprobe）。**未发现制作会话**。本次未运行任何项目脚本，未打开视频、音频和 logo。

`tts.py` 与 [[atmosphere-insulation]] 只差标题、节奏常量和自动分句上限，TTS 部分（端点替换、缓存、429 重试、WordBoundary、语速求解、旁白混音）的详细说明见 [[atmosphere-insulation]] §5。`render.mjs`、`lib.mjs` 和 `audio.mjs` 则是另一套写法：场景模块用 `readdirSync` 自动发现，顶部是五位“选手”的记分牌，`audio.mjs`（sha256 `9e3eeae951b5`）与 [[comet-tail]]、[[meteor-journey]] 等片相同，多了流星类音效。

## 1. 当前结论与制作路线

**状态**：`out/哪些算天体.mp4` 与 `out/哪些算天体.srt` 存在；**没有封面**（`render.mjs` 无 `--cover` 分支）。视觉检查、试听、用户验收**未记录**。

| 项 | 值 | 来源 |
| --- | --- | --- |
| 分辨率 / 画幅 | 1080×1920，9:16 竖屏 | ffprobe；`lib.mjs:7` |
| 帧率 / 帧数 | 30 fps；2561 帧 | ffprobe；85.351×30 四舍五入 |
| 时长 | 85.367 s（timeline 85.351 s） | ffprobe；`build/timeline.json` |
| 视频编码 | H.264 yuv420p，**CRF 19**，preset medium，faststart | ffprobe；`render.mjs:203` |
| 音频编码 | AAC 48 kHz 立体声 192 kbps | ffprobe；`render.mjs:204` |
| 文件大小 | 16,702,042 字节（约 15.9 MiB） | 证据包 |
| 字幕 | 41 条 SRT + 画面烧录（y=1460，角色名胶囊在上方） | `out/哪些算天体.srt`；`render.mjs:101-125` |
| logo | `assets/logo-transparent.png`（sha256 前缀 `3071b98be488`） | 证据包 |
| 其他 | 画面底部常驻小字“资料：NASA · 人教版《地理 必修第一册》” | `render.mjs:166` |

**管线**：`script.json`（23 段、6 个声音、5 条带 URL 的资料）→ `tts.py`（`TARGET = 86`，`LEAD_IN = 0.6`，`TAIL = 2.8`；脚本无全局 `rate`，最终 timeline `rate: 6`）→ `timeline.json` / SRT / `narration.wav` → `render.mjs` 自动导入全部 `scenes-*.mjs`（4 个模块、11 个场景），叠加记分牌、logo、字幕和资料小字，开头 0.25 s 淡入、结尾 0.6 s 淡出 → ffmpeg → MP4。

## 2. 教学目标与课标对齐

- **课标字段**：无 `curriculum`、无 `based_on`、无 `source` 文本；有 `sources` 数组，其中一条是“人教版《地理 必修第一册》：天体定义、脚注、太阳活动”。`README.md:17`：“月球、星云、流星、太阳黑子、待发射的人造卫星能否称为天体；记住‘对象、现象、局部、运行状态’”。
- **核心知识点**：天体的判别——看是不是宇宙中物质的存在形式（对象），区分物体与现象、整体与局部、运行状态。
- **讲解结构**：
  1. **导入**：`hook`“月球、流星、太阳黑子，都能单独叫天体吗？”→ `open` 天体认证处开张，五位选手排队 → `rule` “人教版这节课，通常讨论的是天然天体”（画面：课本《地理 必修第一册》+“这节课一般讨论：天然天体”）。
  2. **逐个审核**：月球 → 通过；星云（一大团气体和尘埃，没有硬壳）→ 通过，“天体不一定是坚硬的球”；流星 → “等等！这道亮光是一种现象”，真正的物体是流星体，冲进大气层才产生亮迹，“物体和现象要分开”；太阳黑子 → 不是星星，是太阳光球层中较暗的区域，是太阳的局部；人造卫星（还在发射台上）→ 还不算，进入绕地轨道运行后才是人造天体，“这是补充知识，要和课本范围分开”。
  3. **检测**：`quiz` 月球和流星现象，哪个是天然天体？（`gap: 1.3` s 留思考时间）→ `answer` 月球。
  4. **总结**：`recap` 四个词：对象、现象、局部、运行状态。
  5. **结尾**：`outro`“关注地理故事工坊，下期见！”+ 品牌卡。
- **科学边界**：教材范围与补充知识分开（`rule`、`satNote` 两句旁白）；黑子画成“比周围暗、但不是纯黑”的棕橙色（`bodies.mjs:70, 104-116` 注释“Darker than surroundings, never pure black”）；流星亮迹只在大气层内出现（`scenes-b.mjs:65` 注释“trail only exists inside the air, never above it”）；星云画成无硬边的重叠光晕和漂移尘粒（`bodies.mjs:118`）。
- **数字**：无具体数值。

## 3. 技术与依赖

Node + `@napi-rs/canvas`（`^1.0.11`，lock 1.0.13；`loadImage` 读 logo）。`package.json` 是 `npm init` 默认模板：`"type": "commonjs"`、`"main": "index.js"`、占位 `test` 脚本——源码全是 `.mjs`，按扩展名走 ESM，不受 `type` 影响。Python `edge-tts`（运行时替换 `WSS_URL`）、`fonts.py`（fontTools）、系统 ffmpeg，版本均未记录。

| 角色（voice key） | voice | 脚本设定 | 实际（叠加全局 +6） | 字幕胶囊 | 段数 |
| --- | --- | --- | --- | --- | --- |
| 旁白 zh | `zh-CN-XiaoxiaoNeural` | +6% | +12% | 无 | 18 |
| 月球 moon | `zh-CN-YunxiaNeural` | +4% | +10% | “月球” | 1 |
| 星云 nebula | `zh-CN-XiaoyiNeural` | −4% / −2 Hz | +2% / −2 Hz | “星云” | 1 |
| 流星 meteor | `zh-CN-YunjianNeural` | +12% | +18% | “流星” | 1 |
| 太阳黑子 spot | `zh-CN-liaoning-XiaobeiNeural` | +6% | +12% | “太阳黑子” | 1 |
| 人造卫星 sat | `zh-CN-YunxiNeural` | +4% | +10% | “人造卫星” | 1 |

`tts.py:31-35`：`TARGET = 86.0`、`LEAD_IN = 0.6`、`GAP = 0.45`、`TAIL = 2.8`；自动分句上限 14 字（保温片为 12）。本片 23 段都没写 `captions`，全部走自动分句。

## 4. 镜头与时间轴

时间来自 `build/timeline.json`（23 段，41 条字幕）。场景一般在下一段开口前 0.3–0.35 s 切入，交叉淡化 `XF = 0.3`。从 `moonHi` 到 `quiz` 之间，顶部常驻五格记分牌（月球 / 星云 / 流星 / 黑子 / 卫星）。

| # | 场景 | 约时间 (s) | 口播段 | 画面要点 | 文件 |
| --- | --- | --- | --- | --- | --- |
| 1 | `hook` | 0–4.48 | hook | 三个“嫌疑人”+ 巨大问号 | `scenes-a.mjs:7-34` |
| 2 | `office` | 4.48–13.88 | open, rule | 霓虹招牌“天体认证处 / 今日开张”，五位选手带号码排队；课本《地理 必修第一册》+“这节课一般讨论：天然天体” | `scenes-a.mjs:48-83` |
| 3 | `moonScene` | 13.88–18.09 | moonHi, moonOk | 月球自我介绍，盖章通过 | `scenes-a.mjs:85-95` |
| 4 | `nebulaScene` | 18.09–26.57 | nebHi, nebOk | 软绵绵的星云，通过；“天体不一定是坚硬的球” | `scenes-a.mjs:97-…` |
| 5 | `showOff` | 26.57–32.41 | metHi, wait | 流星炫耀亮迹，“等等！”警报 + 盖章 + 震屏 | `scenes-b.mjs:7-37` |
| 6 | `split` | 32.41–42.25 | body, enter, split | 小石块从太空飞入大气层，亮迹只在大气内出现；“流星体 · 太空中的物体”与“流星 · 大气中的亮迹”两个标签各用虚线箭头指向；印章“物体 ≠ 现象” | `scenes-b.mjs:55-84` |
| 7 | `sunspot` | 42.25–54.38 | spotHi, spotNo, spotPart | 太阳盘面上棕橙色的黑子群，强调“较暗区域、太阳的局部” | `scenes-c.mjs:10-55` |
| 8 | `satScene` | 54.38–68.47 | satHi ~ satNote | 左右两格：“待发射 / 尚未入轨”≠“入轨运行” | `scenes-c.mjs:67-…` |
| 9 | `quiz` | 68.47–75.36 | quiz, answer | A/B 选项卡，1.3 s 停顿配三声滴答，揭晓“月球” + 彩纸 | `scenes-d.mjs:24-57` |
| 10 | `recap` | 75.36–79.91 | recap | 对象 · 现象 · 局部 · 运行状态四词按词弹出 | `scenes-d.mjs:66-85` |
| 11 | `outro` | 79.91–85.35 | outro | 品牌卡：居中大 logo（宽 820 px）+ 紫色光晕，五位选手绕 logo 公转，“关注 · 下期见！” | `scenes-d.mjs:87-114` |

## 5. 关键算法与实现

> 区分：视频**讲的知识**是天体判别；下面是**制作用的画法**，天体外观都是卡通示意。

### 5.1 场景模块自动发现（`render.mjs:18-24`）

```js
for (const f of readdirSync(here('./')).filter((f) => /^scenes-.*\.mjs$/.test(f)).sort()) {
  const m = await import(`./${f}`);
  SCENES.push(...m.scenes); EVENTS.push(...m.events);
}
```

与保温片固定 `['a','b','c','d']` 列表不同，这里按文件名排序自动导入，新增 `scenes-e.mjs` 不用改 `render.mjs`。场景缩放中心是画面正中 `(W/2, H/2)`（保温片是舞台中心 y=880）。

### 5.2 记分牌：判决时刻绑定到关键词（`render.mjs:64-98`）

```js
{ name: '流星', id: 'metHi', verdict: () => at('split', '现象'), label: '现象' },
{ name: '黑子', id: 'spotHi', verdict: () => at('spotPart', '局部'), label: '局部' },
{ name: '卫星', id: 'satHi', verdict: () => at('satOrbit', '人造天体'), label: '看状态' },
...
const sc = (active ? 1.08 + 0.03 * Math.sin(t * 6) : 1) * (done ? lerp(1.35, 1, easeOut(prog(t, tv, tv + 0.35))) : 1);
```

当前受审选手的格子轻微呼吸放大并加金边；念到判决关键词那一刻，格子从 1.35 倍回弹到 1，通过者变绿打勾，未通过者变琥珀色并改写为“流星·现象”“黑子·局部”“卫星·看状态”。整块记分牌在 `moonHi` 前 0.4 s 淡入，`quiz` 前淡出。

### 5.3 流星体轨迹与“亮迹只在大气内”（`scenes-b.mjs:39-68`）

```js
const len = Math.max(0, Math.min(Math.hypot(x - P1[0], y - P1[1]), (y - AIR_TOP) / (dy / Math.hypot(dx, dy))));
if (inAir > 0) streak(ctx, x, y, dx, dy, len * 1.05, 60, inAir);
```

石块先在太空段 `P0→P1` 缓动（念“流星体”），在“冲进”一词处沿 `P1→P2` 以 `easeIO` 冲入大气层；尾迹长度取“已飞行距离”与“石块到大气顶的斜距”中的较小者，所以尾迹永远不会画到大气层上方。`inAir` 同时控制尾迹透明度和石块发热发光，到达后仍以小速度前移，保持“它是亮迹的头部”。

### 5.4 品牌卡与角标交接（`render.mjs:49-62`；`scenes-d.mjs:86-114`）

角标 logo 按 `AGENTS.md` §4 的竖屏参数绘制（`x=64, y=76, w=290`，0.3–1.0 s 淡入，径向暗晕），并乘以 `1 − prog(t, S('outro') − 0.4, S('outro'))`，在品牌卡出现前淡出，避免同屏两个 logo。品牌卡 logo 宽 820 px，从 0.8 倍缩放淡入；五位选手图标以 `(t − t0)·0.6` rad/s 绕椭圆（480×330）公转。

### 5.5 字幕：固定字号 + 整体缩放（`render.mjs:101-125`）

字幕固定 60 px、14 px 深色描边，超过 960 px 宽时整体按 `960 / w` 缩小；角色台词在字幕上方加一枚彩色角色名胶囊（颜色来自 `lib.mjs:75-77` `SPEAKER`）。与保温片的“按宽度反推字号 + 胶囊底”不同。

## 6. 声音与字幕

- 六个声音见 §3。`build/tts_cache/` 92 个文件 = 23 段 × 2 种全局语速（推断为 +0% 与 +6%），mp3 时间集中在 10-23 16:35–16:37。`build/` 中**没有** `seg*.mp3`（其他片都有），推断合成后被清理；不影响用缓存重建。
- 旁白混音与保温片相同（`highpass` + `loudnorm -16 LUFS`）。成片混音：配乐 ×**0.75** 经侧链压缩、音效 ×**0.5**、`alimiter 0.93`（保温片为 0.8 / 0.55）。
- `audio.mjs` 229 行：配乐框架同保温片，音效表去掉 `zap/wobble/static`，新增 `fizz`（火花熄灭）、`boom`（冲入大气的闪光）、`roar`、`thud`、`sizzle`。本片 `events` 共 49 条（a 18、b 10、c 12、d 9）。
- SRT 41 条：首条 `00:00:00,645 → 00:00:02,687 月球、流星、太阳黑子`，末条 `00:01:20,254 → 00:01:22,431 关注地理故事工坊，下期见！`。
- 本次未试听、未测响度。

## 7. 迭代与问题

| 事项 | 证据 | 说明 |
| --- | --- | --- |
| 品牌卡上有“关注”号召，且有旁白 | `scenes-d.mjs:101` “关注 · 下期见！”；`script.json` `outro`“关注地理故事工坊，下期见！” | `AGENTS.md` §4：品牌卡“只放品牌和标语，不加…‘关注/点赞/收藏’等号召文字”“不加旁白”。用户是否另有要求**未记录** |
| 品牌卡期间字幕仍显示 | `render.mjs:165` 每帧都调 `subtitles()`，没有按 `outro` 关闭 | §4 要求品牌卡出现时字幕停止；角标 logo 的淡出则已实现 |
| 品牌卡 logo 宽 820 px | `scenes-d.mjs:91` | §4 建议约 760 px；也没有“与主题相关的标语”，用“关注 · 下期见！”代替 |
| `TARGET` 把全局语速从 0 推到 +6% | `tts.py:32`；`timeline.json` `rate: 6`；缓存 23×2 组 | 现行 `AGENTS.md` §2 默认 `TARGET = None` |
| 无封面 | `render.mjs` 无 `--cover`；`out/` 无 PNG | 与同期 [[spacecraft-missions]]、两部大气片不同 |
| 底部资料小字 | `render.mjs:166` | 符合 `AGENTS.md` §5“底部可改放资料来源” |
| 缺 `curriculum` / `based_on` | `script.json` 键 | 不满足 §6；`sources` 已写课本条目 |
| `package.json` 是 `npm init` 模板 | `"type": "commonjs"`、占位 `test` | 不影响 `.mjs` 运行 |
| README | `README.md:17` 已收录，标 1:25 | 与 ffprobe 85.37 s 一致 |

## 8. 复现入口

命令来自 `AGENTS.md`“常用命令”和 `render.mjs:2-3`，**本次未执行**：

```bash
cd celestial-judgement
python3 -m venv .venv && .venv/bin/pip install edge-tts   # fonts.py 另需 fonttools（推断）
npm install
.venv/bin/python tts.py          # → build/timeline.json、build/narration.wav、out/哪些算天体.srt
node render.mjs --still 8.0 14.2 # → build/still_<t>.png（本片没有 --safe、--cover）
node render.mjs                  # → out/哪些算天体.mp4
```

先跑 `tts.py`；`lib.mjs` 加载时还需要 `assets/logo-transparent.png`。缓存含两轮全部 23 段，**推断**可离线重建。

## 9. 参考来源

`script.json` `sources` 中的 5 条，均为**项目文件中提及，本次未访问**：

| 来源 | 用途 |
| --- | --- |
| NASA “Meteors and Meteorites” — https://science.nasa.gov/solar-system/meteors-meteorites/ | 流星体 / 流星 / 陨石区分 |
| NASA “Sunspots” — https://science.nasa.gov/sun/sunspots/ | 黑子是光球层较暗区域 |
| NASA Space Place “What Is a Satellite?” — https://spaceplace.nasa.gov/satellite/en/ | 人造卫星 |
| NASA “Basics of Space Flight, Chapter 1-3” — https://science.nasa.gov/learn/basics-of-space-flight/chapter1-3/ | 入轨运行 |
| 人民教育出版社《地理 必修第一册》：天体定义、脚注、太阳活动 — http://www.pep.com.cn | 教材口径 |

## 10. 证据与限制

已读文件（`celestial-judgement/` 下，sha256 前 12 位）：`script.json` 45 行 `fe9b7bc8bdae`（全文）；`tts.py` 208 行 `30b1aacf63f1`（与保温片 diff）；`render.mjs` 212 行 `f60d45dc2b1c`（全文）；`lib.mjs` 244 行 `a09dd64b490a`（导出目录、`C`、`SPEAKER`）；`scenes-a.mjs` 154 行 `125eea1d0240`、`scenes-b.mjs` 104 行 `18efbba084e1`、`scenes-c.mjs` 143 行 `9521e465fcda`、`scenes-d.mjs` 133 行 `e967db53cd65`（场景表、split / outro 全文、其他场景文字）；`bodies.mjs` 172 行 `9c455d3042a5`、`props.mjs` 151 行 `911005ad14ea`（函数目录、`sunspots`）；`audio.mjs` 229 行 `9e3eeae951b5`（与保温片 diff）；`fonts.py` `a6ac7264e548`；`package.json` `aaed20d0c6a1`、`package-lock.json` `718958bd7650`；`build/timeline.json`；`out/*.srt` 首尾。成片仅有证据包 ffprobe。

限制：未发现制作会话；“关注”号召是否为用户要求、语速决策、预览轮次**未记录**；未打开成片和 logo，记分牌、字幕是否与 logo 区或平台 UI 重叠**无法确认**；5 个 URL 本次未访问，内容是否与旁白一致未核对；Node / Python / ffmpeg 版本未记录；git 只有初始化提交 `4831a07`（2026-10-23）。
