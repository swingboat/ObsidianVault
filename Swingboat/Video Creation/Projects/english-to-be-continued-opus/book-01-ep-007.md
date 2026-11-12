---
type: video-production-dossier
project: english-to-be-continued-opus
repository: swingboat/english-to-be-continued-opus
video_id: episodes/book-01/ep-007
title: EP.007 负责人的名字，不能写两个小帅。
captured: '2026-11-12'
source_fingerprint: 084c363c2ae2013c25d62c27a46a9ff5305f3e2f638e092f434c0c4a08150453
pipeline: edge-tts
duration_s: 318.6
tags:
- video-creation
- production-capture
- english-learning
- remotion
---

# EP.007 负责人的名字，不能写两个小帅。

返回 [[项目索引]]。英文片头：*One Job, One Name. / Not Two Xiaoshuais.*

## 目标与状态

- **源材料**：HTML 项目 `ep-007.html` 和 `production/book-01/ep-007/`。`production_language` 写的是「小帅第一人称叙述，朗读英文解说栏」。
- **成片**：`out/ep-007.mp4`，318.6 s（10 集中最长），约 197 MB，11-02 10:31。
- **当前版本**：**edge-tts 对白版**。前情提要、片头和预告用 `narrator-en`（Ava）做第三人称旁白，正片全部是角色对白，没有小帅解说。
- **与现行规则的冲突**：AGENTS 规定 ep-002 起用 `voices/book-01` 的参考音色克隆，HTML 又要求第一人称。广播指导老师在 cast.json 里已有参考音色，但这里仍用 `en-GB-LibbyNeural`。本集**尚未**像 ep-002～006 那样改版。

## 剧情

第二天放学，高马尾同学来广播室帮忙整理稿子，看到两张一样的脸。分工表上有两行「小帅」，她问哪个是哪个。老师先把分工说清楚：小帅 “I will **be responsible for** the schedule.”，苏小满主编，分身在指导下协助声音检查。

试音时，分身对着麦克风说 “Hello?”，却只有自己听得见：“What is the **function** of this button?” 分身问能不能让「另一个小帅」替他签名，苏小满说「同一个名字写两次，音箱也不会自己响」。小帅说 “We need to **seek** help.”，老师请来广播指导老师（“someone with **professional** experience”）。指导老师一眼发现音频线没插牢，插紧后音箱里传出 “Hello!”。之后三人各自签名，分身写的是「小帅（分身）— 声音协助」。小帅总结：“Being **responsible** also means knowing when to ask.” 试播时段定在下周三 16:40–16:50。

## 角色与配音（edge-tts，ep-007～010 通用）

| 键 | 角色 | 声音 | rate / pitch | 滤镜 |
| --- | --- | --- | --- | --- |
| `lin` / `lin-en` | 小帅：普通对白 / 重点英文句 | zh-CN-YunxiNeural | +6% +2Hz / −4% +2Hz | — |
| `lin-inner-en` | 小帅心声 | Yunxi | −4% +2Hz | `aecho=0.8:0.55:70\|140:0.28\|0.14` |
| `clone` / `clone-en` | 分身 | Yunxi | +12% +6Hz / +2% +6Hz | — |
| `su` / `su-en` | 苏小满 | zh-TW-HsiaoChenNeural | +4% +6Hz / +0% +6Hz | — |
| `buzz` / `pony` | 寸头 / 高马尾 | zh-TW-YunJheNeural / zh-CN-XiaoyiNeural | +4% +4Hz | — |
| `mum` | 妈妈 | zh-CN-XiaoxiaoNeural | −2% −2Hz | — |
| `teacher` | 班主任 | en-US-ChristopherNeural | +0% | — |
| `adviser` | 广播指导老师（**本集新增**） | en-GB-LibbyNeural | −2% | — |
| `clone-spk` | 分身声音从音箱里传出 | Yunxi +12% +6Hz | | `highpass=f=320,lowpass=f=3800,aecho=0.8:0.35:35:0.25,volume=1.3` |
| `narrator-en` | 旁白 | en-US-AvaNeural | −4% | — |
| `lesson` / `word-us` / `word-gb` | 课堂 | Jenny −2% / Aria −18% / Sonia −18% | | — |

