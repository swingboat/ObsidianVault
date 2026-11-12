---
type: video-production-dossier
project: video-creation
video_id: rukawa-anime-2d-prototype-20261109
captured: '2026-11-12'
status: 5 秒原型；v2 动作连贯性未通过用户验收
source_fingerprint: d0b3dd197d7b04d56da9f23596194eadc29e290d97d44329396d999ecc6e00c6
tags:
- video-creation
- production-capture
---

# 流川枫 · 九姿态二维有限动画原型

<!-- capture-source-sha256:d0b3dd197d7b04d56da9f23596194eadc29e290d97d44329396d999ecc6e00c6 -->

本次为指定项目的材料回溯与归档。制作结论来自项目记录/源码，未找回关闭的制作对话，也未重新执行生成、渲染、音频合成或用户验收。

## 当前结论与制作路线

**状态：5 秒原型；v2 动作连贯性未通过用户验收。**

imagegen 姿态图集 → 有限动画/独立球轨迹 → 程序球网 → 拟音。

1280×720、30fps、150 帧。九格投篮姿态加五名配角、两处看台反应，分为起跳 0–2.2 秒和篮筐 2.2–5 秒。只交付样片，原定 20 秒未扩展。

## 关键算法、实现与适用边界

3×3 图集每格 418×418；按动作阶段选择图格并校准 handX/handY。配角从 5×3 图集取三种姿态，各自位置轨迹错时；观众划为六组切换绘制状态。篮球用独立连续图层和旋转，篮圈/网分层处理遮挡。区域差异检测只能说明区域有变化，不能证明人体动作连贯。

## 声音与字幕

Python 标准库合成 48kHz 立体声球馆、鞋声、蜂鸣、篮网和欢呼，无旁白。

## 失败、修正与验收边界

诊断认为独立姿态切换无法满足连续动作；README 明确 v2 未通过用户验收。后续路线应采用连续变形或连续视频生成，这份原型保留为失败经验，不能写成最终成功方案。

## 技术与模型版本

以下是本项目声明或锁定的依赖，安装清单本身不证明某项参与了最终画面。具体用途以制作路线、源码和生成记录交叉判断。

| 依赖 | package.json 声明 | 可解析的直接锁定版本 |
| --- | --- | --- |
| `@remotion/cli` | `4.0.541` | `4.0.541` |
| `@remotion/media` | `4.0.541` | `4.0.541` |
| `react` | `19.3.1` | `19.3.1` |
| `react-dom` | `19.3.1` | `19.3.1` |
| `remotion` | `4.0.541` | `4.0.541` |
| `@types/react` | `19.3.0` | `19.3.0` |
| `@types/react-dom` | `19.2.3` | `19.2.3` |
| `typescript` | `5.9.3` | `5.9.3` |

工具底层模型未公开时按“未披露”记录；不能由 imagegen 工具名猜测图像模型。未提供真实推理日志的配置不记作已运行。

## 镜头、时间轴与采用参数

> [!info]- 当前 Composition / 时间轴源码：videos/rukawa-anime-2d-prototype-20261109/src/Root.tsx，L1–31
> ```tsx
> import React,{useEffect,useState} from 'react';
> import {AbsoluteFill,Composition,continueRender,delayRender,cancelRender,interpolate,Sequence,staticFile,useCurrentFrame,useVideoConfig} from 'remotion';
> import {Audio} from '@remotion/media';
> import {CourtShot} from './CourtShot';
> import {HoopShot} from './HoopShot';
> const Film=()=>{
>  const frame=useCurrentFrame();const {fps}=useVideoConfig();
>  const [handle]=useState(()=>delayRender('Load local title font'));
>  useEffect(()=>{const font=new FontFace('FilmHan',`url(${staticFile('fonts/Arial-Unicode.ttf')})`);font.load().then(f=>{document.fonts.add(f);continueRender(handle);}).catch(cancelRender);},[handle]);
>  const won=frame>=3.55*fps;
>  return <AbsoluteFill style={{fontFamily:'FilmHan,Arial,sans-serif',background:'#171c20',overflow:'hidden'}}>
>   <Sequence name="起跳 / 球场与球员" durationInFrames={2.2*fps} premountFor={fps}><CourtShot/></Sequence>
>   <Sequence name="篮筐 / 入网与观众" from={2.2*fps} durationInFrames={2.8*fps} premountFor={fps}><HoopShot/></Sequence>
>   <Audio src={staticFile('soundscape.wav')} premountFor={fps} volume={.9}/>
>   <div style={{position:'absolute',left:0,right:0,top:0,height:23,background:'#10171b'}}/>
>   <div style={{position:'absolute',left:0,right:0,bottom:0,height:23,background:'#10171b'}}/>
>   <div style={{position:'absolute',left:35,top:38,display:'flex',color:'#f4ecda',boxShadow:'2px 3px 0 #1c18197d',fontWeight:700}}>
>    <div style={{background:'#a02c30',padding:'7px 14px',fontSize:18}}>湘北</div>
>    <div style={{background:'#252d32',padding:'5px 14px',fontSize:23,letterSpacing:4}}>{won?'77':'74'} : 76</div>
>    <div style={{background:'#ded2b9',color:'#302c28',padding:'9px 12px',fontSize:13}}>{won?'终场':'第四节'}</div>
>   </div>
>   <div style={{position:'absolute',left:39,bottom:41,color:'#fff7e4',textShadow:'1px 2px 2px #1d2928',opacity:interpolate(frame,[3.9*fps,4.25*fps],[0,1],{extrapolateLeft:'clamp',extrapolateRight:'clamp'})}}>
>    <span style={{fontSize:29,letterSpacing:5,fontWeight:700}}>三分，逆转。</span>
>   </div>
>  </AbsoluteFill>;
> };
> export const Root=()=> <>
>  <Composition id="RukawaAnimeSample" component={Film} durationInFrames={150} fps={30} width={1280} height={720}/>
>  <Composition id="CourtShot" component={CourtShot} durationInFrames={66} fps={30} width={1280} height={720}/>
>  <Composition id="HoopShot" component={HoopShot} durationInFrames={84} fps={30} width={1280} height={720}/>
> </>;
> ```

