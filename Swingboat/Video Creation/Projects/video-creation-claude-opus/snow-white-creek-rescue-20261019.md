---
type: video-production-dossier
project: video-creation-claude-opus
video_id: snow-white-creek-rescue-20261019
captured: '2026-11-12'
status: v2 成片已导出（90 s 1080p30，含声音）；作者自评未达影视级质感，无用户验收记录
source_fingerprint: deae83a6eb9eacb90cf872379fe1a3e5a35f8048c83a19da3ecd6c5eef4db7bd
tags:
- video-creation
- production-capture
- threejs-remotion
- blender-mpfb
- procedural-animation
---

# 白雪公主与七个小矮人：溪谷救援 · Three.js + Remotion 逐帧确定性 3D 叙事短片（MPFB 角色 + 程序化 IK 表演）

<!-- capture-source-sha256:deae83a6eb9eacb90cf872379fe1a3e5a35f8048c83a19da3ecd6c5eef4db7bd -->

本次根据项目文件回溯整理。**未找回制作对话，结论来自项目文件**（README、docs/、源码、脚本、检查图和媒体头）。本轮没有运行项目中的 npm、Blender 或音频脚本，也没有重新渲染，只用 `ffprobe` 读取了已有 MP4 和 WAV 的媒体头。下文所有路径都相对于 `videos/snow-white-creek-rescue-20261019/`。

## 1. 当前结论与制作路线

**状态：v2 成片已导出，作者在检查记录中写明“未达到影视级真人质感”。项目中没有用户对 v2 的验收记录。**

| 项目 | 内容 | 证据等级 / 来源 |
|---|---|---|
| 成片 | `out/snow-white-creek-rescue.mp4`，82,025,677 B（约 82 MB） | 最终采用（README「成品」+ 文件存在） |
| 视频流 | H.264 High，1920×1080，30/1 fps，yuv420p，2700 帧，约 7.09 Mbps | 技术检查通过（本轮 ffprobe） |
| 音频流 | AAC LC，48 kHz 双声道，约 189 kbps | 本轮 ffprobe |
| 时长 | 90.005 s（容器），画面 2700 帧 = 90 s | 本轮 ffprobe；与 `docs/04-检查记录.md` 一致 |
| 画幅 | 16:9 全幅，无黑边 | `src/timeline/cues.json` width/height |
| 编码器标记 | `comment=Made with Remotion 4.0.536`，`Lavc61.19.101 libx264` | 本轮 ffprobe 元数据 |
| 混音母带 | `public/audio/final-mix.wav`，PCM s16le 48 kHz 双声道，90.000 s | 本轮 ffprobe |
| 测试片 | `checks/test-clip-pull.mp4`：拉绳段 46.5–52.5 s，1080p30，182 帧（文档写 181），**无音轨**，**仅为动作试片，不是成片** | 本轮 ffprobe `-count_frames` |

**一句话管线**

> 需求（README / 角色设定 / 分镜） → `cues.json` 单一时间轴（18 镜头 + 44 个动作节拍 + 10 句对白） → 素材：Poly Haven CC0（HDRI/扫描纹理/植被，`fetch-assets.mjs`）+ Blender/MPFB 生成 8 个带骨骼与 ARKit 表情的 GLB 角色 + Blender 建模小鹿 GLB → 动作：TypeScript 程序化表演层（按步幅落脚、双骨 IK、拉绳受力、面部形变）→ 声音：`gen-audio.mjs` 用 ffmpeg 合成环境声、音乐和音效，macOS `say` 做中文对白，再用 loudnorm 混音 → 剪辑：`camera.ts` 每个镜头一个函数，在同一个 Three.js 场景里按时间“硬切” → 导出：Remotion `renderMedia`（无头 Chrome + ANGLE WebGL，CRF 23，x264 slow）。

**版本脉络（据 `docs/04-检查记录.md`）**

- **v1（2026-10-19）**：角色是程序化几何体（`src/characters/build.ts` / `parts.ts` / `specs.ts`），贴图用 canvas 程序生成。调色里每帧变化的颗粒加上 CRF 17，让成片约 430 MB（约 39 Mbps）。
- **v2 重制（2026-10-20）**：用户反馈两条——“画面效果远差于 ocean-shallows”“成片几百 MB”。作者把角色换成 MPFB/MakeHuman CC0 写实人体，环境换成 Poly Haven 扫描素材，颗粒改为静态抖动，编码改为 CRF 23。成片降到 82 MB。
- v1 代码保留在 `src/characters/` 中“作参考”，类型和工具函数仍被 v2 引用（`locomotion.ts` 引用 `build.ts` 的 `Rig` 类型；`fawn.ts` 引用 `parts.ts` 的 `sweep`）。

## 2. 创作目标与需求

- **原始需求原文：未记录。** 目录中没有 `prompt.md`。项目根目录除 `.gitignore` 外也没有提到本片，因此用户最初的提示词无法确认。
- README 写明的目标（摘录）：

> 一支约 90 秒、1920×1080、30fps 的童话叙事短片。雨后森林里，白雪公主和七个小矮人合力把被困溪中岩石上的小鹿救回岸边。
> 本工程**完全独立**：源码、依赖、锁文件、字体、脚本、文档、缓存、截图、样片与最终视频都在本目录内。

- `docs/01-角色设定.md` 定下的创作约束（摘录）：

> 造型为本片原创的童话诠释，只保留公版童话角色的识别要素（黑短发、红发饰、蓝上衣、黄长裙、七位矮人的性格名）。

- 叙事结构：建立 → 发现小鹿 → 白雪踏倒木 → 倒木偏移 → 博士判断受力、糊涂蛋找绳 → 系绳、七人拉绳 → 浪涌与喷嚏导致绳滑 → 爱生气抓住、重新发力 → 白雪抱鹿回岸 → 递苹果 → 夕照回家并出片名（`docs/02-分镜.md`）。
- 角色可识别要求：七个矮人分别用脸型、鼻子、眉毛、胡须、体态、帽子颜色、步态和习惯动作区分（见 `docs/01-角色设定.md` 表格）。糊涂蛋没有对白。
- 光照叙事：0–55 s 雨后冷调（约 7000 K）→ 55–74 s 云开、逆光增强 → 74–90 s 金色夕照（约 3500 K）（`docs/02-分镜.md`「光照曲线」）。
- v2 新增的用户要求（项目文件转述，不是原话）：画面质量要接近同仓库的 `ocean-shallows`，成片体积要降下来（`docs/04-检查记录.md`「v2 重制 · 用户反馈」）。

**文档与代码不一致（并列记录）**

| 主题 | 文档 | 代码（当前版本） |
|---|---|---|
| 角色生成 | `01-角色设定.md`：在 `specs.ts` 用参数定义，由 `build.ts` 程序化生成 | v2 运行时加载 `public/models/<id>.glb`（`scene.ts:159-160`，`glbRig.ts`）；`specs.ts` 只提供步态和姿态参数，身体尺寸由 `dims.json` 覆盖（`glbRig.ts:56-69`） |
| 世界坐标 | `02-分镜.md`：小鹿岩石 R(1.15,−0.35)，锚树 T(−6.6,4.3)，推车 C(3.6,6.6)，倒木近端 N(0.6,3.9) | `src/world/layout.ts:7-20`：rock (−1.08,−0.38)，anchorTree (−4.75,4.85)，cart (9.4,6.6)，logNear (0.72,4.7) |
| 喊号子次数 | `02-分镜.md` 镜头 10：“一、二——拉！”×3 | `cues.json` 只在 47.6 s 和 49.6 s 有两句；第三拍 `pull3` 51.8 s 只有动作和绳索音效 |
| 声线处理 | `01-角色设定.md`：Grandpa 降调、Eddy 升调、Reed 鼻音 | `gen-audio.mjs:159-170` 只设置声线和语速（`say -r`），没有变调或鼻音处理 |
| 小鹿毛发 | `build_fawn.py` 文档串：“fur is added at runtime with shells” | `fawnGlb.ts` 只调高粗糙度并加 sheen，没有壳层毛发；`04-检查记录.md` 也写“无毛发系统” |
| 镜头 8 | `02-分镜.md`：中景摇镜 | v2 改为博士近景，37.0 s 在镜头内硬切到糊涂蛋取绳（`camera.ts:79-88`，`04-检查记录.md` v2 修正） |

