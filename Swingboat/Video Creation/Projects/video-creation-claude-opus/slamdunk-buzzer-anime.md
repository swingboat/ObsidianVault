---
type: video-production-dossier
project: video-creation-claude-opus
video_id: slamdunk-buzzer-anime
captured: '2026-11-12'
status: 已有 30 秒 MP4（推断为 v4 原创角色版：海鸣 #8 白石的压哨三分，EEVEE 卡通渲染）；v2 湘北造型仅存 6 张样片，VRM/外部模型接入代码存在但未提供模型
source_fingerprint: 3363b70542f082f4c4993ec6bde07e336afe6974b617792cde1437af2169ca8a
tags:
- video-creation
- production-capture
- blender
- eevee-toon
- procedural-character
---

# 压哨三分 · 动漫版（海鸣高中）· EEVEE 卡通渲染 + 程序化蒙皮动漫角色

<!-- capture-source-sha256:3363b70542f082f4c4993ec6bde07e336afe6974b617792cde1437af2169ca8a -->

> [!note] 归档说明
> 本档案为 2026-11-12 对源项目的只读回溯。**未找回制作对话，结论来自项目文件**（`prompt.md` 记录的 v2–v4 需求、`plan/`、源码、`samples/` 样片、`output.mp4` 媒体头）。本轮未运行任何构建、渲染或音频脚本，未观看成片；只查看了 1 张样片 `samples/05-rukawa.png` 用于判断样片所属版本。git 只有两次整体提交，`build/` 已清空，`characters/` 为空目录。

## 与 rukawa-buzzer-beater 的关系（两条路线）

本目录是 [[rukawa-buzzer-beater]]（v1）的**后续迭代**，按 `prompt.md` 记录共经历 v2、v3、v4 三次需求变化。两份档案证据分开记录：本档案只写本目录内的文件；v1 的细节请看对应档案。

| | v1 rukawa-buzzer-beater | 本目录 slamdunk-buzzer-anime |
|---|---|---|
| 触发 | 原始需求：review 参考帖、用同样技术做 30 秒压哨三分 | 用户对 v1：「挺好的，只是人物不够逼真，还有办法做得更逼真吗？」 |
| 视觉路线 | 写实光照 + 手办质感人物（Cycles 路径追踪） | 《THE FIRST SLAM DUNK》式「三维 + 卡通渲染」（EEVEE） |
| 人物技术 | 基本体 FK 对象层级 | Armature + Skin 修改器烘成的连续蒙皮网格、射线投影脸部贴片、贝塞尔发束；另写了 VRM/PMX/FBX/glTF 通用导入驱动 |
| 角色身份 | 流川枫 #11 / 樱木 #10 / 山王 #9 | v2：湘北流川枫 #11 等（仅存样片）→ v4 当前代码：原创角色 白石 #8、大地 #5（海鸣）vs 黑泽 #10（白峰） |
| 场所 | 夜场体育馆、LED 记分牌、闪光灯 | 下午的高中体育馆：高窗斜阳 + 体积光、舞台幕布、吊挂篮架、墙上计时器、手翻记分牌；无闪光灯 |
| 节奏 | 全部逐帧（ones） | 人物「一拍二」（on twos），镜头与球仍逐帧 |
| 直接复用 | — | `scene/common.py`、`audio/voices.py`、`post.sh` 与 v1 字节相同；`motion/poses/ball/net/cameras/synth` 为 v1 的小改版本（本次用 diff 核对） |

## 1. 当前结论与制作路线

**状态：已完成 30 秒成片 `output.mp4`；最可能对应 v4 原创角色版本。**

判断依据（推断）：当前源码 `scene/motion.py:7-14` 的角色为 Ace/Mate/Rival（白石/大地/黑泽、KAIMEI/SHIRAMINE）；`characters/` 为空，`build_scene.py:44-48` 会回退到程序化人物；`output.mp4` 修改时间晚于全部源码与 `prompt.md` 的 v4 段落；而 `samples/` 的 6 张样片时间更早，画面是 SHOHOKU #11 与 SENDOH #7（本次查看 `05-rukawa.png` 确认），属于 v2 阶段。

| 证据等级 | 结论 |
|---|---|
| 代码/配置中存在 | v4 的程序化动漫角色、海鸣体育馆、EEVEE 卡通管线；通用外部模型导入 `scene/humanoid.py`；VRM 检查/预览工具 |
| 日志/缓存显示执行过 | 无法确认（`build/` 已清空）；`samples/*.png`（1920×1080 RGBA）证明 v2 阶段渲染过至少 6 张静帧 |
| 最终采用 | `output.mp4`，推断为 v4（见上） |
| 技术检查通过 | 本轮 ffprobe 规格与代码一致 |
| 视觉检查通过 | v2 样片的视觉检查被用户以「这和动漫中的流川枫差远了」否定；v4 成片视觉检查未记录；本次未看成片 |
| 用户验收 | 未记录。用户对 v2 不满意 → v3；对 v3 回复「先试试方案1（找现成的流川枫同人模型）」→ v4 改为原创角色。v4 成片之后无用户意见 |

**规格**

