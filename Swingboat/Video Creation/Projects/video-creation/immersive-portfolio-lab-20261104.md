---
type: video-production-dossier
project: video-creation
video_id: immersive-portfolio-lab-20261104
captured: '2026-11-12'
status: 24 秒 Studio 预览；未发现整片 MP4
source_fingerprint: d8f40f1fc80f5a945bc6cfb5d580580195456da341d93a2832a884faca962fe1
tags:
- video-creation
- production-capture
---

# Immersive Studio · 3D Portfolio 学习试作

<!-- capture-source-sha256:d8f40f1fc80f5a945bc6cfb5d580580195456da341d93a2832a884faca962fe1 -->

本次为指定项目的材料回溯与归档。制作结论来自项目记录/源码，未找回关闭的制作对话，也未重新执行生成、渲染、音频合成或用户验收。

## 当前结论与制作路线

**状态：24 秒 Studio 预览；未发现整片 MP4。**

参考帖与原站研究 → 本地 GLB → 帧驱动骨骼/GSAP → GLSL 全息与排版。

1920×1080、30fps、24 秒，四个场景各 6 秒：办公室、全息角色、作品、联系。借鉴约 17 秒录屏的空间叙事，延长信息展示时间。运行画面使用本地模型和贴图，并非直接拼接参考录屏。

## 关键算法、实现与适用边界

AnimationMixer 按绝对时间定位骨骼，GSAP timeline 暂停后按 Remotion 帧数 seek；避免独立 ticker 造成逐帧漂移。GLSL vertex/fragment shader、扫描环与粒子构成全息外观。原网站使用的 Vue、Lenis 不等于本视频采用：视频没有滚轮交互，实际依赖为 React/Remotion/R3F/Three.js/GSAP。

## 声音与字幕

scripts/synthesize-score.py 使用 Python 标准库合成原创电子配乐与转场声；没有旁白。

## 失败、修正与验收边界

原帖称“物理反馈”，本地技术复核未证实采用物理引擎。模型所用 DCC 未确认。原资产有自定义用途与署名条款，原作者 David Heckhoff 的作品、角色和模型不能记为本项目原创；本次不重新作法律判断，保留源许可记录。

## 技术与模型版本

以下是本项目声明或锁定的依赖，安装清单本身不证明某项参与了最终画面。具体用途以制作路线、源码和生成记录交叉判断。

| 依赖 | package.json 声明 | 可解析的直接锁定版本 |
| --- | --- | --- |
| `@react-three/fiber` | `9.8.2` | `9.8.2` |
| `@remotion/bundler` | `4.0.540` | `4.0.540` |
| `@remotion/cli` | `4.0.540` | `4.0.540` |
| `@remotion/media` | `4.0.540` | `4.0.540` |
| `@remotion/renderer` | `4.0.540` | `4.0.540` |
| `@remotion/three` | `4.0.540` | `4.0.540` |
| `gsap` | `3.13.0` | `3.13.0` |
| `react` | `19.2.0` | `19.2.0` |
| `react-dom` | `19.2.0` | `19.2.0` |
| `remotion` | `4.0.540` | `4.0.540` |
| `three` | `0.181.0` | `0.181.0` |
| `@types/react` | `19.2.2` | `19.2.2` |
| `@types/react-dom` | `19.2.2` | `19.2.2` |
| `@types/three` | `0.181.0` | `0.181.0` |
| `prettier` | `3.6.2` | `3.6.2` |
| `typescript` | `5.9.3` | `5.9.3` |

工具底层模型未公开时按“未披露”记录；不能由 imagegen 工具名猜测图像模型。未提供真实推理日志的配置不记作已运行。

## 镜头、时间轴与采用参数

