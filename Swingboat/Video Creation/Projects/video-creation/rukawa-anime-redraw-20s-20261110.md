---
type: video-production-dossier
project: video-creation
video_id: rukawa-anime-redraw-20s-20261110
captured: '2026-11-12'
status: 已有 20 秒重制；含明确慢放段
source_fingerprint: 64643f16827d4bae08d63b595feb63f016485f037aec50da985c791da0d2e620
tags:
- video-creation
- production-capture
---

# 流川枫 · 完整同框原画与连续视频重制

<!-- capture-source-sha256:64643f16827d4bae08d63b595feb63f016485f037aec50da985c791da0d2e620 -->

本次为指定项目的材料回溯与归档。制作结论来自项目记录/源码，未找回关闭的制作对话，也未重新执行生成、渲染、音频合成或用户验收。

> 本页的环境日志与模型文件清单已删节。

## 当前结论与制作路线

**状态：已有 20 秒重制；含明确慢放段。**

imagegen 同框原画 → LTX-2.3 q4/MLX → 短动作选片 → 程序篮筐修正。

1280×720、24fps、480 帧。人物与球馆在完整原画中统一，连续表演使用本机图生视频。正式投篮和庆祝等存在 2 倍慢放、篮筐段约 2.5 倍慢放；与后来的自然节奏版区分。

## 关键算法、实现与适用边界

lowram_prompt.py 先 Gemma 编码并 materialize 隐藏状态，释放文本权重后加载 connector 投影，NPZ 缓存提示词向量，再逐块读取视频模型。BasketAction.tsx 的篮球入网前用二次贝塞尔，之后二次下降；16 列×8 层网格用接触包络和指数衰减正弦项回弹。绘制后网、球、前网、篮圈前沿，遮罩剔除生成片中的重复球和变形篮网。

## 声音与字幕

保留镜头环境声，合成终场/入网声；final-audio.py 调响度并复制视频流。变速阶段音频 tempo 与视频 timestamps 联动，不等于光流补帧。

## 失败、修正与验收边界

长动作人物融合、金属篮圈/球网变形、庆祝人物交叉、float16 GPU 错误分别由短候选选片、程序修正、固定间距原画和退回已跑通计算设置解决。慢放改善了可用时长但后来被认为节奏偏慢。

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
> -e file://video-creation/videos/rukawa-anime-redraw-20s-20261110/.cache/vendor/ltx-2-mlx/packages/ltx-core-mlx
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

> [!info]- 当前 Composition / 时间轴源码：videos/rukawa-anime-redraw-20s-20261110/src/Root.tsx，L1–34
> ```tsx
> import React from 'react';
> import {Audio} from '@remotion/media';
> import {staticFile} from 'remotion';
> import {AbsoluteFill,Composition,Folder,Series,interpolate,useCurrentFrame,useVideoConfig} from 'remotion';
> import {OpeningScene} from './OpeningScene';import {ShotScene,PilotScene} from './ShotScene';import {RimScene} from './RimScene';import {CelebrationScene} from './CelebrationScene';import {HeroScene} from './HeroScene';import {useLocalFont} from './font';import {BUZZER_SECONDS,SCORE_SECONDS} from './timing';import {clipsReady} from './media-status';
> const Film=()=>{
>  const frame=useCurrentFrame(),{fps}=useVideoConfig(),t=frame/fps;useLocalFont();
>  const complete=clipsReady.close&&clipsReady.shot&&clipsReady.rim&&clipsReady.celebrate&&clipsReady.hero;
>  const score=t>=SCORE_SECONDS?80:77,remaining=Math.max(0,2.4*(1-t/BUZZER_SECONDS));
>  return <AbsoluteFill style={{backgroundColor:'#193b3d',fontFamily:'Arena,sans-serif'}}>
>   <Series>
>    <Series.Sequence name="屏息 · 最后一投" durationInFrames={72} premountFor={fps}><OpeningScene/></Series.Sequence>
>    <Series.Sequence name="蓄力 · 起跳 · 封盖 · 出手" durationInFrames={96} premountFor={fps}><ShotScene/></Series.Sequence>
>    <Series.Sequence name="压哨 · 篮球入网" durationInFrames={120} premountFor={fps}><RimScene/></Series.Sequence>
>    <Series.Sequence name="湘北 · 获胜庆祝" durationInFrames={96} premountFor={fps}><CelebrationScene/></Series.Sequence>
>    <Series.Sequence name="流川枫 · 终场" durationInFrames={96} premountFor={fps}><HeroScene/></Series.Sequence>
>   </Series>
>   {complete?<Audio src={staticFile('audio/buzzer.wav')} from={Math.round(BUZZER_SECONDS*fps)} premountFor={fps} volume={.85}/>:null}
>   {complete?<div style={{position:'absolute',left:28,top:24,padding:'8px 13px',backgroundColor:'#132c30db',color:'#fff2da',fontSize:19,display:'flex',gap:18,borderBottom:'2px solid #a92b30'}}><span>湘北 <b style={{color:t>=SCORE_SECONDS?'#f5cf8e':'#fff2da'}}>{score}</b> : 78 陵南</span><span style={{fontFamily:'Arial,sans-serif',color:'#f5cf8e',fontVariantNumeric:'tabular-nums'}}>{t>=BUZZER_SECONDS?'00.0':(Math.ceil(remaining*10)/10).toFixed(1).padStart(4,'0')}</span></div>:null}
>   {complete&&t>=16?<AbsoluteFill style={{background:'linear-gradient(90deg,#122e33bc,transparent 65%)',pointerEvents:'none'}}/>:null}
>   {complete&&t>=16?<div style={{position:'absolute',left:42,top:330,color:'#fff0d7',opacity:interpolate(frame,[16*fps,16.4*fps],[0,1],{extrapolateLeft:'clamp',extrapolateRight:'clamp'})}}><div style={{fontSize:43,letterSpacing:5}}>流川枫</div><div style={{height:3,width:50,background:'#bd3437',margin:'18px 0'}}/><div style={{fontSize:24,letterSpacing:3}}>压哨三分</div><div style={{fontSize:20,marginTop:11}}>80 : 78　湘北获胜</div></div>:null}
>  </AbsoluteFill>;
> };
> export const Root=()=> <>
>  <Composition id="RukawaRemake" component={Film} width={1280} height={720} fps={24} durationInFrames={480}/>
>  <Folder name="Scenes">
>   <Composition id="Opening" component={OpeningScene} width={1280} height={720} fps={24} durationInFrames={72}/>
>   <Composition id="Shot" component={ShotScene} width={1280} height={720} fps={24} durationInFrames={96}/>
>   <Composition id="Rim" component={RimScene} width={1280} height={720} fps={24} durationInFrames={120}/>
>   <Composition id="Celebration" component={CelebrationScene} width={1280} height={720} fps={24} durationInFrames={96}/>
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
| [www.adobe.com](http://www.adobe.com/) | `videos/rukawa-anime-redraw-20s-20261110/docs/NOTO-OFL.txt` |
| [scripts.sil.org](https://scripts.sil.org/OFL) | `videos/rukawa-anime-redraw-20s-20261110/docs/NOTO-OFL.txt` |
| [github.com](https://github.com/dgrauet/ltx-2-mlx) | `videos/rukawa-anime-redraw-20s-20261110/docs/production-method.md` |
| [github.com](https://github.com/dgrauet/ltx-2-mlx/blob/main/docs/PIPELINE_MATURITY.md) | `videos/rukawa-anime-redraw-20s-20261110/docs/production-method.md` |
| [huggingface.co](https://huggingface.co/dgrauet/ltx-2.3-mlx-q4) | `videos/rukawa-anime-redraw-20s-20261110/docs/production-method.md` |

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

> [!info]- videos/rukawa-anime-redraw-20s-20261110/src/BasketAction.tsx，L1–59
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
>  const frame=useCurrentFrame(),{fps}=useVideoConfig(),t=frame/fps,ball=ballPosition(t),hole=nativeBall(t);
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
>   <Audio src={staticFile('audio/net.wav')} from={2*fps} premountFor={fps} volume={.85}/>
>  </AbsoluteFill>;
> };
> ```

