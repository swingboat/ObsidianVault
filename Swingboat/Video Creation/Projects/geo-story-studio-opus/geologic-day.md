---
type: video-production-dossier
project: geo-story-studio-opus
video_id: geologic-day
title: 地球的一天：人类几点到场？
curriculum: 人教版必修第一册 第一章第三节 地球的历史（地质年代表）；课标 1.2
captured: '2026-11-12'
status: 成片存在（竖屏 1080×1920；完整版 229.3 s + 推荐版 120.9 s + 推荐版封面）；技术检查/视觉检查/用户验收未记录
source_fingerprint: d29a1099696d4aa5f51a0d23b6fc128615860e94d44e6a599b0b88ebe35f84af
tags:
- video-creation
- production-capture
- geography-teaching
- canvas-frame-render
- edge-tts
---

# 地球的一天：人类几点到场？ · “地球钟”比例模型双剪辑（`timemodel.mjs` 单一数据源 + `validate.mjs` 核对旁白钟点）

<!-- capture-source-sha256:d29a1099696d4aa5f51a0d23b6fc128615860e94d44e6a599b0b88ebe35f84af -->

本文是**根据项目文件做的材料回溯**（源码、`script.json` / `script-rec.json`、两份 `timeline.json`、`out/` 下的两版成片、字幕和封面，证据包里的 ffprobe 与 sha256 清单）。证据包中 `host_session` 为空：**未发现制作会话**，用户需求原文与修改过程无法找回。本次未运行任何项目脚本（没有跑 `tts.py`、`render.mjs`、`validate.mjs`、npm、pip），没有打开视频或音频；`build/` 下的 6 个 `.log` 文件只列了文件名和大小，未读取内容。

## 1. 当前结论与制作路线

**状态：两个版本都已出片**——完整版 `out/地球的一天.mp4`（10-30 14:43）+ `.srt`；推荐版 `out/地球的一天_推荐版.mp4`（14:42）+ `.srt` + `_推荐版_封面.png`（14:44）。`build/` 里有 `validate.log`（14:39）、`still.log`（14:40）、`render.log`、`final.log`、`tts.log`、`tts-rec.log`，说明作者跑过校验和预览帧（推断，日志内容未读）。视觉检查结论、用户验收与发布情况均**未记录**。

| 项 | 完整版 | 推荐版（`--rec`） | 来源 |
| --- | --- | --- | --- |
| 分辨率 | 1080×1920 竖屏 | 同左 | ffprobe |
| 帧率 / 帧数 | 30 fps；`round(229.292×30)` = 6879 帧 | 30 fps；`round(120.887×30)` = 3627 帧 | ffprobe；`render.mjs`（推算） |
| 时长 | 229.300 s（timeline 229.292 s，约 3:49） | 120.900 s（timeline 120.887 s，约 2:01） | ffprobe；timeline |
| 编码 | H.264 yuv420p CRF 19 preset medium `+faststart`；AAC 48 kHz 立体声 192 kbps | 同左 | ffprobe；`render.mjs` ffmpeg 参数 |
| 文件大小 | 23,616,760 B（约 23.6 MB） | 11,670,397 B（约 11.7 MB） | 证据包 |
| 字幕 | 133 条（7343 B） | 55 条（3229 B） | timeline cue 计数 |
| 文案 | `script.json` 38 段 | `script-rec.json` 18 段 | 文件 |
| 片尾品牌卡 | 有（`end` 结束 +0.3 s） | **有**（`sum` 结束 +0.3 s） | `render.mjs` `CARD_T = E(REC ? 'sum' : 'end') + 0.3` |
| 封面 | 无 | `_推荐版_封面.png`（`cover.mjs`） | 文件 |

`script.json` 修改时间（14:43:01）晚于 `build/timeline.json`（14:33:01），但逐段比对两者的 `id` 与 `text` 完全一致，后来的修改不涉及旁白文字。