## 3. 技术与依赖

### 运行与渲染栈（精确版本来自 `package.json` / `package-lock.json`，`.npmrc` 设 `save-exact=true`）

| 组件 | 版本 | 用途 |
|---|---|---|
| remotion / @remotion/cli / @remotion/bundler / @remotion/renderer | 4.0.536 | 合成、Studio 预览、打包、`renderStill` / `renderMedia` 导出 |
| three | 0.186.1 | 场景、PBR、阴影、后期（EffectComposer、UnrealBloom、Bokeh、ShaderPass、OutputPass）、GLTFLoader、HDRLoader |
| react / react-dom | 19.2.0 | Remotion 组件 |
| typescript | 5.9.3 | `npm run typecheck`（`tsc --noEmit`） |
| @types/three / @types/node / @types/react | 0.186.0 / 24.10.1 / 19.2.2 | 类型 |

- `remotion.config.ts`：中间帧格式 `jpeg`、`setConcurrency(1)`、`setChromiumOpenGlRenderer('angle')`。
- `scripts/render.mjs`：`gl: 'angle'`、`headless: true`、`concurrency: 1`、单帧超时 240 s；成片参数为 `codec h264`、`crf 23`、`x264Preset 'slow'`、`pixelFormat yuv420p`、`audioBitrate 192k`。它把 `TMPDIR/TMP/TEMP` 指向 `.cache/tmp`。
- 渲染环境（`docs/04-检查记录.md` 声明）：macOS 27、Apple M3 Pro、Node 24.13、ffmpeg 7.1.1、Remotion 4.0.536（无头 Chrome + ANGLE WebGL）。README 要求 Node ≥ 20。

### 素材生成工具

| 工具 / 资源 | 版本 | 用途 | 证据 |
|---|---|---|---|
| Blender | 5.2（`03-素材清单.md`）；README 写“Blender 5.x” | 无界面批量生成角色、小鹿和树木 LOD | 文档 + `package.json` 的 `assets:*` 脚本 |
| MPFB（MakeHuman for Blender） | 2.0.18（`assets-src/vendor/add-on-mpfb-v2.0.18.zip`） | 基础人体、表型、目标形变、game_engine 骨骼（53 骨含手指） | `build_characters.py:20-22` |
| MakeHuman CC0 资源包 | `makehuman_system_assets_cc0`、`skins01_cc0`、`skins02_cc0`、`system_hair_materials01_cc0`、`faceunits01`、`glasses01_cc0`（zip，本轮未解压） | 皮肤、眼睛、毛发、服装网格、ARKit 表情单元、眼镜 | `assets-src/vendor/` 文件名 |
| Poly Haven API | `https://api.polyhaven.com` | 下载 HDRI、纹理和模型，并写来源清单 | `scripts/fetch-assets.mjs:9` |
| macOS `say` | 系统版本未记录 | 中文对白和喷嚏 | `gen-audio.mjs:151,165` |
| ffmpeg | 7.1.1（文档声明） | 合成全部音频并混音；filmstrip | `gen-audio.mjs`、README |

- **没有使用 AI 图像、视频或 TTS 模型。** 代码中的语音只来自 macOS `say` 系统声线，声线名见第 6 节。
- **CMU 动捕**：`assets-src/mocap/` 中有 15 段 BVH（2010 年 cgspeed 的 MotionBuilder 友好版本），但 `03-素材清单.md` 明确写“当前仅作参考……尚未重定向到角色骨骼”。**下载了，但成片没有使用。**
- **字体**：ZCOOL XiaoWei Regular（`public/fonts/ZCOOLXiaoWei-Regular.ttf`，SIL OFL 1.1，许可证在 `public/fonts/OFL-ZCOOLXiaoWei.txt`），通过 `FontFace` 加载（`Overlay.tsx:64-70`）。
- **实际加载与仅下载的素材**：`fetch-assets.mjs` 下载 4 张 HDRI，但 `world/assets.ts:22` 只加载 `monbachtal_riverbank` 和 `sunset_forest`。`mossy_forest`、`misty_pines` 和 fir/pine 的 LOD GLB 已在磁盘上，**当前未启用**（`03-素材清单.md` 也说明 LOD 未启用）。
- **Git 跟踪范围**：仓库根 `.gitignore` 第 24–29 行忽略 `videos/*/public/assets/`、`public/models/`、`final-mix.wav` 和 `assets-src/vendor/*.zip`。这些文件在本机存在，但不在版本库中，需要用脚本重新生成。

### 质量档位（`src/scene.ts:39-40`）

| 档位 | pixelRatio | 阴影贴图 | Bloom | 景深 | 森林 | 使用的合成 |
|---|---|---|---|---|---|---|
| `QUALITY_HIGH` | 1 | 4096 | 开 | 开 | 开 | `film`（有声）、`film-silent`（无声，用于静帧和试片） |
| `QUALITY_PREVIEW` | 1 | 2048 | 开 | 关 | 开 | `preview`（Studio 预览） |

`Root.tsx` 定义了这三个合成，共用 `cues.json` 的 1920×1080、30 fps 和 2700 帧。

## 4. 镜头与时间轴

单一时间轴来源是 `src/timeline/cues.json`（`seed: 20261019`）。下表的时间来自 `shots`，镜头参数来自 `src/camera.ts`。表演入口指 `src/characters/performance.ts` 的行号。焦距为垂直 FOV（度），光圈值会在 `scene.ts:504` 乘以 0.01 后传给 BokehPass。

