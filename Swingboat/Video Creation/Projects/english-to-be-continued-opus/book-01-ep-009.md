---
type: video-production-dossier
project: english-to-be-continued-opus
repository: swingboat/english-to-be-continued-opus
video_id: episodes/book-01/ep-009
title: EP.009 搜了三小时，收获二十个聊天群。
captured: '2026-11-12'
source_fingerprint: 534e66497ab51fb2ae524fc68a65b09451bc44c6a0eed4ad826e4f6ace72b95a
pipeline: edge-tts
duration_s: 311.8
tags:
- video-creation
- production-capture
- english-learning
- remotion
---

# EP.009 搜了三小时，收获二十个聊天群。

返回 [[项目索引]]。英文片头：*Three Hours of Searching. / Twenty Group Chats Later.*

## 目标与状态

- **源材料**：HTML 项目 `ep-009.html` 和 `production/book-01/ep-009/`。`production_language` 写的是「小帅第一人称叙述」。
- **成片**：`out/ep-009.mp4`，311.8 s，约 177 MB，11-02 00:04。
- **当前版本**：edge-tts 对白版，问题与 [[book-01-ep-007|EP.007]] 相同：没有改成克隆解说版。

## 剧情

周六上午九点，三个人在小帅家，用老师登记借出的笔记本电脑找采访素材。妈妈也在家。三人约好每人找一条能用的线索，打开数字笔记，用搜索引擎找思路。

结果一个链接接着一个链接，很快就被带进了 **social media**：小帅开始 **surf** 搞笑视频（第一天上学的猫），苏小满忙着回一条把问题说反了的评论，分身收藏了一个又一个群入口。中午妈妈端菜进来问：「查了三小时，查到的东西能下饭吗？」分身收藏了 20 个群聊入口；苏小满只抄了一句热评，还不知道是谁的经历；小帅记住了猫的每个表情，却一条素材也没记下。

三人重新聚焦：每人带回一条有来源的线索。“Radio is part of the **media**, too.” 要知道是谁的经历、对方愿不愿意讲。小帅可以讲自己的第一天；苏小满借手机打给高马尾同学，对方愿意讲她第一天走错楼、被人带去图书馆的经历；分身在学校手册里找到图书馆的位置，打算周一沿路核对。他们先存下一段 30 秒的口述草稿，这时妈妈喊吃饭，手机又亮起了新消息。

## 配音（相对 EP.007 的差异）

- 新增 `mum-vo`（妈妈在厨房喊）：Xiaoxiao −2% −2Hz + `lowpass=f=3200,aecho=0.8:0.4:40:0.2`。
- 新增 `pony-phone`（高马尾同学在电话里）：Xiaoyi +4% +4Hz + `highpass=f=400,lowpass=f=3400,volume=1.4`。
- 共 34 句：小帅 14（`lin` 8、`lin-en` 3、`lin-inner-en` 3）、苏小满 9（含 `su-en` 1）、分身 7、妈妈 3（含画外 1）、电话 1。
- `durations.json` 共 61 段，合计 256.2 s。

## 场景与笑点

镜头关键帧 56 个，最大 z=2.9（10 集中最大的特写倍率）。前情提要使用 ep-008 的 p3、p5、p8。

| 场 | 地点 | 特效 / 笑点 | 音效 |
| --- | --- | --- | --- |
| 1 | Saturday · 9 a.m. · Xiaoshuai's home | 「WEEKEND LOAN · SIGNED OUT」；「Parents know · Mum is home」；「Mic + speaker: stayed at school」；`Clock`；`LeadsNote`「1 useful lead each」；「Clone's promise:」 | clink、flip、boing |
| 2 | Just starting to browse | `TabStack` 标签页越叠越多、`RabbitHole`（越点越深的兔子洞） | click×3 |
| 3 | Drifting off | 「MEOW~ 🐱」、「funny videos」；`ReplyBubble`；`InviteRain` 群邀请像雨一样落下；`ChatMeter` 聊天值超过搜索值；画面饱和度升高 | giggle、keys、notify |
| 4 | Noon · Mum brings lunch | `TimeWarp` 时钟快进到中午；`InviteCounter` 计数到 20；`Scoreboard`（三人成果计分板） | rewind、clink |
| 5 | Back on track | 印章「20 TABS → CLOSED」、「focus mode」；`MediaFamily`（广播也是媒体）；「BEFORE IT GOES ON AIR：Whose experience is it? / Do they want to share it? / Copy a popular comment ✗」 | thud、click、powerdown |
| 6 | A quick phone call | `CallScreen`；印章「SHE SAID YES ✓」；「LEAD ① · XIAOSHUAI'S OWN FIRST DAY — Scared of saying it wrong… she waited till I finished. source: me · it really happened ✓」 | scribble、ring |
| 7 | Three leads come together | `LeadsNote` 三条线索都注明了来源、红圈；印章「NOT FINAL YET」 | paper、marker、pop、stamp |
| 8 | A 30-second spoken draft | `ProjectFolder`、`Waves`「REC · built-in mic」；「New message · just now」，画面压暗 | beep、click、sparkle、notify |

- 震屏：第 4 场 2 次，第 5、8 场各 1 次。
- 配乐（注释）：浏览和走神时轻快 → 时钟跳到中午时切断 → 空白笔记页时静音 → 重新聚焦和打电话时舒缓 → 三条线索汇齐时轻快 → 新通知响起时切断。
- 本集 `sfx.py` 新增 `meow`（猫叫：基频沿正弦曲线滑动，加 5 次谐波，再做 400–4200 Hz 带通）。

## 课后小课堂（约 52.0 s，估算，超出 45 s）

1. **surf the Internet** phrase 上网浏览。— *We surf the Internet for interview ideas.*
2. **social media** n. phrase 社交媒体。— *One link leads to social media.*
3. **surf** v. 上网浏览。**英美音对比**：英 /sɜːf/、美 /sɜːrf/。— *I start to surf for funny videos.*
4. **chat** v. 聊天。— *Soon we chat more than we search.*
5. **media** n. 媒体。— *Radio is part of the media, too.*

- 复习卡（来自 Episode 8）：search engine（*We use a search engine to find interview ideas.*）、digital（*We open a digital notebook for useful leads.*）。
- 第 1 场的 `s1l3` 用 `kw: "digital|search engine"` 同时高亮两个复习词。

## 迭代与验收

- 初版 11-01 07:46–07:49，推断对应会话 `7d8fc2de` 或 `0e4d0f03`。11-01 晚上做了全局更新，11-02 00:04 重新渲染。
- 技术：ffprobe 读到 311.80 s。视觉检查、用户验收没有记录。
- 证据：`data/story.json`（25.6 KB）、`src/fx.tsx`（30.6 KB）、`scripts/sfx.py`（19.1 KB）。