一句话管线：

> `timemodel.mjs`（年龄数据 + 46 亿年→24 小时换算）→ 文案中的钟点先写进 `script.json` / `script-rec.json`，用 `node validate.mjs` 核对“旁白念的钟点 = 模型算出的钟点” → `tts.py`（可加 `--rec`）调 edge-tts，WordBoundary 取逐词时间 → timeline + `narration.wav` + SRT → `render.mjs` 加载 `scenes-a…d`（13 个场景）或 `scenes-r1…r3`（6 个场景），“地球钟”指针按 `dayHour(t)` 随旁白关键词转动 → `audio.mjs` 程序配乐/音效 → ffmpeg → MP4；`--rec --cover` 出封面。

## 2. 教学目标与课标对齐

- **curriculum**：人教版必修第一册 第一章第三节 地球的历史（地质年代表）；课标 1.2。
- **核心知识点**：用“把地质年代压成一天”的比例模型理解地质年代表的先后与时间跨度；区分人属与智人。
- **完整版结构（按文案）**：
  1. 导入：小林猜“下午三点”，报时员“地球钟”登场（`hook`…`clock-hi`）。
  2. 规则与换算：46 亿年取教材近似，0 点诞生、24 点现在；1 小时≈1.9 亿年、1 分钟≈320 万年、1 秒≈5.3 万年（`rule`、`scale`、`stu-life`）。
  3. 快进：前寒武纪占大半天（并非一片空白）→ 21:11 显生宙 / 古生代 → 放大最后 3 小时：22:41 中生代（爬行动物时代）、23:39 新生代（哺乳动物时代）（`clock-go`…`cz`）。
  4. 概念辨析：“人类”是哪一种？人属 ⊃ 智人，用犬属里的狼、郊狼、家犬类比（`which`…`dog-ok`）。
  5. 放大最后一分钟：LD 350-1 下颌骨 23:59:07–08，“化石标本不等于起源瞬间”；智人 23:59:54，离午夜约 6 秒；文字记载不到 0.1 秒（`clock-zoom`…`clock-blink`）。
  6. 方法迁移：比例公式，小林自己算新生代 20.7 分钟 → 23:39；钟点只是比例位置（`formula`…`calc-ok`）。
  7. 考点三条 + 人地协调（“只占最后几秒，却在深刻改变地球”）+ 一句话总结 + 午夜收尾（`exam`…`end`）。
- **推荐版**（`script-rec.json` 的 `based_on` 说明）：“教学完整版的发布剪辑：保留逐级放大主线（一天→最后三小时→最后一分钟→最后一秒），删去猜钟点、报时员登场、犬属类比、考试总结和情感收尾”。开头第一句就给结论“智人只占最后约 6 秒”，再转向人属。
- **科学边界（`based_on`）**：改编自 `geo-story-studio/storage/builds/douyin-geologic-day/v002`（n07“把46亿年压成一天，人类几点出现？”）。边界：46 亿年是教学近似分母；0 点诞生、24 点现在；界线按 ICS 2026/06；先分清人属与智人；LD 350-1 是标本记录，不是人属起源的精确瞬间；钟点是比例位置，不精确到秒；不画猿到人进步阶梯；不使用旧的“最后 47 秒”。
- **数字与出处**（`script.json` `model` + `numbers`）：

| 年代 | 模型钟点 | 出处 |
| --- | --- | --- |
| 地球约 46 亿年 | 0:00→24:00 | 人教版必修第一册；USGS *Trek Through Time* |
| 5.388 亿年前（显生宙 / 古生代起点） | 约 21:11 | ICS 年代地层表 2026/06 |
| 2.519 亿年前（中生代） | 约 22:41 | 同上 |
| 6600 万年前（新生代） | 约 23:39（剩 20.7 分钟） | 同上 |
| 280–275 万年前（LD 350-1 早期人属下颌骨） | 约 23:59:07–08 | Penn State News 2015（Villmoare et al., *Science*） |
| 约 30 万年前（智人） | 约 23:59:54（剩约 6 秒） | Smithsonian Human Origins |
| 约 5000 年（文字记载） | 不到 0.1 秒（约 0.094 s） | 常用近似（楔形文字约公元前 3200 年） |