| 项 | 值 | 来源 |
|---|---|---|
| 分辨率 | 1920×1080，16:9 | ffprobe；`scene/lights.py:112` |
| 帧率 / 帧数 | 24 fps / 720 帧 | ffprobe `nb_frames=720`；`scene/common.py`（与 v1 相同） |
| 时长 | 30.000 s | ffprobe |
| 视频 | H.264 High，yuv420p，约 5.34 Mb/s | ffprobe；`post.sh`（CRF 16，与 v1 相同） |
| 音频 | AAC LC，48 kHz 立体声，约 257 kb/s | ffprobe |
| 文件大小 | 21,019,078 字节（比 v1 小约 31%，推断与卡通平涂画面更易压缩有关） | 盘点 |

**一句话管线**

v1 工程 → 风格方案选择（方案 C：三维 + 卡通渲染）→ `plan/00-architecture.md`、`plan/01-style.md` → 卡通材质组与反向外壳描边 → 骨架 + 蒙皮动漫角色（脸部贴片、发束）→ 体育馆与道具、校服观众 → 复用 v1 的 τ 时间系统与动作/球/网/镜头烘焙（人物改一拍二）→ 样片静帧确认画风 → EEVEE 逐帧渲染 → v1 同款 NumPy/SciPy 合成音轨（加翻牌声、改混响）→ ffmpeg → `output.mp4`。

## 2. 创作目标与需求

`prompt.md` 记录了四个版本的需求（摘录）：

**v1**（指向 `../rukawa-buzzer-beater/prompt.md`）：review 参考帖，用同样技术做 30 秒《灌篮高手》流川枫压哨三分。

**v2**：

> 挺好的，只是人物不够逼真，还有办法做得更逼真吗？
> 我希望最好有“灌篮高手”一样的人物造型（球员，观众），场所，你觉得哪个方案比较适合呢？
> 按建议做（湘北高中体育馆）

采用方案 C：《THE FIRST SLAM DUNK》式「三维 + 卡通渲染」——骨架 + 蒙皮连续网格、动漫比例、动漫脸、成束发型、描边 + 二/三阶色块明暗；湘北体育馆；校服学生观众与横幅；保留 v1 的逐帧烘焙、事件表、NumPy/SciPy 音频；渲染器改 EEVEE（「卡通着色 Shader to RGB 只在 EEVEE 可用」）；先出起跳出手、击掌两个镜头的样片确认画风。方案 A、B 的内容未记录。

**v3**：

> 这和动漫中的流川枫差远了
> 按最推荐做

改由用户用 VRoid Studio 手工捏人 → 导出 VRM → 接入现有骨骼姿态；`characters/rukawa.vrm`、`sakuragi.vrm`、`sendoh.vrm`，缺哪个就回退程序化人物；安装 Blender 官方扩展「VRM format」。姿态换算：VRM 静止为 T 字，局部旋转 = B⁻¹·A_父⁻¹·R·A·B。

**v4**：

> 好，我们先试试方案1（找现成的流川枫同人模型）→ 按你能做的做

「不复刻既有版权角色的造型」，改为：
- A. 原创角色与原创学校：白石 #8 / 大地 #5（海鸣高中）vs 黑泽 #10（白峰高中），剧情不变；程序化人物加手指、瓜子脸、更大的眼睛、原创发型（狼尾、刺发 + 发带、低马尾）。
- B. 通用模型导入 `scene/humanoid.py`：VRM / PMX / FBX / glTF，自动骨骼映射，「仅限有权使用的原创模型」。

> [!info] 版权取舍
> v4 明确从「同人/原作造型」退回到原创角色和原创学校，只保留剧情结构与动画风格方向。外部模型通道只允许有使用权的原创模型。这是本片与 v1 在题材上的最大差异：v1 仍使用流川枫、湘北、山王等名称（但不复制原作画面）。

## 3. 技术与依赖

| 项 | 内容 | 来源 / 证据等级 |
|---|---|---|
| Blender | 版本未在本目录记录（v1 plan 记为 5.2；代码兼容 `BLENDER_EEVEE` / `BLENDER_EEVEE_NEXT` 两种引擎名） | `scene/lights.py:91-97` |
| 渲染引擎 | EEVEE，TAA 32 采样，屏幕空间光追，阴影 2 光线 / 8 步，体积光 0.5–60 m、tile 4、64 采样、体积阴影 32 采样，GTAO；**无运动模糊** | `scene/lights.py:98-116`；`render.sh:14` |
| 色彩 | Standard 视图变换、look None、曝光 0（「颜色不被压」） | `scene/lights.py:117-119` |
| 着色 | 自建节点组 Toon：Diffuse → Shader to RGB → 阈值 → 亮/暗两色 + 边缘光 + 暖色高光带 | `scene/toon.py:18-88` |
| 字体 | 中日文字使用系统字体 Hiragino Sans GB 或 STHeiti Medium（macOS 系统字体，按存在顺序加载）；其余文字为 Blender 内置字体 | `scene/gym_props.py:13-24` |
| Blender 扩展 | VRM format、mmd_tools：仅在 `humanoid.py` 被调用时启用；**成片推断未使用**（无模型文件） | `scene/humanoid.py:52-55` |
| Python 音频 | NumPy、SciPy（版本未记录，`build/.venv` 已清理） | `post.sh:5` |
| ffmpeg | libx264 / AAC | `post.sh` |
| 外部模型 / 贴图 / 音频 | 无（`characters/` 为空） | 目录列表 |
| AI 生成模型 / TTS | 无 | 全部源码 |
| 硬件 | 未在本目录记录（stills.py 仍保留 Metal 设备设置，推断与 v1 同机） | `stills.py:24-28` |

