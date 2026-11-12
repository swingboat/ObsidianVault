---
type: video-production-dossier
project: english-to-be-continued-opus
repository: swingboat/english-to-be-continued-opus
video_id: episodes/book-01/ep-008
title: EP.008 电脑没坏，是我们点错了地方。
captured: '2026-11-12'
source_fingerprint: ebf599038dd12d6a4db719cfe6462b66c612fba5d0309e7eef0b9d9c4f322c89
pipeline: edge-tts
duration_s: 287.9
tags:
- video-creation
- production-capture
- english-learning
- remotion
---

# EP.008 电脑没坏，是我们点错了地方。

返回 [[项目索引]]。英文片头：*The Computer Isn't Broken. / We Clicked the Wrong Thing.*

## 目标与状态

- **源材料**：HTML 项目 `ep-008.html` 和 `production/book-01/ep-008/`。`production_language` 写的是「小帅第一人称叙述」。
- **成片**：`out/ep-008.mp4`，287.9 s，约 170 MB，11-02 10:35。
- **当前版本**：edge-tts 对白版，问题与 [[book-01-ep-007|EP.007]] 相同：没有改成克隆解说版。

## 剧情

周五放学，小帅已经按时搬完桌子，并**亲自**做了两点的英语分享。广播室里，一台借来的 **laptop** 已经准备好。几个人录了一小段 **digital** recording，回放却没有声音。分身认定电脑太旧，拿出两页升级采购清单（电脑、耳机、转接头……），结果清单把屏幕挡住了。指导老师让他先把纸放下。

苏小满发现小喇叭图标上有一道斜线，原来是小帅想点旁边，却误按了这条音轨的回放静音。取消静音后，同一段录音终于播了出来（`lin-spk` 回放音色）。分身收起了第三页清单：“Our current **hardware** works for this test.” 最后几个人打开 **search engine**，为第一个采访问题找思路：“This **engine** helps us find information. Our job is to understand it.”

## 配音（相对 EP.007 的差异）

- 新增 `lin-spk`（小帅录音的回放）：Yunxi −4% +2Hz，滤镜与 `clone-spk` 相同（`highpass=f=320,lowpass=f=3800,aecho=0.8:0.35:35:0.25,volume=1.3`）。
- 共 34 句：小帅 14（`lin` 7、`lin-en` 4、`lin-inner-en` 3）、苏小满 7、分身 7（含 `clone-en` 1）、指导老师 5、回放 1。
- `durations.json` 共 62 段，合计 227.7 s。

## 场景与笑点

镜头关键帧 59 个，最大 z=2.8。前情提要使用 ep-007 的 p4、p6、p8。

| 场 | 地点 | 特效 / 笑点 | 音效 |
| --- | --- | --- | --- |
| 1 | Friday · The radio room | 印章「DID IT MYSELF」；「FRIDAY · PROMISES KEPT：13:20–13:40 move the desks / 14:00 English talk (given by me!)」；school laptop、测试音「OK ✓」；「NEXT ▶ open the recording software」 | zipper、chime、paper |
| 2 | A short test recording | `RecPanel` 录音界面；`Cursor` 点击「click?」；回放无声时「?!」，画面去饱和 | beep、cricket |
| 3 | Two pages of upgrades | 「CLONE'S UPGRADE IDEAS · 2 PAGES」；「WHAT XIAOMAN CAN SEE: What he wants to buy ✓ / What went wrong ✗」；印章「IDEAS ONLY · NOTHING BOUGHT」 | paper、rustle |
| 4 | Back to the software | 红圈、「SCREEN: VISIBLE ✓」 | paper、tick |
| 5 | The playback track | 红圈圈出「MUTED 🔇」、「IT WAS ME!」；`Balance` 天平；「THE ADVISER CHECKS」：录音还在没被删、只是这条轨道回放静音、麦克风 / 音箱 / 线都正常 | dun、bonk |
| 6 | Unmute and play again | `Unmute`、`Waves`「SAME CLIP, NOW AUDIBLE」；「No new laptop / No new cable / No re-recording」；「XIAOMAN'S NOTE · CHECK FIRST NEXT TIME」 | tap、sparkle、scribble |
| 7 | Saving the test file | `SaveDialog`；印章「PAGE 3: CANCELLED」；「HARDWARE WE ALREADY HAVE — New purchases: 0 · Spent: …」（「Spent」后的数值是动态元素，本次没有读出）；「still ours」（下周三的时段） | paper、click、ding |
| 8 | The search box | `SearchBox`「TOPIC: HELP ON YOUR FIRST DAY」；「ONE USEFUL CLUE EACH」；「prep list」与「content」两个标签对比（呼应台词「别又让准备清单比内容多」） | keys×2、flip、scribble |

- 震屏：第 2、3 场各 1 次，第 5 场 2 次。
- 配乐（注释）：录音时轻快，回放无声时切断 → 采购清单梗轻快 → 盯着屏幕找图标时静音 → 讲解时舒缓 → 同一段录音播出后轻快 → 搜索框段舒缓，最后一拍前切断。
- 本集 `sfx.py` 新增 chime、keys。

## 课后小课堂（约 52.1 s，估算，超出 45 s）

1. **laptop** n. 笔记本电脑。**英美音对比**：英 /ˈlæptɒp/、美 /ˈlæptɑːp/。— *A borrowed laptop is already waiting.*
2. **digital** adj. 数字的。— *We make a short digital recording.*
3. **hardware** n. 硬件。**英美音对比**：英 /ˈhɑːdweə/、美 /ˈhɑːrdwer/。— *Our current hardware works for this test.*
4. **engine** n. 引擎。— *This engine helps us find information.*
5. **search engine** n. phrase 搜索引擎。— *We use a search engine to find ideas for our first question.*

复习卡（来自 Episode 7）：function（*What is the function of this icon?*）、professional（*The adviser gives us professional guidance.*）。

## 迭代与验收

- 初版 11-01 07:21–07:32，推断对应会话 `7d8fc2de` 或 `0e4d0f03`。11-01 晚上做了全局更新，11-02 10:35 重新渲染。
- 技术：ffprobe 读到 287.90 s。视觉检查、用户验收没有记录。
- 证据：`data/story.json`（25.0 KB）、`src/fx.tsx`（31.6 KB）、`scripts/sfx.py`（18.5 KB）。