| # | 时间 (s) | 镜头名（cues） | 内容 | 机位实现（camera.ts） | 关键参数 | 关键节拍 / 表演入口 |
|---|---|---|---|---|---|---|
| 1 | 0–5.5 | 穿叶建立 | 穿过前景树叶推入溪谷 | `:30-35` | pos (10.5,3.4,12.5)→(7.2,2.3,10.2)，FOV 42→38，光圈 0.14，手持 0.04 | 前景叶片挂在相机上，5 s 内移开（`scene.ts:455-466`） |
| 2 | 5.5–10.5 | 回家路上 | 侧向跟拍一行人 | `:37-43` | x 9.5→6.0 跟随，FOV 40 | yawn 6.4，happyLaugh 7.0，docWait 8.4 |
| 3 | 10.5–14.0 | 白雪听见 | 白雪近景转头 | `:45-49` | 跟随头部，偏移 (0.42,0.02,−1.05)，FOV 36，光圈 0.05 | bleat1 11.0，swTurn 11.3，groupStop 11.6；对白 11.9 |
| 4 | 14.0–19.0 | 越肩发现 | 越肩看小鹿并缓推 | `:51-57` | 从白雪肩后向小鹿推进一半，FOV 40→34 | 水位、流速开始上升（`layout.ts:63-91`） |
| 5 | 19.0–22.5 | 小鹿特写 | 树枝撞岩、小鹿颤抖 | `:59-63` | 小鹿偏移 (−0.7,0.55,1.5)，FOV 40 | bleat2 19.4，branchHit 20.1；溅水事件（`story.ts:100-105`） |
| 6 | 22.5–29.5 | 踏上倒木 | 白雪放篮、踏上倒木 | `:65-70` | 跟随白雪 (2.4,1.3,2.0)，FOV 40→44 | basketDown 23.4，stepOnLog 26.0；倒木表面高度覆盖（`performance.ts:181-200`） |
| 7 | 29.5–35.5 | 倒木偏移 | 低机位上摇，倒木偏移 | `:72-77` | 机位 y 0.35→0.9，手持 0.05→0.09 | logShift 30.2，fawnBack 30.6；白雪平衡（`performance.ts:237`） |
| 8 | 35.5–41.5 | 博士与糊涂蛋 | 博士近景，37.0 s 镜头内硬切到糊涂蛋取绳 | `:79-88` | `k = t>=37 ? 1 : 0` 切换机位和目标 | docSpeaks 36.3，dopeyFind 38.4（`performance.ts:342,368`） |
| 9 | 41.5–47.0 | 系绳与支点 | 前景绳结，后景瞌睡虫蹬树根 | `:90-94` | 跟随 `logPoint(t,0.86,1)`，FOV 44，光圈 0.14 | ropeArrive 41.4，grumpyLine 42.1，knotStart 43.0，knotDone 45.6，sleepyBrace 45.4（`:357,389`） |
| 10 | 47.0–53.0 | 一起拉 | 侧面全景，七人同拉 | `:96-100` | 固定 (4.6,2.2,5.0)→(−1.5,0.9,2.2)，FOV 40 | pull1/2/3 = 47.8/49.8/51.8（`:311-340`） |
| 11 | 53.0–58.0 | 害羞鬼安抚 | 低机位：害羞鬼蹲下，小鹿在后 | `:102-107` | 目标在害羞鬼头部和小鹿之间（0.45 插值） | bashfulLine 54.2，fawnLooks 55.6（`:419`） |
| 12 | 58.0–61.2 | 浪涌喷嚏 | 浪涌，喷嚏精憋喷嚏 | `:109-113` | 手持 0.06 | surge 58.3，sneezeBuild 59.0，sneeze 60.3（`:481-490`） |
| 13 | 61.2–64.2 | 绳滑抓住 | 爱生气扑上去抓绳 | `:115-119` | 固定 (1.25,0.85,5.15) 看爱生气 | slip 60.5，grab 62.3，grumpyShout 62.7（`:327-336`） |
| 14 | 64.2–68.5 | 重新发力 | 全景：重新同拉，白雪抱起小鹿 | `:121-125` | 目标跟随小鹿 | repull 64.6，liftFawn 65.4；64.2 s 起环境图换成 sunset_forest（`scene.ts:344-347`） |
| 15 | 68.5–74.0 | 抱鹿回岸 | 跟拍白雪抱鹿走回 | `:127-131` | 跟随白雪 (2.1,1.0,1.9) | carryStart 68.2，bankStep 73.8；抱姿（`performance.ts:259-270`） |
| 16 | 74.0–80.5 | 安全了 | 从溪侧回看岸上群像 | `:133-139` | v2 重构：(2.2,1.35,1.6)→(1.3,1.2,2.6) | relief 75.0，fawnDown 75.4，nuzzle 77.0，grumpySmile 78.6，sleepyNap 79.0 |
| 17 | 80.5–85.5 | 苹果 | 苹果交给糊涂蛋，他递给小鹿 | `:141-145` | 跟随糊涂蛋头部，FOV 44 | appleGive 81.2，wipe 82.2，offer 83.6（`scene.ts:433-450`） |
| 18 | 85.5–90.0 | 回家 | 缓慢拉远，众人走向木屋，片名浮现 | `:147-152` | (2,2,7)→(9,4.6,13.5)，FOV 40→46 | homeWalk 85.6，titleIn 87.0；木屋灯光 80 s 起亮（`scene.ts:383-385`） |

- **切镜方式**：`cameraAt()` 按 `t` 找到当前镜头，计算 `local = (t - start)/(end - start)`，再调用对应函数（`camera.ts:155-162`）。镜头之间是硬切，没有混合；注释说明这样能保持连续性清晰。
- **对焦**：`frame()` 把对焦距离设为机位到目标的距离（`camera.ts:24-26`）；`maxblur` 固定为 0.006（`scene.ts:505`）。
- **手持抖动**：`handheld(t, amp)` 用确定性值噪声 `wobble()` 生成三轴偏移，振幅为 0.03–0.09 m（`camera.ts:21-23`）。

### 叙事物理量随时间变化（`src/world/layout.ts`、`src/anim/story.ts`）

| 量 | 关键帧 | 用途 |
|---|---|---|
| 水位 `waterLevel(t)` | 0 s −0.30 → 14 s −0.27 → 22 s −0.22 → 57 s −0.19 → 74 s −0.18 → 90 s −0.19（m） | 水面高度、溅水落点 |
| 流速 `flowSpeed(t)` | 0.9 → 1.2 (14 s) → 1.5 (22 s) → 1.6 (57 s) → **2.4 (58.3 s 浪涌)** → 1.9 (61 s) → 1.5 (74 s) → 1.3 (90 s) | 水面噪声平流、溪流声音包络 |
| 倒木偏航 `logYaw(t)` | 30.2 s 起 0→0.034 rad → 拉绳后回到 0.012 → 浪涌 0.026 → 抓绳时 0.052 → 重新拉后 0.01；叠加 `ring()` 阻尼振荡 | 倒木位置、绳结、白雪落脚点、水面泡沫线 |
| 裙边湿痕 `hemWetness(t)` | 25.5 s 0 → 30.8 s 0.11 → 58.8 s 0.22 → 90 s 0.24（m） | 白雪裙摆湿区 shader（`glbRig.ts:230` addWetness） |

## 5. 关键算法与实现

> 本片**不讲解知识**，它是叙事短片。以下都是**制作视频所用的算法**，均根据源码整理。

### 5.1 Remotion 驱动命令式 Three.js 的逐帧确定性渲染

- `Film.tsx` 在模块级缓存一个 `Scene` 单例，按分辨率和质量签名 `sig` 复用。每帧执行 `delayRender` → `scene.ready` → `renderAt(frame/fps)` → `requestAnimationFrame` 中调用 `gl.finish()` → `continueRender`，确保 Remotion 截图前 GPU 已经完成绘制（`Film.tsx:17-54`）。
- `Scene.update(t)` 的更新顺序（`scene.ts:486-512`）：时间 uniform → 光照 → `Director.update(t)` 摆姿态 → 小鹿同步 → `updateMatrixWorld` → 道具（篮子、苹果、绳卷读取手腕的世界坐标）→ 再次 `updateMatrixWorld` → 相机 → 景深和调色参数。
- 所有素材在 `AssetLib.load()` 中先 `await` 完再渲染第一帧（`world/assets.ts:2` 注释），避免导出半加载画面。
- 随机数统一用 `mulberry32(seed)`（`engine/math.ts:29-38`），噪声用基于哈希的值噪声和 fbm（`:40-79`）。
- **确定性的例外（推断，依据代码）**：README 称“一切都是时间 t 的纯函数”，但以下几处读取上一帧状态：`glbRig.ts:492-510` 的 `secondary()` 用上一帧骨盆和头部位置估算速度，驱动胡须、头发和裙摆；`performance.ts:189` 用 `prevPos` 估算白雪在倒木上的速度。单独渲染一张静帧时，这些速度为 0；`concurrency: 1` 顺序渲染时才有正常的滞后效果。目前没有记录说明这会影响画面。

