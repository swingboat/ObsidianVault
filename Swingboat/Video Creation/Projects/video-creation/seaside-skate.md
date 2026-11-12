---
type: video-production-dossier
project: video-creation
video_id: seaside-skate
captured: '2026-11-12'
status: 已有完整 MP4；分层有限动画
source_fingerprint: a5c22eb8e62588784868f2ece5a7deb910d4d762b2ae16d97d5cc9596bac7cef
tags:
- video-creation
- production-capture
---

# 海边滑板 · 手绘位图分层动画

<!-- capture-source-sha256:a5c22eb8e62588784868f2ece5a7deb910d4d762b2ae16d97d5cc9596bac7cef -->

本次为指定项目的材料回溯与归档。制作结论来自项目记录/源码，未找回关闭的制作对话，也未重新执行生成、渲染、音频合成或用户验收。

## 当前结论与制作路线

**状态：已有完整 MP4；分层有限动画。**

imagegen 原画与透明素材 → Canvas 姿态/行变形 → 视差 → 程序拟音。

1920×1080、30fps、10 秒。海岸背景、三个人物姿态和野花由内置图像工具生成；按滑入、蹬地、站稳、张臂滑行、拉远的节奏编排。它采用位图分层与代码变形，资料未表明使用 AI 连续视频模型。

## 关键算法、实现与适用边界

`Skater.tsx` 根据时间选择三种姿态，以各姿态的锚点保持脚与滑板的相对关系；Canvas 按小行块重绘，对头发、裙摆和后腿施加随时间与高度变化的正弦偏移。前景野花移动快于远景，形成视差；轮子旋转和远离时的音量变化由同一时间轴驱动。

## 声音与字幕

scripts/create-seaside-audio.py 用固定种子合成海浪、海风、滑板滚动三条 44.1kHz 双声道 WAV；属于声音设计，非现场录音。

## 失败、修正与验收边界

姿态切换与位图小幅变形的能力边界需要保留，不能据此宣称逐帧手绘或连续图生视频。

## 技术与模型版本

以下是本项目声明或锁定的依赖，安装清单本身不证明某项参与了最终画面。具体用途以制作路线、源码和生成记录交叉判断。

| 依赖 | package.json 声明 | 可解析的直接锁定版本 |
| --- | --- | --- |
| `@remotion/bundler` | `4.0.534` | `4.0.534` |
| `@remotion/cli` | `4.0.534` | `4.0.534` |
| `@remotion/renderer` | `4.0.534` | `4.0.534` |
| `react` | `19.2.3` | `19.2.3` |
| `react-dom` | `19.2.3` | `19.2.3` |
| `remotion` | `4.0.534` | `4.0.534` |
| `@remotion/media` | `4.0.534` | `4.0.534` |
| `@types/react` | `19.2.7` | `19.2.7` |
| `@types/react-dom` | `19.2.3` | `19.2.3` |
| `typescript` | `5.9.3` | `5.9.3` |

工具底层模型未公开时按“未披露”记录；不能由 imagegen 工具名猜测图像模型。未提供真实推理日志的配置不记作已运行。

## 镜头、时间轴与采用参数

> [!info]- 当前 Composition / 时间轴源码：videos/seaside-skate/src/Root.tsx，L1–15
> ```tsx
> import {Composition, Folder, staticFile} from 'remotion';
> import {SeasideSkate} from './SeasideSkate';
> import {CoastalWorld} from './seaside/CoastalWorld';
> import {Skater} from './seaside/Skater';
> 
> export const RemotionRoot = () => (
>   <>
>   <Composition id="SeasideSkate" component={SeasideSkate} durationInFrames={300} fps={30} width={1920} height={1080} />
>   <Folder name="SeasideSkate-Layers">
>     <Composition id="CoastalWorld" component={CoastalWorld} durationInFrames={300} fps={30} width={1920} height={1080} />
>     <Composition id="Skater" component={Skater} durationInFrames={300} fps={30} width={800} height={1000}
>       defaultProps={{spriteSrc: staticFile('seaside/girl-poses.png'), motionStrength: 1}} />
>   </Folder>
>   </>
> );
> ```