源时间轴、生成帧数、所用区间与最终时长分别记录；历史文档或候选参数不自动覆盖当前采用配置。

## 参考来源与借鉴说明

在本次盘点的项目自有材料中，未找到可明确归属本片的外部参考视频或 X 帖子链接；这不等于历史上没有参考。

本地造型/参考文件（本轮仅核对存在，不宣称重新看过全部图像）：

- `videos/rukawa-anime-2d-prototype-20261109/public/references/rukawa-approved.png`


## 重现入口与修改位置

以下是 package.json 中的实际命令映射，供在对应视频目录使用；本轮未执行这些制作命令。系统依赖、前置模型/素材和环境准备见后附 README。

| npm 入口 | 实际命令 |
| --- | --- |
| `npm run dev` | `node scripts/run.mjs dev` |
| `npm run render` | `node scripts/run.mjs render` |
| `npm run still` | `node scripts/run.mjs still` |
| `npm run audio` | `node scripts/run.mjs audio` |
| `npm run check` | `node scripts/run.mjs check` |
| `npm run build` | `node scripts/run.mjs bundle` |
| `npm run package` | `node scripts/run.mjs package` |

### 算法源码摘录

> [!info]- videos/rukawa-anime-2d-prototype-20261109/src/CourtShot.tsx，L1–29
> ```tsx
> import React from 'react';
> import {AbsoluteFill, CanvasImage, interpolate, staticFile, useCurrentFrame} from 'remotion';
> import {Basketball} from './Basketball';
> import {SupportingPlayers} from './SupportingPlayers';
> import {Crowd} from './Crowd';
> const S=510;const left=48;const top=153;
> const keyFrames=[0,12,18,22,26,30,34,39,44];
> export const poseAt=(f:number)=>{let i=0;while(i<8&&f>=keyFrames[i+1])i++;return i;};
> const handX=[268,249,241,249,263,270];const handY=[142,191,69,10,0,10];
> export const CourtShot=()=>{
>  const f=useCurrentFrame();const pose=poseAt(f);
>  const jump=interpolate(f,[0,18,22,26,32,38,44,66],[0,0,-19,-44,-49,-23,0,0],{extrapolateRight:'clamp'});
>  const bx=left+handX[Math.min(pose,5)]*S/418;
>  const by=top+handY[Math.min(pose,5)]*S/418+jump;
>  const u=Math.max(0,(f-30)/65);
>  const releaseX=left+270*S/418;const releaseY=top+10*S/418-22-49;
>  const x=f<30?bx:releaseX+(1055-releaseX)*u;
>  const y=f<30?by:releaseY+(130-releaseY)*u-300*u*(1-u);
>  return <AbsoluteFill style={{background:'#cbb494',overflow:'hidden'}}>
>   <CanvasImage src={staticFile('art/court-clean-v2.png')} style={{width:'100%',height:'100%'}}/>
>   <Crowd view="court"/>
>   <SupportingPlayers/>
>   <svg width="1280" height="720" style={{position:'absolute',inset:0}}><ellipse cx="313" cy="655" rx={59+jump*.15} ry="9" fill="#3d3029" opacity={.22+jump*.0015}/></svg>
>   <div style={{position:'absolute',left,top:top+jump,width:S,height:S,overflow:'hidden'}}>
>    <CanvasImage src={staticFile('art/rukawa-poses-v2.png')} style={{position:'absolute',width:S*3,height:S*3,left:-(pose%3)*S,top:-Math.floor(pose/3)*S}}/>
>   </div>
>   <Basketball x={x} y={y} r={f<30?23:23-u*7} angle={f<30?0:(f-30)*13}/>
>  </AbsoluteFill>;
> };
> ```

