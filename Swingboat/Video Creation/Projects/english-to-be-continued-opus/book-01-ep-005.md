---
type: video-production-dossier
project: english-to-be-continued-opus
repository: swingboat/english-to-be-continued-opus
video_id: episodes/book-01/ep-005
title: EP.005 他替我答应的晨跑，我来喊暂停。
captured: '2026-11-12'
source_fingerprint: 12b1ed0d7ae989a447077fc0ab0067bc4b00f04e2657216fb86f195ac2254f44
pipeline: qwen3-clone
duration_s: 302.2
tags:
- video-creation
- production-capture
- english-learning
- remotion
- voice-cloning
---

# EP.005 他替我答应的晨跑，我来喊暂停。

返回 [[项目索引]]。英文片头：*He Said Yes to a Morning Run. / I Call Time‑Out.*

## 目标与状态

- **源材料**：HTML 项目 `ep-005.html` 和 `production/book-01/ep-005/`。`production_language` 要求小帅第一人称英文解说，并「使用小帅已选定的固定参考音色」。
- **成片**：`out/ep-005.mp4`，302.2 s（克隆版中最长），约 200 MB，11-05 10:09。
- **当前版本**：克隆解说版。**首次**加入两位同学：寸头同学、高马尾同学。

## 剧情

分身承诺给这一天一个 **dynamic** 的开场，它理想的 **lifestyle** 连休息都有倒计时。小帅带着秒表到跑道边，先问大家想怎么运动：苏小满想先走走，寸头同学说慢一点可以，老师在旁边看着。半圈下来，小帅开始 **drag** 着脚步走，高马尾同学发现「说好的慢」和她想的「慢」不是一回事。

大家在长椅上坐下，十秒后秒表就响了。苏小满说她在 **leisure** time 也跑步，只是不想一早就比赛，并问：“Can we change the schedule?” 小帅意识到：“I **tend to** say yes before I ask enough questions.” 他关掉秒表重新问一遍，新方案是有人慢跑、有人步行或拉伸，小帅 **volunteer** 带慢组。课间，苏小满请他午休留出十分钟，听她讲完广播提案，这一次秒表留在了口袋里。

## 角色与配音

| 键 | 来源 | 说明 |
| --- | --- | --- |
| `lin` / `lin-narr` / `lin-inner` | 克隆 小帅 | — |
| `clone` | 克隆 小帅的分身 | 只在第 1 场（家中）出现，之后留在家里 |
| `su` / `teacher` | 克隆 | 苏小满 10 句，是她台词最多的一集 |
| `buzz` / `buzz-vo` | 克隆 寸头同学；画外音加 `lowpass=f=3600,aecho=0.8:0.4:30:0.15` | **首次**出场 |
| `pony` | 克隆 高马尾同学 | **首次**出场 |

- 共 49 句：解说 22、苏小满 10、小帅 5、老师 4、寸头 4、分身 2、高马尾 1、心声 1。
- `durations.json` 共 76 段，合计 242.6 s。
- **Whisper 选条**：55 个片段，平均分 0.985，最低 0.786；选中 take 分布 t1 15、t2 19、t3 21。weak 2 条，都没有人工换条：
  - `s4n3` 0.80：“Ten seconds” 被听写成 “10 seconds”。数字和拼写差异会拉低分数，但读音没有错。
  - `s8l3` 0.786：“I don't want a ‘time's up’…” 被听写为 “I don't want to times up…”。

## 场景与笑点

镜头关键帧 67 个，是 10 集中最多的，最大 z=2.4。前情提要使用 ep-004 的 p3、p4、p8。

| 场 | 地点 | 特效 / 笑点 | 音效 |
| --- | --- | --- | --- |
| 1 | Next morning · Home | 名牌：小帅、分身「Blue shirt · the copy · plans everything」；「ENERGY!」；`PlanNote` 计划单上的「REST: 10 SEC」；「Dream lifestyle: everything on a timer」；分工卡「Xiaoshuai → school / Clone → stays home」 | beep、tick、tap、click |
| 2 | Beside the track | 名牌：苏小满「wants to walk first」、寸头「okay with slow」、班主任「Right by the track · stop anytime」；「Step 1: ASK — Before we run」清单 | birds、beep |
| 3 | Halfway through | `HeavyFeet`、印章「GENTLE START? ✗」；名牌：高马尾「her own idea of “slow”」；`SlowMeter`「Pace: “slow” …?」；「BENCH →」；拖步时画面降饱和 | steps、breath |
| 4 | The bench | 「SIT」；`Digits` 显示「REST TIMER (clone's plan)」；「BEEP!」；「Breath caught: not yet」 | tap、beep |
| 5 | Xiaoman asks | 印章「REST = RACE?」；Xiaoman「Runs in her leisure time」；`ScheduleEdit`；老师「A schedule is there to help people」 | — |
| 6 | Talking with the teacher | 「❚❚ OFF」；「Before: YES → questions? / Now: questions → then decide」；新方案卡「Walk a lap + stretch / Run, even slower / Rest first · running is a choice」；印章「APPROVED」；「Plan ⟶ fits people」 | tap、stamp |
| 7 | Sports field · The new plan | 「The RIGHT slow ✓」；「Volunteered — after asking ✓」；「Lap race: cancelled」 | steps、twinkle |
| 8 | Break time | 「00:10 TIME'S UP」被印章「STOPWATCH: OFF DUTY」盖住；「Lunch break · 10 min reserved: Xiaoman's radio plan」 | flip、zipper |

- 震屏在第 3、4、5、8 场。
- 配乐（注释）：试跑期间轻快 BGM，在十秒提示音处切断 → 苏小满发问时静音 → 从 “can we change the schedule” 到重新规划用舒缓 → 新分组轻快 → 走廊舒缓。

## 课后小课堂（约 50.3 s，估算，超出 45 s）

1. **dynamic** adj. 充满活力的。— *My clone promises a dynamic start to the day.*
2. **lifestyle** n. 生活方式。— *His dream lifestyle comes with a stopwatch.*
3. **drag** v. 拖着（脚步）。— *Soon I start to drag my feet.*
4. **leisure** n. 闲暇。**英美音对比**：英 /ˈleʒə/、美 /ˈliːʒər/。— *Xiaoman runs in her leisure time.*
5. **tend to do sth** phrase 往往会做某事。单词发音读 `say` 字段里的 “tend to do something”，高亮关键词 `kw` 是 “tend to”。— *I tend to say yes before I ask enough questions.*

- 复习卡：schedule（*Can we change the schedule?*）、volunteer（*I volunteer to lead the slower group.*）。
- **新增 Recall 词条**：把 ep-003 的 5 个词（actually、differ、in person、frank、to be frank）做成小标签，排在复习卡下方（`segments.tsx` 读取 `story.recall`）。

## 迭代与验收

- 初版大约在 10-31 23:07–23:11。改版在 11-05 09:30–10:09，推断对应会话 `5bbe9ffb`「ep-005 video update and modifications」。宿主显示这个会话一直持续到 11-06 10:08，并带有 1 个文件 +9 行的未提交改动，具体是哪个文件**无法确认**。
- 技术：ffprobe 读到 302.20 s。视觉检查、用户验收没有记录。
- 证据：`data/story.json`（31.5 KB）、`src/fx.tsx`（23.0 KB）、`src/segments.tsx`（21.2 KB）、`.cache/clone/pick.json`。