### 5.2 时间曲线工具（`src/engine/math.ts`）

```ts
smoother(t) = t³(t(6t − 15) + 10)                      // 五次平滑，用作默认缓动
track([[t0,v0],[t1,v1],...], t)                         // 分段平滑关键帧插值
ring(t, t0, freq, decay) = sin(2π·freq·(t−t0))·e^(−decay·(t−t0))   // t≥t0 时的阻尼振荡
pulse(t, a, b, edge) = smoothstep(a−e,a+e,t)·(1 − smoothstep(b−e,b+e,t))
```

倒木的偏航、横滚和起伏，裙边湿痕，水位与流速，都由 `track` 加 `ring` 组合而成，例如 `story.ts:15-42`。

### 5.3 防滑步的步幅落脚规划（`src/characters/locomotion.ts`）

1. `GroundPath` 用关键帧和 `smoother` 插值得到位置；朝向优先采用显式 `face` 关键帧，没有时取速度方向（`:11-54`）。
2. `planFootsteps()` 以 1/30 s 采样路径并累计路程。每累计一个步幅 `stride`，就在 0.12 s 后的路径点上，按髋宽 `hipW + 0.02` 向左或向右放一个落脚点，y 取地形高度（`:65-102`）。
3. 步幅：白雪 0.56 m，矮人为 `0.85 × (大腿 + 小腿)`（`glbRig.ts:68`，数据来自 `dims.json`）。
4. `driveLocomotion()` 让每只脚在 `swingStart = next.t − min(0.52·gap, 0.34)` 之前固定在世界坐标里（支撑相）。之后进入摆动相，按 `smoother` 插值，抬脚高度为 `sin(πu)·(0.06 + 0.03·speed)`，并加入脚掌先离地、再脚跟落地的俯仰（`:144-162`）。
5. 身体：根据步态相位计算骨盆起伏、侧摆、横滚，按加速度前倾（限制在 −0.12 到 0.18），并让手臂反向摆动（`:165-221`）。

### 5.4 解析双骨 IK（`src/characters/glbRig.ts:308-326`）

```text
d   = clamp(|target − root|, |l1 − l2| + 1e-3, 0.999(l1 + l2))
bend = 极向量投影到目标轴的垂直方向
cosA = (l1² + d² − l2²) / (2·l1·d)           // 余弦定理
elbow = root + dir·cos(A)·l1 + bend·sin(A)·l1
aimBone(upper → elbow); aimBone(lower → target)
```

- `aimBone()` 用 `setFromUnitVectors` 把骨骼 +Y 轴对准目标，再绕骨轴扭转，让局部 +Z 朝向极向量提示（`:280-305`）。
- 所有姿态都相对绑定姿势的 `restWorldQ` 计算（`:259-268`）。文件头注释说明，这样 Blender 导出时的骨骼 roll 不影响结果。
- v1 的 `ik.ts` 是同一思路的旧实现，假设骨轴为 −Y；v2 不再调用它。

### 5.5 拉绳受力与绳索几何（`performance.ts:95-155`，`world/props.ts:373-397`）

```ts
engaged = window01(t, knotDone−0.5, pull1) · (1 − window01(t, carryStart+0.5, carryStart+1.5))
tug     = Σ_{bt∈{pull1,pull2,pull3,repull,repull+2,repull+3.6}} max(0, sin(3.2(t−bt)))·e^(−1.2|t−bt|)
slip    = pulse(t, slip, grab, 0.08)
exert   = clamp(engaged + 0.4·tug + 0.5·slip)
```

- 每个矮人的握点 = 站位 `STANCE[id]` 投影到“绳结→锚树”的拉线上，高度约在胸口（腿长 + 脚高 + 0.5 × 躯干）。拉绳时握点后移 `0.08·tug`，滑脱时沿绳滑 `0.35·slip`（`:107-126`）。
- 绳索控制点 = `[logPoint(t, KNOT_S=0.86, 顶面), 按拉线参数 u 排序的各矮人握点, 锚树]`。瞌睡虫不握绳，单独在树根处蹬住（`:143`）。
- `Rope.update()` 每帧用 `CatmullRomCurve3`（centripetal）重新生成半径 0.014 m 的 `TubeGeometry`。分段数为 `clamp(round(len·24), 8, 200)`。贴图 `offset = slide·14` 表现绳子在手中滑动。
- 苹果、绳卷和篮子不建约束，每帧直接把位置设到相关角色手腕骨的世界坐标（`scene.ts:409-450`）。

### 5.6 水体着色器（`src/world/water.ts:15-168`）

- 网格：沿溪流中线展开的条带，x 范围 −70…70，在 |x| < 14 的动作区内采样间距 0.12 m，外围 0.6 m。
- 流场：基础方向 +x，靠近岩石时混合切向分量 `infl = exp(−2.2·max(rd − R, 0))`；岩石下游用 `sin(3x − 1.5t)` 旋转噪声坐标，形成尾流涡。
- 平流：CPU 端用 0.05 s 步长从 0 积分 `flowSpeed`，得到相位 `uFlowPhase`。这样跳到任意帧也能得到相同结果（`:151-157`）。
- 着色：fbm 差分法线 → `fres = 0.02 + 0.38(1 − N·V)^5` → 天空渐变反射（乘 0.6）→ 按深度混合浅水色和深水色 → 高光指数 300 和 30。泡沫由岩石、倒木上游、尾流、岸边、条纹和浪涌叠加；最后在着色器内计算雾。
- 对应 v1 修正：亮天空反射被波法线打散，在水面形成“白斑”。作者降低了菲涅尔、天空反射和法线强度，并加深水色（`04-检查记录.md` 第 2 条）。

### 5.7 事件驱动的溅水粒子（`story.ts:73-118`，`water.ts:171-235`）

- 先确定性地生成事件表：岩石周围每 0.11 s 一次持续翻涌；12 s 后倒木边也有翻涌；另外加入倒木偏移、树枝撞击、蹄子踏水、浪涌（沿倒木 9 个点，每个 120 粒）和绳滑冲击。
- 每个事件用 `mulberry32(1000+i)` 预先生成粒子速度。任意时刻都可以直接计算抛物线：`y = y0 + vy·age − 4.9·age²`，低于水面就剔除。因此不需要逐帧模拟，任意帧都能精确复现。

### 5.8 GLB 角色运行时（`src/characters/glbRig.ts`）

- **面部**：表演层用简化的 `FaceState`（blink、lid、browUp、smile、open、noseWrinkle 等）映射到 42 个 ARKit 形变，例如 `jawOpen = 0.75·open`，`eyeSquint = squint + 0.25·smile`（`:438-468`）。角色习惯表情已在 Blender 中烘焙。
- **裙摆 `SkirtSim`**：以腰为 0、下摆为 1 计算 `k`，滞后量 `lag = k²`。顶点按骨盆速度反向偏移 `0.09/0.11·lag`，叠加两层正弦摆动；靠近膝盖和小腿时把顶点径向推开，防止膝盖穿出裙子（`:560-590`）。这不是布料模拟。
- **胡须和头发**：用头部速度乘系数做反向摆动，再叠加微风正弦（`:508-516`）。
- **材质修正**：MakeHuman 把皮肤、眼睛和牙齿标成 BLEND，这里改为不透明以避免排序问题；织物 sheen 用布料颜色着色（`:166-215`）。

