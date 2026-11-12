---
type: video-production-dossier
project: video-creation
video_id: dental-clinic-city
captured: '2026-11-12'
status: 已有完整 MP4；城市为位置示意
source_fingerprint: 0b034782143f9644884b33849c27841a439bda6818213c3d745965339d2bc067
tags:
- video-creation
- production-capture
---

# 口腔门诊宣传 · 数据驱动 3D 城市

<!-- capture-source-sha256:0b034782143f9644884b33849c27841a439bda6818213c3d745965339d2bc067 -->

本次为指定项目的材料回溯与归档。制作结论来自项目记录/源码，未找回关闭的制作对话，也未重新执行生成、渲染、音频合成或用户验收。

## 当前结论与制作路线

**状态：已有完整 MP4；城市为位置示意。**

商家 JSON → Three.js 基础几何 → 帧驱动相机与 UI → 竖屏导出。

1080×1920、30fps、30 秒。按城市俯瞰、位置、卖点、联系四阶段推进。商家文字与配色由 public/business/clinic.json 提供；建筑、树、门店和标记由 box、plane、sphere 构成，未下载人物或城市模型。

## 关键算法、实现与适用边界

ThreeCanvas 和 React Three Fiber 承载 Three.js 场景，全部动画用 useCurrentFrame 计算，不依赖实时 useFrame。门店 [x,z] 为地面坐标；本地字体渲染到 Canvas 成为招牌纹理。相机位置、建筑进入、发光与文案显隐按阶段插值，重复渲染同一帧用于验证确定性。

## 声音与字幕

未发现独立配音或配乐制作记录。

## 失败、修正与验收边界

地址为占位信息，城市不是杭州真实地图。Fiber 的内部 Clock 存在弃用提示；项目记录重复帧一致性通过，但这不代表依赖完全无警告。

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
| `@react-three/fiber` | `9.8.1` | `9.8.1` |
| `@remotion/three` | `4.0.534` | `4.0.534` |
| `three` | `0.186.1` | `0.186.1` |
| `@types/react` | `19.2.7` | `19.2.7` |
| `@types/react-dom` | `19.2.3` | `19.2.3` |
| `typescript` | `5.9.3` | `5.9.3` |
| `@types/three` | `0.186.0` | `0.186.0` |

工具底层模型未公开时按“未披露”记录；不能由 imagegen 工具名猜测图像模型。未提供真实推理日志的配置不记作已运行。

## 镜头、时间轴与采用参数