源时间轴、生成帧数、所用区间与最终时长分别记录；历史文档或候选参数不自动覆盖当前采用配置。

## 参考来源与借鉴说明

在本次盘点的项目自有材料中，未找到可明确归属本片的外部参考视频或 X 帖子链接；这不等于历史上没有参考。


## 重现入口与修改位置

以下是 package.json 中的实际命令映射，供在对应视频目录使用；本轮未执行这些制作命令。系统依赖、前置模型/素材和环境准备见后附 README。

| npm 入口 | 实际命令 |
| --- | --- |
| `npm run dev` | `node scripts/remotion.mjs studio src/index.ts --no-open --port=3214` |
| `npm run compositions` | `node scripts/remotion.mjs compositions src/index.ts` |
| `npm run typecheck` | `tsc --noEmit` |
| `npm run render` | `node scripts/remotion.mjs render src/index.ts SeasideSkate outputs/seaside-skate.mp4 --codec=h264 --audio-codec=aac --pixel-format=yuv420p --concurrency=2` |
| `npm run still` | `node scripts/remotion.mjs still src/index.ts SeasideSkate outputs/checks/current.png --frame=165` |
| `npm run check` | `npm run typecheck && node scripts/check-isolation.mjs` |
| `npm run pack` | `python3 scripts/package-project.py` |
| `npm run prepare:audio` | `python3 scripts/create-seaside-audio.py` |
| `npm run verify` | `node scripts/verify-media.mjs` |

### 算法源码摘录