- 共 36 句：分身 9（`clone` 7、`clone-en` 1、`clone-spk` 1）、小帅 12（`lin` 6、`lin-en` 6）、苏小满 7、班主任 3、指导老师 3、高马尾 2。
- `durations.json` 共 63 段，合计 258.7 s。
- 这一集的 `tts.py` 是 edge 版：裁剪阈值 −48 dB，**没有逐句 loudnorm**，也没有 Whisper 校对。

## 场景与笑点

镜头关键帧 61 个，最大 z=2.6。前情提要使用 ep-006 的 p4、p6、p8。

| 场 | 地点 | 特效 / 笑点 | 音效 |
| --- | --- | --- | --- |
| 1 | The radio room | 名牌：高马尾「Classmate · volunteer helper today」；`TwinScan` 双人扫描、「WHICH ONE?!」；「ABOUT THE CLONE」三条说明（Made by a phone copy / Teachers + parents know / After school only, with permission）；`RolesForm` 上两行同样的名字 | door、scan、flip、dun |
| 2 | Before the test | `Role` 卡「TODAY'S ROLES (said out loud)」；「Final form: who is who + what they do + own signature」 | twinkle、pop |
| 3 | Sound test | 「TEST MODE · in this room only」；「Hello?」；`Silence`；「FUNCTION = ?」；「CHECKED SO FAR」：电源灯亮、按钮查说明、音箱无声；冷场时画面去饱和 | click、cricket、flip |
| 4 | The roles form | 「signed: “the other Xiaoshuai”?」；「Same name ≠ same person ≠ same work」；印章「NOT DONE (YET)」 | tap、scribble |
| 5 | Calling the adviser | `PhoneCall`、`HelpList`「someone who knows this」 | ring、scribble |
| 6 | The radio adviser arrives | 名牌：Radio Adviser「Knows the equipment · knows about the clone」；红圈标出「LOOSE CABLE!」；「CLICK!」；`Waves` 声波、「IT WORKS! 🔊」；「PREP NOTES · CHECK ORDER：1 Power light / 2 Buttons (guide) / 3 Speaker cable plugged in」 | steps、plug、sparkle |
| 7 | Signing the roles | `RolesForm` 三人各自签名、印章「CLEAR ✓」；「Responsible = do your part + know when to ask」 | scribble、sign×3、stamp |
| 8 | Trial slot confirmed | `Digits`「16:40–16:50」「10 MIN · OURS」；`Status`「STILL TO DO」：初稿、采访素材、下次学录音电脑，「not on air yet」；「Signed by Xiaoshuai (clone)」 | marker、paper |

- 震屏在第 1、4、6、7 场。
- 配乐（注释）：轻快 BGM 一直到音箱无声 → 无声试音和「另一个小帅」签名时完全不放音乐 → 求助时舒缓 → 线插好后轻快 → 签名时舒缓 → 日历时轻快，在分身最后一句前切断。
- 本集 `sfx.py` 新增 scan、ring、plug、sign。

## 课后小课堂（约 51.6 s，估算，超出 45 s）

1. **be responsible for** phrase 负责。发音读 “be responsible for”，高亮 “responsible for”。— *I will be responsible for the schedule.*
2. **function** n. 功能。— *What is the function of this button?*
3. **seek** v. 寻求。— *We need to seek help.*
4. **professional** adj. 专业的。— *Now we have someone with professional experience.*
5. **responsible** adj. 负责任的。**英美音对比**：英 /rɪˈspɒnsəbl/、美 /rɪˈspɑːnsəbl/。— *Being responsible also means knowing when to ask.*

复习卡（`reviewFrom: 6`，旁白 “And two words from Episode 6!”）：voluntary（*Taking part is voluntary.*）、organise（*Xiaoman edits. I organise. My clone checks sound with guidance.*）。

## 迭代与验收

- 初版 11-01 06:56–07:08（`Root.tsx`、`tts.py`、`sfx.py`、`story.json`、`durations.json`），推断对应会话 `7d8fc2de`「Producing anime English episodes 006–007」，正文读不到。
- 11-01 22:21–23:58 做了全局品牌与安全区更新，11-02 10:31 重新渲染。
- 技术：ffprobe 读到 318.60 s。视觉检查、用户验收没有记录。
- 证据：`data/story.json`（26.0 KB）、`scripts/tts.py`（edge 版，3806 B）、`src/fx.tsx`（29.3 KB）、`src/timeline.ts`。`.cache/` 里只有 `frames.py`（复刻时间轴、打印各节拍帧号的辅助脚本）和 `tts/`。
