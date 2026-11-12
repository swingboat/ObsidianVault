---
type: video-production-dossier
project: geo-story-studio-opus
video_id: stratosphere-ozone
title: 平流层的暖气是谁烧的？臭氧怎样让平流层上暖下冷
curriculum: 人教版必修第一册 第二章第一节 大气的组成和垂直分层（平流层气温随高度增加而升高的原因、平流层以水平运动为主）；课标1.4
captured: '2026-11-12'
status: 成片存在（竖屏 1080×1920，257.6 s）；未列入 README 目录；技术检查/视觉检查/用户验收未记录
source_fingerprint: 1bc341a5849f567b14e9fd849c9014cc73a6677c88148d401a262d99936b092e
tags:
- video-creation
- production-capture
- geography-teaching
- canvas-frame-render
- edge-tts
---

# 平流层的暖气是谁烧的？ · 三问结构 + 分子尺度光解动画 + 臭氧累积吸收的紫外线光束（Canvas 逐帧 + 多角色 edge-tts）

<!-- capture-source-sha256:1bc341a5849f567b14e9fd849c9014cc73a6677c88148d401a262d99936b092e -->

本次为材料回溯与归档，结论来自项目文件（`script.json`、`tts.py`、`render.mjs`、`lib.mjs`、`mols.mjs`、`chars.mjs`、`audio.mjs`、`scenes-a…f.mjs`、`build/timeline.json`、`out/` 清单与 ffprobe）。按时间戳**推断**对应的宿主会话为 `7232d07c-05a5-49e2-93d8-db7c5e6bb009`（创建于 2026-10-31 18:08，早于 `script.json` 修改时间 18:21），只按元数据找到，**对话内容无法读取**（宿主返回空 transcript）。本轮未运行任何项目脚本，未打开视频或音频。

## 1. 当前结论与制作路线

**状态：成片存在**——`out/平流层的暖气是谁烧的.mp4` 与同名 `.srt`；视觉检查、试听、用户验收**未记录**。**本片没有列入项目 `README.md` 的视频目录**。

| 项 | 值 | 来源 |
| --- | --- | --- |
| 分辨率 / 画幅 | 1080×1920，9:16 竖屏 | ffprobe；`lib.mjs:7` |
| 帧率 / 帧数 | 30 fps；约 7729 帧 | ffprobe 时长 × 30；`render.mjs:217` |
| 时长 | 257.633 s（五片中最长） | ffprobe；`timeline.json` `duration: 257.65` |
| 视频 / 音频 | H.264（libx264 CRF 19，preset medium，yuv420p，faststart）/ AAC 48 kHz 立体声 192 kbps | ffprobe；`render.mjs:214-215` |
| 文件大小 | 49,502,835 字节（约 49.5 MB，平均约 1.54 Mbps） | 证据包 |
| 字幕 | SRT 107 条 + 画面烧录 | SRT 计数；`render.mjs:83-108` |
| 其他版本 | 无 | `out/` 清单 |

管线属于「A 模板」，直接沿用 [[atmosphere-layers]]：`tts.py` 只差 `TITLE`，`audio.mjs`、`chars.mjs` 哈希完全相同，`render.mjs` 只差场景文件列表（加入 `f`）、背景微粒配色、说话人颜色、资料页脚与品牌卡标语。流程：`script.json`（39 段、4 个角色）→ `tts.py` → `narration.wav` / `timeline.json` / SRT → `render.mjs` 加载 `scenes-a…f`（6 个「章节式」场景，每个场景内部再分阶段）+ 新写的 `mols.mjs` 分子库 → Canvas 逐帧 → ffmpeg。文件时间：`script.json` 18:21 → logo 复制 18:35 → TTS 与 `mols.mjs` 18:37 → `render.mjs` 18:38 → 场景 18:39–18:47 → 成片 18:51。

## 2. 教学目标与课标对齐