> [!info]- videos/rukawa-anime-2d-prototype-20261109/src/HoopShot.tsx，L1–34
> ```tsx
> import React from 'react';
> import {AbsoluteFill, CanvasImage, staticFile, useCurrentFrame, useVideoConfig} from 'remotion';
> import {Basketball} from './Basketball';
> import {Crowd} from './Crowd';
> const cx=732,cy=302,rx=187,ry=48;
> export const HoopShot=()=>{
>  const f=useCurrentFrame();const {fps}=useVideoConfig();const t=f/fps+2.2;
>  const tau=Math.max(0,t-3.35);const pulse=tau>0?Math.exp(-tau*2.8)*Math.sin(tau*11):0;
>  const net=(front:boolean)=>Array.from({length:12},(_,j)=>{
>   const a=j*Math.PI*2/12;const points=Array.from({length:8},(_,k)=>{
>    const u=k/7;const radius=rx*(1-u*.42);const aa=a+(j%2===0?1:-1)*u*.95;
>    return `${cx+radius*Math.cos(aa)+pulse*22*u},${cy+ry*Math.sin(aa)*(1-u)+u*(191+pulse*28)}`;
>   }).join(' ');
>   if((Math.sin(a)>=0)!==front)return null;
>   return <g key={j}><polyline points={points} fill="none" stroke="#332c29" strokeWidth="5.3" strokeLinejoin="round"/><polyline points={points} fill="none" stroke="#f8f0d9" strokeWidth="3.3" strokeLinejoin="round"/></g>;
>  });
>  const u=Math.max(0,Math.min(1,(t-2.2)/1.15));
>  const x=t<=3.35?500+(cx-500)*u:cx+tau*40;
>  const y=t<=3.35?-160+(cy+160)*u*u:cy+tau*510+tau*tau*190;
>  return <AbsoluteFill style={{background:'#74909d',overflow:'hidden'}}>
>   <CanvasImage src={staticFile('art/hoop-crowd.png')} style={{width:'100%',height:'100%'}}/>
>   <Crowd view="hoop"/>
>   <svg width="1280" height="720" style={{position:'absolute',inset:0}}>{net(false)}</svg>
>   <Basketball x={x} y={y} r={65} angle={f*12}/>
>   <svg width="1280" height="720" style={{position:'absolute',inset:0}}>
>    <defs><clipPath id="rim-front"><path d="M541 301 C548 370 913 381 922 298 L911 298 C901 354 562 355 551 300 Z"/></clipPath></defs>
>    <image href={staticFile('art/hoop-crowd.png')} width="1280" height="720" clipPath="url(#rim-front)"/>
>    {net(true)}
>    {Array.from({length:4},(_,i)=>{
>     const u=(i+1)/4;return <ellipse key={i} cx={cx+pulse*22*u} cy={cy+u*(191+pulse*28)} rx={rx*(1-u*.42)} ry={ry*(1-u)*.65+3} fill="none" stroke="#eae3d2" strokeWidth="2"/>;
>    })}
>   </svg>
>  </AbsoluteFill>;
> };
> ```

## 完整关键提示词与生成记录

以下保留项目自有提示词、候选参数与版本。存在失败候选和历史参数；最终采用以 selected-takes、edit-decisions 与制作复盘为准，不能将所有候选都写成已采用。

> [!info]- videos/rukawa-anime-2d-prototype-20261109/docs/continuity-review/continuous-test-prompt.txt
> ```text
> Locked wide sideline shot, one uninterrupted five-second sequence. Preserve the supplied frame's traditional hand-drawn basketball-anime style, character identities, body proportions, red and white uniforms, and gym perspective.
> 
> The red number 11 shooter smoothly lowers his center of gravity while keeping both shoes planted, raises the basketball from his chest to above his forehead, straightens his legs into a jump, releases the ball from his shooting fingertips with a natural wrist follow-through, and lands on both feet with knees absorbing the impact. The ball remains in contact with his hands until the release, then travels on one continuous arc toward the basket.
> 
> The white number 6 defender takes two grounded closing steps, reacts slightly later than the shooter, jumps with his arms raised to contest, and lands with a controlled knee bend. His approach and jump are continuous, with stable limb lengths and a consistent face.
> 
> The other players make small, continuous boxing-out and balance adjustments, track the airborne ball with their heads, and keep their feet grounded until they actually step. Their reactions occur at different moments, following the shot. Fans in the stands clap and wave towels in small asynchronous motions.
> 
> The camera, floor lines, basket, walls and roof remain stable. Character sizes, jersey numbers and anatomy remain consistent across every frame. Continuous intermediate body motion connects all actions.
> ```

> [!info]- videos/rukawa-anime-2d-prototype-20261109/docs/prompts/court.txt
> ```text
> Use case: illustration-story. Asset: one 16:9 2D hand-painted basketball anime background plate, 1536x864 composition. Use the supplied Rukawa reference ONLY to match the exact 1990s Slam Dunk anime ink linework, warm cel colors, anatomy and uniform design; do not reproduce its layout.
> Scene: an indoor Japanese inter-high championship basketball gymnasium as in classic Slam Dunk. Eye-level sideline camera from a front-left three-quarter angle, looking diagonally across the maple floor toward a hoop on the upper right. Polished honey-colored wood, clear white court markings including the curved three-point line in the near foreground, green boundary padding, a believable transparent rectangular backboard and orange rim with white net. Large steel roof trusses and tall clerestory windows, muted green and cream walls. Dense raised spectator stands across the entire upper background, illustrated Japanese students and supporters in varied casual clothes; recognizable human faces, some standing and some sitting, several small red cheering towels, coherent rows and railings. It must feel like a scene from Slam Dunk rather than a modern NBA arena.
> Players: 5 supporting players naturally spaced in the midground, all drawn in the same detailed anime style. Shohoku teammates wear red uniforms with black numbers edged in ivory: Sakuragi-like tall red-haired number 10 near the left lane, imposing Akagi-like black-haired number 4 under the basket, a short dark-haired red number 7 farther back. Two opponents wear white uniforms with dark navy trim, athletic youthful proportions; one is contesting with hands raised just right of the empty shooting area, the other is near the right lane. No player should occupy the large open foreground shooting area from 22% to 50% of picture width, from 38% to 88% of picture height. This space is reserved for a separately animated Rukawa. Do not draw Rukawa or any number 11. No basketball anywhere. Single consistent camera, physically coherent perspective, grounded feet.
> Style: crisp hand-inked cel animation, expressive natural human anatomy, restrained manga hatching, subtle watercolor background grain, warm amber floor and blue-green structural shadows. No 3D renders, no dolls, no photorealistic people. No subtitle, no watermark, no title text. Keep the floor foreground spacious and the hoop fully visible.
> ```

