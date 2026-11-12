---
type: video-production-dossier
project: english-to-be-continued-opus
repository: swingboat/english-to-be-continued-opus
video_id: episodes/book-01/ep-003
title: EP.003 同桌认出我，靠的是一句大实话。
captured: '2026-11-12'
source_fingerprint: 249773240be35b3e3662ce860fa44e61b2bd557d02844c51f7ae897c0e5130fc
pipeline: qwen3-clone
duration_s: 202.2
tags:
- video-creation
- production-capture
- english-learning
- remotion
- voice-cloning
---

# EP.003 同桌认出我，靠的是一句大实话。

返回 [[项目索引]]。英文片头：*My Deskmate Figured Me Out / with One Honest Sentence.*

## 目标与状态

- **源材料**：HTML 项目 `ep-003.html` 和 `production/book-01/ep-003/`。`production_language` 要求小帅第一人称英文解说，「人物引语仍由小帅转述」。
- **成片**：`out/ep-003.mp4`，202.2 s，约 120 MB，11-02 17:15。
- **当前版本**：克隆解说版。正片 25 句，其中 20 句是小帅解说。
- **本集最有价值的证据**：`.cache/story.before-voice-update.json` 保存了改版前的剧本（文件时间 11-02 16:58），可以直接看出旧版对白体是怎样改成解说体的。

## 剧情

苏小满收下了活动表，但还在等一个解释。分身准备了一篇完美演说，把这件事说成「勇敢的课堂实验」。小帅说了实话：“**Actually**, it was not that brave. … **To be frank**, I was afraid of making mistakes.” 苏小满回答：“That's a **frank** answer.” 两人长得一样，目标却不同（**differ**）：分身想要掌声，小帅想不再躲着。

苏小满也提出了她的广播节目想法，这一次两人认真听完。她答应明天陪他们去找老师，小帅承诺 **in person** 向老师和父母解释。小帅先对着一小群观众，用发抖的声音做了自我介绍。分身也想介绍自己，小帅叮嘱：“Just don't **volunteer** for another job!”

## 改版对照（旧快照 → 当前）

| 项目 | 改版前 | 改版后 |
| --- | --- | --- |
| 配音 | 全部 edge-tts：小帅 / 分身用 Yunxi，苏小满用 HsiaoChen，旁白用 `narrator-en`（Ava） | 角色用参考音色克隆（`ref`），课堂仍用 edge |
| 正片组织 | 31 句对白，例如苏小满问 “I've got your form. Now, who's explaining first?”，两人合声 “He is.”，苏小满接 “You even pass the buck in sync.” | 25 句，其中 20 句是小帅解说，例如 “Xiaoman has our form, but she still wants an answer.” “My clone and I point at each other.” “That explains nothing.” |
| 旁白 | “Last time: two Xiaoshuais… The real Xiaoshuai walked into the office by himself” | “Last time: two of me, three jobs. So I walked into the office by myself” |
| 声音键 | `lin-en`、`lin-inner-en`、`clone-en`、`su-en`、`narrator-en`、`both` | `lin-narr`、`lin-shaky`、`lin-inner` 等；去掉了 `-en` 后缀，也去掉了 `both` |

共有 53 个节拍 ID 新增、删除或改了文本：大部分对白被改写成解说，小帅的分镜动作则交给画面特效表现。

## 角色与配音

| 键 | 来源 | 说明 |
| --- | --- | --- |
| `lin` / `lin-narr` / `lin-inner` | 克隆 小帅 | 心声的回声同 ep-002 |
| `lin-shaky` | 克隆 小帅 + `vibrato=f=5.5:d=0.06` | **本集新增**：第 7 场发抖的自我介绍 “Hi, I'm Xiaoshuai. I like basketball. I get nervous, but I want to try.” |
| `clone` / `su` | 克隆 | — |
| 课堂 | edge：Jenny −2%、Aria/Sonia −18% | — |

- Whisper 选条：31 个片段，平均分 0.991，最低 0.857，**没有 weak**，也没有人工换条。选中的 take 分布：t1 12、t2 9、t3 10。
- `durations.json` 共 52 段，合计 138.0 s。