EEVEE 单帧耗时与总渲染时间：**未记录**。

> [!warning] 遗留问题
> `stills.py` 与 `check.sh` 的 `--samples/SPP` 设置的是 `sc.cycles.samples`（`stills.py:18-20`），而工程渲染引擎是 EEVEE，因此该参数对静帧检查不起作用（本次源码阅读结论，未运行验证）。`render.sh` 已正确改用 `sc.eevee.taa_render_samples`。

## 4. 镜头与时间轴

### 4.1 时间系统（与 v1 相同）

`scene/common.py` 与 v1 字节相同：720 帧、24 fps，帧 0 对应故事时间 τ = −3.5 s；`SEGMENTS` 速度段（0–185 实速 → 185–205 渐慢 → 205–312 0.2× → **312–372 定格** → 372–432 0.2× → 432–444 渐快 → 444–488 实速 → 488–498 渐慢 → 498–548 0.3× 击掌 → 548–558 渐快 → 558–720 实速）。关键时刻与 v1 相同：出手 τ4.75、计时归零/蜂鸣 τ5.6（f312）、空心入网 τ5.95、比分 τ5.97。帧号换算见 [[rukawa-buzzer-beater]] 第 4.1 节（为本次按代码自行推算）。

`plan/00-architecture.md` 将「节奏」列为改动项：人物「一拍二」（偶数帧取样、常量插值），镜头连续。实现为 `motion.py:244` `t = tau_of_frame(f - f % 2)`：奇数帧复用上一偶数帧的故事时间，因此人物根节点和骨骼姿态每两帧才变一次。球（`ball.py`）、相机（`cameras.py`）、篮网仍按逐帧 τ 计算；观众动画每 2 帧打一次关键帧（`crowd.py:118`）。

### 4.2 镜头表

镜头划分、帧段、运动函数与 v1 相同（`scene/cameras.py`，diff 只有 4 行），差异只在开头与结尾的取景目标：

| # | 帧 | 内容 | 与 v1 的差异 |
|---|---|---|---|
| C1 | 0–84 | 开场摇到计时器 | 机位 (2,−5.5,1.2)→(6.5,−4,2.4)，目标从 (10,10,9.5) 摇到 +X 端墙上计时器 (18.8,0,7.2)，焦距 22→40 mm（`cameras.py:49-50`）；v1 摇向悬挂 LED 记分牌 |
| C2 | 84–185 | 低机位侧向跟拍运球突破 | 同 v1（目标改为 `M.ace`） |
| C3 | 185–262 | 正前低角度仰拍起跳出手 | 同 v1 |
| C4 | 262–312 | 侧后方追拍空中球 | 同 v1 |
| C5 | 312–372 | 定格环绕空中球 | 同 v1 |
| C6 | 372–444 | 篮下仰拍刷网 | 同 v1 |
| C7 | 444–520 | 中景主角 + 搭档跑入 | 同 v1 |
| C8 | 520–580 | 击掌侧面特写慢动作 | 同 v1 |
| C9 | 580–720 | 后拉上升 | 终点 (−2,−10.5,5.6)、焦距 35→30 mm，最后看向墙上计时器（`cameras.py:104-105`）；v1 拉得更远更高（体育馆更小，推断为避免穿出墙/看台） |

`samples/` 的 6 张样片文件名对应镜头：`01-gym-timer`（C1）、`02-drive`（C2）、`03-jumpshot`（C3）、`04-swish`（C6）、`05-rukawa`（C7）、`06-high-five`（C8）——为 v2 阶段的画风确认图（只查看了 05，画面为 SHOHOKU #11 与右侧入画的 SENDOH #7 蓝色球衣）。

### 4.3 角色（当前代码，v4）

`scene/motion.py:7-14` + `scene/character.py:13-18`：

| 键 | 角色 | 身高 | 球队 / 球衣（亮 / 暗） | 发型 | 眼型 / 瞳色 |
|---|---|---|---|---|---|
| Ace | 白石 SHIRAISHI #8 | 1.87 m | 海鸣 KAIMEI，#b3122e / #7c0e2a，白边 | `wolf` 狼尾，#33384a / #151826 | `cool` / #3c6688，有护腕 |
| Mate | 大地 DAICHI #5 | 1.86 m | 海鸣 KAIMEI | `spiky_band` 刺发 + 白发带，#f08a24 / #b0501c | `fiery` / #4a2c18，肤色 #f1c19c |
| Rival | 黑泽 KUROSAWA #10 | 1.90 m | 白峰 SHIRAMINE，#1f2d5c / #121a3a，银边 | `ponytail` 后梳低马尾，#5a4636 / #2e2220 | `easy` / #4a3a2a |

动作路径、姿态时序与 v1 的流川 / 樱木 / 防守者一一对应（只改名）。`poses.py` 有两处差异：`REST` 中上臂外展由 10° 清零（网格本身已做 A 字 10°，见 `plan/01-style.md`），出手姿态 `RELEASE` 的左臂改为 `(138, 40, 0)` 并减小前臂弯曲。

