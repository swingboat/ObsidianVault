---
type: video-production-dossier
project: english-to-be-continued-opus
repository: swingboat/english-to-be-continued-opus
video_id: episodes/book-01/ep-001
title: EP.001 替我说句 Hello，你怎么当上班长了？
captured: '2026-11-12'
source_fingerprint: b98fbcb1355facf40d66844eff813d7871060685a546259c9bef0cfc31267e54
pipeline: edge-tts
duration_s: 196.2
tags:
- video-creation
- production-capture
- english-learning
- remotion
---

# EP.001 替我说句 Hello，你怎么当上班长了？

返回 [[项目索引]]。共用管线、算法和工具版本见索引中的「共用制作管线」。

## 目标与状态

- **源材料**：HTML 项目的 `episodes/book-01/ep-001.html`（只读，本次没有打开正文）。季名「我被自己的分身卷了」。这一集的 `story.json` 还没有 `titleLines`、`next`、`storyboard`、`panelRects` 字段：分格坐标直接写死在 `scripts/upscale.sh` 里，下集预告也写在旁白里。
- **成片**：`out/ep-001.mp4`。196.2 s，1080×1920，30 fps，H.264 + AAC 48 kHz，约 100 MB。文件修改时间 11-01 23:36（UTC+8）。
- **当前版本**：edge-tts 对白版。剧本 10-31 22:00 定稿；11-01 下午到晚上，品牌（台标、片头 logo、片尾账号卡）和抖音安全区经过全局修改，之后重新渲染。
- **未完成 / 不一致**：HTML 项目 `production/book-01/ep-001/episode.json` 的 `production_language` 写的是「小帅第一人称叙述」，但本片仍是角色对白 + 英文旁白，**没有**按 ep-002～006 的方式改成克隆解说版。

## 剧情（按 story.json）

小帅轮到用英语做自我介绍时脑子「下线」，就用手机生成了一个分身，让它替自己说句 Hello。没想到分身自告奋勇当上了试任班长，还领回三份全在周五 14:00 的活动邀请。小帅决定这次自己去找老师改时间，却在走廊拐角被苏小满撞见「两个小帅」。

## 结构

```text
冷开场 第 1 场（结果：一份自我介绍换来三项新任务）
→ 片头 n-title（en-US-AvaNeural）
→ Rewind「Three minutes earlier.」（drama-en：en-US-GuyNeural，加回声）
→ 第 2–8 场
→ TBC
→ 课后小课堂（约 33.8 s，估算）
→ 预告（Ava）+ 片尾 “Follow the story. Learn the words.”
```

- 第 1 集的写法是先给结果再回溯，所以多了一个 `rewind` 段。片头使用第 1 场画面放大推进：`Shot p1`，`z` 从 1.9 起随帧数增大。
- 配乐只有两段：从开头到第 8 场 `s8l1` 为止用 `bgm`，在苏小满开口那一刻切断；课堂到片尾再用 `bgm`。这一集还没有 `bgm-calm`。

## 角色与配音（edge-tts）

| 键 | 角色 | 声音 | rate / pitch | 滤镜 |
| --- | --- | --- | --- | --- |
| `lin` | 小帅 | zh-CN-YunxiNeural | +6% / +2Hz | — |
| `lin-inner` | 小帅心声 | zh-CN-YunxiNeural | +0% / +2Hz | `aecho=0.8:0.55:70\|140:0.28\|0.14` |
| `clone` / `clone-en` | 分身 | zh-CN-YunxiNeural | +12% / +6Hz；重点句 +2% / +6Hz | — |
| `teacher` / `teacher-vo` | 老师 | en-US-ChristopherNeural | +0% | 画外音：`lowpass=f=2600,aecho=0.8:0.5:35:0.22,volume=0.9` |
| `su` | 苏小满 | zh-TW-HsiaoChenNeural | +4% / +6Hz | — |
| `phone` | 手机提示音 | en-US-MichelleNeural | −8% | `vibrato=f=36:d=0.22,highpass=f=280,lowpass=f=3800`（机械感） |
| `narrator-en` / `drama-en` | 旁白 | en-US-AvaNeural −4% / en-US-GuyNeural −12% −4Hz | | Guy 加 `aecho=0.8:0.6:90:0.3` |
| `lesson` / `word-us` / `word-gb` | 课堂 / 美音 / 英音 | Jenny −2% / Aria −18% / Sonia −18% | | — |

