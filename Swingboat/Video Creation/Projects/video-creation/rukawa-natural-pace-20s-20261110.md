---
type: video-production-dossier
project: video-creation
video_id: rukawa-natural-pace-20s-20261110
captured: '2026-11-12'
status: 已有 20 秒成片；源复盘记录用户认可可观看
source_fingerprint: 53a16476a82798fa4e93923cf618953d71e0ea2d33faf2acfe9527ef4fc500ea
tags:
- video-creation
- production-capture
---

# 流川枫 · 原速剪辑与自然节奏版

<!-- capture-source-sha256:53a16476a82798fa4e93923cf618953d71e0ea2d33faf2acfe9527ef4fc500ea -->

本次为指定项目的材料回溯与归档。制作结论来自项目记录/源码，未找回关闭的制作对话，也未重新执行生成、渲染、音频合成或用户验收。

> 本页的环境日志与模型文件清单已删节。

## 当前结论与制作路线

**状态：已有 20 秒成片；源复盘记录用户认可可观看。**

同框原画 → 本地连续生成 → 原速选帧 → 2 秒受控入网 → Remotion。

1280×720、24fps、480 帧。八镜头为控球 1.5 秒、防守 2.5 秒、持球 2 秒、出手 2 秒、入网 2 秒、庆祝 2 秒、看台 4 秒、终场 4 秒。用新增镜头补足时长，恢复人物动作 1 倍速。

## 关键算法、实现与适用边界

select-native-clips.py 记录源起点、持续秒数和取景矩形，分步处理视频/音频/封装；源 49/73/97 帧选出所需 48/72/96 帧，不冻结、倒放或光流补时。BasketAction 的轨迹/遮罩/声音重新按 2 秒镜头标定；终场声第 167 帧、比分第 207 帧。低内存推理沿用顺序文本编码、缓存与 --low-ram。

## 声音与字幕

采用生成环境声及程序终场/入网声，无解说配音；最后响度处理保留画面流。

## 失败、修正与验收边界

控球候选约 2.2 秒后畸变，只取 0.5–2 秒；防守边缘多球用固定 864×486 上身取景排除。核心人体错误须重做，不能用任意裁切掩盖。旧慢放时长问题通过生成新叙事镜头解决。

## 技术与模型版本

以下是本项目声明或锁定的依赖，安装清单本身不证明某项参与了最终画面。具体用途以制作路线、源码和生成记录交叉判断。

| 依赖 | package.json 声明 | 可解析的直接锁定版本 |
| --- | --- | --- |
| `@remotion/cli` | `4.0.541` | `4.0.541` |
| `@remotion/media` | `4.0.541` | `4.0.541` |
| `remotion` | `4.0.541` | `4.0.541` |
| `react` | `19.3.1` | `19.3.1` |
| `react-dom` | `19.3.1` | `19.3.1` |
| `@types/react` | `19.3.0` | `19.3.0` |
| `@types/react-dom` | `19.2.3` | `19.2.3` |
| `typescript` | `5.9.3` | `5.9.3` |

> [!info]- requirements-local-video.txt
> ```text
> annotated-doc==0.0.5
> annotated-types==0.8.0
> anyio==4.16.0
> click==8.5.1
> filelock==4.2.10
> fsspec==2026.9.0
> h11==0.16.0
> hf-xet==1.8.0
> httpcore2==2.16.0
> httpx2==2.16.0
> huggingface-hub==2.4.0
> idna==3.20
> jinja2==3.1.6
> markdown-it-py==4.2.0
> markupsafe==3.0.4
> mdurl==0.1.2
> mlx==0.32.4
> mlx-arsenal==0.19.0
> mlx-lm==0.32.2
> mlx-metal==0.32.4
> numpy==2.5.4
> packaging==26.3
> pillow==13.0.0
> protobuf==7.36.2
> pydantic==2.14.3
> pydantic-core==2.50.4
> pygments==2.21.0
> pyyaml==6.0.3
> regex==2026.11.9
> rich==15.0.0
> safetensors==0.8.0
> sentencepiece==0.2.3
> shellingham==1.5.4
> tqdm==4.70.1
> transformers==5.22.2
> truststore==0.10.5
> typer==0.29.0
> typing-extensions==4.16.0
> typing-inspection==0.4.5
> .cache/vendor/ltx-2-mlx/packages/ltx-core-mlx
> .cache/vendor/ltx-2-mlx/packages/ltx-pipelines-mlx
> ```


> [!info]- 版本证据：docs/local-generator-commit.txt（配置存在与实际使用分开）
> ```text
> 747b30ef10e92a011e85b2aa0f6c4ee86ced5218
> ```


> [!info]- 版本证据：docs/local-models.json（配置存在与实际使用分开）
> ```json
> [
>   {
>     "repo": "dgrauet/ltx-2.3-mlx-q4",
>     "revision": "f306259444116d71927188af2fbbf7fdc1a8089a",
>     "local": ".cache/models/ltx-q4",
>     "files": [
>       {
>         "name": "LICENSE",
>         "bytes": 19197
>       },
>       {
>         "name": "config.json",
>         "bytes": 951
>       },
>       {
>         "name": "connector.safetensors",
>         "bytes": 6344495512
>       },
>       {
>         "name": "embedded_config.json",
>         "bytes": 7234
>       },
>       {
>         "name": "quantize_config.json",
>         "bytes": 100
>       },
>       {
>         "name": "spatial_upscaler_x2_v1_1.safetensors",
>         "bytes": 995745061
>       },
>       {
>         "name": "spatial_upscaler_x2_v1_1_config.json",
>         "bytes": 275
>       },
>       {
>         "name": "split_model.json",
>         "bytes": 1893
>       },
>       {
>         "name": "transformer-distilled-1.1.safetensors",
>         "bytes": 11322002285
>       },
>       {
>         "name": "vae_decoder.safetensors",
>         "bytes": 814349531
>       },
>       {
>         "name": "vae_encoder.safetensors",
>         "bytes": 637885319
>       },
>       {
>         "name": "audio_vae.safetensors",
>         "bytes": 106509048
>       },
>       {
>         "name": "vocoder.safetensors",
>         "bytes": 258313851
>       }
>     ]
>   },
>   {
>     "repo": "mlx-community/gemma-3-12b-it-4bit",
>     "revision": "86cc6a8dedbc456dd0e4af01a9d09f396f77e558",
>     "local": ".cache/models/gemma-q4",
>     "files": [
>       {
>         "name": "README.md",
>         "bytes": 945
>       },
>       {
>         "name": "chat_template.json",
>         "bytes": 1615
>       },
>       {
>         "name": "config.json",
>         "bytes": 1141
>       },
>       {
>         "name": "generation_config.json",
>         "bytes": 192
>       },
>       {
>         "name": "model-00001-of-00002.safetensors",
>         "bytes": 5367455313
>       },
>       {
>         "name": "model-00002-of-00002.safetensors",
>         "bytes": 2661219935
>       },
>       {
>         "name": "model.safetensors.index.json",
>         "bytes": 108605
>       },
>       {
>         "name": "preprocessor_config.json",
>         "bytes": 570
>       },
>       {
>         "name": "processor_config.json",
>         "bytes": 70
>       }
>     ]
>   }
> ]
> ```


工具底层模型未公开时按“未披露”记录；不能由 imagegen 工具名猜测图像模型。未提供真实推理日志的配置不记作已运行。

## 镜头、时间轴与采用参数

> [!info]- 当前 Composition / 时间轴源码：videos/rukawa-natural-pace-20s-20261110/src/Root.tsx，L1–49
> ```tsx
> import React from 'react';
> import {Audio} from '@remotion/media';
> import {AbsoluteFill,Composition,Folder,Series,interpolate,staticFile,useCurrentFrame,useVideoConfig} from 'remotion';
> import {DribbleScene} from './DribbleScene';
> import {DefenderScene} from './DefenderScene';
> import {OpeningScene} from './OpeningScene';
> import {ShotScene,PilotScene} from './ShotScene';
> import {RimScene} from './RimScene';
> import {CelebrationScene} from './CelebrationScene';
> import {CrowdScene} from './CrowdScene';
> import {HeroScene} from './HeroScene';
> import {useLocalFont} from './font';
> import {BUZZER_SECONDS,SCORE_SECONDS} from './timing';
> import {clipsReady} from './media-status';
> const Film=()=>{
>  const frame=useCurrentFrame(),{fps}=useVideoConfig(),t=frame/fps;useLocalFont();
>  const complete=clipsReady.dribble&&clipsReady.defender&&clipsReady.close&&clipsReady.shot&&clipsReady.rim&&clipsReady.celebrate&&clipsReady.crowd&&clipsReady.hero;
>  const score=t>=SCORE_SECONDS?80:77,remaining=Math.max(0,BUZZER_SECONDS-t);
>  return <AbsoluteFill style={{backgroundColor:'#193b3d',fontFamily:'Arena,sans-serif'}}>
>   <Series>
>    <Series.Sequence name="最后七秒 · 控球" durationInFrames={1.5*fps} premountFor={fps}><DribbleScene/></Series.Sequence>
>    <Series.Sequence name="仙道 · 防守判断" durationInFrames={2.5*fps} premountFor={fps}><DefenderScene/></Series.Sequence>
>    <Series.Sequence name="抬眼 · 锁定篮筐" durationInFrames={2*fps} premountFor={fps}><OpeningScene/></Series.Sequence>
>    <Series.Sequence name="起跳 · 封盖 · 出手" durationInFrames={2*fps} premountFor={fps}><ShotScene/></Series.Sequence>
>    <Series.Sequence name="压哨 · 入网" durationInFrames={2*fps} premountFor={fps}><RimScene/></Series.Sequence>
>    <Series.Sequence name="湘北 · 庆祝" durationInFrames={2*fps} premountFor={fps}><CelebrationScene/></Series.Sequence>
>    <Series.Sequence name="看台 · 欢呼" durationInFrames={4*fps} premountFor={fps}><CrowdScene/></Series.Sequence>
>    <Series.Sequence name="流川枫 · 终场" durationInFrames={4*fps} premountFor={fps}><HeroScene/></Series.Sequence>
>   </Series>
>   {complete?<Audio src={staticFile('audio/buzzer.wav')} from={Math.round(BUZZER_SECONDS*fps)} premountFor={fps} volume={.85}/>:null}
>   {complete?<div style={{position:'absolute',left:28,top:24,padding:'8px 13px',backgroundColor:'#132c30db',color:'#fff2da',fontSize:19,display:'flex',gap:18,borderBottom:'2px solid #a92b30'}}><span>湘北 <b style={{color:t>=SCORE_SECONDS?'#f5cf8e':'#fff2da'}}>{score}</b> : 78 陵南</span><span style={{fontFamily:'Arial,sans-serif',color:'#f5cf8e',fontVariantNumeric:'tabular-nums'}}>{t>=BUZZER_SECONDS?'00.0':(Math.ceil(remaining*10)/10).toFixed(1).padStart(4,'0')}</span></div>:null}
>   {complete&&t>=16?<AbsoluteFill style={{background:'linear-gradient(90deg,#122e33bc,transparent 65%)',pointerEvents:'none'}}/>:null}
>   {complete&&t>=16?<div style={{position:'absolute',left:42,top:330,color:'#fff0d7',opacity:interpolate(frame,[16*fps,16.4*fps],[0,1],{extrapolateLeft:'clamp',extrapolateRight:'clamp'})}}><div style={{fontSize:43,letterSpacing:5}}>流川枫</div><div style={{height:3,width:50,background:'#bd3437',margin:'18px 0'}}/><div style={{fontSize:24,letterSpacing:3}}>压哨三分</div><div style={{fontSize:20,marginTop:11}}>80 : 78　湘北获胜</div></div>:null}
>  </AbsoluteFill>;
> };
> export const Root=()=> <>
>  <Composition id="RukawaNaturalPace" component={Film} width={1280} height={720} fps={24} durationInFrames={480}/>
>  <Folder name="Scenes">
>   <Composition id="Dribble" component={DribbleScene} width={1280} height={720} fps={24} durationInFrames={36}/>
>   <Composition id="Defender" component={DefenderScene} width={1280} height={720} fps={24} durationInFrames={60}/>
>   <Composition id="Opening" component={OpeningScene} width={1280} height={720} fps={24} durationInFrames={48}/>
>   <Composition id="Shot" component={ShotScene} width={1280} height={720} fps={24} durationInFrames={48}/>
>   <Composition id="Rim" component={RimScene} width={1280} height={720} fps={24} durationInFrames={48}/>
>   <Composition id="Celebration" component={CelebrationScene} width={1280} height={720} fps={24} durationInFrames={48}/>
>   <Composition id="Crowd" component={CrowdScene} width={1280} height={720} fps={24} durationInFrames={96}/>
>   <Composition id="Hero" component={HeroScene} width={1280} height={720} fps={24} durationInFrames={96}/>
>   <Composition id="MotionPilot" component={PilotScene} width={1280} height={720} fps={24} durationInFrames={49}/>
>  </Folder>
> </>;
> ```

