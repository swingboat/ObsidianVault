---
type: video-production-dossier
project: english-to-be-continued-opus
repository: swingboat/english-to-be-continued-opus
video_id: episodes/book-01/ep-006
title: EP.006 她想要的不是保密费，是十分钟。
captured: '2026-11-12'
source_fingerprint: 912dd9284ad87b7e7f34650512f40f76abead47de6f200efbdcb32ef5218b728
pipeline: qwen3-clone
duration_s: 283.0
tags:
- video-creation
- production-capture
- english-learning
- remotion
- voice-cloning
---

# EP.006 她想要的不是保密费，是十分钟。

返回 [[项目索引]]。英文片头：*Not Hush Money. / She Wants Ten Minutes.*

## 目标与状态

- **源材料**：HTML 项目 `ep-006.html` 和 `production/book-01/ep-006/`。`production_language` 要求小帅第一人称英文解说，使用固定参考音色。
- **成片**：`out/ep-006.mp4`，283.0 s，约 194 MB，11-06 14:34。
- **当前版本**：克隆解说版，也是**目前最新、最完整的单集模板**。新复制一集时，AGENTS 要求「复制最新一集目录作模板」，按文件时间看就是这一集。
- 这一集有过两个版本：11-01 晨间的 edge 初版，以及 11-06 的克隆改版。

## 剧情

午休时，小帅兑现约定的十分钟，收起秒表，认真听苏小满讲提案。她说：“I **aim** to give quiet students a voice.” 第一期的问题是：入学第一天，什么帮到了你？放学后开提案会，妈妈经老师和家长同意，把分身送来参加这一次讨论。分身带来一张展开后垂到地上的节目表（早间新闻、午间访谈、晚间音乐……），苏小满只圈出其中一个小格：“Our **target** is one ten-minute trial.”

老师问：“Who will **organise** it?” 三人各自认领分工：苏小满选题写稿，小帅排人排时间，分身想学声音检查。分身随后承认自己从没用过这套设备，老师说：“We will build **competence** through practice.” 小帅强调：“Taking part is **voluntary**.” 分身这次「只报我自己」。最后老师原则同意做一期十分钟试播：先交分工、再检查设备，然后再定日期。结尾是小帅的心声：“One small goal. Still a real challenge.”

## 角色与配音

| 键 | 来源 | 说明 |
| --- | --- | --- |
| `lin` / `lin-narr` / `lin-inner` | 克隆 小帅 | — |
| `clone` / `su` / `teacher` | 克隆 | — |
| `lesson` | edge en-US-JennyNeural **+6%** | 之前各集都是 −2%，本集提速；原因没有记录，推断与压缩课堂时长有关 |
| `word-ex` | edge en-US-AriaNeural **−8%** | **本集新增**：例句用比单词发音（−18%）快的语速单独朗读 |
| `word-us` / `word-gb` | Aria / Sonia −18% | — |

- 共 47 句：解说 17、小帅 8、苏小满 8、分身 7、老师 5、心声 2。
- `durations.json` 共 74 段，合计 223.7 s。

**Whisper 选条与人工换条**

- 53 个片段，平均分 0.972，最低 0.727。
- `data/voice-picks.json` = `{"s4n1": 1, "s5n1": 4, "n-prev3": 5}`：
  - `s4n1` “Xiaoman circles one small box.” 被听写为 “Shio Man…”，得分 0.727，人工改用第 1 条；
  - `s5n1` 用第 4 条、`n-prev3` 用第 5 条。说明这两句曾用 `ONLY=… TAKES=5` 补生成了更多 take。`clone.py` 在 11-06 14:21 加入了 `ONLY` 环境变量过滤，正是为这种局部重生成服务的。
- 其余 weak：`s5l1` 0.75，原因是 “organise” 被听写为英式 / 美式的另一种拼法 “organize”，不是读错，保留自动选择。

## 场景与笑点

镜头关键帧 65 个，最大 z=2.7。前情提要使用 ep-005 的 p4、p6、p8。