> [!info]- videos/seaside-skate/src/seaside/Skater.tsx，L1–86
> ```tsx
> import {useEffect, useLayoutEffect, useRef, useState, type CSSProperties} from 'react';
> import {AbsoluteFill, Interactive, cancelRender, continueRender, delayRender, useCurrentFrame, useVideoConfig} from 'remotion';
> 
> type SkaterProps = {readonly spriteSrc: string; readonly motionStrength: number; readonly style?: CSSProperties};
> 
> const SkaterInner = ({spriteSrc, motionStrength, style}: SkaterProps) => {
>   const frame = useCurrentFrame();
>   const {fps} = useVideoConfig();
>   const time = frame / fps;
>   const canvasRef = useRef<HTMLCanvasElement>(null);
>   const [image, setImage] = useState<HTMLImageElement | null>(null);
>   const [handle] = useState(() => delayRender('Loading watercolor girl sprite'));
>   useEffect(() => {
>     const img = new Image();
>     let mounted = true;
>     img.onload = () => { if (mounted) setImage(img); };
>     img.onerror = () => cancelRender(new Error(`Unable to load character asset: ${spriteSrc}`));
>     img.src = spriteSrc;
>     return () => { mounted = false; continueRender(handle); };
>   }, [spriteSrc, handle]);
>   useEffect(() => { if (image) continueRender(handle); }, [image, handle]);
> 
>   useLayoutEffect(() => {
>     const context = canvasRef.current?.getContext('2d');
>     if (!context || !image) return;
>     context.clearRect(0, 0, 800, 1000);
>     const pose = time < 3.3 ? 0 : time < 4.8 ? 1 : 2;
>     const source = [{left: 0, width: 557, anchor: 435}, {left: 567, width: 456, anchor: 819}, {left: 1024, width: 512, anchor: 1324}][pose];
>     const offset = 400 - source.anchor;
>     // Small scanline deformations give the painted hair and dress a breeze.
>     for (let y = 0; y < 1024; y += 8) {
>       const dress = y >= 440 && y <= 640;
>       const hair = y >= 130 && y <= 300;
>       const sway = dress ? Math.sin(time * 6.8 + y * 0.025) * ((y - 430) / 210) * 4.5
>         : hair ? Math.sin(time * 4.3 + y * 0.035) * 1.7 : 0;
>       const rowHeight = Math.min(8, 1024 - y);
>       if (pose === 0 && y >= 632) {
>         // Deform the trailing leg as one continuous image, without cutting it
>         // into overlapping pieces. The planted front shoe stays fixed.
>         const amount = Math.min(1, (y - 632) / 224);
>         const kick = Math.sin(time * 6.1);
>         context.drawImage(image, 0, y, 360, rowHeight,
>           offset + amount * kick * 9 * motionStrength, y + amount * (30 + kick * 5), 360, rowHeight * 1.18);
>         context.drawImage(image, 360, y, source.width - 360, rowHeight,
>           360 + offset, y, source.width - 360, rowHeight);
>       } else {
>         context.drawImage(image, source.left, y, source.width, rowHeight,
>           source.left + offset + sway * motionStrength, y, source.width, rowHeight);
>       }
>     }
>   }, [frame, fps, time, image, motionStrength]);
> 
>   return (
>     <AbsoluteFill style={{width: 800, height: 1000, ...style}}>
>       <canvas ref={canvasRef} width={800} height={1000} style={{position: 'absolute', width: 800, height: 1000}} />
>       <svg width="800" height="1000" viewBox="0 0 800 1000" style={{position: 'absolute'}}>
>         <defs>
>           <linearGradient id="skate-deck" x1="0" y1="0" x2="0" y2="1">
>             <stop stopColor="#b78657" /><stop offset="1" stopColor="#795542" />
>           </linearGradient>
>           <filter id="skate-painted"><feTurbulence type="fractalNoise" baseFrequency="0.09" numOctaves="2" seed="2" />
>             <feComposite in="SourceGraphic" operator="in" /><feBlend in="SourceGraphic" mode="multiply" /></filter>
>         </defs>
>         <path d="M 257 858 Q 263 865 285 866 L 555 866 Q 572 863 582 854 L 576 866 Q 566 874 548 875 L 282 875 Q 265 874 256 866 Z"
>           fill="url(#skate-deck)" stroke="#6b5546" strokeWidth="2.6" />
>         <path d="M 263 858 Q 288 862 555 861 L 579 853" stroke="#d5af7c" strokeWidth="3" fill="none" />
>         <path d="M 306 875 L 306 884 L 337 884 M 503 875 L 503 884 L 531 884" stroke="#778382" strokeWidth="7" fill="none" />
>         {[320, 518].map((x) => (
>           <g key={x} style={{transformOrigin: `${x}px 893px`, rotate: `${time * 1500}deg`}}>
>             <circle cx={x} cy="893" r="15" fill="#bf7d51" stroke="#6e5846" strokeWidth="2.5" />
>             <circle cx={x} cy="893" r="6" fill="#e4c79b" />
>             <path d={`M ${x - 9} 893 L ${x + 9} 893 M ${x} 884 L ${x} 902`} stroke="#92643e" strokeWidth="2" />
>           </g>
>         ))}
>       </svg>
>     </AbsoluteFill>
>   );
> };
> 
> export const Skater = Interactive.withSchema({
>   Component: SkaterInner, componentName: '<Skater>', wrapInSequence: true,
>   schema: {
>     spriteSrc: {type: 'asset', assetType: 'image', default: '', description: '女孩手绘姿态素材'},
>     motionStrength: {type: 'number', default: 1, min: 0, max: 2, step: 0.1, hiddenFromList: false, description: '海风摆动幅度'},
>   },
> });
> ```