本次按 `timemodel.mjs` 的公式手算复核，上表钟点与公式一致（如 `24×(1−538.8/4600)` = 21.189 h ≈ 21:11）。

## 3. 技术与依赖

| 类别 | 内容 | 证据 |
| --- | --- | --- |
| 渲染 | Node.js ESM，`@napi-rs/canvas` 声明 `^1.0.11`，lock 解析 **1.0.13** | `package.json`；lock |
| npm scripts | 仅 npm init 默认 `test`（报错退出），无实际脚本 | `package.json` |
| 校验脚本 | `validate.mjs`（33 行）+ `timemodel.mjs`（32 行），纯 Node，无额外依赖 | 文件 |
| TTS | Python `edge-tts` + `aiohttp`；版本未记录 | `tts.py` |
| 字体 | `fonts.py`（fontTools）；与其他视频同一份 | sha `a6ac7264e548` |
| 合成 | 系统 ffmpeg；版本未记录 | `render.mjs`、`tts.py` |

**声线**（两版相同；全局 `"rate": 4` 叠加）：

| 角色 | key | 声音 | 角色 rate / pitch | 实际 rate |
| --- | --- | --- | --- | --- |
| 旁白 | `zh` | zh-CN-XiaoxiaoNeural | 0 / 0 | +4% |
| 小林（学生） | `stu` | zh-CN-YunxiNeural | +6 / +2 Hz | +10% |
| 地球钟（报时员） | `clock` | zh-CN-YunyangNeural | −2 / −6 Hz | +2% |

## 4. 镜头与时间轴

场景对象直接写 `{ from, to, draw }`，边界多数取某句开始或某个词的发音时刻；交叉淡化 `XF = 0.3 s`（`render.mjs:24`）。

**完整版（4 个模块、13 个场景）**

| 场景 | 约时间 | 内容 | 模块 |
| --- | --- | --- | --- |
| `hook` | 0–11.2 s | 标题“把46亿年压成一天”；小林猜 15:00 | `scenes-a.mjs:27` |
| `intro` | 11.1–18.5 s | 地球钟闹钟登场 | `scenes-a.mjs:43` |
| `rule` | 18.5–27.6 s | 0 点诞生、24 点现在 | `scenes-a.mjs:64` |
| `scale` | 27.6–41.9 s | 1 小时 / 1 分钟 / 1 秒换算卡 | `scenes-a.mjs:97` |
| `fast` | 41.9–68.8 s | 指针快进过前寒武纪，21:11 显生宙 | `scenes-b.mjs:25` |
| `zoom` | 68.8–95.4 s | 放大 21–24 点：三个代、恐龙、哺乳动物 | `scenes-b.mjs:61` |
| `which` | 95.2–119.5 s | 人属 ⊃ 智人；犬属类比 | `scenes-c.mjs:14` |
| `minute` | 119.5–155.4 s | 最后一分钟标尺：LD 350-1 窄带 07–08 s，智人 54 s | `scenes-c.mjs:97` |
| `writing` | 155.4–164.6 s | 最后一秒：文字记载 <0.1 s | `scenes-c.mjs:153` |
| `formula` | 164.6–191.4 s | 比例公式 + 小林演算 | `scenes-d.mjs:7` |
| `exam` | 191.4–205.2 s | 考点三条；资料来源行出现 | `scenes-d.mjs:51` |
| `earth` | 205.2–214.5 s | 地球与人类（人地协调） | `scenes-d.mjs:80` |
| `finale` | 214.5 s–结束 | 一句话总结、午夜 | `scenes-d.mjs:104` |
| 品牌卡 | 约 226.3–229.3 s | logo + “用地理的眼光，读懂地球的时间 / 46亿年压成一天 · 先分清人属与智人” | `render.mjs` `endCard` |