- **课标出处**（`script.json:3`）：必修第一册 第二章第一节，平流层气温随高度增加而升高的原因、平流层以水平运动为主；课标 1.4。
- **核心知识点**：臭氧吸收太阳紫外线并把能量转为热，使平流层上暖下冷；上暖下冷 → 大气稳定 → 以水平运动为主，有利于高空飞行。
- **讲解结构**（`timeline.json`，明确的「三个问题」框架）：
  - 导入：小宇「平流层越往上越暖，天上装了暖气片？」→ 臭氧自我介绍（O₃）→ 列出三个问题（`hook`…`plan`）。
  - 问题一·阳光怎么变成热：紫外线能量高 → O₃ 吸收后分解为 O₂ + O → O 与 O₂ 再结合 → 多余能量经碰撞交给空气分子、分子更快即升温 → 「吸收，不是反射」+ 教材原话（`q1`…`absorb`）。
  - 问题二·为什么越往上越暖：「臭氧最多处最暖吧？」→ 否，最暖在顶部约 50 千米 → 原因一「排队领紫外线」、原因二「顶部空气约为海平面千分之一」→ 平流层下部变化小、约 30 千米以上明显升温（`q2`…`profile`）。
  - 问题三·上暖下冷的脾气：思想实验推一团空气上去 → 膨胀降温 → 同一高度与周围比更冷更重 → 回落，对流被压住 → 教材原话；稳定 ≠ 一动不动（`q3`…`notstill`）。
  - 迁移：积雨云在对流层顶摊成砧状云顶；1991 年皮纳图博火山颗粒滞留平流层，之后一年多全球平均气温降约 0.6℃；有利于高空飞行（`proof`…`plane`）。
  - 总结：一句话总结 + 三个答题关键词 + 两个易错点 + 臭氧「替地面挡掉有害紫外线」+ 小宇收尾（`recap`…`end`）。
- **科学口径边界**：`based_on` 指向 `geo-story-studio/storage/builds/douyin-stratosphere-ozone/v002`，口径包括：吸收而非反射；只演 O₃→O₂+O 光解与再生成，不展开完整反应网络；只说能被臭氧吸收的那部分紫外线沿路减少，不说全部消失；臭氧含量不能单独决定最暖位置；平流层下部气温变化小；气团抬升只在未饱和、忽略热交换时成立，比较在同一高度进行；稳定不等于没有垂直运动；不讲航空安全保证；不用固定臭氧峰值高度和实测温度读数。代码对应：臭氧密度「qualitative … (no numbers on screen)」（`scenes-c.mjs:12`）、温度廓线「no ℃ ticks」（`scenes-c.mjs:157`）、「same-level comparison」（`scenes-d.mjs:89`）、「stable ≠ motionless: gentle waves」（`scenes-d.mjs:147`）、紫外线光束衰减但不归零（§5.2）。
- **数字与出处**（`script.json:13-19`）：平流层顶约 50 千米（教材、UCAR）；约 30 千米以上明显升温（教材图示、UCAR 虚拟探空）；顶部空气约海平面千分之一（UCAR）；约 90% 臭氧在平流层（WMO/NOAA 2018，注明「旁白不直接念出，仅供图注核对」）；1991 年皮纳图博、约 1500 万吨 SO₂、约 15 个月全球降温约 0.6℃（NASA Earth Observatory）。

## 3. 技术与依赖

| 类别 | 内容 | 证据 |
| --- | --- | --- |
| 渲染 | Node.js ESM + `@napi-rs/canvas` 声明 `^1.0.11`，锁定 1.1.0（darwin-arm64 1.1.0） | `package.json`；`package-lock.json` |
| npm scripts | 仅 npm 默认 `test` 占位 | `package.json` |
| TTS | Python `edge-tts` + `aiohttp`；版本未记录 | `tts.py:16-18` |
| 字体 | `fonts.py`（fontTools，同哈希 `a6ac7264e548`） | 证据包 |
| ffmpeg | 同 A 模板：旁白 `adelay`/`amix`/`highpass`/`loudnorm`（支持角色 `fx`，本片未用）；总混音 `sidechaincompress`/`amix`/`alimiter` | `tts.py:177-192`；`render.mjs:205-216` |
| 素材 | `assets/logo-transparent.png`（使用）与 `assets/logo.png`（深蓝底版，**代码未引用**） | `lib.mjs:25`；证据包 |

