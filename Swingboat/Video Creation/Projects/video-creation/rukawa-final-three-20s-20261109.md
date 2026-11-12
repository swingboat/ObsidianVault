---
type: video-production-dossier
project: video-creation
video_id: rukawa-final-three-20s-20261109
captured: '2026-11-12'
status: 20 秒文件存在；视觉协调与动作质量被用户指出不足
source_fingerprint: 4832714e0737e343c157691bf321dcac7e6af331eb8ccb81ffdf27f0f0012140
tags:
- video-creation
- production-capture
---

# 流川枫 · 20 秒有限动画试作与路线修正

<!-- capture-source-sha256:4832714e0737e343c157691bf321dcac7e6af331eb8ccb81ffdf27f0f0012140 -->

本次为指定项目的材料回溯与归档。制作结论来自项目记录/源码，未找回关闭的制作对话，也未重新执行生成、渲染、音频合成或用户验收。

## 当前结论与制作路线

**状态：20 秒文件存在；视觉协调与动作质量被用户指出不足。**

完整人物原画 → 有限运动与分镜 → 单球轨迹/程序球网 → 合成。

1280×720、30fps、600 帧，七段独立镜头。主角以完整原画呈现，身体大体刚性，手腕和配角微动。当前完整文件与后续单张风格修正图属于不同阶段。

## 关键算法、实现与适用边界

math.ts 管理关键时间：7.55 秒离手、7.9 秒蜂鸣、11.55 秒球心到篮圈、11.75 秒比分更新；drawScenes.ts 绘制角色与球网，镜头切换连接动作。原画尺度、取景与独立图层决定遮挡，需要按画面校准。

## 声音与字幕

程序合成 20 秒立体声环境、鞋声、蜂鸣和入网声；没有旁白或外部音乐采样。

## 失败、修正与验收边界

2026-11-10 评估指出主角、配角、背景画风/比例/透视不协调，刚性贴图缺少起跳到出手的连续过程。后续生成同框修正图仅验证构图和风格，没有生成新的 20 秒成片，不能据此宣称修复完成。

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

工具底层模型未公开时按“未披露”记录；不能由 imagegen 工具名猜测图像模型。未提供真实推理日志的配置不记作已运行。

## 镜头、时间轴与采用参数