> [!info]- videos/rukawa-anime-redraw-20s-20261110/scripts/lowram_prompt.py，L1–44
> ```python
> """Sequential Gemma / connector lifetime for this 18GB host.
> 
> Uses the upstream encoder, weights and projection unchanged. Materializes
> Gemma hidden states, releases the language model, then loads the connector.
> The upstream default keeps both weight sets resident at once.
> """
> import hashlib,os
> import mlx.core as mx
> from ltx_core_mlx.model.transformer.model import LTXModelConfig
> from ltx_core_mlx.text_encoders.gemma.encoders.base_encoder import GemmaLanguageModel
> from ltx_core_mlx.text_encoders.gemma.encoders.encoder_configurator import select_text_encoder
> from ltx_core_mlx.text_encoders.gemma.feature_extractor import GemmaFeaturesExtractorV2
> from ltx_core_mlx.utils.memory import aggressive_cleanup
> from ltx_core_mlx.utils.weights import load_split_safetensors
> from ltx_pipelines_mlx.utils.blocks import PromptEncoder
> 
> def install(root):
>  cache=root/'.cache/prompt-embeddings';cache.mkdir(parents=True,exist_ok=True)
>  def deferred_load(self):
>   if select_text_encoder(self.model_dir)!='gemma3':raise ValueError('This project adapter is pinned to the Gemma 3 LTX-2.3 pack')
>   self._encoder_kind='gemma3'
>  def sequential_encode(self,prompt):
>   key=hashlib.sha256(('LTX2.3-f306259-core0.18.0-sequential-v1:'+prompt).encode()).hexdigest()
>   path=cache/(key+'.npz')
>   if path.exists():
>    values=mx.load(str(path));mx.eval(values['video'],values['audio']);print('Loaded cached prompt embeddings',flush=True);return values['video'],values['audio']
>   deferred_load(self)
>   print('Encoding prompt with Gemma only',flush=True)
>   self._text_encoder=GemmaLanguageModel();self._text_encoder.load(self.gemma_model_id)
>   states,mask=self._text_encoder.encode_all_layers(prompt,max_length=int(os.environ.get('LTX2_GEMMA_MAX_LENGTH','1024')))
>   mx.eval(*states,mask)
>   self._text_encoder=None;aggressive_cleanup()
>   print('Projecting materialized hidden states with connector only',flush=True)
>   precision=LTXModelConfig.from_checkpoint_dir(self.model_dir).double_precision_rope
>   self._feature_extractor=GemmaFeaturesExtractorV2(double_precision_rope=precision)
>   weights=load_split_safetensors(self.model_dir/'connector.safetensors',prefix='connector.')
>   self._feature_extractor.connector.load_weights(list(weights.items()));del weights
>   video,audio=self._feature_extractor(states,attention_mask=mask);mx.eval(video,audio)
>   mx.savez(str(path),video=video,audio=audio)
>   self._feature_extractor=None;del states,mask;aggressive_cleanup()
>   print('Prompt ready; both text-model weight sets released',flush=True)
>   return video,audio
>  PromptEncoder.load=deferred_load
>  PromptEncoder.encode=sequential_encode
> ```

## 完整关键提示词与生成记录

以下保留项目自有提示词、候选参数与版本。存在失败候选和历史参数；最终采用以 selected-takes、edit-decisions 与制作复盘为准，不能将所有候选都写成已采用。

> [!info]- videos/rukawa-anime-redraw-20s-20261110/docs/celebration-correction-prompt.txt
> ```text
> Edit the reference image with ONE precise correction: remove the orange basketball visible inside the hoop/net at the upper-left. Restore that small region as empty space inside the same white basketball net, matching the teal/gray gym wall behind it and the existing net strings, so the hoop is empty after the winning shot has already fallen through. Preserve every character, face, uniform, body pose, hands, shoes, cheering audience, framing, floor, linework, colors, shadows and ALL other pixels as closely as possible. No other changes. Output the same complete widescreen anime celebration scene. No added text, no new ball anywhere.
> ```

> [!info]- videos/rukawa-anime-redraw-20s-20261110/docs/celebration-grounded-image-prompt.txt
> ```text
> Edit this SAME complete anime celebration scene. Preserve the exact gym, crowd, hoop, lighting, linework, character identities, costume colors and all five players. Change only the two red teammates' starting body poses so a subsequent animation can celebrate IN PLACE.
> Red-haired SHOHOKU 10 at middle-left must stand with BOTH shoes fully planted on the court, feet comfortably apart, natural slightly flexed knees, torso upright, right fist held near chest ready to pump upward. He grins excitedly toward Rukawa. He is not taking a running step and must stay a clear body-width away from Rukawa.
> Tall muscular SHOHOKU 4 at far left must also stand with BOTH shoes planted, feet shoulder-width apart and upright balanced weight. His hands are raised naturally in front of chest ready to clap, smiling toward Rukawa. He must remain far left with clear spacing from the other players.
> Keep Rukawa in red SHOHOKU 11 standing calmly at the right, same sharp face, black fringe and black left wristband, relaxed arms and grounded shoes. Keep the two white/blue opponents farther behind, disappointed. Keep all people separated with clear silhouettes and correct hands and legs. Keep the basket EMPTY. The gallery audience is still celebrating with red towels.
> Match the exact existing restrained 1990s sports anime ink and two-tone cel shading. ONE integrated widescreen 16:9 image, no panels, captions, UI or extra characters. All feet on their original depth plane, anchored contact shadows, no walking pose or crossing bodies.
> ```

> [!info]- videos/rukawa-anime-redraw-20s-20261110/docs/prep-image-prompt.txt
> ```text
> Use case: illustration-story. Asset type: first frame for a coherent hand-drawn basketball anime video, one finished widescreen 16:9 scene.
> Input image is the approved unified STYLE MASTER and the exact camera / gym / five character identities to preserve. Redraw the SAME scene approximately 0.7 seconds BEFORE the shown jump-shot release. Keep the 1990s sports anime ink, cel shading, muted teal gym wall, overhead daylight, spectator gallery, hoop on upper left, five players, uniforms, hair, proportions and scene perspective consistent.
> On the right, black-fringe Kaede Rukawa in red SHOHOKU 11 and black left wristband is now standing with both shoes planted BEHIND the three-point line on the side AWAY from the basket. Knees flexed naturally to gather for a jump shot, hips lowered slightly, torso upright with small forward lean. Both natural five-finger hands securely cradle one orange basketball near upper chest; shooting hand behind/under ball, guide hand on side, normal wrists and elbows. Eyes fixed on hoop. Preserve his handsome sharp calm face.
> White/blue RYONAN 7 defender stands immediately to Rukawa's left, feet planted with loaded knees, leaning toward him, one hand coming up to contest. Their apparent heights are similar at this depth; they share the same court plane. The defender has tall spiky black hair and a focused face.
> Farther left, red SHOHOKU 4 is boxing out the SAME white/blue opponent, feet firmly on floor and weight against the opponent; red-haired SHOHOKU 10 has just begun running toward the basket while watching Rukawa. These are the same characters and costumes as the master, all drawn with the same ink density and restrained two-tone colors.
> Correct floor three-point marking so it clearly runs on the basket side of Rukawa's planted feet, about one shoe length away. Avoid a large arc behind his feet that would place him inside the line. Maintain enough empty margin around heads, fingers and shoes for movement. One shared light direction, attached contact shadows on all five players, plausible depth and proportions. Gallery crowd is a coherent anime crowd with varied poses, simpler faces at distance.
> NO airborne player at this starting moment. No ball in sky, no duplicate basketball. No scoreboard, subtitles, labels, arrows, panels, photographic textures, 3D render, stickers, cutout border or UI. ONE integrated scene for the beginning of an animated shot.
> ```