| 角色 key | 标签 | 声音 | rate（角色 + 全局 4） | pitch |
| --- | --- | --- | --- | --- |
| `zh` | 旁白 | zh-CN-XiaoxiaoNeural | +4% | 0 |
| `kid` | 小宇 | zh-CN-YunxiNeural | +10% | +2 Hz |
| `o3` | 臭氧 | zh-CN-XiaoyiNeural | +12% | +4 Hz |
| `parcel` | 小气团 | zh-CN-YunxiaNeural | +10% | +2 Hz |

`build/tts_cache/` 78 个文件 = 39 段 × (mp3 + json)，只有一批语速；`TARGET = None`；`timeline.json` `rate: 4`。

## 4. 镜头与时间轴

每个场景文件只导出 **一个**章节式场景（`FROM = S('qN') − 0.3`），章节内部用多个「阶段」函数按词时间交叉淡化（例如 `scenes-b.mjs:181-187`）；章节之间仍用 `render.mjs` 的 `XF = 0.3` 淡化 + 推拉缩放。

| # | 场景文件（章节） | 约时间 (s) | 旁白要点 | 画面（阶段） | 模块 |
| --- | --- | --- | --- | --- | --- |
| 1 | 导入 | 0–25.1 | 天上有暖气片？臭氧登场；三个问题 | 对流层/平流层窄条中漂浮的暖气片 →「有！」章 → 臭氧三原子吉祥物 → 三张问题卡 | `scenes-a.mjs:8-120` |
| 2 | 问题一 | 25.1–74.4 | 紫外线、光解、再结合、碰撞升温、吸收不是反射 | `stageMolecules`：紫外线波包击中 O₃ → O₂ + O → O 撞上 O₂ 复合，循环环；`stageHeat`：气体分子盒子随加热越动越快、温度计上升；`stageAbsorb`：吸收 vs 反射对比 + 教材卡 | `scenes-b.mjs:39-187` |
| 3 | 问题二 | 74.4–138.4 | 最暖不在臭氧最多处；排队领紫外线；稀薄空气；廓线 | 左侧高度柱 + 臭氧点 + 逐渐变窄的紫外光束 + 强度表；右侧「平流层顶 4 个分子 vs 海平面附近 40 个分子分同一份热」；再换成定性温度廓线 | `scenes-c.mjs:31-249` |
| 4 | 问题三 | 138.4–182.4 | 推一团空气上去 → 膨胀变冷 → 同高度比较 → 回落；稳定 ≠ 不动 | 上暖下冷背景色带；推手箭头、压力箭头缩小、气团发抖；同高度对比；对流被划掉；教材卡 + 水平流线 + 轻微波动 | `scenes-d.mjs:18-162` |
| 5 | 迁移 | 182.4–222.2 | 砧状云；皮纳图博火山；高空飞行 | 积雨云长到对流层顶后摊开成砧；火山烟柱、平流层霾层、降雨只在对流层顶以下、阳光被部分遮挡；飞机在平稳的平流层下部 | `scenes-e.mjs:10-191` |
| 6 | 总结 | 222.2–257.7 | 一句话总结、关键词与易错点、臭氧道别、小宇收尾 | 因果链 → 答题卡 → 臭氧挥手 → 收尾 | `scenes-f.mjs:9-116` |
| — | 品牌卡 | 254.6–257.7 | 无旁白 | 居中 logo +「用地理的眼光，看懂头顶的天空 / 臭氧吸收紫外线 · 平流层上暖下冷」 | `render.mjs:119-140` |

