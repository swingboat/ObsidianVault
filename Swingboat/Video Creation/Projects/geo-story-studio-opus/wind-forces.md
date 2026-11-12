---
type: video-production-dossier
project: geo-story-studio-opus
video_id: wind-forces
title: 风为什么不直奔低气压？三个力的拔河赛
curriculum: 人教版必修第一册 第二章第二节 大气受热过程和大气运动（大气的水平运动——风：三力与高空风/近地面风）；课标 1.4
captured: '2026-11-12'
status: 成片存在（竖屏 1080×1920，204.7 s）；技术检查/视觉检查/用户验收未记录
source_fingerprint: 2605bf6f9e55673928590c8b947cbd3c559903a5042c309d57526e0a57e1c778
tags:
- video-creation
- production-capture
- geography-teaching
- canvas-frame-render
- edge-tts
---

# 风为什么不直奔低气压？ · 三个力的拔河赛：程序计算受力平衡的风向图（Canvas 逐帧 + 五角色 edge-tts）

<!-- capture-source-sha256:2605bf6f9e55673928590c8b947cbd3c559903a5042c309d57526e0a57e1c778 -->

本文是根据项目文件做的材料回溯（`script.json`、`tts.py`、`render.mjs`、`lib.mjs`、`cast.mjs`、`props.mjs`、`audio.mjs`、5 个 `scenes-*.mjs`、`build/timeline.json`，以及 ffprobe 结果）。宿主环境中按元数据找到一个可能对应的制作会话 `30f7bb5d-4b72-4b95-b95e-2c6bfaaa7039`（2026-10-31 22:29 创建），但**读不到对话内容**（宿主返回空 transcript），对应关系只是根据时间戳**推断**：`script.json` 于 22:35:28 落盘。本次没有运行任何项目脚本。

## 1. 当前结论与制作路线

**状态**：`out/` 里有成片 MP4 和 SRT。画面检查、用户验收、发布情况都**未记录**。

| 项 | 值 | 来源 |
| --- | --- | --- |
| 分辨率 / 帧率 | 1080×1920 竖屏，30 fps | ffprobe；`lib.mjs:7` |
| 帧数 | 6142（`round(204.723×30)`，与 ffprobe 的 204.733 s 一致） | `render.mjs` 主循环公式 |
| 时长 | 204.733 s（timeline 204.723） | ffprobe；timeline |
| 编码 | H.264 libx264 medium CRF 19 yuv420p + AAC 48 kHz 立体声 192k，faststart | ffprobe；`render.mjs:176` 附近 |
| 文件 | 31,380,596 B，平均约 1.23 Mbps | evidence pack |
| 字幕 | `out/风为什么不直奔低气压？三个力的拔河赛.srt`，93 条；画面同时烧录 | 文件 |
| 文件时间 | script 22:35:28 → timeline 22:37:36 → MP4 22:55:38 | mtime |

**一句话管线**：公共管线和 [[air-pressure]] 一样（37 段，5 个声音，全局 `rate: 4`），只有 `render.mjs` 的背景从漂浮的分子光点改成了横向飘动的风纹（`render.mjs:22-45`）。`lib.mjs` 新增了一套**受力图配色约定**：“力用实心箭头，风速用空心箭头”。`props.mjs` 是受力图工具集（等压线、力箭头、空心风向箭头、指北针、图例）。5 个场景模块共 9 个场景。

## 2. 教学目标与课标对齐

- **curriculum**：人教版必修第一册 第二章第二节 大气受热过程和大气运动（大气的水平运动——风：水平气压梯度力、地转偏向力、摩擦力；高空风与近地面风）；课标 1.4。
- **核心知识点**：水平气压梯度力垂直于等压线、由高压指向低压，是形成风的直接原因。地转偏向力北半球向右、南半球向左，要顺着风的前进方向判断；它始终与风向垂直，只改变风向，不改变风速。高空两力平衡，风向与等压线平行（地转风）。近地面三力平衡，风斜穿等压线、偏向低压，斜交角度不固定。
- **叙事结构**：
  - 导入：小林问“天气图上的风怎么都斜着走”，阿飘抱怨被推、被拽（`ask/piao/hook`）。
  - 原理：① 梯度力（`iso…rush`）→ ② 转盘实验引出地转偏向力（`but…pian2`）→ ③ 高空两力平衡，地转风（`high…geo`）→ ④ 近地面加上摩擦力（`ground…piao3`）。
  - 方法：考试画风向三步走（`exam/s1-s3`）。
  - 迁移：北半球近地面低压，风逆时针旋转流入中心（`swirl/wow`）。
  - 总结：一句话总结（`sum`）和道别（`bye`）。