> [!info]- 当前 Composition / 时间轴源码：videos/rukawa-final-three-20s-20261109/src/Root.tsx，L1–54
> ```tsx
> import React,{useEffect,useState} from 'react';
> import {AbsoluteFill,Composition,Folder,Series,continueRender,delayRender,interpolate,staticFile,useCurrentFrame,useVideoConfig} from 'remotion';
> import {Audio} from '@remotion/media';
> import {OpeningScene} from './scenes/OpeningScene';import {GatherScene} from './scenes/GatherScene';import {HandsScene} from './scenes/HandsScene';import {ReleaseScene} from './scenes/ReleaseScene';import {RimScene} from './scenes/RimScene';import {ReactionScene} from './scenes/ReactionScene';import {HeroScene} from './scenes/HeroScene';
> import {SCORE_GLOBAL,BUZZER} from './math';
> const Scoreboard:React.FC=()=>{
>  const frame=useCurrentFrame(),{fps}=useVideoConfig(),t=frame/fps,won=t>=SCORE_GLOBAL;
>  const remaining=Math.max(0,2.4*(1-t/BUZZER)),clock='00:0'+(Math.ceil(remaining*10)/10).toFixed(1);
>  return <div style={{position:'absolute',left:30,top:26,display:'flex',alignItems:'center',gap:18,padding:'12px 18px',backgroundColor:'#071d27e8',color:'#f3eddd',borderBottom:'2px solid #ab252d',boxShadow:'0 3px 12px #071c2830'}}>
>   <div><div style={{fontSize:14,letterSpacing:3,color:'#c0cbd0',marginBottom:4}}>SHOHOKU</div><div style={{fontSize:22}}>湘北 <span style={{fontFamily:'Arial,sans-serif',fontSize:35,fontWeight:700,color:won?'#f0c677':'#fff',marginLeft:8}}>{won?80:77}</span></div></div>
>   <div style={{height:43,width:1,backgroundColor:'#c0cbd040'}}/>
>   <div><div style={{fontSize:14,letterSpacing:3,color:'#c0cbd0',marginBottom:4}}>RYONAN</div><div style={{fontSize:22}}>陵南 <span style={{fontFamily:'Arial,sans-serif',fontSize:35,fontWeight:700,color:'#fff',marginLeft:8}}>78</span></div></div>
>   <div style={{paddingLeft:12,borderLeft:'1px solid #c0cbd040',textAlign:'right'}}><div style={{fontSize:13,color:'#bcc6cb',marginBottom:5}}>{t>=BUZZER?'终场':'决胜时刻'}</div><div style={{fontFamily:'Arial,sans-serif',fontVariantNumeric:'tabular-nums',fontSize:23,color:'#f0c677'}}>{clock}</div></div>
>  </div>;
> };
> export const Film:React.FC=()=>{
>  const frame=useCurrentFrame(),{fps}=useVideoConfig(),t=frame/fps,[fontHandle]=useState(()=>delayRender('Loading local Chinese type'));
>  useEffect(()=>{const f=new FontFace('NotoArena',`url(${staticFile('fonts/NotoSansSC.ttf')})`);f.load().then(x=>{document.fonts.add(x);continueRender(fontHandle);}).catch(()=>continueRender(fontHandle));},[fontHandle]);
>  return <AbsoluteFill style={{backgroundColor:'#112932',fontFamily:'NotoArena,sans-serif'}}>
>   <Series>
>    <Series.Sequence name="终场前 · 全场" durationInFrames={90} premountFor={fps}><OpeningScene/></Series.Sequence>
>    <Series.Sequence name="持球蓄力" durationInFrames={36} premountFor={fps}><GatherScene/></Series.Sequence>
>    <Series.Sequence name="最后一投 · 手部特写" durationInFrames={54} premountFor={fps}><HandsScene/></Series.Sequence>
>    <Series.Sequence name="起跳与出手" durationInFrames={120} premountFor={fps}><ReleaseScene/></Series.Sequence>
>    <Series.Sequence name="篮球入网" durationInFrames={120} premountFor={fps}><RimScene/></Series.Sequence>
>    <Series.Sequence name="看台与队友反应" durationInFrames={90} premountFor={fps}><ReactionScene/></Series.Sequence>
>    <Series.Sequence name="终场 · 流川枫" durationInFrames={90} premountFor={fps}><HeroScene/></Series.Sequence>
>   </Series>
>   <Audio name="Arena sound" src={staticFile('audio/arena.wav')} premountFor={fps} volume={.9}/>
>   <AbsoluteFill style={{pointerEvents:'none',background:'radial-gradient(ellipse at 55% 45%, transparent 35%, #061b2738 100%)'}}/>
>   {t>=17?<AbsoluteFill style={{background:'linear-gradient(90deg,#071b27ed 0%,#071b2790 38%,transparent 68%)'}}/>:null}
>   <Scoreboard/>
>   {t<2.7?<div style={{position:'absolute',left:45,bottom:66,color:'#fff6e3',textShadow:'0 2px 8px #071b27',opacity:interpolate(frame,[0,12,65,81],[0,1,1,0],{extrapolateLeft:'clamp',extrapolateRight:'clamp'})}}><div style={{fontSize:18,letterSpacing:5,marginBottom:10}}>终场前</div><div style={{fontSize:38,letterSpacing:3}}>落后一分。</div></div>:null}
>   {t>=4.2&&t<6?<div style={{position:'absolute',right:42,bottom:46,color:'#f9eed8',fontSize:23,letterSpacing:7,textShadow:'0 2px 8px #172b36'}}>最后一投</div>:null}
>   {t>=SCORE_GLOBAL&&t<12.55?<div style={{position:'absolute',right:74,top:48,color:'#fff3ca',fontFamily:'Arial,sans-serif',fontWeight:700,fontSize:58,scale:interpolate(frame,[SCORE_GLOBAL*fps,(SCORE_GLOBAL+.18)*fps,(SCORE_GLOBAL+.8)*fps],[1.25,1,1],{extrapolateLeft:'clamp',extrapolateRight:'clamp'}),textShadow:'0 3px 14px #0a2638'}}>+3</div>:null}
>   {t>=17?<div style={{position:'absolute',left:48,top:270,color:'#f6edda',opacity:interpolate(frame,[17*fps,17.4*fps],[0,1],{extrapolateLeft:'clamp',extrapolateRight:'clamp'})}}>
>    <div style={{fontSize:18,letterSpacing:6,color:'#d2b77d',marginBottom:16}}>SHOHOKU  ·  NO.11</div><div style={{fontSize:60,letterSpacing:8,marginBottom:12}}>流川枫</div>
>    <div style={{width:62,height:3,backgroundColor:'#b72832',margin:'16px 0 23px'}}/>
>    <div style={{fontSize:29,letterSpacing:4,marginBottom:12}}>压哨三分</div><div style={{fontSize:22,color:'#d5dde0',letterSpacing:3}}>80 : 78　湘北获胜</div>
>   </div>:null}
>  </AbsoluteFill>;
> };
> export const Root:React.FC=()=> <>
>  <Composition id="RukawaFinalThree" component={Film} width={1280} height={720} fps={30} durationInFrames={600}/>
>  <Folder name="Scenes">
>   <Composition id="Opening" component={OpeningScene} width={1280} height={720} fps={30} durationInFrames={90}/>
>   <Composition id="Gather" component={GatherScene} width={1280} height={720} fps={30} durationInFrames={36}/>
>   <Composition id="Hands" component={HandsScene} width={1280} height={720} fps={30} durationInFrames={54}/>
>   <Composition id="Release" component={ReleaseScene} width={1280} height={720} fps={30} durationInFrames={120}/>
>   <Composition id="Rim" component={RimScene} width={1280} height={720} fps={30} durationInFrames={120}/>
>   <Composition id="Reaction" component={ReactionScene} width={1280} height={720} fps={30} durationInFrames={90}/>
>   <Composition id="Hero" component={HeroScene} width={1280} height={720} fps={30} durationInFrames={90}/>
>  </Folder>
> </>;
> ```