## 5. 关键算法与实现

> 区分：视频讲的是教材层面的「臭氧吸收紫外线 → 平流层增温」；下面是**制作算法**。光束衰减和臭氧分布都是定性形状，不是辐射传输或化学模式计算。

### 5.1 共用管线（A 模板，与 [[air-composition]]、[[atmosphere-layers]] 相同）

WordBoundary 逐词时间 → `lib.mjs:36-46` `word()/at()`；`tts.py:47-63` 缓存键 + 429 退避；`tts.py:20-26` 替换 `WSS_URL`；`render.mjs:159-188` 场景淡化缩放、震屏、角标 logo 与 `CARD_T = E('end') + 0.3` 品牌卡。

### 5.2 「排队领紫外线」：按上方臭氧累积量衰减的光束（`scenes-c.mjs:12-24, 59-85`）

```js
const n3 = (z) => (z < TROP - 1 ? 0.04 : Math.exp(-(((z - 23) / 7.5) ** 2)));
const CUM = (() => { const a = []; let s = 0;
  for (let z = 56; z >= 0; z -= 0.5) { s += n3(z) * 0.5; a.push([z, s]); }
  return a; })();
const uvLeft = (z) => {
  const tot = CUM[CUM.length - 1][1];
  const hit = CUM.find(([zz]) => zz <= z) ?? CUM[CUM.length - 1];
  return Math.exp(-2.4 * hit[1] / tot);
};
```

- 臭氧数密度取以 23 km 为中心的高斯形状（只作形状，画面不标数字）；自上而下累积求和得到「头顶已经有多少臭氧」。
- 剩余紫外线 `uvLeft = exp(−2.4 × 累积量 / 总量)`，是比尔—朗伯式的指数衰减；光束宽度 `30 + 150·uvLeft(z)`，下行的波包透明度也随之变淡（`scenes-c.mjs:59-85`）。
- 因为指数下限约 `e^−2.4 ≈ 0.09`，光束到底部仍留一条细线，对应口径「只说能被吸收的那部分沿路减少，不说全部消失」。
- 臭氧点用拒绝采样生成：在 12–52 km 随机取高度，按 `n3(z)` 概率保留，凑够 120 个（`scenes-c.mjs:25-29`）。

### 5.3 分子尺度：弯曲 O₃ 与相位连续的「越动越快」（`mols.mjs:71-80, 157-163`）

- `o3Pos()` 按约 117° 键角放置三个原子，中间原子带表情；光解时第三个原子被拉开、右侧键淡出。
- 加热阶段分子运动速度从 0.8 线性升到 3.6，用速度的积分做相位，避免直接改频率时画面跳变：

```js
export function rampPhase(t, t0, t1, s0, s1) {
  if (t <= t0) return t * s0;
  const u = Math.min(t, t1) - t0, k = (s1 - s0) / (t1 - t0);
  const base = t0 * s0 + s0 * u + 0.5 * k * u * u;
  return t > t1 ? base + (t - t1) * s1 : base;
}
```

`scenes-b.mjs:121` 调用 `rampPhase(t, t0, t1, 0.8, 3.6)` 驱动 `gasField()` 的抖动相位。

### 5.4 稀薄空气分热（`scenes-c.mjs:112-156`）

两只盒子分别放 4 个和 40 个分子，同一份热量在「分给」一词时到达：上面盒子的分子抖动幅度放大 `1 + 2.2·heatOn`，下面只 `1 + 0.3·heatOn`。画面数量比是 1:10，只做定性对比，旁白给出的才是「约千分之一」。

### 5.5 章节内阶段切换

章节文件内部用 `fadeIn(t, 词时间)` 与 `1 − prog(t, 下一阶段时间)` 相乘，把同一章节拆成多段（`scenes-b.mjs:181-187`）。这让 39 段旁白只对应 6 个场景文件，每个文件 129–266 行，符合 AGENTS.md §3「一次写一个文件、约 300 行以内」。