> [!info]- videos/seaside-skate/scripts/create-seaside-audio.py，L1–54
> ```python
> """Create deterministic, project-local surf, wind and skateboard sound design."""
> from pathlib import Path
> from array import array
> import math
> import random
> import wave
> 
> ROOT = Path(__file__).resolve().parents[1]
> DEST = ROOT / 'public' / 'seaside'
> RATE = 44100
> SECONDS = 10
> 
> def write_track(name, kind):
>     rng = random.Random(819 + kind)
>     samples = array('h')
>     low = soft = rumble = 0.0
>     for i in range(RATE * SECONDS):
>         t = i / RATE
>         noise = rng.uniform(-1, 1)
>         low += 0.018 * (noise - low)
>         soft += 0.12 * (noise - soft)
>         rumble += 0.004 * (noise - rumble)
>         if kind == 0:
>             # A swell rises, breaks, and washes back every few seconds.
>             phase = (t + 0.7) % 3.9 / 3.9
>             swell = math.sin(math.pi * phase) ** 2
>             sample = (low * 2.9 + soft * 0.65 + noise * 0.09) * (0.22 + 0.55 * swell)
>         elif kind == 1:
>             gust = 0.52 + 0.2 * math.sin(t * 0.87) + 0.12 * math.sin(t * 2.13)
>             sample = (rumble * 4.0 + low * 0.4) * gust
>         else:
>             # Low wheel rumble, granular road texture, and quiet pavement joints.
>             sample = low * 1.45 + soft * 0.28 + noise * 0.04
>             sample *= 0.7 + 0.13 * math.sin(t * 46) + 0.06 * math.sin(t * 101)
>             for at in (0.9, 1.65, 2.3, 2.95, 4.3, 6.1, 7.1):
>                 age = t - at
>                 if 0 <= age < 0.09:
>                     sample += 0.15 * math.exp(-age * 65) * math.sin(age * 1600)
>         fade = min(1, t / 0.5, (SECONDS - t) / 0.55)
>         sample = max(-0.95, min(0.95, sample * fade))
>         # Slight stereo variation, kept quiet and spacious.
>         samples.append(int(sample * 26000))
>         samples.append(int(sample * (0.97 + 0.02 * math.sin(t * 0.5)) * 26000))
>     with wave.open(str(DEST / name), 'wb') as wav:
>         wav.setnchannels(2)
>         wav.setsampwidth(2)
>         wav.setframerate(RATE)
>         wav.writeframes(samples.tobytes())
>     print(name, SECONDS, 'seconds, stereo')
> 
> DEST.mkdir(parents=True, exist_ok=True)
> write_track('surf.wav', 0)
> write_track('wind.wav', 1)
> write_track('skateboard.wav', 2)
> ```

## 完整关键提示词与生成记录

以下保留项目自有提示词、候选参数与版本。存在失败候选和历史参数；最终采用以 selected-takes、edit-decisions 与制作复盘为准，不能将所有候选都写成已采用。

