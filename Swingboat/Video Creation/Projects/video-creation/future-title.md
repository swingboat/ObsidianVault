---
type: video-production-dossier
project: video-creation
video_id: future-title
captured: '2026-11-12'
status: 已有完整 MP4；原说明含预览阶段记录
source_fingerprint: fad2864b7e77ded140b7e28f47dd5bfc9041dfb79fd2e40bac65f0386b781400
tags:
- video-creation
- production-capture
---

# 未来科技标题 · 粒子聚合与 Logo

<!-- capture-source-sha256:fad2864b7e77ded140b7e28f47dd5bfc9041dfb79fd2e40bac65f0386b781400 -->

本次为指定项目的材料回溯与归档。制作结论来自项目记录/源码，未找回关闭的制作对话，也未重新执行生成、渲染、音频合成或用户验收。

## 当前结论与制作路线

**状态：已有完整 MP4；原说明含预览阶段记录。**

字体加载 → Canvas 文字采样 → 粒子插值与光效 → Logo 出场。

1920×1080、30fps、300 帧。0–3.3 秒粒子聚合，3.3–6.15 秒停留，随后消散并在末段显示 Logo。标题、副标题与 Logo 在 defaultProps 或 Studio 控制。先等待本地 MiSans 完成加载，再采样文字轮廓。

## 关键算法、实现与适用边界

`ParticleTitle.tsx` 在 Canvas 绘制文字，通过 getImageData 获取像素，采样得到粒子目标点；使用带字符串种子的 Remotion random 产生各粒子的初始半径、角度、延迟和透明度。按当前帧插值，从散布位置靠近文字，再叠加正弦旋动并消散。背景网格与星点也由帧驱动。

## 声音与字幕

未发现本片独立声音制作记录；不能由视觉标题动画推断存在音乐。

## 失败、修正与验收边界

未加载指定字体就采样会改变文字轮廓；项目通过等待字体解决。早期 production-notes 将预览作为交付，README 更新说明迁移后补齐完整 MP4，应区分阶段。

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
| `@remotion/fonts` | `4.0.534` | `4.0.534` |
| `@types/react` | `19.2.7` | `19.2.7` |
| `@types/react-dom` | `19.2.3` | `19.2.3` |
| `typescript` | `5.9.3` | `5.9.3` |

工具底层模型未公开时按“未披露”记录；不能由 imagegen 工具名猜测图像模型。未提供真实推理日志的配置不记作已运行。

## 镜头、时间轴与采用参数

> [!info]- 当前 Composition / 时间轴源码：videos/future-title/src/Root.tsx，L1–27
> ```tsx
> import {Composition, Folder} from 'remotion';
> import {FutureTitle} from './FutureTitle';
> import {ParticleTitle} from './future/ParticleTitle';
> import {LogoReveal} from './future/LogoReveal';
> import {TechBackground} from './future/TechBackground';
> 
> export const RemotionRoot = () => (
>   <>
>   <Composition
>     id="FutureTitle"
>     component={FutureTitle}
>     durationInFrames={300}
>     fps={30}
>     width={1920}
>     height={1080}
>     defaultProps={{title: '未来，正在发生', subtitle: '向下一刻，出发', logoText: 'FUTURE', logoSrc: ''}}
>   />
>   <Folder name="FutureTitle-Layers">
>     <Composition id="ParticleTitle" component={ParticleTitle} width={1920} height={1080}
>       fps={30} durationInFrames={222} defaultProps={{title: '未来，正在发生', subtitle: '向下一刻，出发'}} />
>     <Composition id="LogoReveal" component={LogoReveal} width={1920} height={1080}
>       fps={30} durationInFrames={84} defaultProps={{logoText: 'FUTURE', logoSrc: ''}} />
>     <Composition id="TechBackground" component={TechBackground} width={1920} height={1080}
>       fps={30} durationInFrames={300} />
>   </Folder>
>   </>
> );
> ```

源时间轴、生成帧数、所用区间与最终时长分别记录；历史文档或候选参数不自动覆盖当前采用配置。

## 参考来源与借鉴说明

