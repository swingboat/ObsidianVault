---
type: video-production-dossier
project: video-creation
video_id: anime-scene-forge-portable-smoke-20261111
captured: '2026-11-12'
status: 未制作；30 秒规划脚手架
source_fingerprint: d16d1a0820a3b10e92f9474d5756e143c1036e9ac726d837a9d77ddc4993efee
tags:
- video-creation
- production-capture
---

# Anime Scene Forge · 便携工程脚手架测试

<!-- capture-source-sha256:d16d1a0820a3b10e92f9474d5756e143c1036e9ac726d837a9d77ddc4993efee -->

本次为指定项目的材料回溯与归档。制作结论来自项目记录/源码，未找回关闭的制作对话，也未重新执行生成、渲染、音频合成或用户验收。

## 当前结论与制作路线

**状态：未制作；30 秒规划脚手架。**

production.json 配置 → 帧时间轴与素材占位 → 技术检查入口。

1280×720、24fps、720 帧的默认规划，八个 90 帧镜头均 pending，art/clip/dialogue 为空。没有实际叙事或已生成成片。external 是后端选项，不是具体模型。

## 关键算法、实现与适用边界

Root.tsx 按 production.json 构造 Series/Sequence；project_ops.py 管理选片、驱动音频、sha256 和审核状态，缺素材显示 Source footage pending。正式渲染与草稿入口分开。

## 声音与字幕

未提供真实对白、背景声或音效。

## 失败、修正与验收边界

不能把默认等长分段当成真实分镜，不能把安装/检查入口当成生成执行记录。

## 技术与模型版本

以下是本项目声明或锁定的依赖，安装清单本身不证明某项参与了最终画面。具体用途以制作路线、源码和生成记录交叉判断。

| 依赖 | package.json 声明 | 可解析的直接锁定版本 |
| --- | --- | --- |
| `@remotion/cli` | `4.0.541` | `4.0.541` |
| `@remotion/fonts` | `4.0.541` | `4.0.541` |
| `@remotion/media` | `4.0.541` | `4.0.541` |
| `react` | `19.3.1` | `19.3.1` |
| `react-dom` | `19.3.1` | `19.3.1` |
| `remotion` | `4.0.541` | `4.0.541` |
| `@types/react` | `19.3.0` | `19.3.0` |
| `@types/react-dom` | `19.2.3` | `19.2.3` |
| `typescript` | `5.9.3` | `5.9.3` |

工具底层模型未公开时按“未披露”记录；不能由 imagegen 工具名猜测图像模型。未提供真实推理日志的配置不记作已运行。

## 镜头、时间轴与采用参数

> [!info]- 配置：production.json
> ```json
> {
>   "schemaVersion": 1,
>   "title": "Anime narrative scene",
>   "format": {
>     "width": 1280,
>     "height": 720,
>     "fps": 24,
>     "durationFrames": 720
>   },
>   "fontFile": null,
>   "background": null,
>   "sfx": [],
>   "shots": [
>     {
>       "id": "scene-01",
>       "frames": 90,
>       "action": "",
>       "art": null,
>       "clip": null,
>       "status": "pending",
>       "review": null,
>       "generation": null,
>       "caption": null,
>       "dialogue": null
>     },
>     {
>       "id": "scene-02",
>       "frames": 90,
>       "action": "",
>       "art": null,
>       "clip": null,
>       "status": "pending",
>       "review": null,
>       "generation": null,
>       "caption": null,
>       "dialogue": null
>     },
>     {
>       "id": "scene-03",
>       "frames": 90,
>       "action": "",
>       "art": null,
>       "clip": null,
>       "status": "pending",
>       "review": null,
>       "generation": null,
>       "caption": null,
>       "dialogue": null
>     },
>     {
>       "id": "scene-04",
>       "frames": 90,
>       "action": "",
>       "art": null,
>       "clip": null,
>       "status": "pending",
>       "review": null,
>       "generation": null,
>       "caption": null,
>       "dialogue": null
>     },
>     {
>       "id": "scene-05",
>       "frames": 90,
>       "action": "",
>       "art": null,
>       "clip": null,
>       "status": "pending",
>       "review": null,
>       "generation": null,
>       "caption": null,
>       "dialogue": null
>     },
>     {
>       "id": "scene-06",
>       "frames": 90,
>       "action": "",
>       "art": null,
>       "clip": null,
>       "status": "pending",
>       "review": null,
>       "generation": null,
>       "caption": null,
>       "dialogue": null
>     },
>     {
>       "id": "scene-07",
>       "frames": 90,
>       "action": "",
>       "art": null,
>       "clip": null,
>       "status": "pending",
>       "review": null,
>       "generation": null,
>       "caption": null,
>       "dialogue": null
>     },
>     {
>       "id": "scene-08",
>       "frames": 90,
>       "action": "",
>       "art": null,
>       "clip": null,
>       "status": "pending",
>       "review": null,
>       "generation": null,
>       "caption": null,
>       "dialogue": null
>     }
>   ]
> }
> ```