- **科学边界**（`source` 里 review-001 的审校结论，写进旁白）：
  - 用“同高、直等压线”作参照。
  - 偏向力的左右相对于**当前运动方向**判断，与风向垂直，不改变风速。
  - 地转风成立的前提是“远离赤道、摩擦很小、等压线近似平直”（`geo`）。
  - 斜交角度“并不固定，和摩擦大小、纬度高低都有关系”（`angle`）。
  - `numbers` 只有一条说明：“本片不出现实测风速、力值或固定偏转角度”。
- `script.json` **没有 `based_on`**。`source` 写的是“改编自 Geo Story Studio · douyin-wind-forces v002”，按命名惯例推断源 build 为 `geo-story-studio/storage/builds/douyin-wind-forces/v002/`（**推断**）。

## 3. 技术与依赖

| 类别 | 内容 | 依据 |
| --- | --- | --- |
| 渲染 | `@napi-rs/canvas` 声明 `^1.0.11`，lock 解析为 **1.1.0**；没有 npm scripts | `package.json`、lock |
| TTS / 字体 / 合成 | edge-tts + aiohttp（版本未记录）；fontTools；ffmpeg | `tts.py`、`fonts.py` |
| 配色约定 | `grad #ff6b4a`（梯度力）、`cor #b98cff`（偏向力）、`fric #e0a458`（摩擦力）、`vel #7df9ff`（风向）、`iso #9fc4ff`（等压线） | `lib.mjs` 的 `C` 调色板注释 |

| 角色 | 声音 | rate（+全局 4） | pitch |
| --- | --- | --- | --- |
| 旁白老师 `zh` | zh-CN-XiaoxiaoNeural | +4% | 0 |
| 学生小林 `stu` | zh-CN-YunxiNeural | +10% | +2 Hz |
| 风团阿飘 `wind` | zh-CN-XiaoyiNeural | +14% | +6 Hz |
| 偏偏（地转偏向力）`cor` | zh-CN-YunxiaNeural | +12% | +2 Hz |
| 糙叔（摩擦力）`fric` | zh-CN-YunjianNeural | +4% | −2 Hz |

三个力各有一个角色，台词标签的颜色和受力图里对应箭头的颜色相同（`render.mjs:81`）。

## 4. 镜头与时间轴

| # | 场景 | 约时间 (s) | 旁白要点 | 画面 | 模块 |
| --- | --- | --- | --- | --- | --- |
| 1 | intro | 0–16.9 | 天气图上的风为什么斜着走 | 一张“近地面天气图”：先画一条从高直指低的虚线箭头（预期），再画一圈斜着流向低压的风向箭头（实际）；阿飘被推、被拽 | `scenes-a.mjs:51` |
| 2 | grad | 16.9–47.3 | ① 梯度力 | 竖直等压线 1012～1000 hPa（西高东低），1004 线上光点表示“处处相等”；梯度力配直角标记，等压线收紧时变长；阿飘匀加速冲向低压，弹出“地球在自转！” | `scenes-a.mjs:77` |
| 3 | spin | 47.3–63.4 | 转盘实验 | 逆时针转盘：先从外面看（球走直线），再站在盘上看（球向右偏） | `scenes-b.mjs:47` |
| 4 | cor | 63.4–95.2 | ② 偏向力 | 地球俯视；南北半球面板（右偏 / 左偏）；行人先向东再转向南，提示“往南吹：右手边 = 画面左侧”；圆周运动演示“始终垂直，只改风向，不改风速” | `scenes-b.mjs:159` |
| 5 | high | 95.2–126.1 | ③ 高空两力平衡 | 糙叔“够不着”；阿飘从静止出发，一边加速一边右转，最后与等压线平行；地转风条件卡 | `scenes-c.mjs:29` |
| 6 | low | 126.1–156.2 | ④ 近地面三力平衡 | 地面树和房子，糙叔拽住阿飘；三力平衡后风斜穿等压线，标出“斜交”角度弧；斜交角在一个范围内来回摆动 | `scenes-c.mjs:113` |
| 7 | exam | 156.2–175.7 | 考试画风向三步走 | 倾斜的等压线和 P 点：画梯度力 → 右偏弧 → 近地面风（锐角）和高空风（90°） | `scenes-d.mjs:41` |
| 8 | swirl | 175.7–187.9 | 北半球近地面低压 | 同心等压线 1000/1004/1008；梯度力指向中心，风向右偏；粒子沿螺线流入，标“逆时针 · 向中心流入” | `scenes-d.mjs:96` |
| 9 | sum | 187.9–204.7 | 一句话总结 | 三张“分工卡”，高空（平行）和近地面（斜交）两个小面板对比；约 202.1 s 起切到片尾品牌卡 | `scenes-e.mjs:36` |