## 场景与笑点

镜头关键帧 47 个，最大 z=2.5。前情提要使用 ep-002 的 p5、p7、p8。

| 场 | 地点 | 特效 / 笑点 | 音效 |
| --- | --- | --- | --- |
| 1 | Hallway outside the office | 名牌：苏小满「Deskmate · holds our form」、小帅「Red shirt · the original · telling this story」、分身「Blue shirt · the copy · loves a stage」；印章「BLAME: IN SYNC」「MIRROR MODE: ON」；「EXPLAINED: 0%」 | bell、rustle、stamp、dun |
| 2 | Classroom door | `Keynote`（产品发布会式的演讲幻灯片）被「FACT CHECK」打断；「Brave? very」被划掉；「The real plan was just… Someone says hello for me」 | paper、sparkle、scratch |
| 3 | Close-up | `HonestyMeter` 诚实度表、`PhoneOnce`（只能用一次）、印章「FRANK ✓」；`TopFX` 让走廊在坦白前安静下来 | heartbeat、tap、twinkle |
| 4 | Side by side | `VsCard`：「Face scan 100% match ✓」对「Goals 不同」；分身一侧 `ApplauseSign` 掌声牌，换来 `Chirps` 虫鸣冷场（画面降饱和）；`Door`「Entered the office himself」 | cough、cricket、stamp、ding |
| 5 | Empty classroom · Window desk | `OnAir` 灯，被 `CutOff` 打断的旧回忆，「LISTENING」，「Clone mic: OFF」 | flip、pop、zipper |
| 6 | By the desk | 印章「IN PERSON」；清单「Tomorrow: See our teacher / Tell my parents」 | stamp、zipper |
| 7 | A tiny audience | `IntroChips` 自我介绍词条、`ConfidenceBars` 信心条、「clap clap」；听众从「whole class」缩小到眼前几个人；画面转暖 | breath、clap、sparkle |
| 8 | All three | 「NO MORE JOBS」「hehe」「Tomorrow: explaining」 | pop、bonk、giggle |

- 震屏在第 1、2、4、6、8 场。
- 配乐（注释）：轻快 BGM → 坦白前安静 → 从 “frank” 起转舒缓 → 分身掌声梗时轻快 → 虫鸣处静音 → 承诺时舒缓 → 发抖的自我介绍时静音 → 结尾轻快，最后一句前切断。

## 课后小课堂（约 47.2 s，估算，略超 45 s）

1. **actually** /ˈæktʃuəli/ adv. 实际上。— *Actually, it was not that brave.*
2. **to be frank** phrase 坦白说。— *To be frank, I was afraid of making mistakes.*
3. **frank** /fræŋk/ adj. 坦率的。— *That's a frank answer.*
4. **differ** v. 不同。**英美音对比**：英 /ˈdɪfə/、美 /ˈdɪfər/。— *We look the same, but our goals differ.*
5. **in person** phrase 亲自。— *I promise to explain everything to the teacher and my parents in person.*

复习卡：goal（*My goal is to stop hiding.*）、confidence（*My voice shakes. My confidence grows.*）。

## 迭代与验收

- 初版 10-31 22:13–22:26（`Root.tsx`、`sfx.py`、`upscale.sh`），当时是 edge 对白版。ep-002 的 `upscale.sh` 已经从 `story.json` 读取 `storyboard` 和 `panelRects`；这一集又加了一段：读取 `prevSource`，把上一集的超分图复制成 `prevN.png`，并加上 `chmod +x`。后续各集都沿用这个版本。
- 改版 11-02 16:58–17:15，推断对应会话 `e0e50bbd`「ep-003 video update based on HTML」，正文读不到。
- 技术：ffprobe 读到 202.20 s。视觉检查、用户验收没有记录。
- 证据：`data/story.json`（23.6 KB）、`.cache/story.before-voice-update.json`、`.cache/clone/pick.json`、`src/fx.tsx`（29.2 KB）。