> [!info]- 当前 Composition / 时间轴源码：videos/anime-scene-forge-portable-smoke-20261111/src/Root.tsx，L1–32
> ```tsx
> import {Audio,Video} from '@remotion/media';
> import {loadFont} from '@remotion/fonts';
> import {AbsoluteFill,Composition,Folder,Img,Sequence,Series,staticFile,useCurrentFrame} from 'remotion';
> import raw from '../production.json';
> 
> type Dialogue={file:string|null;text:string;fromFrame:number;sync:'on-screen'|'voiceover'};
> type ShotSpec={id:string;frames:number;art:string|null;clip:string|null;caption:{text:string;startFrame:number;endFrame:number}|null;dialogue:Dialogue|null};
> type Production={title:string;format:{width:number;height:number;fps:number;durationFrames:number};fontFile:string|null;background:{file:string;volume:number}|null;sfx:{file:string;frame:number;volume:number}[];shots:ShotSpec[]};
> const data=raw as unknown as Production;
> if(data.fontFile)loadFont({family:'ProductionFont',url:staticFile(data.fontFile)});
> const fontFamily=data.fontFile?'ProductionFont':'sans-serif';
> 
> const Shot=({shot}:{shot:ShotSpec})=>{
>   const frame=useCurrentFrame();
>   const caption=shot.caption&&frame>=shot.caption.startFrame&&frame<shot.caption.endFrame?shot.caption.text:null;
>   return <AbsoluteFill style={{background:'#171b22',fontFamily}}>
>     {shot.clip?<Video src={staticFile(shot.clip)} volume={0} style={{width:'100%',height:'100%'}} objectFit="cover"/>:
>       shot.art?<Img src={staticFile(shot.art)} style={{width:'100%',height:'100%',objectFit:'contain'}}/>:null}
>     {!shot.clip?<div style={{position:'absolute',left:28,top:24,color:'white',background:'#111c',padding:12,fontSize:24}}>Source footage pending · {shot.id}</div>:null}
>     {caption?<div style={{position:'absolute',bottom:38,left:36,right:36,textAlign:'center',fontSize:Math.round(data.format.width/42),fontWeight:600,color:'white',textShadow:'0 2px 5px #000, 0 0 2px #000',lineHeight:1.4,whiteSpace:'pre-wrap'}}>{caption}</div>:null}
>     {shot.dialogue?.file?<Sequence from={shot.dialogue.fromFrame} layout="none"><Audio src={staticFile(shot.dialogue.file)}/></Sequence>:null}
>   </AbsoluteFill>;
> };
> const Episode=()=> <AbsoluteFill style={{background:'#171b22'}}>
>   <Series>{data.shots.map(shot=><Series.Sequence key={shot.id} durationInFrames={shot.frames} premountFor={data.format.fps}><Shot shot={shot}/></Series.Sequence>)}</Series>
>   {data.background?<Audio src={staticFile(data.background.file)} volume={data.background.volume}/>:null}
>   {data.sfx.map((s,i)=><Sequence key={i} from={s.frame} layout="none"><Audio src={staticFile(s.file)} volume={s.volume}/></Sequence>)}
> </AbsoluteFill>;
> export const RemotionRoot=()=> <>
>   <Composition id="Episode" component={Episode} width={data.format.width} height={data.format.height} fps={data.format.fps} durationInFrames={data.format.durationFrames}/>
>   <Folder name="Scenes">{data.shots.map(shot=><Composition key={shot.id} id={shot.id} component={Shot} defaultProps={{shot}} width={data.format.width} height={data.format.height} fps={data.format.fps} durationInFrames={shot.frames}/>)}</Folder>
> </>;
> ```