> [!info]- videos/rukawa-anime-redraw-20s-20261110/docs/rim-clean-plate-prompt.txt
> ```text
> Precise cleanup edit of this SAME anime frame. Remove ONLY the orange basketball above-left of the hoop and remove ONLY the hanging white basketball NET and its white cords, so the orange metal rim is empty and has NO net. Restore the areas uncovered by the ball and net using the same transparent glass backboard, subtle white reflections, painted white rectangle and muted teal gym wall behind them. Preserve the exact original camera, the ENTIRE orange rim ellipse, its scale and position, the supporting orange bracket, glass backboard edges, metal support, every cheering spectator, windows, lighting, ink strokes and colors everywhere else. The rigid orange rim MUST NOT shift or change shape. No other changes, no new object, no text. Output the complete same-size widescreen image. The basketball and net will be animated separately.
> ```

> [!info]- videos/rukawa-anime-redraw-20s-20261110/docs/scene-image-prompts.json
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

> [!info]- videos/rukawa-anime-redraw-20s-20261110/docs/style-master-prompt.txt
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

> [!info]- videos/rukawa-anime-redraw-20s-20261110/docs/video-prompts.json
> ```json
> {
>   "pilot": "A continuous hand-drawn Japanese basketball anime shot in the same cel animation style as the reference frame. The black-haired boy in red number 11 bends his knees, smoothly raises the basketball from his chest above his forehead, extends his legs and jumps straight up, shoots the single ball toward the basket on the left with a natural right-handed wrist flick, then lands on his two feet with softened knees. The white and blue number 7 defender reacts to the ball rising, steps closer and jumps to contest with his right arm reaching upward, then lands a moment after the shooter. The red number 4 at the left shifts his feet and keeps boxing out the white-shirted opponent; the red-haired number 10 moves toward the basket while watching the flight. Everyone moves together through the same continuous time, with naturally bending joints, grounded weight and coordinated eyelines. The small gallery spectators turn their heads and lift hands. Static wide camera, unchanged gym architecture and court markings. The video remains hand-inked two-tone cel animation throughout, same identities, jerseys, faces and limb lengths, clean linework and warm gym colors.",
>   "close": "A restrained dramatic close-up from a hand-drawn Japanese basketball anime, preserving the reference character's exact face, black fringe and red number 11 jersey. He focuses his eyes toward the basket at the left, breathes once, blinks once, and slightly raises the single basketball from upper chest toward his chin. His two hands retain a natural basketball grip and normal fingers. His shoulders and elbows move together as he prepares his shot. Tiny natural hair movement and moving spectators far behind. Fixed cinematic camera, same muted teal gym, same thin ink outlines and two-tone cel coloring. Smooth purposeful subtle character acting, no change of illustration style.",
>   "rim": "An unbroken close-up in a hand-drawn Japanese basketball anime, exactly the same inked cel style as the reference. The ONE orange basketball falls diagonally down-right from its starting position above the hoop, enters cleanly through the center of the orange rim without hitting the metal or backboard, pushes the white net down and outward, then drops out the bottom of the net and exits the lower frame. The net rebounds and swings gently to settle while the rigid rim and backboard stay fixed. Spectators below simultaneously throw up their arms and wave red towels in celebration. A clean decisive swish, correct ball size relative to the basket. Locked-off camera, continuous natural motion, preserve the exact illustrated gym and coherent drawn spectators.",
>   "celebrate": "A coherent hand-drawn Japanese basketball anime celebration, keeping the exact five identities, ink lines and two-tone cel colors of the reference. All five players retain their exact floor positions and clear separation throughout. Red-haired red number 10 pumps his raised right fist once near his chest while grinning. The tall muscular red number 4 claps his two hands together once and smiles. Black-haired red number 11 makes a small head turn toward his teammates and relaxes his shoulders, calm and quietly satisfied. The two white and blue opponents lower their gaze and shoulders in disappointment. All feet remain planted on the same court marks, with natural balance and stable body proportions. The gallery audience independently raises arms and waves red towels. Fixed camera, same rigid gym architecture, same jersey numbers and facial identities, coherent fully drawn cel anime motion, restrained small upper-body gestures.",
>   "hero": "A quiet final close-up from a hand-drawn Japanese basketball anime. Keep this exact black-haired teenage player's face, fringe and red SHOHOKU 11 jersey and the same inked two-tone cel shading throughout. After the winning shot he releases a soft breath, the tiny vapor puff disperses, he blinks once and slowly raises his gaze a little toward the left, with restrained satisfaction and a barely visible smile. Hair and shoulders settle naturally. The small gallery audience behind him continues cheering and waving red towels. Fixed close-up camera, coherent muted teal gym, natural subtle facial acting and consistent outlines, no dialogue.",
>   "shot": "A continuous hand-drawn Japanese basketball anime shot in the same cel animation style as the reference frame. The black-haired boy in red number 11 bends his knees, smoothly raises the basketball from his chest above his forehead, extends his legs and jumps straight up, shoots the single ball toward the basket on the left with a natural right-handed wrist flick, then lands on his two feet with softened knees. The white and blue number 7 defender reacts to the ball rising, steps closer and jumps to contest with his right arm reaching upward, then lands a moment after the shooter. The red number 4 at the left shifts his feet and keeps boxing out the white-shirted opponent; the red-haired number 10 moves toward the basket while watching the flight. Everyone moves together through the same continuous time, with naturally bending joints, grounded weight and coordinated eyelines. The small gallery spectators turn their heads and lift hands. Static wide camera, unchanged gym architecture and court markings. The video remains hand-inked two-tone cel animation throughout, same identities, jerseys, faces and limb lengths, clean linework and warm gym colors."
> }
> ```

> [!info]- videos/rukawa-anime-redraw-20s-20261110/outputs/checks/generation-celebrate-crossing-v1.json
> ```json
> {
>   "scene": "celebrate",
>   "output": "public/video/celebrate.mp4",
>   "native_source": "public/video/celebrate-native24.mp4",
>   "source_seconds_used": 2.0,
>   "slow_motion_factor": 2.0,
>   "frames": 96,
>   "fps": 24,
>   "duration": 4.0,
>   "exit_code": 0,
>   "method": "Reviewed local whole-scene video; video timestamps and audio tempo retimed together, without optical-flow deformation.",
>   "native_generation": {
>     "scene": "celebrate",
>     "output": "public/video/celebrate-native24.mp4",
>     "frames": 49,
>     "width": 1024,
>     "height": 576,
>     "fps": 24,
>     "seed": 7711,
>     "prompt": "A lively continuous scene from a hand-drawn Japanese basketball anime, keeping the exact identities and the reference's clean ink and two-tone cel colors. After the winning three-pointer, the red-haired red number 10 takes two excited running steps toward the black-haired red number 11 on the right, raises one fist and grins with his mouth open. Red number 11 turns his head slightly toward his teammate, lowers his shoulders and takes one relaxed step, calm and quietly satisfied. The tall muscular red number 4 walks toward them smiling, pumping one fist. Behind them the white and blue number 7 defender drops his gaze and lowers his arms in disappointment. The gallery crowd cheers, moving their arms and waving red towels independently. All feet push off and contact the same wooden floor naturally; normal connected anatomy and consistent proportions. Fixed medium-wide camera with very subtle tracking toward the teammates, same gym perspective and lighting, fully drawn anime throughout.",
>     "method": "whole-scene local LTX-2.3 image-to-video, q4, block streaming",
>     "dit_compute_dtype": "default-float32",
>     "exit_code": 0,
>     "wall_seconds": 329.91,
>     "output_exists": true
>   },
>   "wall_seconds": 329.91
> }
> ```

