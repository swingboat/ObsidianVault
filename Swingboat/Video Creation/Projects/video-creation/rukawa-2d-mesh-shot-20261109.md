---
type: video-production-dossier
project: video-creation
video_id: rukawa-2d-mesh-shot-20261109
captured: '2026-11-12'
status: 当前为 2 秒微动作预览；旧 3 秒绑定样片退出活动版本
source_fingerprint: 7aa8eb763cddbec960ccd5a1a1276f3c2d4967b1a4af32fbf87c8278e8572106
tags:
- video-creation
- production-capture
---

# 流川枫 · 修正原画蓄力微动作

<!-- capture-source-sha256:7aa8eb763cddbec960ccd5a1a1276f3c2d4967b1a4af32fbf87c8278e8572106 -->

本次为指定项目的材料回溯与归档。制作结论来自项目记录/源码，未找回关闭的制作对话，也未重新执行生成、渲染、音频合成或用户验收。

## 当前结论与制作路线

**状态：当前为 2 秒微动作预览；旧 3 秒绑定样片退出活动版本。**

完整修正原画 → 局部受限微动 → 脚底锁定 → Remotion。

1280×720、30fps、60 帧。完整人物原画保持脸、手和球的轮廓，上身共用刚性变换，足部固定，只做约 5 像素重心变化。蓄力过渡在 0.2–0.7 秒，不包含完整抬臂/起跳/出手。

## 关键算法、实现与适用边界

gatherMotion.ts 给出位置相关的受限变形；CorrectedGather.tsx 重绘完整纹理及脚底接触阴影。头脸、双手和球共用刚性区域，避免拆分旋转造成手球连接错误。审计检查距离保持、脚部位移、局部变形与网格翻转。

## 声音与字幕

当前无音频。

## 失败、修正与验收边界

旧人物网格畸变已保留但退出默认导出与打包。放大微动参数不能获得可信的大幅投篮，需要新姿态或连续生成。

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

> [!info]- 当前 Composition / 时间轴源码：videos/rukawa-2d-mesh-shot-20261109/src/Root.tsx，L1–13
> ```tsx
> import React,{useEffect,useState} from 'react';
> import {AbsoluteFill,Composition,continueRender,delayRender,staticFile} from 'remotion';
> import {CorrectedGather} from './CorrectedGather';
> const GatherShot:React.FC=()=>{
>  const [handle]=useState(()=>delayRender('Chinese font'));
>  useEffect(()=>{const f=new FontFace('NotoShot',`url(${staticFile('fonts/NotoSansSC.ttf')})`);f.load().then(font=>{document.fonts.add(font);continueRender(handle);}).catch(()=>continueRender(handle));},[handle]);
>  return <AbsoluteFill style={{backgroundColor:'#e3dfd4'}}>
>   <CorrectedGather/>
>   <div style={{position:'absolute',left:42,top:35,fontFamily:'NotoShot,sans-serif',fontSize:21,color:'#202b32',letterSpacing:2}}>流川枫 · 蓄力微动作预览</div>
>   <div style={{position:'absolute',left:43,top:73,width:38,height:2,backgroundColor:'#b5242c'}}/>
>  </AbsoluteFill>;
> };
> export const Root:React.FC=()=> <Composition id="RukawaGatherPreview" component={GatherShot} width={1280} height={720} fps={30} durationInFrames={60}/>;
> ```

源时间轴、生成帧数、所用区间与最终时长分别记录；历史文档或候选参数不自动覆盖当前采用配置。

## 参考来源与借鉴说明

在本次盘点的项目自有材料中，未找到可明确归属本片的外部参考视频或 X 帖子链接；这不等于历史上没有参考。

本地造型/参考文件（本轮仅核对存在，不宣称重新看过全部图像）：

- `videos/rukawa-2d-mesh-shot-20261109/public/references/rejected-limb-pose.png`
- `videos/rukawa-2d-mesh-shot-20261109/public/references/rukawa-approved.png`

### 项目记录的技术、内容或素材来源

这些 URL 来自项目文件，尚不自动证明本片采用；除本节明确标注的访问外，本轮未逐一在线重读。