### 5.9 Blender/MPFB 角色构建（`assets-src/blender/build_characters.py`）

- `CHARS` 字典定义每个角色的 MPFB 表型（gender/age/muscle/weight/height/proportions/race）、脸部 target（如白雪 `head-oval 0.6`、`nose-volume-decr 0.3`；博士 `nose-volume-incr 1.0`、`stomach-pregnant-incr 0.35`）、皮肤和眼睛材质、发型、`rest_expr` 习惯表情，以及胡须、服装和声线字段（`:60-140`）。
- **侏儒比例** `dwarf_proportions()`（`:1032-1066`）：在 pose 模式缩放腿、手臂、手、脚、脊柱、颈和头 → 把形变连同形态键一起烘焙进网格 → `armature_apply` 设为新的绑定姿势 → 恢复 Armature 修改器 → 把最低点放回地面。
- 服装（泡泡袖、高领、长裙、外衣、腰带、尖帽）用 lathe、tube 和身体区域复制建模，并转移权重；胡须是按脸部特征点生长的发卡，并把表情形变迁移到胡须上（`transfer_face_units`）。
- 导出（`:1352-1371`）：大于 1K 的贴图缩到 1K 或 2K（4K 皮肤缩到 2K），GLB 使用 JPEG q85，包含 skins 和 morph，不导出动画。`--preview` 用 EEVEE 和 monbachtal HDRI 渲染 1280² 的全身和脸部预览，保存到 `checks/characters/`。
- 小鹿 `build_fawn.py`：用 Skin 修改器搭身体骨架 → Subsurf → 23 根骨骼自动权重 → 顶点色画毛色和白斑；成片中整体缩放 0.72（`04-检查记录.md` 第 5 条）。
- `dims.json`（各角色的髋高、腿长、臂长、肩宽等）由运行时 `syncSpecsToModels()` 读取。**它的生成方式没有记录**，Python 脚本中没有写出该文件的代码；README 要求重建角色后手动同步。
- `decimate_trees.py` 把 Poly Haven 的 fir 和 pine 原模型按 0.012 和 0.005 的比例减面，生成 LOD；成片未启用。`probe.py`、`probe2.py` 是探查 MPFB API、骨骼名和资源列表的一次性脚本。

### 5.10 光照、调色与后期（`src/scene.ts:287-393`）

- 管线：`RenderPass` → `BokehPass`（仅高质量档）→ `UnrealBloomPass(strength 0.42, radius 0.7, threshold 0.85)` → 自定义调色 `ShaderPass` → `OutputPass`。色调映射为 ACES Filmic，曝光 1.05。
- 调色着色器：`c·uWarm + uLift·(1−c)` → 饱和度 `uSat` → 暗角 `1 − |d|²·uVig·2.2`，`uVig = 0.32` → **静态抖动** `±0.75/255`，由 `gl_FragCoord` 哈希得到，不随帧变化。52–72 s 期间，`uWarm` 从 (1.05,1,0.94) 渐变到 (1.1,1,0.86)，`uSat` 从 1.05 升到 1.14。
- 太阳：方位角 −2.1→−1.0（40–85 s），高度角 0.28→0.5（30–80 s）。暖色权重 `warm = smoother((t−52)/18)`；太阳颜色从 #cdd8e0 变到 #ffd79a，74 s 后再变到 #ffbf73；强度 1.5→3.1。轮廓光强度 = `0.9·warm + 0.6·clamp((t−74)/16)`。
- 雾 `FogExp2`：密度 0.013→0.006，浪涌期间额外加 0.004；颜色从 #9fb0b0 变到 #e8d4b0。
- 环境图：64.2 s 前用 `monbachtal_riverbank`（强度 0.55），之后直接切到 `sunset_forest`（强度 0.45→0.6）。切换点和镜头 14 的切点重合，用镜头切换掩盖环境图跳变（推断）。

### 5.11 植被布置（`src/world/scatter.ts`）

- 用 `mulberry32(4711)` 生成确定性候选点。65% 偏向近岸和对岸可见带；跳过溪流（|creekD| < 3.15–3.6）和 `blocked()` 禁区，包括小路、木屋、拉绳动作区、推车区和倒木走廊。
- 数量示例：fern_02 520、grass_medium_01 420、nettle 260、celandine 220、periwinkle 160、shrub_04 90、dandelion 80、shrub_02 45，每种模型通过 `AssetLib.instanced()` 生成 InstancedMesh，一个矩阵对应一个实例（`world/assets.ts:92-102`）。草、花和碎树皮不投射阴影。远景针叶树的循环写成 `k < 0`，实际已关闭（`scatter.ts:108`）。
- 树干：`environment.ts:222-` 程序化生成森林，并加入 4 棵英雄树（其中一棵是锚树）。树干贴图换成 bark_brown_02 扫描纹理（`scene.ts:227-230`）。

## 6. 声音与字幕

所有声音都由 `scripts/gen-audio.mjs` **离线合成**，没有录音或采样素材。脚本读取与画面相同的 `cues.json`（`beats`、`dialogue`、`duration`），采样率为 48 kHz 双声道，输出 `public/audio/final-mix.wav`。`film` 合成通过 `<Audio src="audio/final-mix.wav">` 挂载音频（`Root.tsx:9,22`）。

### 6.1 分层与参数

| 层 | 实现（ffmpeg lavfi） | 关键参数 | 行号 |
|---|---|---|---|
| 森林底噪 | 粉噪声 → lowpass 1600 / highpass 120，再加 8 kHz 正弦“空气感”，以 0.1 Hz 调幅 | 混音增益 0.5 | `:38-48` |
| 鸟鸣 | 0.25 s 调频啁啾 `sin(2π(2500+1200 sin(2π·18t))t)·e^(−14t)`，用 LCG 种子 7 随机间隔 1.5–5.5 s | 紧张段音量降到 0.22；58–66 s 静音 | `:50-70` |
| 溪流 | 白噪声 → 1100 Hz 带通（2 个八度）→ highpass 300；音量 `0.22 + 0.12·clip((t−14)/8) + 0.5·e^(−((t−58.3)/0.8)²)` | 浪涌处出现高斯峰；另加 220 Hz 低通棕噪声作为水流低频 | `:73-88` |
| 音乐 | 7 段正弦和弦 pad，起音 1.2 s、收尾 1.5 s，lowpass 2200，部分加 5 Hz 颤音 | A 大三和弦 (0–24 s) → 加小三度 (10–24 s) → G Bb D F (22–42 s) → D A C Eb (40–62 s) → 低音 A E A (57–69 s) → F A C E (66–82 s) → C G C E (80–90 s) | `:91-106` |
| 铃音旋律 | 指数衰减正弦拨弦琶音 | 开头 1.0–7.4 s 共 7 音，结尾 82–88.6 s 共 7 音 | `:108-114` |
| 脚步 | 120–500 Hz 带通噪声短脉冲 | 0.5–11 s 每 0.42 s 一次（加抖动），85.8–90 s 每 0.45 s 一次 | `:128-130` |
| 动作音效 | 噪声脉冲 + 衰减正弦 | 放篮、倒木“咔”声（70 Hz + 噪声）、树枝撞击、绳索吱响（140 Hz，摆动 10）、浪涌（1.4 s）、溅水、绳滑（420 Hz）、鹿鸣（520 Hz，颤音 40）、蹭手 | `:132-146` |
| 喷嚏 | `say -v Reed -r 190 "阿——嚏！"`，放在 `B.sneeze` | 音量 0.9 | `:148-156` |
| 对白 | macOS `say`，每个角色单独设置声线和语速，按 `d.t` 用 `adelay` 定位，音量 1.5 | 见下表 | `:159-171` |
| 母带 | 所有轨道 `apad` + `atrim` 对齐到 90 s → `amix normalize=0` → `loudnorm=I=-16:TP=-1.5:LRA=11` → `alimiter=limit=0.95` | 目标响度 −16 LUFS | `:173-186` |