> [!info]- videos/rukawa-anime-redraw-20s-20261110/outputs/checks/generation-celebrate-native.json
> ```json
> {
>   "scene": "celebrate",
>   "output": "public/video/celebrate-native24.mp4",
>   "frames": 49,
>   "width": 1024,
>   "height": 576,
>   "fps": 24,
>   "seed": 7711,
>   "prompt": "A coherent hand-drawn Japanese basketball anime celebration, keeping the exact five identities, ink lines and two-tone cel colors of the reference. All five players retain their exact floor positions and clear separation throughout. Red-haired red number 10 pumps his raised right fist once near his chest while grinning. The tall muscular red number 4 claps his two hands together once and smiles. Black-haired red number 11 makes a small head turn toward his teammates and relaxes his shoulders, calm and quietly satisfied. The two white and blue opponents lower their gaze and shoulders in disappointment. All feet remain planted on the same court marks, with natural balance and stable body proportions. The gallery audience independently raises arms and waves red towels. Fixed camera, same rigid gym architecture, same jersey numbers and facial identities, coherent fully drawn cel anime motion, restrained small upper-body gestures.",
>   "method": "whole-scene local LTX-2.3 image-to-video, q4, block streaming",
>   "dit_compute_dtype": "default-float32",
>   "exit_code": 0,
>   "wall_seconds": 355.44,
>   "output_exists": true
> }
> ```

> [!info]- videos/rukawa-anime-redraw-20s-20261110/outputs/checks/generation-celebrate.json
> ```json
> {
>   "scene": "celebrate",
>   "output": "public/video/celebrate.mp4",
>   "native_source": "public/video/celebrate-native24.mp4",
>   "source_seconds_used": 2.0,
>   "slow_motion_factor": 2.0,
>   "frames": 96,
>   "fps": 24,
>   "duration": 4.0,
>   "exit_code": 0,
>   "method": "Reviewed local whole-scene video; video timestamps and audio tempo retimed together, without optical-flow deformation.",
>   "native_generation": {
>     "scene": "celebrate",
>     "output": "public/video/celebrate-native24.mp4",
>     "frames": 49,
>     "width": 1024,
>     "height": 576,
>     "fps": 24,
>     "seed": 7711,
>     "prompt": "A coherent hand-drawn Japanese basketball anime celebration, keeping the exact five identities, ink lines and two-tone cel colors of the reference. All five players retain their exact floor positions and clear separation throughout. Red-haired red number 10 pumps his raised right fist once near his chest while grinning. The tall muscular red number 4 claps his two hands together once and smiles. Black-haired red number 11 makes a small head turn toward his teammates and relaxes his shoulders, calm and quietly satisfied. The two white and blue opponents lower their gaze and shoulders in disappointment. All feet remain planted on the same court marks, with natural balance and stable body proportions. The gallery audience independently raises arms and waves red towels. Fixed camera, same rigid gym architecture, same jersey numbers and facial identities, coherent fully drawn cel anime motion, restrained small upper-body gestures.",
>     "method": "whole-scene local LTX-2.3 image-to-video, q4, block streaming",
>     "dit_compute_dtype": "default-float32",
>     "exit_code": 0,
>     "wall_seconds": 355.44,
>     "output_exists": true
>   },
>   "wall_seconds": 355.44
> }
> ```

> [!info]- videos/rukawa-anime-redraw-20s-20261110/outputs/checks/generation-close-fp16-failed.json
> ```json
> {
>   "scene": "close",
>   "output": "public/video/close.mp4",
>   "frames": 73,
>   "width": 1024,
>   "height": 576,
>   "fps": 24,
>   "seed": 7711,
>   "prompt": "A restrained dramatic close-up from a hand-drawn Japanese basketball anime, preserving the reference character's exact face, black fringe and red number 11 jersey. He focuses his eyes toward the basket at the left, breathes once, blinks once, and slightly raises the single basketball from upper chest toward his chin. His two hands retain a natural basketball grip and normal fingers. His shoulders and elbows move together as he prepares his shot. Tiny natural hair movement and moving spectators far behind. Fixed cinematic camera, same muted teal gym, same thin ink outlines and two-tone cel coloring. Smooth purposeful subtle character acting, no change of illustration style.",
>   "method": "whole-scene local LTX-2.3 image-to-video, q4, block streaming",
>   "dit_compute_dtype": "float16",
>   "exit_code": 1,
>   "wall_seconds": 61.33,
>   "output_exists": false
> }
> ```

> [!info]- videos/rukawa-anime-redraw-20s-20261110/outputs/checks/generation-close-native.json
> ```json
> {
>   "scene": "close",
>   "output": "public/video/close.mp4",
>   "frames": 73,
>   "width": 1024,
>   "height": 576,
>   "fps": 24,
>   "seed": 7711,
>   "prompt": "A restrained dramatic close-up from a hand-drawn Japanese basketball anime, preserving the reference character's exact face, black fringe and red number 11 jersey. He focuses his eyes toward the basket at the left, breathes once, blinks once, and slightly raises the single basketball from upper chest toward his chin. His two hands retain a natural basketball grip and normal fingers. His shoulders and elbows move together as he prepares his shot. Tiny natural hair movement and moving spectators far behind. Fixed cinematic camera, same muted teal gym, same thin ink outlines and two-tone cel coloring. Smooth purposeful subtle character acting, no change of illustration style.",
>   "method": "whole-scene local LTX-2.3 image-to-video, q4, block streaming",
>   "dit_compute_dtype": "default-float32",
>   "exit_code": 0,
>   "wall_seconds": 412.37,
>   "output_exists": true
> }
> ```

> [!info]- videos/rukawa-anime-redraw-20s-20261110/outputs/checks/generation-close.json
> ```json
> {
>   "scene": "close",
>   "output": "public/video/close.mp4",
>   "native_source": "public/video/close-native24.mp4",
>   "source_seconds_used": 2.0,
>   "slow_motion_factor": 1.5,
>   "frames": 72,
>   "fps": 24,
>   "duration": 3.0,
>   "exit_code": 0,
>   "method": "Reviewed local whole-scene video; video timestamps and audio tempo retimed together, without optical-flow deformation."
> }
> ```

> [!info]- videos/rukawa-anime-redraw-20s-20261110/outputs/checks/generation-hero-native.json
> ```json
> {
>   "scene": "hero",
>   "output": "public/video/hero-native24.mp4",
>   "frames": 49,
>   "width": 1024,
>   "height": 576,
>   "fps": 24,
>   "seed": 7711,
>   "prompt": "A quiet final close-up from a hand-drawn Japanese basketball anime. Keep this exact black-haired teenage player's face, fringe and red SHOHOKU 11 jersey and the same inked two-tone cel shading throughout. After the winning shot he releases a soft breath, the tiny vapor puff disperses, he blinks once and slowly raises his gaze a little toward the left, with restrained satisfaction and a barely visible smile. Hair and shoulders settle naturally. The small gallery audience behind him continues cheering and waving red towels. Fixed close-up camera, coherent muted teal gym, natural subtle facial acting and consistent outlines, no dialogue.",
>   "method": "whole-scene local LTX-2.3 image-to-video, q4, block streaming",
>   "dit_compute_dtype": "default-float32",
>   "exit_code": 0,
>   "wall_seconds": 375.07,
>   "output_exists": true
> }
> ```