## 5. 关键算法与实现：等压线和力箭头是怎么算出来、画出来的

> 视频**讲的知识**是风的受力分析；下面说的是**做视频用的算法**。各个力的大小只是**无量纲的示意值**：梯度力固定为 1，对应 `LG=150` px。这不是大气动力学的数值积分，脚本里也故意不给出实测力值和固定偏转角。

**5.1 坐标约定和等压线**（`scenes-c.mjs:2`；`props.mjs:53-89`）。代码注释写明“Screen frame: x east, y south (上北下南); a right turn is a positive canvas angle. High pressure west, low east.”。Canvas 的 y 轴向下，所以角度加正值时，屏幕上看是**顺时针**转动，也就是相对于前进方向**向右**转。`isobars()` 画 4 条竖直的直等压线，数值 1012/1008/1004/1000 hPa 从西向东递减，相邻两条差 4 hPa；数值标在线的上端，右上角标单位 hPa；背景从左侧的暖色（高）渐变到右侧的冷色（低），两侧分别写“高 / 低”。`spacing` 参数让等压线向中间收紧（`isoXs`）。指北针 `compass()` 是一个白色箭头加“N”，出现在 grad、high、exam、swirl 四个场景（`scenes-a.mjs:86`、`scenes-c.mjs:34`、`scenes-d.mjs:57, 110`）。low 场景和开场的“近地面天气图”没有画指北针，按照“上北下南”的默认约定处理（AGENTS §6）。

**5.2 箭头分两类**（`props.mjs:91-128`）：`force()` 是实心箭头，先画一层深色描边，再画彩色箭身，标签放在箭头尖的外侧。`vel()` 是**空心虚线**箭头，填充半透明青色，专门表示风向（速度），和“力”在形状上分开。图例 `legend()` 用“实心 / 空心”两种色块区分。

**5.3 梯度力和“只有一个力”的情形**（`scenes-a.mjs:77-149`）：3 支梯度力箭头都朝东（角度 0），长度为 `lerp(110, 200, squeeze)`，旁白讲“等压线越密”时变长；箭头和 1008 线交点处画一个直角标记，表示二者垂直。阿飘冲刺时 `px = lerp(x0, x1, q²)`，也就是从静止开始匀加速、垂直穿过等压线。

**5.4 转盘：惯性系和旋转系的坐标变换**（`scenes-b.mjs:10-24, 47-95`）：

```js
const DC = [540, 860], DR = 360, OMEGA = 0.45; // counter-clockwise on screen
const U = (2 * DR) / FLIGHT, LAND = (2 * DR * U) / ((OMEGA * DR) ** 2 + U * U);
function ballInertial(tau) { return [OMEGA * DR * tau, DR - U * tau]; }   // 直线 + 抛出点的切向速度
function toDisk([x, y], tau) { const c = Math.cos(OMEGA * tau), s = Math.sin(OMEGA * tau);
  return [x * c - y * s, x * s + y * c]; }                                 // 逆旋转回盘面坐标
```

惯性系里球走直线；`LAND` 是球再次碰到盘边的解析时刻。切到“站在转盘上看”后，把同一组轨迹点逐点逆旋转 `OMEGA·tau`，得到向右弯的曲线。转盘在屏幕上逆时针转，对应从北极上空俯视地球自转的方向，所以偏转向右，和北半球一致。

**5.5 高空：梯度力与偏向力平衡**（`scenes-c.mjs:12-27, 42-58`）。风向角 `th` 用 easeIO 从正东（0）转到正南（π/2），速度 `v = 1−(1−u)²` 从 0 增加到 1。轨迹按这两个量逐步累加 240 步，再缩放到画面里：