> [!info]- videos/rukawa-anime-2d-prototype-20261109/docs/prompts/hoop.txt
> ```text
> Use case: illustration-story. Asset: one cinematic 16:9 2D anime close-up background plate of the basket in a classic Slam Dunk indoor inter-high basketball gymnasium. Use the supplied reference image for the same warm 1990s hand-drawn cel anime palette and ink outlines.
> Camera from in front and slightly below the hoop, looking up at the backboard from the left side. The orange rim is a clear wide horizontal ellipse centered approximately at x=58%, y=43% of the image, fully visible. Transparent glass backboard with a white target rectangle behind it at the upper right, metal supports and cream/green padding. Draw the rim and its support bracket accurately but OMIT the hanging white net entirely: it will be animated as a separate layer. Absolutely no basketball anywhere. Leave ample clear air above the rim for an approaching ball.
> Background across the lower third: packed spectator stands with expressive hand-drawn Japanese high school students and fans, coherent human faces and varied clothes, several red towels, no featureless repeated blobs. Above them are tall blue-gray stadium roof trusses, bright diffused window light and warm ceiling lamps, believable perspective. Background can be softly painted, but retain manga/anime linework on hoop and spectators. No 3D, no photograph, no title, no scoreboard digits, no watermark. Balanced composition, no split panels.
> ```

> [!info]- videos/rukawa-anime-2d-prototype-20261109/docs/prompts/poses-edge-cleanup.txt
> ```text
> Edit target: the supplied 3x3 transparent anime Rukawa animation sprite sheet. Preserve all nine drawings, their exact cell positions and dimensions, all character identities, poses, anatomy, face, hair and uniform details. Change ONLY the extraction edges: completely remove every stray red/yellow colored glow, red fringe, yellow fringe, white floor speck and semi-transparent paint noise outside the character silhouettes. The output must have clean natural black ink contours and genuine transparent alpha surrounding each figure, including the gaps between legs and arms. Do not add anything. Do not redraw the layout or change poses. Do not add a background or checkerboard pattern. Keep the 3x3 grid alignment and the same square canvas dimensions.
> ```

> [!info]- videos/rukawa-anime-2d-prototype-20261109/docs/prompts/poses.txt
> ```text
> Use case: illustration-story. Asset: a clean transparent 3 by 3 animation key-pose sprite sheet, ONE SQUARE IMAGE, exactly nine equal square cells arranged in a precise 3-column x 3-row grid; no borders, no lettering, no cell labels. Each cell must contain one complete full-body depiction of the SAME Kaede Rukawa character, matching the supplied reference face, soft black bangs, red SHOHOKU number 11 jersey, matching shorts, black forearm band and white-black-red high-top shoes EXACTLY. Natural tall slender athletic anatomy, consistent scale and head size across all nine cells, consistent front-left three-quarter side view facing toward SCREEN RIGHT. Head and both shoes fully within each cell, generous transparent margins, feet near the bottom baseline, identical camera and proportions. Draw hands and all fingers clearly.
> This is a sequential basketball jump-shot animation. Read cells left to right then top to bottom:
> 1. relaxed shot-ready stance, knees slightly bent, both empty hands cupping an imaginary ball in front of lower chest;
> 2. deeper knee bend and weight-loading, hands slightly lower in front of stomach;
> 3. legs beginning to straighten, both hands raise the imaginary ball toward forehead;
> 4. takeoff, heels and both feet just off floor, imaginary ball above the forehead, right elbow underneath it;
> 5. airborne peak, right shooting arm extending up and forward, left guiding palm beside imaginary ball;
> 6. release, fully extended right arm, wrist flexed naturally downward in classic follow-through, left guide hand separated and relaxed;
> 7. descent, right arm still high in follow-through, knees relaxed;
> 8. landing with both feet down, knees absorb impact, arms begin to lower;
> 9. composed upright finish, arms relax toward waist, calm confident expression.
> IMPORTANT: Do NOT draw a basketball in ANY cell, because one separate continuous ball layer will be composited to the hands. No orange circles, no props. No ground shadows or scenery, the actual background must be transparent alpha.
> Medium: high quality traditional 2D cel anime drawings with clean thin black contour lines and flat red/skin colors with 2-3 shadow tones. This is not a wooden toy, not a 3D model, not a chibi. Preserve the natural human joints and gravity-following black hair. No duplicate or missing cells, no body part crossing into adjacent cells, no additional characters.
> ```

