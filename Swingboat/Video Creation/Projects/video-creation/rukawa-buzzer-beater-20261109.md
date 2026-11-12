---
type: video-production-dossier
project: video-creation
video_id: rukawa-buzzer-beater-20261109
captured: '2026-11-12'
status: 已有 20 秒三维手办风格成片
source_fingerprint: 055ed1983f8ba0f433d7a0fcbae0db8ee6d08d7c5bb56dcaed7fa1569aac8475
tags:
- video-creation
- production-capture
---

# 流川枫三分绝杀 · 程序化三维手办路线

<!-- capture-source-sha256:055ed1983f8ba0f433d7a0fcbae0db8ee6d08d7c5bb56dcaed7fa1569aac8475 -->

本次为指定项目的材料回溯与归档。制作结论来自项目记录/源码，未找回关闭的制作对话，也未重新执行生成、渲染、音频合成或用户验收。

## 当前结论与制作路线

**状态：已有 20 秒三维手办风格成片。**

程序建模与双段 IK → Cycles/Metal → 合成声音 → Remotion。

1280×720、24fps、20 秒。建模、人物发型球衣、球场、看台和相机由 Python/bpy/mathutils 构造。先生成本地 picture.mp4，再叠加独立音轨、时钟和比分。

## 关键算法、实现与适用边界

motion.py 在米制空间计算跑动/后撤、起跳、抛物线及慢动作时间重映射；双段 IK 解肢体，动作烘焙为原生关键帧。记录出手 9.0 秒、蜂鸣 9.15 秒、入网 13.6 秒；渲染用 24 samples、固定噪声种子、降噪和轻微运动模糊。续渲通过场景指纹防止新旧帧混合。

## 声音与字幕

NumPy/SciPy 合成运球、鞋声、蜂鸣、入网、欢呼和音乐，无旁白；20 秒声音与画面事件对应。

## 失败、修正与验收边界

这是三维手办风格路线，不能据此认定满足后来要求的二维动漫造型。帧序列不进交付包，解包后检查可能跳过 PNG，但媒体/声音/路径检查仍可执行。

## 技术与模型版本

以下是本项目声明或锁定的依赖，安装清单本身不证明某项参与了最终画面。具体用途以制作路线、源码和生成记录交叉判断。

| 依赖 | package.json 声明 | 可解析的直接锁定版本 |
| --- | --- | --- |
| `@remotion/cli` | `4.0.541` | `4.0.541` |
| `@remotion/media` | `4.0.541` | `4.0.541` |
| `remotion` | `4.0.541` | `4.0.541` |
| `react` | `19.3.1` | `19.3.1` |
| `react-dom` | `19.3.1` | `19.3.1` |
| `typescript` | `5.9.3` | `5.9.3` |
| `@types/react` | `19.3.0` | `19.3.0` |
| `@types/react-dom` | `19.2.3` | `19.2.3` |

> [!info]- requirements.txt
> ```text
> numpy==2.5.4
> scipy==1.18.1
> pillow==13.0.0
> ```


工具底层模型未公开时按“未披露”记录；不能由 imagegen 工具名猜测图像模型。未提供真实推理日志的配置不记作已运行。

## 镜头、时间轴与采用参数

> [!info]- 配置：docs/timeline.json
> ```json
> {"fps":24,"duration":20,"frames":480,"width":1280,"height":720,"release":9.0,"buzzer":9.15,"swish":13.6,"score":13.85,"shots":[{"start":0,"end":3.2,"name":"arena"},{"start":3.2,"end":6.1,"name":"dribble"},{"start":6.1,"end":9.4,"name":"jump-shot"},{"start":9.4,"end":12.7,"name":"ball-flight"},{"start":12.7,"end":15.4,"name":"swish"},{"start":15.4,"end":20,"name":"winner"}]}
> ```