源时间轴、生成帧数、所用区间与最终时长分别记录；历史文档或候选参数不自动覆盖当前采用配置。

## 参考来源与借鉴说明

在本次盘点的项目自有材料中，未找到可明确归属本片的外部参考视频或 X 帖子链接；这不等于历史上没有参考。

本地造型/参考文件（本轮仅核对存在，不宣称重新看过全部图像）：

- `videos/rukawa-final-three-20s-20261109/public/references/rukawa-approved.png`

### 项目记录的技术、内容或素材来源

这些 URL 来自项目文件，尚不自动证明本片采用；除本节明确标注的访问外，本轮未逐一在线重读。

| 链接 | 记录文件 |
| --- | --- |
| [www.adobe.com](http://www.adobe.com/) | `videos/rukawa-final-three-20s-20261109/docs/NOTO-OFL.txt` |
| [scripts.sil.org](https://scripts.sil.org/OFL) | `videos/rukawa-final-three-20s-20261109/docs/NOTO-OFL.txt` |

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

> [!info]- videos/rukawa-final-three-20s-20261109/src/math.ts，L1–33
> ```typescript
> export type Point={x:number;y:number};
> export const p=(x:number,y:number):Point=>({x,y});
> export const add=(a:Point,b:Point)=>p(a.x+b.x,a.y+b.y);
> export const sub=(a:Point,b:Point)=>p(a.x-b.x,a.y-b.y);
> export const mul=(a:Point,s:number)=>p(a.x*s,a.y*s);
> export const rot=(a:Point,t:number)=>p(a.x*Math.cos(t)-a.y*Math.sin(t),a.x*Math.sin(t)+a.y*Math.cos(t));
> export const mix=(a:Point,b:Point,u:number)=>add(mul(a,1-u),mul(b,u));
> export const clamp=(u:number)=>Math.min(1,Math.max(0,u));
> export const sm=(a:number,b:number,t:number)=>{const u=clamp((t-a)/(b-a));return u*u*u*(10+u*(-15+6*u));};
> export const RELEASE_LOCAL=1.55,RELEASE_GLOBAL=7.55,BUZZER=7.9,SWISH_LOCAL=1.55,SWISH_GLOBAL=11.55,SCORE_GLOBAL=11.75;
> export const jump=(t:number)=>140*Math.sin(Math.PI*clamp(t/4.4));
> export function releaseLocalPoint(src:Point,t:number){
>  const angle=.35*(1-sm(1.20,1.85,t)),w=(1-sm(100,160,src.y))*(1-sm(410,455,src.x));
>  const a=add(p(366,99),rot(sub(src,p(366,99)),angle));
>  return mix(src,a,w);
> }
> export const shotOrigin=(t:number)=>p(700-13*sm(.1,2.2,t),70-.05*jump(t));
> export const releaseWorld=(src:Point,t:number)=>add(shotOrigin(t),mul(releaseLocalPoint(src,t),.37));
> export function shotBall(t:number){
>  if(t<=RELEASE_LOCAL)return releaseWorld(p(263,-40),t);
>  const b=releaseWorld(p(263,-40),RELEASE_LOCAL),u=t-RELEASE_LOCAL;
>  return add(b,p(-320*u,-125*u+110*u*u));
> }
> export function rimBall(t:number){
>  const u=t/SWISH_LOCAL,a=p(1250,-170),b=p(1010,20),c=p(734,304);
>  if(u<=1){return add(add(mul(a,(1-u)**2),mul(b,2*u*(1-u))),mul(c,u*u));}
>  const d=t-SWISH_LOCAL;return p(734-35*(1-Math.exp(-5*d)),304+370*d+310*d*d);
> }
> export const netPulse=(t:number)=>{const age=t-SWISH_LOCAL;return age<0?0:Math.exp(-3.8*age)*Math.sin(12*age);};
> export function gatherLocal(src:Point,t:number){
>  const load=sm(.12,.65,t)*(1-.12*sm(.9,1.5,t)),v=rot(sub(src,p(550,900)),-.006*load),rigid=add(add(v,p(550,900)),p(-1.4*load,14.2*load));
>  return mix(src,rigid,1-sm(900,1290,src.y));
> }
> ```

## 完整关键提示词与生成记录

以下保留项目自有提示词、候选参数与版本。存在失败候选和历史参数；最终采用以 selected-takes、edit-decisions 与制作复盘为准，不能将所有候选都写成已采用。

> [!info]- videos/rukawa-final-three-20s-20261109/docs/new-asset-prompts.json
> ```json
> {
>   "rukawa_release": "Use case: illustration-story. Asset type: ONE complete full-body transparent 2D anime cutout for a slow-motion basketball jump-shot scene.\nInput 1 is the APPROVED full-body Rukawa appearance and proportions. Input 2 is the approved character design sheet. Preserve exactly the same handsome lean teenage Kaede Rukawa, black straight fringe, sharp calm face, natural long limbs, loose red SHOHOKU jersey number 11, matching red shorts, white socks and white/black sneakers. Keep the left-wrist black band as in reference. Match the clean thin ink and restrained warm cel colors.\nRedraw the entire person as one anatomically correct connected figure in a beautiful AIRBORNE jump-shot release pose, three-quarter FRONT/side view, shooting direction towards SCREEN LEFT. Both feet are off the ground, knees very slightly and naturally bent, lower legs are straight bone segments, relaxed ankle/toe extension, no S-shaped or twisted legs. Torso upright with only slight forward lean. The RIGHT shooting arm reaches high above and slightly in front of the head, elbow naturally nearly extended. Shooting palm and all five fingers are elegantly open and gently curved as if a ball has JUST left the fingertips; a mild natural wrist flex, no broken wrist. The LEFT guide hand with black wristband opens a little away from the shooting hand, elbow softly bent. Both hands and both arm connections must be fully visible and correct, no arms crossing through face or body.\nCRUCIAL: NO BASKETBALL anywhere. It will be animated separately. NO other character, no sequence of poses, no atlas, no ground, no shadow, no text outside the uniform, no background paint. One large attractive whole-body drawing centered with padding on genuine alpha transparency. Keep all fingertips and shoes in frame. NOT 3D, NOT a muscular adult, no mechanical deformation, no duplicate limbs.",
>   "defender_jump": "Use case: illustration-story. Asset type: ONE full-body transparent 2D anime basketball defender cutout.\nInput 1 supplies the approved 1990s hand-inked basketball anime visual style, natural athletic teenage proportions, clean cel shading and line quality. The subject is a DIFFERENT player from Rukawa: a tall Japanese teenage RYONAN opponent, white sleeveless jersey and roomy white basketball shorts with deep blue edging, number 7, tall spiky black hair, white/blue sneakers. No red uniform, no Rukawa fringe.\nDraw one entire anatomically correct connected person in an AIRBORNE contested-shot defensive jump. Three-quarter front/side view facing SCREEN RIGHT, looking up/right at the shooter. The near arm stretches upward/right to contest a shot, open natural hand with five fingers; far arm raised but relaxed for balance. Torso slightly turns, knees mildly bent with believable thigh and calf lengths, both feet off ground with natural ankle extension. No rubbery knee shapes, no broken wrists, no extra or missing limbs. Strong lively action silhouette, teenage athletic build, attractive 1990s anime cel art, not 3D.\nGenuine transparent alpha around the full figure, generous padding around all fingers and shoes. No ball, no court, no ground shadow, no other characters, no pose sheet, no labels; only the jersey text RYONAN and number 7."
> }
> ```

> [!info]- videos/rukawa-final-three-20s-20261109/docs/revision-20261110/coherent-scene-prompt.txt
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

## 已有产物与检查依据

文件存在、技术检查、画面检查与用户验收分别看待。下表是本次文件盘点，旧版与当前版可能共存。MP4/HTML 按内容计算 SHA-256，ZIP 仅记录文件大小和修改时间供增量判断。

| 本地产物 | 字节数 | SHA-256 |
| --- | ---: | --- |
| `videos/rukawa-final-three-20s-20261109/outputs/rukawa-final-three-20s.mp4` | 22977059 | `1f9a2aa4cd040ba859ec3eb058cad0b1ab54f655e0d0445f026c9d771c10ca80` |
| `videos/rukawa-final-three-20s-20261109/outputs/rukawa-final-three-project.zip` | 56699604 | `ZIP：仅元数据` |

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
>       "r_frame_rate": "30/1",
>       "duration": "20.000000",
>       "nb_frames": "600",
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
>     "size": "22977059"
>   },
>   "file": "outputs/rukawa-final-three-20s.mp4",
>   "scope": "本次只读取媒体头；未全片解码、播放或重新进行视觉/听觉验收。"
> }
> ```


已有检查报告路径（未在本轮重新执行）：

- `videos/rukawa-final-three-20s-20261109/outputs/checks/audio-report.json`
- `videos/rukawa-final-three-20s-20261109/outputs/checks/media-metadata.json`
- `videos/rukawa-final-three-20s-20261109/outputs/checks/package-report.json`
- `videos/rukawa-final-three-20s-20261109/outputs/checks/verification.json`

## 制作方法与迭代记录选编

下面是项目自有文档的归档摘录，保留原阶段与原检查表述，个人路径已泛化、相对链接已注明源项目位置。它们属于来源陈述，不代表本次重新完成其中的生成或验收。

> [!info]- 来源文档：videos/rukawa-final-three-20s-20261109/README.md
> # 流川枫 · 压哨三分（20秒试作）
> 
> 完整视频：`outputs/rukawa-final-three-20s.mp4`。1280×720、30fps、600帧、20秒，含立体声现场音效。湘北以77–78落后，流川枫出手后终场蜂鸣器响，篮球入网，最终80–78获胜。
> 
> 本版采用完整人物原画、分镜剪辑、有限动画、独立篮球轨迹和程序球网。持球、跳投、封盖分别使用完整造型；身体主要保持刚性，出手仅有小幅手腕变化。配角、观众和旗帜有连续的小幅运动。它仍是有限动画试作，起跳和收球之间通过镜头切换连接，没有逐帧重绘全部人体动作。图集中的配角分辨率低于主角原画。
> 
> ## 独立安装与预览
> 
> 需要Node.js 22或更新版本、npm，以及检查与打包时使用的Python 3、ffmpeg、ffprobe。解压后在本目录执行：
> 
> ```sh
> mkdir -p .cache/tmp .cache/npm
> TMPDIR="$PWD/.cache/tmp" TMP="$PWD/.cache/tmp" TEMP="$PWD/.cache/tmp" npm ci --cache .cache/npm
> npm run dev
> ```
> 
> 预览地址为 `http://localhost:3096/RukawaFinalThree`。macOS默认使用系统Google Chrome；其他位置可设置 `CHROME_BIN`。没有设置可用Chrome时，Remotion自行下载浏览器至本工程的 `node_modules/.remotion/`。全部npm依赖在本目录安装，不需要项目根目录或其他视频的文件。
> 
> ## 检查、导出与打包
> 
> ```sh
> npm run render
> npm run check
> npm run package
> ```
> 
> `render`导出完整20秒MP4；`check`执行TypeScript检查、本地资源与路径检查、视频规格检查和完整解码检查；`package`生成 `outputs/rukawa-final-three-project.zip`，包含源码、全部素材、锁定依赖清单、最终MP4和检查报告，不包含可重建的 `node_modules/` 与缓存。
> 
> 辅助检查关键帧：`node scripts/run.mjs checks`。导出指定帧：`npm run still -- outputs/checks/frame.png --frame=232`。所有脚本都将工作目录、npm缓存与临时目录限定在本工程内部。
> 
> ## 修改素材与时间轴
> 
> - `public/art/`保存持球图、跳投图、防守者、球馆、观众、配角图集与篮球。`public/references/`保存造型参考。新生成的两张原画提示词在 `docs/new-asset-prompts.json`，生成来源和工具保留文件说明在 `docs/generation-origin.md`。
> - `src/Root.tsx`中的七个 `Series.Sequence` 控制镜头长度，总和保持600帧；同处管理比分和文字。每个镜头也注册为独立Composition，便于在Studio预览。
> - `src/scenes/drawScenes.ts`控制景别、人物、球网与观众运动。`src/math.ts`控制球轨迹和关键时刻：7.55秒离手、7.9秒蜂鸣器、11.55秒球心抵达篮圈、11.75秒更新比分。
> - `public/audio/arena.wav`是程序合成的20秒立体声环境音、球鞋声、蜂鸣器和入网声。修改 `scripts/audio.py` 后运行 `node scripts/run.mjs audio`，再重新导出。没有旁白或外部音乐采样。
> - 中文字体位于 `public/fonts/`，授权文本在 `docs/NOTO-OFL.txt`。
> 
> 目录内的所有运行资源均为独立副本。历史作品没有移动或修改。本轮图片生成工具的原始输出已移入本工程 `.cache/generation-originals/`，运行素材在 `public/art/`；原始工具输出位置和移动记录保存在 `docs/generation-origin.md`。