> [!info]- videos/rukawa-anime-2d-prototype-20261109/docs/prompts/v2/clean.txt
> ```text
> Edit target: the supplied single anime basketball gym background. Preserve the exact camera, canvas dimensions, architecture, windows, roof trusses, floor perspective, all court lines, basket and every spectator in their existing locations. Remove ONLY all FIVE basketball players standing on the court: the three red-uniformed teammates and the two white-uniformed opponents. Remove their cast shadows and floor reflections as well. Fill their former locations with perfectly coherent empty maple floor, green padded wall and court markings, continuous with the surroundings. Do not add any new people or a basketball to the court. The spectators in the stands must stay untouched. Keep the exact same 1990s Slam Dunk anime linework, painted textures and color palette. This is a clean animation background plate for separately animated player layers. No text additions.
> ```

> [!info]- videos/rukawa-anime-2d-prototype-20261109/docs/prompts/v2/courtCheer.txt
> ```text
> Edit target: supplied anime court CLEAN background plate. Preserve the exact canvas dimensions, locked camera, gym architecture, windows, steel trusses, basket, railing, empty maple floor, green wall, shadows and every court marking. Keep the court EMPTY.
> Change ONLY selected spectators' arm gestures in the existing stands to create the second frame of a cheering animation. Keep each spectator's identity, clothes, scale, head position and seat location consistent. Fans who originally hold a red towel now raise or bend it to the opposite side; several foreground fans now have palms together mid-clap; some previously low arms lift into cheers; a few high hands come down. Make the arm/hand and red-towel shapes clearly different, but do not shift or redraw the surrounding building. Faces remain coherent classic Slam Dunk anime characters. No new foreground players, no basketball, no new text. This output must align as closely as possible with the input for animating only masked spectator regions.
> ```

> [!info]- videos/rukawa-anime-2d-prototype-20261109/docs/prompts/v2/hoopCheer.txt
> ```text
> Edit target: supplied Slam Dunk anime hoop-close-up background. Preserve the exact camera, canvas dimensions and pixel registration of the complete backboard, orange rim, brackets, support post, roof, walls, windows, railing and lights. Do not add a basketball or net.
> Change ONLY the visible spectators to create another cheering pose: keep all their faces, hairstyles, clothes, body locations and seats identifiable and consistent. The man carrying the red towel waves it to his other side; the central boys lift or lower their arms; several girls and boys clap with palms together rather than apart; other supporters raise fists, and red towels take clearly different curved shapes. Animate real arm, hand and towel gestures, not a shifted copy of the whole image. Keep the painted anime style and colors exactly coherent. No extra people, no moving architecture, no new text. All non-spectator pixels should remain as unchanged as possible.
> ```

> [!info]- videos/rukawa-anime-2d-prototype-20261109/docs/prompts/v2/players.txt
> ```text
> Use case: illustration-story. Reference: the supplied anime basketball court image defines the FIVE supporting players' identities, face designs, hair, uniforms and exact anime drawing style. Generate ONLY these five characters, extracted and redrawn as an animation sprite sheet on genuine transparent alpha.
> Layout: exactly THREE COLUMNS and FIVE ROWS in one portrait image, fifteen equally sized rectangular cells. Each row is one specific character in three consecutive poses, left to right. One complete full-body figure per cell, every shoe and hand entirely inside the cell. Consistent character scale and baseline within each row, same camera looking from the front-left at three-quarter angle. Generous transparent margins, no grid lines or labels. No scenery, no ground shadows, no basketball, no Rukawa or number 11.
> Row 1: Shohoku number 10, Sakuragi, tall slim-muscular teenager with bright red short spiky hair, red jersey and shorts, white-red shoes. Pose A: bent-knee rebound-ready stance watching the shooter to his left. Pose B: an actual stepping stride toward the basket to screen right, one foot moving forward and arms preparing. Pose C: both arms raised for a rebound, gaze upward/right.
> Row 2: Shohoku number 7, Miyagi, shorter athletic teenager with dark curly hair, red uniform. A: ready stance looking left. B: jogging toward screen right, different leg stride and arm swing. C: turning his gaze upward to the ball, lifting one arm to signal.
> Row 3: Shohoku number 4, Akagi, very tall muscular teenage center with dark crew cut and strong square jaw, red uniform and black knee support. A: wide boxing-out stance, bent knees and arms out. B: turning his shoulders to the hoop on screen right, elbows and hands raised near shoulders. C: extending both arms upward for the rebound.
> Row 4: opponent number 6, youthful Japanese male with short black hair and white uniform with navy blue trim. A: low defensive stance, facing toward the shooter at screen left. B: clearly airborne contested-shot jump, both arms fully extended toward screen left/up. C: landing, knees absorbing impact, arms beginning to lower while looking up/right at the ball.
> Row 5: opponent number 5, youthful Japanese male with short side-parted dark hair and white/navy uniform. A: guarding and boxing-out stance. B: a lateral shuffle, one arm reaching out and knees bent. C: head turned toward the rim up/right, shoulders and arms lowered in reaction.
> All jerseys must retain their correct row's number and red-team chest text SHOHOKU. Natural continuous human anatomy, coherent hands and facial identity across poses. Match classic Slam Dunk cel animation faithfully. Clean ink outlines and flat shaded colors, no 3D, no dolls, no extra limbs, no other characters, no poses spilling into adjacent cells.
> ```