> [!info]- 当前 Composition / 时间轴源码：videos/rukawa-buzzer-beater-20261109/src/Root.tsx，L1–40
> ```tsx
> import React, {useEffect, useState} from 'react';
> import {AbsoluteFill, CanvasImage, Composition, continueRender, delayRender, cancelRender, interpolate, staticFile, useCurrentFrame, useVideoConfig} from 'remotion';
> import {Audio, Video} from '@remotion/media';
> 
> type FilmProps = {source: string; preview: boolean};
> const Film: React.FC<FilmProps> = ({source, preview}) => {
>  const [fontHandle]=useState(()=>delayRender('Loading project font'));
>  useEffect(()=>{
>   const face=new FontFace('FilmHan', `url(${staticFile('fonts/Arial-Unicode.ttf')})`);
>   face.load().then(font=>{document.fonts.add(font);continueRender(fontHandle);}).catch(cancelRender);
>  },[fontHandle]);
>  const frame=useCurrentFrame();const {fps}=useVideoConfig();const t=frame/fps;
>  const clock=t<5.2?5.2-t*.67:t<9.15?1.716*(1-(t-5.2)/3.95):0;
>  const won=t>=13.85;
>  return <AbsoluteFill style={{backgroundColor:'#090d12',color:'#f4f0e6',fontFamily:'Arial, FilmHan, "PingFang SC", "Microsoft YaHei", sans-serif'}}>
>   {preview?<CanvasImage src={staticFile(source)} style={{width:'100%',height:'100%'}} premountFor={fps}/>:<Video src={staticFile(source)} muted premountFor={fps} style={{width:'100%',height:'100%'}}/>}
>   <Audio src={staticFile('court-soundscape.wav')} premountFor={fps} volume={0.9}/>
>   <AbsoluteFill style={{background:'linear-gradient(180deg,rgba(3,7,13,.25),transparent 25%,transparent 75%,rgba(3,7,13,.38))'}}/>
>   <div style={{position:'absolute',left:0,right:0,top:0,height:30,background:'#080c11'}}/>
>   <div style={{position:'absolute',left:0,right:0,bottom:0,height:30,background:'#080c11'}}/>
>   <div style={{position:'absolute',left:46,top:49,display:'flex',height:58,boxShadow:'0 6px 30px #0005',fontWeight:700}}>
>    <div style={{background:'#9b1626',padding:'9px 18px',borderLeft:'3px solid #e5cda4'}}>
>     <div style={{fontSize:11,letterSpacing:2,opacity:.8}}>SHOHOKU</div><div style={{fontSize:20,letterSpacing:3}}>湘北</div>
>    </div>
>    <div style={{background:'#121b25ed',display:'flex',alignItems:'center',padding:'0 20px',gap:17,fontSize:29,fontVariantNumeric:'tabular-nums'}}><span style={{color:won?'#f4ce84':'#fff'}}>{won?'77':'74'}</span><span style={{fontSize:16,color:'#73818e'}}>:</span><span>76</span></div>
>    <div style={{background:'#24303be8',padding:'8px 14px',minWidth:78,textAlign:'center'}}><div style={{fontSize:10,letterSpacing:2,color:'#c2b7a3'}}>{won?'FINAL':'4TH'}</div><div style={{fontSize:23,color:clock===0?'#ff6a61':'#f4f0e6',fontVariantNumeric:'tabular-nums'}}>{clock.toFixed(1).padStart(4,'0')}</div></div>
>   </div>
>   <div style={{position:'absolute',right:47,top:53,fontSize:11,letterSpacing:3,color:'#e4d4b5',textShadow:'0 1px 5px #000'}}>THE LAST SHOT <span style={{marginLeft:12,color:'#b33d45'}}>●</span></div>
>   <div style={{position:'absolute',left:48,bottom:51,opacity:interpolate(frame,[0,.5*fps,2.7*fps,3.1*fps],[0,1,1,0],{extrapolateLeft:'clamp',extrapolateRight:'clamp'})}}>
>    <div style={{fontSize:11,letterSpacing:4,color:'#dfc08b',marginBottom:8}}>SLAM DUNK / FAN ANIMATION</div><div style={{fontSize:29,fontWeight:700,letterSpacing:4}}>最后一投</div>
>   </div>
>   {t>=6.1&&t<9.15?<div style={{position:'absolute',right:49,bottom:53,fontSize:13,letterSpacing:3,color:'#e3cfad'}}>11 / RUKAWA</div>:null}
>   {t>=15.4?<div style={{position:'absolute',left:50,bottom:56,opacity:interpolate(frame,[15.4*fps,16.2*fps],[0,1],{extrapolateLeft:'clamp',extrapolateRight:'clamp'}),translate:`0 ${interpolate(frame,[15.4*fps,16.2*fps],[15,0],{extrapolateLeft:'clamp',extrapolateRight:'clamp'})}px`}}>
>    <div style={{display:'flex',alignItems:'center',gap:12,marginBottom:10}}><div style={{height:2,width:34,background:'#d6b67e'}}/><span style={{fontSize:12,letterSpacing:4,color:'#ecd3a8'}}>THREE POINTS. ONE WIN.</span></div>
>    <div style={{fontSize:44,fontWeight:700,letterSpacing:8,textShadow:'0 2px 15px #0006'}}>流川枫</div>
>    <div style={{fontSize:15,letterSpacing:5,marginTop:10,color:'#ecdfca'}}>三分绝杀 · 湘北获胜</div>
>   </div>:null}
>  </AbsoluteFill>;
> };
> export const Root = () => <Composition id="RukawaFinalShot" component={Film} durationInFrames={480} fps={24} width={1280} height={720} defaultProps={{source:'picture.mp4',preview:false}}/>;
> ```

源时间轴、生成帧数、所用区间与最终时长分别记录；历史文档或候选参数不自动覆盖当前采用配置。

## 参考来源与借鉴说明

在本次盘点的项目自有材料中，未找到可明确归属本片的外部参考视频或 X 帖子链接；这不等于历史上没有参考。