> [!info]- videos/seaside-skate/public/seaside/asset-prompts.json
> ```json
> {
>   "generator": "Built-in image_gen",
>   "coast": "Use case: illustration-story. Asset type: panoramic painted background for a 1920x1080 animated film. Create one exceptionally beautiful wide horizontal watercolor and gouache Japanese hand-drawn animated film background, gentle warm summer childhood mood, Hayao Miyazaki-inspired landscape painting. No people. Camera eye level looking along a quiet coastal road, sea on the LEFT and meadow/hills on the RIGHT. Frame composition: pale blue summer sky and very soft hand-painted white clouds in the top 42%, far turquoise coast and hazy green headlands at the horizon, luminous blue-green sea extending from left edge to middle in the middle third. A warm gray paved road sweeps from the bottom-left foreground across the lower third and curves to a vanishing point slightly right of center around (65% width, 52% height). Clear generous road surface in the lower 35% for an animated skater. Narrow weathered cream guardrail separates ocean from road along its far edge. To the right soft grassy slopes, tiny yellow and white wildflowers, low verdant mountains. Road recedes naturally into distance; quiet rural coastal setting. Soft afternoon light, warm ivory highlights, delicate pencil contours and tactile watercolor paper texture, layered brushwork, atmospheric perspective. No harsh black line art, no 3D, no photographic realism, no giant foreground objects, no boats, no buildings, no lettering, no subtitles, no logo, no watermark. Extra wide panoramic composition, aspect ratio about 2.5:1, high resolution.",
>   "girl": "Use case: illustration-story. Asset type: transparent animation character sprite sheet, three poses, single horizontal row. Create a charming hand-drawn cel-animation character asset in a warm Japanese hand-painted animated film style with soft watercolor/gouache shading and fine pencil outlines, childhood summer mood, Hayao Miyazaki-inspired. Same ONE 8-year-old girl in all THREE panels, identical face, black chin-length bob hair, pale butter-yellow knee-length short-sleeved dress covered in tiny blue and white floral sprigs, white canvas lace-up sneakers, rosy cheeks, joyful gentle smile. Three equal-width vertical panels laid side-by-side across a 3:2 wide canvas, transparent background. FULL BODY in every panel, head to shoes with generous transparent margins. Girl faces RIGHT in gentle three-quarter side view, feet oriented right. Identical character scale and head height, all planted shoe soles at the same baseline 90% down the image. NO skateboard; skateboard will be separately animated below her.\nLEFT PANEL: pushing pose. Front/right leg bent slightly with its shoe planted on an imaginary skateboard deck baseline, trailing/left leg stretched diagonally backward left and downward in a light ground-push, torso leaning slightly forward, arms balancing.\nMIDDLE PANEL: coasting pose. BOTH sneaker soles firmly planted horizontally on the SAME deck baseline, feet spaced apart, knees slightly bent, torso relaxed, one arm forward and one backward for balance.\nRIGHT PANEL: joyful balancing pose. BOTH sneaker soles firmly planted on the same baseline, feet spaced apart and knees slightly bent, BOTH arms extended outward at shoulder level, happy open smile, breeze gently lifting the left edge of dress and hair ends.\nKeep head/face/dress/scale identical between panels, clean natural anatomy, no extra limbs. Separate panels clearly with EMPTY transparent gutters, no panel border or labels. The alpha background must be truly TRANSPARENT, not painted white and not a baked checkerboard. No ground, no shadows, no road, no skateboard, no scenery, no text, no watermark. High resolution, aspect ratio 3:2.",
>   "flowers": "Use case: illustration-story. Asset type: transparent foreground foliage layer for a warm hand-drawn Japanese watercolor animated film. Create a very wide horizontal strip with small clusters of delicate coastal meadow grasses and wildflowers growing from the BOTTOM EDGE, some butter-yellow flowers, tiny white daisies, soft sage leaves, graceful thin stems, warm afternoon sunlight. Brushy gouache and watercolor on paper, fine pencil edges, charming natural childhood summer feeling consistent with a lovingly painted coastal anime landscape. Separate airy little clumps scattered across the width, tallest grasses reach only halfway up, lots of genuinely transparent empty space above and between stems. Palette natural soft meadow green, straw yellow, ivory white. No scenery, no sky, no ground slab, no people, no pot, no text, no logo, no watermark. Landscape 3:1 aspect ratio. True transparent alpha background; do not paint a white or colored backdrop.",
>   "savedAssets": [
>     {
>       "path": "video-creation/videos/seaside-skate/public/seaside/coast-watercolor.png",
>       "sha256": "0feca3c1a4fccb94dcc7145dec65fcb88a9c8eeed1a1264e0b43d7603b6c81b1"
>     },
>     {
>       "path": "video-creation/videos/seaside-skate/public/seaside/girl-poses.png",
>       "sha256": "f52721959af4f28d82a9ab9ab90437e451723e4ab967bf3661fc4464fd4433a4"
>     },
>     {
>       "path": "video-creation/videos/seaside-skate/public/seaside/foreground-flowers.png",
>       "sha256": "6f517d4e9c245ea2e359b67f530d412832c8322dd597c6538a807f171fd98b91"
>     }
>   ]
> }
> ```

## 已有产物与检查依据

文件存在、技术检查、画面检查与用户验收分别看待。下表是本次文件盘点，旧版与当前版可能共存。MP4/HTML 按内容计算 SHA-256，ZIP 仅记录文件大小和修改时间供增量判断。

| 本地产物 | 字节数 | SHA-256 |
| --- | ---: | --- |
| `videos/seaside-skate/outputs/seaside-skate-project.zip` | 36452827 | `ZIP：仅元数据` |
| `videos/seaside-skate/outputs/seaside-skate.mp4` | 7217698 | `9c5cba3fca93f0787a451b63278faf087ed5184230f4ea6ad5eb59fdcb93f294` |