> [!info]- 当前 Composition / 时间轴源码：videos/dental-clinic-city/src/Root.tsx，L1–21
> ```tsx
> import {Composition, Folder} from 'remotion';
> import {DentalClinicCity} from './DentalClinicCity';
> import {CityScene} from './clinic/CityScene';
> import {CityIntro} from './clinic/CityIntro';
> import {CityLocation} from './clinic/CityLocation';
> import {ClinicBenefits} from './clinic/ClinicBenefits';
> import {ClinicContact} from './clinic/ClinicContact';
> 
> export const RemotionRoot = () => (
>   <>
>   <Composition id="DentalClinicCity" component={DentalClinicCity} durationInFrames={900} fps={30} width={1080} height={1920}
>     defaultProps={{configSrc: 'business/clinic.json'}} />
>   <Folder name="DentalClinic-Layers">
>     <Composition id="CityScene" component={CityScene} durationInFrames={900} fps={30} width={1080} height={1920} defaultProps={{configSrc: 'business/clinic.json'}} />
>     <Composition id="CityIntro" component={CityIntro} durationInFrames={150} fps={30} width={1080} height={1920} defaultProps={{configSrc: 'business/clinic.json'}} />
>     <Composition id="CityLocation" component={CityLocation} durationInFrames={270} fps={30} width={1080} height={1920} defaultProps={{configSrc: 'business/clinic.json'}} />
>     <Composition id="ClinicBenefits" component={ClinicBenefits} durationInFrames={240} fps={30} width={1080} height={1920} defaultProps={{configSrc: 'business/clinic.json'}} />
>     <Composition id="ClinicContact" component={ClinicContact} durationInFrames={240} fps={30} width={1080} height={1920} defaultProps={{configSrc: 'business/clinic.json'}} />
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
| [hyperos.mi.com](https://hyperos.mi.com/font/download) | `videos/dental-clinic-city/public/fonts/misans-source.json` |
| [hyperos.mi.com](https://hyperos.mi.com/font-download/MiSans.zip) | `videos/dental-clinic-city/public/fonts/misans-source.json` |

## 重现入口与修改位置

以下是 package.json 中的实际命令映射，供在对应视频目录使用；本轮未执行这些制作命令。系统依赖、前置模型/素材和环境准备见后附 README。

| npm 入口 | 实际命令 |
| --- | --- |
| `npm run dev` | `node scripts/remotion.mjs studio src/index.ts --no-open --port=3215` |
| `npm run compositions` | `node scripts/remotion.mjs compositions src/index.ts` |
| `npm run typecheck` | `tsc --noEmit` |
| `npm run render` | `node scripts/remotion.mjs render src/index.ts DentalClinicCity outputs/dental-clinic-city.mp4 --codec=h264 --audio-codec=aac --pixel-format=yuv420p --concurrency=2 --gl=angle` |
| `npm run still` | `node scripts/remotion.mjs still src/index.ts DentalClinicCity outputs/checks/current.png --frame=600 --gl=angle` |
| `npm run check` | `npm run typecheck && node scripts/check-isolation.mjs` |
| `npm run pack` | `python3 scripts/package-project.py` |
| `npm run check:clinic` | `npm run typecheck && node scripts/check-clinic.mjs` |
| `npm run verify` | `node scripts/verify-media.mjs` |

### 算法源码摘录

> [!info]- videos/dental-clinic-city/src/clinic/CityScene.tsx，L1–147
> ```tsx
> import {memo, useLayoutEffect, useMemo, useRef} from 'react';
> import {useThree} from '@react-three/fiber';
> import {ThreeCanvas} from '@remotion/three';
> import {AbsoluteFill, Interactive, useCurrentFrame, useVideoConfig} from 'remotion';
> import {CanvasTexture, PCFShadowMap, SRGBColorSpace, type Group, type PerspectiveCamera} from 'three';
> import {useMiSans} from '../fonts';
> import {configSchema, useMerchant, type BuildingConfig, type ClinicProps, type MerchantConfig} from './config';
> import {tween} from './motion';
> 
> const Box = ({position, size, color, glow = 0}: {position: [number, number, number]; size: [number, number, number]; color: string; glow?: number}) => (
>   <mesh position={position} castShadow receiveShadow>
>     <boxGeometry args={size} />
>     <meshStandardMaterial color={color} roughness={0.78} emissive={color} emissiveIntensity={glow} />
>   </mesh>
> );
> 
> const Building = ({item}: {item: BuildingConfig}) => {
>   const floors = Math.max(1, Math.floor(item.height / 0.85));
>   return <group position={[item.position[0], 0, item.position[1]]}>
>     <Box position={[0, item.height / 2, 0]} size={[item.width, item.height, item.depth]} color={item.color} />
>     <Box position={[0, item.height + 0.08, 0]} size={[item.width + 0.08, 0.16, item.depth + 0.08]} color="#eef1eb" />
>     {Array.from({length: floors}, (_, floor) => [0, 1].map((column) => <Box key={`${floor}-${column}`}
>       position={[(column - 0.5) * item.width * 0.46, 0.5 + floor * 0.85, item.depth / 2 + 0.015]}
>       size={[item.width * 0.23, 0.36, 0.045]} color="#91b2af" />))}
>   </group>;
> };
> 
> const StaticCity = memo(({config, rise}: {config: MerchantConfig; rise: number}) => <>
>   <Box position={[0, -0.38, 0]} size={[19, 0.7, 17.8]} color="#dce6de" />
>   <Box position={[0, -0.035, 0]} size={[18.7, 0.07, 17.5]} color="#edf2e9" />
>   {Array.from({length: 19}, (_, i) => <group key={i}>
>     <Box position={[i - 9, 0.015, 0]} size={[0.018, 0.016, 17.4]} color="#d5e1d8" />
>     {i < 18 ? <Box position={[0, 0.015, i - 8.5]} size={[18.6, 0.016, 0.018]} color="#d5e1d8" /> : null}
>   </group>)}
>   <Box position={[0, 0.07, 3.7]} size={[18.6, 0.12, 2.3]} color="#b1c3bd" />
>   <Box position={[0, 0.1, 2.42]} size={[18.6, 0.14, 0.24]} color="#fffdf4" />
>   <Box position={[0, 0.1, 4.98]} size={[18.6, 0.14, 0.24]} color="#fffdf4" />
>   {Array.from({length: 14}, (_, i) => <Box key={i} position={[-8.7 + i * 1.35, 0.139, 3.7]} size={[0.65, 0.018, 0.07]} color="#faf9ee" />)}
>   <Box position={[-0.75, 0.08, -3.6]} size={[1.1, 0.12, 10.2]} color="#c6d4cd" />
>   <group scale={[1, rise, 1]}>{config.buildings.map((item) => <Building key={item.id} item={item} />)}</group>
>   {[-8, -4.9, -1.4, 5, 8].map((x, i) => <group key={x} position={[x, 0, i % 2 ? 1.4 : -3.2]}>
>     <Box position={[0, 0.35, 0]} size={[0.13, 0.7, 0.13]} color="#a89577" />
>     <mesh position={[0, 1.02, 0]} castShadow><sphereGeometry args={[0.61, 8, 6]} /><meshStandardMaterial color="#86b79a" roughness={1} /></mesh>
>   </group>)}
> </>);
> 
> const CameraRig = ({config, time}: {config: MerchantConfig; time: number}) => {
>   const {camera} = useThree();
>   const sx = config.store.position[0];
>   const sz = config.store.position[1];
>   useLayoutEffect(() => {
>     const aerial = tween(time, 0, 5);
>     const approach = tween(time, 14, 21.5);
>     const final = tween(time, 22, 24);
>     const drift = tween(time, 5, 14);
>     const wide = [tween(aerial, 0, 1, 0, 18) - drift, tween(aerial, 0, 1, 36, 20) - drift * 2,
>       tween(aerial, 0, 1, 5, 23) - drift * 1.5];
>     camera.position.set(wide[0] * (1 - approach) + (sx + 7.8 - final) * approach,
>       wide[1] * (1 - approach) + (10.3 - final * 0.7) * approach,
>       wide[2] * (1 - approach) + (sz + 12 - final * 0.5) * approach);
>     camera.lookAt(sx * approach, 0.8 + approach * 1.6 + final * 0.5, sz * approach);
>     (camera as PerspectiveCamera).fov = 43;
>     camera.updateProjectionMatrix();
>   }, [camera, sx, sz, time]);
>   return null;
> };
> 
> const Store = ({config, time}: {config: MerchantConfig; time: number}) => {
>   const {width: w, height: h, depth: d, position} = config.store;
>   const glow = tween(time, 14, 16) * (0.22 + Math.sin(time * 2.4) * 0.06);
>   const texture = useMemo(() => {
>     const canvas = document.createElement('canvas');
>     canvas.width = 1024; canvas.height = 192;
>     const ctx = canvas.getContext('2d')!;
>     ctx.fillStyle = config.theme.brand; ctx.fillRect(0, 0, 1024, 192);
>     ctx.fillStyle = '#ffffff'; ctx.font = '700 108px MiSans'; ctx.textAlign = 'center'; ctx.textBaseline = 'middle';
>     ctx.fillText(config.name, 512, 96, 950);
>     const result = new CanvasTexture(canvas); result.colorSpace = SRGBColorSpace;
>     return result;
>   }, [config.name, config.theme.brand]);
>   useLayoutEffect(() => () => texture.dispose(), [texture]);
>   return <group position={[position[0], 0, position[1]]}>
>     <Box position={[0, h / 2, 0]} size={[w, h, d]} color="#fffdf6" glow={glow} />
>     <Box position={[0, h + 0.1, 0]} size={[w + 0.25, 0.2, d + 0.25]} color={config.theme.brand} glow={glow * 3} />
>     <Box position={[0, 0.15, d / 2 + 0.35]} size={[w + 0.5, 0.25, 0.85]} color="#ffffff" />
>     <Box position={[0, h - 0.46, d / 2 + 0.06]} size={[w + 0.04, 0.61, 0.15]} color={config.theme.brand} glow={glow} />
>     <mesh position={[0, h - 0.46, d / 2 + 0.142]}>
>       <planeGeometry args={[w - 0.2, 0.54]} /><meshBasicMaterial map={texture} />
>     </mesh>
>     <Box position={[-w * 0.28, 1.05, d / 2 + 0.025]} size={[w * 0.28, 1.4, 0.07]} color="#a3d4d2" />
>     <Box position={[w * 0.28, 1.05, d / 2 + 0.025]} size={[w * 0.28, 1.4, 0.07]} color="#a3d4d2" />
>     <Box position={[0, 0.94, d / 2 + 0.05]} size={[w * 0.24, 1.64, 0.09]} color="#6dafb0" />
>     <Box position={[0, 0.94, d / 2 + 0.11]} size={[0.045, 1.64, 0.035]} color="#ffffff" />
>     <Box position={[0.17, 0.92, d / 2 + 0.13]} size={[0.04, 0.24, 0.03]} color="#ffffff" />
>     <Box position={[w / 2 + 0.02, 1.3, 0]} size={[0.035, 1.4, d * 0.55]} color="#acd3d0" />
>     <mesh position={[0, 0.17, 0]} scale={[w * 0.78, 0.04, d * 0.85]}>
>       <sphereGeometry args={[1, 32, 16]} /><meshBasicMaterial color={config.theme.brand} transparent opacity={tween(time, 14, 16, 0, 0.22)} depthWrite={false} />
>     </mesh>
>   </group>;
> };
> 
> const LocationPin = ({config, time}: {config: MerchantConfig; time: number}) => {
>   const {camera} = useThree();
>   const pin = useRef<Group>(null);
>   const appear = tween(time, 5, 5.8);
>   const bob = Math.sin(time * 2) * 0.08;
>   const pulse = (time * 0.6) % 1;
>   useLayoutEffect(() => {pin.current?.quaternion.copy(camera.quaternion);}, [camera, time]);
>   return <group position={[config.store.position[0], config.store.height + 1.9 + bob, config.store.position[1]]} scale={appear}>
>     <mesh castShadow><sphereGeometry args={[0.62, 24, 16]} /><meshStandardMaterial color={config.theme.marker} roughness={0.4} /></mesh>
>     <group ref={pin}>
>       <group position={[0, 0, 0.625]}><mesh><sphereGeometry args={[0.22, 16, 12]} /><meshBasicMaterial color="#fff8f3" /></mesh></group>
>     </group>
>     <Box position={[0, -0.72, 0]} size={[0.11, 0.75, 0.11]} color={config.theme.marker} />
>     <mesh position={[0, -config.store.height - 1.72 - bob, 0]} scale={[1.25 + pulse * 0.7, 0.025, 1.25 + pulse * 0.7]}>
>       <sphereGeometry args={[1, 32, 16]} /><meshBasicMaterial color={config.theme.marker} transparent opacity={(1 - pulse) * 0.15} depthWrite={false} />
>     </mesh>
>   </group>;
> };
> 
> const CitySceneInner = ({configSrc, style}: ClinicProps) => {
>   const config = useMerchant(configSrc);
>   const fontsReady = useMiSans();
>   const frame = useCurrentFrame();
>   const {fps, width} = useVideoConfig();
>   const time = frame / fps;
>   if (!config || !fontsReady) return null;
>   return <AbsoluteFill style={{top: 325, height: 1190, maskImage: 'linear-gradient(to bottom, #000 0%, #000 84%, transparent 100%)', ...style}}>
>     <ThreeCanvas width={width} height={1190} camera={{position: [18, 20, 23], fov: 43}} shadows={{type: PCFShadowMap}}
>       gl={{antialias: true, alpha: true, preserveDrawingBuffer: true}} dpr={1}>
>       <ambientLight intensity={1.6} />
>       <hemisphereLight args={['#ffffff', '#adc9bc', 1.4]} />
>       <directionalLight position={[-12, 24, 14]} intensity={2.6} castShadow shadow-mapSize={[2048, 2048]}
>         shadow-camera-left={-18} shadow-camera-right={18} shadow-camera-top={18} shadow-camera-bottom={-18}
>         shadow-camera-far={80} shadow-bias={-0.0005} />
>       <CameraRig config={config} time={time} />
>       <mesh rotation={[-Math.PI / 2, 0, 0]} position={[0, -0.77, 0]} receiveShadow>
>         <planeGeometry args={[150, 150]} /><shadowMaterial transparent opacity={0.13} />
>       </mesh>
>       <StaticCity config={config} rise={tween(time, 0.6, 3, 0.05, 1)} />
>       <Store config={config} time={time} />
>       <LocationPin config={config} time={time} />
>     </ThreeCanvas>
>   </AbsoluteFill>;
> };
> 
> export const CityScene = Interactive.withSchema({Component: CitySceneInner, componentName: '<CityScene>', wrapInSequence: true, schema: configSchema});
> ```

> [!info]- videos/dental-clinic-city/src/clinic/motion.ts，L1–6
> ```typescript
> import {Easing, interpolate} from 'remotion';
> 
> export const tween = (time: number, start: number, end: number, a = 0, b = 1) => interpolate(time, [start, end], [a, b], {
>   easing: Easing.bezier(0.32, 0, 0.18, 1), extrapolateLeft: 'clamp', extrapolateRight: 'clamp',
> });
> export const fade = (time: number, duration: number) => tween(time, 0, 0.55) * tween(time, duration - 0.45, duration, 1, 0);
> ```

## 完整关键提示词与生成记录

以下保留项目自有提示词、候选参数与版本。存在失败候选和历史参数；最终采用以 selected-takes、edit-decisions 与制作复盘为准，不能将所有候选都写成已采用。

未发现实际生成提示词或推理执行记录；不从脚手架、模型安装清单推测生成过程。

## 已有产物与检查依据

文件存在、技术检查、画面检查与用户验收分别看待。下表是本次文件盘点，旧版与当前版可能共存。MP4/HTML 按内容计算 SHA-256，ZIP 仅记录文件大小和修改时间供增量判断。

| 本地产物 | 字节数 | SHA-256 |
| --- | ---: | --- |
| `videos/dental-clinic-city/outputs/dental-clinic-city-project.zip` | 18943381 | `ZIP：仅元数据` |
| `videos/dental-clinic-city/outputs/dental-clinic-city.mp4` | 4156998 | `330ef53a1f8a4c027b802253e531e196f6b5beef8ce1be298981f0b49c0448d0` |

> [!info]- 本次只读 ffprobe 结果
> ```json
> {
>   "programs": [],
>   "stream_groups": [],
>   "streams": [
>     {
>       "codec_name": "h264",
>       "codec_type": "video",
>       "width": 1080,
>       "height": 1920,
>       "r_frame_rate": "30/1",
>       "duration": "30.000000",
>       "nb_frames": "900"
>     }
>   ],
>   "format": {
>     "duration": "30.000000",
>     "size": "4156998"
>   },
>   "file": "outputs/dental-clinic-city.mp4",
>   "scope": "本次只读取媒体头；未全片解码、播放或重新进行视觉/听觉验收。"
> }
> ```


已有检查报告路径（未在本轮重新执行）：

- `videos/dental-clinic-city/outputs/checks/archive-report.json`
- `videos/dental-clinic-city/outputs/checks/full-export-report.json`
- `videos/dental-clinic-city/outputs/checks/isolation-report.json`
- `videos/dental-clinic-city/outputs/checks/media-verification.json`
- `videos/dental-clinic-city/outputs/checks/migration-report.json`

## 制作方法与迭代记录选编

下面是项目自有文档的归档摘录，保留原阶段与原检查表述，个人路径已泛化、相对链接已注明源项目位置。它们属于来源陈述，不代表本次重新完成其中的生成或验收。

> [!info]- 来源文档：videos/dental-clinic-city/README.md
> # DentalClinicCity
> 
> 独立 Remotion 视频项目：1080×1920，30 fps，30 秒。仅包含本视频的组件、素材与脚本。
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
> 预览地址：http://localhost:3215/DentalClinicCity。最终视频输出至 `outputs/dental-clinic-city.mp4`；缓存和临时文件位于 `.cache/`。本目录的 `package-lock.json` 锁定依赖，npm 缓存和浏览器均在本目录内。项目不能依赖其他视频或上层工作区文件。
> 
> 修改 `src/DentalClinicCity.tsx` 及本视频的子组件、`public/` 素材；Composition 注册与时长在 `src/Root.tsx`。字体为本目录 `public/fonts/` 中的 MiSans。
> 商家配置在 `public/business/clinic.json`；3D 由基础几何体构成。`npm run check:clinic` 检查四阶段、重复帧与配置替换。
> 
> 迁移后已从独立项目导出完整 MP4，保存在 `outputs/`。需要重新导出时执行 `npm run render`。
> 
> 原始制作说明及可编辑参数见 docs/production-notes.md（项目文件：`videos/dental-clinic-city/docs/production-notes.md`）。

> [!info]- 来源文档：videos/dental-clinic-city/docs/production-notes.md
> ## 30 秒竖屏 3D 门诊宣传
> 
> 新增 `DentalClinicCity`：1080×1920、9:16、30 fps、900 帧。打开 [安心口腔门诊预览](http://localhost:3215/DentalClinicCity)，点击播放即可观看。若服务停止，运行 `npm run dev`。
> 
> 0–5 秒从城市上空进入并展示名称；5–14 秒展示网格、道路、建筑和红色定位标记；14–22 秒镜头推进，门店材质发光，三个卖点依次出现；22–30 秒展示名称、地址和“私信预约，到店先检查”。当前地址是用户给出的占位地址，城市模型是位置示意，不对应真实杭州地图。输入中的 `svg` 已按笔误移除。
> 
> 商家数据在 `public/business/clinic.json`：修改 `name`、`city`、`district`、`address`、`services`、三个 `highlights`、`cta` 和 `theme` 后刷新预览，即可更新文案与配色。`store.position`、建筑的 `position` 使用 `[x, z]` 地面坐标；尺寸和高度使用统一场景单位。`buildings` 数组控制楼房位置、尺寸和颜色。换门店时请同时避开其他建筑的位置。
> 
> 也可以复制 JSON 到 `public/business/other.json`，在 `src/Root.tsx` 中将 `DentalClinicCity` 的 `defaultProps.configSrc` 改为 `business/other.json`，或通过导出参数选择数据文件：
> 
> ```sh
> npm run render -- --props='{"configSrc":"business/other.json"}'
> ```
> 
> 代码入口是 `src/DentalClinicCity.tsx`。`src/clinic/CityScene.tsx` 使用 `ThreeCanvas`、React Three Fiber 和 Three.js；城市、树木、门店、定位点均由 box、plane、sphere 构成，招牌文字是本地 MiSans 生成的 canvas 纹理，没有下载 3D 模型。相机位置、标记、建筑进入、发光和卡片动画均由 `useCurrentFrame()` 驱动，没有使用 `useFrame()`。四个文案阶段和连续 3D 镜头分别注册在 `DentalClinic-Layers` 中，可单独查看时间线。
> 
> `@remotion/three@4.0.534`、`@react-three/fiber@9.8.1`、`three@0.186.1` 和 `@types/three@0.186.0` 均安装在当前项目。当前版本的 Fiber 使用 Three.js 内部 Clock 会输出弃用提示；它不参与本项目的动画计算，重复帧渲染已经验证一致。
> 
> `npm run check:clinic` 执行类型检查、Composition 参数检查、四段关键帧渲染、重复帧一致性与另一个 JSON 的替换验证。报告为 `outputs/clinic-check-report.json`。关键帧为 `outputs/clinic-intro.png`、`outputs/clinic-location.png`、`outputs/clinic-benefits.png` 和 `outputs/clinic-contact.png`。当前交付是可播放的 Studio 预览；需要 MP4 时运行 `npm run render`，输出 `outputs/dental-clinic-city.mp4`。`npm run still` 可以单独重新生成卖点镜头截图。

## 证据清单与缺失信息

盘点的文本/报告文件 32 个，源文本、报告及媒体产物身份摘要为 `0b034782143f9644884b33849c27841a439bda6818213c3d745965339d2bc067`。哈希证明采集内容身份，不代表其中每个断言已验证。模型权重、缓存、依赖、第三方源码树和未指定的历史会话未采集；大型文本仅做哈希并按需要选读。

- 本次整理基于明确引用的 README、制作方法、配置、提示词、关键源码与已有报告。未重新观看全部参考/候选/成片，也未做人工听辨。
- 项目未提供的原始模型内部实现、未披露生成后端、未记录 seed/参数、真实费用、人工总工时和用户最终验收，均不补造。实际日志有记录的耗时见上文，流水线耗时不等于整个制作总耗时。
- 源项目内的路径只用于查证；本档案未复制大型素材或模型，也不改变源项目运行依赖。

| 已盘点证据文件 | SHA-256（前 16 位）/状态 |
| --- | --- |
| `videos/dental-clinic-city/README.md` | `8d3805f5c6fd5533` |
| `videos/dental-clinic-city/package-lock.json` | `5c3c97407b34642d` |
| `videos/dental-clinic-city/package.json` | `c4d47bd6a0704cd5` |
| `videos/dental-clinic-city/project.json` | `1c29fc7adf14d8a8` |
| `videos/dental-clinic-city/remotion.config.ts` | `fe968ee4188f28ba` |
| `videos/dental-clinic-city/tsconfig.json` | `2cfb14dc584e98ab` |
| `videos/dental-clinic-city/docs/production-notes.md` | `8abce345409ab5c1` |
| `videos/dental-clinic-city/outputs/clinic-check-report.json` | `9395284605529963` |
| `videos/dental-clinic-city/outputs/checks/archive-report.json` | `e166ca7f2f492eb7` |
| `videos/dental-clinic-city/outputs/checks/full-export-report.json` | `5fdceef82f2c76dc` |
| `videos/dental-clinic-city/outputs/checks/isolation-report.json` | `e7ef3ab2e61a8536` |
| `videos/dental-clinic-city/outputs/checks/media-verification.json` | `c458c85a3b9791c7` |
| `videos/dental-clinic-city/outputs/checks/migration-report.json` | `734caaaaca0845f6` |
| `videos/dental-clinic-city/public/business/clinic.json` | `f7782ab64506133a` |
| `videos/dental-clinic-city/public/fonts/misans-source.json` | `f675e829b466ffa8` |
| `videos/dental-clinic-city/scripts/check-clinic.mjs` | `0338fae8a9246657` |
| `videos/dental-clinic-city/scripts/check-isolation.mjs` | `f7d2e18736bae672` |
| `videos/dental-clinic-city/scripts/package-project.py` | `62fc9770e7659b40` |
| `videos/dental-clinic-city/scripts/remotion.mjs` | `a1a87d7bd574cc39` |
| `videos/dental-clinic-city/scripts/verify-media.mjs` | `3ba189cbc90be977` |
| `videos/dental-clinic-city/src/DentalClinicCity.tsx` | `5bcd5994d72978b9` |
| `videos/dental-clinic-city/src/Root.tsx` | `c69f4dba98ddf50e` |
| `videos/dental-clinic-city/src/fonts.ts` | `13ee436fdd06e832` |
| `videos/dental-clinic-city/src/index.ts` | `5462d3268136731c` |
| `videos/dental-clinic-city/src/clinic/CityIntro.tsx` | `da1d5f8d92c2f89d` |
| `videos/dental-clinic-city/src/clinic/CityLocation.tsx` | `9dbf69b71ecbd791` |
| `videos/dental-clinic-city/src/clinic/CityScene.tsx` | `9e95f3713a261a85` |
| `videos/dental-clinic-city/src/clinic/ClinicBenefits.tsx` | `94a87fa544a01cc7` |
| `videos/dental-clinic-city/src/clinic/ClinicContact.tsx` | `6aa388bed594aeb6` |
| `videos/dental-clinic-city/src/clinic/Overlay.tsx` | `62c29629ab37f295` |
| `videos/dental-clinic-city/src/clinic/config.ts` | `f4c138970c6e14b6` |
| `videos/dental-clinic-city/src/clinic/motion.ts` | `c229677d2db383a4` |