源时间轴、生成帧数、所用区间与最终时长分别记录；历史文档或候选参数不自动覆盖当前采用配置。

## 参考来源与借鉴说明

在本次盘点的项目自有材料中，未找到可明确归属本片的外部参考视频或 X 帖子链接；这不等于历史上没有参考。


## 重现入口与修改位置

以下是 package.json 中的实际命令映射，供在对应视频目录使用；本轮未执行这些制作命令。系统依赖、前置模型/素材和环境准备见后附 README。

| npm 入口 | 实际命令 |
| --- | --- |
| `npm run dev` | `node scripts/run.mjs dev` |
| `npm run render` | `node scripts/run.mjs render` |
| `npm run render:draft` | `node scripts/run.mjs render-draft` |
| `npm run check:source` | `node scripts/run.mjs source-check` |
| `npm run check` | `node scripts/run.mjs check` |
| `npm run package` | `node scripts/run.mjs package` |

### 算法源码摘录

> [!info]- videos/anime-scene-forge-portable-smoke-20261111/src/Root.tsx，L1–32
> ```tsx
> import {Audio,Video} from '@remotion/media';
> import {loadFont} from '@remotion/fonts';
> import {AbsoluteFill,Composition,Folder,Img,Sequence,Series,staticFile,useCurrentFrame} from 'remotion';
> import raw from '../production.json';
> 
> type Dialogue={file:string|null;text:string;fromFrame:number;sync:'on-screen'|'voiceover'};
> type ShotSpec={id:string;frames:number;art:string|null;clip:string|null;caption:{text:string;startFrame:number;endFrame:number}|null;dialogue:Dialogue|null};
> type Production={title:string;format:{width:number;height:number;fps:number;durationFrames:number};fontFile:string|null;background:{file:string;volume:number}|null;sfx:{file:string;frame:number;volume:number}[];shots:ShotSpec[]};
> const data=raw as unknown as Production;
> if(data.fontFile)loadFont({family:'ProductionFont',url:staticFile(data.fontFile)});
> const fontFamily=data.fontFile?'ProductionFont':'sans-serif';
> 
> const Shot=({shot}:{shot:ShotSpec})=>{
>   const frame=useCurrentFrame();
>   const caption=shot.caption&&frame>=shot.caption.startFrame&&frame<shot.caption.endFrame?shot.caption.text:null;
>   return <AbsoluteFill style={{background:'#171b22',fontFamily}}>
>     {shot.clip?<Video src={staticFile(shot.clip)} volume={0} style={{width:'100%',height:'100%'}} objectFit="cover"/>:
>       shot.art?<Img src={staticFile(shot.art)} style={{width:'100%',height:'100%',objectFit:'contain'}}/>:null}
>     {!shot.clip?<div style={{position:'absolute',left:28,top:24,color:'white',background:'#111c',padding:12,fontSize:24}}>Source footage pending · {shot.id}</div>:null}
>     {caption?<div style={{position:'absolute',bottom:38,left:36,right:36,textAlign:'center',fontSize:Math.round(data.format.width/42),fontWeight:600,color:'white',textShadow:'0 2px 5px #000, 0 0 2px #000',lineHeight:1.4,whiteSpace:'pre-wrap'}}>{caption}</div>:null}
>     {shot.dialogue?.file?<Sequence from={shot.dialogue.fromFrame} layout="none"><Audio src={staticFile(shot.dialogue.file)}/></Sequence>:null}
>   </AbsoluteFill>;
> };
> const Episode=()=> <AbsoluteFill style={{background:'#171b22'}}>
>   <Series>{data.shots.map(shot=><Series.Sequence key={shot.id} durationInFrames={shot.frames} premountFor={data.format.fps}><Shot shot={shot}/></Series.Sequence>)}</Series>
>   {data.background?<Audio src={staticFile(data.background.file)} volume={data.background.volume}/>:null}
>   {data.sfx.map((s,i)=><Sequence key={i} from={s.frame} layout="none"><Audio src={staticFile(s.file)} volume={s.volume}/></Sequence>)}
> </AbsoluteFill>;
> export const RemotionRoot=()=> <>
>   <Composition id="Episode" component={Episode} width={data.format.width} height={data.format.height} fps={data.format.fps} durationInFrames={data.format.durationFrames}/>
>   <Folder name="Scenes">{data.shots.map(shot=><Composition key={shot.id} id={shot.id} component={Shot} defaultProps={{shot}} width={data.format.width} height={data.format.height} fps={data.format.fps} durationInFrames={shot.frames}/>)}</Folder>
> </>;
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

- `videos/anime-scene-forge-portable-smoke-20261111/outputs/checks/portable-scaffold-report.json`

## 制作方法与迭代记录选编

下面是项目自有文档的归档摘录，保留原阶段与原检查表述，个人路径已泛化、相对链接已注明源项目位置。它们属于来源陈述，不代表本次重新完成其中的生成或验收。

> [!info]- 来源文档：videos/anime-scene-forge-portable-smoke-20261111/README.md
> # Independent anime video project
> 
> Edit `docs/brief.md` and `production.json` before generating footage. All paths below are relative to this directory. The initial shot allocation is a timing scaffold, not a finished storyboard.
> 
> ## Install and preview
> 
> Requirements: Node.js 22+, npm, Python 3.10+, ffmpeg/ffprobe, Chrome or a project-local Remotion browser. Optional generation environments have separate requirements in `docs/mlx-profile.json` when installed.
> 
> ```sh
> npm ci --cache .cache/npm
> npm run check:source
> npm run dev
> ```
> 
> `CHROME_BIN` can point to an installed Chrome; `PYTHON_BIN` can select Python. The run wrapper sets cache/temp paths inside this project. Any extra installation/generation command must also set `TMPDIR`, `TMP`, `TEMP` and package caches inside `.cache` (use `scripts/project_env.py` for Python entrypoints). Do not share another video's virtualenv, node_modules, model paths or assets.
> 
> ## Add media and sound
> 
> - Keep complete reference artwork in `public/art`, raw candidates in `public/candidates`, and selected clips in `public/video`. Record prompts and backend versions under `docs`. Use forward slashes for browser-facing paths on every platform.
> - Set format and shot durations in `production.json`; their sum must equal `format.durationFrames`. Use the same native integer fps throughout unless a different route is explicitly designed.
> - Put adopted voice WAVs in `public/audio/voices` (48 kHz PCM16). Set `dialogue` to null for silent shots; for dialogue use `text`, `file`, `fromFrame`, and `sync` (`on-screen` or `voiceover`). On-screen speech additionally needs a matching `driverFile` and A2V generation record.
> - Add optional captions with local `startFrame`/`endFrame` and `text`. Copy a licensed font into `public/fonts` and set `fontFile` when exact glyph rendering matters.
> - `background` is null or `{ "file": "audio/bed.wav", "volume": 0.2 }`. Duck the bed for dialogue using your audio tools. `sfx` contains `{ "file": "audio/clap.wav", "frame": 100, "volume": 0.2 }`; event frames are global.
> 
> Examples (replace identifiers and paths with real files):
> 
> ```sh
> python3 scripts/project_ops.py pad-voice scene-01 --voice public/audio/voices/scene-01-v1.wav --from-frame 12 --source-frames 97 --take v1
> python3 scripts/project_ops.py select scene-01 --source public/candidates/scene-01-v1.mp4 --start 0 --take v1 --generation docs/generation/scene-01-v1.json
> python3 scripts/project_ops.py sheet scene-01
> npm run render:draft
> ```
> 
> `select` keeps native speed, mutes candidate audio and resets visual approval. A crop can be supplied as `--crop x y width height`; check heads, hands and objects after cropping. On a changed voice or timing, create a new versioned driver and regenerate on-screen speech. Never reuse the previous visual approval after media changes.
> 
> After actual frame review and normal-speed playback, write a project-local review JSON with `clipSha256`, `visualReviewed`, `normalSpeedReviewed`, `reviewer`, `observations`, and `limitations`. Point `shot.review` to it and set `shot.status` to `approved`. Helper success does not fill this record automatically. Keep candidate and user acceptance distinct.
> 
> ## Export, check, package
> 
> ```sh
> npm run render
> npm run check
> npm run package
> ```
> 
> Outputs: `outputs/final.mp4` and `outputs/project.zip`. The package contains this project's code, locked dependencies, media, source records and final video; reinstallable environments, models, caches and node_modules are excluded. Record actual techniques, failures, time and limitations in `docs/production-method.md`. Technical checks do not assess expression or phoneme accuracy.

> [!info]- 来源文档：videos/anime-scene-forge-portable-smoke-20261111/docs/brief.md
> # Anime narrative scene
> 
> 状态：制作规划，未生成成片。
> 
> - 时长：30秒，24fps，720帧。
> - 画幅：1280×720。
> - 角色、造型、场景与参考来源：待按用户要求填写。
> - 故事与动作：按叙事修改production.json中的默认等长分段。
> - 对白模式、语言、声线与情绪：待按用户要求填写。
> - 图像与生成后端：external；实际能力、版本和预算需记录。
> - 交付：按用户要求提供预览、MP4和独立工程。

> [!info]- 来源文档：videos/anime-scene-forge-portable-smoke-20261111/docs/production-method.md
> # 实际制作记录
> 
> 状态：尚未制作。完成后记录真实版本、候选、失败、修正、检查、耗时与限制。

## 证据清单与缺失信息

盘点的文本/报告文件 14 个，源文本、报告及媒体产物身份摘要为 `d16d1a0820a3b10e92f9474d5756e143c1036e9ac726d837a9d77ddc4993efee`。哈希证明采集内容身份，不代表其中每个断言已验证。模型权重、缓存、依赖、第三方源码树和未指定的历史会话未采集；大型文本仅做哈希并按需要选读。

- 本次整理基于明确引用的 README、制作方法、配置、提示词、关键源码与已有报告。未重新观看全部参考/候选/成片，也未做人工听辨。
- 项目未提供的原始模型内部实现、未披露生成后端、未记录 seed/参数、真实费用、人工总工时和用户最终验收，均不补造。实际日志有记录的耗时见上文，流水线耗时不等于整个制作总耗时。
- 源项目内的路径只用于查证；本档案未复制大型素材或模型，也不改变源项目运行依赖。

| 已盘点证据文件 | SHA-256（前 16 位）/状态 |
| --- | --- |
| `videos/anime-scene-forge-portable-smoke-20261111/README.md` | `e0761af8ee7817e3` |
| `videos/anime-scene-forge-portable-smoke-20261111/package-lock.json` | `944505517f445389` |
| `videos/anime-scene-forge-portable-smoke-20261111/package.json` | `c04bb4b2faf2916c` |
| `videos/anime-scene-forge-portable-smoke-20261111/production.json` | `b9c6b2f80423705d` |
| `videos/anime-scene-forge-portable-smoke-20261111/remotion.config.ts` | `5ad026e95030791b` |
| `videos/anime-scene-forge-portable-smoke-20261111/tsconfig.json` | `c414c90c7e726c32` |
| `videos/anime-scene-forge-portable-smoke-20261111/docs/brief.md` | `f1a5e34d6acff991` |
| `videos/anime-scene-forge-portable-smoke-20261111/docs/production-method.md` | `8e9c4ca8a8e974ac` |
| `videos/anime-scene-forge-portable-smoke-20261111/outputs/checks/portable-scaffold-report.json` | `350f9541c4743243` |
| `videos/anime-scene-forge-portable-smoke-20261111/scripts/project_env.py` | `9fa4016b6cce04fe` |
| `videos/anime-scene-forge-portable-smoke-20261111/scripts/project_ops.py` | `9d07bbd3574305df` |
| `videos/anime-scene-forge-portable-smoke-20261111/scripts/run.mjs` | `753d9012ec380054` |
| `videos/anime-scene-forge-portable-smoke-20261111/src/Root.tsx` | `90bb85ddcbaa652a` |
| `videos/anime-scene-forge-portable-smoke-20261111/src/index.ts` | `f9bc4e836a064aca` |