## 重现入口与修改位置

以下是 package.json 中的实际命令映射，供在对应视频目录使用；本轮未执行这些制作命令。系统依赖、前置模型/素材和环境准备见后附 README。

| npm 入口 | 实际命令 |
| --- | --- |
| `npm run dev` | `node scripts/run.mjs dev` |
| `npm run scene` | `node scripts/run.mjs scene` |
| `npm run preview:scene` | `node scripts/run.mjs preview` |
| `npm run render:scene` | `node scripts/run.mjs frames` |
| `npm run audio` | `node scripts/run.mjs audio` |
| `npm run encode` | `node scripts/run.mjs encode` |
| `npm run render` | `node scripts/run.mjs render` |
| `npm run check` | `node scripts/run.mjs check` |
| `npm run build` | `node scripts/run.mjs bundle` |
| `npm run package` | `node scripts/run.mjs package` |

### 算法源码摘录

> [!info]- videos/rukawa-buzzer-beater-20261109/scripts/motion.py，L1–47
> ```python
> """Shared deterministic choreography. Metres; +Y is the attacking direction."""
> import math
> FPS=24
> RELEASE=9.0
> SWISH=13.6
> RIM=(0.,12.,3.05)
> def clamp(x):return min(1.,max(0.,x))
> def ease(x):x=clamp(x);return x*x*(3-2*x)
> def mix(a,b,u):return tuple(x+(y-x)*u for x,y in zip(a,b))
> def hero_root(t):
>     u=ease(t/5.1)
>     x=-2.4+1.2*u;y=1.8+3.0*u
>     if t>5.1:y-=.38*ease((t-5.1)/1.0)
>     bounce=.025*math.sin(t*9) if t<5.1 else 0
>     crouch=-.24*ease((t-6.3)/1.3)*(1-ease((t-7.6)/.7))
>     jump=.46*math.sin(math.pi*clamp((t-7.75)/2.65)) if 7.75<t<10.4 else 0
>     return x,y,bounce+crouch+jump
> RELEASE_POINT=(*hero_root(RELEASE)[:2],hero_root(RELEASE)[2]+2.34)
> def ball_at(t):
>     x,y,z=hero_root(t)
>     if t<4.8:
>         phase=t*1.25
>         h=.125+1.04*abs(math.cos(math.pi*phase))
>         return x+.47,y+.28,h
>     if t<7:
>         return mix((x+.47,y+.28,1.14+z),(x+.12,y+.35,1.37+z),ease((t-4.8)/2.2))
>     if t<RELEASE:
>         return mix((x+.12,y+.35,1.37+z),RELEASE_POINT,ease((t-7)/(RELEASE-7)))
>     if t<=SWISH:
>         u=(t-RELEASE)/(SWISH-RELEASE);d=1.55;px,py,pz=RELEASE_POINT
>         return px*(1-u),py+(12-py)*u,pz+(3.05-pz)*u+4.905*d*d*u*(1-u)
>     dt=(t-SWISH)*.55
>     impact=(-2.3+math.sqrt(2.3**2+4*4.905*(3.05-.125)))/(2*4.905)
>     if dt<impact:return (dt*.12,12+dt*.4,3.05-2.3*dt-4.905*dt*dt)
>     age=dt-impact
>     # Continuous damped bounces, with exact shared floor-contact endpoints.
>     period=.78;height=.62;elapsed=0.
>     while age>period and period>.08:
>         age-=period;elapsed+=period;period*=.66;height*=.4356
>     u=min(1,age/period)
>     return (.12*impact+(dt-impact)*.4,12+.4*impact-(dt-impact)*.3,.125+4*height*u*(1-u))
> 
> def game_clock(t):
>     # Actual play is slowed during gather, release and ball flight.
>     if t<5.2:return 5.2-t*.67
>     if t<9.15:return 1.716*(1-(t-5.2)/3.95)
>     return 0.
> ```