> [!info]- 当前 Composition / 时间轴源码：videos/immersive-portfolio-lab-20261104/src/Root.tsx，L1–128
> ```tsx
> import {
>   AbsoluteFill,
>   Composition,
>   Folder,
>   Sequence,
>   interpolate,
>   useCurrentFrame,
>   useVideoConfig,
>   staticFile,
> } from "remotion";
> import { Audio } from "@remotion/media";
> import { Office } from "./Office";
> import { Hologram } from "./Hologram";
> import { Projects } from "./Projects";
> import { Contact } from "./Contact";
> import { clamp } from "./theme";
> const Wipe = ({ at, color }: { at: number; color: string }) => {
>   const f = useCurrentFrame();
>   const { fps } = useVideoConfig();
>   return (
>     <AbsoluteFill
>       style={{
>         background: color,
>         translate: interpolate(
>           f / fps - at,
>           [-0.3, 0, 0.3],
>           ["0px 1080px", "0px 0px", "0px -1080px"],
>           clamp,
>         ),
>         pointerEvents: "none",
>       }}
>     />
>   );
> };
> export const PortfolioFilm = () => {
>   const { fps } = useVideoConfig();
>   return (
>     <AbsoluteFill>
>       <Sequence
>         name="01 · Office / welcome"
>         from={0}
>         durationInFrames={6 * fps}
>         premountFor={fps}
>       >
>         <Office />
>       </Sequence>
>       <Sequence
>         name="02 · Hologram / toolkit"
>         from={6 * fps}
>         durationInFrames={6 * fps}
>         premountFor={fps}
>       >
>         <Hologram />
>       </Sequence>
>       <Sequence
>         name="03 · Selected projects"
>         from={12 * fps}
>         durationInFrames={6 * fps}
>         premountFor={fps}
>       >
>         <Projects />
>       </Sequence>
>       <Sequence
>         name="04 · Contact / finale"
>         from={18 * fps}
>         durationInFrames={6 * fps}
>         premountFor={fps}
>       >
>         <Contact />
>       </Sequence>
>       <Wipe at={6} color="#0c3457" />
>       <Wipe at={12} color="#e8dfce" />
>       <Wipe at={18} color="#e9dfcf" />
>       <Audio
>         name="Original electronic score"
>         src={staticFile("audio/studio-score.wav")}
>         premountFor={fps}
>         volume={0.8}
>       />
>     </AbsoluteFill>
>   );
> };
> export const Root = () => (
>   <>
>     <Composition
>       id="PortfolioFilm"
>       component={PortfolioFilm}
>       width={1920}
>       height={1080}
>       fps={30}
>       durationInFrames={720}
>     />
>     <Folder name="Scenes">
>       <Composition
>         id="Office"
>         component={Office}
>         width={1920}
>         height={1080}
>         fps={30}
>         durationInFrames={180}
>       />
>       <Composition
>         id="Hologram"
>         component={Hologram}
>         width={1920}
>         height={1080}
>         fps={30}
>         durationInFrames={180}
>       />
>       <Composition
>         id="Projects"
>         component={Projects}
>         width={1920}
>         height={1080}
>         fps={30}
>         durationInFrames={180}
>       />
>       <Composition
>         id="Contact"
>         component={Contact}
>         width={1920}
>         height={1080}
>         fps={30}
>         durationInFrames={180}
>       />
>     </Folder>
>   </>
> );
> ```

源时间轴、生成帧数、所用区间与最终时长分别记录；历史文档或候选参数不自动覆盖当前采用配置。

## 参考来源与借鉴说明