在本次盘点的项目自有材料中，未找到可明确归属本片的外部参考视频或 X 帖子链接；这不等于历史上没有参考。

### 项目记录的技术、内容或素材来源

这些 URL 来自项目文件，尚不自动证明本片采用；除本节明确标注的访问外，本轮未逐一在线重读。

| 链接 | 记录文件 |
| --- | --- |
| [hyperos.mi.com](https://hyperos.mi.com/font/download) | `videos/future-title/docs/production-notes.md` |
| [hyperos.mi.com](https://hyperos.mi.com/font-download/MiSans.zip) | `videos/future-title/public/fonts/misans-source.json` |

## 重现入口与修改位置

以下是 package.json 中的实际命令映射，供在对应视频目录使用；本轮未执行这些制作命令。系统依赖、前置模型/素材和环境准备见后附 README。

| npm 入口 | 实际命令 |
| --- | --- |
| `npm run dev` | `node scripts/remotion.mjs studio src/index.ts --no-open --port=3213` |
| `npm run compositions` | `node scripts/remotion.mjs compositions src/index.ts` |
| `npm run typecheck` | `tsc --noEmit` |
| `npm run render` | `node scripts/remotion.mjs render src/index.ts FutureTitle outputs/future-title.mp4 --codec=h264 --audio-codec=aac --pixel-format=yuv420p --concurrency=2` |
| `npm run still` | `node scripts/remotion.mjs still src/index.ts FutureTitle outputs/checks/current.png --frame=135` |
| `npm run check` | `npm run typecheck && node scripts/check-isolation.mjs` |
| `npm run pack` | `python3 scripts/package-project.py` |
| `npm run verify` | `node scripts/verify-media.mjs` |

### 算法源码摘录

> [!info]- videos/future-title/src/future/ParticleTitle.tsx，L1–141
> ```tsx
> import {useLayoutEffect, useMemo, useRef, type CSSProperties} from 'react';
> import {AbsoluteFill, Easing, Interactive, interpolate, random, useCurrentFrame, useVideoConfig} from 'remotion';
> import {useMiSans} from './fonts';
> 
> type TitleProps = {readonly title: string; readonly subtitle: string; readonly style?: CSSProperties};
> type Particle = {x: number; y: number; startX: number; startY: number; delay: number; size: number; phase: number; alpha: number; tint: string};
> 
> const createParticles = (title: string): {particles: Particle[]; fontSize: number; spacing: number} => {
>   const canvas = document.createElement('canvas');
>   canvas.width = 1920;
>   canvas.height = 1080;
>   const context = canvas.getContext('2d');
>   if (!context) throw new Error('Canvas 2D is required for particle typography');
>   const characters = Array.from(title);
>   const fontSize = Math.min(174, 1500 / Math.max(characters.length, 1));
>   const spacing = 12;
>   context.font = `700 ${fontSize}px MiSans`;
>   context.textBaseline = 'middle';
>   context.fillStyle = '#fff';
>   const textWidth = characters.reduce((width, character) => width + context.measureText(character).width, 0)
>     + Math.max(0, characters.length - 1) * spacing;
>   let x = (1920 - textWidth) / 2;
>   for (const character of characters) {
>     context.fillText(character, x, 480);
>     x += context.measureText(character).width + spacing;
>   }
>   const pixels = context.getImageData(0, 0, 1920, 1080).data;
>   const particles: Particle[] = [];
>   for (let y = 360; y < 600; y += 5) {
>     for (let targetX = 120; targetX < 1800; targetX += 5) {
>       if (pixels[(y * 1920 + targetX) * 4 + 3] < 150) continue;
>       const i = particles.length;
>       const angle = random(`angle-${i}`) * Math.PI * 2;
>       const radius = 530 + random(`radius-${i}`) * 850;
>       particles.push({
>         x: targetX,
>         y,
>         startX: 960 + Math.cos(angle) * radius,
>         startY: 480 + Math.sin(angle) * radius * 0.6,
>         delay: random(`delay-${i}`) * 0.62,
>         size: 1.25 + random(`size-${i}`) * 1.3,
>         phase: random(`phase-${i}`) * Math.PI * 2,
>         alpha: 0.45 + random(`particle-alpha-${i}`) * 0.55,
>         tint: i % 5 === 0 ? '#e8edff' : targetX < 960 ? '#68caff' : '#ae8bff',
>       });
>     }
>   }
>   return {particles, fontSize, spacing};
> };
> 
> const ParticleTitleInner = ({title, subtitle, style}: TitleProps) => {
>   const frame = useCurrentFrame();
>   const {fps} = useVideoConfig();
>   const fontsReady = useMiSans();
>   const canvasRef = useRef<HTMLCanvasElement>(null);
>   const {particles, fontSize, spacing} = useMemo(() => fontsReady ? createParticles(title)
>     : {particles: [], fontSize: 174, spacing: 12}, [title, fontsReady]);
>   const dissolve = interpolate(frame, [6.3 * fps, 7.35 * fps], [0, 1], {extrapolateLeft: 'clamp', extrapolateRight: 'clamp'});
>   const particleOpacity = interpolate(frame, [0, 0.35 * fps, 2.85 * fps, 3.4 * fps, 6.15 * fps, 6.5 * fps, 7.35 * fps],
>     [0, 1, 1, 0, 0, 0.85, 0], {extrapolateLeft: 'clamp', extrapolateRight: 'clamp'});
> 
>   // A single canvas avoids updating thousands of DOM nodes during Studio playback.
>   useLayoutEffect(() => {
>     const context = canvasRef.current?.getContext('2d');
>     if (!context) return;
>     context.clearRect(0, 0, 1920, 1080);
>     if (particleOpacity === 0) return;
>     for (const particle of particles) {
>       const progress = interpolate(frame, [(0.35 + particle.delay) * fps, (2.65 + particle.delay * 0.3) * fps], [0, 1], {
>         easing: Easing.bezier(0.18, 0.75, 0.18, 1), extrapolateLeft: 'clamp', extrapolateRight: 'clamp',
>       });
>       const swirl = Math.sin(progress * Math.PI) * 85;
>       const x = particle.startX + (particle.x - particle.startX) * progress + Math.sin(particle.phase + progress * 4) * swirl
>         + dissolve * (particle.startX - particle.x) * 0.32;
>       const y = particle.startY + (particle.y - particle.startY) * progress + Math.cos(particle.phase + progress * 4) * swirl
>         + dissolve * (particle.startY - particle.y) * 0.32;
>       context.globalAlpha = particleOpacity * particle.alpha;
>       context.fillStyle = particle.tint;
>       context.beginPath();
>       context.arc(x, y, particle.size, 0, Math.PI * 2);
>       context.fill();
>     }
>   }, [frame, fps, particles, particleOpacity, dissolve]);
> 
>   return (
>     <AbsoluteFill style={{fontFamily: 'MiSans', ...style}}>
>       <canvas ref={canvasRef} width={1920} height={1080} style={{position: 'absolute', width: '100%', height: '100%'}} />
>       <svg width="1920" height="1080" viewBox="0 0 1920 1080" style={{position: 'absolute'}}>
>         <defs>
>           <linearGradient id="future-title-gradient" x1="0" x2="1">
>             <stop stopColor="#b7e8ff" />
>             <stop offset="0.4" stopColor="#d7e4ff" />
>             <stop offset="1" stopColor="#c7a5ff" />
>           </linearGradient>
>           <linearGradient id="future-rule-gradient">
>             <stop stopColor="#44bfff" stopOpacity="0" />
>             <stop offset="0.5" stopColor="#8e94ff" />
>             <stop offset="1" stopColor="#a475ff" stopOpacity="0" />
>           </linearGradient>
>           <filter id="future-title-glow" x="-20%" y="-40%" width="140%" height="180%">
>             <feGaussianBlur stdDeviation="9" />
>           </filter>
>         </defs>
>         <g opacity={interpolate(frame, [2.7 * fps, 3.3 * fps, 6.15 * fps, 6.6 * fps], [0, 1, 1, 0], {
>           extrapolateLeft: 'clamp', extrapolateRight: 'clamp',
>         })}>
>           <text x="960" y="480" textAnchor="middle" dominantBaseline="central"
>             fontFamily="MiSans" fontWeight="700" fontSize={fontSize} letterSpacing={spacing}
>             fill="#7798ff" opacity="0.25" filter="url(#future-title-glow)">{title}</text>
>           <text x="960" y="480" textAnchor="middle" dominantBaseline="central"
>             fontFamily="MiSans" fontWeight="700" fontSize={fontSize} letterSpacing={spacing}
>             fill="url(#future-title-gradient)">{title}</text>
>         </g>
>         <path d="M 710 594 L 1210 594" stroke="url(#future-rule-gradient)" strokeWidth="1.3"
>           opacity={interpolate(frame, [2.7 * fps, 3.8 * fps, 6 * fps, 6.6 * fps], [0, 1, 1, 0], {
>             extrapolateLeft: 'clamp', extrapolateRight: 'clamp',
>           })} />
>       </svg>
>       <Interactive.Div name="中文副标题" premountFor={fps} style={{
>         position: 'absolute', top: 625, width: '100%', textAlign: 'center', color: '#a6b6d9',
>         fontSize: 40, letterSpacing: 12, fontWeight: 400,
>         opacity: interpolate(frame, [3.1 * fps, 4.1 * fps, 6.1 * fps, 6.6 * fps], [0, 1, 1, 0], {
>           extrapolateLeft: 'clamp', extrapolateRight: 'clamp',
>         }),
>         translate: interpolate(frame, [3.1 * fps, 4.1 * fps], ['0px 18px', '0px 0px'], {
>           easing: Easing.bezier(0.16, 1, 0.3, 1), extrapolateLeft: 'clamp', extrapolateRight: 'clamp',
>         }),
>       }}>{subtitle}</Interactive.Div>
>     </AbsoluteFill>
>   );
> };
> 
> export const ParticleTitle = Interactive.withSchema({
>   Component: ParticleTitleInner,
>   componentName: '<ParticleTitle>',
>   schema: {
>     title: {type: 'text-content', default: '未来，正在发生', description: '中文主标题'},
>     subtitle: {type: 'text-content', default: '向下一刻，出发', description: '中文副标题'},
>   },
>   wrapInSequence: true,
> });
> ```

> [!info]- videos/future-title/src/future/TechBackground.tsx，L1–96
> ```tsx
> import type {CSSProperties} from 'react';
> import {AbsoluteFill, Interactive, random, useCurrentFrame, useVideoConfig} from 'remotion';
> 
> type BackgroundProps = {readonly style?: CSSProperties};
> 
> const stars = Array.from({length: 95}, (_, i) => ({
>   x: random(`star-x-${i}`) * 1920,
>   y: random(`star-y-${i}`) * 920,
>   radius: 0.6 + random(`star-r-${i}`) * 1.4,
>   phase: random(`star-phase-${i}`) * Math.PI * 2,
> }));
> 
> const TechBackgroundInner = ({style}: BackgroundProps) => {
>   const frame = useCurrentFrame();
>   const {fps} = useVideoConfig();
>   const time = frame / fps;
> 
>   return (
>     <AbsoluteFill style={{background: '#040713', overflow: 'hidden', ...style}}>
>       <svg width="1920" height="1080" viewBox="0 0 1920 1080" style={{position: 'absolute'}}>
>         <defs>
>           <radialGradient id="future-left-glow">
>             <stop stopColor="#1269ff" stopOpacity="0.28" />
>             <stop offset="1" stopColor="#1269ff" stopOpacity="0" />
>           </radialGradient>
>           <radialGradient id="future-right-glow">
>             <stop stopColor="#9649ff" stopOpacity="0.31" />
>             <stop offset="1" stopColor="#9649ff" stopOpacity="0" />
>           </radialGradient>
>           <linearGradient id="future-stream" x1="0" x2="1" y1="0" y2="0">
>             <stop stopColor="#258aff" stopOpacity="0" />
>             <stop offset="0.3" stopColor="#4ac4ff" />
>             <stop offset="0.7" stopColor="#ab6aff" />
>             <stop offset="1" stopColor="#9c43ff" stopOpacity="0" />
>           </linearGradient>
>           <linearGradient id="future-floor" x1="0" x2="0" y1="0" y2="1">
>             <stop stopColor="#7974ff" stopOpacity="0.03" />
>             <stop offset="0.6" stopColor="#636fff" stopOpacity="0.30" />
>             <stop offset="1" stopColor="#48a4ff" stopOpacity="0.12" />
>           </linearGradient>
>           <pattern id="future-grid" width="80" height="80" patternUnits="userSpaceOnUse"
>             patternTransform={`translate(${time * 3} ${time * 6})`}>
>             <path d="M 80 0 L 0 0 0 80" fill="none" stroke="#6374cd" strokeWidth="0.75" strokeOpacity="0.12" />
>             <circle cx="0" cy="0" r="1.5" fill="#7999e4" opacity="0.15" />
>           </pattern>
>           <filter id="future-ribbon-blur" x="-20%" y="-100%" width="140%" height="300%">
>             <feGaussianBlur stdDeviation="16" />
>           </filter>
>           <radialGradient id="future-vignette">
>             <stop offset="0.2" stopColor="#000" stopOpacity="0" />
>             <stop offset="1" stopColor="#000" stopOpacity="0.6" />
>           </radialGradient>
>         </defs>
>         <ellipse cx={560 + Math.sin(time * 0.5) * 140} cy="485" rx="830" ry="670" fill="url(#future-left-glow)" />
>         <ellipse cx={1440 + Math.cos(time * 0.45) * 130} cy="465" rx="750" ry="670" fill="url(#future-right-glow)" />
>         <rect width="1920" height="1080" fill="url(#future-grid)" />
>         {stars.map((star, i) => (
>           <circle key={i} cx={star.x} cy={star.y - time * (i % 3 + 1)} r={star.radius}
>             fill={i % 3 ? '#99baff' : '#c2a0ff'} opacity={0.18 + (Math.sin(time * 0.7 + star.phase) + 1) * 0.18} />
>         ))}
>         <g fill="none" stroke="url(#future-floor)">
>           {Array.from({length: 25}, (_, i) => (
>             <path key={i} d={`M ${960 + (i - 12) * 30} 635 L ${(i - 12) * 190 + 960} 1180`} strokeWidth="1.2" />
>           ))}
>           {Array.from({length: 15}, (_, i) => {
>             const distance = ((i / 15 + time * 0.035) % 1);
>             const y = 638 + distance * distance * 530;
>             return <path key={i} d={`M 0 ${y} L 1920 ${y}`} strokeWidth={0.5 + distance} opacity={0.25 + distance * 0.65} />;
>           })}
>         </g>
>         {[0, 1, 2].map((i) => {
>           const y = 640 + i * 95 + Math.sin(time * 0.6 + i) * 35;
>           const d = `M -180 ${y + 220} C 420 ${y - 380} 760 ${y + 180} 1120 ${y - 70} S 1720 ${y - 210} 2140 ${y - 40}`;
>           return (
>             <g key={i} fill="none">
>               <path d={d} stroke="url(#future-stream)" strokeWidth="17" opacity="0.20" filter="url(#future-ribbon-blur)" />
>               <path d={d} stroke="url(#future-stream)" strokeWidth={i === 0 ? 2.2 : 1.1} opacity={0.4 - i * 0.09} />
>               <path d={d} stroke="#b8ddff" strokeWidth="2.4" opacity="0.5"
>                 strokeDasharray="18 1900" strokeDashoffset={-time * 260 - i * 500} />
>             </g>
>           );
>         })}
>         <ellipse cx="960" cy="585" rx="805" ry="205" fill="none" stroke="url(#future-stream)"
>           strokeWidth="1" opacity="0.15" transform={`rotate(${-12 + Math.sin(time * 0.3) * 2} 960 585)`} />
>         <rect width="1920" height="1080" fill="url(#future-vignette)" />
>       </svg>
>     </AbsoluteFill>
>   );
> };
> 
> export const TechBackground = Interactive.withSchema({
>   Component: TechBackgroundInner,
>   componentName: '<TechBackground>',
>   schema: {},
>   wrapInSequence: true,
> });
> ```

## 完整关键提示词与生成记录

以下保留项目自有提示词、候选参数与版本。存在失败候选和历史参数；最终采用以 selected-takes、edit-decisions 与制作复盘为准，不能将所有候选都写成已采用。

未发现实际生成提示词或推理执行记录；不从脚手架、模型安装清单推测生成过程。

## 已有产物与检查依据

文件存在、技术检查、画面检查与用户验收分别看待。下表是本次文件盘点，旧版与当前版可能共存。MP4/HTML 按内容计算 SHA-256，ZIP 仅记录文件大小和修改时间供增量判断。

| 本地产物 | 字节数 | SHA-256 |
| --- | ---: | --- |
| `videos/future-title/outputs/future-title-project.zip` | 26314875 | `ZIP：仅元数据` |
| `videos/future-title/outputs/future-title.mp4` | 6807337 | `b3cc987b4b3cbf4461736d82570a7e05749ce92afc46d0d84a5f60c6ad025fac` |

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
>     }
>   ],
>   "format": {
>     "duration": "10.000000",
>     "size": "6807337"
>   },
>   "file": "outputs/future-title.mp4",
>   "scope": "本次只读取媒体头；未全片解码、播放或重新进行视觉/听觉验收。"
> }
> ```


已有检查报告路径（未在本轮重新执行）：

- `videos/future-title/outputs/checks/archive-report.json`
- `videos/future-title/outputs/checks/full-export-report.json`
- `videos/future-title/outputs/checks/isolation-report.json`
- `videos/future-title/outputs/checks/media-verification.json`
- `videos/future-title/outputs/checks/migration-report.json`

## 制作方法与迭代记录选编

下面是项目自有文档的归档摘录，保留原阶段与原检查表述，个人路径已泛化、相对链接已注明源项目位置。它们属于来源陈述，不代表本次重新完成其中的生成或验收。

> [!info]- 来源文档：videos/future-title/README.md
> # FutureTitle
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
> 预览地址：http://localhost:3213/FutureTitle。最终视频输出至 `outputs/future-title.mp4`；缓存和临时文件位于 `.cache/`。本目录的 `package-lock.json` 锁定依赖，npm 缓存和浏览器均在本目录内。项目不能依赖其他视频或上层工作区文件。
> 
> 修改 `src/FutureTitle.tsx` 及本视频的子组件、`public/` 素材；Composition 注册与时长在 `src/Root.tsx`。字体为本目录 `public/fonts/` 中的 MiSans。
> 
> 迁移后已从独立项目导出完整 MP4，保存在 `outputs/`。需要重新导出时执行 `npm run render`。
> 
> 原始制作说明及可编辑参数见 docs/production-notes.md（项目文件：`videos/future-title/docs/production-notes.md`）。

> [!info]- 来源文档：videos/future-title/docs/production-notes.md
> ## 10 秒未来科技标题动画
> 
> 新增 `FutureTitle`：1920×1080，30 fps，共 300 帧（10 秒）。在 Codex 内置浏览器打开 [动画预览](http://localhost:3213/FutureTitle)，点击播放即可观看；若服务停止，运行 `npm run dev`。
> 
> 动画节奏：0–3.3 秒蓝紫粒子聚合为中文标题；3.3–6.15 秒标题停留；6.15–7.4 秒粒子消散；7.2–10 秒 Logo 缩放出现。背景包含流动光带、漂移网格、透视地面网格和星点，所有运动由当前帧驱动。
> 
> 当前文案为“未来，正在发生”，副标题为“向下一刻，出发”，结尾使用可替换的 `FUTURE` 几何占位 Logo。可以在 Studio 右侧 Default Props 修改 `title`、`subtitle`、`logoText`、`logoSrc`，或直接编辑 `src/Root.tsx` 中 `FutureTitle` 的 `defaultProps`。自己的 Logo 放入 `public/`，`logoSrc` 填相对该目录的路径，例如 `logo.png`。
> 
> MiSans Regular / Bold 原始 WOFF2 文件来自[小米官方 MiSans 下载](https://hyperos.mi.com/font/download)，保存于本项目 `public/fonts/`，来源和 SHA-256 见该目录的 `misans-source.json`。粒子轮廓在字体加载完成后采样，避免预览和渲染使用替代字体。
> 
> 关键帧：`outputs/future-title-particles.png`（2.4 秒）、`outputs/future-title-main.png`（4.5 秒）、`outputs/future-title-logo.png`（9 秒）。已通过 TypeScript 检查、Composition 发现与关键帧渲染验证，并在 Studio 中播放检查。预览是本次交付；如需 MP4，可运行 `npm run render`，输出 `outputs/future-title.mp4`。重新生成标题截图可运行 `npm run still`。

## 证据清单与缺失信息

盘点的文本/报告文件 25 个，源文本、报告及媒体产物身份摘要为 `fad2864b7e77ded140b7e28f47dd5bfc9041dfb79fd2e40bac65f0386b781400`。哈希证明采集内容身份，不代表其中每个断言已验证。模型权重、缓存、依赖、第三方源码树和未指定的历史会话未采集；大型文本仅做哈希并按需要选读。

- 本次整理基于明确引用的 README、制作方法、配置、提示词、关键源码与已有报告。未重新观看全部参考/候选/成片，也未做人工听辨。
- 项目未提供的原始模型内部实现、未披露生成后端、未记录 seed/参数、真实费用、人工总工时和用户最终验收，均不补造。实际日志有记录的耗时见上文，流水线耗时不等于整个制作总耗时。
- 源项目内的路径只用于查证；本档案未复制大型素材或模型，也不改变源项目运行依赖。

| 已盘点证据文件 | SHA-256（前 16 位）/状态 |
| --- | --- |
| `videos/future-title/README.md` | `dcfae4e8b689222a` |
| `videos/future-title/package-lock.json` | `d3ab4722ca4919c7` |
| `videos/future-title/package.json` | `d8073c45bd323fbb` |
| `videos/future-title/project.json` | `c5770cf68d9c0cad` |
| `videos/future-title/remotion.config.ts` | `fe968ee4188f28ba` |
| `videos/future-title/tsconfig.json` | `2cfb14dc584e98ab` |
| `videos/future-title/docs/production-notes.md` | `f5e9f7d60c38e589` |
| `videos/future-title/outputs/future-title-preview-report.json` | `f24592f5b14d9101` |
| `videos/future-title/outputs/checks/archive-report.json` | `03513d40de2a4c8b` |
| `videos/future-title/outputs/checks/full-export-report.json` | `e4d98bb8914a9f8f` |
| `videos/future-title/outputs/checks/isolation-report.json` | `20c4861071990606` |
| `videos/future-title/outputs/checks/media-verification.json` | `35bc9617013cba7f` |
| `videos/future-title/outputs/checks/migration-report.json` | `d510ef2cdc1f897e` |
| `videos/future-title/public/fonts/misans-source.json` | `f675e829b466ffa8` |
| `videos/future-title/scripts/check-isolation.mjs` | `f7d2e18736bae672` |
| `videos/future-title/scripts/package-project.py` | `62fc9770e7659b40` |
| `videos/future-title/scripts/remotion.mjs` | `a1a87d7bd574cc39` |
| `videos/future-title/scripts/verify-media.mjs` | `3ba189cbc90be977` |
| `videos/future-title/src/FutureTitle.tsx` | `63583d728344d18c` |
| `videos/future-title/src/Root.tsx` | `e15f83b184afad0b` |
| `videos/future-title/src/index.ts` | `5462d3268136731c` |
| `videos/future-title/src/future/LogoReveal.tsx` | `b0e1974b307e95f4` |
| `videos/future-title/src/future/ParticleTitle.tsx` | `32853d38a5d0e76a` |
| `videos/future-title/src/future/TechBackground.tsx` | `2b320489e6e87c03` |
| `videos/future-title/src/future/fonts.ts` | `13ee436fdd06e832` |