## 已有产物与检查依据

文件存在、技术检查、画面检查与用户验收分别看待。下表是本次文件盘点，旧版与当前版可能共存。MP4/HTML 按内容计算 SHA-256，ZIP 仅记录文件大小和修改时间供增量判断。

| 本地产物 | 字节数 | SHA-256 |
| --- | ---: | --- |
| `videos/rukawa-anime-2d-prototype-20261109/outputs/rukawa-anime-sample-5s.mp4` | 1747249 | `33a3090078775510ca31ae6185f2a06873ff462176b297390389dea5ba705803` |
| `videos/rukawa-anime-2d-prototype-20261109/outputs/rukawa-anime-sample-project.zip` | 53172443 | `ZIP：仅元数据` |
| `videos/rukawa-anime-2d-prototype-20261109/outputs/rukawa-anime-sample-v2-5s.mp4` | 3746425 | `cb3298396e74d9b5a5f8e1d48bfc15db4e9cb0ca664608a772c0d68be2ed271e` |
| `videos/rukawa-anime-2d-prototype-20261109/outputs/rukawa-anime-sample-v2-project.zip` | 81934808 | `ZIP：仅元数据` |

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
>       "duration": "5.000000",
>       "nb_frames": "150",
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
>       "duration": "5.013000",
>       "nb_frames": "237"
>     }
>   ],
>   "format": {
>     "duration": "5.013000",
>     "size": "3746425"
>   },
>   "file": "outputs/rukawa-anime-sample-v2-5s.mp4",
>   "scope": "本次只读取媒体头；未全片解码、播放或重新进行视觉/听觉验收。"
> }
> ```


已有检查报告路径（未在本轮重新执行）：

- `videos/rukawa-anime-2d-prototype-20261109/outputs/checks/audio.json`
- `videos/rukawa-anime-2d-prototype-20261109/outputs/checks/final-check.json`

## 制作方法与迭代记录选编

下面是项目自有文档的归档摘录，保留原阶段与原检查表述，个人路径已泛化、相对链接已注明源项目位置。它们属于来源陈述，不代表本次重新完成其中的生成或验收。

> [!info]- 来源文档：videos/rukawa-anime-2d-prototype-20261109/README.md
> # 流川枫 二维投篮动作样片 v2
> 
> 当前质量状态：v2 的动作连贯性未通过用户验收。现有媒体参数、解码与区域差异检查仅表示文件可播放和画面有变化，不代表人物动作连贯。诊断见 docs/continuity-review/audit.json；下一阶段需改用连续变形的人物绑定或连续视频生成流程。
> 
> 
> 5 秒 / 1280×720 / 30fps / 150 帧。按已认可的流川枫原型，制作投篮、篮球飞行与入网的二维有限动画样片，同时检查湘北队友、对手、球馆和观众的画风。此轮交付 5 秒样片，尚未扩展为原定 20 秒版本。
> 
> ## 安装、预览、检查、导出
> 
> 需要 Node.js 24、Python 3、FFmpeg / ffprobe 和系统 Chrome。从本目录执行：
> 
> ```bash
> mkdir -p .cache/tmp .cache/npm
> export TMPDIR="$PWD/.cache/tmp" TMP="$PWD/.cache/tmp" TEMP="$PWD/.cache/tmp"
> npm ci
> npm run dev
> npm run check
> npm run render
> npm run package
> ```
> 
> 预览为 http://localhost:3091/RukawaAnimeSample 。默认导出 outputs/rukawa-anime-sample-v2-5s.mp4；工程包 outputs/rukawa-anime-sample-v2-project.zip。音轨已包含在 public/，无需重新合成；要重建原创拟音可执行 npm run audio。类型检查：node_modules/.bin/tsc。构建：npm run build。
> 
> ## 修改素材与时间轴
> 
> - public/references/rukawa-approved.png：用户认可的造型原型。本目录保留独立副本。
> - public/art/court-clean-v2.png：移除场内球员后的独立球馆背景。
> - public/art/court-cheer-v2.png 和 hoop-cheer-v2.png：两个镜头的观众反应姿态；原有对应背景提供另一姿态。
> - public/art/support-players-v2.png：5 行 × 3 列的配角动作格，共 15 个姿态。
> - src/SupportingPlayers.tsx：五名配角独立安排跑动、起跳、落地与篮板动作。src/support-bounds.json 是透明通道分析得到的裁切和脚部对齐数据。
> - src/Crowd.tsx：两处看台各划分六组，以错开的时序切换手掌、手臂和毛巾的绘制状态。建筑、地板和篮架保留在固定底图。
> - public/art/rukawa-poses-v2.png：九格透明投篮关键姿态，3×3，1254×1254。每格 418×418。v1 为本目录内的生成中间稿。
> - public/art/hoop-crowd.png：篮筐近景和观众背景；篮网由代码绘制并响应进球。
> - src/CourtShot.tsx：动作格时间、角色位置、跳跃高度和手部持球点。替换动作素材后，需要校准每格尺寸及 handX / handY。
> - src/HoopShot.tsx：篮球通过篮圈的时间、前后遮挡和篮网摆动。替换篮筐画面需同时调整 cx、cy、rx、ry 和前缘遮挡区域。
> - src/Basketball.tsx：独立的连续篮球图层，线条、颜色、位置与旋转。
> - src/Root.tsx：两段镜头各有独立 JSX 时间节点；起跳镜头 0–2.2 秒，篮筐近景 2.2–5 秒；比分在 3.55 秒从 74∶76 变为 77∶76。
> - scripts/audio.py：Python 标准库合成 48kHz 立体声球馆底噪、鞋声、蜂鸣、篮网和欢呼，无旁白。要加配音，将录音放在本目录 public/ 并在 Root.tsx 添加独立 Audio 节点。
> 
> 如改变 fps 或时长，同时修改 Root.tsx、CourtShot.tsx 动作格帧数、audio.py、check.py，随后重建声音、检查与导出。
> 
> ## 素材生成与范围
> 
> 使用内置 image_gen 生成二维素材，并用当前目录中的造型原型作为身份与画风参考。完整提示词：
> 
> - docs/prompts/court.txt
> - docs/prompts/poses.txt
> - docs/prompts/poses-edge-cleanup.txt
> - docs/prompts/hoop.txt
> 
> 生成输出从工具默认目录移入本视频目录，未留下本次生成图片的外部临时副本；来源与路径见 docs/assets.json。没有调用 AI 视频服务，也没有使用现成《灌篮高手》视频片段。
> 
> 这是九个绘制关键姿态与独立篮球、篮网图层组合的有限动画。流川枫有动作变化，篮球和篮网连续运动；v2 增加了五名配角的独立位置轨迹与每人三个绘制姿态；观众按六个区域错时切换两个绘制手势状态。观众未逐人独立绑定，整片仍属有限关键帧动画。此样片用于检查画风、镜头、接球/出手接触与节奏，不能称作已完成完整逐帧动画。上一版样片保存在 outputs/versions/v1/，源码快照保存在 src/versions/v1/。
> 
> ## 独立性与打包
> 
> 所有受控素材、脚本、依赖、缓存、临时文件、声音、导出和检查报告均位于本目录。运行时不读取其他视频或项目根目录；系统工具属于外部软件依赖。系统 Chrome 必须已安装，配置优先使用该安装；系统工具自身的系统缓存未声称完全隔离。
> 
> npm 缓存指向 .cache/npm/；run.mjs 固定工作目录，并设置 TMPDIR / TMP / TEMP / XDG_CACHE_HOME。工程包包含此项目文件和本地素材，排除 node_modules、.cache 和可重建的 bundle；解压后可独立 npm ci、预览和导出。
> 
> ## v2 修复验证
> 
> 新增提示词保存在 docs/prompts/v2/。使用内置 image_gen 生成，四张新素材都已从工具默认输出目录移入本视频目录，生成的外部临时副本未保留。
> 
> 检查会解码实际成片，对比五个配角区域、运球镜头和篮筐镜头的观众区域，要求各区域有可测变化；另检查无角色的墙面区域保持稳定，避免用整个背景移动代替角色动作。区域变化是画面层检查，人物互相遮挡时不代表每一个角色的每个像素都独立可见。

> [!info]- 来源文档：videos/rukawa-anime-2d-prototype-20261109/docs/brief.md
> # 二维投篮样片
> 
> 范围：先制作 5 秒、1280×720、30fps 的二维动作样片，验证人物造型、投篮关键帧和球场、球员、观众的整体画风。样片通过外观与动作反馈后再扩展原定 20 秒版本。
> 
> 人物：沿用用户认可的流川枫原型，黑色自然碎发、修长运动员比例、湘北红色 11 号球衣。关键姿态使用绘制的二维动作格，不使用木偶式几何人体。
> 
> 环境：日式高中篮球联赛室内体育馆，木地板、白色场线、篮板、篮圈、层叠看台、可辨认的人类观众；湘北红色队服、对手白色深蓝滚边队服。所有元素保持同一赛璐璐漫画画风。
> 
> 制作：内置 image_gen 生成场景与透明动作素材，参考图为本目录副本。Remotion 编排动作格、连续篮球、篮网和声音。此样片采用有限动画关键帧，观众和其他球员作为场景层，尚不代表完成整场比赛的逐帧角色动画。
> 
> ## v2 修正
> 
> 针对配角和观众静止的问题，将五位配角从底图移除并作为独立角色层，新增每人三个姿态和不同位置轨迹。两个看台镜头新增挥毛巾、拍掌和举拳手势，并按六个区域错时切换。人物动作保持二维有限动画方式，不宣称完整逐帧或每名观众均独立绑定。

## 证据清单与缺失信息

盘点的文本/报告文件 35 个，源文本、报告及媒体产物身份摘要为 `d0b3dd197d7b04d56da9f23596194eadc29e290d97d44329396d999ecc6e00c6`。哈希证明采集内容身份，不代表其中每个断言已验证。模型权重、缓存、依赖、第三方源码树和未指定的历史会话未采集；大型文本仅做哈希并按需要选读。

- 本次整理基于明确引用的 README、制作方法、配置、提示词、关键源码与已有报告。未重新观看全部参考/候选/成片，也未做人工听辨。
- 项目未提供的原始模型内部实现、未披露生成后端、未记录 seed/参数、真实费用、人工总工时和用户最终验收，均不补造。实际日志有记录的耗时见上文，流水线耗时不等于整个制作总耗时。
- 源项目内的路径只用于查证；本档案未复制大型素材或模型，也不改变源项目运行依赖。

| 已盘点证据文件 | SHA-256（前 16 位）/状态 |
| --- | --- |
| `videos/rukawa-anime-2d-prototype-20261109/README.md` | `df0615fcd1dcd3ea` |
| `videos/rukawa-anime-2d-prototype-20261109/package-lock.json` | `fc187b28b86c9200` |
| `videos/rukawa-anime-2d-prototype-20261109/package.json` | `21a5f8cdfb916347` |
| `videos/rukawa-anime-2d-prototype-20261109/remotion.config.ts` | `6353a1053a703c9e` |
| `videos/rukawa-anime-2d-prototype-20261109/tsconfig.json` | `d5ac6d9622a45adf` |
| `videos/rukawa-anime-2d-prototype-20261109/docs/assets.json` | `0e64ceb865b4e81e` |
| `videos/rukawa-anime-2d-prototype-20261109/docs/brief.md` | `e59836cbf07cc133` |
| `videos/rukawa-anime-2d-prototype-20261109/docs/continuity-review/action-timing.json` | `9cffac1bb21fba90` |
| `videos/rukawa-anime-2d-prototype-20261109/docs/continuity-review/audit.json` | `79d4516cdbb828ca` |
| `videos/rukawa-anime-2d-prototype-20261109/docs/continuity-review/continuous-test-prompt.txt` | `7b58877de4ea2528` |
| `videos/rukawa-anime-2d-prototype-20261109/docs/prompts/court.txt` | `cb928a8529cc6290` |
| `videos/rukawa-anime-2d-prototype-20261109/docs/prompts/hoop.txt` | `e0a0f07007fc0d76` |
| `videos/rukawa-anime-2d-prototype-20261109/docs/prompts/poses-edge-cleanup.txt` | `dc4f6f94bfd96bb7` |
| `videos/rukawa-anime-2d-prototype-20261109/docs/prompts/poses.txt` | `7761f195e074a0b2` |
| `videos/rukawa-anime-2d-prototype-20261109/docs/prompts/v2/clean.txt` | `3521319bf5f0baea` |
| `videos/rukawa-anime-2d-prototype-20261109/docs/prompts/v2/courtCheer.txt` | `bae3865f24f0db3f` |
| `videos/rukawa-anime-2d-prototype-20261109/docs/prompts/v2/hoopCheer.txt` | `7794d2af9e6a16be` |
| `videos/rukawa-anime-2d-prototype-20261109/docs/prompts/v2/players.txt` | `4e4718ce9bd97dfb` |
| `videos/rukawa-anime-2d-prototype-20261109/outputs/checks/audio.json` | `2e90c3bfae7f275e` |
| `videos/rukawa-anime-2d-prototype-20261109/outputs/checks/final-check.json` | `f850621b636018be` |
| `videos/rukawa-anime-2d-prototype-20261109/scripts/analyze_support.py` | `9428dffdc0236717` |
| `videos/rukawa-anime-2d-prototype-20261109/scripts/audio.py` | `76d4567f9e4b0afb` |
| `videos/rukawa-anime-2d-prototype-20261109/scripts/check.py` | `0ef0f350dfb8c68c` |
| `videos/rukawa-anime-2d-prototype-20261109/scripts/package.py` | `fb3772840c4750af` |
| `videos/rukawa-anime-2d-prototype-20261109/scripts/run.mjs` | `7389e07380975ffe` |
| `videos/rukawa-anime-2d-prototype-20261109/src/Basketball.tsx` | `4a2a18a4af60353a` |
| `videos/rukawa-anime-2d-prototype-20261109/src/CourtShot.tsx` | `c9b048dae34a7bd6` |
| `videos/rukawa-anime-2d-prototype-20261109/src/Crowd.tsx` | `a29dbd0c7a943a3b` |
| `videos/rukawa-anime-2d-prototype-20261109/src/HoopShot.tsx` | `2f3bd7c06f743a84` |
| `videos/rukawa-anime-2d-prototype-20261109/src/Root.tsx` | `523d88b498696f83` |
| `videos/rukawa-anime-2d-prototype-20261109/src/SupportingPlayers.tsx` | `abd27c1bfb624044` |
| `videos/rukawa-anime-2d-prototype-20261109/src/index.ts` | `2e0e358282a1b998` |
| `videos/rukawa-anime-2d-prototype-20261109/src/support-bounds.json` | `391c1f7cf138eaaf` |
| `videos/rukawa-anime-2d-prototype-20261109/src/versions/v1/CourtShot.txt` | `4ceb5cb7a8d276fd` |
| `videos/rukawa-anime-2d-prototype-20261109/src/versions/v1/HoopShot.txt` | `2b6b60a826202799` |