## 6. 声音与字幕

| 项 | 内容 | 来源 |
| --- | --- | --- |
| 声音 | 旁白 / 小宇 / 臭氧 / 小气团，见 §3；未用 `fx` | `script.json` `voices` |
| 旁白混音 | `aresample 48k` → `adelay` → `amix normalize=0` → `highpass=f=80` → `loudnorm=I=-16:TP=-1.5:LRA=11` | `tts.py:177-192` |
| 时间参数 | `LEAD_IN 0.6`、`GAP 0.45`（逐段另设）、`TAIL 3.2` | `tts.py:33-36` |
| 配乐 | 与 atmosphere-layers 同文件：106 BPM，C–Am–F–G 拨弦 + 低音 + 沙锤 + 钟琴，种子 42 | `audio.mjs:189-225` |
| 音效 | 实际触发 19 种：note ×16、stamp ×10、whoosh / pop ×6、boing ×5、ding ×4 等；说到「30千米」时触发一次 `hum`（`scenes-c.mjs:263`）；14 个事件带 `shake` 震屏 | 场景 `events` 统计 |
| 总混音 | 配乐 ×0.75 侧链压缩、音效 ×0.5、`alimiter 0.93` | `render.mjs:209-212` |
| 字幕 | SRT 107 条；烧录 `SUB_Y = 1500`，说话人标签小宇（蓝）、臭氧（青）、小气团（粉） | `render.mjs:23, 81` |

未试听、未测响度。

## 7. 迭代与问题

| 事项 | 证据 | 说明 |
| --- | --- | --- |
| **未列入 README 目录** | `README.md` 中检索不到 `stratosphere-ozone` | 成片已存在，目录缺行 |
| TTS 说明沿用上一片 | `tts.py:3-4` 写「host / student / lift announcer / ozone」「e.g. the lift loudspeaker」 | 本片没有电梯广播角色；docstring 从 [[atmosphere-layers]] 复制未改（**推断**） |
| 音频库整份复制 | `audio.mjs` 与 atmosphere-layers 哈希相同 | 电梯 `chime` 音效在本片未使用，`hum` 只在「30千米」处用一次 |
| 深蓝底 logo 未使用 | `assets/logo.png` 存在，`lib.mjs:25` 只加载 `logo-transparent.png` | 多复制了一份素材；不影响成片 |
| `based_on` 写成本机绝对路径 | `script.json:4` | 本档案改写为 `geo-story-studio/storage/builds/douyin-stratosphere-ozone/v002`；源 build 未读取 |
| 与 [[atmosphere-layers]] 内容衔接 | atmosphere-layers 的 `o3`/`upper`/`flat` 段已讲臭氧加热和平流层水平运动 | 本片把这部分单独展开（光解循环、最暖位置、稳定性思想实验、砧状云、火山）；是否有意做成系列**未记录** |
| 画面数量是定性比例 | `scenes-c.mjs:116` 用 4 对 40 个分子 | 旁白说「约千分之一」，画面只做定性对比；是否需要在画面上注明未见要求 |
| 规则符合情况（代码中存在） | 角标 logo、品牌卡、资料页脚「人教版地理必修第一册 · UCAR · NOAA · NASA」（`render.mjs:115`）；臭氧密度与温度廓线不标数字；未检出「示意」类声明 | 画面效果未核对 |

## 8. 复现入口

命令来自 AGENTS.md「常用命令」与 `render.mjs:2-3`，**本次均未执行**：

```bash
cd stratosphere-ozone
npm install
python3 -m venv .venv && .venv/bin/pip install edge-tts   # fonts.py 另需 fontTools
.venv/bin/python tts.py              # → build/narration.wav、build/timeline.json、out/平流层的暖气是谁烧的.srt
node render.mjs --still 45 100 160   # 预览帧
node render.mjs                      # → out/平流层的暖气是谁烧的.mp4
```