> [!warning] 文档与代码不一致
> `plan/00-architecture.md` 的文件表仍写 `hair.py` 提供「rukawa / sakuragi / sendoh / 学生发型」、`lights.py` 含「合成辉光」；当前代码的发型是 `wolf / spiky_band / ponytail`（`hair.py:129`），`lights.py` 没有合成器辉光。`prompt.md` v3 提到的 `scene/vrm_char.py`、`tools/preview_vrm.py` 不存在，实际文件为 `scene/humanoid.py`、`tools/preview_model.py`（v4 B 方案的通用化版本，推断取代了 v3 的 VRM 专用实现）。`plan/01-style.md` 写室内 38×26 m，代码 `gym.py:9` 为 x −21…19（40 m）× y ±13（26 m）。

### 4.4 场景：海鸣高中体育馆（`plan/01-style.md` + `scene/gym.py`、`gym_props.py`）

- 尺寸：内部 x −21…19、y ±13，檐高 11 m、屋脊 14 m；二楼看台 z 4.0、深 1.6 m；高窗 z 5.2–9.6（`gym.py:9-12`）。
- 地板：Brick 纹理做木条（#e2b77c / #d9ab70），白色篮球线、黄色排球线、绿色羽毛球线（`gym.py:98-121`）。
- −X 端舞台：木台、酒红幕布 #9a1f33、「海鳴高等学校」金字牌匾（`gym_props.py:57-85`）。
- 篮架：白色背板红框、橙色篮圈（`gym_props.py:87-123`）。
- +X 端墙上计时器：海鳴 / 白峰、78→81 / 80，逐 0.1 s 切换的红/琥珀色数字（`gym_props.py:125-153`）。
- 记录台手翻记分牌：铰链空物体在 τ5.97+0.6 后 6 帧内绕 X 轴翻转 −180°，露出 81（`gym_props.py:168-192`），并写入 `flip` 事件触发翻牌声。
- 观众（`crowd.py`）：二楼两侧两排、舞台边 14 人、场边 5 名队员；5 种变体（立领黑、白衬衫、藏青水手服 ×2、球队红）；横幅「海鳴」「一球入魂」「白石 ファイト！」「必勝」「白峰」「大地 魂」。

## 5. 关键算法与实现

本片同样是叙事短片，没有讲解知识点；以下均为制作算法，来自源码阅读，未运行验证。与 v1 相同的部分（τ 时间重映射、抛体解算投篮、入网后物理、每帧形态键篮网、事件表）不再重复，见 [[rukawa-buzzer-beater]] 第 5 节。

### 5.1 卡通着色节点组（`scene/toon.py:18-88`）

```
Diffuse BSDF → Shader to RGB → RGB→BW 得到受光亮度 L
lit  = MapRange(L, 0.40→0.44 ⇒ 0→1)          # 近似硬阈值，留 0.04 的软边防锯齿
base = mix(Shadow, Color, lit)                 # 二阶色块
rim  = MapRange(LayerWeight.Facing, 0.74→0.78) × Rim × lit
c1   = Screen(base, 暖白 (1,0.97,0.9), rim)     # 只在受光侧的细边缘光
hi   = MapRange(L, 1.6→1.7 ⇒ 0→0.28)           # 第三阶：直射阳光斑块
c2   = Screen(c1, 暖黄 (1,0.88,0.62), hi)
out  = mix(Emission(c2), Glossy(0.18), Gloss)
```

- 暗部色默认 = 亮部色 × (0.62, 0.58, 0.74)（偏冷紫，`toon.py:6,102-103`）；角色等关键材质手工指定暗部色。
- 用 Emission 输出最终颜色，使明暗只由阈值决定、不再受二次光照影响。Shader to RGB 只在 EEVEE 可用，这是改用 EEVEE 的直接原因（`prompt.md` v2）。

### 5.2 反向外壳描边（`toon.py:137-171`）

每个需要描边的网格追加 Outline 材质槽（#1a1418 自发光、背面剔除），再加 Solidify：`offset=1`、`use_flip_normals=True`、`material_offset` 指向描边材质、`thickness_clamp=0.6`。外壳法线朝内、正面被剔除，只剩轮廓处的背面可见，形成线条。厚度：角色身体 0.010·k、头 0.006·k、头发 0.005·k、短裤 0.008·k、球 0.006、道具 0.012–0.02、观众 0.012（`plan/01-style.md` 写观众 0.015，代码为 0.012）。

### 5.3 蒙皮动漫身体（`scene/body.py`）