> [!info]- 来源文档：videos/rukawa-final-three-20s-20261109/docs/storyboard.md
> # 20 秒绝杀试作
> 
> 0.0–3.0 球馆与末段比分：湘北 77，陵南 78，比赛时间 2.4 秒。流川枫在三分线附近持球，队友与防守者准备。
> 3.0–4.2 持球蓄力中景，使用已确认完整原画的小幅运动。
> 4.2–6.0 手与球特写，保持手球形状关系，镜头上移。
> 6.0–10.0 低角度慢镜跳投，补绘完整跳投原画；防守者起跳封盖。7.55 秒离手，7.9 秒蜂鸣器响。
> 10.0–14.0 篮圈特写，球从右上方进入画面，11.55 秒球心抵达篮圈，球网发生弹性摆动，11.75 秒篮球穿过篮圈后比分更新为 80–78。
> 14.0–17.0 观众与湘北队友的获胜反应，分组连续晃动和挥旗。
> 17.0–20.0 流川枫安静特写与终场比分，湘北获胜。
> 
> 方法：分镜剪辑、补绘原画、有限动画与独立球轨迹。避免大幅拉伸静态四肢。它是 20 秒试作版，不声称逐帧手绘长动画或动作捕捉。

> [!info]- 来源文档：videos/rukawa-final-three-20s-20261109/docs/visual-review.md
> # 成片视觉检查记录
> 
> 检查对象为600帧的20秒试作。使用Remotion关键帧和最终MP4解码画面检查构图、持球手部、完整腿部轮廓、出手球位置、入网遮挡、比分和终场画面。
> 
> 本轮修正：
> 
> - 持球和跳投使用独立完整原画，移除旧版本的大幅手腿变形；跳投只对投篮手腕做小范围弯曲。
> - 开场站位放到画面右侧三分线外，防守者站在靠近篮筐的一侧。
> - 庆祝者置于球场地板上，观众保持在看台上；图集采样收紧到人物边界，去掉邻行带出的零星像素。
> - 篮球7.55秒离手，7.9秒终场蜂鸣器，11.55秒球心到达篮圈，11.75秒球已穿过篮圈后更新至80–78。
> - 篮球与后网、前网和前侧篮圈分别排序，避免球全程覆盖篮圈；球网加入衰减摆动。
> 
> 技术检查报告为 `outputs/checks/verification.json`。技术检查只验证路径、类型、编码、帧数、时长和解码，不等同于人体动作质量认证。
> 
> 仍然存在的局限：本版通过镜头切换连接不同原画，配角和观众主要为连续小幅晃动；没有逐帧重绘完整的收球、伸腿、落地和庆祝动作。配角图集的局部分辨率较低。本目录中早期的检查帧和修改过程截图是中间产物，最终交付以 `outputs/rukawa-final-three-20s.mp4` 为准。