> [!info]- videos/rukawa-anime-redraw-20s-20261110/outputs/checks/generation-hero.json
> ```json
> {
>   "scene": "hero",
>   "output": "public/video/hero.mp4",
>   "native_source": "public/video/hero-native24.mp4",
>   "source_seconds_used": 2.0,
>   "slow_motion_factor": 2.0,
>   "frames": 96,
>   "fps": 24,
>   "duration": 4.0,
>   "exit_code": 0,
>   "method": "Reviewed local whole-scene video; video timestamps and audio tempo retimed together, without optical-flow deformation.",
>   "native_generation": {
>     "scene": "hero",
>     "output": "public/video/hero-native24.mp4",
>     "frames": 49,
>     "width": 1024,
>     "height": 576,
>     "fps": 24,
>     "seed": 7711,
>     "prompt": "A quiet final close-up from a hand-drawn Japanese basketball anime. Keep this exact black-haired teenage player's face, fringe and red SHOHOKU 11 jersey and the same inked two-tone cel shading throughout. After the winning shot he releases a soft breath, the tiny vapor puff disperses, he blinks once and slowly raises his gaze a little toward the left, with restrained satisfaction and a barely visible smile. Hair and shoulders settle naturally. The small gallery audience behind him continues cheering and waving red towels. Fixed close-up camera, coherent muted teal gym, natural subtle facial acting and consistent outlines, no dialogue.",
>     "method": "whole-scene local LTX-2.3 image-to-video, q4, block streaming",
>     "dit_compute_dtype": "default-float32",
>     "exit_code": 0,
>     "wall_seconds": 375.07,
>     "output_exists": true
>   },
>   "wall_seconds": 375.07
> }
> ```

> [!info]- videos/rukawa-anime-redraw-20s-20261110/outputs/checks/generation-pilot.json
> ```json
> {
>   "scene": "pilot",
>   "output": "outputs/checks/pilot.mp4",
>   "frames": 49,
>   "width": 768,
>   "height": 448,
>   "fps": 24,
>   "seed": 7711,
>   "prompt": "A continuous hand-drawn Japanese basketball anime shot in the same cel animation style as the reference frame. The black-haired boy in red number 11 bends his knees, smoothly raises the basketball from his chest above his forehead, extends his legs and jumps straight up, shoots the single ball toward the basket on the left with a natural right-handed wrist flick, then lands on his two feet with softened knees. The white and blue number 7 defender reacts to the ball rising, steps closer and jumps to contest with his right arm reaching upward, then lands a moment after the shooter. The red number 4 at the left shifts his feet and keeps boxing out the white-shirted opponent; the red-haired number 10 moves toward the basket while watching the flight. Everyone moves together through the same continuous time, with naturally bending joints, grounded weight and coordinated eyelines. The small gallery spectators turn their heads and lift hands. Static wide camera, unchanged gym architecture and court markings. The video remains hand-inked two-tone cel animation throughout, same identities, jerseys, faces and limb lengths, clean linework and warm gym colors.",
>   "method": "whole-scene local LTX-2.3 image-to-video, q4, block streaming",
>   "exit_code": 0,
>   "wall_seconds": 191.15,
>   "output_exists": true
> }
> ```

> [!info]- videos/rukawa-anime-redraw-20s-20261110/outputs/checks/generation-rim-native.json
> ```json
> {
>   "scene": "rim",
>   "output": "public/video/rim-native24.mp4",
>   "frames": 49,
>   "width": 1024,
>   "height": 576,
>   "fps": 24,
>   "seed": 7711,
>   "prompt": "An unbroken close-up in a hand-drawn Japanese basketball anime, exactly the same inked cel style as the reference. The ONE orange basketball falls diagonally down-right from its starting position above the hoop, enters cleanly through the center of the orange rim without hitting the metal or backboard, pushes the white net down and outward, then drops out the bottom of the net and exits the lower frame. The net rebounds and swings gently to settle while the rigid rim and backboard stay fixed. Spectators below simultaneously throw up their arms and wave red towels in celebration. A clean decisive swish, correct ball size relative to the basket. Locked-off camera, continuous natural motion, preserve the exact illustrated gym and coherent drawn spectators.",
>   "method": "whole-scene local LTX-2.3 image-to-video, q4, block streaming",
>   "dit_compute_dtype": "default-float32",
>   "exit_code": 0,
>   "wall_seconds": 352.87,
>   "output_exists": true
> }
> ```

> [!info]- videos/rukawa-anime-redraw-20s-20261110/outputs/checks/generation-rim.json
> ```json
> {
>   "scene": "rim",
>   "output": "public/video/rim.mp4",
>   "native_source": "public/video/rim-native24.mp4",
>   "source_seconds_used": 2.0,
>   "slow_motion_factor": 2.5,
>   "frames": 120,
>   "fps": 24,
>   "duration": 5.0,
>   "exit_code": 0,
>   "method": "Reviewed local whole-scene video; video timestamps and audio tempo retimed together, without optical-flow deformation.",
>   "native_generation": {
>     "scene": "rim",
>     "output": "public/video/rim-native24.mp4",
>     "frames": 49,
>     "width": 1024,
>     "height": 576,
>     "fps": 24,
>     "seed": 7711,
>     "prompt": "An unbroken close-up in a hand-drawn Japanese basketball anime, exactly the same inked cel style as the reference. The ONE orange basketball falls diagonally down-right from its starting position above the hoop, enters cleanly through the center of the orange rim without hitting the metal or backboard, pushes the white net down and outward, then drops out the bottom of the net and exits the lower frame. The net rebounds and swings gently to settle while the rigid rim and backboard stay fixed. Spectators below simultaneously throw up their arms and wave red towels in celebration. A clean decisive swish, correct ball size relative to the basket. Locked-off camera, continuous natural motion, preserve the exact illustrated gym and coherent drawn spectators.",
>     "method": "whole-scene local LTX-2.3 image-to-video, q4, block streaming",
>     "dit_compute_dtype": "default-float32",
>     "exit_code": 0,
>     "wall_seconds": 352.87,
>     "output_exists": true
>   },
>   "wall_seconds": 352.87
> }
> ```

> [!info]- videos/rukawa-anime-redraw-20s-20261110/outputs/checks/generation-shot-long-v1.json
> ```json
> {
>   "scene": "shot",
>   "output": "public/video/shot.mp4",
>   "frames": 121,
>   "width": 1024,
>   "height": 576,
>   "fps": 24,
>   "seed": 7711,
>   "prompt": "A continuous hand-drawn Japanese basketball anime shot in the same cel animation style as the reference frame. The black-haired boy in red number 11 bends his knees, smoothly raises the basketball from his chest above his forehead, extends his legs and jumps straight up, shoots the single ball toward the basket on the left with a natural right-handed wrist flick, then lands on his two feet with softened knees. The white and blue number 7 defender reacts to the ball rising, steps closer and jumps to contest with his right arm reaching upward, then lands a moment after the shooter. The red number 4 at the left shifts his feet and keeps boxing out the white-shirted opponent; the red-haired number 10 moves toward the basket while watching the flight. Everyone moves together through the same continuous time, with naturally bending joints, grounded weight and coordinated eyelines. The small gallery spectators turn their heads and lift hands. Static wide camera, unchanged gym architecture and court markings. The video remains hand-inked two-tone cel animation throughout, same identities, jerseys, faces and limb lengths, clean linework and warm gym colors.",
>   "method": "whole-scene local LTX-2.3 image-to-video, q4, block streaming",
>   "exit_code": 0,
>   "wall_seconds": 618.49,
>   "output_exists": true
> }
> ```

> [!info]- videos/rukawa-anime-redraw-20s-20261110/outputs/checks/generation-shot-short-v2.json
> ```json
> {
>   "scene": "shot",
>   "output": "public/video/shot-short-v2.mp4",
>   "frames": 49,
>   "width": 768,
>   "height": 448,
>   "fps": 24,
>   "seed": 7711,
>   "prompt": "A continuous hand-drawn Japanese basketball anime shot in the same cel animation style as the reference frame. The black-haired boy in red number 11 bends his knees, smoothly raises the basketball from his chest above his forehead, extends his legs and jumps straight up, shoots the single ball toward the basket on the left with a natural right-handed wrist flick, then lands on his two feet with softened knees. The white and blue number 7 defender reacts to the ball rising, steps closer and jumps to contest with his right arm reaching upward, then lands a moment after the shooter. The red number 4 at the left shifts his feet and keeps boxing out the white-shirted opponent; the red-haired number 10 moves toward the basket while watching the flight. Everyone moves together through the same continuous time, with naturally bending joints, grounded weight and coordinated eyelines. The small gallery spectators turn their heads and lift hands. Static wide camera, unchanged gym architecture and court markings. The video remains hand-inked two-tone cel animation throughout, same identities, jerseys, faces and limb lengths, clean linework and warm gym colors.",
>   "method": "whole-scene local LTX-2.3 image-to-video, q4, block streaming",
>   "dit_compute_dtype": "default-float32",
>   "exit_code": 0,
>   "wall_seconds": 203.1,
>   "output_exists": true
> }
> ```