```js
force(ctx, x, y, 0, LG, '梯度力', C.grad, ...);                 // 恒向东，长度 LG
force(ctx, x, y, th + Math.PI / 2, LG * v, '偏向力', C.cor, ...);  // 永远在风向右侧 90°，大小 ∝ 风速
vel(ctx, x, y, th, 40 + 160 * v, { label: '风向' });
```

到达平衡（旁白说到“风向与”）时，`th = π/2`，风向朝南；偏向力朝西，长度等于梯度力，两力大小相等、方向相反。这时风向与南北向的等压线平行，低压在风向左侧（东）。这符合北半球的地转风关系，即白贝罗定律：背风而立，低压在左。

**5.6 近地面：每一帧都严格满足三力平衡**（`scenes-c.mjs:119-147`）：

```js
if (t > tS + 1.2) { spd = Math.cos(al); cor = spd; fr = Math.sin(al); }
const head = Math.PI / 2 - al;                                       // 由正南向东（低压侧）偏 α
force(ctx, x, y, 0, LG, '梯度力', C.grad, ...);
force(ctx, x, y, head + Math.PI / 2, LG * cor, '偏向力', C.cor, ...);  // 风向右侧 90°
force(ctx, x, y, head + Math.PI, LG * fr, '摩擦力', C.fric, ...);      // 与风向相反
```

本次做了代数核对：设梯度力为 (1, 0)，偏向力为 cosα·(−cosα, sinα)，摩擦力为 sinα·(−sinα, −cosα)，三者相加，x 分量 = 1 − cos²α − sin²α = 0，y 分量 = sinα·cosα − sinα·cosα = 0。所以每一帧的合力都**严格为零**；风速 ∝ cosα，所以摩擦越大（α 越大）风越慢。动画分两步：先 `m1`，摩擦力出现，风速和偏向力一起减小；再 `m2`，风向朝低压一侧转到 α=30°；讲到“顶不住”时，在梯度力箭头尖端加一个红色光晕，表示短时间内梯度力占上风。讲到“角度并不固定”时，α 按 `30° + 14°·sin(...)`，在 16°～44° 之间来回摆，每一帧仍是三力平衡。斜交角由金色弧线标出，弧从风向画到正南的等压线方向。

**5.7 考试三步和北半球近地面低压**（`scenes-d.mjs:9-85, 88-135`）：
- exam：等压线倾斜 `BETA=−0.45` rad，1012→1000 沿 `G_ANG = BETA+π/2` 的方向（指向东南）依次排开。三步依次是：梯度力（垂直于等压线，配直角标记）→ 紫色右偏弧 → 近地面风取 `G_ANG+55°`（与等压线斜交 35°，锐角），高空风取 `G_ANG+90°`（与等压线平行）。
- swirl：圆形等压线由内向外为 1000/1004/1008，中心标“低”。8 个点上先画指向中心的梯度力，再画右偏 `(π/2 − 35°)` 的风向箭头。粒子沿**对数螺线** `θ = θ0 − cot35°·ln(r0/r)` 向内流动，这条曲线与每一圈等压线都以 35° 相交；θ 随 r 减小而减小，屏幕上看是逆时针转入中心，最后加一段逆时针弧形箭头标记。开场“天气图”里的风向箭头也用同一套写法（`scenes-a.mjs:43`，偏角 `π/2−0.6`）。那里的等压线画成椭圆，但梯度方向是取“指向中心”近似的（**本次读代码的判断**）。

公共管线（WSS_URL 替换、sha1 缓存、429 退避、WordBoundary → `at()`、`adelay/amix/loudnorm`）与 [[sky-colors]] 相同；`LEAD_IN=0.8`，`TAIL=2.8`。

## 6. 声音与字幕

- 旁白：`highpass 80 → loudnorm I=-16:TP=-1.5:LRA=11`。配乐 112 BPM（C–G–Am–F），音量 0.55，以旁白侧链压缩；音效 0.5；`alimiter 0.93`；切场景自动加 whoosh。总结段按三个力依次触发 note 72/76/79，两种风各配一个 pop，道别时放 confetti（`scenes-e.mjs` 的 events）。
- 字幕：SRT 93 条，每块 ≤14 字或用 `captions`；`SUB_Y=1560`；角色标签为小林 / 阿飘 / 偏偏 / 糙叔，颜色同各自箭头。
- 品牌：左上角 logo；片尾卡从 `E('bye')+0.3` 开始（约 202.1 s，持续约 2.6 s），标语“用地理的眼光，看懂风的来去”；页脚“资料：人教版地理必修第一册 · Stull《Practical Meteorology》”（`render.mjs:49-79, 112`）。