> [!info]- videos/rukawa-buzzer-beater-20261109/scripts/audio.py，L1–51
> ```python
> """Original 20 s synthesized court Foley, arena ambience and restrained musical pulse."""
> from pathlib import Path
> import numpy as np
> from scipy import signal
> from scipy.io import wavfile
> SR=48000;D=20;n=SR*D;t=np.arange(n)/SR;rng=np.random.default_rng(11)
> a=np.zeros((n,2))
> def noise(low,high,size=n):return signal.sosfilt(signal.butter(3,[low,high],btype='bandpass',fs=SR,output='sos'),rng.normal(size=size))
> def add(x,when,gain=1,pan=0):
>  start=int(when*SR);count=min(len(x),n-start)
>  if count>0:a[start:start+count,0]+=x[:count]*gain*(1-pan*.4);a[start:start+count,1]+=x[:count]*gain*(1+pan*.4)
> # Air and diffuse crowd, with a narrow hush during flight.
> bed=noise(180,2600);env=.020+.012*np.sin(t*.7)**2;env*=1-.66*np.clip((t-8.8)/.7,0,1)*(1-np.clip((t-13.6)/.25,0,1))
> a[:,0]+=bed*env;a[:,1]+=(.8*np.roll(bed,757)+.2*noise(400,4400))*env
> # Court dribbles match minima of |cos(pi*1.25*t)|.
> for when in np.arange(.4,5.15,.8):
>  u=np.arange(int(.32*SR))/SR
>  hit=np.sin(2*np.pi*(76*u-25*u*u))*np.exp(-u*24)+noise(400,1600,len(u))*.16*np.exp(-u*70)
>  add(hit,when,.28,-.25)
> for when in [.18,.69,1.12,1.62,2.1,2.55,3.02,3.46,4.0,4.48,5.16,5.63,6.1,7.65,10.42]:
>  u=np.arange(int(.16*SR))/SR
>  squeak=np.sin(2*np.pi*(1400*u+230*u*u))*np.sin(np.pi*np.minimum(1,u/.16))**2*np.exp(-u*13)
>  add(squeak,when,.055,.15);add(noise(70,350,len(u))*np.exp(-u*44),when,.18)
> # Low pulse, original synthetic score, fades under the shot.
> for when in np.arange(0,8.2,.75):
>  u=np.arange(int(.45*SR))/SR;add(np.sin(2*np.pi*(48*u-12*u*u))*np.exp(-u*13),when,.10)
> # Breathy release/flight and heartbeat suspense.
> u=np.arange(int(.35*SR))/SR;add(noise(1000,9000,len(u))*np.sin(np.pi*u/.35)**2,8.91,.065)
> for when in [9.9,11.3,12.4]:
>  u=np.arange(int(.4*SR))/SR;add(np.sin(2*np.pi*44*u)*np.exp(-u*17),when,.11)
> # Buzzer after release, before score; controlled relative to net swish.
> u=np.arange(int(.42*SR))/SR;buzz=signal.sawtooth(2*np.pi*380*u)*.45+np.sin(2*np.pi*760*u)*.2;add(buzz*np.minimum(1,u/.02)*np.minimum(1,(.42-u)/.08),9.15,.12)
> u=np.arange(int(.6*SR))/SR;add(noise(2200,12500,len(u))*np.exp(-u*9)*np.minimum(1,u/.025),13.60,.28)
> # Cheering is an original synthetic broad crowd with vowel-like resonances.
> u=np.arange(int(6.25*SR))/SR;raw=rng.normal(size=len(u));cheer=np.zeros_like(u)
> for fq in [420,670,970,1500]:cheer+=signal.lfilter(*signal.iirpeak(fq,3,fs=SR),raw)
> cheer*=np.minimum(1,u/.35)*(.8+.2*np.sin(u*5)**2);add(cheer,13.7,.50,-.25);add(np.roll(cheer,1703),13.7,.40,.3)
> # Scattered applause transients.
> for when in rng.uniform(14,19.8,65):
>  u=np.arange(int(.08*SR))/SR;add(noise(700,11000,len(u))*np.exp(-u*65),when,.08,rng.uniform(-1,1))
> # Bright concluding chord, made from simple sine partials.
> u=np.arange(int(5.9*SR))/SR
> for fq in [146.83,220,293.66,369.99,440]:
>  x=(np.sin(2*np.pi*fq*u)+.2*np.sin(4*np.pi*fq*u))*np.exp(-u*.5)*np.minimum(1,u/.04);add(x,14.05,.028)
> # Reverb taps inside the 20-second output.
> for delay,gain in [(.073,.12),(.149,.07),(.287,.04)]:
>  ds=int(delay*SR);a[ds:]+=a[:-ds]*gain
> fade=np.minimum(1,t/.06)*np.minimum(1,(20-t)/.45);a*=fade[:,None]
> a=np.tanh(a*1.3);a*=.88/max(.88,np.max(np.abs(a)))
> root=Path(__file__).resolve().parents[1];wavfile.write(root/'public/court-soundscape.wav',SR,(a*32767).astype(np.int16))
> print('AUDIO',a.shape,'peak',np.max(np.abs(a)))
> ```

## 完整关键提示词与生成记录

以下保留项目自有提示词、候选参数与版本。存在失败候选和历史参数；最终采用以 selected-takes、edit-decisions 与制作复盘为准，不能将所有候选都写成已采用。