- 分享帖：[Berryxia.AI / @berryxia](https://x.com/berryxia/status/2055784649830670829)。本地快照记录发布时间 `Sat May 16 22:57:09 +0000 2026`；本次未在线重新验证该时间。
- 原创作者：David Heckhoff；引用链为 Berryxia 分享帖 → [原站](https://david-hckh.com) → [原仓库](https://github.com/davidhckh/portfolio-2025)。分享者与作品作者分开署名。
- 本地快照：`docs/research/post.json`。快照视频规格 17.085 秒、1280×720；[当时记录的媒体地址](https://video.twimg.com/amplify_video/2055784433287061504/vid/avc1/1280x720/o9aZSbAss1m5b07B.mp4?tag=27)，本次未重新下载或验证媒体。
- 本次联网核对：2026-11-12 X 原帖返回 HTTP 403。原站、仓库和本地媒体的历史研究结论见项目 docs，本次未重新进行外站全面审阅。
- 原参考约 0–3 秒办公室、3–7 秒全息、7–10 秒作品、10–17 秒联系。本片对应四段各 6 秒，增加大字与阅读停留；参考录屏留作研究，实际画面用本地 GLB 重新合成。
- 原作者角色、办公室、联系模型、贴图及项目缩略图的来源和许可按 `docs/research/original-license.md` 保存；技术表区分原网站与本片实际使用。

### 项目记录的技术、内容或素材来源

这些 URL 来自项目文件，尚不自动证明本片采用；除本节明确标注的访问外，本轮未逐一在线重读。

| 链接 | 记录文件 |
| --- | --- |
| [david-hckh.com](https://david-hckh.com) | `videos/immersive-portfolio-lab-20261104/README.md` |
| [github.com](https://github.com/davidhckh/portfolio-2025) | `videos/immersive-portfolio-lab-20261104/README.md` |
| [threejs.org](https://threejs.org/docs/) | `videos/immersive-portfolio-lab-20261104/docs/review-and-tech.md` |
| [gsap.com](https://gsap.com/docs/v3/) | `videos/immersive-portfolio-lab-20261104/docs/review-and-tech.md` |
| [github.com](https://github.com/darkroomengineering/lenis) | `videos/immersive-portfolio-lab-20261104/docs/review-and-tech.md` |
| [vuejs.org](https://vuejs.org/guide/introduction.html) | `videos/immersive-portfolio-lab-20261104/docs/review-and-tech.md` |
| [www.typescriptlang.org](https://www.typescriptlang.org/docs/handbook/intro.html) | `videos/immersive-portfolio-lab-20261104/docs/review-and-tech.md` |
| [vite.dev](https://vite.dev/guide/) | `videos/immersive-portfolio-lab-20261104/docs/review-and-tech.md` |
| [howlerjs.com](https://howlerjs.com/) | `videos/immersive-portfolio-lab-20261104/docs/review-and-tech.md` |
| [threejs.org](https://threejs.org/docs/pages/AnimationMixer.html) | `videos/immersive-portfolio-lab-20261104/docs/review-and-tech.md` |

## 重现入口与修改位置

以下是 package.json 中的实际命令映射，供在对应视频目录使用；本轮未执行这些制作命令。系统依赖、前置模型/素材和环境准备见后附 README。

| npm 入口 | 实际命令 |
| --- | --- |
| `npm run studio` | `node scripts/run.mjs studio` |
| `npm run check` | `tsc --noEmit && node scripts/audit.mjs` |
| `npm run still` | `node scripts/run.mjs still` |
| `npm run render` | `node scripts/run.mjs render` |
| `npm run bundle` | `node scripts/run.mjs bundle` |
| `npm run check:frames` | `node scripts/check-frames.mjs` |

### 算法源码摘录

> [!info]- videos/immersive-portfolio-lab-20261104/src/World.tsx，L1–230
> ```tsx
> /** Models/textures: David Heckhoff, https://david-hckh.com
>  * Source: https://github.com/davidhckh/portfolio-2025
>  * Personal/educational use only; license in docs/research/original-license.md.
>  * Frame-driven scenes and hologram shader authored for this study. */
> import { useMemo, useLayoutEffect, useEffect } from "react";
> import { useLoader, useThree } from "@react-three/fiber";
> import { ThreeCanvas } from "@remotion/three";
> import { staticFile, useCurrentFrame, useVideoConfig } from "remotion";
> import * as T from "three";
> import { GLTFLoader } from "three/examples/jsm/loaders/GLTFLoader.js";
> import { clone } from "three/examples/jsm/utils/SkeletonUtils.js";
> import { gsap } from "gsap";
> type Mode = "office" | "hologram" | "contact";
> const texturePaths = [
>   "room.webp",
>   "contact.webp",
>   "head.webp",
>   "face-spritesheet.png",
>   "matcap-black.webp",
>   "matcap-gray.webp",
>   "matcap-skin.webp",
>   "matcap-white.webp",
>   "desktops.webp",
>   "room-shadow.webp",
>   "contact-shadow.webp",
> ];
> const vertex = `varying vec2 vUv; varying vec3 vNormal; varying vec3 vWorld;
> #include <common>
> #include <skinning_pars_vertex>
> void main(){vUv=uv;
> #include <beginnormal_vertex>
> #include <skinbase_vertex>
> #include <skinnormal_vertex>
> #include <defaultnormal_vertex>
> vNormal=normalize(transformedNormal);
> #include <begin_vertex>
> #include <skinning_vertex>
> vec4 world=modelMatrix*vec4(transformed,1.);vWorld=world.xyz;gl_Position=projectionMatrix*viewMatrix*world;}`;
> const shadowVertex = `varying vec2 vUv;void main(){vUv=uv;gl_Position=projectionMatrix*modelViewMatrix*vec4(position,1.);}`;
> const shadowFragment = `uniform sampler2D uTexture;varying vec2 vUv;void main(){vec4 s=texture2D(uTexture,vUv);gl_FragColor=vec4(.42,.32,.20,(1.-s.r)*s.a*.28);}`;
> const fragment = `uniform float uTime;uniform float uAmount;varying vec2 vUv;varying vec3 vNormal;varying vec3 vWorld;
> void main(){float scan=pow(.5+.5*sin(vWorld.y*105.-uTime*8.),5.);float band=exp(-pow((vWorld.y-mod(uTime*.9,4.6))*7.,2.));float rim=pow(1.-abs(normalize(vNormal).z),2.2);float alpha=(.13+rim*.63+scan*.12+band*.3)*uAmount;vec3 cyan=mix(vec3(.015,.38,.7),vec3(.25,.95,1.),rim+band*.45);gl_FragColor=vec4(cyan,alpha);}`;
> function Scene({ mode }: { mode: Mode }) {
>   const f = useCurrentFrame();
>   const { fps } = useVideoConfig();
>   const sec = f / fps;
>   const { camera } = useThree();
>   const [room, avatar, contact] = useLoader(
>     GLTFLoader,
>     ["room.glb", "avatar.glb", "contact.glb"].map((x) =>
>       staticFile("upstream/models/" + x),
>     ),
>   );
>   const textures = useLoader(
>     T.TextureLoader,
>     texturePaths.map((x) => staticFile("upstream/textures/" + x)),
>   );
>   const data = useMemo(() => {
>     const tx = textures.map((x, i) => {
>       const t = x.clone();
>       t.flipY = i === 3;
>       t.colorSpace = T.SRGBColorSpace;
>       t.needsUpdate = true;
>       return t;
>     });
>     tx[3].repeat.set(0.25, 0.25);
>     const roomClone = room.scene.clone(true),
>       contactClone = contact.scene.clone(true),
>       actor = clone(avatar.scene.children[0]);
>     actor.rotation.z = 0;
>     const roomMat = new T.MeshBasicMaterial({ map: tx[0] }),
>       screenMat = new T.MeshBasicMaterial({ map: tx[8] });
>     roomClone.traverse((o) => {
>       if (o instanceof T.Mesh) {
>         o.material = o.name.startsWith("desktop-plane") ? screenMat : roomMat;
>         if (o.name === "shadow-catcher") {
>           o.material = new T.ShaderMaterial({
>             vertexShader: shadowVertex,
>             fragmentShader: shadowFragment,
>             uniforms: { uTexture: { value: tx[9] } },
>             transparent: true,
>             depthWrite: false,
>           });
>           o.renderOrder = -10;
>         }
>         o.frustumCulled = false;
>       }
>     });
>     contactClone.traverse((o) => {
>       if (o instanceof T.Mesh) {
>         o.material = new T.MeshBasicMaterial({ map: tx[1] });
>         if (o.name === "shadow-catcher") {
>           o.material = new T.ShaderMaterial({
>             vertexShader: shadowVertex,
>             fragmentShader: shadowFragment,
>             uniforms: { uTexture: { value: tx[10] } },
>             transparent: true,
>             depthWrite: false,
>           });
>           o.renderOrder = -10;
>         }
>       }
>     });
>     const faceMat = new T.MeshBasicMaterial({
>       map: tx[3],
>       transparent: true,
>       depthWrite: false,
>       polygonOffset: true,
>       polygonOffsetFactor: -2,
>     });
>     const holo = new T.ShaderMaterial({
>       vertexShader: vertex,
>       fragmentShader: fragment,
>       uniforms: { uTime: { value: 0 }, uAmount: { value: 1 } },
>       transparent: true,
>       depthWrite: false,
>       side: T.DoubleSide,
>       blending: T.AdditiveBlending,
>     });
>     actor.traverse((o) => {
>       if (o instanceof T.Mesh) {
>         o.frustumCulled = false;
>         if (mode === "hologram") {
>           o.material = o.name === "face" ? faceMat : holo;
>           o.renderOrder = o.name === "face" ? 10 : 0;
>         } else if (o.name === "head")
>           o.material = new T.MeshBasicMaterial({ map: tx[2] });
>         else if (o.name === "face") {
>           o.material = faceMat;
>           o.renderOrder = 10;
>         } else {
>           const idx =
>             (
>               { black: 4, gray: 5, skin: 6, white: 7 } as Record<string, number>
>             )[o.name] ?? 4;
>           o.material = new T.MeshMatcapMaterial({ matcap: tx[idx] });
>         }
>       }
>     });
>     const mixer = new T.AnimationMixer(actor);
>     const actions = Object.fromEntries(
>       avatar.animations.map((c) => {
>         const a = mixer.clipAction(c);
>         a.play();
>         a.setEffectiveWeight(0);
>         return [c.name, a];
>       }),
>     );
>     const motion = { x: 0, y: 0, z: 0, turn: 0 };
>     const timeline = gsap.timeline({ paused: true });
>     if (mode === "office")
>       timeline.fromTo(
>         motion,
>         { x: 0.25, turn: -0.08 },
>         { x: 0, turn: 0.08, duration: 6, ease: "sine.inOut" },
>       );
>     if (mode === "hologram")
>       timeline.fromTo(
>         motion,
>         { turn: -0.28, y: -0.18 },
>         { turn: 0.3, y: 0, duration: 6, ease: "sine.inOut" },
>       );
>     if (mode === "contact")
>       timeline.fromTo(
>         motion,
>         { x: 0.1, turn: -0.08 },
>         { x: 0, turn: 0.04, duration: 6, ease: "sine.inOut" },
>       );
>     return {
>       tx,
>       roomClone,
>       contactClone,
>       actor,
>       mixer,
>       actions,
>       holo,
>       motion,
>       timeline,
>     };
>   }, [room, avatar, contact, textures, mode]);
>   // Seek before JSX transforms are read, including direct out-of-order frame jumps.
>   data.timeline.seek(sec, true);
>   useEffect(
>     () => () => {
>       data.timeline.kill();
>     },
>     [data],
>   );
>   useLayoutEffect(() => {
>     for (const a of Object.values(data.actions)) {
>       a.enabled = true;
>       a.setEffectiveWeight(0);
>       a.time = 0;
>     }
>     let pose = "idle",
>       time = sec;
>     if (mode === "office") {
>       pose = sec < 2.7 ? "wave" : "idle";
>       time = sec < 2.7 ? Math.min(sec, 2.65) : sec - 2.7;
>     }
>     if (mode === "hologram") pose = "t-idle";
>     if (mode === "contact") {
>       pose = sec < 0.75 ? "sleeping" : sec < 1.58 ? "wake-up" : "contact-idle";
>       time = sec < 0.75 ? sec : sec < 1.58 ? sec - 0.75 : sec - 1.58;
>     }
>     const action = data.actions[pose];
>     action.setEffectiveWeight(1);
>     action.time = time % action.getClip().duration;
>     if (mode === "office") {
>       const w = 1 - Math.max(0, Math.min(1, (sec - 2.3) / 0.4));
>       data.actions.wave.setEffectiveWeight(w);
>       data.actions.wave.time = Math.min(sec, 2.7);
>       data.actions.idle.setEffectiveWeight(1 - w);
>       data.actions.idle.time =
>         Math.max(0, sec - 2.3) % data.actions.idle.getClip().duration;
>     }
>     data.mixer.update(0);
>     data.tx[3].offset.set(0, mode === "contact" ? (sec < 0.8 ? 0.5 : 0) : 0.75);
>     if (Math.abs(sec - 4.15) < 0.08) data.tx[3].offset.x = 0.75;
>     data.holo.uniforms.uTime.value = sec;
>     data.holo.uniforms.uAmount.value = Math.min(1, sec * 1.8);
>     if (mode === "office") {
>       camera.position.set(data.motion.x, 6, 11.3 - sec * 0.1);
>       camera.lookAt(0, 2.9, 0);
>     } else if (mode === "hologram") {
>       camera.position.set(0.3 * Math.sin(sec * 0.25), 3.4, 9.7);
>       camera.lookAt(0, 1.9, 0);
>     } else {
>       camera.position.set(0.1, 4.35, 10.6 - sec * 0.06);
>       camera.lookAt(0, 2.1, 0);
> ```
> 
> 本摘录到第 230 行；完整文件共 331 行，余下实现请按源文件继续阅读。

> [!info]- videos/immersive-portfolio-lab-20261104/scripts/synthesize-score.py，L1–30
> ```python
> """Original deterministic 24-second electronic cue; no third-party audio."""
> from pathlib import Path
> import math,wave,array,random
> SR=48000;DURATION=24;N=SR*DURATION
> left=array.array('f',[0])*N;right=array.array('f',[0])*N
> rng=random.Random(273)
> def add(start,dur,fn,pan=0):
>  begin=int(start*SR);end=min(N,begin+int(dur*SR))
>  for i in range(begin,end):
>   t=(i-begin)/SR;v=fn(t);left[i]+=v*(1-pan*.3);right[i]+=v*(1+pan*.3)
> chords=[(146.832,220,261.626,329.628),(130.813,195.998,261.626,329.628),(164.814,220,293.665,391.995),(130.813,195.998,261.626,349.228)]
> for j,chord in enumerate(chords):
>  for k,f in enumerate(chord):
>   add(j*6,6,lambda t,f=f:.026*min(1,t/.4)*min(1,(6-t)/.7)*(math.sin(2*math.pi*f*t)+.22*math.sin(2*math.pi*f*1.002*t)),(k-1.5)/2)
> for b in range(48):
>  start=b*.5
>  add(start,.22,lambda t:.17*math.exp(-t*22)*math.sin(2*math.pi*(48*t+3*(1-math.exp(-t*25)))))
>  if b%2==1:add(start,.1,lambda t:.014*math.exp(-t*44)*(math.sin(t*7921)+math.sin(t*12187)),.3)
>  chord=chords[min(3,int(start/6))];f=chord[b%4]*2
>  add(start+.25,.38,lambda t,f=f:.048*min(1,t/.005)*math.exp(-t*10)*math.sin(2*math.pi*f*t),(-1)**b*.6)
> for j in (6,12,18):
>  add(j-.28,.65,lambda t:.027*math.sin(2*math.pi*(300*t+1100*t*t))*math.sin(math.pi*t/.65)**2,-.4)
>  add(j,.6,lambda t:.05*math.exp(-t*7)*math.sin(2*math.pi*880*t),.4)
> peak=max(max(abs(x) for x in left),max(abs(x) for x in right))
> raw=array.array('h')
> for i,(l,r) in enumerate(zip(left,right)):
>  fade=min(1,i/(SR*.12),(N-i)/(SR*1.1));raw.append(int(l/peak*.62*fade*32767));raw.append(int(r/peak*.62*fade*32767))
> Path('public/audio').mkdir(parents=True,exist_ok=True)
> with wave.open('public/audio/studio-score.wav','wb') as w:w.setnchannels(2);w.setsampwidth(2);w.setframerate(SR);w.writeframes(raw.tobytes())
> print('24s stereo, 48kHz, normalized peak -4.15 dBFS')
> ```

## 完整关键提示词与生成记录

以下保留项目自有提示词、候选参数与版本。存在失败候选和历史参数；最终采用以 selected-takes、edit-decisions 与制作复盘为准，不能将所有候选都写成已采用。

未发现实际生成提示词或推理执行记录；不从脚手架、模型安装清单推测生成过程。

## 已有产物与检查依据

文件存在、技术检查、画面检查与用户验收分别看待。下表是本次文件盘点，旧版与当前版可能共存。MP4/HTML 按内容计算 SHA-256，ZIP 仅记录文件大小和修改时间供增量判断。

| 本地产物 | 字节数 | SHA-256 |
| --- | ---: | --- |
| 本次未发现 outputs 根目录的 MP4/HTML/ZIP | — | — |

已有检查报告路径（未在本轮重新执行）：

- `videos/immersive-portfolio-lab-20261104/outputs/checks/frames.json`
- `videos/immersive-portfolio-lab-20261104/outputs/checks/isolation.json`

## 制作方法与迭代记录选编

下面是项目自有文档的归档摘录，保留原阶段与原检查表述，个人路径已泛化、相对链接已注明源项目位置。它们属于来源陈述，不代表本次重新完成其中的生成或验收。

> [!info]- 来源文档：videos/immersive-portfolio-lab-20261104/README.md
> # Immersive Studio · 3D Portfolio Study
> 
> 24 秒、1920×1080、30fps 的 Remotion 学习试作。参考 Berryxia 分享的 [David Heckhoff portfolio](https://david-hckh.com)，依次展示 3D 办公室、全息角色、作者作品和联系场景。
> 
> 技术收集与 Review（项目文件：`videos/immersive-portfolio-lab-20261104/docs/review-and-tech.md`） · 分镜（项目文件：`videos/immersive-portfolio-lab-20261104/docs/storyboard.md`） · 原许可（项目文件：`videos/immersive-portfolio-lab-20261104/docs/research/original-license.md`）
> 
> ## 独立安装
> 
> 在本目录执行，需 Node.js 22+、npm；静帧检查/导出默认使用系统安装的 Google Chrome。
> 
> ```bash
> mkdir -p .cache/tmp .cache/npm
> TMPDIR="$PWD/.cache/tmp" TMP="$PWD/.cache/tmp" TEMP="$PWD/.cache/tmp" npm_config_cache="$PWD/.cache/npm" npm ci
> ```
> 
> `.npmrc` 已将 npm 缓存设置在本目录；所有直接依赖固定版本，lockfile 完整。`legacy-peer-deps` 用于跳过 R3F 可选的 React Native/Expo peer 解析；本项目只在浏览器运行。
> 
> ## 预览
> 
> ```bash
> npm run studio
> ```
> 
> 打开终端给出的 URL，默认 [PortfolioFilm](http://localhost:3124/PortfolioFilm)。总片及四个场景可分别选择和拖动时间轴；服务保持运行时，修改源码会热更新。
> 
> ## 检查与导出
> 
> ```bash
> npm run check
> npm run check:frames
> npm run still -- --frame=60 --image-format=png
> npm run render
> ```
> 
> - `check`：TypeScript 与独立性检查。
> - `check:frames`：14 个关键帧，覆盖四个镜头、三个转场及首尾；写入 `outputs/checks/`，不生成 MP4。
> - `still`：单帧 `outputs/checks/frame.png`。
> - `render`：完整 H.264 MP4，写入 `outputs/immersive-portfolio.mp4`。本次默认交付 Studio 预览，未预先运行整片导出。
> - `bundle`：独立网页 bundle，写入 `outputs/bundle/`，之后由 HTTP 服务托管（不能用 file:// 直接打开）。
> 
> `run.mjs` 为 Remotion 子进程设置 TMPDIR/TMP/TEMP；检查脚本将 webpack bundle、临时文件放到 `.cache/`。`remotion.config.ts` 及 `scripts/check-frames.mjs` 使用系统 Chrome；如果你的 Chrome 路径不同，需要同时修改这两处。没有下载渲染浏览器到其他目录。Codex 内置预览浏览器的应用级缓存没有可配置的项目路径，不将其宣称为已隔离。
> 
> ## 修改素材、配音和时间轴
> 
> - 镜头顺序和时长：`src/Root.tsx` 中四个独立 `<Sequence>`，目前各 6 秒；改时长后同步修改主合成 `durationInFrames`、转场 `Wipe at`、分镜及配乐时长。
> - 办公室和角色动画：`src/World.tsx`。Three.js 的 AnimationMixer 以绝对时间定位；GSAP timeline 暂停并按当前帧 seek。
> - 全息效果：`World.tsx` 顶部 vertex/fragment GLSL、扫描环和粒子。
> - 标题和位置：`Office.tsx`、`Hologram.tsx`、`Projects.tsx`、`Contact.tsx`；颜色和页眉页脚：`theme.tsx`。
> - 模型与贴图：`public/upstream/` 本地副本。替换 GLB 后需核对 mesh 名称、UV、朝向和动画片段名。不能直接将不同骨架的 GLB 替换后假设原动作可用。
> - 音乐：`public/audio/studio-score.wav`。`python3 scripts/synthesize-score.py` 可重建原创合成配乐，不依赖 Python 外部库。
> - 配音：当前无旁白。可把自己的音频放到 `public/audio/`，在 `Root.tsx` 添加独立 `<Audio>`，以 `from` 和 `trimBefore` 调整时点；若新增字幕，应单独编辑文字和时间轴。
> - 字体：`public/fonts/`，Urbanist 使用 OFL；`theme.tsx` 声明本地字体，不在播放时请求 Google Fonts。
> 
> ## Credits & Attribution
> 
> **Original portfolio, reused character/office/contact models, related textures and project thumbnails were created and designed by David Heckhoff.**
> 
> Original portfolio: https://david-hckh.com
> 
> Original repository: https://github.com/davidhckh/portfolio-2025
> 
> 本试作保留片内可见署名、源码注释和原许可。原资产限个人与教育用途，商用、转售或大部分内容再分发需作者书面许可。CubeWar、Quibbo、Sharkie、Particles 为原作者作品，本试作未将它们标记为用户自己的作品。参考帖视频仅保存于研究目录，不进入试作画面。
> 
> 本项目新增的 Remotion 时间轴、排版、GLSL 全息材质与电子配乐用于本次学习试作。所有运行资源均在当前视频目录内，不引用其他视频或仓库根目录的文件。

> [!info]- 来源文档：videos/immersive-portfolio-lab-20261104/docs/review-and-tech.md
> # 参考帖 Review 与技术收集
> 
> 参考：[Berryxia 帖子](https://x.com/berryxia/status/2055784649830670829) · [David Heckhoff 网站](https://david-hckh.com) · [源码](https://github.com/davidhckh/portfolio-2025)。核对日期：2026-11-04。
> 
> ## Review 结论
> 
> 这是交互式 portfolio 的约 17 秒操作录屏。它的吸引力来自有表演的角色、统一的 3D 美术和空间连续性，技术栈只是实现手段。参考素材为 1280×720，有音轨；本地 `research/reference.mp4` 仅供研究，未拼接进试作。
> 
> - 0–3 秒：奶油色办公室，角色在桌前挥手、转向屏幕。双显示器、植物、软阴影让场景有生活感。
> - 约 3–7 秒：切入蓝色实验室，角色成为青色全息投影；身份与技能面板环绕人物。
> - 约 7–10 秒：明亮项目列表，CubeWar、Quibbo、Sharkie、Particles 等卡片。
> - 约 10–17 秒：联系场景、快递纸箱，角色由打盹转为站立抱臂，形成小型叙事收尾。
> 
> 可改进的地方：原演示滚动很快，项目内容不易读；导航与正文较小，手机观看需要放大。因此本试作延长到 24 秒、1920×1080，保留大字标题并让项目页停留更久。
> 
> ## 已核实的技术及职责
> 
> | 技术                             | 原项目声明版本 | 作用                                                 | 证据/学习入口                                                             |
> | -------------------------------- | -------------- | ---------------------------------------------------- | ------------------------------------------------------------------------- |
> | Three.js                         | ^0.181.0       | WebGL 场景、模型、相机与材质                         | [官方文档](https://threejs.org/docs/)；`src/three/`                       |
> | GSAP / GreenSock                 | ^3.13.0        | 角色权重、相机路径、场景切换、交互反馈的时间编排     | [官方文档](https://gsap.com/docs/v3/)；`src/animations/`                  |
> | Lenis                            | ^1.3.11        | 平滑滚动，并使 DOM 与 WebGL 由共同进度驱动           | [官方仓库](https://github.com/darkroomengineering/lenis)                  |
> | Vue 3                            | ^3.5.22        | 导航、文本、项目列表和页面 UI                        | [官方指南](https://vuejs.org/guide/introduction.html)                     |
> | TypeScript                       | ~5.9.3         | 类型与模块结构                                       | [官方手册](https://www.typescriptlang.org/docs/handbook/intro.html)       |
> | Vite                             | ^7.1.7         | 开发服务和打包                                       | [官方指南](https://vite.dev/guide/)                                       |
> | GLSL + vite-plugin-glsl          | ^1.5.4（插件） | 全息扫描线、透明度、角色渐变、网格和电子光效         | 仓库 `src/three/shaders/`                                                 |
> | Howler                           | ^2.2.4         | 鼠标、键盘、通知、角色等音效与 audio sprite          | [官网](https://howlerjs.com/)；仓库 `sounds/`                             |
> | GLB + AnimationMixer             | Three.js API   | 模型加载、角色骨骼动画与动画混合                     | [官方 AnimationMixer](https://threejs.org/docs/pages/AnimationMixer.html) |
> | 烘焙贴图 + Matcap + Sprite atlas | 美术/渲染方法  | 办公室明暗预烘焙；身体使用 Matcap；表情使用 4×4 图集 | 仓库 `room.webp`、`matcap-*`、`face-spritesheet.png`                      |
> 
> 版本来自抓取时的 `package.json`，不是建议盲目升级到这些版本。帖子中的“物理反馈”是视觉描述；依赖清单及本次阅读到的实现未发现 Rapier/Cannon 等物理引擎，不将其写成已证实的物理模拟。模型究竟用哪个 DCC 软件制作也未在本次核对中证实。
> 
> ## 本试作实际采用
> 
> - Remotion 4.0.540 + React 19：固定帧率、可拖动时间轴、镜头独立预览和导出入口。
> - Three.js 0.181.0 + React Three Fiber + ThreeCanvas：真实 3D，不是从帖子截取画面拼接。
> - 角色与办公室、纸箱模型，以及相关贴图，使用作者允许的个人学习用途本地副本；各画面保留 David Heckhoff 署名。项目卡片展示的仍为作者作品。
> - 角色用骨骼动画，以当前帧计算绝对动画时间；全息材质、扫描环、镜头运动及排版为本项目实现。
> - GSAP 用暂停 timeline 的绝对 seek 编排相机/角色变换，以 Remotion 帧数为时钟，不用独立实时 ticker。
> - 视频没有滚轮输入，故不安装 Lenis。没有把 Vue 网站整个搬进 React。
> - 音乐为本项目代码合成的轻量电子配乐和转场音效，无旁白；可替换为本目录内的自有音频。
> 
> ## 许可与署名
> 
> 该公开仓库使用自定义许可，**不是 MIT**：允许个人、教育用途，要求保留作者署名与原站链接；商业用途、转售或大部分内容再分发需要作者许可。原许可快照见 `research/original-license.md`。本项目定位个人学习试作；不要把借用的角色、办公室和项目缩略图作为自有作品售卖。
> 
> David Heckhoff created and designed the original portfolio and the reused 3D assets.
> Original portfolio: https://david-hckh.com
> Original source: https://github.com/davidhckh/portfolio-2025

> [!info]- 来源文档：videos/immersive-portfolio-lab-20261104/docs/storyboard.md
> # 24 秒试作分镜
> 
> | 时间   | 画面                                                            | 动作                                           | 声音                   |
> | ------ | --------------------------------------------------------------- | ---------------------------------------------- | ---------------------- |
> | 0–6s   | 奶油色 3D 办公室，左侧大标题「A little world. A lot of ideas.」 | 相机轻推，角色挥手，进入工作状态               | 轻量电子节奏、按键点音 |
> | 6–12s  | 深蓝全息实验室                                                  | 环形投影台亮起，角色轮廓扫描，技能文字依次出现 | 扫描上升音、柔和高频   |
> | 12–18s | 四个项目缩略图                                                  | 项目卡片错峰滑入，短暂悬浮；保留作者项目名称   | 节拍连续、轻微点音     |
> | 18–24s | 明亮纸箱场景，左侧「Let’s make something move.」                | 角色醒来站立，最终抱臂定格                     | 收束和弦、短提示音     |
> 
> 1920×1080，30fps，共720帧。无旁白，英文画面呼应参考站；中文研究报告单独保存。

## 证据清单与缺失信息

盘点的文本/报告文件 33 个，源文本、报告及媒体产物身份摘要为 `d8f40f1fc80f5a945bc6cfb5d580580195456da341d93a2832a884faca962fe1`。哈希证明采集内容身份，不代表其中每个断言已验证。模型权重、缓存、依赖、第三方源码树和未指定的历史会话未采集；大型文本仅做哈希并按需要选读。

- 本次整理基于明确引用的 README、制作方法、配置、提示词、关键源码与已有报告。未重新观看全部参考/候选/成片，也未做人工听辨。
- 项目未提供的原始模型内部实现、未披露生成后端、未记录 seed/参数、真实费用、人工总工时和用户最终验收，均不补造。实际日志有记录的耗时见上文，流水线耗时不等于整个制作总耗时。
- 源项目内的路径只用于查证；本档案未复制大型素材或模型，也不改变源项目运行依赖。

| 已盘点证据文件 | SHA-256（前 16 位）/状态 |
| --- | --- |
| `videos/immersive-portfolio-lab-20261104/README.md` | `e01a86f4ed3793d2` |
| `videos/immersive-portfolio-lab-20261104/package-lock.json` | `5f99ed72952f99de` |
| `videos/immersive-portfolio-lab-20261104/package.json` | `a9702a8351e560da` |
| `videos/immersive-portfolio-lab-20261104/remotion.config.ts` | `e530a7e780e3116c` |
| `videos/immersive-portfolio-lab-20261104/tsconfig.json` | `4c6e7c845dd994c8` |
| `videos/immersive-portfolio-lab-20261104/docs/review-and-tech.md` | `6d53acef07b84389` |
| `videos/immersive-portfolio-lab-20261104/docs/storyboard.md` | `82ea5649ab5e58de` |
| `videos/immersive-portfolio-lab-20261104/docs/research/about-transition.ts` | `2fb80b857a18f482` |
| `videos/immersive-portfolio-lab-20261104/docs/research/asset-manifest.json` | `2a2eacb950ae349f` |
| `videos/immersive-portfolio-lab-20261104/docs/research/avatar-metadata.json` | `add4f64026726d2d` |
| `videos/immersive-portfolio-lab-20261104/docs/research/contact-metadata.json` | `b98576a948939340` |
| `videos/immersive-portfolio-lab-20261104/docs/research/lab-metadata.json` | `0c46f2a5eef02360` |
| `videos/immersive-portfolio-lab-20261104/docs/research/original-license.md` | `ad37947735af6fc0` |
| `videos/immersive-portfolio-lab-20261104/docs/research/original-package.json` | `874c414f54ed3783` |
| `videos/immersive-portfolio-lab-20261104/docs/research/post.json` | `33de8f543bb58d05` |
| `videos/immersive-portfolio-lab-20261104/docs/research/repository-files.txt` | `41b5ea0018a18d4a` |
| `videos/immersive-portfolio-lab-20261104/docs/research/room-metadata.json` | `f1f3ee0107dd9fd8` |
| `videos/immersive-portfolio-lab-20261104/outputs/checks/frames.json` | `c27300a21cf93827` |
| `videos/immersive-portfolio-lab-20261104/outputs/checks/isolation.json` | `d1eabe5be2186276` |
| `videos/immersive-portfolio-lab-20261104/public/fonts/OFL.txt` | `ee1221b1c2d08920` |
| `videos/immersive-portfolio-lab-20261104/scripts/audit.mjs` | `c0ead4e47c0b4d86` |
| `videos/immersive-portfolio-lab-20261104/scripts/check-frames.mjs` | `b37257a23f1efdff` |
| `videos/immersive-portfolio-lab-20261104/scripts/fetch-assets.py` | `a0733fe5fd35911a` |
| `videos/immersive-portfolio-lab-20261104/scripts/run.mjs` | `2196aa64fc80a47c` |
| `videos/immersive-portfolio-lab-20261104/scripts/synthesize-score.py` | `5380348596ebe24a` |
| `videos/immersive-portfolio-lab-20261104/src/Contact.tsx` | `a683c3488a7f687f` |
| `videos/immersive-portfolio-lab-20261104/src/Hologram.tsx` | `efead672a5d4f225` |
| `videos/immersive-portfolio-lab-20261104/src/Office.tsx` | `e13de45ece7f5ad5` |
| `videos/immersive-portfolio-lab-20261104/src/Projects.tsx` | `d7cb27440cdb21b8` |
| `videos/immersive-portfolio-lab-20261104/src/Root.tsx` | `50a43d5f9d10c48c` |
| `videos/immersive-portfolio-lab-20261104/src/World.tsx` | `a1049c232af84bce` |
| `videos/immersive-portfolio-lab-20261104/src/index.ts` | `b9900e8f2d9dd050` |
| `videos/immersive-portfolio-lab-20261104/src/theme.tsx` | `31dad222f94e0be1` |
