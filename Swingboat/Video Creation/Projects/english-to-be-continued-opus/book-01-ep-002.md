---
type: video-production-dossier
project: english-to-be-continued-opus
repository: swingboat/english-to-be-continued-opus
video_id: episodes/book-01/ep-002
title: EP.002 三个我才够用？先删掉一个承诺。
captured: '2026-11-12'
source_fingerprint: 0143021f8810ebb8e01faf39586d2074c169970131959b68f48679efea7cd96b
pipeline: qwen3-clone
duration_s: 195.8
tags:
- video-creation
- production-capture
- english-learning
- remotion
- voice-cloning
---

# EP.002 三个我才够用？先删掉一个承诺。

返回 [[项目索引]]。英文片头：*Do I Need Three of Me? / First, Drop One Promise.*

## 目标与状态

- **源材料**：HTML 项目 `ep-002.html`，以及 `production/book-01/ep-002/` 下的 `episode.json`、`storyboard.png`。
  - `production_language` 要求「小帅（Xiaoshuai），第一人称英文解说」；
  - 原文还写着「解说中的引号为小帅转述人物原话，沿用小帅音色」。
- **成片**：`out/ep-002.mp4`，195.8 s，约 117 MB，11-02 16:33。
- **当前版本**：**第一个 Qwen3-TTS 克隆版**。叙述句用小帅音色，色牌显示「✎ Xiaoshuai · narrating」。
- **与 HTML 原文的差异**：
  - 带引号的人物台词由被引用的角色**亲口**说出，没有按 HTML 写的那样全部由小帅转述。
  - AGENTS 对这种冲突的规定是：先用 `ask_user` 确认一次组织方式，再制作。本片的做法与 AGENTS 后来写下的规则一致，但当时确认的过程在会话里，读不到。

## 剧情

苏小满撞见两个小帅，同时又在收活动确认表，于是一路跟着他们。三份邀请都在周五两点，分身算了一笔账：「两个我、三件事，还缺一个我。」小帅指出生成分身的按钮只能用一次，「手机又不是打印机」。苏小满说：“Problems **arise** when you say yes to everything.”

小帅把分身留在门外，独自走进办公室请老师改时间：搬桌子提前到 13:20，分享仍在 14:00，篮球初选这次放弃，改报月底补选。出来以后，苏小满要他们解释为什么有两个人，两人同时指着对方说：“He will.”

## 结构

```text
前情提要（3 句，lin-narr）→ 第 1 场 → 片头 → 第 2–8 场 → TBC
→ 课堂（约 40.9 s，估算；其中复习卡约 7.3 s）→ 预告 → 片尾
```

- 前情提要用了 3 张图，但这一集的 `story.json` 还没有 `prevSource` 字段（从 ep-003 开始才有），所以 `prev1..3.png` 应当是手动复制的。具体用了上一集哪几格，**没有记录**。
- 配乐（`timeline.ts` 注释）：轻快 BGM → 办公室里静音，只留心跳 → 老师做决定时转为舒缓钢琴 → 回到轻快 → 尴尬停顿时切断。

## 角色与配音

| 键 | 角色 | 来源 | 滤镜 / 说明 |
| --- | --- | --- | --- |
| `lin` / `lin-narr` / `lin-inner` | 小帅（对白 / 解说 / 心声） | 克隆 `ref: 小帅` | 心声加 `aecho=0.8:0.7:70\|140:0.28\|0.14` |
| `clone` | 分身 | 克隆 `ref: 小帅的分身` | — |
| `both` | 两人同时说 | `mix: [lin, clone]` | **本集新增**：第 2 人延后 60 ms，音量 ×0.88，总量 ×0.72 |
| `teacher` | 班主任 | 克隆 `ref: 班主任` | — |
| `su` | 苏小满 | 克隆 `ref: 苏小满` | — |
| `lesson` / `word-us` / `word-gb` | 课堂 | edge：Jenny −2%、Aria −18%、Sonia −18% | — |

- 共 36 句：解说 18、小帅 7、分身 5、苏小满 3、心声 1、老师 1、合声 1。
- `durations.json` 共 62 段，合计 133.0 s。

**Whisper 选条**（`.cache/clone/pick.json`）

- 43 个克隆片段，平均分 0.985，最低 0.727。
- 选中的 take 分布：t1 9 条、t2 11 条、t3 23 条。
- weak（低于 0.85）2 条：
  - `s1n1` 0.727，听写为 “Shao man saw both of us.”。人工改用第 1 条：`data/voice-picks.json` = `{"s1n1": 1}`。
  - `s6n1` 0.824，“one twenty” 被听写成 “120”，保留自动选择。