### 6.2 对白、声线与时间点

| t (s) | 角色 | 文本 | `say` 声线 | 语速 |
|---|---|---|---|---|
| 11.9 | 白雪 | 听……是小鹿！ | Tingting | 180 |
| 36.3 | 博士 | 先稳住倒木！ | Grandpa | 160 |
| 42.1 | 爱生气 | 麻烦……交给我。 | Rocko | 170 |
| 47.6 / 49.6 | 开心果 | 一、二——拉！ | Eddy | 195 |
| 54.2 | 害羞鬼 | 别怕……看那边。 | Reed | 165 |
| 62.7 | 爱生气 | 抓紧了！ | Rocko | 170 |
| 64.4 | 开心果 | 一起——拉！ | Eddy | 195 |
| 75.8 | 白雪 | 没事了。 | Tingting | 180 |
| 86.2 | 白雪 | 我们回家吧。 | Tingting | 180 |

- 瞌睡虫和糊涂蛋没有对白。喷嚏精只有喷嚏，使用 Reed 声线。
- 对时方式：对白、音效和画面表演都引用 `cues.json` 中的同一组时间点，画面和声音不需要另做对齐。**没有对口型**：`FaceState.open` 由表演层设置，代码没有读取 TTS 音频包络（推断，依据 `gen-audio.mjs` 只输出 WAV，`src/` 中没有音频分析代码）。

### 6.3 字幕与片名（`src/Overlay.tsx`）

- 字幕是 DOM 文本，叠加在 WebGL canvas 上，所以文字边缘清晰：ZCOOL 字体 40 px，距底部 70 px，颜色 #fdf6e8，双层黑色阴影。
- 显示区间：从 `d.t − 0.1` 开始，持续 `max(1.1, 0.16 × 字数)` 秒（`:13-16`）。这是按字数估算的时长，没有用实际 TTS 音频长度对时。片名出现后不再显示字幕（`t < titleIn`）。
- 片名：87.0 s 起用 1.4 s 的 cubic ease-out 淡入，并上移 26 px，显示“溪谷救援”（96 px）；88–89 s 淡入副标题“白雪公主与七个小矮人”（34 px）。

### 6.4 响度与检查

- `04-检查记录.md`（v2）：mean −17.8 dB、max −1.1 dB；v1 段落记录为 max −1.2 dB。数值格式像 `volumedetect` 的输出（推断）。**没有记录积分 LUFS 实测值**，作者把 “loudnorm + 限制器” 视为“响度达标”。
- 没有试听记录，也没有 LUFS 测量或人工听感检查。本轮也未试听。

## 7. 迭代、问题与修正

项目只有 `docs/04-检查记录.md` 记录了迭代，git 历史无法还原过程（仓库只有两次提交）。以下为该文档的整理。

### v1 迭代（2026-10-19，程序化角色版本）

| # | 问题 | 根因 / 修正 | 当前代码中的对应 |
|---|---|---|---|
| 1 | 粒子着色器编译报错 | 三处 `#include` 必须单独占一行 | `water.ts` 中的 `#include <colorspace_fragment>` 单独成行 |
| 2 | 水面全屏“白斑” | 雾天亮天空的反射被波法线打散 → 降低菲涅尔、天空反射和法线强度，加深水色 | `water.ts:114-117`：`fres` 上限 0.4，天空乘 0.6 |
| 3 | 白雪在倒木段沉到河床高度，手臂按绝对高度上举 | 增加倒木表面高度覆盖和落脚点；休息和摆臂高度改为相对根节点 | `performance.ts:181-200` |
| 4 | 白雪头顶头发穿出，鼻子过大 | 增加实心发帽和整齐刘海，缩小鼻子 | v2 中对应 `build_characters.py` 的 `tuck_fringe`、鼻部 target |
| 5 | 被抱时小鹿过大且竖立 | 整体缩放 0.72，抱姿下移并倾斜，四肢收拢 | `performance.ts:259-270` |
| 6 | 结尾镜头推得太近，白雪的蓝衣挡住镜头，只能看到矮人后脑 | 镜头 16 改为从溪侧回看岸上群像 | `camera.ts:132-139` 注释 “faces, not backs” |
| 7 | 递苹果时白雪和糊涂蛋相距 4.7 m | 结尾让白雪先走到糊涂蛋身旁再递；小鹿落点移近害羞鬼，让“轻碰手”成立 | `staging.ts` 结尾走位 |

### v2 重制（2026-10-20）

- **用户反馈**（文档转述）：① 画面远差于 `ocean-shallows`；② 成片几百 MB。
- **体积问题根因**：调色着色器里**每帧变化的全屏颗粒**，加上 CRF 17，使 x264 几乎无法做帧间压缩，成片约 39 Mbps / 430 MB。修正：颗粒改为基于 `gl_FragCoord` 的**静态抖动**（`scene.ts:318-320`），`render.mjs` 默认改为 CRF 23、slow、yuv420p、AAC 192k。结果：82 MB，约 7 Mbps；本轮 ffprobe 视频码率为 7.09 Mbps，与此一致。
- **画面升级**：角色从程序化几何体换成 MPFB/MakeHuman CC0 写实人体（皮肤贴图、真实眼睛、睫毛、头发、42 个 ARKit 形变、53 骨含手指）；小鹿换成 Blender 带骨骼 GLB；环境使用 Poly Haven HDRI、扫描纹理和实例化植被。
- **v2 镜头修正**：镜头 8 改为博士近景，再硬切到糊涂蛋在推车取绳；推车移到人群后方；新增糊涂蛋抱绳卷走向绳结；镜头 11 和 13 重新取景；减弱溪流漩涡；移除遮挡画面的程序化树根和黑色程序石块。
- **作者列出的未达成目标**：① 没有达到影视级真人质感，MakeHuman 人体在近景下仍有游戏或 CG 感；服装是重新着色加程序化部件，没有布料模拟；② 动作仍是程序化 IK，CMU 动捕没有重定向，部分镜头姿态僵硬，手部有穿插；③ 小鹿没有毛发系统。
- **作者建议的下一步**：① 换成 Character Creator / MetaHuman 等商业角色；② 在 Blender 中把 CMU BVH 重定向到 game_engine 骨骼，并烘焙为 glTF 动画；③ 小鹿换成带毛发的扫描模型，或在 three.js 中加壳层毛发。

### 本轮发现的残留问题（档案作者核对，非项目记录）