1. **骨架**（`:25-65`）：17 根骨骼，按 1.87 m 标准身高的单位坐标定义（骨盆 1.00、胸 1.27、颈根 1.52、头 1.60–1.86，肩 x ±0.20、z 1.47，手臂 A 字 10°），乘 `k = 身高/1.87`。
2. **连续网格**（`:69-135`）：用「节点 + 半径」图描述身体（躯干 6 节、每臂含 4 指 × 3 节 + 拇指 3 节、腿含膝/小腿肚/踝/脚尖/脚跟），交给 Skin 修改器生成管状网格，再 3 级细分，最后 `new_from_object` 烘成普通网格并清除修改器。这样一张连续网格自然带有分叉的手指和平滑关节。
3. **权重**（`:139-167`）：对每个顶点计算到每根骨骼线段的距离，再除以该骨骼的「肢体粗细」`BONE_R`；取最近 3 根中距离不超过最近者 1.45 倍的骨骼，权重 ∝ `1/d^8` 后归一化。高次幂让关节处过渡很窄，避免卡通角色常见的「软面条」变形。
4. **服装分区**（`:171-269`）：不切分网格，而是把静止高度 `rest_z`、`|x|`、前/后、手臂/脚/手腕权重写成顶点属性；材质中用一串 `MapRange`（±0.003 软边）从下到上叠出 背心（U 领 + 肩带 + 袖窿）、裤腰边、短裤、裤脚边、皮肤、袜、鞋、鞋底、护腕。每一区有亮/暗两色，分别接入 Toon 组的 Color、Shadow。
5. **球衣文字**：号码、队名、姓氏先建文字曲线 → 转网格 → 骨骼父子到胸 → Shrinkwrap 贴到一个**只带 Armature、不可见的身体孪生体**（`character.py:111-118,131-154`），这样文字跟随动作贴身，又不会贴到描边外壳上。
6. **短裤裤管**：独立的三圈喇叭管 + Solidify，父子到大腿骨（`character.py:70-84`）。

### 5.4 动漫头部与脸（`scene/head.py`）

- **头型**（`:18-54`）：40×28 的单位球，下半部 `x *= 1 − 0.48·t`、后脑与下颌分别收窄（`t = (−z)^1.3`），前方中线加一条正弦隆起做鼻梁；尺寸 0.090 × 0.106 × 0.124 m（×k），形成尖下巴的「瓜子脸」。
- **脸部贴片**（`:72-150`）：眼白、虹膜、高光、上眼线、下眼线、眉、鼻、嘴都在 2D (x, z) 平面上画成多边形或「渐变粗细线条带」，再从正前方沿 +Y 向头部做 `ray_cast` 投影，落点沿法线外推 0.7 mm + 层号 × 0.5 mm 防止 Z 冲突，然后父子到头部。三种眼型参数（`EYES`，`:10-15`）：

| 眼型 | 眼睑弧度 | 睁开高度 | 虹膜半径 | 外眼角上挑 | 眉角 | 眉粗 |
|---|---|---|---|---|---|---|
| cool（白石） | 0.005 | 0.015 | 0.012 | 0.007 | 0.10 | 0.0035 |
| fiery（大地） | 0.008 | 0.019 | 0.011 | 0.010 | 0.28 | 0.0065 |
| easy（黑泽） | 0.006 | 0.014 | 0.011 | −0.003 | −0.05 | 0.0035 |

### 5.5 成束发型（`scene/hair.py`）

- **发帽**（`:13-33`）：72×44 椭球，按「前额 → 两侧 → 后颈」逐渐下降的发际线只保留其上方的面；三种发型分别给前发际高度 0.30 / 0.42 / 0.5。
- **发束**（`:36-56`）：每束沿二次贝塞尔曲线（根、控制点、尖）扫掠一个 6 边透镜形截面，宽度按 `(1−t)^0.75`、厚度按 `(1−t)^0.6` 收尖；截面法向取「离头中心向外」与切线正交化。
- 三个风格函数用 `random.Random(名字字符和)` 生成可复现的细节；发带为单独环面（`:132-149`）。发束与发帽合并成一个网格，统一描边。

### 5.6 姿态到骨骼的换算

- **程序化角色**（`character.py:21-41`）：v1 的姿态角是「与世界轴对齐的父空间欧拉角 R」。骨骼静止朝向矩阵 B（armature 空间 3×3）下，骨骼局部旋转 = **B⁻¹·R·B**（共轭），所以 v1 `poses.py` 的姿态库和符号表可以原样复用。`point()/dir()` 把「v1 对象约定」（肢体沿 −Z、面向 −Y）下的局部向量换算到世界，供 `ball.py` 计算手的位置。
- **外部模型**（`humanoid.py:139-181`）：骨骼命名可能与本项目不同、层级也可能不同（MMD 的腿挂在「下半身」下）。对每个映射到的关节 j，静止校正 A_j 把骨骼静止方向转到 −Z（四肢），F_j 是按本项目层级累乘得到的世界对齐帧，ap 是 j 在真实骨架中最近的「已映射祖先」：
  `local = B_j⁻¹ · A_ap⁻¹ · (F_ap⁻¹ · F_j) · A_j · B_j`
  另外按左上臂的 x 符号判断模型朝向，必要时整体加 π 偏航（`rig.yaw`，`motion.py:251` 读取）。
- **自动骨骼映射**（`humanoid.py:64-86`）：优先读 VRM 1.0 / 0.x 人形骨骼表，其次 MMD 日文骨名（下半身、上半身、首、頭、左腕、左ひじ……），最后把骨名归一化（去前缀 `J_Bip_`、`DEF-`、`mixamorig:` 等，去非字母数字）匹配 Mixamo、Rigify、Unity 人形常用名。导入后静音 IK 约束、按包围盒高度缩放到目标身高、非 VRM 材质转为 Toon 组并加描边（`:104-136,184-224`）。

### 5.7 光照与体积光（`scene/lights.py`）