- 这正是 AGENTS 写下「Whisper 只是辅助，人名拼音常被听写成别的拼法」的出处。

## 场景与笑点

镜头关键帧 51 个，最大 z=2.6。

| 场 | 地点 | 关键台词 | 特效 / 笑点 | 音效 |
| --- | --- | --- | --- | --- |
| 1 | Hallway · Right after Episode 1 | No escape. | 三张 `NameCard` 名牌：小帅「Red shirt · the original · telling this story」、苏小满「Classmate · collects the activity forms」、分身「Blue shirt · born last episode · trial monitor」；`OffscreenCall`；印章「NO ESCAPE」 | steps、dun、stamp |
| 2 | Same hallway | This is **stressful**! / Two of me. Three jobs. We need one more. | `Gloom` 阴云、`Sweat` 汗滴；`Chalk` 分身逐行写出的黑板算式；三份 pending 邀请（Fri 14:00）；说 stressful 时画面去饱和 | paper、bonk、pop×2、ding |
| 3 | Hallway outside the classroom | This phone isn't a printer. / Problems **arise**… | `PhoneDisabled`（按钮只能用一次）、`Printer`、`RisingTrouble`；「Add people」与「Change the plan」两个圆圈对比 | scribble、tap、whoosh |
| 4 | Outside the classroom | It's **challenging**, but possible. / different **expectations** | `VsCard` 分屏、`Gauge` 难度表「Possible ✓」、`Bulb` 灵光、`OrderCard` 调整顺序 | paper、twinkle、stamp、ding |
| 5 | Head teacher's office | I'm under **pressure**. / Can we change the schedule? | 「KNOCK」；名牌 Head Teacher「Class adviser · hasn't seen the clone」；分身「wait outside」；`TopFX` 压暗；说 pressure 时画面降饱和、提对比 | knock、heartbeat |
| 6 | Same office | Basketball still clashed. | `Clash` 撞期、`Ball`（篮球垂头丧气）、`Notice` 月底补选公告、`UpdateCard` | bonk、pop |
| 7 | At the teacher's desk | Making a choice is a **challenge**, too. | 「Xiaoshuai's confirmed plan」表格，篮球一栏写「not this time」；`Twinkles` | scribble、twinkle、paper |
| 8 | Outside the closed office door | He will.（合声） | `PointAt` 两个大箭头互指、`SaySame`、印章「100% IN SYNC」、「Choice +1」 | steps、twinkle、whoosh |

震屏共 6 次：第 1 场 2 次，第 2、3、6、8 场各 1 次。

## 课后小课堂

单词（按剧情顺序）：

1. **stressful** /ˈstresfl/ adj. 充满压力的。— *This is stressful!*
2. **arise** /əˈraɪz/ v.（问题）出现。— *Problems arise when you say yes to everything.*
3. **challenging** /ˈtʃælɪndʒɪŋ/ adj. 有挑战性的。— *It's challenging, but possible.*
4. **expectation** /ˌekspekˈteɪʃn/ n. 期待；预期。— *We have different expectations.*
5. **pressure** /ˈpreʃə(r)/ n. 压力。— *I'm under pressure.*

复习卡：schedule（*Can we change the schedule?*）、challenge（*Making a choice is a challenge, too.*），每个词各读一句 `rv-<word>`。

## 迭代与版本

- **初版**（10-31 19:03–19:31）：`upscale.sh`、`sfx.py`、`Root.tsx`、`Ep001.tsx` 创建。21:04–21:54 生成了 `.cache/voice-samples/` 里的 40 条 edge-tts 试音，用来为小帅、分身、苏小满选声线。初版当时用的是什么配音，没有快照留存。
- **改克隆版**（11-02 16:02–16:33，推断对应会话 `e67991ad`「Ep-002 voice cloning video」，正文读不到）：
  - 新建 `lines.py`、`clone.py`、`check.py`，改写 `tts.py`，`story.json` 改成解说体，加入 `voice-picks.json`。
  - 新增的这些脚本成为后续各集的模板，AGENTS 也写明「模板早于 ep-002 时，从 ep-002 补拷音频脚本」。
- **遗留文件**：`src/Ep001.tsx` 还在目录里，但 `Root.tsx` 引用的是 `Episode`。

## 验收与证据

- 技术：ffprobe 读到 195.80 s。Whisper 选条结果见上。响度没有测量。
- 视觉检查、用户验收：没有记录。
- 关键文件：`data/story.json`（24.6 KB）、`data/voice-picks.json`、`scripts/{lines,clone,check,tts}.py`、`src/fx.tsx`（32.7 KB）、`src/timeline.ts`、`.cache/clone/pick.json`。