**推荐版（3 个模块、6 个场景）**：`opening` 0–28.0 s（首帧即完整 24 小时表盘、顶部红点脉冲标出“最后约 6 秒”，随后表盘让位给人属 / 智人包含图）→ `fast` 28.0–42.6 s → `zoom` 42.6–68.7 s（含新生代演算）→ `minute` 68.7–97.3 s → `second` 97.3–108.9 s → `summary` 108.9 s–结束 → 品牌卡约 117.9–120.9 s（`scenes-r1.mjs:34`、`scenes-r2.mjs:17/52`、`scenes-r3.mjs:31/103/137`）。

## 5. 关键算法与实现

> 区分：视频**讲的科学**是地质年代表的顺序与跨度、人属与智人的区别；下面是**做视频用的算法**。

**5.1 单一数据源 `timemodel.mjs`（`timemodel.mjs:1-31`）**。所有年龄（Ma）集中在 `AGES`，钟点由公式计算，**只在显示时取整**：

```js
export const EARTH_MA = 4600;
export const DAY_S = 86400;
export const AGES = { pz: 538.8, mz: 251.902, cz: 66.0, homoA: 2.8, homoB: 2.75, sap: 0.3, write: 0.005 };
export const clockH = (ma) => 24 * (1 - ma / EARTH_MA); // model hour for an age in Ma
export const leftS = (ma) => (DAY_S * ma) / EARTH_MA; // model seconds still left before 24:00
```

`LABEL` 派生出画面要用的字符串（`21:11`、`23:59:07—08`、`sapLeft ≈ 6`、`czMin ≈ 20.7`、`hourYi ≈ 1.9`、`secWan ≈ 5.3`）。`lib.mjs:79` 把它们再导出，两版场景、封面都从这里取数，不在场景里手写数字。

**5.2 旁白钟点校验 `validate.mjs`（`validate.mjs:17-32`）**。把模型钟点转成口语形式（`21:11` → “21点11分”，`23:59:54` → “23点59分54秒”），检查两份脚本是否都包含应念的钟点；同时列出禁用的旧说法，发现就报错退出：

```js
const banned = ['1.4%', '约21分钟', '人属在最后一分钟', '人属：最后一分钟'];
for (const [cut, file] of [['rec', 'script-rec.json'], ['full', 'script.json']]) {
  const text = JSON.parse(readFileSync(new URL(file, import.meta.url))).segments.map((s) => s.text).join('\n');
  for (const s of must[cut]) if (!text.includes(s)) { console.log(`✗ ${file}: missing "${s}"`); bad++; }
  for (const s of banned) if (text.includes(s)) { console.log(`✗ ${file}: stale "${s}"`); bad++; }
}
```

这是本项目少见的“文案—数据一致性测试”，把 AGENTS.md 第 6 节“数字注明出处、不要凭印象”落实成了可运行的检查。

**5.3 指针关键帧 `dayHour(t)`（`lib.mjs:78-117`）**。`FULL_KEYS` / `REC_KEYS` 是一串 `[影片时刻, 模型钟点]`，影片时刻都取旁白词时间（如 `[at('clock-21','叮'), HR.pz]`、`[at('pre','中午'), 12]`），相邻关键帧之间用 `easeIO` 插值。注释写明“每次放大都先把快进走完，再出现放大标尺，所以读数、指针和标尺范围一致”。推荐版首帧指针在 24 点，进入前寒武纪时在 `S('pre') − 0.1 → −0.099` 两帧之间从 24 跳到 0（注释：24:00 与 0:00 在表盘同一位置，避免指针倒转一圈）。