- 太阳光 5.5、角直径 0.6°、颜色 #ffe3bd，从 +Y 侧以约 52° 斜射，穿过高窗形成光柱与地面窗影（`:24-27`）。
- 顶灯 5×3 盘形面光，各 150 W，带发光灯罩；背阴侧两扇敞开的推拉门用自发光平面 + 120 W 面光模拟室外反光（`:28-46`）。
- 世界：天空色 #bfdcf4，用 Light Path「Is Camera Ray」把强度映射为 0.3（间接）→ 1.0（直视），使窗外天空明亮而室内环境光克制（`:49-63`）。
- 体积雾：覆盖整个室内的盒子，Volume Scatter 密度 0.006（`build_scene.py --haze` 可改）、各向异性 0.35（`:64-80`），配合 EEVEE 体积阴影形成光柱。

### 5.8 观众

4 种校服变体 + 1 种球队服，每种由胶囊拼出的 6 个部件原型（腿、下装/裙、躯干、头、头发、手臂）+ 眼睛点；每个观众是一个空物体根节点，部件是共享网格数据的对象，并逐个复制 Solidify 描边参数（`crowd.py:54-112`）。动画每 2 帧一个关键帧：入网后按随机延迟起跳（幅度 0.16 m）、双臂上举 + 挥动；τ3–4 起按 cheer 系数（看台 0.8、舞台边 0.6、场边队员 0.3）先半举手助威（`:115-128`、`:143-175`）。不同于 v1，**没有相机闪光**，因此音频中的快门声不会出现（`flash` 事件在本目录代码中不存在）。

## 6. 声音与字幕

- **配音 / TTS / 字幕：无。**
- 音频生成器 `audio/voices.py` 与 v1 字节相同；混音脚本 `audio/synth.py` 与 v1 只有两处差异（本次 diff 核对）：
  1. 新增 `flip` 事件（手翻记分牌）：0.14 s、1.2–6 kHz 的 `swoosh` + 0.12 s 后的高音 `slap(1.6)`，声像偏左 −0.3（`synth.py:92-94`）。事件位置 (0.38, −8.84, 1.0)，时刻 τ5.97+0.6（`build_scene.py:61`）。
  2. 混响缩短以匹配较小的学校体育馆：主混响 RT60 2.3→**1.6 s**（IR 长 2.4 s），低频滚动尾音 3.6→**2.8 s**（`synth.py:148-149`）。
- 其余与 v1 相同：距离声延迟 `d/343` 加在故事时间上、慢动作按速度降调、蜂鸣前 riser + 次低频 boom + 蜂鸣器、心跳、人群低语/欢呼床（慢动作时低通闷住）、球后旋呼啸、A 小调张力 → C 大调明亮的合成和弦垫、`tanh` 软限幅与淡入淡出。机制细节见 [[rukawa-buzzer-beater]] 第 6 节。
- 蜂鸣/比分事件位置改为墙上计时器 (18.8, 0, 7.2)（`build_scene.py:58-59`），但 `synth.py` 跳过这两类事件、在固定帧 312 放蜂鸣，所以位置变化不影响声音。
- 本目录没有 `flash` 事件，快门声循环不会产生声音（推断，基于代码中无 `flash` 事件来源）。
- 响度、听感检查：未记录。

## 7. 迭代、问题与修正

| 阶段 | 用户反馈 / 触发 | 处理 | 留存证据 |
|---|---|---|---|
| v1 → v2 | 「挺好的，只是人物不够逼真」「希望最好有灌篮高手一样的人物造型（球员，观众），场所」 | 给出多个方案，采用方案 C（三维 + 卡通渲染），重写人物/场馆/观众/灯光，复用 v1 的时间、动作、球、网、镜头、音频；渲染改 EEVEE | `prompt.md` v2、`plan/00-architecture.md`、`plan/01-style.md`；`samples/01–06`（v2 画风确认静帧） |
| v2 样片 | 「这和动漫中的流川枫差远了」「按最推荐做」 | 改由用户在 VRoid Studio 捏人导出 VRM，程序接入；写 VRM 检查工具 | `prompt.md` v3；`tools/inspect_vrm.py`。`characters/` 为空，无 VRM 文件留存，未确认用户是否捏过人 |
| v3 → v4 | 「先试试方案1（找现成的流川枫同人模型）」「按你能做的做」 | 不复刻既有版权角色：A 改为原创角色与原创学校，并升级程序化人物（手指、瓜子脸、大眼、原创发型）；B 写通用外部模型导入（仅限有权使用的原创模型） | `prompt.md` v4；`scene/motion.py` 新 SPECS；`scene/humanoid.py`；`tools/preview_model.py` |
| v4 成片 | — | 程序化人物渲染成片（推断） | `output.mp4`（时间晚于全部源码） |

从 v1 迁移时的技术改动（摘录自 `plan/00-architecture.md`「改动」表，并与代码核对）：

- 渲染 Cycles → EEVEE，Standard 视图变换，关闭运动模糊（卡通画面不需要，也与一拍二节奏冲突，后半句为推断）。
- 着色 Principled → Toon 节点组 + 反向外壳描边。
- 人物 胶囊 FK 对象层级 → Armature + Skin 烘焙网格 + 距离权重。
- 头部 椭球 + 点眼 → 尖下巴头型 + 射线投影脸部贴片；头发 圆锥 → 二次贝塞尔发束。
- 记分 LED 记分牌 → 墙上数字计时器 + 手翻记分牌（翻牌动画）。
- 姿态库：上臂 `REST` 外展清零，避免网格自带的 A 字与姿态外展叠加（`poses.py:23`）。

未记录：v2 样片具体存在哪些画面问题、v4 期间的静帧检查轮次、渲染耗时、有无失败重渲。

