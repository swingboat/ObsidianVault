---
type: video-production-dossier
project: english-to-be-continued-opus
repository: swingboat/english-to-be-continued-opus
video_id: episodes/book-01/ep-004
title: EP.004 两张脸，只能领一张学生任务单。
captured: '2026-11-12'
source_fingerprint: 976f2b702b2df453fd0a328068937a84e09ae703108433a2f8488a45275848b6
pipeline: qwen3-clone
duration_s: 265.0
tags:
- video-creation
- production-capture
- english-learning
- remotion
- voice-cloning
---

# EP.004 两张脸，只能领一张学生任务单。

返回 [[项目索引]]。英文片头：*Two Faces, / Only One Student Task Sheet.*

## 目标与状态

- **源材料**：HTML 项目 `ep-004.html` 和 `production/book-01/ep-004/`。`production_language`：小帅第一人称英文解说，引语由小帅转述。
- **成片**：`out/ep-004.mp4`，265.0 s，约 169 MB，11-04 10:30。
- **当前版本**：克隆解说版。**首次**加入家长角色（妈妈、爸爸），场景从学校扩展到家里。

## 剧情

第二天早上，苏小满陪两人去办公室，两人齐声说 “Good morning, sir!”，老师问怎么听到了两声。小帅从头解释：手机、害怕开口、在 **previous** school 一直沉默、读完 **junior secondary** 也没改变。老师说明，上课、作业和班长试任都由本人承担，任务单「只给本人」；分身可以在 **campus** 里已获批准的课后项目中帮忙，前提是妈妈同意、有老师在场。

妈妈赶来听完全部经过。老师让小帅安排明天的晨间活动，分身抢先说 “We **volunteer** to lead a run!”，小帅说先问问同学。回到家，爸爸在画外说「有事当面说」，妈妈给分身铺床，然后指着碗说：「两个人，就洗两份碗。」两人互相指来指去，最后一个洗、一个擦。夜里小帅写下要问同学的问题，分身已经拿出秒表，想象起比赛了。

## 角色与配音

| 键 | 来源 | 滤镜 / 说明 |
| --- | --- | --- |
| `lin` / `lin-narr` / `lin-inner` | 克隆 小帅 | — |
| `clone` | 克隆 小帅的分身 | — |
| `clone-rush` | 克隆 分身 + `atempo=1.08` | **本集新增**：抢着报名时语速加快 |
| `both` | `mix [lin, clone]` | “Good morning, sir!”、“Huh?!” |
| `su` / `teacher` | 克隆 苏小满 / 班主任 | — |
| `mum` | 克隆 小帅妈妈 | **首次**出场 |
| `dad-vo` | 克隆 小帅爸爸 + `lowpass=f=3400,aecho=0.8:0.4:40:0.18` | 画外音 |

- 共 40 句：解说 20、分身 6（含 rush）、妈妈 4、老师 3、小帅 2、合声 2、苏小满 1、爸爸 1、心声 1。
- `durations.json` 共 67 段，合计 200.9 s。
- Whisper 选条：48 个片段，平均分 0.990，最低 0.875，**没有 weak**；选中 take 分布 t1 15、t2 13、t3 20。

## 场景与笑点

镜头关键帧 60 个，最大 z=2.4。`fx.tsx` 从本集起定义 `LOW = 820`（叠加层放在人脸以下、字幕以上的区域）。前情提要使用 ep-003 的 p3、p6、p7。

| 场 | 地点 | 特效 / 笑点 | 音效 |
| --- | --- | --- | --- |
| 1 | Head teacher's office · Next morning | 双 `Bubble`「Good morning, sir!」×2；名牌：分身「Blue shirt · the copy · loves to volunteer」、班主任「Sees two Xiaoshuais for the first time」、苏小满「Deskmate · saw it with her own eyes」；`FaceCount`；印章「GOOD MORNING ×2」；「pen.exe has stopped」 | knock、tick、paper |
| 2 | Xiaoshuai explains | 印章「STILL QUIET」；时间轴 previous school → Junior secondary → now · Senior secondary；`Calling` 打电话给妈妈 | tap、rustle、notify |
| 3 | At the teacher's desk | `TaskSheet`（任务单只给本人，分身那份被拒）；「Attend lessons for me ✗」；「The clone may help when…」条件卡（`timeline.ts` 用 `[标签, 台词, 比例]` 控制逐条出现） | paper、rustle |
| 4 | Later that day · The office | 名牌：妈妈「Got the call · came to hear us both」；`Rewind` 复述（画面做旧）；「NEW MISSION」；`Volunteer` 抢答配速度线；「Organiser's plan — Step 1: ask the class」对「Say yes for everyone ✗」 | thud、rewind、whoosh |
| 5 | That night · The spare room | 名牌：Dad；`DoorTag`「Clone's request today: A new job / A bed」；`TopFX` 暖色台灯光 | door、rustle |
| 6 | After dinner · The kitchen | `Bowl`、「Mum's math: 2 boys」、双「Huh?!」、`HypeMeter`（多一双手的兴奋值骤降） | clink、ding |
| 7 | At the sink | `Bubbles`；「Wash ✓ Dry ✓ Escape: ✗」；「My goal: Do my own part ✓」 | water、clink、twinkle |
| 8 | The study desk | `RaceDream`（分身脑补的赛跑）、`Stopwatch`、印章「PAUSE」；「Classmates asked so far: 0」；「Clone's new job: not pressing it」 | scribble、beep |

- 震屏在第 1、4、6、8 场。
- 配乐（注释）：办公室轻快 BGM → 坦白时舒缓 → 讲规则时轻快，分身突然报名时切断 → 备用房间里舒缓 → 洗碗梗轻快 → 水池边舒缓 → 书桌前轻快，在 “I have not even asked” 处切断。

## 课后小课堂（约 49.5 s，估算，超出 45 s）

1. **previous** adj. 以前的。— *At my previous school, I often stayed quiet.*
2. **junior** adj. 较低年级的。— *Finishing junior secondary school did not change that.*
3. **secondary** adj. 中等教育的。**英美音对比**：英 /ˈsekəndri/、美 /ˈsekənderi/。— *I finished junior secondary school.*
4. **senior** adj. 较高年级的。— *Now I'm in senior secondary school.*
5. **campus** n. 校园。— *He may help with approved projects on campus after class.*

复习卡：volunteer（*We volunteer to lead a run!*）、goal（*My goal is to do my own part.*）。

## 迭代与验收

- 初版大约在 10-31 22:45（`sfx.py`）。改版在 11-04 10:02–10:30：`lines/clone/check/tts.py` 10:02，`story.json` 10:04，`Root/segments/timeline` 10:06，`fx.tsx` 10:26。推断对应会话 `1aa132fd`「ep-004 video update and modifications」，正文读不到。
- 技术：ffprobe 读到 265.00 s。视觉检查、用户验收没有记录。
- 证据：`data/story.json`（28.6 KB）、`src/fx.tsx`（31.2 KB）、`.cache/clone/pick.json`。