源时间轴、生成帧数、所用区间与最终时长分别记录；历史文档或候选参数不自动覆盖当前采用配置。

## 参考来源与借鉴说明

在本次盘点的项目自有材料中，未找到可明确归属本片的外部参考视频或 X 帖子链接；这不等于历史上没有参考。

### 项目记录的技术、内容或素材来源

这些 URL 来自项目文件，尚不自动证明本片采用；除本节明确标注的访问外，本轮未逐一在线重读。

| 链接 | 记录文件 |
| --- | --- |
| [www.adobe.com](http://www.adobe.com/) | `videos/rukawa-natural-pace-20s-20261110/docs/NOTO-OFL.txt` |
| [scripts.sil.org](https://scripts.sil.org/OFL) | `videos/rukawa-natural-pace-20s-20261110/docs/NOTO-OFL.txt` |

## 重现入口与修改位置

以下是 package.json 中的实际命令映射，供在对应视频目录使用；本轮未执行这些制作命令。系统依赖、前置模型/素材和环境准备见后附 README。

| npm 入口 | 实际命令 |
| --- | --- |
| `npm run dev` | `node scripts/run.mjs dev` |
| `npm run render` | `node scripts/run.mjs render` |
| `npm run still` | `node scripts/run.mjs still` |
| `npm run check` | `node scripts/run.mjs check` |
| `npm run package` | `node scripts/run.mjs package` |

### 算法源码摘录

> [!info]- videos/rukawa-natural-pace-20s-20261110/src/BasketAction.tsx，L1–59
> ```tsx
> import React,{useEffect,useState} from 'react';
> import {Audio,Video} from '@remotion/media';
> import {AbsoluteFill,CanvasImage,cancelRender,continueRender,delayRender,staticFile,useCurrentFrame,useVideoConfig} from 'remotion';
> const clamp=(n:number)=>Math.max(0,Math.min(1,n));
> const sm=(a:number,b:number,t:number)=>{const u=clamp((t-a)/(b-a));return u*u*(3-2*u);};
> type P={x:number;y:number};
> function ballPosition(t:number):P{
>  if(t<=2){const u=t/2;return{x:683*(1-u)**2+2*760*u*(1-u)+849*u*u,y:239*(1-u)**2+2*50*u*(1-u)+386*u*u};}
>  const d=t-2;return{x:849+89*d,y:386+336*d+131.25*d*d};
> }
> function contact(t:number){const d=t-2;return sm(-.03,.22,d)*(1-sm(.85,1.12,d));}
> function pulse(t:number){const d=t-3;return .9*contact(t)-(d<0?0:.28*Math.exp(-3*d)*Math.sin(9*d));}
> function node(i:number,row:number,t:number):P{
>  const u=row/8,a=(i+(row%2)*.5)*Math.PI/8,k=pulse(t),r=181-101*u+25*k*u;
>  const age=Math.max(0,t-2),lean=89*Math.min(age,1)*contact(t)+12*k;
>  return{x:849+r*Math.cos(a)+lean*u,y:394+300*u+(46-27*u)*Math.sin(a)+72*k*u*u};
> }
> const Net:React.FC<{t:number;front:boolean}>=({t,front})=>{
>  const paths:React.ReactNode[]=[];
>  for(let i=0;i<16;i++)for(let row=0;row<8;row++){
>   if((Math.sin((i+.5)*Math.PI/8)>=0)!==front)continue;
>   const a=node(i,row,t),b=node(i,row+1,t),c=node(i+(row%2?1:-1),row+1,t);
>   paths.push(<path key={`s${i}-${row}`} d={`M${a.x} ${a.y}L${b.x} ${b.y}M${a.x} ${a.y}L${c.x} ${c.y}`} fill="none" stroke="#655c50" strokeWidth={6} opacity={front?.58:.3} strokeLinejoin="round"/>);
>   paths.push(<path key={`r${i}-${row}`} d={`M${a.x} ${a.y}L${b.x} ${b.y}M${a.x} ${a.y}L${c.x} ${c.y}`} fill="none" stroke={front?'#eee3ce':'#c8c0ad'} strokeWidth={4.1} opacity={front?1:.72} strokeLinejoin="round"/>);
>   paths.push(<circle key={`k${i}-${row}`} cx={b.x} cy={b.y} r={3.4} fill={front?'#eee3ce':'#c8c0ad'} stroke="#928673" strokeWidth={.8} opacity={front?1:.7}/>);
>  }
>  return <g strokeLinecap="round">{paths}</g>;
> };
> function nativeBall(t:number):P{
>  const keys=[[0,420,150],[1.25,529,95],[1.875,560,312],[2.5,571,450],[3.125,583,570],[3.5,592,680],[5,592,900]];
>  let j=1;while(j<keys.length-1&&t>keys[j][0])j++;
>  const a=keys[j-1],b=keys[j],u=clamp((t-a[0])/(b[0]-a[0]));return{x:(a[1]+(b[1]-a[1])*u)*1.25,y:(a[2]+(b[2]-a[2])*u)*1.25};
> }
> export const BasketAction=()=>{
>  const frame=useCurrentFrame(),{fps}=useVideoConfig(),t=frame/fps*5,ball=ballPosition(t),hole=nativeBall(frame/fps*2.5);
>  const [handle]=useState(()=>delayRender('Loading matching basketball texture'));
>  useEffect(()=>{const im=new Image();im.onload=()=>continueRender(handle);im.onerror=()=>cancelRender(new Error('Ball texture failed to load'));im.src=staticFile('art/rim-scene.png');},[handle]);
>  return <AbsoluteFill>
>   <CanvasImage src={staticFile('art/rim-clean-plate.png')} style={{width:'100%',height:'100%',objectFit:'cover'}}/>
>   <AbsoluteFill style={{maskImage:'linear-gradient(to bottom,transparent 61%,black 68%)',WebkitMaskImage:'linear-gradient(to bottom,transparent 61%,black 68%)'}}>
>    <AbsoluteFill style={{maskImage:`radial-gradient(circle 114px at ${hole.x}px ${hole.y}px,transparent 92%,black 100%)`,WebkitMaskImage:`radial-gradient(circle 114px at ${hole.x}px ${hole.y}px,transparent 92%,black 100%)`}}>
>     <AbsoluteFill style={{maskImage:'radial-gradient(ellipse 185px 160px at 650px 435px,transparent 95%,black 100%)',WebkitMaskImage:'radial-gradient(ellipse 185px 160px at 650px 435px,transparent 95%,black 100%)'}}>
>      <Video src={staticFile('video/rim.mp4')} premountFor={fps} volume={.65} style={{width:'100%',height:'100%',objectFit:'cover'}}/>
>     </AbsoluteFill>
>    </AbsoluteFill>
>   </AbsoluteFill>
>   <svg viewBox="0 0 1672 941" preserveAspectRatio="none" style={{position:'absolute',width:'100%',height:'100%'}}>
>    <defs><clipPath id="basketball-original-texture"><circle cx={0} cy={0} r={79.5}/></clipPath></defs>
>    <Net t={t} front={false}/>
>    <g transform={`translate(${ball.x} ${ball.y}) rotate(${-t*140})`}>
>     <image href={staticFile('art/rim-scene.png')} x={-683} y={-239} width={1672} height={941} clipPath="url(#basketball-original-texture)"/>
>     <circle r={80.3} fill="none" stroke="#342623" strokeWidth={2.9}/>
>    </g>
>    <Net t={t} front/>
>    {t<2.3?<g fill="none" strokeLinecap="round"><path d="M1039 386A190 53 0 0 1 659 386" stroke="#392722" strokeWidth={14}/><path d="M1039 386A190 53 0 0 1 659 386" stroke="#db702c" strokeWidth={10}/><path d="M1039 383A190 53 0 0 1 659 383" stroke="#f59942" strokeWidth={2}/></g>:null}
>   </svg>
>   <Audio src={staticFile('audio/net.wav')} from={Math.round(.4*fps)} premountFor={fps} volume={.85}/>
>  </AbsoluteFill>;
> };
> ```

> [!info]- videos/rukawa-natural-pace-20s-20261110/scripts/select-native-clips.py，L1–20
> ```python
> """Trim selected continuous source frames at 1x speed; never pad motion."""
> import argparse,json,os,re,shutil,subprocess
> from pathlib import Path
> ROOT=Path(__file__).resolve().parents[1]
> SOURCES={'dribble':('dribble-native24-natural4.mp4',1.5,.5),'defender':('defender-native24-natural3.mp4',2.5),'close':('close-native24.mp4',2),'shot':('shot-short-v2.mp4',2),'rim':('rim-native24.mp4',2),'celebrate':('celebrate-native24.mp4',2),'crowd':('crowd-native24-natural4.mp4',4),'hero':('hero-native24-natural4.mp4',4)}
> p=argparse.ArgumentParser();p.add_argument('scene',choices=list(SOURCES));args=p.parse_args();name=args.scene
> choice=SOURCES[name];source_name,seconds=choice[:2];start=choice[2] if len(choice)>2 else 0;source=ROOT/'public/video'/source_name
> ffmpeg=shutil.which('ffmpeg') or '/opt/homebrew/bin/ffmpeg';ffprobe=shutil.which('ffprobe') or '/opt/homebrew/bin/ffprobe'
> env={**os.environ,'TMPDIR':str(ROOT/'.cache/tmp'),'TMP':str(ROOT/'.cache/tmp'),'TEMP':str(ROOT/'.cache/tmp')}
> meta=json.loads(subprocess.check_output([ffprobe,'-v','error','-show_streams','-of','json',str(source)],text=True,env=env));v=next(s for s in meta['streams'] if s['codec_type']=='video')
> assert float(v['duration'])>=start+seconds and v['avg_frame_rate']=='24/1','Insufficient native frames; generate more footage.'
> frames=round(seconds*24);video=ROOT/'.cache/tmp'/f'{name}-native-video.mp4';audio=ROOT/'.cache/tmp'/f'{name}-native-audio.wav';out=ROOT/'public/video'/f'{name}.mp4'
> def run(args):subprocess.run([ffmpeg,'-v','error','-nostdin','-y',*args],env=env,cwd=ROOT,check=True,timeout=60)
> portrait_crop='crop=864:486:0:0,' if name=='defender' else ''
> run(['-filter_threads','1','-threads','2','-i',str(source),'-an','-vf',f'trim=start={start}:duration={seconds},setpts=PTS-STARTPTS,fps=24,{portrait_crop}scale=1024:576:force_original_aspect_ratio=increase:flags=lanczos,crop=1024:576','-frames:v',str(frames),'-c:v','libx264','-threads','2','-crf','17','-preset','medium','-pix_fmt','yuv420p',str(video)])
> run(['-i',str(source),'-vn','-af',f'atrim=start={start}:duration={seconds},asetpts=PTS-STARTPTS','-t',str(seconds),'-c:a','pcm_s16le',str(audio)])
> run(['-i',str(video),'-i',str(audio),'-map','0:v:0','-map','1:a:0','-c:v','copy','-c:a','aac','-b:a','160k','-af','apad','-t',str(seconds),'-movflags','+faststart',str(out)])
> status=ROOT/'src/media-status.ts';status.write_text(re.sub(r'\b'+name+r':false',name+':true',status.read_text()))
> record={'scene':name,'source':str(source.relative_to(ROOT)),'output':str(out.relative_to(ROOT)),'frames':frames,'fps':24,'duration':seconds,'source_seconds_used':seconds,'source_start_seconds':start,'crop_source_rect':[0,0,864,486] if name=='defender' else None,'playback_speed':1.0,'motion_interpolation':False,'method':'Native source frames trimmed at 1x; no slow motion, frame repetition, optical flow or freeze padding.'}
> (ROOT/'outputs/checks'/f'generation-{name}.json').write_text(json.dumps(record,indent=2));print(json.dumps(record))
> ```

## 完整关键提示词与生成记录

以下保留项目自有提示词、候选参数与版本。存在失败候选和历史参数；最终采用以 selected-takes、edit-decisions 与制作复盘为准，不能将所有候选都写成已采用。

> [!info]- videos/rukawa-natural-pace-20s-20261110/docs/celebration-correction-prompt.txt
> ```text
> Edit the reference image with ONE precise correction: remove the orange basketball visible inside the hoop/net at the upper-left. Restore that small region as empty space inside the same white basketball net, matching the teal/gray gym wall behind it and the existing net strings, so the hoop is empty after the winning shot has already fallen through. Preserve every character, face, uniform, body pose, hands, shoes, cheering audience, framing, floor, linework, colors, shadows and ALL other pixels as closely as possible. No other changes. Output the same complete widescreen anime celebration scene. No added text, no new ball anywhere.
> ```

> [!info]- videos/rukawa-natural-pace-20s-20261110/docs/celebration-grounded-image-prompt.txt
> ```text
> Edit this SAME complete anime celebration scene. Preserve the exact gym, crowd, hoop, lighting, linework, character identities, costume colors and all five players. Change only the two red teammates' starting body poses so a subsequent animation can celebrate IN PLACE.
> Red-haired SHOHOKU 10 at middle-left must stand with BOTH shoes fully planted on the court, feet comfortably apart, natural slightly flexed knees, torso upright, right fist held near chest ready to pump upward. He grins excitedly toward Rukawa. He is not taking a running step and must stay a clear body-width away from Rukawa.
> Tall muscular SHOHOKU 4 at far left must also stand with BOTH shoes planted, feet shoulder-width apart and upright balanced weight. His hands are raised naturally in front of chest ready to clap, smiling toward Rukawa. He must remain far left with clear spacing from the other players.
> Keep Rukawa in red SHOHOKU 11 standing calmly at the right, same sharp face, black fringe and black left wristband, relaxed arms and grounded shoes. Keep the two white/blue opponents farther behind, disappointed. Keep all people separated with clear silhouettes and correct hands and legs. Keep the basket EMPTY. The gallery audience is still celebrating with red towels.
> Match the exact existing restrained 1990s sports anime ink and two-tone cel shading. ONE integrated widescreen 16:9 image, no panels, captions, UI or extra characters. All feet on their original depth plane, anchored contact shadows, no walking pose or crossing bodies.
> ```

> [!info]- videos/rukawa-natural-pace-20s-20261110/docs/defender-image-prompt.txt
> ```text
> Use case: identity-preserve. Asset type: a new camera shot for this SAME anime basketball scene. The input is an identity, gym and rendering reference. Create a waist-up medium close-up of ONLY the central white-blue RYONAN number 7 defender (Sendoh Akira from Slam Dunk): exactly his tall spiky black hair, strong athletic shoulders, recognizable calm sharp face, white jersey with blue trim and blue RYONAN / 7 lettering. He faces three-quarter RIGHT toward Rukawa off screen, leaning a little forward in a defensive stance. Frame from lower chest to above hair, head centered a little left, one upper arm extended toward the lower right with the hand naturally outside frame; keep the other arm outside the lower crop. Focused eyes, one small sweat bead, closed lips, attentive restrained expression. Behind him the same muted teal gym wall, beige gallery band, pale metal railing, tall windows and small seated spectators of the reference, correct continuous location and daylight. Match exactly the fine dark ink, simple two-tone cel shadows, warm skin and painted late-1990s Japanese basketball anime style of the reference. Normal anatomy, face detailed and crisp, stable perspective, ONE person in foreground. No basketball, no foreground teammates, no exaggerated smile, no jump, no motion lines, no overlays, no captions. 16:9 landscape. This is a new shot within the same scene, not a blurry crop or enlargement of the source.
> ```

> [!info]- videos/rukawa-natural-pace-20s-20261110/docs/new-image-prompts.json
> ```json
> {
>   "dribble": "Use case: identity-preserve. Asset type: full-frame anime keyframe for the opening of a continuous video. Input image is the edit target and style reference. Preserve exactly this Japanese school gym, camera, daylight, teal walls, wood floor, hoop on left, gallery, thin ink lines and muted warm two-tone cel shading. Preserve the five player identities and jerseys: Rukawa black fringe red SHOHOKU 11 at far right, white-blue RYONAN 7 defender in center, muscular red 4 left, white-blue opponent far left, red-haired red 10 left-center. Change only their starting basketball poses: Rukawa is preparing to dribble, two feet planted naturally outside the three-point arc, knees slightly bent, his RIGHT hand controlling ONE orange basketball beside his right thigh at about knee height, left free hand shielding the ball. Hands and feet must be normal, well drawn, attached, no stretching. The center defender is in a low balanced stance, both feet on floor, arms partly spread, still clearly separate from Rukawa. Red 4 boxes out at far left with a clear readable silhouette; red 10 stands in the open left-center lane well apart from red 4, both feet planted, ready to move. Keep all player spacing clear. This is a few seconds BEFORE the jump shot, no one jumps or shoots in this frame. Keep manga-like proportion, coherent perspective and the same whole-frame anime artwork. 16:9 landscape. No captions, new overlays or duplicate balls.",
>   "crowd": "Use case: illustration-story. Asset type: full-frame anime crowd reaction keyframe for a basketball video. Input image is a STYLE AND LOCATION REFERENCE, not a crop to upscale. Create a NEW camera shot from near the wooden court looking up at the same gallery: waist-up medium-wide view of eight Japanese teenage and adult spectators standing behind the pale grey metal balcony railing. They are ordinary manga-like spectators in casual cream, blue, grey and muted red shirts; several hold small solid red towels. Natural varied faces, excited smiles and open cheering mouths, clear individual bodies and hands, no person fused with another. Their elbows begin partly bent, ready to lift arms and wave towels in celebration of the winning basket. Tall rectangular windows and soft green trees behind, teal gym wall edges and warm beige gallery trim matching reference. Match the reference's late-1990s Japanese basketball anime look, clean fine dark ink, flat two-tone cel shadows, muted warm daylight and subtle painted background texture. Important: whole frame is one coherent anime drawing, readable individual fans, stable railing perspective, no court players, no basketball, no visible hoop, no text or logos, no 3D, 16:9 landscape."
> }
> ```

> [!info]- videos/rukawa-natural-pace-20s-20261110/docs/new-motion-prompts.json
> ```json
> {
>   "dribble": "A continuous hand-drawn Japanese basketball anime at normal real-time human speed, preserving the five exact players, red and white-blue jerseys and inked cel style of the reference. Black-haired red number 11 on the far right dribbles the ONE basketball vertically with his right hand: three quick controlled low bounces during the first two seconds, each bounce visibly touches the wooden court and returns to his palm. He then gathers that same ball cleanly with both hands at chest height, steadies his feet and looks toward the basket on the left, ready for a jump shot. No shot yet. His feet stay outside the three-point arc, knees and elbow move naturally with the dribble. White-blue number 7 stays in his own area, makes one short lateral defensive shuffle and opens his arms, maintaining a clear gap from the ball handler. The left-side players shift their weight and shoulders without crossing each other; red 10 watches from his own open position. Gallery spectators react with small natural movements. Static wide camera, rigid court lines and architecture, consistent body lengths and faces, normal-speed basketball cadence, continuous clean hand-drawn cel animation.",
>   "crowd": "A lively continuous hand-drawn Japanese basketball anime crowd reaction at ordinary real-time human speed. Preserve these exact eight spectators, individual faces, clothing, red towels, standing positions, grey balcony railing and sunny gym windows. The far-left young man pumps his fist once quickly, the girl next to him makes a small excited cheer with hands near her chest, the young man in red gives two short energetic fist pumps. The bespectacled boy claps twice near his chest. The girl holding the red towel swings it in a small overhead arc twice, the older man raises his fist and cheers, the young man in blue shakes his red towel excitedly, and the woman at right cups a hand briefly beside her mouth while smiling. Small independent gestures with ordinary human timing, natural blinking and cheering expressions, all bodies retain clear separation and hands remain normal. Fixed camera, unchanged railing and background, same fine ink and two-tone cel animation, warm muted gym palette. No camera zoom, no newly appearing people or objects.",
>   "defender": "A continuous hand-drawn Japanese basketball anime close-up at ordinary real-time human speed, preserving the exact spiky-haired RYONAN number 7 defender and inked two-tone cel drawing of the reference. He watches the ball handler off screen to his right: his eyes briefly track down-right toward the low ball, then return up-right as the ball is gathered. His brows tighten slightly, he blinks once, shifts his shoulders a little forward and raises his extended arm slightly to prepare to contest. Natural alert breathing, normal human reaction timing, closed lips, no speaking, no smile. His head keeps the same three-quarter right angle, his face and hair remain unchanged, and his hands stay outside the lower frame. The small seated spectators behind turn their heads slightly, tense before the last shot. Fixed camera, rigid same gym and railing, no new people, coherent clean hand-drawn cel animation."
> }
> ```

> [!info]- videos/rukawa-natural-pace-20s-20261110/docs/prep-image-prompt.txt
> ```text
> Use case: illustration-story. Asset type: first frame for a coherent hand-drawn basketball anime video, one finished widescreen 16:9 scene.
> Input image is the approved unified STYLE MASTER and the exact camera / gym / five character identities to preserve. Redraw the SAME scene approximately 0.7 seconds BEFORE the shown jump-shot release. Keep the 1990s sports anime ink, cel shading, muted teal gym wall, overhead daylight, spectator gallery, hoop on upper left, five players, uniforms, hair, proportions and scene perspective consistent.
> On the right, black-fringe Kaede Rukawa in red SHOHOKU 11 and black left wristband is now standing with both shoes planted BEHIND the three-point line on the side AWAY from the basket. Knees flexed naturally to gather for a jump shot, hips lowered slightly, torso upright with small forward lean. Both natural five-finger hands securely cradle one orange basketball near upper chest; shooting hand behind/under ball, guide hand on side, normal wrists and elbows. Eyes fixed on hoop. Preserve his handsome sharp calm face.
> White/blue RYONAN 7 defender stands immediately to Rukawa's left, feet planted with loaded knees, leaning toward him, one hand coming up to contest. Their apparent heights are similar at this depth; they share the same court plane. The defender has tall spiky black hair and a focused face.
> Farther left, red SHOHOKU 4 is boxing out the SAME white/blue opponent, feet firmly on floor and weight against the opponent; red-haired SHOHOKU 10 has just begun running toward the basket while watching Rukawa. These are the same characters and costumes as the master, all drawn with the same ink density and restrained two-tone colors.
> Correct floor three-point marking so it clearly runs on the basket side of Rukawa's planted feet, about one shoe length away. Avoid a large arc behind his feet that would place him inside the line. Maintain enough empty margin around heads, fingers and shoes for movement. One shared light direction, attached contact shadows on all five players, plausible depth and proportions. Gallery crowd is a coherent anime crowd with varied poses, simpler faces at distance.
> NO airborne player at this starting moment. No ball in sky, no duplicate basketball. No scoreboard, subtitles, labels, arrows, panels, photographic textures, 3D render, stickers, cutout border or UI. ONE integrated scene for the beginning of an animated shot.
> ```

> [!info]- videos/rukawa-natural-pace-20s-20261110/docs/rim-clean-plate-prompt.txt
> ```text
> Precise cleanup edit of this SAME anime frame. Remove ONLY the orange basketball above-left of the hoop and remove ONLY the hanging white basketball NET and its white cords, so the orange metal rim is empty and has NO net. Restore the areas uncovered by the ball and net using the same transparent glass backboard, subtle white reflections, painted white rectangle and muted teal gym wall behind them. Preserve the exact original camera, the ENTIRE orange rim ellipse, its scale and position, the supporting orange bracket, glass backboard edges, metal support, every cheering spectator, windows, lighting, ink strokes and colors everywhere else. The rigid orange rim MUST NOT shift or change shape. No other changes, no new object, no text. Output the complete same-size widescreen image. The basketball and net will be animated separately.
> ```

> [!info]- videos/rukawa-natural-pace-20s-20261110/docs/scene-image-prompts.json
> ```json
> [
>   {
>     "key": "close",
>     "text": "Use case: illustration-story. One full 16:9 finished cel-anime frame for the opening of a basketball scene. Reference input is the approved full-scene style and all character identity. Draw a tight cinematic medium close-up of Kaede Rukawa in red SHOHOKU 11, black straight fringe, pale warm skin, slim teenage proportions, black left wristband. His eyes are intensely fixed on the off-screen basket to the left; the face is handsome and calm, clearly the SAME Rukawa as reference. Show natural hands supporting ONE orange basketball near his upper chest, shooting hand under/back and guide hand on side; arms connected naturally, no contorted fingers. Crop just below jersey number near waist, full head and fingertips stay within frame. Keep 1990s hand-drawn anime ink, restrained warm cel colors, consistent two-tone shading, muted teal gym background with small unified gallery crowd and windows softly simplified at depth. Background figures use the same visual language as the hero, no photographic bokeh or realistic skin. Shallow staging but no blurry face. Single overhead gym light direction. No captions, scoreboard, framing border, labels, panel grid or UI. This will be animated by a local video model, so leave modest margin around head/hands and keep the detailed identity clean."
>   },
>   {
>     "key": "rim",
>     "text": "Use case: illustration-story. ONE finished 16:9 hand-drawn basketball anime background frame, a basket close-up in the EXACT same gym and 1990s cel style as the reference. Camera looks up slightly at the hoop, backboard occupying upper middle/right and gym teal wall/windows/roof beams behind. Orange basketball rim is near center at x56%, y40%, seen from a low three-quarter angle as a clearly readable ellipse, correct rigid geometry, white painted rectangle on the transparent glass board, metal support on right. A natural hanging white knotted basketball net is attached around the rim and hangs down to y70%, clean and legible without overcomplicated duplicate lines. Make the basket, rim, net and characters use a consistent hand-inked drawing, modest flat color and warm/cool cel shading. Packed spectators with raised hands and red towels appear in the lower gallery/background under the basket; correct small scale and simplified anime faces like the reference. Warm gym light and muted teal palette. ONE orange basketball is approaching from the upper left, above the rim, about a ball diameter away from the mouth; ball size physically fits within rim (diameter roughly half opening width). It is clearly outside/above the net and not touching backboard or rim. This frame precedes a clean swish; plenty of space for the ball to fall through the hoop. No players blocking the rim. No text overlays, scoreboard, captions, panels, photoreal texture or 3D render. One coherent drawn scene, not separate pasted objects."
>   },
>   {
>     "key": "celebrate",
>     "text": "Use case: illustration-story. ONE finished widescreen 16:9 frame from the SAME coherent 1990s basketball anime as the reference input. The decisive three-pointer has just scored. Medium-wide on the COURT: Kaede Rukawa in red SHOHOKU 11, slim black straight fringe, black left wristband, calmly looking left toward the basket, shoulders relaxed after landing, both feet grounded; his red-haired teammate Sakuragi in SHOHOKU 10 is a few steps to his left turning toward him excitedly with a clenched fist, and muscular tall Akagi in red SHOHOKU 4 farther left smiling as he releases his box-out stance. The white-and-blue RYONAN 7 defender is farther behind, disappointed, lowering his arms. All are recognizable as the exact same characters, hair, body proportions and uniforms in the reference. Place the people at the proper scales and depths on ONE shared court plane; natural knees, elbows, hands, shared overhead lighting, grounded contact shadows. Show clear room between the teammates for them to approach and celebrate. Above the muted teal wall, a packed gallery crowd has just begun lifting arms and red towels in celebration. Same hand-painted wood and windows, all consistent two-tone cel shading, line weight and visual detail. Balanced cinematic layout, all heads and shoes within safe margin. No frozen cutout look, no model turntable, no huge poster-like face, no 3D rendering or photoreal texture. No captions, scoreboard, text outside uniforms, UI or panels."
>   },
>   {
>     "key": "hero",
>     "text": "Use case: illustration-story. One widescreen 16:9 finished 1990s hand-drawn sports anime frame for the quiet final shot AFTER a basketball victory. Reference input fixes the exact visual style and Kaede Rukawa identity. Draw a cinematic chest-up close-up of the SAME slim teenage Rukawa in red SHOHOKU 11, black straight fringe, sharply defined handsome eyes and small calm mouth, slight sweat at temple, natural neck and shoulders, one relaxed exhale. He looks slightly left and downward with restrained satisfaction, head in right half of frame, face at three-quarter view; keep full hair/head in frame with space above. No ball in hands or in frame after victory. Background is the same muted teal gym and gallery with small cheering anime spectators and red towels, drawn simply at depth. Consistent clean ink, natural anatomy, two-tone cel shade, warm skin and gym overhead light. Rukawa should look like the reference, NOT muscular mature man, photoreal human or 3D puppet. Composition leaves calmer darker teal space on left for an eventual minimal score overlay. No actual captions, text overlays, framing devices or panels. One integrated scene, not a pasted portrait.\nAdditional reference 2 is the approved Rukawa close-up: prioritize its exact facial identity and matching line work; change from tense pre-shot concentration to restrained post-victory calm. Reference 1 fixes the gym and overall palette."
>   }
> ]
> ```

> [!info]- videos/rukawa-natural-pace-20s-20261110/docs/style-master-prompt.txt
> ```text
> Use case: illustration-story
> Asset type: ONE integrated 16:9 anime-film keyframe, not a collage, no panels.
> Primary request: Rebuild a coherent scene from Slam Dunk: Kaede Rukawa in red SHOHOKU number 11 executes the final three-point jump shot while a RYONAN number 7 defender contests it, in a crowded Japanese high-school gym.
> Input images: Image 1 is the identity / costume reference for Rukawa ONLY: preserve the black straight fringe, slim athletic teenage build, sharp calm face, red uniform, white-and-black sneakers and black left wristband. Image 2 is the failed old video frame, supplied only to identify the situation to replace. Its mismatched perspective, widely separated pasted figures, stiff silhouettes, excessive empty floor and scoreboard must be completely rebuilt.
> Style: One cohesive high-quality 1990s hand-drawn basketball TV-anime frame. All players share the SAME clean ink stroke weight, expressive natural anatomy, warm skin palette, modest flat two-tone cel shadows and level of detail. A restrained hand-painted gym background with muted teal walls and warm wood, without photoreal floor texture. Scene, characters and crowd must look drawn by one animation team.
> Composition: a dynamic eye-level/slightly low three-quarter view of the whole contested jump shot, with all fingers, heads, basketball and shoes inside the frame with clear margin. Rukawa is the main figure on the right at the apex of a believable jump, shooting toward screen left. Right arm extended naturally above the forehead, right wrist relaxed, ball just leaving fingertips and visibly separated by a tiny gap. Left guide hand open near the ball, natural elbow. Torso upright, two legs with natural slight knee flex and relaxed ankles, about 30cm above the same court plane. Do not make the legs rubbery.
> The defender is immediately to Rukawa's left, close enough to contest, similar real-world height and head size at their shared depth; eyes tracking the ball, near arm reaching toward it with correct five-finger hand. His jump is a little lower, with believable momentum and balance. Their silhouettes overlap naturally a little, but their hands and bodies do not intersect. No disconnected limbs.
> Secondary players: three smaller players farther behind on the court in their correct perspective: red SHOHOKU number 10 with short red hair moving toward the basket, a red number 4 tall muscular center boxing out a white/blue opponent. These players must be part of the same drawing, equally coherent cel style, eyes and body orientation reacting to the shot.
> Space and light: a single consistent camera and floor perspective, 3-point line clearly in front of Rukawa's takeoff position toward the basket. Visible airborne cast shadows below the two jumpers, rear players' feet visibly contacting the floor, same overhead gym lighting on everyone, unified warm/cool balance and atmospheric depth. Basketball hoop visible upper left, correct scale and court relationship. Packed spectators on an elevated gallery beyond the teal wall, varied natural poses, painted more simply with distance.
> Text: only authentic uniform lettering and numbers. No scoreboard overlay, captions, decorative text, watermarks or UI.
> Avoid: separate cutout/sprite appearance, a 3D render, doll faces, different art styles across players, tiny defender next to giant shooter, floating standing players, wrong head scale, highly polished realistic wood, big empty floor, cropped fingertips, twisted wrists or S-shaped legs.
> Output one finished full-scene anime keyframe with integrated people, environment and lighting.
> ```

> [!info]- videos/rukawa-natural-pace-20s-20261110/docs/video-prompts.json
> ```json
> {
>   "pilot": "A continuous hand-drawn Japanese basketball anime shot in the same cel animation style as the reference frame. The black-haired boy in red number 11 bends his knees, smoothly raises the basketball from his chest above his forehead, extends his legs and jumps straight up, shoots the single ball toward the basket on the left with a natural right-handed wrist flick, then lands on his two feet with softened knees. The white and blue number 7 defender reacts to the ball rising, steps closer and jumps to contest with his right arm reaching upward, then lands a moment after the shooter. The red number 4 at the left shifts his feet and keeps boxing out the white-shirted opponent; the red-haired number 10 moves toward the basket while watching the flight. Everyone moves together through the same continuous time, with naturally bending joints, grounded weight and coordinated eyelines. The small gallery spectators turn their heads and lift hands. Static wide camera, unchanged gym architecture and court markings. The video remains hand-inked two-tone cel animation throughout, same identities, jerseys, faces and limb lengths, clean linework and warm gym colors.",
>   "close": "A restrained dramatic close-up from a hand-drawn Japanese basketball anime, preserving the reference character's exact face, black fringe and red number 11 jersey. He focuses his eyes toward the basket at the left, breathes once, blinks once, and slightly raises the single basketball from upper chest toward his chin. His two hands retain a natural basketball grip and normal fingers. His shoulders and elbows move together as he prepares his shot. Tiny natural hair movement and moving spectators far behind. Fixed cinematic camera, same muted teal gym, same thin ink outlines and two-tone cel coloring. Smooth purposeful subtle character acting, no change of illustration style.",
>   "rim": "An unbroken close-up in a hand-drawn Japanese basketball anime, exactly the same inked cel style as the reference. The ONE orange basketball falls diagonally down-right from its starting position above the hoop, enters cleanly through the center of the orange rim without hitting the metal or backboard, pushes the white net down and outward, then drops out the bottom of the net and exits the lower frame. The net rebounds and swings gently to settle while the rigid rim and backboard stay fixed. Spectators below simultaneously throw up their arms and wave red towels in celebration. A clean decisive swish, correct ball size relative to the basket. Locked-off camera, continuous natural motion, preserve the exact illustrated gym and coherent drawn spectators.",
>   "celebrate": "A coherent hand-drawn Japanese basketball anime celebration, keeping the exact five identities, ink lines and two-tone cel colors of the reference. All five players retain their exact floor positions and clear separation throughout. Red-haired red number 10 pumps his raised right fist once near his chest while grinning. The tall muscular red number 4 claps his two hands together once and smiles. Black-haired red number 11 makes a small head turn toward his teammates and relaxes his shoulders, calm and quietly satisfied. The two white and blue opponents lower their gaze and shoulders in disappointment. All feet remain planted on the same court marks, with natural balance and stable body proportions. The gallery audience independently raises arms and waves red towels. Fixed camera, same rigid gym architecture, same jersey numbers and facial identities, coherent fully drawn cel anime motion, restrained small upper-body gestures.",
>   "hero": "A continuous hand-drawn Japanese basketball anime close-up at ordinary human speed, preserving the exact black-haired teenage face, narrow focused eyes, fringe and red SHOHOKU 11 jersey from the reference. He exhales briefly, the tiny vapor puff disperses quickly, then raises his gaze toward the basket on the left. He makes one short restrained nod to acknowledge his teammates, blinks naturally once, and lets his shoulders settle. His expression remains calm, cool and almost neutral, with no broad grin and no talking. The gallery spectators behind cheer and wave red towels at normal lively speed. Consistent inked two-tone cel rendering throughout, fixed camera, natural facial timing, stable identity and gym perspective.",
>   "shot": "A continuous hand-drawn Japanese basketball anime shot in the same cel animation style as the reference frame. The black-haired boy in red number 11 bends his knees, smoothly raises the basketball from his chest above his forehead, extends his legs and jumps straight up, shoots the single ball toward the basket on the left with a natural right-handed wrist flick, then lands on his two feet with softened knees. The white and blue number 7 defender reacts to the ball rising, steps closer and jumps to contest with his right arm reaching upward, then lands a moment after the shooter. The red number 4 at the left shifts his feet and keeps boxing out the white-shirted opponent; the red-haired number 10 moves toward the basket while watching the flight. Everyone moves together through the same continuous time, with naturally bending joints, grounded weight and coordinated eyelines. The small gallery spectators turn their heads and lift hands. Static wide camera, unchanged gym architecture and court markings. The video remains hand-inked two-tone cel animation throughout, same identities, jerseys, faces and limb lengths, clean linework and warm gym colors."
> }
> ```

> [!info]- videos/rukawa-natural-pace-20s-20261110/outputs/checks/generation-celebrate.json
> ```json
> {
>   "scene": "celebrate",
>   "source": "public/video/celebrate-native24.mp4",
>   "output": "public/video/celebrate.mp4",
>   "frames": 48,
>   "fps": 24,
>   "duration": 2,
>   "source_seconds_used": 2,
>   "playback_speed": 1.0,
>   "motion_interpolation": false,
>   "method": "Native source frames trimmed at 1x; no slow motion, frame repetition, optical flow or freeze padding."
> }
> ```

> [!info]- videos/rukawa-natural-pace-20s-20261110/outputs/checks/generation-close.json
> ```json
> {
>   "scene": "close",
>   "source": "public/video/close-native24.mp4",
>   "output": "public/video/close.mp4",
>   "frames": 48,
>   "fps": 24,
>   "duration": 2,
>   "source_seconds_used": 2,
>   "playback_speed": 1.0,
>   "motion_interpolation": false,
>   "method": "Native source frames trimmed at 1x; no slow motion, frame repetition, optical flow or freeze padding."
> }
> ```

> [!info]- videos/rukawa-natural-pace-20s-20261110/outputs/checks/generation-crowd-natural4.json
> ```json
> {
>   "scene": "crowd",
>   "output": "public/video/crowd-native24-natural4.mp4",
>   "frames": 97,
>   "width": 1024,
>   "height": 576,
>   "fps": 24,
>   "seed": 7711,
>   "source_image": "public/art/crowd-scene.png",
>   "prompt": "A lively continuous hand-drawn Japanese basketball anime crowd reaction at ordinary real-time human speed. Preserve these exact eight spectators, individual faces, clothing, red towels, standing positions, grey balcony railing and sunny gym windows. The far-left young man pumps his fist once quickly, the girl next to him makes a small excited cheer with hands near her chest, the young man in red gives two short energetic fist pumps. The bespectacled boy claps twice near his chest. The girl holding the red towel swings it in a small overhead arc twice, the older man raises his fist and cheers, the young man in blue shakes his red towel excitedly, and the woman at right cups a hand briefly beside her mouth while smiling. Small independent gestures with ordinary human timing, natural blinking and cheering expressions, all bodies retain clear separation and hands remain normal. Fixed camera, unchanged railing and background, same fine ink and two-tone cel animation, warm muted gym palette. No camera zoom, no newly appearing people or objects.",
>   "method": "Native-speed whole-scene local LTX-2.3, q4, default float32 compute, low-ram block streaming",
>   "exit_code": 0,
>   "wall_seconds": 417.04,
>   "output_exists": true
> }
> ```

> [!info]- videos/rukawa-natural-pace-20s-20261110/outputs/checks/generation-crowd.json
> ```json
> {
>   "scene": "crowd",
>   "source": "public/video/crowd-native24-natural4.mp4",
>   "output": "public/video/crowd.mp4",
>   "frames": 96,
>   "fps": 24,
>   "duration": 4,
>   "source_seconds_used": 4,
>   "source_start_seconds": 0,
>   "playback_speed": 1.0,
>   "motion_interpolation": false,
>   "method": "Native source frames trimmed at 1x; no slow motion, frame repetition, optical flow or freeze padding."
> }
> ```

> [!info]- videos/rukawa-natural-pace-20s-20261110/outputs/checks/generation-defender-natural3.json
> ```json
> {
>   "scene": "defender",
>   "output": "public/video/defender-native24-natural3.mp4",
>   "frames": 73,
>   "width": 1024,
>   "height": 576,
>   "fps": 24,
>   "seed": 7711,
>   "source_image": "public/art/defender-scene.png",
>   "prompt": "A continuous hand-drawn Japanese basketball anime close-up at ordinary real-time human speed, preserving the exact spiky-haired RYONAN number 7 defender and inked two-tone cel drawing of the reference. He watches the ball handler off screen to his right: his eyes briefly track down-right toward the low ball, then return up-right as the ball is gathered. His brows tighten slightly, he blinks once, shifts his shoulders a little forward and raises his extended arm slightly to prepare to contest. Natural alert breathing, normal human reaction timing, closed lips, no speaking, no smile. His head keeps the same three-quarter right angle, his face and hair remain unchanged, and his hands stay outside the lower frame. The small seated spectators behind turn their heads slightly, tense before the last shot. Fixed camera, rigid same gym and railing, no new people, coherent clean hand-drawn cel animation.",
>   "method": "Native-speed whole-scene local LTX-2.3, q4, default float32 compute, low-ram block streaming",
>   "exit_code": 0,
>   "wall_seconds": 301.3,
>   "output_exists": true
> }
> ```

> [!info]- videos/rukawa-natural-pace-20s-20261110/outputs/checks/generation-defender.json
> ```json
> {
>   "scene": "defender",
>   "source": "public/video/defender-native24-natural3.mp4",
>   "output": "public/video/defender.mp4",
>   "frames": 60,
>   "fps": 24,
>   "duration": 2.5,
>   "source_seconds_used": 2.5,
>   "source_start_seconds": 0,
>   "crop_source_rect": [
>     0,
>     0,
>     864,
>     486
>   ],
>   "playback_speed": 1.0,
>   "motion_interpolation": false,
>   "method": "Native source frames trimmed at 1x; no slow motion, frame repetition, optical flow or freeze padding."
> }
> ```

> [!info]- videos/rukawa-natural-pace-20s-20261110/outputs/checks/generation-dribble-natural4.json
> ```json
> {
>   "scene": "dribble",
>   "output": "public/video/dribble-native24-natural4.mp4",
>   "frames": 97,
>   "width": 1024,
>   "height": 576,
>   "fps": 24,
>   "seed": 7711,
>   "source_image": "public/art/dribble-scene.png",
>   "prompt": "A continuous hand-drawn Japanese basketball anime at normal real-time human speed, preserving the five exact players, red and white-blue jerseys and inked cel style of the reference. Black-haired red number 11 on the far right dribbles the ONE basketball vertically with his right hand: three quick controlled low bounces during the first two seconds, each bounce visibly touches the wooden court and returns to his palm. He then gathers that same ball cleanly with both hands at chest height, steadies his feet and looks toward the basket on the left, ready for a jump shot. No shot yet. His feet stay outside the three-point arc, knees and elbow move naturally with the dribble. White-blue number 7 stays in his own area, makes one short lateral defensive shuffle and opens his arms, maintaining a clear gap from the ball handler. The left-side players shift their weight and shoulders without crossing each other; red 10 watches from his own open position. Gallery spectators react with small natural movements. Static wide camera, rigid court lines and architecture, consistent body lengths and faces, normal-speed basketball cadence, continuous clean hand-drawn cel animation.",
>   "method": "Native-speed whole-scene local LTX-2.3, q4, default float32 compute, low-ram block streaming",
>   "exit_code": 0,
>   "wall_seconds": 399.82,
>   "output_exists": true
> }
> ```

> [!info]- videos/rukawa-natural-pace-20s-20261110/outputs/checks/generation-dribble.json
> ```json
> {
>   "scene": "dribble",
>   "source": "public/video/dribble-native24-natural4.mp4",
>   "output": "public/video/dribble.mp4",
>   "frames": 36,
>   "fps": 24,
>   "duration": 1.5,
>   "source_seconds_used": 1.5,
>   "source_start_seconds": 0.5,
>   "playback_speed": 1.0,
>   "motion_interpolation": false,
>   "method": "Native source frames trimmed at 1x; no slow motion, frame repetition, optical flow or freeze padding."
> }
> ```

> [!info]- videos/rukawa-natural-pace-20s-20261110/outputs/checks/generation-hero-natural4.json
> ```json
> {
>   "scene": "hero",
>   "output": "public/video/hero-native24-natural4.mp4",
>   "frames": 97,
>   "width": 1024,
>   "height": 576,
>   "fps": 24,
>   "seed": 7711,
>   "prompt": "A continuous hand-drawn Japanese basketball anime close-up at ordinary human speed, preserving the exact black-haired teenage face, narrow focused eyes, fringe and red SHOHOKU 11 jersey from the reference. He exhales briefly, the tiny vapor puff disperses quickly, then raises his gaze toward the basket on the left. He makes one short restrained nod to acknowledge his teammates, blinks naturally once, and lets his shoulders settle. His expression remains calm, cool and almost neutral, with no broad grin and no talking. The gallery spectators behind cheer and wave red towels at normal lively speed. Consistent inked two-tone cel rendering throughout, fixed camera, natural facial timing, stable identity and gym perspective.",
>   "method": "whole-scene local LTX-2.3 image-to-video, q4, block streaming",
>   "dit_compute_dtype": "default-float32",
>   "exit_code": 0,
>   "wall_seconds": 457.46,
>   "output_exists": true
> }
> ```

> [!info]- videos/rukawa-natural-pace-20s-20261110/outputs/checks/generation-hero.json
> ```json
> {
>   "scene": "hero",
>   "source": "public/video/hero-native24-natural4.mp4",
>   "output": "public/video/hero.mp4",
>   "frames": 96,
>   "fps": 24,
>   "duration": 4,
>   "source_seconds_used": 4,
>   "playback_speed": 1.0,
>   "motion_interpolation": false,
>   "method": "Native source frames trimmed at 1x; no slow motion, frame repetition, optical flow or freeze padding."
> }
> ```

> [!info]- videos/rukawa-natural-pace-20s-20261110/outputs/checks/generation-rim.json
> ```json
> {
>   "scene": "rim",
>   "source": "public/video/rim-native24.mp4",
>   "output": "public/video/rim.mp4",
>   "frames": 48,
>   "fps": 24,
>   "duration": 2,
>   "source_seconds_used": 2,
>   "playback_speed": 1.0,
>   "motion_interpolation": false,
>   "method": "Native source frames trimmed at 1x; no slow motion, frame repetition, optical flow or freeze padding."
> }
> ```

> [!info]- videos/rukawa-natural-pace-20s-20261110/outputs/checks/generation-shot.json
> ```json
> {
>   "scene": "shot",
>   "source": "public/video/shot-short-v2.mp4",
>   "output": "public/video/shot.mp4",
>   "frames": 48,
>   "fps": 24,
>   "duration": 2,
>   "source_seconds_used": 2,
>   "playback_speed": 1.0,
>   "motion_interpolation": false,
>   "method": "Native source frames trimmed at 1x; no slow motion, frame repetition, optical flow or freeze padding."
> }
> ```

## 已有产物与检查依据

文件存在、技术检查、画面检查与用户验收分别看待。下表是本次文件盘点，旧版与当前版可能共存。MP4/HTML 按内容计算 SHA-256，ZIP 仅记录文件大小和修改时间供增量判断。

| 本地产物 | 字节数 | SHA-256 |
| --- | ---: | --- |
| `videos/rukawa-natural-pace-20s-20261110/outputs/review.html` | 1876 | `95dab3de3c701dc47d02cc67f61e179f13b1cd2a2ba1b9d3aca056a832fce318` |
| `videos/rukawa-natural-pace-20s-20261110/outputs/rukawa-natural-pace-20s.mp4` | 17203887 | `846f5cf630d72295f72a825166e5f182a00508ba251ea49c3bb610f2bf61148e` |
| `videos/rukawa-natural-pace-20s-20261110/outputs/rukawa-natural-pace-project.zip` | 82408915 | `ZIP：仅元数据` |

> [!info]- 本次只读 ffprobe 结果
> ```json
> {
>   "programs": [],
>   "stream_groups": [],
>   "streams": [
>     {
>       "codec_name": "h264",
>       "codec_type": "video",
>       "width": 1280,
>       "height": 720,
>       "r_frame_rate": "24/1",
>       "duration": "20.000000",
>       "nb_frames": "480",
>       "side_data_list": [
>         {}
>       ]
>     },
>     {
>       "codec_name": "aac",
>       "codec_type": "audio",
>       "sample_rate": "48000",
>       "channels": 2,
>       "r_frame_rate": "0/0",
>       "duration": "20.000000",
>       "nb_frames": "935"
>     }
>   ],
>   "format": {
>     "duration": "20.000000",
>     "size": "17203887"
>   },
>   "file": "outputs/rukawa-natural-pace-20s.mp4",
>   "scope": "本次只读取媒体头；未全片解码、播放或重新进行视觉/听觉验收。"
> }
> ```


已有检查报告路径（未在本轮重新执行）：

- `videos/rukawa-natural-pace-20s-20261110/outputs/checks/audio-normalization.json`
- `videos/rukawa-natural-pace-20s-20261110/outputs/checks/media-metadata.json`
- `videos/rukawa-natural-pace-20s-20261110/outputs/checks/package-report.json`
- `videos/rukawa-natural-pace-20s-20261110/outputs/checks/verification.json`
- `videos/rukawa-natural-pace-20s-20261110/outputs/checks/cadence/report.json`
- `videos/rukawa-natural-pace-20s-20261110/outputs/checks/clip-frames/crowd/source-frames.json`
- `videos/rukawa-natural-pace-20s-20261110/outputs/checks/clip-frames/defender/source-frames.json`
- `videos/rukawa-natural-pace-20s-20261110/outputs/checks/clip-frames/dribble/source-frames.json`
- `videos/rukawa-natural-pace-20s-20261110/outputs/checks/clip-frames/hero-native24-natural4/source-frames.json`
- `videos/rukawa-natural-pace-20s-20261110/outputs/checks/final-frames/positions.json`

## 制作方法与迭代记录选编

下面是项目自有文档的归档摘录，保留原阶段与原检查表述，个人路径已泛化、相对链接已注明源项目位置。它们属于来源陈述，不代表本次重新完成其中的生成或验收。

> [!info]- 来源文档：videos/rukawa-natural-pace-20s-20261110/README.md
> # 流川枫三分绝杀 · 自然节奏版
> 
> 20秒，1280×720，24fps。人物片段采用原速，入网段采用2秒受控合成；新增控球、防守反应、看台及终场连续镜头，补足时长。素材均保存在本目录。
> 
> 本轮实际技术、失败原因、修正方案、模型版本、耗时和以后制作同类视频的步骤，见 制作复盘与复用流程（项目文件：`videos/rukawa-natural-pace-20s-20261110/docs/production-method.md`）。
> 
> ## 独立安装与预览
> 
> 需要 Node.js 22或更新版本、npm、Python 3、ffmpeg、ffprobe 和 Chrome。
> 
> ```sh
> mkdir -p .cache/tmp .cache/npm
> TMPDIR="$PWD/.cache/tmp" TMP="$PWD/.cache/tmp" TEMP="$PWD/.cache/tmp" npm ci --cache .cache/npm
> npm run dev
> ```
> 
> 预览：`http://localhost:3099/RukawaNaturalPace`。制作中未就绪的镜头显示“原画预览”，导出脚本会拒绝把它们作为成片。
> 
> ## 检查、导出和打包
> 
> ```sh
> npm run render
> npm run check
> npm run package
> ```
> 
> 成片：`outputs/rukawa-natural-pace-20s.mp4`；工程包：`outputs/rukawa-natural-pace-project.zip`。导出后调整声音响度，复制视频流以保留画面。检查包括类型、独立路径、源片段时长、480帧、24fps、音视频编码及全片解码。画面与动作质量另见 `docs/visual-review.md`。
> 
> ## 修改素材与时间轴
> 
> `src/Root.tsx` 中的八个 `Series.Sequence` 是独立可编辑镜头，时长为1.5、2.5、2、2、2、2、4、4秒。`src/timing.ts` 控制终场声和记分时刻。`src/BasketAction.tsx` 控制固定篮圈、单球轨迹与球网回弹。`public/art/` 存完整原画，`public/video/` 存连续动画；`public/audio/` 是本地补充音效。没有解说配音；环境声保存在各镜头视频中。修改相应组件的 `volume` 可调整声音。字体来自本目录 `public/fonts/`，许可在 `docs/NOTO-OFL.txt`。
> 
> `src/media-status.ts` 标记素材是否准备完成。`scripts/select-native-clips.py` 从本目录的候选视频中按1倍速选择原帧，没有慢放、光流补帧、冻结补时或反向循环。
> 
> ## 本地重新生成
> 
> 普通预览与导出使用已保存的片段，不需要模型。重新生成连续片段需要 Apple Silicon Mac、Python 3.13、uv、git 及约30GB模型空间。模型、环境和缓存均在本目录。制作时每次只运行一个推理进程。
> 
> ```sh
> python3 scripts/setup-generator.py
> .venv/bin/python scripts/fetch-video-model.py
> .venv/bin/python scripts/generate-clip.py dribble --suffix=-natural4
> .venv/bin/python scripts/select-native-clips.py dribble
> .venv/bin/python scripts/generate-clip.py defender --suffix=-natural3
> .venv/bin/python scripts/select-native-clips.py defender
> .venv/bin/python scripts/generate-clip.py crowd --suffix=-natural4
> .venv/bin/python scripts/select-native-clips.py crowd
> .venv/bin/python scripts/generate-clip.py hero --suffix=-natural4
> .venv/bin/python scripts/select-native-clips.py hero
> ```
> 
> 新增动作提示词在 `docs/new-motion-prompts.json`，其他提示词在 `docs/video-prompts.json`，新增原画提示词在 `docs/new-image-prompts.json`。`select-native-clips.py` 也可处理 `close`、`shot`、`rim` 和 `celebrate`。
> 
> ZIP包含源码、锁定依赖、原画、已生成的连续片段和成片。可重新安装或下载的 `node_modules/`、`.venv/`、模型和工具缓存不打包。所有运行依赖在当前工程内；从旧版复用的内容均是本地独立副本。

> [!info]- 来源文档：videos/rukawa-natural-pace-20s-20261110/docs/production-method.md
> # 流川枫二维动漫视频制作复盘与复用流程
> 
> 本次最终采用的组合是：**统一完整场景原画 → 本地整幅图生视频 → 选择干净的原速片段 → 程序修正关键物体 → Remotion剪辑与音效 → 画面和文件双重验收**。
> 
> 成片为20秒、1280×720、24fps，共480帧，H.264视频与AAC音频。用户在重制后认可了画面，在恢复正常动作节奏后确认成片可以观看。以下记录2026年11月10日这套工程实际跑通的方法，供以后制作类似的二维动漫角色短片复用。
> 
> ## 一 最终使用的技术
> 
> | 环节 | 实际技术 | 在本片中的作用 |
> |---|---|---|
> | 原画与身份参考 | 内置 `image_gen`，参考已认可的人设和球馆画面 | 在一幅完整画面内统一人物、队服、配角、观众、透视、线条和光照；生成新机位与干净底图 |
> | 连续角色动画 | 本机 `LTX-2.3 q4`，通过 `ltx-2-mlx` 推理 | 从整幅原画生成同一时间内的角色与观众动作 |
> | 文本编码与内存适配 | Gemma 3 12B 4bit、MLX、Python | 顺序加载文本模型与连接器，缓存提示词向量，逐块读取视频模型 |
> | 画面选择与音视频处理 | FFmpeg、ffprobe、Python | 按原速裁切、取景、缩放、音轨对齐、编码、响度处理和媒体检查 |
> | 关键物体动画 | React、SVG、遮罩、参数化轨迹和阻尼回弹 | 固定篮圈与篮板，控制单个篮球、前后遮挡、球网形变与回弹 |
> | 时间轴与合成 | Remotion `4.0.541`、React `19.3.1`、TypeScript、Chrome | 独立镜头、比分、倒计时、终场文字、音效及最终导出 |
> | 交付 | 独立npm工程、版本锁、Python打包脚本 | 保存素材、源码、提示词、检查报告、成片和ZIP；验证压缩包CRC与成片哈希 |
> 
> 原画由内置图像工具生成；视频推理在本机离线运行。本轮没有使用外部视频生成服务，也没有使用现成动漫视频片段。工具保留的图像原始位置见 image-assets.json（项目文件：`videos/rukawa-natural-pace-20s-20261110/docs/image-assets.json`），实际运行素材均已复制到本工程。
> 
> ### 固定的模型和代码版本
> 
> - 视频实现：`dgrauet/ltx-2-mlx`，提交 `747b30ef10e92a011e85b2aa0f6c4ee86ced5218`。
> - 视频权重：`dgrauet/ltx-2.3-mlx-q4`，修订 `f306259444116d71927188af2fbbf7fdc1a8089a`。
> - 文本模型：`mlx-community/gemma-3-12b-it-4bit`，修订 `86cc6a8dedbc456dd0e4af01a9d09f396f77e558`。
> - 使用量化权重，视频变换器采用本轮已验证的默认float32计算设置；权重量化与计算精度是两件事。
> - 精确依赖、模型文件和版本见 package.json（项目文件：`videos/rukawa-natural-pace-20s-20261110/package.json`）、requirements-local-video.txt（项目文件：`videos/rukawa-natural-pace-20s-20261110/requirements-local-video.txt`）、local-models.json（项目文件：`videos/rukawa-natural-pace-20s-20261110/docs/local-models.json`）。版本升级应先做短片验证。
> 
> ## 二 从最初方案到最终方案
> 
> 最初沿用了《瓶中风暴》的程序化三维路线：Python、`bpy` / `mathutils`、IK、Blender Cycles与Metal渲染，再用FFmpeg和Remotion合成。后续也试过连续人体网格与骨骼、二维姿态图集、人物网格变形、完整原画的有限运动。这些尝试分别改善了局部问题，但没有同时满足本次的动漫造型、人体观感、多人协调和动作连续性要求。
> 
> 最终将人物与环境放进完整同框原画，再生成连续视频。镜头剪辑负责动作衔接与节奏，确定性要求高的篮圈、篮球和球网单独修正。骨骼连接、画面有变化或文件能播放，都不能作为动漫效果已经合格的证明。
> 
> ## 三 发生过的问题和采用的解决方案
> 
> | 问题 | 本轮发现 | 最终处理与复用原则 |
> |---|---|---|
> | 人物像木偶，造型不符 | 程序化三维人物和人体基础模型未达到已认可的动漫角色观感 | 以认可的二维人设为身份基准，后续各镜头沿用相同发型、脸型、球衣与配色 |
> | 不同人物动作不连贯 | 独立姿态图切换、不同角色各自摆动，缺少统一的动作过程 | 主体表演改用整幅场景的连续图生视频；动作提示同时写明主角、对手、队友和观众的反应 |
> | 手腕和腿部畸变 | 同一伸手图旋转不能形成握球手型；骨长约束不能修正弯膝后的外轮廓 | 先生成正确人体结构与手球接触的原画；完整抬臂、起跳等动作采用连续生成，避免把微动绑定放大成大动作 |
> | 配角与观众不动 | 只描述主角，或只给背景做整体位移 | 在同框动作提示里分配防守、卡位、挥拳、拍手、转头、挥毛巾等反应，并逐项查看实际视频 |
> | 长镜头后段人物融合 | 长版投篮及多人交叉移动的后段出现重叠与伪影 | 采用更短、单一目标的动作段，只选干净时间范围；用反应镜头和新机位连接故事 |
> | 庆祝时身体穿插 | 队友奔跑接近流川枫，人物越靠越近 | 重绘双脚站稳、间距清楚的完整场景；改为原地挥拳、拍手、小幅转头 |
> | 篮圈变形、球网漂移 | 直接生成的入网镜头无法保持刚性结构和稳定网形 | 保留固定底图；用原画篮球纹理、单球轨迹、固定连接点的球网和前后遮挡重建前景；生成视频只保留看台区域与环境声 |
> | 整片像慢动作 | 将约2秒源动作拉长为3至5秒；旧投篮段4秒中有48处近似延长显示 | 投篮与庆祝恢复1倍速，篮筐段缩短；新增控球、防守反应、看台与终场素材，保持20秒总长 |
> | 控球段提前投篮并出现手部伪影 | 新生成4秒候选的约2.2秒后开始失真 | 只取源片段0.5至2.0秒，共1.5秒；补入2.5秒防守判断特写 |
> | 防守者边缘出现多余篮球 | 新特写右下边缘出现不应有的球 | 采用固定864×486的脸部与上身取景，完整排除该边缘；重新检查输出。若错误进入核心动作或人物主体，应重做该片段 |
> | 18GB内存下模型加载压力大 | 文本模型、连接器和视频模型需要控制驻留顺序 | Gemma编码后物化隐藏状态并释放，再加载连接器投影；使用 `--low-ram`、提示词向量缓存、单个GPU推理进程及MLX内存额度设置 |
> | float16试跑遇到GPU错误 | Metal返回 `InnocentVictim` 错误，不能据此认定具体硬件原因 | 本轮退回已跑通的默认计算设置；不把这次错误泛化为所有机器都不能使用float16 |
> | FFmpeg组合处理曾卡住 | 同时处理视频与音频的一次命令未完成，原因未确定 | 拆成视频处理、音频处理、有限时长封装三步，各步骤设置超时 |
> | 复用项目出现依赖路径冲突 | Python可编辑依赖残留旧视频的绝对URL | 改成本目录相对包路径，重新建立虚拟环境；检查配置、脚本、依赖与素材路径 |
> | 技术检查通过，画面仍不可用 | 类型、解码、骨长、区域差异无法判断造型和自然度 | 技术检查与视觉验收分别执行；逐帧查看关键动作，再播放实际导出的完整视频 |
> 
> 裁切和剪辑只用于能完整排除错误、保持叙事成立的情况。核心人体畸变、握球失败、关键物体复制或严重身份变化，应通过修正原画、减少动作复杂度或重新生成来解决。
> 
> ## 四 关键技术的具体实现
> 
> ### 整幅场景原画与动作提示
> 
> 先确定角色身份和球馆，再生成各机位的完整原画。提示词分别说明输入图片是编辑目标、身份参考还是画风参考。新机位需要重新生成清楚的画面，不能把远景小脸直接放大当成高清特写。
> 
> 每段提示写清：动作的开始与结束、哪个角色做什么、人物间距、脚下支撑、视线、物体数量、镜头是否移动、配角与观众如何反应。对本轮多人场景，保持站位的小幅表演比人物互相穿越的长动作更容易获得可用结果。
> 
> 完整提示词保存在 video-prompts.json（项目文件：`videos/rukawa-natural-pace-20s-20261110/docs/video-prompts.json`）、new-motion-prompts.json（项目文件：`videos/rukawa-natural-pace-20s-20261110/docs/new-motion-prompts.json`）、scene-image-prompts.json（项目文件：`videos/rukawa-natural-pace-20s-20261110/docs/scene-image-prompts.json`）、new-image-prompts.json（项目文件：`videos/rukawa-natural-pace-20s-20261110/docs/new-image-prompts.json`） 以及本目录各原画修正提示文件中。身份和动作是否一致仍需检查成片，固定seed不能代替这一检查。
> 
> ### 在这台Mac上运行本地模型
> 
> 本机为M3 Pro、18GB统一内存。所选模型文件合计约28.55GB，即26.59GiB；还需要给依赖、虚拟环境、缓存和中间视频留出空间。
> 
> lowram_prompt.py（项目文件：`videos/rukawa-natural-pace-20s-20261110/scripts/lowram_prompt.py`） 将Gemma与连接器分开加载，并把提示词向量缓存为NPZ；ltx-entry.py（项目文件：`videos/rukawa-natural-pace-20s-20261110/scripts/ltx-entry.py`） 配置本工程缓存、离线推理、11GiB的MLX内存额度和缓存清理。额度与MLX峰值不等于整个进程或系统的内存占用。本轮每次只运行一个视频推理任务。
> 
> 以下来自本片实际生成记录，包含编码、去噪、解码与封装，不含原画制作、首次下载及人工修正时间：
> 
> | 候选 | 原生帧数与规格 | 本机耗时 | 最终选用 |
> |---|---|---|---|
> | 终场 | 97帧，1024×576，24fps | 约7.6分钟 | 前4秒 |
> | 控球 | 97帧，1024×576，24fps | 约6.7分钟 | 0.5至2.0秒 |
> | 看台 | 97帧，1024×576，24fps | 约7.0分钟 | 前4秒 |
> | 防守者 | 73帧，1024×576，24fps | 约5.0分钟 | 前2.5秒，并调整取景 |
> 
> 这些记录用于估算同机、同版本、同规格工作的等待时间，不是其他设备的速度或内存保证。所用管线已测试的短片预设包括49、73、97帧，分别略长于2、3、4秒，剪辑时选取所需的48、72、96帧等范围。提示词缓存的模型或适配器版本变化时，应更新缓存命名或重新计算。
> 
> ### 篮圈与球网的混合合成
> 
> 实现见 BasketAction.tsx（项目文件：`videos/rukawa-natural-pace-20s-20261110/src/BasketAction.tsx`）：
> 
> 1. 用相同原画生成移除篮球与球网的干净底图，保留刚性的篮板和篮圈。
> 2. 从原画截取篮球纹理，绘制单个篮球的连续轨迹与旋转。
> 3. 用16列、8层的网结构成菱形球网；上端固定在篮圈，接触时拉伸、偏移，再阻尼回弹。
> 4. 按后网、篮球、前网和篮圈前沿的顺序绘制，检查穿网过程的遮挡。
> 5. 看台区域使用生成视频，通过渐变和局部遮罩排除其原有篮球及变形球网。
> 6. 球网动作、看台遮罩跟踪、入网声与记分各自对齐时间；改变速度时分别重新检查。
> 
> 本片2秒入网镜头中，受控篮球约在0.4秒穿过篮圈，约0.63秒落出画面。当前坐标、遮罩和速度按这幅原画校准；换机位、分辨率或篮筐图后，需要重新标定，不能直接照抄这些数值。
> 
> ### 原速剪辑与声音
> 
> select-native-clips.py（项目文件：`videos/rukawa-natural-pace-20s-20261110/scripts/select-native-clips.py`） 记录源文件、起点、所用秒数、取景矩形、帧数和1倍速设置。它分三步处理视频、音频和封装。最终人物片段没有使用光流补帧、倒放循环或冻结来补时。
> 
> 各镜头保留本地生成的环境声；sfx.py（项目文件：`videos/rukawa-natural-pace-20s-20261110/scripts/sfx.py`） 合成补充终场声与入网声。final-audio.py（项目文件：`videos/rukawa-natural-pace-20s-20261110/scripts/final-audio.py`） 调整响度并复制视频流。响度配置目标与实际测量应分别查看，记录保存在 `outputs/checks/audio-normalization.json`。
> 
> ## 五 最终20秒时间轴
> 
> | 全片时间 | 镜头 | 选用方式 |
> |---|---|---|
> | 0至1.5秒 | 控球与收球 | 候选0.5至2.0秒，原速 |
> | 1.5至4秒 | 仙道判断与防守反应 | 2.5秒，原速，上身取景 |
> | 4至6秒 | 流川枫持球、锁定篮筐 | 2秒，原速 |
> | 6至8秒 | 起跳、封盖、出手、落地 | 由4秒慢放恢复为2秒原速 |
> | 8至10秒 | 篮球入网、球网回弹 | 2秒混合合成 |
> | 10至12秒 | 队友庆祝 | 由4秒慢放恢复为2秒原速 |
> | 12至16秒 | 看台欢呼 | 新生成4秒原速动作 |
> | 16至20秒 | 流川枫终场表演 | 新生成4秒原速动作 |
> 
> 终场声位于第167帧，约6.958秒，已核对该帧篮球离手；比分在第207帧，即8.625秒更新为80:78。镜头与文字见 Root.tsx（项目文件：`videos/rukawa-natural-pace-20s-20261110/src/Root.tsx`），事件时点见 timing.ts（项目文件：`videos/rukawa-natural-pace-20s-20261110/src/timing.ts`）。
> 
> ## 六 以后制作类似视频的操作流程
> 
> ### 1 建立独立工程并确定视觉基准
> 
> 先建立新的 `videos/<video-id>/`。写明故事、时长、画幅、角色身份、参考图、场景、重要配角和声音要求。将这些制作要求保存在新视频自己的 `docs/` 中。
> 
> 复用素材、脚本和模型时复制到新目录，在新目录重新安装依赖。核对npm包名与锁文件，清除Python可编辑依赖中的旧绝对路径。模型可以采用独立的普通复制或APFS文件克隆，不能用跨视频符号链接或硬链接。缓存、临时文件和输出均使用当前视频目录。
> 
> ### 2 先验证一个2至4秒的关键动作
> 
> 优先选择最容易暴露问题的动作，例如握球、起跳或多人接触。先检查原画的手型、四肢、重心与透视，再生成短片，按正常速度查看。完成内部质量检查后再扩展其他镜头；按用户已有授权持续推进，用户明确要求先看样片时再按其要求交付该阶段。
> 
> ### 3 逐镜头生成和选择
> 
> 一段镜头围绕一个主要动作目标。分别写明主角、配角和观众的反应，给人物留出清楚间距。候选采用版本后缀保存，保留原始源片段和生成元数据。先看候选，再选择可用时间范围。
> 
> 生成素材不足以覆盖目标时长时，补充真实的新镜头：防守判断、手部准备、队友、观众或终场反应。默认按正常节奏剪辑；慢动作应有明确表达目的。
> 
> ### 4 修正关键物体并剪成故事
> 
> 对必须固定或数量准确的物体，使用与原画一致的程序合成。检查球与手的接触、离手、飞行、入网、音效及记分的先后关系。新机位重新校准坐标和遮罩。镜头交接要保留人物位置、视线和动作因果。
> 
> ### 5 分别完成视觉验收和技术验收
> 
> 视觉验收：查看开头、中间、结尾及关键接触帧，再以1倍速播放完整导出文件。检查脸与队服、手指与膝盖、脚底接触、物体数量、人物分离、观众反应、镜头衔接、事件时点和整体节奏。均匀抽取9帧只是定位工具，关键动作仍应逐帧查看。
> 
> 技术验收：检查独立路径、依赖锁、素材状态、时长、帧数、编码、全片解码和包内成片哈希。原画预览和未验收的候选不能作为完成交付。将实际失败、采用的修正和遗留限制写进本视频的复盘。
> 
> ## 七 本工程可以直接复用的入口
> 
> 已有素材的预览、导出与打包不需要重新运行视频模型。在当前视频目录执行：
> 
> ```sh
> mkdir -p .cache/tmp .cache/npm
> export TMPDIR="$PWD/.cache/tmp" TMP="$PWD/.cache/tmp" TEMP="$PWD/.cache/tmp"
> npm ci --cache .cache/npm
> npm run dev
> npm run render
> npm run check
> npm run package
> ```
> 
> 需要重新生成本工程的看台候选时，可按以下顺序操作。模型首次准备需要下载，之后推理离线运行；每次只启动一个视频推理进程：
> 
> ```sh
> python3 scripts/setup-generator.py
> .venv/bin/python scripts/fetch-video-model.py
> .venv/bin/python scripts/generate-clip.py crowd --suffix=-natural4
> python3 scripts/review-clip.py crowd-native24-natural4
> # 查看候选，通过画面检查后选择
> python3 scripts/select-native-clips.py crowd
> ```
> 
> 换成新故事时，下列内容需要一起调整：
> 
> | 文件 | 要修改的内容 |
> |---|---|
> | `scripts/generate-clip.py` | 场景名、原画、分辨率、原生帧数与生成预设 |
> | `scripts/select-native-clips.py` | 候选文件名、源起点、选取时长与取景框 |
> | `src/Root.tsx` 和各场景组件 | 镜头顺序、每段时长、独立Composition、文本和声音 |
> | `src/timing.ts` 与物体动画组件 | 动作事件、音效、记分、轨迹、遮罩和遮挡 |
> | `scripts/run.mjs`、`check.py`、`final-audio.py`、`package.py` | 合成ID、总时长、成片文件名、检查规格和打包目标 |
> | 模型与提示词记录 | 代码提交、模型修订、seed、图片哈希、提示词及缓存版本 |
> 
> 这些脚本包含本片的场景名、20秒时长和篮球坐标，属于已验证的具体实现。复用时需要在新工程内改成新片参数。
> 
> ## 八 下次可以直接使用的需求描述
> 
> ```text
> 按本项目已验证的二维动漫短片流程制作。
> 故事与关键事件：……
> 人物身份和参考图：……
> 场景、配角与观众反应：……
> 时长、画幅、字幕及声音要求：……
> 采用统一完整场景原画、本地连续图生视频、原速剪辑和关键物体程序修正。
> 先验证2至4秒核心动作，通过内部画面检查后继续完整制作。
> 所有素材、源码、缓存、模型和交付放进一个新的独立视频目录。
> 保留提示词、版本、失败候选、解决方案和最终验收记录。
> ```
> 
> ## 九 本次结果与边界
> 
> 最终成片通过480帧、24fps、20秒、H.264/AAC、独立运行路径和全片解码检查；浏览器已直接播放导出的MP4到20.0秒结束。验收细节见 visual-review.md（项目文件：`videos/rukawa-natural-pace-20s-20261110/docs/visual-review.md`），技术报告见 verification.json（项目文件：`videos/rukawa-natural-pace-20s-20261110/outputs/checks/verification.json`）。
> 
> 远景脸部、球衣英文和快速动作线条仍可能软化。该流程已在这次短片中获得可用结果，后续仍要按镜头检查角色身份、人体与节奏；它不等同于逐张手绘的专业角色动画。

> [!info]- 来源文档：videos/rukawa-natural-pace-20s-20261110/docs/pace-plan.md
> # 镜头节奏
> 
> | 全片时间 | 镜头 | 处理 |
> |---|---|---|
> | 0–1.5秒 | 控球与收球 | 使用新候选0.5–2.0秒的干净部分，原速 |
> | 1.5–4秒 | 仙道判断与防守反应 | 新生成连续特写，原速2.5秒 |
> | 4–6秒 | 持球、锁定篮筐 | 已有源片段前2秒，原速 |
> | 6–8秒 | 起跳、封盖、出手、落地 | 由4秒慢放恢复到2秒 |
> | 8–10秒 | 篮球入网、球网回弹 | 由5秒缩短到2秒 |
> | 10–12秒 | 队友庆祝 | 由4秒慢放恢复到2秒 |
> | 12–16秒 | 看台欢呼 | 新生成4秒连续动作，原速 |
> | 16–20秒 | 流川枫终场表演 | 新生成4秒连续动作，原速 |
> 
> 所有素材在1倍速下使用，终场声与比分时点随时间轴重排。

> [!info]- 来源文档：videos/rukawa-natural-pace-20s-20261110/docs/visual-review.md
> # 自然节奏版视觉验收
> 
> ## 已检查
> 
> - 投篮：原速2秒使用48个连续源帧。与上版4秒版本比较，低分辨率亮度检测中，近似停留的相邻帧由48处减少到0处。此指标用于发现延长显示，不能单独证明人体动作自然。详细数据在 `outputs/checks/cadence/`。
> - 入网：以2秒正常速度渲染，先检查篮圈固定、单球穿网、网底拉伸与复位。进一步把实际球网动作集中在前1秒，背景看台原速播放；最终时点需再验收。
> - 终场：重新生成4秒原生连续帧，检查0至4秒的面部与发型、抬眼、呼气、点头及眨眼，表情比上一版更克制，未见后段脸部融合。
> 
> - 控球：候选后段出现提前投篮与手部伪影，已排除。只保留0.5至2.0秒，检查接球、下落和收球，队员保持分离。
> 
> - 看台：4秒原速片段中，八位观众的面孔与站位保持分离，检查1、2、3、4秒的拍手、挥拳与红毛巾运动；未见多人融合。快速动作存在轻微线条软化。
> - 压哨：检查投篮源片段第23帧，即全片终场声触发帧，篮球已离开手部并飞出画面。
> 
> - 防守者：使用2.5秒原速反应，采用脸部与上身取景。原候选右下角出现不应有的篮球，已通过固定864×486取景框排除该边缘区域；重新检查所用开头、中间、结尾，脸部、肩臂与发型保持一致。
> - 入网最终时点：检查第10、15、47帧，单球在约0.4秒穿过篮圈，约0.63秒落出画面，网结随后复位，无重复篮球。
> 
> ## 最终验收
> 
> 已抽查控球→防守、防守→持球、持球→起跳、出手→篮筐、入网记分、庆祝→看台及终场文字的切镜帧。前段使用原速片段，篮球在终场声前已离手，比分在球通过篮圈后变为80:78。浏览器直接播放导出的MP4到20.0秒结束，视频元素无错误，分辨率1280×720。
> 
> 技术检查通过：480帧、24fps、H.264/AAC、20.0秒、全片解码、类型、素材独立路径与运行配置路径。每个选用镜头的1倍速、源起点与所用时长记录在本工程的 `outputs/checks/generation-*.json`。
> 
> ## 限制
> 
> 远景脸部与球衣英文仍可能软化。原速剪辑改善节奏，不等于逐张手绘的专业动画。技术检查不能代替画面与动作验收。

## 证据清单与缺失信息

盘点的文本/报告文件 77 个，源文本、报告及媒体产物身份摘要为 `53a16476a82798fa4e93923cf618953d71e0ea2d33faf2acfe9527ef4fc500ea`。哈希证明采集内容身份，不代表其中每个断言已验证。模型权重、缓存、依赖、第三方源码树和未指定的历史会话未采集；大型文本仅做哈希并按需要选读。

- 本次整理基于明确引用的 README、制作方法、配置、提示词、关键源码与已有报告。未重新观看全部参考/候选/成片，也未做人工听辨。
- 项目未提供的原始模型内部实现、未披露生成后端、未记录 seed/参数、真实费用、人工总工时和用户最终验收，均不补造。实际日志有记录的耗时见上文，流水线耗时不等于整个制作总耗时。
- 源项目内的路径只用于查证；本档案未复制大型素材或模型，也不改变源项目运行依赖。

| 已盘点证据文件 | SHA-256（前 16 位）/状态 |
| --- | --- |
| `videos/rukawa-natural-pace-20s-20261110/README.md` | `e7b7df97a0778fa6` |
| `videos/rukawa-natural-pace-20s-20261110/package-lock.json` | `36cc0494b4111d10` |
| `videos/rukawa-natural-pace-20s-20261110/package.json` | `236442955e183ad7` |
| `videos/rukawa-natural-pace-20s-20261110/remotion.config.ts` | `89558888f253880f` |
| `videos/rukawa-natural-pace-20s-20261110/requirements-local-video.txt` | `022301641961a33b` |
| `videos/rukawa-natural-pace-20s-20261110/tsconfig.json` | `af262af0dd89b1e2` |
| `videos/rukawa-natural-pace-20s-20261110/docs/NOTO-OFL.txt` | `1c05c68c34f97084` |
| `videos/rukawa-natural-pace-20s-20261110/docs/asset-reuse.json` | `ffd541a949e79d54` |
| `videos/rukawa-natural-pace-20s-20261110/docs/celebration-correction-prompt.txt` | `33647520ba4c5a1a` |
| `videos/rukawa-natural-pace-20s-20261110/docs/celebration-grounded-image-prompt.txt` | `8308c0e38b456cad` |
| `videos/rukawa-natural-pace-20s-20261110/docs/defender-image-prompt.txt` | `ec2243992fc5502d` |
| `videos/rukawa-natural-pace-20s-20261110/docs/image-assets.json` | `a241e981a640aa65` |
| `videos/rukawa-natural-pace-20s-20261110/docs/local-generator-commit.txt` | `cefeb3f58443a4e6` |
| `videos/rukawa-natural-pace-20s-20261110/docs/local-models.json` | `061b2acac26482fc` |
| `videos/rukawa-natural-pace-20s-20261110/docs/new-image-prompts.json` | `633b23e8646bdf71` |
| `videos/rukawa-natural-pace-20s-20261110/docs/new-motion-prompts.json` | `6749dfae131d798c` |
| `videos/rukawa-natural-pace-20s-20261110/docs/pace-plan.md` | `06cf02f1c22c2874` |
| `videos/rukawa-natural-pace-20s-20261110/docs/prep-image-prompt.txt` | `65e6f303828db237` |
| `videos/rukawa-natural-pace-20s-20261110/docs/production-method.md` | `e9309ad6fd3d8da1` |
| `videos/rukawa-natural-pace-20s-20261110/docs/rim-clean-plate-prompt.txt` | `9d1ec48b1ab4d01c` |
| `videos/rukawa-natural-pace-20s-20261110/docs/scene-image-prompts.json` | `fa5b1ac5b2482604` |
| `videos/rukawa-natural-pace-20s-20261110/docs/style-master-prompt.txt` | `62ed3d6d00647704` |
| `videos/rukawa-natural-pace-20s-20261110/docs/video-prompts.json` | `2f1c5eb47a5d4456` |
| `videos/rukawa-natural-pace-20s-20261110/docs/visual-review.md` | `aa75eb80e4e3d8c4` |
| `videos/rukawa-natural-pace-20s-20261110/outputs/checks/audio-normalization.json` | `69641bf0de772f63` |
| `videos/rukawa-natural-pace-20s-20261110/outputs/checks/generation-celebrate.json` | `aa9dcfc4ca803fb5` |
| `videos/rukawa-natural-pace-20s-20261110/outputs/checks/generation-close.json` | `ee8fa8ff9f9f5f6e` |
| `videos/rukawa-natural-pace-20s-20261110/outputs/checks/generation-crowd-natural4.json` | `9d6e19b27f67cf93` |
| `videos/rukawa-natural-pace-20s-20261110/outputs/checks/generation-crowd.json` | `017b2807628a563c` |
| `videos/rukawa-natural-pace-20s-20261110/outputs/checks/generation-defender-natural3.json` | `b629d90d7738c5e5` |
| `videos/rukawa-natural-pace-20s-20261110/outputs/checks/generation-defender.json` | `3d509c27cb0f57c7` |
| `videos/rukawa-natural-pace-20s-20261110/outputs/checks/generation-dribble-natural4.json` | `ad8c036920dab6d9` |
| `videos/rukawa-natural-pace-20s-20261110/outputs/checks/generation-dribble.json` | `11784f832c75f373` |
| `videos/rukawa-natural-pace-20s-20261110/outputs/checks/generation-hero-natural4.json` | `8af2245fd581c31b` |
| `videos/rukawa-natural-pace-20s-20261110/outputs/checks/generation-hero.json` | `df09c4e828ed03bb` |
| `videos/rukawa-natural-pace-20s-20261110/outputs/checks/generation-rim.json` | `cd6982b6c9983708` |
| `videos/rukawa-natural-pace-20s-20261110/outputs/checks/generation-shot.json` | `c152d657c1bcf4f0` |
| `videos/rukawa-natural-pace-20s-20261110/outputs/checks/media-metadata.json` | `fa1549a8b5852aa8` |
| `videos/rukawa-natural-pace-20s-20261110/outputs/checks/package-report.json` | `109bd2f3f21a5417` |
| `videos/rukawa-natural-pace-20s-20261110/outputs/checks/verification.json` | `b052d4f1f73c64e7` |
| `videos/rukawa-natural-pace-20s-20261110/outputs/checks/cadence/report.json` | `3644970207b36e72` |
| `videos/rukawa-natural-pace-20s-20261110/outputs/checks/clip-frames/crowd/source-frames.json` | `6fd1a296066c9d3f` |
| `videos/rukawa-natural-pace-20s-20261110/outputs/checks/clip-frames/defender/source-frames.json` | `4651f9097e5ff708` |
| `videos/rukawa-natural-pace-20s-20261110/outputs/checks/clip-frames/dribble/source-frames.json` | `b2af8eeeca34cce3` |
| `videos/rukawa-natural-pace-20s-20261110/outputs/checks/clip-frames/hero-native24-natural4/source-frames.json` | `d70ba00ec6bf74d8` |
| `videos/rukawa-natural-pace-20s-20261110/outputs/checks/final-frames/positions.json` | `02b62d5e25b1e449` |
| `videos/rukawa-natural-pace-20s-20261110/scripts/check.py` | `9ecc13b4201d32d6` |
| `videos/rukawa-natural-pace-20s-20261110/scripts/fetch-video-model.py` | `73d752c167e8831d` |
| `videos/rukawa-natural-pace-20s-20261110/scripts/final-audio.py` | `3a8276fb98642bf6` |
| `videos/rukawa-natural-pace-20s-20261110/scripts/finish-motion-clip.py` | `d10649dfdc0c96d5` |
| `videos/rukawa-natural-pace-20s-20261110/scripts/generate-clip.py` | `9d4b971903c5db2c` |
| `videos/rukawa-natural-pace-20s-20261110/scripts/generate-defender.py` | `8855594a98c8713e` |
| `videos/rukawa-natural-pace-20s-20261110/scripts/generate-natural-queue.py` | `ff3215b1760fe5f4` |
| `videos/rukawa-natural-pace-20s-20261110/scripts/lowram_prompt.py` | `f8267e06ce389727` |
| `videos/rukawa-natural-pace-20s-20261110/scripts/ltx-entry.py` | `6643f955c03c6588` |
| `videos/rukawa-natural-pace-20s-20261110/scripts/package.py` | `3d0a8022e9916261` |
| `videos/rukawa-natural-pace-20s-20261110/scripts/progress.py` | `30253b87cad4ab9b` |
| `videos/rukawa-natural-pace-20s-20261110/scripts/review-clip.py` | `b96034c458489886` |
| `videos/rukawa-natural-pace-20s-20261110/scripts/run.mjs` | `afb7862b87f7db57` |
| `videos/rukawa-natural-pace-20s-20261110/scripts/select-native-clips.py` | `71bd3316360b30d8` |
| `videos/rukawa-natural-pace-20s-20261110/scripts/setup-generator.py` | `197725e597c7794c` |
| `videos/rukawa-natural-pace-20s-20261110/scripts/sfx.py` | `1b284890293fb018` |
| `videos/rukawa-natural-pace-20s-20261110/src/BasketAction.tsx` | `96ce7d6c1fd9082d` |
| `videos/rukawa-natural-pace-20s-20261110/src/CelebrationScene.tsx` | `1d81a1af199f6659` |
| `videos/rukawa-natural-pace-20s-20261110/src/CrowdScene.tsx` | `288df7f6a0b6cc6d` |
| `videos/rukawa-natural-pace-20s-20261110/src/DefenderScene.tsx` | `16feb6d8ce469945` |
| `videos/rukawa-natural-pace-20s-20261110/src/DribbleScene.tsx` | `e683cdd6be8e5762` |
| `videos/rukawa-natural-pace-20s-20261110/src/HeroScene.tsx` | `508d5b1ba7fb7a0c` |
| `videos/rukawa-natural-pace-20s-20261110/src/OpeningScene.tsx` | `1343d8eb45e0b65e` |
| `videos/rukawa-natural-pace-20s-20261110/src/PendingFrame.tsx` | `21869e780cb6d791` |
| `videos/rukawa-natural-pace-20s-20261110/src/RimScene.tsx` | `165b044fcbb173c1` |
| `videos/rukawa-natural-pace-20s-20261110/src/Root.tsx` | `98bb35b10f9672c2` |
| `videos/rukawa-natural-pace-20s-20261110/src/ShotScene.tsx` | `f1013e0d7cc9030b` |
| `videos/rukawa-natural-pace-20s-20261110/src/font.ts` | `2779d73a080406f7` |
| `videos/rukawa-natural-pace-20s-20261110/src/index.ts` | `2e0e358282a1b998` |
| `videos/rukawa-natural-pace-20s-20261110/src/media-status.ts` | `8ca93c79d067e9ea` |
| `videos/rukawa-natural-pace-20s-20261110/src/timing.ts` | `92e7d7d2802096f4` |