- `voices` 里还留着中文旁白 `narrator`（zh-CN-XiaoxiaoNeural）和 `drama`（zh-CN-YunyangNeural），**当前剧本没有用到**，应是早期中文旁白方案留下的。
- `pronounce` 把 Xiaoshuai 读作 “Shiao Shwai”，Xiaoman 读作 “Shiao Man”。
- 共 30 句台词：小帅 8、心声 6、分身 9（`clone-en` 7 + `clone` 2）、老师 5、手机 1、苏小满 1。

## 场景、镜头与笑点

镜头关键帧共 41 个，最大 z=2.8。

| 场 | 地点字幕 | 内容 | 画面特效 / 笑点（`fx.tsx`） | 音效 |
| --- | --- | --- | --- | --- |
| 1 | Hallway · After school | 「一份自我介绍，三项新任务」 | 标签卡 + `Seal` | paper、twinkle |
| 2 | Same hallway · 3 minutes earlier | 点名、脑袋下线、点「生成分身」 | `PhoneMock` 手机 UI，「Brain: OFFLINE」 | powerdown、notify、tap |
| 3 | Outside the classroom | 分身生成 “Have **confidence** in me.” | `Particles` 粒子；`shotFilter` 让画面在 40 帧内从模糊、去色、色相偏移恢复正常（生成过程） | sparkle、notify、twinkle |
| 4 | Grade 10 classroom | “I **volunteer**!” / 两个 **goal** 对比 | `SpeedLines`、`Burst`「！ ？！」、`VsCard` 分屏（皇冠对房子） | whoosh、bonk、stamp |
| 5 | Same classroom | 试任一周，**challenge** | `Stamp`「TRIAL: 1 WEEK」「INVITES: NOT CONFIRMED」 | clap、stamp |
| 6 | Hallway · After class | “Here is your **schedule**.” 三项撞期 | `ScheduleTable`，周五 14:00 × 3 | paper、boing |
| 7 | Same hallway | 「先别勾！这次我自己说」 | 「Don't check!」「Courage +1」 | scratch、steps |
| 8 | Hallway corner | 「怎么有两个小帅？」 | `RecallUI`（撤回自我介绍）、「Xiaoshuai ×2 ?!」；`shotFilter` 在 `s8-dun` 后变化 | scratch、dun |

震屏在 `segments.tsx` 的第 1、6、8 场。

## 课后小课堂

单词按剧情顺序（音标、词性、中文释义、英文释义、剧中例句）：

1. **confidence** /ˈkɒnfɪdəns/ n. 信心；自信。Confidence is the feeling that you can do things well. — *Have confidence in me.*
2. **volunteer** /ˌvɒlənˈtɪə(r)/ v. 自愿做；志愿者。To volunteer is to offer to do something without being asked. — *I volunteer!*
3. **goal** /ɡəʊl/ n. 目标。A goal is something you want to achieve. — *My goal is to be class monitor.*
4. **challenge** /ˈtʃælɪndʒ/ n. 挑战。A challenge is something difficult that tests your ability. — *I accept the challenge.*
5. **schedule** 英 /ˈʃedjuːl/、美 /ˈskedʒuːl/ n. 日程表。**英美音对比**：`tts.py` 里硬编码生成 `w-schedule-gb`。— *Here is your schedule.*

- 单词数据里还留有测验字段 `question`、`blank`（例如「Have ___ in me.」），但 `src/` 中没有引用，是早期设计的残留。
- 没有复习卡（第一集）。

## 迭代与版本

- 10-31 17:49 写 `sfx.py`，17:58 搭好 Remotion 骨架，18:45 写 `upscale.sh`，20:40 写 `tts.py`，22:00 定稿 `story.json`。
- 11-01 18:43 改 `Ep001.tsx` 和 `timeline.ts`；21:41 改 `fx.tsx`；22:00 改 `ui.tsx` 和 `preview.sh`；23:14 改 `segments.tsx`；23:36 渲染成片。
- 这一集的合成入口是 `src/Ep001.tsx`，其余各集是 `Episode.tsx`。它的 `tts.py` 在失败时会打印 `FAILED JOB`，其他集没有这一行。
- 制作会话：**没有发现**（宿主列出的会话都晚于这些文件时间）。

## 验收与证据

- 技术：ffprobe 确认 196.20 s、1080×1920、H.264、AAC 48 kHz。响度没有测量。
- 视觉检查、用户验收：没有记录。
- 关键文件：`data/story.json`（15.7 KB）、`data/durations.json`（52 段，合计 139.1 s）、`src/fx.tsx`（18.4 KB）、`src/segments.tsx`、`src/timeline.ts`、`scripts/tts.py`、`scripts/upscale.sh`（硬编码的 8 个分格矩形 `3:3:474:433 … 449:1242:435:528`）。
- 没有读取：HTML 正文、`public/` 素材、`.cache/`（`tl.mjs`、`tts/`）。