## 8. 可复用制作经验

1. **风格大改时保留「时间—动作—镜头—声音」骨架，只换外观层。** v2 直接复制 v1 的 `common.py`、`voices.py`、`post.sh`，`motion/ball/net/cameras` 只改名和少量参数，所有节奏、镜头、声画同步无需重做。依据：本次 diff；`plan/00-architecture.md`「不变的部分」。
2. **姿态库用「世界对齐父空间欧拉角」定义，换骨架时用共轭 B⁻¹RB 转换。** 同一套姿态既能驱动 FK 对象层级、自建骨架，也能扩展到外部模型（再加静止校正 A 与最近已映射祖先）。依据：`scene/character.py:21-33`、`scene/humanoid.py:1-8,158-169`。
3. **卡通渲染的三件套：Shader to RGB 阈值、反向外壳描边、Standard 视图变换。** AgX/Filmic 会压低饱和度，卡通色块应走 Standard。依据：`scene/toon.py`、`scene/lights.py:117`。
4. **服装分区用顶点属性 + 材质阈值，不切网格。** 衣服边界可以在材质里调，一张连续网格保证蒙皮无缝。依据：`scene/body.py:171-269`。
5. **贴在变形体表面的文字/贴花，Shrinkwrap 到一个不可见的「只蒙皮不描边」孪生体。** 避免贴到描边外壳上。依据：`scene/character.py:111-118,150-154`。
6. **「一拍二」只作用于角色，镜头保持逐帧。** 一行 `tau_of_frame(f − f % 2)` 就能得到动画质感，同时镜头运动依然顺滑。依据：`scene/motion.py:244`；`plan/00-architecture.md`。
7. **先出几张关键镜头样片给用户确认画风，再渲全片。** 本片 v2 正是在样片阶段被用户否定，避免了整片 EEVEE 渲染的浪费。依据：`prompt.md` v2「先出两个镜头的样片静帧」、v3 用户反馈、`samples/`。
8. **涉及既有 IP 角色时，提前约定「原创角色 + 可替换外部模型通道」。** 本片在 v4 才转为原创角色，若一开始就把角色身份与动作工程解耦，可减少返工。依据：`prompt.md` v4、`build_scene.py:44-48`。

## 9. 复现入口

记录的命令与顺序（**本轮未执行**）：

```sh
cd slamdunk-buzzer-anime

# 0) 可选：角色造型预览（三人摆 RELEASE / FIST / DEFEND 姿态，EEVEE 1280×720）
blender -b --factory-startup -P preview_char.py          # → build/preview/char_front|face|three.png

# 0') 可选：外部模型（仅限有使用权的原创模型）
blender -b -P tools/inspect_vrm.py -- <模型.vrm>          # 打印骨骼与 VRM 人形映射
blender -b -P tools/preview_model.py -- <模型> [前缀]      # → build/preview/<前缀>_*.png
#    放入 characters/shiraishi|daichi|kurosawa.(vrm|pmx|fbx|glb|gltf) 后，构建时自动替换程序化人物

# 1) 构建工程 + 检查静帧（BUILD_ARGS 可传 --nocrowd、--haze 0.006、--pct、--samples）
BUILD_ARGS="--nocrowd" ./check.sh 190 312 538            # 帧号为本次举例 → build/anime.blend、events.json、stills/

# 2) EEVEE 全片（可分段、可断点续渲）
PCT=100 SPP=32 ./render.sh [start end]

# 3) 合成音轨 + 成片
./post.sh                                                # → build/audio.wav → output.mp4
```

需要先恢复：Blender（EEVEE；版本未记录）；`build/.venv`（NumPy、SciPy）；ffmpeg；macOS 系统字体 Hiragino Sans GB 或 STHeiti（否则中日文字形可能缺失，推断）；使用外部模型时需 VRM format / mmd_tools 扩展。`build/` 中间产物全部需重新生成。

注意：`check.sh` 第 6 行 `|| true` 会吞掉构建错误，只靠 grep 日志中的 `Error|line N` 发现问题；静帧采样参数对 EEVEE 无效（见第 3 节）。

改参数的入口：角色外观 `motion.SPECS`、`character.TEAMS`、`head.EYES`、`hair.STYLES`；卡通阈值与暗部色调 `toon.toon_group`、`toon.SHADOW_TINT`；描边粗细 `outline(..., thickness)`；阳光与体积雾 `lights.build(sun_strength, lamp_power)`、`--haze`；体育馆尺寸 `gym.X0/X1/Y1/EAVE/RIDGE`；节奏与镜头同 v1。

## 10. 参考来源

| 链接 / 作品 | 类型 | 借鉴内容 → 本片对应 | 访问状态 |
|---|---|---|---|
| https://x.com/guanmo_ai/status/2118258208045494688（X / 观默 @guanmo_ai《瓶中风暴》） | 创作参考（技术方法），经由 v1 间接继承 | 逐帧解算烘焙关键帧、事件驱动声画、NumPy/SciPy 合成音频、原生 .blend 工程 → 本片「保留 v1 的工程方法」 | 2026-11-12 web_fetch 成功读取帖子文字（记录见 v1 档案）；视频未观看 |
| 《THE FIRST SLAM DUNK》 | 风格参考（仅名称，见 `prompt.md` v2「方案 C」） | 「三维 + 卡通渲染」路线 → 第 5.1–5.5 节 | 无链接；未查看任何画面，具体借鉴点仅限 `prompt.md` 文字 |
| 《灌篮高手》原作 | 题材 / 角色参考（v2 阶段） | v2 的湘北、流川枫等造型；v4 起改为原创角色与原创学校 | 无链接 |
| VRoid Studio、Blender 扩展「VRM format」、mmd_tools | 工具（v3/v4 方案） | 外部模型导入路线 | 仅在 `prompt.md` / 代码中出现；成片推断未使用 |