> [!info]- 来源文档：videos/rukawa-final-three-20s-20261109/docs/revision-20261110/assessment.md
> # 2026-11-10 视觉协调反馈与路线修正
> 
> 用户反馈：20秒试作不像动漫视频，角色之间不协调。该反馈成立。此前的可解码、600帧等技术检查不能证明动画质量合格。
> 
> 现版的问题：主角、配角、背景分别制作，线条密度、面部画法、清晰度和比例不一致；独立摆放的角色缺少共同透视、视线和身体互动；角色主要刚性移动与小幅变形，跨镜头的收球、起跳、出手缺少连贯动作。增加晃动幅度不能解决这些问题。
> 
> 本轮制作 `public/art/revision-20261110/coherent-contested-shot-v1.png`：使用内置 image_gen 把出手、封盖、篮下球员与观众作为整幅场景重新绘制。参考已确认的流川枫造型和旧成片关键帧，重新安排共同空间、阴影与画风。这是风格与构图验证图，不是连续动画，不能据此声称已解决动作连贯性；三分线、镜头尺度等仍需在后续连续布局中复核。
> 
> 提示词位于 `coherent-scene-prompt.txt`，保存路径和摘要位于 `asset-record.json`。工作素材已保存到本项目；内置工具默认输出的原件仍由工具保留，具体位置已记录。本轮没有覆盖旧视频，也没有生成新的20秒成片。
> 
> 后续制作必须以同一镜头连续原画为单位：共用角色比例、球场布局、摄影机和光照；主角蓄力、伸腿、出手、落地与对手的封盖、落地要按同一时序设计；配角根据球的位置转头、移动和卡位。需要先用正常速度检查一段连续多人动作，再扩展完整叙事。当前工程的刚性贴图动画不足以达到用户要求的动漫动作质量。