- `checks/strip/pull-strip.png`（2026-10-19 21:43）和 `ending-strip.png`（2026-10-19 22:07）是 **v1 的旧图**。本轮查看 `pull-strip.png`，画面是程序化几何角色，水面有白斑。但 README 和 `04-检查记录.md` 仍把它当作当前拉绳试片的 filmstrip 引用，**v2 的拉绳 filmstrip 没有重新生成**。
- `checks/strip/full-overview.png` 的修改时间是 2026-10-20 20:38，早于成片 20:40。全片渲染约需 150–175 s，因此它可能来自上一版 v2 成片；**是否对应最终成片无法确认**。
- 本轮查看 `full-overview.png`（5×5 抽帧）的视觉观察（仅供参考，不是验收）：多帧中白雪从背后或侧后入镜，头部被画面上沿裁掉，只看到黄裙和蓝衣，约在倒木段和镜头 14、15 附近。`checks/characters/lineup.png` 中多名矮人的胡须与脸部交界处、嘴角和耳朵有明显的网格撕裂或贴片痕迹。
- `package.json` 的 `stills` 脚本指向 `scripts/gen-stills.mjs`，但**该文件不存在**。
- 文档与代码的其他不一致见第 2 节表格。

## 8. 可复用制作经验

1. **用一份 JSON 时间轴同时驱动画面、声音和字幕。** `cues.json` 的 `shots`、`beats` 和 `dialogue` 同时供镜头函数、表演层、溅水事件、音效脚本和字幕使用；改一个节拍时，画面、音效和字幕会一起移动，不需要手工对齐。依据：`src/timeline/cues.json`、`scripts/gen-audio.mjs:15-16`、`src/Overlay.tsx`、`src/anim/story.ts`。
2. **调色颗粒不要逐帧变化。** 全屏逐帧噪声会让 x264 的帧间压缩失效：v1 约 39 Mbps / 430 MB，改成静态抖动加 CRF 23 后约 7 Mbps / 82 MB。如果只是为了防止色带，用基于 `gl_FragCoord` 的静态抖动即可。依据：`docs/04-检查记录.md` v2「体积问题」、`src/scene.ts:318-320`。
3. **需要跳帧渲染的系统，用解析式或从 0 积分的计算。** 溅水粒子用事件表、预生成速度和抛物线公式；水面平流相位每帧从 0 积分 `flowSpeed`；倒木运动用 `track + ring`。这样 Remotion 渲染任意单帧都能得到一致结果。依赖上一帧速度的二级运动（裙摆、胡须）在单帧静帧里会退化为静止，需要另行处理。依据：`story.ts:73-118`、`water.ts:151-157,196-235`、`glbRig.ts:492-510`。
4. **防滑步先规划落脚点，再用 IK 跟随。** 按路程每走一个步幅生成世界坐标落脚点，支撑相保持脚不动，摆动相走弧线，最后用解析双骨 IK 求腿。拉绳、抱鹿等接触动作也只需要给手脚提供世界坐标目标。依据：`locomotion.ts:65-162`、`glbRig.ts:308-326`、`performance.ts:107-126`。
5. **Remotion 中嵌入命令式 WebGL 时，复用 Scene 单例，并在 `continueRender` 前调用 `gl.finish()`。** 同时设置 `preserveDrawingBuffer: true`、`concurrency: 1` 和 ANGLE 后端，并在第一帧前 `await` 全部素材。依据：`src/Film.tsx:17-54`、`scene.ts:81`、`remotion.config.ts`、`world/assets.ts:2`。
6. **用镜头切点掩盖不可插值的切换。** 环境 HDRI 在 64.2 s 从冷调直接切到夕照，正好与镜头 14 的切点重合（推断这是有意安排）。镜头 8 用镜头内硬切，以低成本同时交代博士和糊涂蛋两条线。依据：`scene.ts:344-347`、`camera.ts:79-88`。
7. **生成角色时同步导出预览图并拼成阵容图，及早检查辨识度和穿插。** `build_characters.py --preview` 用 EEVEE 输出全身和脸部图，供人工复看。但本片在 `lineup.png` 中仍能看到胡须和脸部的撕裂，说明有预览图还需要逐张检查后再接入成片。依据：`build_characters.py:1374-1412`、`checks/characters/`。
8. **检查图要随版本重新生成，否则会误导后续读者。** 本片 v2 文档仍引用 v1 的 `pull-strip.png`。建议给检查图加版本号或生成时间。依据：本轮比较文件修改时间并查看画面（第 7 节）。

## 9. 复现入口

> 以下命令摘自 README 和 `package.json`。**记录命令不代表本轮执行过**。所有命令都以 `videos/snow-white-creek-rescue-20261019/` 为工作目录。

**前置条件**：Node ≥ 20（开发使用 24.13）、macOS（`say` 中文声线 Tingting、Grandpa、Rocko、Eddy、Reed）、`ffmpeg` / `ffprobe`。重建角色需要 Blender 5.x、MPFB 2.0.18 插件和 MakeHuman CC0 资源包（均在 `assets-src/vendor/`，不在 git 中）。

```bash
# 0) 安装依赖（.npmrc：cache=.cache/npm，save-exact）
npm install

# 1) 素材（gitignore 不保存，需重建；本机已有成品）
npm run assets:fetch        # Poly Haven → public/assets + docs/assets-polyhaven.json
# 首次重建角色前：安装 MPFB 插件并解包资源到 .cache/blender（README「素材」节的 4 行命令）
npm run assets:characters   # Blender + MPFB → public/models/*.glb + checks/characters/*.png
npm run assets:fawn         # → public/models/fawn.glb
npm run assets:trees        # 可选：fir/pine LOD（成片未启用，原件需放在 .cache/heavy/）
# 修改角色后：同步 src/characters/dims.json（生成方式未记录）

# 2) 预览
npm run preview             # Remotion Studio：film / film-silent / preview

# 3) 检查
npm run still -- 2226 checks/still-01-group.png film-silent
npm run still -- 378  checks/still-02-snow-face.png film-silent
npm run still -- 1500 checks/still-03-rescue.png film-silent
npm run clip  -- 46.5 52.5 checks/test-clip-pull.mp4 film-silent
ffmpeg -y -i checks/test-clip-pull.mp4 -vf "select=not(mod(n\,30)),scale=620:-1,tile=3x2" -frames:v 1 -update 1 checks/strip/pull-strip.png

# 4) 音频 → 成片
npm run audio               # → public/audio/final-mix.wav
npm run render              # → out/snow-white-creek-rescue.mp4（film 合成）
npm run typecheck
```

- **执行顺序依赖**：必须先运行 `audio` 再导出 `film`，否则会因缺少 WAV 报错；无声版可用 `npm run render -- out/silent.mp4 film-silent`。
- **耗时**（文档声明）：渲染约 15–19 fps，全片 2700 帧约 150–175 s。Blender 角色生成耗时未记录；按文件修改时间推测，8 个 GLB 约 2 分钟内依次写出（17:11–17:13）。
- **压缩分发版**（README 提供，未见产物）：`ffmpeg -i out/... -c:v libx264 -crf 22 -preset medium -pix_fmt yuv420p -movflags +faststart -c:a aac -b:a 192k out/snow-white-creek-rescue-web.mp4`。
- **常改参数位置**：时间轴和对白在 `cues.json`；站位在 `staging.ts`（`STANCE`、`KNOT_POS`）；世界坐标在 `layout.ts`；机位在 `camera.ts`；光照、调色和雾在 `scene.ts` 的 `updateLighting` / `setupPost`；声线和音乐在 `gen-audio.mjs`；造型在 `build_characters.py` 的 `CHARS`。

## 10. 参考来源

**未发现创作参考视频、X 帖子或参考图。** 项目文件中出现的链接都属于素材来源或技术资料。本轮没有联网访问这些链接，内容以本地文件为准。