| 场 | 地点 | 特效 / 笑点（`fx.tsx`） | 剧本音效 + `sceneExtras` 补充音效 |
| --- | --- | --- | --- |
| 1 | Lunch break · A quiet classroom | 名牌：小帅「Red shirt · the real one · telling this story」、苏小满「Classmate · has a radio plan」；`PocketWatch` 收进口袋；`Digits`「10:00」；「LUNCH BREAK · RESERVED FOR Xiaoman's whole plan — no countdown · no rushing」 | birds、zipper；pop、tick、stamp |
| 2 | Xiaoman's pitch | `QuietVoices`：安静同学的气泡依次亮起；`MicIcon`、`OnAir`「SHOW #1 · ONE SHORT EPISODE」；「After school → ask the teacher」 | flip、twinkle、bell；pop×4、sparkle、flip、pop |
| 3 | After school · Planning meeting | 名牌：班主任「Here for the whole meeting」、分身「Blue shirt · Mum said OK · this meeting only」；`MumChat` 家长同意的消息；`Unroll` 节目表垂到地面；「?!」；「We said: ONE show.」 | door、notify、unfold；whoosh、thud、dun |
| 4 | The very long timetable | `ProgramTiles` 节目格、`LengthBars`（节目表比整天的课还长）、红笔 `DrawCircle` 只圈一格、`TargetBoard`；「The rest of the day: folded away」 | jingle、marker、ding、paper；pop×3、scratch、bonk |
| 5 | Who does what? | 「WHO?」；`Role` 分工卡逐一弹出；印章「DRAFT」；「WHO ORGANISES WHAT — draft · confirm next time」 | boing、pop×3；click×3 |
| 6 | The equipment manual | 印章「SAME FACE ≠ SAME SKILLS」；`StatsPanel`「CAN DO ✓ ideas · topics · timing / NEED TO LEARN ✎ controls · sound check」；「Next time: equipment test with the teacher」；冷场时画面去饱和 | thud、cricket、scribble；error、ding |
| 7 | Who's in? | 「TRIAL SHOW · TAKING PART IS VOLUNTARY」；苏小满「her own choice ✋」、分身「only himself ✋」、「Anyone else: ask them first」；把第 4 集的「“We volunteer!”」与今天的「“just me”」对照；印章「ASK FIRST ✓」 | twinkle、rustle×2、stamp；ding×2、scribble |
| 8 | End of the meeting · One box | 印章「APPROVED IN PRINCIPLE」「NEXT STEP ✓」；「1 BOX · 10 MIN」；`Status`「WHERE WE ARE NOW: 10-min trial: OK in principle (not on air yet)」；「Evening music: folded」；心声前画面压暗 | paper、stamp、sparkle、paper；ding×2、rustle、heartbeat |

- 震屏：第 3 场 2 次（节目表落地 `s3n3` 85% 处，以及 `s3l3`），第 4、6、8 场各 1 次。
- **配乐**（`timeline.ts`）：
  1. 冷开场到第 1 场用轻快 BGM；
  2. 苏小满讲提案（第 2 场）转舒缓；
  3. 节目表展开时回到轻快；
  4. 红笔圈格（`s4n1`）时切断；
  5. 分工（`s4l3` 到 `s6-hmm`）再轻快；
  6. 分身坦白前静音；
  7. 「靠练习培养能力」处舒缓；
  8. 举手报名轻快；
  9. 结论舒缓，在心声 `s8-quiet` 前切断，只留心跳。

## 课后小课堂（约 44.7 s，估算，在 45 s 以内）

1. **aim** /eɪm/ v. 力求达到。If you aim to do something, you try to do it. — *I aim to give quiet students a voice.*
2. **target** /ˈtɑːɡɪt/ n. 目标。— *Our target is one ten-minute trial.*
3. **organise** v. 组织（美式拼写 organize）。— *Who will organise it?*
4. **competence** n. 能力。**英美音对比**：英 /ˈkɒmpɪtəns/、美 /ˈkɑːmpɪtəns/。— *We will build competence through practice.*
5. **voluntary** adj. 自愿的。**英美音对比**：英 /ˈvɒləntri/、美 /ˈvɑːlənteri/。— *Taking part is voluntary.*

- **复习卡改版**：不再每个旧词各读一句（`rv-<word>`），改为两个旧词共用一句剧中原句 `reviewExample`：“One small goal. Still a real challenge.”。
  - 卡片标注出处「Scene 8 · Xiaoshuai (thinking)」；
  - 朗读时两个词卡依次高亮（`KaraokeEn`，进度按词数比例推进）；
  - `lines.py` 只生成一条 `rv-example`，用 `word-ex` 语速。
- Recall 词条：ep-004 的 senior、secondary、previous、campus、junior。

## 迭代与验收

- **初版**：11-01 06:33（`Root.tsx`）、06:37（`sfx.py`，新增 unfold、marker、jingle 音效）。推断对应会话 `0e4d0f03` 或 `7d8fc2de`（后者标题为「Producing anime English episodes 006–007」），正文读不到。
- **改版**：11-06 09:40（`tts/check/ui`）→ 14:21（`clone.py` 加 `ONLY`，`fx.tsx`）→ 14:22（`voice-picks.json`）→ 14:26–14:27（`story/durations/lines/segments/timeline`）→ 14:34 成片。推断对应会话 `6a954757`「Update and check ep-006 video」。宿主显示这个会话最后修改于 11-07 07:59，带有 1 个文件 +4/−2 的改动，具体文件**无法确认**。
- 技术：ffprobe 读到 283.00 s。Whisper 选条结果见上。视觉检查、用户验收没有记录。
- 证据：`data/story.json`（26.2 KB）、`data/voice-picks.json`、`scripts/clone.py`（2866 B，含 `ONLY`）、`scripts/lines.py`（含 `word-ex`、`rv-example`）、`src/fx.tsx`（34.4 KB，10 集中最大）、`src/timeline.ts`、`.cache/clone/pick.json`。