无素材来源链接。

## 11. 证据与限制

**实际读取的文件**（项目相对路径，sha256 前 16 位取自盘点指纹）：

| 文件 | 行数 | sha256 前 16 | 读取方式 |
|---|---|---|---|
| `slamdunk-buzzer-anime/prompt.md` | 41 | b1bca3355765875e | 全文 |
| `slamdunk-buzzer-anime/plan/00-architecture.md` | 39 | 772d585121cb66b5 | 全文 |
| `slamdunk-buzzer-anime/plan/01-style.md` | 30 | ae61e6bbb85bbe0b | 全文 |
| `slamdunk-buzzer-anime/build_scene.py` | 75 | b4eeef7d030b0fab | 全文 |
| `slamdunk-buzzer-anime/preview_char.py` | 72 | ae3668e49e513717 | 全文 |
| `slamdunk-buzzer-anime/stills.py` | 37 | 6eaafe827e2c0b7f | 全文 |
| `slamdunk-buzzer-anime/check.sh` / `render.sh` / `post.sh` | 9 / 18 / 10 | ef87ce7c6c7877a5 / 71f17873b67e9dfc / 65e14fc52d0f64ca | 全文 |
| `slamdunk-buzzer-anime/scene/toon.py` | 171 | 40d1b8e5be5526a3 | 全文 |
| `slamdunk-buzzer-anime/scene/body.py` | 269 | a8aa1a9fbc931c33 | 全文 |
| `slamdunk-buzzer-anime/scene/character.py` | 155 | 40a80c43226b98b1 | 全文 |
| `slamdunk-buzzer-anime/scene/head.py` | 150 | 651f673acd2d191f | 头型、投影、`face()`；`stroke/ellipse/arc_pts` 辅助函数未逐行读 |
| `slamdunk-buzzer-anime/scene/hair.py` | 165 | c005faed0bfe5166 | 发帽、发束、发带、`build()`；三个风格函数内部参数未逐行读 |
| `slamdunk-buzzer-anime/scene/humanoid.py` | 224 | dcef904ea8341433 | 全文 |
| `slamdunk-buzzer-anime/scene/lights.py` | 123 | 51b33c4942e3fe53 | 全文 |
| `slamdunk-buzzer-anime/scene/gym.py` | 195 | 26b4d246ff109111 | 常量、地板与球线；墙/窗/看台/屋架只看了函数结构 |
| `slamdunk-buzzer-anime/scene/gym_props.py` | 202 | a8fa642ca053eac3 | 字体、舞台、篮架、计时器、翻牌；部分几何细节未逐行读 |
| `slamdunk-buzzer-anime/scene/crowd.py` | 175 | ef1385aa879684f4 | 变体、原型、人物、动画、布置、横幅 |
| `slamdunk-buzzer-anime/scene/motion.py`、`poses.py`、`ball.py`、`net.py`、`cameras.py` | 261 / 139 / 190 / 118 / 150 | e5d454781cc84f66 / f0340e56d3822fe6 / 7f75705e6fce82db / 57ab9dba04949467 / 468f77937168ecad | 与 v1 对应文件做 diff，差异全部阅读 |
| `slamdunk-buzzer-anime/scene/common.py`、`audio/voices.py` | 223 / 168 | eb16d3d7e995ff56 / 27aa120374920ed4 | 与 v1 字节相同（cmp） |
| `slamdunk-buzzer-anime/audio/synth.py` | 159 | 0ccae28c086f8dff | 与 v1 diff |
| `slamdunk-buzzer-anime/tools/inspect_vrm.py`、`tools/preview_model.py` | 35 / 60 | 1e0e307bb5736b1d / bb9b24d11e3094a1 | 全文 |
| `slamdunk-buzzer-anime/samples/05-rukawa.png` | — | 未计算 | 查看画面（v2 样片）；其余 5 张只读文件头（1920×1080 RGBA） |
| `slamdunk-buzzer-anime/output.mp4` | — | 未计算 | 仅 ffprobe 媒体头（21,019,078 字节） |

**缺失 / 无法确认**

- 成片对应版本：v4 原创角色为推断（基于源码现状与文件时间），未看成片画面。
- v2 阶段的源码（湘北造型、rukawa/sakuragi/sendoh 发型、Sendoh 对手）已被 v4 覆盖，仅剩样片；v3 的 `vrm_char.py`、`preview_vrm.py` 不存在。
- 构建/渲染日志、`events.json`、`anime.blend`、帧、`audio.wav` 均已清理：观众实际人数、事件数、EEVEE 单帧耗时与总耗时无法核对。
- Blender、NumPy、SciPy、ffmpeg 版本未记录；硬件未在本目录记录。
- 未找回制作对话；用户对 v4 成片无记录的意见；未测响度。