> [!info]- videos/rukawa-buzzer-beater-20261109/docs/prototypes/rukawa-anime-prototype-v1-prompt.txt
> ```text
> Use case: stylized-concept.
> Asset type: a single character appearance prototype sheet for a planned 3D cel-shaded basketball animation.
> Primary request: faithfully depict Kaede Rukawa (流川枫 / 流川楓) from the classic 1990s anime Slam Dunk, closely preserving the original anime character design. This is an appearance concept, not a screenshot of an existing 3D model.
> Subject and identity: instantly recognizable Rukawa, Japanese teenage male basketball player, tall slim athletic build, long limbs and natural human anatomy, lean youthful face with a narrow jaw and chin, narrow cool blue-gray eyes, restrained confident expression. Accurate black softly layered hair with long uneven curtain-like bangs falling over the forehead and temples, subtle blue-black highlights, the familiar Rukawa silhouette. Hair is soft and gravity-following; no upright crown spikes.
> Uniform: classic red Shohoku basketball sleeveless jersey and matching red shorts, black and white trim, exact chest lettering "SHOHOKU", exact jersey number "11", black lettering and numerals with a fine pale outline. White sports socks and white basketball high-tops with black details and restrained red accents. Match the classic anime uniform.
> Composition: one polished landscape design sheet on a quiet warm light-gray background with faint basketball-court line hints. A large, full-body three-quarter standing view occupies the left half, entire head and shoes visible with generous margins. The right upper quadrant contains a larger close-up of the SAME character's face for checking resemblance; the right lower quadrant shows the SAME character in a graceful, anatomically credible jump-shot pose holding an orange basketball, wrists and fingers natural. These are three views of one consistent character, not separate people. Clean professional spacing, minimal decoration. No crowd or opponents. No labels needed beyond uniform lettering.
> Style and medium: high quality anime-accurate 3D cel-shaded character concept, very close to traditional Slam Dunk animation drawing in facial design, crisp controlled ink outlines, 2 to 3 broad shadow tones, matte skin and cloth, subtle fabric folds. Convincing continuous human surfaces, smoothly integrated shoulders, elbows and knees, fitted but naturally hanging jersey and shorts, believable hands and fingers. Preserve the original anime proportions and design even though the image is conceived for later 3D modeling.
> Avoid: toy or puppet appearance, disconnected cylindrical limbs, ball-joint doll anatomy, plastic sheen, chibi proportions, Western adult scanned face, generic musclebound body, exaggerated spiky hair, photorealistic skin pores, unrelated character identity, extra logos, extra text, watermarks, cropped feet or hair, malformed hands.
> ```

## 已有产物与检查依据

文件存在、技术检查、画面检查与用户验收分别看待。下表是本次文件盘点，旧版与当前版可能共存。MP4/HTML 按内容计算 SHA-256，ZIP 仅记录文件大小和修改时间供增量判断。