**5.4 地球钟表盘（`cast.mjs:81-151`）**。`angleOf(h) = −π/2 + h/24·2π`，0/24 点在正上方、6 点在右；`ERAS(HR)` 把前寒武纪、古生代、中生代、新生代四段画成彩色圆环，`upTo` 控制圆环画到哪里；`rimPoint()` 给出圆周上某钟点的位置，用于标注。最后 6 秒在真实比例下远小于 1 像素，推荐版开场改用顶端脉冲红点表示（`scenes-r1.mjs` 注释“true width is far below one pixel”）。

**5.5 背景与品牌卡（`render.mjs`）**。背景保留按钟点计算天色、星星和日月弧的代码（`skyAt`、`nightness`），但实际把小时固定在 24（注释“one neutral night sky: no day/night cue tied to the model hour”），只有完整版片尾过了午夜才变亮。品牌卡 `CARD_T = E(REC ? 'sum' : 'end') + 0.3`，品牌卡出现后字幕停止（`if (t >= CARD_T) return;`），推荐版不从黑场淡入，角标 logo 首帧就显示。

## 6. 声音与字幕

- **TTS**：缓存键 `sha1(voice|rate|pitch|text)[:16]`，两版共用 `build/tts_cache/`（116 个文件）；429 退避重试；WSS 地址替换；`LEAD_IN = 0.6 s`、`TAIL = 3.2 s`（两版相同），`TARGET = None`。SRT 文件名取 `script.get('output_title', TITLE)`，推荐版用 `output_title: 地球的一天_推荐版`。
- **混音**：旁白 `adelay → amix → highpass=80 → loudnorm I=-16:TP=-1.5:LRA=11`；成片音乐音量 0.7（[[fossil-record]] 等为 0.75）经旁白侧链压缩避让，音效 0.5，`alimiter=0.93`。
- **音效**：两版场景事件合计约 97 个，以 pop 18、note 19、ding 13、whoosh 12 为主（钟点“叮”多用 ding），另有 alarm 4、tick 5、sparkle 8 等；配乐为共用的 112 BPM 拨弦循环。
- **字幕**：`SUB_Y = 1500`，小林 / 地球钟带名牌；起止取逐词时间；品牌卡出现即停。

## 7. 迭代与问题

- **README 未收录**：`README.md` 的“第一章第三节”表格只有 [[fossil-record]]，没有本片（全文检索“geologic-day”“地球的一天”均无结果）。
- **旧说法被淘汰的证据**：`validate.mjs` 的 `banned` 列出“1.4%”“约21分钟”“人属在最后一分钟”“人属：最后一分钟”，`based_on` 也写“不使用旧的‘最后47秒’”。推断：这些是早期文案或源 build 用过、后经审校改掉的说法；新生代剩余时间由“约 21 分钟”改为按公式取整的 20.7 分钟。
- **注释与代码不一致**：`render.mjs` 背景段标题注释仍写“sky follows the Earth Clock's model hour; stars come out at night”，`scenes-b.mjs:1` 也写“sky follows the hand”，但实际代码已把天色固定为夜空。推断：曾让天色随钟点变化，后因“钟点只是比例位置、不是真实时刻”的边界改为中性夜空，注释没同步。
- **复制残留**：`tts.py:3-4` docstring 仍写“detective host / student / shell fossil / drifted fossil … Neptune's far-away echo”，与 [[fossil-record]] 的 `tts.py` 相同；`audio.mjs` 与 [[fossil-record]]、[[meteor-journey]] 等逐字相同。
- **`based_on` 写的是本机绝对路径**（此处改写为 `geo-story-studio/...`）。
- **与 [[plateau-basin-sunlight]] 的推荐版对比**：本片推荐版**有**片尾品牌卡，符合 AGENTS.md 第 4 节“推荐版也要加”。
- 时间先后：两版 TTS 14:32–14:33 → `validate.log` 14:39 → `still.log` 14:40 → 推荐版成片 14:42、完整版 14:43 → 封面 14:44。git 只有批量提交 `8cbe673`（2026-11-01）。