| 链接 | 记录文件 |
| --- | --- |
| [www.adobe.com](http://www.adobe.com/) | `videos/rukawa-2d-mesh-shot-20261109/docs/NOTO-OFL.txt` |
| [scripts.sil.org](https://scripts.sil.org/OFL) | `videos/rukawa-2d-mesh-shot-20261109/docs/NOTO-OFL.txt` |

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

> [!info]- videos/rukawa-2d-mesh-shot-20261109/src/gatherMotion.ts，L1–19
> ```typescript
> export type Point={x:number;y:number};
> export const ART_SCALE=.35;
> export const ART_ORIGIN={x:735,y:113};
> const clamp=(x:number)=>Math.max(0,Math.min(1,x));
> export const smooth=(a:number,b:number,t:number)=>{const u=clamp((t-a)/(b-a));return u*u*u*(10+u*(-15+6*u));};
> export const loadAt=(t:number)=>smooth(.2,.7,t)*(1-smooth(1.2,1.85,t));
> export const basePoint=(p:Point):Point=>({x:ART_ORIGIN.x+p.x*ART_SCALE,y:ART_ORIGIN.y+p.y*ART_SCALE});
> export function deformPoint(p:Point,t:number):Point{
>  const b=basePoint(p),load=loadAt(t),pivot=basePoint({x:550,y:900}),angle=-.006*load,c=Math.cos(angle),s=Math.sin(angle),dx=b.x-pivot.x,dy=b.y-pivot.y;
>  const upper={x:pivot.x+dx*c-dy*s-.5*load,y:pivot.y+dx*s+dy*c+5*load};
>  const bodyWeight=1-smooth(900,1290,p.y);
>  const kneeWeight=smooth(885,1010,p.y)*(1-smooth(1080,1290,p.y));
>  // Every face, finger, palm and basketball pixel shares one rigid transform.
>  const out={x:b.x+(upper.x-b.x)*bodyWeight-1.6*load*kneeWeight,y:b.y+(upper.y-b.y)*bodyWeight};
>  // A subpixel fold movement is confined to the shorts, below the hands.
>  const clothMask=smooth(690,725,p.y)*(1-smooth(840,875,p.y))*smooth(430,475,p.x)*(1-smooth(730,770,p.x));
>  out.x+=.25*load*clothMask*Math.sin((p.y-690)/185*Math.PI);
>  return out;
> }
> ```

> [!info]- videos/rukawa-2d-mesh-shot-20261109/src/CorrectedGather.tsx，L1–35
> ```tsx
> import React,{useEffect,useLayoutEffect,useRef,useState} from 'react';
> import {continueRender,delayRender,staticFile,useCurrentFrame,useVideoConfig} from 'remotion';
> import {deformPoint,Point,loadAt} from './gatherMotion';
> const W=1280,H=720;
> type GPU={gl:WebGLRenderingContext;program:WebGLProgram;buffer:WebGLBuffer;art:WebGLTexture;court:WebGLTexture;position:number;uv:number};
> const shader=(gl:WebGLRenderingContext,type:number,source:string)=>{const s=gl.createShader(type)!;gl.shaderSource(s,source);gl.compileShader(s);if(!gl.getShaderParameter(s,gl.COMPILE_STATUS))throw Error(gl.getShaderInfoLog(s)??'Shader compile error');return s;};
> function init(canvas:HTMLCanvasElement,images:HTMLImageElement[]):GPU{
>  const gl=canvas.getContext('webgl',{alpha:true,premultipliedAlpha:true,preserveDrawingBuffer:true,antialias:true});if(!gl)throw Error('WebGL is required');
>  const program=gl.createProgram()!;
>  gl.attachShader(program,shader(gl,gl.VERTEX_SHADER,'attribute vec2 aPosition;attribute vec2 aUV;varying vec2 uv;void main(){gl_Position=vec4(aPosition.x/640.-1.,1.-aPosition.y/360.,0.,1.);uv=aUV;}'));
>  gl.attachShader(program,shader(gl,gl.FRAGMENT_SHADER,'precision mediump float;varying vec2 uv;uniform sampler2D tex;uniform float shadowAlpha;void main(){if(shadowAlpha>0.){vec2 q=(uv-.5)*2.;float a=shadowAlpha*pow(max(0.,1.-dot(q,q)),2.);gl_FragColor=vec4(vec3(.10,.08,.06)*a,a);}else{gl_FragColor=texture2D(tex,uv);}}'));
>  gl.linkProgram(program);if(!gl.getProgramParameter(program,gl.LINK_STATUS))throw Error('Shader link error');gl.useProgram(program);gl.enable(gl.BLEND);gl.blendFunc(gl.ONE,gl.ONE_MINUS_SRC_ALPHA);gl.pixelStorei(gl.UNPACK_PREMULTIPLY_ALPHA_WEBGL,1);
>  const textures=images.map(im=>{const tex=gl.createTexture()!;gl.bindTexture(gl.TEXTURE_2D,tex);gl.texParameteri(gl.TEXTURE_2D,gl.TEXTURE_WRAP_S,gl.CLAMP_TO_EDGE);gl.texParameteri(gl.TEXTURE_2D,gl.TEXTURE_WRAP_T,gl.CLAMP_TO_EDGE);gl.texParameteri(gl.TEXTURE_2D,gl.TEXTURE_MIN_FILTER,gl.LINEAR);gl.texParameteri(gl.TEXTURE_2D,gl.TEXTURE_MAG_FILTER,gl.LINEAR);gl.texImage2D(gl.TEXTURE_2D,0,gl.RGBA,gl.RGBA,gl.UNSIGNED_BYTE,im);return tex;});
>  return {gl,program,buffer:gl.createBuffer()!,art:textures[0],court:textures[1],position:gl.getAttribLocation(program,'aPosition'),uv:gl.getAttribLocation(program,'aUV')};
> }
> function mesh(g:GPU,rect:number[],step:number,map:(v:Point)=>Point,texture:WebGLTexture,tw:number,th:number,shadowAlpha=0){
>  const [x,y,w,h]=rect,nx=Math.ceil(w/step),ny=Math.ceil(h/step),data:number[]=[];
>  const vertex=(i:number,j:number)=>{const p={x:x+w*i/nx,y:y+h*j/ny},q=map(p);data.push(q.x,q.y,p.x/tw,p.y/th);};
>  for(let j=0;j<ny;j++)for(let i=0;i<nx;i++){vertex(i,j);vertex(i+1,j);vertex(i,j+1);vertex(i+1,j);vertex(i+1,j+1);vertex(i,j+1);}
>  const {gl}=g;gl.uniform1f(gl.getUniformLocation(g.program,'shadowAlpha'),shadowAlpha);gl.bindTexture(gl.TEXTURE_2D,texture);gl.bindBuffer(gl.ARRAY_BUFFER,g.buffer);gl.bufferData(gl.ARRAY_BUFFER,new Float32Array(data),gl.DYNAMIC_DRAW);gl.enableVertexAttribArray(g.position);gl.vertexAttribPointer(g.position,2,gl.FLOAT,false,16,0);gl.enableVertexAttribArray(g.uv);gl.vertexAttribPointer(g.uv,2,gl.FLOAT,false,16,8);gl.drawArrays(gl.TRIANGLES,0,data.length/4);
> }
> export const CorrectedGather:React.FC=()=>{
>  const frame=useCurrentFrame(),{fps}=useVideoConfig(),canvas=useRef<HTMLCanvasElement>(null),gpu=useRef<GPU|null>(null),handle=useRef<number|null>(null),[images,setImages]=useState<HTMLImageElement[]|null>(null);
>  if(handle.current===null&&!images)handle.current=delayRender('Loading corrected character art');
>  useEffect(()=>{let active=true;Promise.all(['art/correctives/rukawa-gather-anatomy-v1.png','art/court-background.png'].map(file=>new Promise<HTMLImageElement>((resolve,reject)=>{const im=new Image();im.onload=()=>resolve(im);im.onerror=reject;im.src=staticFile(file);}))).then(im=>{if(active)setImages(im);});return()=>{active=false;};},[]);
>  useLayoutEffect(()=>{
>   if(!images||!canvas.current)return;if(!gpu.current)gpu.current=init(canvas.current,images);const g=gpu.current,{gl}=g,t=frame/fps;gl.viewport(0,0,W,H);gl.clearColor(.91,.88,.83,1);gl.clear(gl.COLOR_BUFFER_BIT);
>   const bg=images[1],crop=350;mesh(g,[0,crop,bg.width,bg.height-crop],128,p=>({x:W-p.x/bg.width*W,y:(p.y-crop)/(bg.height-crop)*H}),g.court,bg.width,bg.height);
>   // Separate static contact shadows for the staggered feet.
>   for(const sh of [{x:842,y:631,w:91,h:13},{x:974,y:648,w:87,h:14}])mesh(g,[0,0,1,1],1,p=>({x:sh.x+(p.x-.5)*sh.w,y:sh.y+(p.y-.5)*sh.h}),g.art,1,1,.20+.025*loadAt(t));
>   mesh(g,[0,0,images[0].width,images[0].height],22,p=>deformPoint(p,t),g.art,images[0].width,images[0].height);
>   gl.flush();if(handle.current!==null){continueRender(handle.current);handle.current=null;}
>  },[frame,fps,images]);
>  return <canvas ref={canvas} width={W} height={H} style={{position:'absolute',width:'100%',height:'100%'}}/>;
> };
> ```

## 完整关键提示词与生成记录

以下保留项目自有提示词、候选参数与版本。存在失败候选和历史参数；最终采用以 selected-takes、edit-decisions 与制作复盘为准，不能将所有候选都写成已采用。

> [!info]- videos/rukawa-2d-mesh-shot-20261109/docs/atlas-prompt.txt
> ```text
> Use case: illustration-story. Asset type: a transparent production cutout atlas for a LOCAL 2D skeletal/mesh animation, NOT a finished scene and NOT a sequence of poses.
> Input image 1 is the approved character/design reference. Preserve the SAME Kaede Rukawa from Slam Dunk: teenage Japanese basketball player, lean long-limbed build, sharp quiet face, straight black fringe and navy cel highlights, red loose SHOHOKU #11 sleeveless jersey, loose red basketball shorts with white/black edging, white socks and white/black basketball sneakers. Match the attractive hand-inked 1990s basketball anime cel artwork of the reference, strong clean thin contour, restrained cel shadows, warm natural skin, NOT 3D, NOT a muscular adult.
> Create ONE large transparent atlas, landscape 3 columns by 2 rows of equal cells, with generous empty padding BETWEEN parts. No labels, no grid marks, no whole assembled character, no extra figures. All parts use the same 3/4 FRONT view with the face, feet and shooting direction turned towards SCREEN LEFT. Use the same drawn body proportions, scale and lighting for parts.
> Top left cell: HEAD + NECK + entire TORSO + loose SHORTS as ONE connected piece, NO arms, NO legs. Face and bangs exactly match approved reference. Include rounded skin shoulder sockets hidden beneath the sleeveless jersey trim. Include enough neck. Torso neutral upright. Jersey text SHOHOKU and 11 clearly drawn. Shorts roomy with deep folds, hem ending above knees. Top right and top middle cells: the TWO separate complete continuous arms, one per cell, each shoulder through elbow through wrist and ALL FIVE natural fingers. Both arms drawn nearly straight vertically downward, palms open, fingers pointing downward, subtle relaxed bend at elbow only. The near arm has the black wrist band seen in the reference. The far arm is slightly slimmer from perspective. Include rounded shoulder cap and some generous overlap skin. Do not draw torso attached to these arms. These are complete connected arm cutouts, not disconnected upper/forearm pieces.
> Bottom left cell: near/front continuous LEG from upper thigh through straight knee through shin, white sock and left-facing basketball SHOE, all ONE connected piece. Bottom middle cell: the other/far continuous LEG and shoe, same design and similar scale, ONE connected piece. Keep both leg cutouts close to vertical, slightly relaxed knees, all foot details clean. Upper thigh continues above shorts hem for overlap. Bottom right cell: ONE clean orange basketball with inked channels, no hands or other objects.
> Constraints: genuine clean alpha transparency around every piece, no background paint or ground shadows, no shadow blobs, no annotation, no disconnected missing fingers, no mannequin anatomy, no gradient 3D rendering. Finished beautifully inked colored anime artwork, clean enough to assemble for continuous 2D mesh skinning. The cell arrangement is strict; keep each piece fully contained in its own cell with generous transparent padding.
> ```

> [!info]- videos/rukawa-2d-mesh-shot-20261109/docs/corrective-pose-prompt.txt
> ```text
> Use case: illustration-story. Asset type: ONE anatomy-corrected full-body key drawing for a future hand-drawn basketball animation, on genuine transparent alpha.
> Input image 1 is the APPROVED appearance/style reference for Kaede Rukawa from Slam Dunk. Preserve his teenage lean athletic proportions, exact black fringe and navy highlights, quiet sharp face, red loose SHOHOKU number 11 uniform, white/black trim, white socks, white-and-black sneakers and near-wrist black band. Match the attractive thin ink lines and restrained 1990s anime cel coloring of reference 1.
> Input image 2 is a REJECTED animation frame. It shows the approximate left-facing three-quarter staging and basketball gather, but its hands and legs are visibly distorted. DO NOT reproduce its wrist angles, leg curves, limb proportions or close-together feet.
> Draw the entire character afresh as ONE anatomically coherent connected person. A basketball shooting coach should find the stance plausible:
> - Three-quarter front view facing SCREEN LEFT. All of the head, both hands, both legs and both shoes fit comfortably in frame. Keep the body lean, not bulky.
> - Balanced ready-to-shoot gather position, a SMALL natural knee bend, hips back slightly, torso leaning forward only a little. Thigh and shin are straight bone segments joined at a believable knee, with clear natural kneecap anatomy. No rubbery double bends or S-shaped shins. The knees track over the feet.
> - Both soles planted on one level ground plane; feet comfortably about shoulder-width apart, slightly staggered in the shooting direction, with a visible gap between shoes. Do not balance on tiptoe. Do not make the lower legs unnaturally long or narrow.
> - Hold ONE basketball at the LOWER CHEST rather than down at the hips. Elbows below the ball and close to the ribs, shoulders relaxed. The shooting hand supports the underside/back of the ball; the guide hand rests lightly on its side. Fingers curve naturally over the spherical surface and may be partly occluded. Natural hand size, convincing thumb placement, normal wrist alignment. No flat vertical paddle palm, no bent-pipe forearm, no right-angle wrist, no extra or missing fingers.
> - All shoulders, elbows, wrists, hips, knees and ankles connect clearly and correctly. No limb crosses through the torso. Baggy shorts drape naturally over the upper thighs with clean consistent hems.
> Composition: one large complete full-body character and basketball, centered, with ample transparent padding. No other figures, no poses sheet, no detached body parts, no labels, no captions, no gym background, no ground shadow. Finished beautiful 2D anime cel artwork, not a 3D mannequin. This is a corrected static keyframe, not a request to distort or mechanically warp the previous picture.
> ```

## 已有产物与检查依据

文件存在、技术检查、画面检查与用户验收分别看待。下表是本次文件盘点，旧版与当前版可能共存。MP4/HTML 按内容计算 SHA-256，ZIP 仅记录文件大小和修改时间供增量判断。

| 本地产物 | 字节数 | SHA-256 |
| --- | ---: | --- |
| `videos/rukawa-2d-mesh-shot-20261109/outputs/rukawa-2d-mesh-project.zip` | 34906142 | `ZIP：仅元数据` |
| `videos/rukawa-2d-mesh-shot-20261109/outputs/rukawa-2d-mesh-test-3s.mp4` | 951514 | `5f5c9aec9dec5a37cedde518ecb612c7c8625bf08bc629aa00dd3febca750ff6` |
| `videos/rukawa-2d-mesh-shot-20261109/outputs/rukawa-corrected-gather-2s.mp4` | 524947 | `a28f33c2c380c8b95d6ed6a8f7f57380aaf89a72ec34d080cb8b51b7da0487b9` |
| `videos/rukawa-2d-mesh-shot-20261109/outputs/rukawa-corrected-gather-project.zip` | 21743552 | `ZIP：仅元数据` |

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
>       "duration": "2.000000",
>       "nb_frames": "60",
>       "side_data_list": [
>         {}
>       ]
>     }
>   ],
>   "format": {
>     "duration": "2.000000",
>     "size": "524947"
>   },
>   "file": "outputs/rukawa-corrected-gather-2s.mp4",
>   "scope": "本次只读取媒体头；未全片解码、播放或重新进行视觉/听觉验收。"
> }
> ```


已有检查报告路径（未在本轮重新执行）：

- `videos/rukawa-2d-mesh-shot-20261109/outputs/checks/corrected-gather-audit.json`
- `videos/rukawa-2d-mesh-shot-20261109/outputs/checks/corrected-project-check.json`
- `videos/rukawa-2d-mesh-shot-20261109/outputs/checks/project-check.json`
- `videos/rukawa-2d-mesh-shot-20261109/outputs/checks/rig-audit.json`

## 制作方法与迭代记录选编

下面是项目自有文档的归档摘录，保留原阶段与原检查表述，个人路径已泛化、相对链接已注明源项目位置。它们属于来源陈述，不代表本次重新完成其中的生成或验收。

> [!info]- 来源文档：videos/rukawa-2d-mesh-shot-20261109/README.md
> # 流川枫 · 修正原画蓄力微动作预览
> 
> 当前活动版本使用用户确认的完整修正图 `public/art/correctives/rukawa-gather-anatomy-v1.png`，输出 2 秒、1280×720、30 fps 的小幅动作预览。
> 
> 本轮替换了旧版畸变的人物，保留原画中的脸、手型、篮球和腿部轮廓。头部、双手与篮球共用刚性变换，脚部固定，身体仅有约 5 像素的轻微重心变化。蓄力过渡发生在 0.2–0.7 秒，随后恢复。它不包含完整抬臂、起跳或出手，也不是完整 20 秒比赛视频。那些动作仍需补绘相应姿态和过渡。
> 
> 旧版样片和失败的绑定代码保留在工作区，已退出默认预览、导出与本轮打包流程。
> 
> ## 独立安装与预览
> 
> 需要 Node.js 22.18+、系统 Google Chrome。FFmpeg/ffprobe 用于文件检查。
> 
> ```sh
> cd rukawa-2d-mesh-shot-20261109
> mkdir -p .cache/tmp .cache/npm
> TMPDIR="$PWD/.cache/tmp" TMP="$PWD/.cache/tmp" TEMP="$PWD/.cache/tmp" npm ci --cache .cache/npm
> npm run dev
> ```
> 
> 打开 http://localhost:3095/RukawaGatherPreview 。所有运行素材在本项目中，预览不调用在线服务。
> 
> ## 导出、检查与打包
> 
> ```sh
> npm run render
> npm run check
> npm run still -- outputs/checks/review.png --frame=25
> npm run package
> ```
> 
> 当前视频：`outputs/rukawa-corrected-gather-2s.mp4`。
> 
> 独立工程包：`outputs/rukawa-corrected-gather-project.zip`。只包含当前活动版本的源代码、素材、依赖锁文件、视频和检查报告。旧版失败视频、旧绑定代码、node_modules 和缓存不进入本轮压缩包。
> 
> `run.mjs` 固定当前工作目录，将 TMPDIR、TMP、TEMP、XDG_CACHE_HOME、npm cache 指向本视频目录，使用系统 Chrome，不下载浏览器。
> 
> ## 修改素材、动作、声音和时间轴
> 
> - `src/gatherMotion.ts`：重心变化、衣物微动和足部锁定；头脸、双手与球所在区域保持刚性。大幅度动作不应通过放大这些参数来实现。
> - `src/CorrectedGather.tsx`：完整原画纹理、球馆背景、脚底阴影与绘制。前景只读取一张完整修正图。
> - `src/Root.tsx`：2 秒时间轴、标题和画幅。修改时长后同步检查脚本里的帧数和持续时间。
> - `public/art/correctives/rukawa-gather-anatomy-v1.png`：已确认修正图。请保留透明通道和完整人物。
> - `docs/corrective-pose-prompt.txt`：生成修正图的完整提示词，使用内置 image_gen；本轮没有重新生成该图。
> - 当前无音频。需要音效或配音时，将有权使用的文件放在本项目 `public/audio/`，通过 `@remotion/media` 的 Audio 引用，并按新时间轴定位。
> 
> ## 验证范围与素材来源
> 
> `outputs/checks/corrected-gather-audit.json` 检查上身距离保持、脚部位移、局部变形幅度及网格翻转；`corrected-project-check.json` 检查类型、当前运行路径与视频解码。数值检查不能替代对画面自然程度的观看判断。
> 
> 背景是本项目内的独立球馆插画副本，当前画面裁切到看台以下。Noto Sans SC 许可见 `docs/NOTO-OFL.txt`。内置图像工具保留的生成源位置见 `docs/generation-origin.md`，不构成运行依赖。

> [!info]- 来源文档：videos/rukawa-2d-mesh-shot-20261109/docs/corrected-gather-notes.md
> # 本轮修正
> 
> 按用户选定的 `rukawa-gather-anatomy-v1.png` 改善视频。
> 
> - 当前前景以一个完整人物绘制，手指、手掌、球、面部不再分别旋转或重新拼接。
> - 全身只进行受限的小幅重心变化；双脚固定，衣物有亚像素位移。
> - 新增构图与修正图中的左右脚分别对应的接触阴影。
> - 活动合成：RukawaGatherPreview，2 秒 / 60 帧。
> - 旧的 Rukawa2DMesh 和 3 秒失败样片不再作为活动交付。
> 
> 适用范围：静态修正图的小幅动作验证。尚未制作新的出手原画、起跳原画、观众或其他球员动作，完整 20 秒绝杀视频仍未完成。

> [!info]- 来源文档：videos/rukawa-2d-mesh-shot-20261109/docs/anatomy-review.md
> # 肢体问题与修正方案
> 
> 状态：原 3 秒网格样片已被用户否决，不作为合格动画交付。
> 
> 核对 `src/rig.ts`：持球初始近侧手掌方向与前臂方向约相差 102.1 度，15 帧处约 102.4 度；手型是同一张伸手图旋转，没有握球的手指造型修正。腿部只有二维双四元数的关节权重插值，没有弯膝轮廓校正。骨长已有约束，单加骨长检查不能消除皮肤轮廓畸变。
> 
> 本轮新增：`public/art/correctives/rukawa-gather-anatomy-v1.png`。内置 image_gen 根据已确认造型参考及用户失败截图，新绘一张完整持球准备姿态。手指随球面弯曲，双脚分开，膝盖按正常屈膝形态绘制。它是静态候选基准图，尚未成为动画绑定或连续动作的证明。
> 
> 后续方法：
> 1. 先以完整姿态原画确定持球、蓄力、出手、落地的正确人体结构，再从统一比例的原画制作分层素材。
> 2. 小幅动作使用受限绑定；大角度弯曲加入对应的校正原画与过渡帧，手掌/手指独立修绘。
> 3. 先检查静态轮廓、手球接触与重心，再验证 0.5 秒短过渡。数值连续性不能代替视觉验收。
> 4. 本地制作的自然程度与 20 秒多人动画仍未得到验证，不作效果保证。
> 
> 完整提示词：`docs/corrective-pose-prompt.txt`。

> [!info]- 来源文档：videos/rukawa-2d-mesh-shot-20261109/docs/generation-origin.md
> Built-in image_gen generated atlas at <本机路径> Copied into public/art/rukawa-layer-atlas-v1.png. Provider original retained. No runtime dependency on provider folder.
> 
> Anatomy corrective generated by built-in image_gen at <本机路径> Copied into public/art/correctives/rukawa-gather-anatomy-v1.png; provider original retained. No runtime dependency on that provider path.

## 证据清单与缺失信息

盘点的文本/报告文件 31 个，源文本、报告及媒体产物身份摘要为 `7aa8eb763cddbec960ccd5a1a1276f3c2d4967b1a4af32fbf87c8278e8572106`。哈希证明采集内容身份，不代表其中每个断言已验证。模型权重、缓存、依赖、第三方源码树和未指定的历史会话未采集；大型文本仅做哈希并按需要选读。

- 本次整理基于明确引用的 README、制作方法、配置、提示词、关键源码与已有报告。未重新观看全部参考/候选/成片，也未做人工听辨。
- 项目未提供的原始模型内部实现、未披露生成后端、未记录 seed/参数、真实费用、人工总工时和用户最终验收，均不补造。实际日志有记录的耗时见上文，流水线耗时不等于整个制作总耗时。
- 源项目内的路径只用于查证；本档案未复制大型素材或模型，也不改变源项目运行依赖。

| 已盘点证据文件 | SHA-256（前 16 位）/状态 |
| --- | --- |
| `videos/rukawa-2d-mesh-shot-20261109/README.md` | `c04cff658e255e30` |
| `videos/rukawa-2d-mesh-shot-20261109/package-lock.json` | `7e67911fc466c674` |
| `videos/rukawa-2d-mesh-shot-20261109/package.json` | `0e5fe448b41eea8b` |
| `videos/rukawa-2d-mesh-shot-20261109/remotion.config.ts` | `ec70b0acd4585a5d` |
| `videos/rukawa-2d-mesh-shot-20261109/tsconfig.json` | `af262af0dd89b1e2` |
| `videos/rukawa-2d-mesh-shot-20261109/docs/NOTO-OFL.txt` | `1c05c68c34f97084` |
| `videos/rukawa-2d-mesh-shot-20261109/docs/anatomy-review.md` | `b4306b58df618113` |
| `videos/rukawa-2d-mesh-shot-20261109/docs/asset-request.md` | `0ec058f6f75b6542` |
| `videos/rukawa-2d-mesh-shot-20261109/docs/atlas-prompt.txt` | `936849e5ff05a047` |
| `videos/rukawa-2d-mesh-shot-20261109/docs/corrected-gather-notes.md` | `434049ce9abc4521` |
| `videos/rukawa-2d-mesh-shot-20261109/docs/corrective-pose-prompt.txt` | `4a3f5d9cae0d34f8` |
| `videos/rukawa-2d-mesh-shot-20261109/docs/generation-origin.md` | `bf0f3ee9e5c19c58` |
| `videos/rukawa-2d-mesh-shot-20261109/docs/rejected/Root-before-corrected.txt` | `b43004ce84c4a8b9` |
| `videos/rukawa-2d-mesh-shot-20261109/docs/rejected/audit-before-corrected.txt` | `f3f83023e4640e1a` |
| `videos/rukawa-2d-mesh-shot-20261109/docs/rejected/check-before-corrected.txt` | `63e334e67acee656` |
| `videos/rukawa-2d-mesh-shot-20261109/outputs/checks/corrected-gather-audit.json` | `904a45baf5541450` |
| `videos/rukawa-2d-mesh-shot-20261109/outputs/checks/corrected-project-check.json` | `c40059d5ef9da0e7` |
| `videos/rukawa-2d-mesh-shot-20261109/outputs/checks/pose-samples.json` | `760c9216894ad3ed`；内容超限未展开 |
| `videos/rukawa-2d-mesh-shot-20261109/outputs/checks/project-check.json` | `7d4d7a5ce979605f` |
| `videos/rukawa-2d-mesh-shot-20261109/outputs/checks/rig-audit.json` | `c1048a0bc516a058` |
| `videos/rukawa-2d-mesh-shot-20261109/scripts/audit-corrected.mjs` | `32cf48dbf9537628` |
| `videos/rukawa-2d-mesh-shot-20261109/scripts/audit.mjs` | `f3f83023e4640e1a` |
| `videos/rukawa-2d-mesh-shot-20261109/scripts/check.py` | `95acb8ded5515552` |
| `videos/rukawa-2d-mesh-shot-20261109/scripts/package.py` | `ec2de58102a0a427` |
| `videos/rukawa-2d-mesh-shot-20261109/scripts/run.mjs` | `0517b4e9ed2f723e` |
| `videos/rukawa-2d-mesh-shot-20261109/src/CorrectedGather.tsx` | `54d4c064e1bd54fb` |
| `videos/rukawa-2d-mesh-shot-20261109/src/Root.tsx` | `1dcafe0a6ef5b4f2` |
| `videos/rukawa-2d-mesh-shot-20261109/src/SceneCanvas.tsx` | `1d23b3ef42fb5159` |
| `videos/rukawa-2d-mesh-shot-20261109/src/gatherMotion.ts` | `2a800d53177ffa57` |
| `videos/rukawa-2d-mesh-shot-20261109/src/index.ts` | `309d14a1bb4ad80b` |
| `videos/rukawa-2d-mesh-shot-20261109/src/rig.ts` | `5cbc09a9503e888b` |