> [!info]- videos/rukawa-anime-redraw-20s-20261110/outputs/checks/generation-shot.json
> ```json
> {
>   "scene": "shot",
>   "output": "public/video/shot.mp4",
>   "native_source": "public/video/shot-short-v2.mp4",
>   "source_seconds_used": 2.0,
>   "slow_motion_factor": 2.0,
>   "frames": 96,
>   "fps": 24,
>   "duration": 4.0,
>   "exit_code": 0,
>   "method": "Reviewed local whole-scene video; video timestamps and audio tempo retimed together, without optical-flow deformation."
> }
> ```

## 已有产物与检查依据

文件存在、技术检查、画面检查与用户验收分别看待。下表是本次文件盘点，旧版与当前版可能共存。MP4/HTML 按内容计算 SHA-256，ZIP 仅记录文件大小和修改时间供增量判断。

| 本地产物 | 字节数 | SHA-256 |
| --- | ---: | --- |
| `videos/rukawa-anime-redraw-20s-20261110/outputs/review.html` | 1779 | `d82bc6955178644505bf59c5db7705f4448b8251a78e589d50db263625069eaa` |
| `videos/rukawa-anime-redraw-20s-20261110/outputs/rukawa-anime-remake-20s.mp4` | 11111928 | `2238c30add1817a0bb977fa3e7e3ee9ac3e70590e5d240e90d7e10423eb4a2c5` |
| `videos/rukawa-anime-redraw-20s-20261110/outputs/rukawa-anime-remake-project.zip` | 57064657 | `ZIP：仅元数据` |

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
>     "size": "11111928"
>   },
>   "file": "outputs/rukawa-anime-remake-20s.mp4",
>   "scope": "本次只读取媒体头；未全片解码、播放或重新进行视觉/听觉验收。"
> }
> ```


已有检查报告路径（未在本轮重新执行）：

- `videos/rukawa-anime-redraw-20s-20261110/outputs/checks/audio-normalization.json`
- `videos/rukawa-anime-redraw-20s-20261110/outputs/checks/media-metadata.json`
- `videos/rukawa-anime-redraw-20s-20261110/outputs/checks/package-report.json`
- `videos/rukawa-anime-redraw-20s-20261110/outputs/checks/shot-media.json`
- `videos/rukawa-anime-redraw-20s-20261110/outputs/checks/verification.json`
- `videos/rukawa-anime-redraw-20s-20261110/outputs/checks/clip-frames/celebrate/source-frames.json`
- `videos/rukawa-anime-redraw-20s-20261110/outputs/checks/clip-frames/close/source-frames.json`
- `videos/rukawa-anime-redraw-20s-20261110/outputs/checks/clip-frames/hero/source-frames.json`
- `videos/rukawa-anime-redraw-20s-20261110/outputs/checks/clip-frames/rim/source-frames.json`
- `videos/rukawa-anime-redraw-20s-20261110/outputs/checks/clip-frames/shot/source-frames.json`
- `videos/rukawa-anime-redraw-20s-20261110/outputs/checks/final-frames/positions.json`

## 制作方法与迭代记录选编

下面是项目自有文档的归档摘录，保留原阶段与原检查表述，个人路径已泛化、相对链接已注明源项目位置。它们属于来源陈述，不代表本次重新完成其中的生成或验收。

> [!info]- 来源文档：videos/rukawa-anime-redraw-20s-20261110/README.md
> # 流川枫三分绝杀 · 重制工程
> 
> 工程目标为20秒、1280×720、24fps（480帧）的独立Remotion作品。整幅原画驱动本地连续视频生成，再用Remotion编辑分镜、比分与终场文字。制作方法、来源与限制见 `docs/production-method.md`。
> 
> ## 独立安装、预览与导出
> 
> 需要 Node.js 22或更新版本、npm、Python 3、ffmpeg 和 ffprobe。解压后在本目录执行：
> 
> ```sh
> mkdir -p .cache/tmp .cache/npm
> TMPDIR="$PWD/.cache/tmp" TMP="$PWD/.cache/tmp" TEMP="$PWD/.cache/tmp" npm ci --cache .cache/npm
> npm run dev
> ```
> 
> 预览地址为 `http://localhost:3097/RukawaRemake`。Studio中的各镜头也可独立打开。制作时没有生成完成的镜头会明确显示“原画预览”；它们不是最终动画。
> 
> 所有连续镜头生成并检查后，执行：
> 
> ```sh
> npm run render
> npm run check
> npm run package
> ```
> 
> 导出后脚本会统一声音响度，视频流保持不变。最终MP4路径为 `outputs/rukawa-anime-remake-20s.mp4`。脚本会将工作目录、npm缓存、临时目录和输出限定在本项目。macOS默认使用系统Google Chrome，也可通过 `CHROME_BIN` 指定浏览器；需要由Remotion下载浏览器时，其缓存位于本目录的 `node_modules/.remotion/`。
> 
> ## 修改素材和时间轴
> 
> `public/art/`是完整场景原画，`public/video/`是实际生成的连续镜头，`public/audio/`保存补充音效，`public/fonts/`是本地中文字体。字体授权在 `docs/NOTO-OFL.txt`。
> 
> 五个镜头的顺序和帧数在 `src/Root.tsx` 的独立 `Series.Sequence` 节点里，时长为3、4、5、4、4秒，总和480帧。`src/BasketAction.tsx` 控制固定篮筐、单个篮球轨迹和球网回弹；看台运动来自本地生成的视频。庆祝镜头的源图是 `public/art/celebrate-grounded-v2.png`，采用保持脚下位置的上身表演。各镜头的独立组件控制视频和声音，`src/timing.ts`记录终场声与记分时刻。`src/media-status.ts`记录连续素材是否准备完成。全片按慢镜节奏剪辑，投篮等镜头保留连续源帧、延长每帧显示时间，没有光流补帧对人体做额外变形。
> 
> 如需重新生成连续镜头，需要Apple Silicon Mac、Python 3.13、uv、git、ffmpeg及足够内存和磁盘空间。初次准备会下载数十GB模型，全部放在本目录内。源图和提示词也位于本目录：
> 
> ```sh
> python3 scripts/setup-generator.py
> .venv/bin/python scripts/fetch-video-model.py
> .venv/bin/python scripts/generate-clip.py pilot
> .venv/bin/python scripts/generate-clip.py shot --suffix=-short-v2
> .venv/bin/python scripts/finish-motion-clip.py shot
> .venv/bin/python scripts/generate-clip.py close
> .venv/bin/python scripts/generate-clip.py rim
> .venv/bin/python scripts/generate-clip.py celebrate
> .venv/bin/python scripts/generate-clip.py hero
> ```
> 
> 生成时每次只运行一个视频推理任务。修改 `docs/video-prompts.json` 可调整动作描述；`--seed`可更换随机种子，`--suffix=-v2`可另存候选版本。开场、篮筐、庆祝和结尾的正常生成会自动应用已记录的慢放剪辑；另存候选版本则保留原始时间，供人工选择。正常导出已保存的连续素材不需要重新加载本地生成模型。
> 
> 项目ZIP包含源码、配置、锁定依赖、场景素材、已生成的视频片段和最终成片；可重建的 `node_modules/`、`.venv/`、模型缓存及工具缓存不包含在ZIP内。ZIP的安装和导出不引用其他视频或项目根目录的文件。