## 证据清单与缺失信息

盘点的文本/报告文件 37 个，源文本、报告及媒体产物身份摘要为 `4832714e0737e343c157691bf321dcac7e6af331eb8ccb81ffdf27f0f0012140`。哈希证明采集内容身份，不代表其中每个断言已验证。模型权重、缓存、依赖、第三方源码树和未指定的历史会话未采集；大型文本仅做哈希并按需要选读。

- 本次整理基于明确引用的 README、制作方法、配置、提示词、关键源码与已有报告。未重新观看全部参考/候选/成片，也未做人工听辨。
- 项目未提供的原始模型内部实现、未披露生成后端、未记录 seed/参数、真实费用、人工总工时和用户最终验收，均不补造。实际日志有记录的耗时见上文，流水线耗时不等于整个制作总耗时。
- 源项目内的路径只用于查证；本档案未复制大型素材或模型，也不改变源项目运行依赖。

| 已盘点证据文件 | SHA-256（前 16 位）/状态 |
| --- | --- |
| `videos/rukawa-final-three-20s-20261109/README.md` | `8a3b6783be631187` |
| `videos/rukawa-final-three-20s-20261109/package-lock.json` | `6e7a3f6c79c5340f` |
| `videos/rukawa-final-three-20s-20261109/package.json` | `d3bbc5f076523bd5` |
| `videos/rukawa-final-three-20s-20261109/remotion.config.ts` | `89558888f253880f` |
| `videos/rukawa-final-three-20s-20261109/tsconfig.json` | `af262af0dd89b1e2` |
| `videos/rukawa-final-three-20s-20261109/docs/NOTO-OFL.txt` | `1c05c68c34f97084` |
| `videos/rukawa-final-three-20s-20261109/docs/asset-inventory.json` | `352806c1f62f5edb` |
| `videos/rukawa-final-three-20s-20261109/docs/generation-origin.md` | `5500c258cf925b04` |
| `videos/rukawa-final-three-20s-20261109/docs/new-asset-prompts.json` | `e64e798be075ba19` |
| `videos/rukawa-final-three-20s-20261109/docs/source-support-bounds.json` | `391c1f7cf138eaaf` |
| `videos/rukawa-final-three-20s-20261109/docs/storyboard.md` | `a2e1b22298416f30` |
| `videos/rukawa-final-three-20s-20261109/docs/visual-review.md` | `c737a99da1c12367` |
| `videos/rukawa-final-three-20s-20261109/docs/revision-20261110/assessment.md` | `53b6c4d9761a0ed4` |
| `videos/rukawa-final-three-20s-20261109/docs/revision-20261110/asset-record.json` | `718e7a64a3fe991d` |
| `videos/rukawa-final-three-20s-20261109/docs/revision-20261110/coherent-scene-prompt.txt` | `62ed3d6d00647704` |
| `videos/rukawa-final-three-20s-20261109/outputs/checks/audio-report.json` | `c738de6e3546aa23` |
| `videos/rukawa-final-three-20s-20261109/outputs/checks/media-metadata.json` | `55989ec55c9ce42d` |
| `videos/rukawa-final-three-20s-20261109/outputs/checks/package-report.json` | `9374360adf2af3b4` |
| `videos/rukawa-final-three-20s-20261109/outputs/checks/verification.json` | `524db556f165341a` |
| `videos/rukawa-final-three-20s-20261109/scripts/audio.py` | `b5e7e496590035a8` |
| `videos/rukawa-final-three-20s-20261109/scripts/check.py` | `004630da33d45722` |
| `videos/rukawa-final-three-20s-20261109/scripts/package.py` | `763ae1ff693c756d` |
| `videos/rukawa-final-three-20s-20261109/scripts/run.mjs` | `cb33736c41a58a51` |
| `videos/rukawa-final-three-20s-20261109/src/FilmCanvas.tsx` | `a287f3a04b07e163` |
| `videos/rukawa-final-three-20s-20261109/src/Root.tsx` | `47b0bf653c3335c5` |
| `videos/rukawa-final-three-20s-20261109/src/gpu.ts` | `6fbc417e582416c5` |
| `videos/rukawa-final-three-20s-20261109/src/index.ts` | `309d14a1bb4ad80b` |
| `videos/rukawa-final-three-20s-20261109/src/math.ts` | `dfbb44695c423bfc` |
| `videos/rukawa-final-three-20s-20261109/src/supportBounds.ts` | `be7e44a6bb557b11` |
| `videos/rukawa-final-three-20s-20261109/src/scenes/GatherScene.tsx` | `2255aa9c0f369570` |
| `videos/rukawa-final-three-20s-20261109/src/scenes/HandsScene.tsx` | `6a3e1c1079c95ae0` |
| `videos/rukawa-final-three-20s-20261109/src/scenes/HeroScene.tsx` | `73a883067018a33d` |
| `videos/rukawa-final-three-20s-20261109/src/scenes/OpeningScene.tsx` | `664da4989ac5f2d3` |
| `videos/rukawa-final-three-20s-20261109/src/scenes/ReactionScene.tsx` | `d1b42917b9149e1e` |
| `videos/rukawa-final-three-20s-20261109/src/scenes/ReleaseScene.tsx` | `acfb29ccd0d59589` |
| `videos/rukawa-final-three-20s-20261109/src/scenes/RimScene.tsx` | `9b0a7072a347a831` |
| `videos/rukawa-final-three-20s-20261109/src/scenes/drawScenes.ts` | `b50fad6d469e8e30` |