| 本地产物 | 字节数 | SHA-256 |
| --- | ---: | --- |
| `videos/rukawa-buzzer-beater-20261109/outputs/rukawa-final-shot-20s.mp4` | 15482061 | `9c9f19b4d62cc57b6bb3007683953679df324b2fb7a7887bc28ee66800fe84c2` |
| `videos/rukawa-buzzer-beater-20261109/outputs/rukawa-project.zip` | 52411245 | `ZIP：仅元数据` |

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
>       "duration": "20.011000",
>       "nb_frames": "940"
>     }
>   ],
>   "format": {
>     "duration": "20.011000",
>     "size": "15482061"
>   },
>   "file": "outputs/rukawa-final-shot-20s.mp4",
>   "scope": "本次只读取媒体头；未全片解码、播放或重新进行视觉/听觉验收。"
> }
> ```


已有检查报告路径（未在本轮重新执行）：

- `videos/rukawa-buzzer-beater-20261109/outputs/checks/audio-audit.json`
- `videos/rukawa-buzzer-beater-20261109/outputs/checks/final-check.json`
- `videos/rukawa-buzzer-beater-20261109/outputs/checks/render-cpu-partial.json`
- `videos/rukawa-buzzer-beater-20261109/outputs/checks/render-final-shard0.json`
- `videos/rukawa-buzzer-beater-20261109/outputs/checks/render-final.json`
- `videos/rukawa-buzzer-beater-20261109/outputs/checks/render-gpu-partial.json`
- `videos/rukawa-buzzer-beater-20261109/outputs/checks/render-preview.json`
- `videos/rukawa-buzzer-beater-20261109/outputs/checks/scene-audit.json`

## 制作方法与迭代记录选编

下面是项目自有文档的归档摘录，保留原阶段与原检查表述，个人路径已泛化、相对链接已注明源项目位置。它们属于来源陈述，不代表本次重新完成其中的生成或验收。

> [!info]- 来源文档：videos/rukawa-buzzer-beater-20261109/README.md
> # 流川枫 三分绝杀
> 
> 《灌篮高手》背景的程序化三维手办风格同人动画，20 秒 / 24 fps / 1280×720。流川枫身穿湘北 11 号球衣，运球后撤、三分线外跳投，在终场蜂鸣前出手，空心入网，以 77∶76 反超获胜。
> 
> 沿用《瓶中风暴》制作会话的技术：Python + bpy / mathutils 制作模型、IK 动作及镜头；Blender Cycles + Metal 逐帧渲染；NumPy / SciPy 合成完整声音；FFmpeg 编码；Remotion / React / TypeScript 合成导出。
> 
> ## 独立安装
> 
> 需要 Node 24、Python 3.13、Blender 5.2、FFmpeg 和系统 Chrome。所有命令从本目录执行：
> 
> ```bash
> mkdir -p .cache/tmp .cache/npm .cache/python
> export TMPDIR="$PWD/.cache/tmp" TMP="$PWD/.cache/tmp" TEMP="$PWD/.cache/tmp"
> export PIP_CACHE_DIR="$PWD/.cache/python"
> npm ci
> python3 -m venv .venv
> .venv/bin/python -m pip install -r requirements.txt
> ```
> 
> ## 预览、检查、导出
> 
> ```bash
> npm run dev
> npm run check
> npm run render
> ```
> 
> Studio 默认 http://localhost:3088/RukawaFinalShot 。选择 RukawaFinalShot，可编辑画面、音轨和比分信息。成片为 outputs/rukawa-final-shot-20s.mp4；可编辑 Blender 工程为 public/rukawa.blend；工程包为 outputs/rukawa-project.zip。已有 public/picture.mp4 时无需重渲三维，即可预览及导出。
> 
> `npm run check` 检查 480 张帧图、出手位置、飞行连续性、入网终点、20 秒音轨、每秒声音能量、最终媒体参数与工程路径隔离。帧序列属于可重建产物，不放进工程压缩包；如只解压工程包且未重渲三维，检查会明确跳过未包含的逐帧 PNG；媒体、声音和路径检查仍可运行。执行重建步骤后可再次检查帧完整性。成片媒体参数也可用 `ffprobe outputs/rukawa-final-shot-20s.mp4` 独立读取。
> 
> ## 重建或修改
> 
> ```bash
> npm run scene
> npm run preview:scene
> npm run render:scene
> npm run encode
> npm run audio
> npm run render
> npm run check
> npm run package
> ```
> 
> - scripts/build_scene.py：人物轮廓、发型、球衣、球场、灯光、看台、镜头，以及双段 IK 和烘焙。
> - scripts/motion.py：米制坐标、跑动与后撤路径、篮球轨迹、起跳、出手和慢动作时间重映射。出手为 9.0 秒、蜂鸣为 9.15 秒、入网为 13.6 秒。
> - scripts/audio.py：运球、脚步、鞋底摩擦、蜂鸣、篮网、欢呼及合成音乐。无旁白；如需旁白，将录音放进 public/ 并在 src/Root.tsx 单独添加 Audio 节点。
> - public/fonts/Arial-Unicode.ttf：片中中文图层使用的工程内字体，由 FontFace 加载并等待就绪后导出。
> - public/fonts/Arial-Bold.ttf：从系统复制的工程内字体，用于 Blender 球衣、标牌及比分。
> - src/Root.tsx：画面和音轨各自独立，比分、时钟、片尾身份信息可修改。替换画面用本目录 public/ 中的文件，并修改 Composition 的 source。
> - scripts/render_scene.py：24 samples、固定噪声种子、降噪、轻微运动模糊。支持断点续渲；如显存紧张，可在命令前设置 `RUKAWA_CPU=1` 使用 Cycles CPU 继续。支持场景指纹检查，场景指纹改变后拒绝混用旧帧。修改场景后先把 outputs/frames 和 render-final.json 移到本目录中的版本文件夹，再重渲。
> 
> 时长与帧率修改时，同步 motion.py、build_scene.py、Root.tsx、audio.py、run.mjs 的帧数和编码参数，然后重新生成画面和声音。详细模型关联和分镜见 docs/production-plan.md；场景清单见 docs/scene-inventory.json。
> 
> ## 路径与打包
> 
> 本目录声明自己的 npm / Python 依赖和锁文件，所有受控缓存、临时文件、Blender 配置、素材、帧图、日志、视频和压缩包均放在本目录。没有符号链接或跨视频运行依赖。run.mjs 固定工作目录并设置 TMPDIR/TMP/TEMP、npm/pip 缓存、Blender 用户配置和 XDG 缓存。
> 
> 系统安装的 Blender、FFmpeg、Chrome 属于系统工具；macOS Metal 驱动系统缓存以及已安装浏览器自身缓存不能完全由项目控制。脚本设置 CYCLES_METAL_DISABLE_BINARY_ARCHIVES=1，沿用既有作品的 Metal 编译归档兼容处理。
> 
> 工程包包含当前工程源码、锁文件、已生成画面和声音、Blender 场景、文档、检查报告和成片；排除 node_modules、.venv、缓存、逐帧 PNG、旧 .blend1 和压缩包自身。重新解压后可独立 npm ci、预览、导出；更改三维动作才需 Blender 重建。

> [!info]- 来源文档：videos/rukawa-buzzer-beater-20261109/docs/production-plan.md
> # 流川枫 三分绝杀 制作记录
> 
> 20 秒，24 fps，1280×720，480 帧。以《灌篮高手》中的湘北 11 号流川枫为主角的独立三维手办风格同人短片，球场和人物均由本工程程序生成。
> 
> ## 参考会话 review
> 
> 已读取本项目会话《瓶中风暴》制作（01a1af7b-a50b-7303-99c7-7c489b197a58）的实际历史，回溯到最早的原帖分析与初版重建，核对后续小兔动作、鱼群优化、制作技术总结、模型和场景关系、关键帧动画讲解及详细制作手册整理记录；并对照既有作品的 README、package.json、run.mjs、render_scene.py、production-method.md。
> 
> 可复用的方法是 Python/bpy 建模、mathutils 计算关节、原生关键帧烘焙、Cycles/Metal 渲染、NumPy/SciPy 合成声音、FFmpeg 编码、Remotion/React 合成。原作品发现过 20 秒画面只配 14 秒 WAV，本片从一开始生成完整 20 秒立体声音轨，检查时逐秒验证。
> 
> 本项目重新编写场景、动作、声音和工具脚本。系统字体复制至本项目；没有引用既有视频文件。
> 
> ## 镜头
> 
> - 0–3.2 秒：室内球馆全景、湘北落后两分，流川枫推进。
> - 3.2–6.1 秒：低机位近景，运球、摆脱、后撤到三分线外。
> - 6.1–9.4 秒：慢动作蓄力、起跳、9.0 秒右手出球，9.15 秒计时归零。
> - 9.4–12.7 秒：跟随篮球旋转与抛物线飞行。
> - 12.7–15.4 秒：篮筐近景，13.6 秒空心入网、篮网拉伸回弹，13.85 秒记分。
> - 15.4–20 秒：流川枫沉静收势，队友举手，77∶76 胜利信息。
> 
> 慢镜时间与比赛时钟分离。持球轨迹和双手目标共用 motion.ball_at；出手后篮球沿受重力约束的抛物线飞行，用时间重映射放慢。物理飞行时间 1.55 秒映射为片中 4.6 秒，起点和篮圈终点连续。游戏时钟在出手 0.15 秒后归零。三分位置以篮筐地面投影为圆心计算，保证出手点位于 6.75 米线外。
> 
> ## 模型关联
> 
> 每位角色都有独立根控制，头部、手腕、脚踝和四肢独立烘焙。球衣是截面网格，头部是下颌收尖的脸部网格，黑发为冠部及尖发束。双段 IK 从肩/髋和手/脚目标求肘/膝；篮球为独立世界轨迹，其球皮和弯曲接缝共享一个父控制。篮球不是在出手时替换的第二颗球。
> 
> 篮筐、篮板、三分线共享同一米制场景坐标；篮网各股控制点在入网后产生衰减摆动，写成原生关键帧。相机位置、朝向和焦距均烘焙，每一剪辑点采用逐帧线性插值。球场为枫木板、油漆区、场线、玻璃篮板、金属支架、分层看台、观众与软箱光。
> 
> ## 声音
> 
> 原创程序合成 48 kHz 双声道运球撞击、鞋底摩擦、出手气流、终场蜂鸣、篮网摩擦、观众欢呼和鼓掌；辅以简单合成脉冲与终场和弦。无旁白。音效事件与动画共用时间约定，结尾仍有完整声音与淡出。
> 
> ## 正式渲染与恢复记录
> 
> 正式画面为 Cycles 24 samples、固定采样种子、降噪和轻微运动模糊。GPU 渲染进程在第 376 帧后退出，未产生 Blender 崩溃报告；保留了完整帧，通过 PNG 尾标记与场景指纹续渲。部分收尾帧使用 Cycles CPU，之后恢复 Metal GPU，并将降噪切到 CPU 以降低 GPU 占用。渲染器会跳过完整的现有帧，不混入未完成的 PNG。
> 
> 最终检查以完整的 480 张图片和导出的媒体文件为准；分阶段日志与 render-gpu-partial.json、render-cpu-partial.json 供排查恢复过程使用。设备切换不改变分辨率、帧率、动画、材质、采样数或时间轴。不同设备的路径追踪噪声可能略有差异，均经过降噪。

## 证据清单与缺失信息

盘点的文本/报告文件 36 个，源文本、报告及媒体产物身份摘要为 `055ed1983f8ba0f433d7a0fcbae0db8ee6d08d7c5bb56dcaed7fa1569aac8475`。哈希证明采集内容身份，不代表其中每个断言已验证。模型权重、缓存、依赖、第三方源码树和未指定的历史会话未采集；大型文本仅做哈希并按需要选读。

- 本次整理基于明确引用的 README、制作方法、配置、提示词、关键源码与已有报告。未重新观看全部参考/候选/成片，也未做人工听辨。
- 项目未提供的原始模型内部实现、未披露生成后端、未记录 seed/参数、真实费用、人工总工时和用户最终验收，均不补造。实际日志有记录的耗时见上文，流水线耗时不等于整个制作总耗时。
- 源项目内的路径只用于查证；本档案未复制大型素材或模型，也不改变源项目运行依赖。

| 已盘点证据文件 | SHA-256（前 16 位）/状态 |
| --- | --- |
| `videos/rukawa-buzzer-beater-20261109/README.md` | `dd2b6e23e7d09822` |
| `videos/rukawa-buzzer-beater-20261109/package-lock.json` | `cc051265e9d6c896` |
| `videos/rukawa-buzzer-beater-20261109/package.json` | `6d94fd82d93b531d` |
| `videos/rukawa-buzzer-beater-20261109/remotion.config.ts` | `6353a1053a703c9e` |
| `videos/rukawa-buzzer-beater-20261109/requirements.txt` | `4fda4e054773f608` |
| `videos/rukawa-buzzer-beater-20261109/tsconfig.json` | `af262af0dd89b1e2` |
| `videos/rukawa-buzzer-beater-20261109/docs/production-plan.md` | `ab23b49bfba53e86` |
| `videos/rukawa-buzzer-beater-20261109/docs/scene-inventory.json` | `cea4257cd50b3618` |
| `videos/rukawa-buzzer-beater-20261109/docs/timeline.json` | `a66dd9fb4bfda130` |
| `videos/rukawa-buzzer-beater-20261109/docs/trajectory.json` | `003256ab31889da1` |
| `videos/rukawa-buzzer-beater-20261109/docs/prototypes/rukawa-anime-prototype-v1-prompt.txt` | `9dce84b069d468dd` |
| `videos/rukawa-buzzer-beater-20261109/docs/prototypes/rukawa-anime-prototype-v1.json` | `5bd6588e5474ec79` |
| `videos/rukawa-buzzer-beater-20261109/outputs/checks/audio-audit.json` | `bdb21e736f4ff8a4` |
| `videos/rukawa-buzzer-beater-20261109/outputs/checks/final-check.json` | `49a90ca44bedb176` |
| `videos/rukawa-buzzer-beater-20261109/outputs/checks/render-cpu-partial.json` | `1fa9c89dcbc5954a` |
| `videos/rukawa-buzzer-beater-20261109/outputs/checks/render-final-shard0.json` | `a0995ffe4e8b5711` |
| `videos/rukawa-buzzer-beater-20261109/outputs/checks/render-final.json` | `4e6d0906b42e4895` |
| `videos/rukawa-buzzer-beater-20261109/outputs/checks/render-gpu-partial.json` | `969bbd9210d9f73f` |
| `videos/rukawa-buzzer-beater-20261109/outputs/checks/render-preview.json` | `a772c75020f3a6c8` |
| `videos/rukawa-buzzer-beater-20261109/outputs/checks/scene-audit.json` | `f24ab1df6340a716` |
| `videos/rukawa-buzzer-beater-20261109/scripts/audio.py` | `a8485e9aefced9fd` |
| `videos/rukawa-buzzer-beater-20261109/scripts/audit_scene.py` | `deaa3d0d6aedf064` |
| `videos/rukawa-buzzer-beater-20261109/scripts/blocking.py` | `5e0eaf0a2aea2f8d` |
| `videos/rukawa-buzzer-beater-20261109/scripts/build_scene.py` | `2a4a8ba2e076ff69` |
| `videos/rukawa-buzzer-beater-20261109/scripts/check.py` | `7b81ee0f84a37fed` |
| `videos/rukawa-buzzer-beater-20261109/scripts/finish.py` | `4ab6dad397a0ff3a` |
| `videos/rukawa-buzzer-beater-20261109/scripts/finish_when_ready.py` | `1218a711f1d97639` |
| `videos/rukawa-buzzer-beater-20261109/scripts/fix_camera.py` | `6820bbb8df2c10b2` |
| `videos/rukawa-buzzer-beater-20261109/scripts/motion.py` | `e816f4bd3eeb8962` |
| `videos/rukawa-buzzer-beater-20261109/scripts/package.py` | `5e8f1ac473fce59a` |
| `videos/rukawa-buzzer-beater-20261109/scripts/render_scene.py` | `e3765e0bfdf25de9` |
| `videos/rukawa-buzzer-beater-20261109/scripts/revise_scene.py` | `95141d53c6b7e34f` |
| `videos/rukawa-buzzer-beater-20261109/scripts/run.mjs` | `99b2278e6612f3b4` |
| `videos/rukawa-buzzer-beater-20261109/scripts/update_ball.py` | `7e2f5a20226b7e68` |
| `videos/rukawa-buzzer-beater-20261109/src/Root.tsx` | `0f31aa6fcb870b7d` |
| `videos/rukawa-buzzer-beater-20261109/src/index.ts` | `2e0e358282a1b998` |