修改入口：臭氧分布与衰减强度 → `scenes-c.mjs:12-24`（`n3`、系数 2.4）；分子造型 → `mols.mjs`；章节边界 → 各 `scenes-*.mjs` 第 6–7 行 `FROM/END`。

## 9. 参考来源

| 来源 | 用途 | 状态 |
| --- | --- | --- |
| 人教版必修第一册 第二章第一节 | 平流层增温原因、上热下冷、水平运动 | `script.json:21`，无 URL |
| <https://scied.ucar.edu/learning-zone/atmosphere/stratosphere> | 臭氧加热、稳定分层、顶部空气约千分之一、颗粒滞留 | 项目文件中提及，本次未访问 |
| <https://science.nasa.gov/earth/earth-observatory/ozone/> | O₃ 光解与再生成循环把紫外能量变成热 | 项目文件中提及，本次未访问 |
| <https://www.csl.noaa.gov/assessments/ozone/2018/twentyquestions/> | 臭氧是平流层自然热源；约 90% 臭氧在平流层 | 项目文件中提及，本次未访问 |
| <https://www.noaa.gov/jetstream/atmosphere/layers-of-atmosphere> | 最高温在平流层顶部；砧状云顶标出平流层底 | 项目文件中提及，本次未访问 |
| <https://scied.ucar.edu/activity/virtual-ballooning-explore-atmosphere-activity> | 上部先遇到较强紫外线 | 项目文件中提及，本次未访问 |
| <https://www.weather.gov/lmk/indices> | 抬升气团比同高度周围冷则回落、大气稳定 | 项目文件中提及，本次未访问 |
| <https://science.nasa.gov/earth/earth-observatory/global-effects-of-mount-pinatubo-1510/> | 皮纳图博火山气溶胶滞留、全球降温约 0.6℃ | 项目文件中提及，本次未访问 |
| `geo-story-studio/storage/builds/douyin-stratosphere-ozone/v002` | 改编来源与口径 | 本次未读取 |

## 10. 证据与限制

| 文件 | 行数 | sha256[:12] | 读取程度 |
| --- | --- | --- | --- |
| `stratosphere-ozone/script.json` | 77 | `af9fee744b8b` | 全部字段（段落经 timeline 读出） |
| `stratosphere-ozone/tts.py` | 210 | `35cb57108b5b` | 与 air-composition diff |
| `stratosphere-ozone/render.mjs` | 228 | `4da04b6ab8d3` | 与 air-composition diff |
| `stratosphere-ozone/lib.mjs` | 222 | `273dcb311e1c` | 与 air-composition diff |
| `stratosphere-ozone/mols.mjs` | 214 | `89655ca8ec36` | 导出表、`o3Pos`、`rampPhase` |
| `stratosphere-ozone/audio.mjs` / `chars.mjs` | 244 / 187 | `50c6a7a5dc02` / `ccf861832fc8` | 与 atmosphere-layers 哈希相同 |
| `stratosphere-ozone/scenes-a…f.mjs` | 129/204/266/178/210/132 | `6da77195f491` / `c9421847f6da` / `138e706ea8b3` / `d85853aca8b0` / `11eacfd14c03` / `d4bc4f640f01` | 头注释、函数表、`FROM/END`；`scenes-c` 光束与分热片段；`scenes-d/e` 注释 |
| `stratosphere-ozone/build/timeline.json` | — | — | 全部段落时间与文字 |
| `stratosphere-ozone/out/平流层的暖气是谁烧的.mp4` | — | 49,502,835 字节 | 仅 ffprobe |

限制：宿主会话不可读；需求、预览、用户反馈未记录；未运行脚本、未看画面、未试听；依赖版本未记录；git 仅有批量提交 `8cbe673`（2026-11-01）。