> [!info]- 本次只读 ffprobe 结果
> ```json
> {
>   "programs": [],
>   "stream_groups": [],
>   "streams": [
>     {
>       "codec_name": "h264",
>       "codec_type": "video",
>       "width": 1920,
>       "height": 1080,
>       "r_frame_rate": "30/1",
>       "duration": "10.000000",
>       "nb_frames": "300"
>     },
>     {
>       "codec_name": "aac",
>       "codec_type": "audio",
>       "sample_rate": "48000",
>       "channels": 2,
>       "r_frame_rate": "0/0",
>       "duration": "10.005000",
>       "nb_frames": "471"
>     }
>   ],
>   "format": {
>     "duration": "10.005000",
>     "size": "7217698"
>   },
>   "file": "outputs/seaside-skate.mp4",
>   "scope": "本次只读取媒体头；未全片解码、播放或重新进行视觉/听觉验收。"
> }
> ```


已有检查报告路径（未在本轮重新执行）：

- `videos/seaside-skate/outputs/checks/archive-report.json`
- `videos/seaside-skate/outputs/checks/full-export-report.json`
- `videos/seaside-skate/outputs/checks/isolation-report.json`
- `videos/seaside-skate/outputs/checks/media-verification.json`
- `videos/seaside-skate/outputs/checks/migration-report.json`

## 制作方法与迭代记录选编

下面是项目自有文档的归档摘录，保留原阶段与原检查表述，个人路径已泛化、相对链接已注明源项目位置。它们属于来源陈述，不代表本次重新完成其中的生成或验收。

> [!info]- 来源文档：videos/seaside-skate/README.md
> # SeasideSkate
> 
> 独立 Remotion 视频项目：1920×1080，30 fps，10 秒。仅包含本视频的组件、素材与脚本。
> 
> ```bash
> npm ci
> npm run dev
> npm run typecheck
> npm run check
> npm run still
> npm run render
> npm run verify
> npm run pack
> ```
> 
> 预览地址：http://localhost:3214/SeasideSkate。最终视频输出至 `outputs/seaside-skate.mp4`；缓存和临时文件位于 `.cache/`。本目录的 `package-lock.json` 锁定依赖，npm 缓存和浏览器均在本目录内。项目不能依赖其他视频或上层工作区文件。
> 
> 修改 `src/SeasideSkate.tsx` 及本视频的子组件、`public/` 素材；Composition 注册与时长在 `src/Root.tsx`。
> 音效在 `public/seaside/`；`npm run prepare:audio` 使用固定种子重新生成海浪、海风、滑板滚动声。
> 
> 迁移后已从独立项目导出完整 MP4，保存在 `outputs/`。需要重新导出时执行 `npm run render`。
> 
> 原始制作说明及可编辑参数见 docs/production-notes.md（项目文件：`videos/seaside-skate/docs/production-notes.md`）。