## 7. 迭代与问题

| 事项 | 证据 | 说明 |
| --- | --- | --- |
| 缺少 `based_on` | `script.json` keys | 不符合 AGENTS §1，只在 `source` 文字里写了 build 名 |
| 页脚资料不全 | `render.mjs:112` 只写了教材和 Stull | `source` 里还引用了 PSU METEO 300《Friction and force balance》，页脚没有列出（**本次观察**） |
| 部分平面图没有指北针 | low 场景和开场天气图没有调用 `compass()` | 默认上北下南，不违反 AGENTS §6，只是前后不统一 |
| low 场景把剖面元素画进了平面图 | `scenes-c.mjs:94-111` 在平面等压线图底部画了一排树和房子，表示“地面摩擦” | 这是比喻性装饰；会不会让学生误以为是剖面，**未做视觉评估** |
| 规则符合度（代码中存在） | `brand()`、`outro()`、页脚只写资料来源；画面上没有示意类免责声明 | 符合 AGENTS §4/§5 |
| README | 写的是 3:25 | 与 ffprobe 的 204.7 s 一致 |

预览帧、返工过程、用户反馈都**未记录**。

## 8. 复现入口

以下命令来自 README 和 `render.mjs` 头部注释，本次**均未执行**：

```bash
cd wind-forces
npm install
python3 -m venv .venv && .venv/bin/pip install edge-tts   # fonts.py 另需 fontTools
.venv/bin/python tts.py
node render.mjs --still 110 150 180   # → build/still_<t>.png
node render.mjs                       # → out/风为什么不直奔低气压？三个力的拔河赛.mp4
```

修改入口：等压线数值 `props.mjs:55`；力的比例 `LG` `scenes-c.mjs:9`；斜交角 `A_END` `scenes-c.mjs:93`；螺线角 `ALPHA` `scenes-d.mjs:88`；转盘 `scenes-b.mjs:10-11`。

## 9. 参考来源

项目文件里**没有 URL**（`urls: []`）。下表是文件中提到的资料名称，本次**均未访问**：

| 资料 | 用途 |
| --- | --- |
| 人教版地理必修第一册 印刷第 38—39 页 | 三力、高空风与近地面风的教材表述 |
| Roland Stull《Practical Meteorology》Ch.10 | 力的平衡、地转风 |
| PSU METEO 300《Friction and force balance》 | 摩擦与三力平衡、斜交角不固定 |
| `geo-story-studio/storage/builds/douyin-wind-forces/v002/`（推断路径） | 分镜与 review-001 审校 |

## 10. 证据与限制

| 文件 | 行数 | sha256[:12] | 读取程度 |
| --- | --- | --- | --- |
| `wind-forces/script.json` | 62 | `dd64d86dfa42` | 全文 |
| `wind-forces/scenes-a…e.mjs` | 170/226/182/157/66 | `2f70d3b9bfab`/`db1b38850e36`/`d26c54375195`/`c3dbbafea490`/`46887d0a61d4` | 结构全部；关键场景（天气图、grad、转盘、hemi、high、low、exam、swirl）的代码 |
| `wind-forces/props.mjs` | 163 | `a7207e76892c` | 等压线、`force`、`vel`、`compass` 全文 |
| `wind-forces/cast.mjs` | 218 | `b124aab40437` | 导出与注释 |
| `wind-forces/render.mjs` / `lib.mjs` / `tts.py` | 193/220/209 | `a3502ad7abea`/`be04193b164a`/`d70b4db40056` | 与 sky-colors / air-pressure 版 diff |
| `wind-forces/audio.mjs` / `fonts.py` | 204/37 | `f9e04b2cfd54`/`a6ac7264e548` | 与其他几部片相同 |
| `wind-forces/build/timeline.json` | — | — | 读取各段 start |
| `wind-forces/out/*.mp4` / `*.srt` | — | 31,380,596 B / 5,882 B | 只看 ffprobe 结果和 SRT 条数 |

限制：没有播放视频，也没有试听；受力关系是读代码和代数核对得出的，未做视觉检查；依赖版本、渲染耗时未记录。