| 来源 | 类型 | 在本片中的用途 | 本地证据 | 访问状态 |
|---|---|---|---|---|
| Poly Haven `https://polyhaven.com`（API `https://api.polyhaven.com`；单个素材页 `https://polyhaven.com/a/<id>`） | 素材来源（CC0 1.0） | 下载 4 张 HDRI、16 套 PBR 纹理和 29 个模型。运行时 `assets.ts:21-27` 加载其中 2 张 HDRI、11 套纹理和 26 个模型；服装布料纹理由 Blender 构建脚本通过 `TEX` 路径读取（推断）。`docs/assets-polyhaven.json` 共列 49 项，含作者和链接，抓取时间 2026-10-20T08:25:41Z；作者以 Rico Cilliers（20）、Rob Tuytel（19）为主 | `scripts/fetch-assets.mjs`、`docs/assets-polyhaven.json` | 本轮未访问 |
| MPFB 2.0.18 / MakeHuman CC0 资源包 | 素材与工具来源（插件 GPL-3.0；资源 CC0） | 生成 8 个角色 | `assets-src/vendor/*.zip`、`docs/03-素材清单.md` | 文件中没有下载 URL，本轮未访问 |
| CMU Graphics Lab Motion Capture（BVH 转换：cgspeed；文档来源：`github.com/una-dinosauria/cmu-mocap`；原库：mocap.cs.cmu.edu） | 素材来源 | **仅下载参考，成片未使用** | `assets-src/mocap/READMEFIRST.txt`、`docs/03-素材清单.md:33` | 本轮未访问 |
| ZCOOL XiaoWei `https://github.com/googlefonts/zcool-xiaowei/`（SIL OFL 1.1，`http://scripts.sil.org/OFL`） | 素材来源（字体） | 字幕和片名 | `public/fonts/OFL-ZCOOLXiaoWei.txt` | 本轮未访问 |
| 同仓库 `ocean-shallows/` | 质量对比基准（用户反馈中的比较对象） | v2 重制的动机 | `docs/04-检查记录.md` v2「用户反馈」 | 本地目录，本档案未分析其内容 |

## 11. 证据与限制

### 实际读取的关键文件（sha256 前 16 位，取自 fingerprints.json）

| 文件 | sha256[:16] | 读取范围 |
|---|---|---|
| `README.md` | 33a574480f15e108 | 全文 |
| `docs/01-角色设定.md` | 5b777995cbd2787d | 全文 |
| `docs/02-分镜.md` | c45b5a86354348f9 | 全文 |
| `docs/03-素材清单.md` | d075a56990260b67 | 全文 |
| `docs/04-检查记录.md` | c5ef1bbbe0a92a36 | 全文 |
| `docs/assets-polyhaven.json` | d2a846629968f4d3 | 统计条目和作者 |
| `package.json` / `package-lock.json` | bb6226b792311b0d / f6753b2a1d115578 | 全文 / 关键包版本 |
| `remotion.config.ts` / `tsconfig.json` | b13e65948f6a02d8 / 3c8fe02ca5fa59f7 | 全文 |
| `scripts/render.mjs` / `gen-audio.mjs` | e22564d1515bb73d / 976a2023742c1ebc | 全文 |
| `scripts/fetch-assets.mjs` | 6ca01396a525814a | 1–60 行 |
| `src/timeline/cues.json` | ec7b754c46006fbe | 全文 |
| `src/Root.tsx` / `Film.tsx` / `Overlay.tsx` / `index.ts` | 2a5a10c58dd9dada / 3a665a7540a2d93f / bd6847f6db4dd13d / 8ad7cfb50e325464 | 全文 |
| `src/scene.ts` / `camera.ts` | 2b05cecc3083dc63 / a211ea7292088599 | 全文 |
| `src/anim/story.ts` / `world/layout.ts` / `engine/math.ts` | 850bf2de8a470f42 / 649c799e80173808 / befb368a754a846d | 全文 |
| `src/characters/locomotion.ts` / `ik.ts` | 3c67d5ace1840ef4 / cda2eb55e2e833d6 | 全文 |
| `src/characters/glbRig.ts` | be48f4304b50a653 | 结构概览 + 56–69、259–330、425–470、492–600 行 |
| `src/characters/performance.ts` | 08146cc153ff688d | 1–160、178–200 行 + 注释和节拍检索 |
| `src/characters/staging.ts` / `fawnGlb.ts` / `dims.json` | 585b0664c5f9beba / 020d31268db16a24 / 1af38a7ef11b900d | 1–40 行 / 1–40 行 / 前 30 行 |
| `src/world/water.ts` | 81fad5faf1864268 | 1–20、92–170、196–235 行 + 结构 |
| `src/world/props.ts` / `environment.ts` / `scatter.ts` / `assets.ts` | a1e421d14054f9d3 / f65ef0233fdab9ef / 1021689ad5f435f5 / 833cf89ff22030ef | 结构 + Rope / 森林开头 / 1–60、95–122 行 / 13–40 行 |
| `assets-src/blender/build_characters.py` | f3bcb6400c828ed6 | 1–80、1032–1070、1352–1415 行 + 函数目录 |
| `assets-src/blender/build_fawn.py` | cc464110e83060a9 | 1–30 行 + 关键字检索 |
| `assets-src/blender/decimate_trees.py` / `probe.py` / `probe2.py` | 779b781184a6c70d / b039a77703102b0a / df3d8ffa69debbe0 | 全文 |
| `assets-src/mocap/READMEFIRST.txt` | 8e6fe2e640b3728e | 前 30 行 + 来源行 |

**媒体**：本轮用 ffprobe 读取 `out/snow-white-creek-rescue.mp4`、`checks/test-clip-pull.mp4`（含 `-count_frames`）和 `public/audio/final-mix.wav` 的媒体头。查看了 `checks/strip/full-overview.png`、`checks/strip/pull-strip.png` 和 `checks/characters/lineup.png`。

### 未读取或未核验

- `src/characters/build.ts`、`parts.ts`、`apply.ts`、`specs.ts`、`state.ts`、`fawn.ts`（v1 程序化角色和小鹿驱动），`src/engine/textures.ts`，以及 `environment.ts`、`props.ts` 的大部分内容：只看了结构，没有逐行阅读。
- `assets-src/vendor/*.zip`：按要求未解压；15 个 BVH 文件未读取；GLB 和 HDR 素材本体未检查。
- `checks/still-0*.png` 三张静帧、`ending-strip.png` 和各角色单张预览：未逐张查看。
- 成片未播放、未试听，也没有逐帧检查；只看了 overview 拼图。

### 缺失信息

- 原始用户需求和提示词（没有 `prompt.md`），以及制作对话：**未找回制作对话，结论来自项目文件**。
- 用户对 v2 的验收意见：未记录。
- `dims.json` 的生成方法、Blender 和 `say` 的精确版本、角色生成耗时与总制作耗时：未记录。
- 积分响度 LUFS 实测值：未记录（只有 loudnorm 目标 −16 和 volumedetect 风格的电平）。
- `build/` 中间产物（渲染帧、`.cache/`）已不在目录中；缺少中间产物不代表它们从未生成。

**证据等级汇总**：代码/配置中存在 = 全部管线；日志/缓存显示执行过 = 无日志，仅能根据产物和修改时间判断；最终采用 = `out/snow-white-creek-rescue.mp4`；技术检查通过 = 规格、黑帧和电平（作者记录，本轮 ffprobe 复核规格）；视觉检查通过 = 作者声明三张静帧和试片通过（试片 filmstrip 实际是 v1 旧图）；用户验收 = 无记录。