## 8. 复现入口

依据 `render.mjs`、`validate.mjs`、`cover.mjs` 头注释和 AGENTS.md（本次**均未运行**）：

```bash
cd geologic-day
node validate.mjs                  # 核对两份脚本里的钟点（改文案后先跑）
.venv/bin/python tts.py            # 完整版 → build/, out/地球的一天.srt
.venv/bin/python tts.py --rec      # 推荐版 → build/rec/, out/地球的一天_推荐版.srt
node render.mjs --still 3.2 7.6    # 预览帧
node render.mjs                    # → out/地球的一天.mp4
node render.mjs --rec              # → out/地球的一天_推荐版.mp4
node render.mjs --rec --cover      # → out/地球的一天_推荐版_封面.png
```

环境：`.venv` 需 `edge-tts`（及 `fontTools` 供 `fonts.py`），`npm install` 安装 `@napi-rs/canvas`。

## 9. 参考来源

`script.json` `sources` 列出 5 个 URL（项目文件中提及，本次未访问）：

| 来源 | URL | 用途 |
| --- | --- | --- |
| 人教版 地理必修第一册 | https://jc.pep.com.cn/ | 46 亿年、代的概括 |
| USGS · Introduction: Trek Through Time | https://www.usgs.gov/youth-and-education-in-science/introduction | 地球年龄近似 |
| ICS International Chronostratigraphic Chart 2026/06 | https://stratigraphy.org/ICSchart/ChronostratChart2026-06.pdf | 显生宙、中生代、新生代界线 |
| Penn State · Earliest known fossil of the genus Homo… | https://www.psu.edu/news/research/story/earliest-known-fossil-genus-homo-dates-28-275-million-years-ago | LD 350-1 年代 |
| Smithsonian Human Origins · Homo sapiens | https://humanorigins.si.edu/evidence/human-fossils/species/homo-sapiens | 智人约 30 万年 |

改编来源：`geo-story-studio/storage/builds/douyin-geologic-day/v002`（n07 科学审校版，本次未读取）。

## 10. 证据与限制

| 文件 | 行数 | sha256 前 12 位 | 本次读取 |
| --- | --- | --- | --- |
| `script.json` / `script-rec.json` | 76 / 35 | `e2aaffe378da` / `5bf07c682b39` | 全文 |
| `timemodel.mjs` / `validate.mjs` / `cover.mjs` | 32 / 33 / 16 | `9c8f025c1432` / `f5b7356a4254` / `aea8c89f7848` | 全文 |
| `render.mjs` | 265 | `bd5890773fa7` | 与 [[fossil-record]] 版逐行 diff |
| `tts.py` | 216 | `262546bae55e` | 与 [[plateau-basin-sunlight]] 版逐行 diff |
| `lib.mjs` | 285 | `36153662676b` | 76–117（时间模型、`dayHour`） |
| `cast.mjs` | 290 | `77f5e4e89009` | 导出清单、`skyAt`、`earthClock` 片段 |
| `scenes-a/b/c/d.mjs` | 138/154/209/145 | `1a7bac92e3aa` / `c009b9b37093` / `43d78e7e71a7` / `61ad0cb074b0` | 头注释、场景对象、`minute` 开头 |
| `scenes-r1/r2/r3.mjs` | 115/140/174 | `0ca22557bf33` / `2616cf27f593` / `5200dc7a7b55` | 头注释、场景对象、`opening` |
| `audio.mjs` | 229 | `9e3eeae951b5` | 与 [[fossil-record]] 相同 |
| `build/timeline.json`、`build/rec/timeline.json` | — | — | 顶层字段、segment 起止、cue 数、与脚本逐段比对 |

未读 / 无法确认：`build/*.log` 内容（`validate.log` 724 B 是否显示通过无法确认）；成片画面与声音；制作对话（无会话）；工具版本；用户验收与发布。