> [!info]- 来源文档：videos/seaside-skate/docs/production-notes.md
> ## 10 秒海边滑板手绘动画
> 
> 新增 `SeasideSkate`：1920×1080、30 fps、300 帧。Codex 预览地址：[海边滑板动画](http://localhost:3214/SeasideSkate)。若 Studio 已停止，运行 `npm run dev`，在画面下方点击播放；扬声器按钮可控制声音。
> 
> 这是由手绘位图素材编排的分层动画：0–2 秒女孩滑入海边公路；2–3.3 秒蹬地；3.3–4.8 秒双脚站稳；4.8–7 秒张开双臂滑行；7–10 秒拉远并滑向弯曲海岸公路。女孩保持同一套人物设计，前脚与滑板贴合，轮子转动，头发、裙摆与后腿采用逐帧变形。背景包含海面波光、云层和海鸥，野花前景移动速度快于远景，形成视差。
> 
> 内置 imagegen 生成的海岸全景、人物三姿态透明素材和野花透明素材保存在本项目 `public/seaside/`。完整提示词、最终文件路径与 SHA-256 见 `public/seaside/asset-prompts.json`；保留原始素材，动画变形在代码中完成。
> 
> 声音由本项目的 `scripts/create-seaside-audio.py` 合成，包含海浪、海风和滑板滚动三条 44.1 kHz 双声道 WAV，随距离变化调整滑板音量。它们是原创声音设计，并非现场录音。画面不含标题、字幕或 Logo。
> 
> 代码入口为 `src/SeasideSkate.tsx`；海岸与人物可以从 Studio 的 `SeasideSkate-Layers` 文件夹打开独立时间线。当前预览已完成 TypeScript、Composition 发现、关键帧尺寸与音频长度/电平检查，并检查了 Studio 播放和运行日志。
> 
> `npm run still` 生成第 165 帧截图；如需导出 MP4，运行 `npm run render`，输出 `outputs/seaside-skate.mp4`。迁移前交付为可播放预览；迁移时已补齐独立项目的完整 MP4。

## 证据清单与缺失信息

盘点的文本/报告文件 25 个，源文本、报告及媒体产物身份摘要为 `a5c22eb8e62588784868f2ece5a7deb910d4d762b2ae16d97d5cc9596bac7cef`。哈希证明采集内容身份，不代表其中每个断言已验证。模型权重、缓存、依赖、第三方源码树和未指定的历史会话未采集；大型文本仅做哈希并按需要选读。

- 本次整理基于明确引用的 README、制作方法、配置、提示词、关键源码与已有报告。未重新观看全部参考/候选/成片，也未做人工听辨。
- 项目未提供的原始模型内部实现、未披露生成后端、未记录 seed/参数、真实费用、人工总工时和用户最终验收，均不补造。实际日志有记录的耗时见上文，流水线耗时不等于整个制作总耗时。
- 源项目内的路径只用于查证；本档案未复制大型素材或模型，也不改变源项目运行依赖。

| 已盘点证据文件 | SHA-256（前 16 位）/状态 |
| --- | --- |
| `videos/seaside-skate/README.md` | `1c49ba899a223b9e` |
| `videos/seaside-skate/package-lock.json` | `09f2f5bbf5ea176a` |
| `videos/seaside-skate/package.json` | `a064076bfb7078a9` |
| `videos/seaside-skate/project.json` | `54ea4ca00169c11e` |
| `videos/seaside-skate/remotion.config.ts` | `fe968ee4188f28ba` |
| `videos/seaside-skate/tsconfig.json` | `2cfb14dc584e98ab` |
| `videos/seaside-skate/docs/production-notes.md` | `d3ad6fe4320d8854` |
| `videos/seaside-skate/outputs/seaside-audio-check.json` | `ca21ce757609198b` |
| `videos/seaside-skate/outputs/seaside-preview-report.json` | `1a064826266b5e8d` |
| `videos/seaside-skate/outputs/checks/archive-report.json` | `6df953a0f4a3dec4` |
| `videos/seaside-skate/outputs/checks/full-export-report.json` | `f9e1372392914a69` |
| `videos/seaside-skate/outputs/checks/isolation-report.json` | `4a7329b933930cd7` |
| `videos/seaside-skate/outputs/checks/media-verification.json` | `3799d17c30f57e27` |
| `videos/seaside-skate/outputs/checks/migration-report.json` | `321fb778bdee10a9` |
| `videos/seaside-skate/public/seaside/asset-prompts.json` | `f6022051aa6a75f2` |
| `videos/seaside-skate/scripts/check-isolation.mjs` | `f7d2e18736bae672` |
| `videos/seaside-skate/scripts/create-seaside-audio.py` | `a7fe40b648ac91b3` |
| `videos/seaside-skate/scripts/package-project.py` | `62fc9770e7659b40` |
| `videos/seaside-skate/scripts/remotion.mjs` | `a1a87d7bd574cc39` |
| `videos/seaside-skate/scripts/verify-media.mjs` | `3ba189cbc90be977` |
| `videos/seaside-skate/src/Root.tsx` | `74d1b52080ad525d` |
| `videos/seaside-skate/src/SeasideSkate.tsx` | `ab7bc95f2d50cbd5` |
| `videos/seaside-skate/src/index.ts` | `5462d3268136731c` |
| `videos/seaside-skate/src/seaside/CoastalWorld.tsx` | `da302837879924fb` |
| `videos/seaside-skate/src/seaside/Skater.tsx` | `959c91517952acfe` |