> [!info]- 来源文档：videos/rukawa-anime-redraw-20s-20261110/docs/production-method.md
> # 本轮重制方法
> 
> 目标：20秒动漫风格的流川枫三分绝杀，改善前版人物与场景不协调、四肢被拉伸和配角缺少反应的问题。完整交付必须以实际连续视频片段为基础；制作中的原画预览不能视为已完成视频。
> 
> 原画：以同一球馆和完整同框画面为基准，用内置 image_gen 制作各镜头。角色与场景在同一幅图里统一比例、线条、透视和光照；没有从旧版复制分离的人物图集或变形绑定。提示词、生成文件与工具保留的原始位置均记录在本目录 docs 中。
> 
> 运动：本机 Apple M3 Pro、18GB统一内存，使用 LTX-2.3 q4 的整幅图生视频推理。安装的代码固定在 `local-generator-commit.txt` 的提交，模型版本在 `local-models.json`。仅下载所需的 distilled 模型与组件，缓存位于本目录 `.cache/models/`。视频推理设为离线，不把提示词和图片发送到外部视频服务。
> 
> 为降低内存峰值，`scripts/lowram_prompt.py` 将文本模型与连接器依次加载：先计算并物化文本隐藏状态，再释放文本模型，最后用原始连接器投影。权重和投影运算沿用所固定的库；提示词向量缓存到本目录。随后使用库的 `--low-ram` 逐块读取视频模型。单个推理进程的MLX内存额度设为11GiB，不修改系统的内存或安全设置。一次float16优化试跑遇到GPU错误，已撤回该设置；最终镜头使用先前已跑通的默认精度。
> 
> 长版投篮候选的后半段出现人物重叠失真，因此不选用该候选作为成片。正式投篮使用已检查的短动作版本，采用2倍慢放；篮筐镜头以2.5倍慢放连接，庆祝与终场表演采用2倍慢放。慢放调整视频时间戳和音频速度，不使用光流补帧拉伸人体。源视频与剪辑处理记录保存在本项目内。
> 
> 参考实现与说明：[ltx-2-mlx](https://github.com/dgrauet/ltx-2-mlx)、[管线成熟度说明](https://github.com/dgrauet/ltx-2-mlx/blob/main/docs/PIPELINE_MATURITY.md)、[q4模型](https://huggingface.co/dgrauet/ltx-2.3-mlx-q4)。这些是制作工具的能力说明，不能替代本片的视觉验收。最终必须逐帧检查手脚、离手、封盖、入网和比分时刻，并按正常速度检查多人动作。
> 
> 篮筐修正：本地生成的入网视频中，金属篮圈和球网出现形变，因此成片不直接显示该视频的篮筐区域。用同一原画生成移除篮球和球网的干净底图，保持篮板与篮圈固定；在 Remotion 内使用原画篮球纹理、连续的单球轨迹和固定连接点的菱形网格表现入网和回弹。下方看台保留生成视频的连续欢呼，使用羽化遮罩去掉生成视频里的重复篮球。不是对人体做网格拉伸。
> 
> 庆祝修正：弃用人物奔跑接近的候选片段，因其后段出现身体重叠。新原画将三位队员改为间距明确、双脚站稳的姿势，动作提示也改为挥拳、拍手和小幅转头，以减少多人交叉引发的融合。
> 
> 声音：各镜头保留本地生成的环境声，叠加本目录内合成的终场声与入网声。Remotion导出后由 `scripts/final-audio.py` 调整响度并限制峰值，只复制视频流，不再次编码画面。响度测量记录位于 `outputs/checks/`。

> [!info]- 来源文档：videos/rukawa-anime-redraw-20s-20261110/docs/visual-review.md
> # 视觉检查记录
> 
> ## 已排除的候选
> 
> - 5秒长投篮：后半段出现人物融合与手部网状伪影；保存在本地缓存供追溯，未采用。
> - 直接使用生成的篮筐视频：金属篮圈弯曲，球网位置漂移；只保留其看台区域和环境声，前景用固定篮筐和可控的球、网运动重建。
> - 第一版庆祝：队员靠近时发生身体穿插；未采用。
> - float16推理试跑：发生GPU错误；最终采用已通过实机生成的默认精度。
> 
> ## 正式镜头检查
> 
> - 蓄力特写：选取前2秒有效动作，延长为3秒；排除尾端过早举球导致的切镜动作倒退。
> - 投篮：采用49帧短镜头，已检查下蹲、起跳、出手、封盖、落地；两倍慢放为4秒。多人在同一画面内连续运动，未使用单独人偶摆动。
> - 入网：单球、固定篮圈、连续轨迹、球网连接点和回弹单独检查。
> - 庆祝：站稳原画重新生成；检查0、12、24、36、48、59、71、83、95帧中的多人间距，未再出现靠近版本的身体融合。樱木挥拳后转头、赤木作小幅上身庆祝，流川枫保持克制，两个对手低头。
> - 终场：脸部、发型、球衣和看台持续运动检查；有轻微笑容。
> 
> ## 实际限制
> 
> 素材为本地整幅图生视频和慢镜剪辑，不是逐张手绘的正式动画。远景面孔及球衣英文可能偏软；短源片段采用重复显示原帧的慢放，因此动作流畅度与原生24帧手绘动画有差别。技术检查只能核对时长、编码、路径和文件完整性，不能保证人体、人物身份与叙事质量。
> 
> ## 最终验收
> 
> 最终20秒MP4已抽查各镜头交界、离手后终场声时点、入网记分和终场文字。浏览器直接加载导出的MP4并播放到20.0秒结束，分辨率1280×720，视频元素无错误。技术检查通过：480帧、24fps、H.264/AAC、全片解码、独立路径与锁定依赖。观众、封盖队员和庆祝队员均有画面内动作；远景面部与球衣文字的柔化仍然存在。

## 证据清单与缺失信息

盘点的文本/报告文件 70 个，源文本、报告及媒体产物身份摘要为 `64643f16827d4bae08d63b595feb63f016485f037aec50da985c791da0d2e620`。哈希证明采集内容身份，不代表其中每个断言已验证。模型权重、缓存、依赖、第三方源码树和未指定的历史会话未采集；大型文本仅做哈希并按需要选读。

- 本次整理基于明确引用的 README、制作方法、配置、提示词、关键源码与已有报告。未重新观看全部参考/候选/成片，也未做人工听辨。
- 项目未提供的原始模型内部实现、未披露生成后端、未记录 seed/参数、真实费用、人工总工时和用户最终验收，均不补造。实际日志有记录的耗时见上文，流水线耗时不等于整个制作总耗时。
- 源项目内的路径只用于查证；本档案未复制大型素材或模型，也不改变源项目运行依赖。

| 已盘点证据文件 | SHA-256（前 16 位）/状态 |
| --- | --- |
| `videos/rukawa-anime-redraw-20s-20261110/README.md` | `ab610ad27ee067ac` |
| `videos/rukawa-anime-redraw-20s-20261110/package-lock.json` | `c2cf4a4ca2a4a425` |
| `videos/rukawa-anime-redraw-20s-20261110/package.json` | `93aa0b72d203e79c` |
| `videos/rukawa-anime-redraw-20s-20261110/remotion.config.ts` | `89558888f253880f` |
| `videos/rukawa-anime-redraw-20s-20261110/requirements-local-video.txt` | `9a5f7de7f33ae496` |
| `videos/rukawa-anime-redraw-20s-20261110/tsconfig.json` | `af262af0dd89b1e2` |
| `videos/rukawa-anime-redraw-20s-20261110/docs/NOTO-OFL.txt` | `1c05c68c34f97084` |
| `videos/rukawa-anime-redraw-20s-20261110/docs/celebration-correction-prompt.txt` | `33647520ba4c5a1a` |
| `videos/rukawa-anime-redraw-20s-20261110/docs/celebration-grounded-image-prompt.txt` | `8308c0e38b456cad` |
| `videos/rukawa-anime-redraw-20s-20261110/docs/image-assets.json` | `9d428af078a266b4` |
| `videos/rukawa-anime-redraw-20s-20261110/docs/local-generator-commit.txt` | `cefeb3f58443a4e6` |
| `videos/rukawa-anime-redraw-20s-20261110/docs/local-models.json` | `061b2acac26482fc` |
| `videos/rukawa-anime-redraw-20s-20261110/docs/prep-image-prompt.txt` | `65e6f303828db237` |
| `videos/rukawa-anime-redraw-20s-20261110/docs/production-method.md` | `4d91b1088157aff3` |
| `videos/rukawa-anime-redraw-20s-20261110/docs/rim-clean-plate-prompt.txt` | `9d1ec48b1ab4d01c` |
| `videos/rukawa-anime-redraw-20s-20261110/docs/scene-image-prompts.json` | `fa5b1ac5b2482604` |
| `videos/rukawa-anime-redraw-20s-20261110/docs/style-master-prompt.txt` | `62ed3d6d00647704` |
| `videos/rukawa-anime-redraw-20s-20261110/docs/video-prompts.json` | `2f8bcf0a149bce01` |
| `videos/rukawa-anime-redraw-20s-20261110/docs/visual-review.md` | `44220f4ec7a4ea11` |
| `videos/rukawa-anime-redraw-20s-20261110/outputs/checks/audio-normalization.json` | `922a7037cd9ed153` |
| `videos/rukawa-anime-redraw-20s-20261110/outputs/checks/generation-celebrate-crossing-v1.json` | `a9392452960c930d` |
| `videos/rukawa-anime-redraw-20s-20261110/outputs/checks/generation-celebrate-native.json` | `3670f49d2495451e` |
| `videos/rukawa-anime-redraw-20s-20261110/outputs/checks/generation-celebrate.json` | `441d8f415f40b827` |
| `videos/rukawa-anime-redraw-20s-20261110/outputs/checks/generation-close-fp16-failed.json` | `529f5957c7cd57dc` |
| `videos/rukawa-anime-redraw-20s-20261110/outputs/checks/generation-close-native.json` | `99ef96c930e201e1` |
| `videos/rukawa-anime-redraw-20s-20261110/outputs/checks/generation-close.json` | `5ff805e37b644d9e` |
| `videos/rukawa-anime-redraw-20s-20261110/outputs/checks/generation-hero-native.json` | `7cc5fc7031f7ab4b` |
| `videos/rukawa-anime-redraw-20s-20261110/outputs/checks/generation-hero.json` | `f810ab88c7588ff1` |
| `videos/rukawa-anime-redraw-20s-20261110/outputs/checks/generation-pilot.json` | `f3aed3d971265c65` |
| `videos/rukawa-anime-redraw-20s-20261110/outputs/checks/generation-rim-native.json` | `26bd3af17abf7362` |
| `videos/rukawa-anime-redraw-20s-20261110/outputs/checks/generation-rim.json` | `91fac8bf2b33ab45` |
| `videos/rukawa-anime-redraw-20s-20261110/outputs/checks/generation-shot-long-v1.json` | `babda0376e3bd836` |
| `videos/rukawa-anime-redraw-20s-20261110/outputs/checks/generation-shot-short-v2.json` | `10fe5e1140765a72` |
| `videos/rukawa-anime-redraw-20s-20261110/outputs/checks/generation-shot.json` | `872b22b96e18909e` |
| `videos/rukawa-anime-redraw-20s-20261110/outputs/checks/media-metadata.json` | `5a07272b6d7f170b` |
| `videos/rukawa-anime-redraw-20s-20261110/outputs/checks/package-report.json` | `dd66bedd49d1ce37` |
| `videos/rukawa-anime-redraw-20s-20261110/outputs/checks/shot-media.json` | `ca9c18fbd9f27cd5` |
| `videos/rukawa-anime-redraw-20s-20261110/outputs/checks/verification.json` | `146fe215989912b2` |
| `videos/rukawa-anime-redraw-20s-20261110/outputs/checks/clip-frames/celebrate/source-frames.json` | `fd9df2c47d8c9ccd` |
| `videos/rukawa-anime-redraw-20s-20261110/outputs/checks/clip-frames/close/source-frames.json` | `357feba699093400` |
| `videos/rukawa-anime-redraw-20s-20261110/outputs/checks/clip-frames/hero/source-frames.json` | `e8b71e09293b3f9d` |
| `videos/rukawa-anime-redraw-20s-20261110/outputs/checks/clip-frames/rim/source-frames.json` | `04fd8924aa327576` |
| `videos/rukawa-anime-redraw-20s-20261110/outputs/checks/clip-frames/shot/source-frames.json` | `946cd8cb1ba74b4d` |
| `videos/rukawa-anime-redraw-20s-20261110/outputs/checks/final-frames/positions.json` | `f6d526da2484174a` |
| `videos/rukawa-anime-redraw-20s-20261110/scripts/check.py` | `f2038621129d1b4d` |
| `videos/rukawa-anime-redraw-20s-20261110/scripts/fetch-video-model.py` | `73d752c167e8831d` |
| `videos/rukawa-anime-redraw-20s-20261110/scripts/final-audio.py` | `71d993d09254fd4a` |
| `videos/rukawa-anime-redraw-20s-20261110/scripts/finish-motion-clip.py` | `2b2c3ec3486329de` |
| `videos/rukawa-anime-redraw-20s-20261110/scripts/generate-clip.py` | `5914fa0bd5818307` |
| `videos/rukawa-anime-redraw-20s-20261110/scripts/generate-remaining.py` | `0e0a5eb2cdc82796` |
| `videos/rukawa-anime-redraw-20s-20261110/scripts/lowram_prompt.py` | `f8267e06ce389727` |
| `videos/rukawa-anime-redraw-20s-20261110/scripts/ltx-entry.py` | `6643f955c03c6588` |
| `videos/rukawa-anime-redraw-20s-20261110/scripts/package.py` | `ba12de1ad64fe5ea` |
| `videos/rukawa-anime-redraw-20s-20261110/scripts/progress.py` | `3e84a268ea70fc35` |
| `videos/rukawa-anime-redraw-20s-20261110/scripts/review-clip.py` | `b96034c458489886` |
| `videos/rukawa-anime-redraw-20s-20261110/scripts/run.mjs` | `c3c8a59389b7fc85` |
| `videos/rukawa-anime-redraw-20s-20261110/scripts/setup-generator.py` | `197725e597c7794c` |
| `videos/rukawa-anime-redraw-20s-20261110/scripts/sfx.py` | `1b284890293fb018` |
| `videos/rukawa-anime-redraw-20s-20261110/src/BasketAction.tsx` | `3871ff584631ab65` |
| `videos/rukawa-anime-redraw-20s-20261110/src/CelebrationScene.tsx` | `1d81a1af199f6659` |
| `videos/rukawa-anime-redraw-20s-20261110/src/HeroScene.tsx` | `508d5b1ba7fb7a0c` |
| `videos/rukawa-anime-redraw-20s-20261110/src/OpeningScene.tsx` | `1343d8eb45e0b65e` |
| `videos/rukawa-anime-redraw-20s-20261110/src/PendingFrame.tsx` | `21869e780cb6d791` |
| `videos/rukawa-anime-redraw-20s-20261110/src/RimScene.tsx` | `165b044fcbb173c1` |
| `videos/rukawa-anime-redraw-20s-20261110/src/Root.tsx` | `bac0ee98a54e94cc` |
| `videos/rukawa-anime-redraw-20s-20261110/src/ShotScene.tsx` | `f1013e0d7cc9030b` |
| `videos/rukawa-anime-redraw-20s-20261110/src/font.ts` | `2779d73a080406f7` |
| `videos/rukawa-anime-redraw-20s-20261110/src/index.ts` | `2e0e358282a1b998` |
| `videos/rukawa-anime-redraw-20s-20261110/src/media-status.ts` | `1945702564d5e211` |
| `videos/rukawa-anime-redraw-20s-20261110/src/timing.ts` | `95cd855af057f73a` |
