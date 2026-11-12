---
type: video-production-dossier
project: video-creation
video_id: anime-scene-forge-skill-smoke-20261111
captured: '2026-11-12'
status: 0.5 秒合成测试；approved 不代表作品或用户验收
source_fingerprint: 495c94703e781f6a74d32d7df6b1d1193a0a599d2932e60d6dfbee10c4fd7cdc
tags:
- video-creation
- production-capture
---

# Anime Scene Forge · 技术门禁 smoke fixture

<!-- capture-source-sha256:495c94703e781f6a74d32d7df6b1d1193a0a599d2932e60d6dfbee10c4fd7cdc -->

本次为指定项目的材料回溯与归档。制作结论来自项目记录/源码，未找回关闭的制作对话，也未重新执行生成、渲染、音频合成或用户验收。

> 本页的环境日志与模型文件清单已删节。

## 当前结论与制作路线

**状态：0.5 秒合成测试；approved 不代表作品或用户验收。**

合成测试媒体 → 选片/音轨/审核输入 → Remotion 门禁测试。

production.json 当前 1280×720、24fps、12 帧，单镜头 Technical fixture。存在测试 MP4、合成脉冲音频和 automated-test-fixture 审核 JSON；不是动漫人物制作样本。brief 仍保留制作规划文字，应与实际 fixture 分开。

## 关键算法、实现与适用边界

测试覆盖素材哈希、原速选片、音频 fromFrame 偏移、字幕窗口和批准状态输入。虽然配置有 MLX profile、setup/fetch/generate 脚本，generation 字段为空，不能据此声称运行过 LTX 或 TTS。

## 声音与字幕

Synthetic test pulse, not speech；不作为实际角色配音或口型测试。

## 失败、修正与验收边界

reviewer 为 automated-test-fixture，observations 明确只用于门禁；visualReviewed/normalSpeedReviewed 的 true 不证明真实人物、对嘴或审美通过。

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


> [!info]- requirements-tts.txt
> ```text
> annotated-doc==0.0.5
> annotated-types==0.8.0
> anyio==4.16.0
> cffi==2.1.1
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
> miniaudio==1.72
> mlx==0.32.4
> mlx-audio==0.6.5
> mlx-lm==0.32.2
> mlx-metal==0.32.4
> numpy==2.5.4
> packaging==26.3
> protobuf==7.36.2
> pycparser==3.12
> pydantic==2.14.3
> pydantic-core==2.50.4
> pygments==2.21.0
> pyyaml==6.0.3
> regex==2026.11.9
> rich==15.0.0
> safetensors==0.8.0
> scipy==1.18.1
> sentencepiece==0.2.3
> shellingham==1.5.4
> sounddevice==0.5.6
> tqdm==4.70.1
> transformers==5.22.2
> truststore==0.10.5
> typer==0.29.0
> typing-extensions==4.16.0
> typing-inspection==0.4.5
> ```


> [!info]- 版本证据：docs/mlx-profile.json（配置存在与实际使用分开）
> ```json
> {
>   "profileVersion": "0.1.0",
>   "evidence": "Prior local 20/30 second production; not a guarantee for other devices",
>   "source": {
>     "url": "https://github.com/dgrauet/ltx-2-mlx.git",
>     "commit": "747b30ef10e92a011e85b2aa0f6c4ee86ced5218"
>   },
>   "models": [
>     {
>       "role": "video",
>       "repo": "dgrauet/ltx-2.3-mlx-q4",
>       "revision": "f306259444116d71927188af2fbbf7fdc1a8089a",
>       "local": ".cache/models/ltx-q4",
>       "files": [
>         {
>           "path": "LICENSE",
>           "bytes": 19197
>         },
>         {
>           "path": "config.json",
>           "bytes": 951
>         },
>         {
>           "path": "connector.safetensors",
>           "bytes": 6344495512
>         },
>         {
>           "path": "embedded_config.json",
>           "bytes": 7234
>         },
>         {
>           "path": "quantize_config.json",
>           "bytes": 100
>         },
>         {
>           "path": "spatial_upscaler_x2_v1_1.safetensors",
>           "bytes": 995745061
>         },
>         {
>           "path": "spatial_upscaler_x2_v1_1_config.json",
>           "bytes": 275
>         },
>         {
>           "path": "split_model.json",
>           "bytes": 1893
>         },
>         {
>           "path": "transformer-distilled-1.1.safetensors",
>           "bytes": 11322002285
>         },
>         {
>           "path": "vae_decoder.safetensors",
>           "bytes": 814349531
>         },
>         {
>           "path": "vae_encoder.safetensors",
>           "bytes": 637885319
>         },
>         {
>           "path": "audio_vae.safetensors",
>           "bytes": 106509048
>         },
>         {
>           "path": "vocoder.safetensors",
>           "bytes": 258313851
>         }
>       ]
>     },
>     {
>       "role": "text",
>       "repo": "mlx-community/gemma-3-12b-it-4bit",
>       "revision": "86cc6a8dedbc456dd0e4af01a9d09f396f77e558",
>       "local": ".cache/models/gemma-q4",
>       "files": [
>         {
>           "path": "README.md",
>           "bytes": 945
>         },
>         {
>           "path": "chat_template.json",
>           "bytes": 1615
>         },
>         {
>           "path": "config.json",
>           "bytes": 1141
>         },
>         {
>           "path": "generation_config.json",
>           "bytes": 192
>         },
>         {
>           "path": "model-00001-of-00002.safetensors",
>           "bytes": 5367455313
>         },
>         {
>           "path": "model-00002-of-00002.safetensors",
>           "bytes": 2661219935
>         },
>         {
>           "path": "model.safetensors.index.json",
>           "bytes": 108605
>         },
>         {
>           "path": "preprocessor_config.json",
>           "bytes": 570
>         },
>         {
>           "path": "processor_config.json",
>           "bytes": 70
>         }
>       ]
>     },
>     {
>       "role": "voice",
>       "repo": "mlx-community/Qwen3-TTS-12Hz-1.7B-VoiceDesign-bf16",
>       "revision": "7d3824abff87e49756bb0f83fb5411de75d160c4",
>       "local": ".cache/tts-models/qwen3-voicedesign-bf16",
>       "files": [
>         {
>           "path": ".gitattributes",
>           "bytes": 1519
>         },
>         {
>           "path": "README.md",
>           "bytes": 1068
>         },
>         {
>           "path": "config.json",
>           "bytes": 5232
>         },
>         {
>           "path": "generation_config.json",
>           "bytes": 245
>         },
>         {
>           "path": "merges.txt",
>           "bytes": 1671839
>         },
>         {
>           "path": "model.safetensors",
>           "bytes": 3833402589
>         },
>         {
>           "path": "model.safetensors.index.json",
>           "bytes": 32458
>         },
>         {
>           "path": "preprocessor_config.json",
>           "bytes": 127
>         },
>         {
>           "path": "vocab.json",
>           "bytes": 2776833
>         }
>       ]
>     },
>     {
>       "role": "asr-optional",
>       "repo": "mlx-community/Qwen3-ASR-0.6B-4bit",
>       "revision": "313d850181767edf09f00a9c289becca70e58cd0",
>       "local": ".cache/tts-models/qwen3-asr-4bit",
>       "files": [
>         {
>           "path": ".gitattributes",
>           "bytes": 1519
>         },
>         {
>           "path": "README.md",
>           "bytes": 1008
>         },
>         {
>           "path": "chat_template.json",
>           "bytes": 1161
>         },
>         {
>           "path": "config.json",
>           "bytes": 7187
>         },
>         {
>           "path": "generation_config.json",
>           "bytes": 142
>         },
>         {
>           "path": "merges.txt",
>           "bytes": 1671853
>         },
>         {
>           "path": "model.safetensors",
>           "bytes": 708236945
>         },
>         {
>           "path": "model.safetensors.index.json",
>           "bytes": 71814
>         },
>         {
>           "path": "preprocessor_config.json",
>           "bytes": 330
>         },
>         {
>           "path": "vocab.json",
>           "bytes": 2776833
>         }
>       ]
>     }
>   ],
>   "referenceHost": {
>     "hardware": "Apple M3 Pro",
>     "unifiedMemoryGiB": 18,
>     "python": "3.13",
>     "videoMemoryLimitGiB": 11,
>     "videoCacheGiB": 0,
>     "compute": "default float32",
>     "nativeFps": 24,
>     "nativeSize": [
>       1024,
>       576
>     ]
>   }
> }
> ```


工具底层模型未公开时按“未披露”记录；不能由 imagegen 工具名猜测图像模型。未提供真实推理日志的配置不记作已运行。

## 镜头、时间轴与采用参数

> [!info]- 配置：production.json
> ```json
> {
>   "schemaVersion": 1,
>   "title": "Technical skill smoke fixture",
>   "format": {
>     "width": 1280,
>     "height": 720,
>     "fps": 24,
>     "durationFrames": 12
>   },
>   "fontFile": null,
>   "background": null,
>   "sfx": [],
>   "shots": [
>     {
>       "id": "scene-01",
>       "frames": 12,
>       "action": "",
>       "art": "art/smoke.png",
>       "clip": "video/scene-01-v1.mp4",
>       "status": "approved",
>       "review": "docs/reviews/smoke-fixture.json",
>       "generation": null,
>       "caption": {
>         "text": "Technical fixture",
>         "startFrame": 0,
>         "endFrame": 12
>       },
>       "dialogue": {
>         "text": "Synthetic test pulse, not speech",
>         "file": "audio/voices/smoke.wav",
>         "sync": "voiceover",
>         "fromFrame": 3,
>         "driverFile": "audio/drivers/scene-01-v1.wav",
>         "driverSha256": "2d378e758bef5d67dd18f727f26a8d2f2b04666dc7a44fbcba996a1029a1444e",
>         "voiceSha256": "c707eb834231346fb362c25f824f86e04f33752fc69f523784eb3db88332a939"
>       }
>     }
>   ]
> }
> ```

> [!info]- 当前 Composition / 时间轴源码：videos/anime-scene-forge-skill-smoke-20261111/src/Root.tsx，L1–32
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

### 项目记录的技术、内容或素材来源

这些 URL 来自项目文件，尚不自动证明本片采用；除本节明确标注的访问外，本轮未逐一在线重读。

| 链接 | 记录文件 |
| --- | --- |
| [github.com](https://github.com/dgrauet/ltx-2-mlx.git) | `videos/anime-scene-forge-skill-smoke-20261111/docs/mlx-profile.json` |

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

> [!info]- videos/anime-scene-forge-skill-smoke-20261111/scripts/project_ops.py，L1–213
> ```python
> """Project-local, standard-library helpers. Technical checks do not approve art."""
> import project_env
> import argparse,hashlib,json,math,re,subprocess,wave,zipfile
> from fractions import Fraction
> from pathlib import Path,PureWindowsPath
> 
> ROOT=Path(__file__).resolve().parents[1]
> 
> def require(ok,message):
>     if not ok:raise ValueError(message)
> 
> def local(name,public=False):
>     require(isinstance(name,str) and name and not Path(name).is_absolute() and not PureWindowsPath(name).is_absolute(),f'Use a local relative path: {name}')
>     if public:require('\\' not in name,'Use forward slashes in browser-facing asset paths')
>     base=ROOT/'public' if public else ROOT
>     p=base/name
>     require(p.resolve().is_relative_to(base.resolve()),f'Path escapes project: {name}')
>     for q in [p,*p.parents]:
>         if q==base.parent:break
>         require(not q.is_symlink(),f'Symlink not allowed: {name}')
>     return p
> 
> def sha(p):
>     h=hashlib.sha256()
>     with p.open('rb') as f:
>         for part in iter(lambda:f.read(1024*1024),b''):h.update(part)
>     return h.hexdigest()
> 
> def load():return json.loads((ROOT/'production.json').read_text())
> def save(data):
>     path=ROOT/'.cache/tmp/production-next.json';path.write_text(json.dumps(data,ensure_ascii=False,indent=2)+'\n');path.replace(ROOT/'production.json')
> def emit(path,data):path.parent.mkdir(parents=True,exist_ok=True);path.write_text(json.dumps(data,ensure_ascii=False,indent=2)+'\n')
> def integer(x):return isinstance(x,int) and not isinstance(x,bool)
> def call(cmd):return subprocess.run(cmd,cwd=ROOT,capture_output=True,text=True,check=True)
> def probe(path):return json.loads(call(['ffprobe','-v','error','-count_frames','-show_streams','-show_format','-of','json',str(path)]).stdout)
> def video_info(path):
>     m=probe(path);v=next(s for s in m['streams'] if s['codec_type']=='video')
>     n=v.get('nb_read_frames',v.get('nb_frames'))
>     require(n not in [None,'N/A'],f'Cannot count video frames: {path.name}')
>     return m,v,int(n)
> def pcm(path):
>     with wave.open(str(path),'rb') as f:
>         require((f.getframerate(),f.getnchannels(),f.getsampwidth())==(48000,2,2),f'Use 48kHz stereo PCM16 WAV: {path.name}')
>         return f.readframes(f.getnframes())
> def get_shot(data,name):
>     shots=[s for s in data['shots'] if s['id']==name]
>     require(len(shots)==1,f'Unknown or duplicate shot: {name}');return shots[0]
> 
> def validate(stage='final'):
>     data=load();require(data.get('schemaVersion')==1,'Unsupported production schema version');fmt=data['format'];fps=fmt['fps'];total=fmt['durationFrames']
>     require(all(integer(fmt[k]) and fmt[k]>0 for k in ['fps','width','height','durationFrames']),'Format requires positive integers')
>     require(fmt['width']%2==fmt['height']%2==0,'Video dimensions must be even')
>     require(data['shots'] and sum(s['frames'] for s in data['shots'])==total,'Shot durations must sum to durationFrames')
>     ids=[s['id'] for s in data['shots']]
>     require(len(ids)==len(set(ids)) and all(re.fullmatch(r'[a-zA-Z0-9-]+',s) for s in ids),'Use unique alphanumeric/hyphen shot IDs')
>     for folder in ['src','public','scripts']:
>         for p in (ROOT/folder).rglob('*'):
>             require(not p.is_symlink() and p.resolve().is_relative_to(ROOT),f'Nonlocal file: {p}')
>             if p.is_file():require(p.stat().st_nlink==1,f'Hard link not allowed: {p}')
>     for s in data['shots']:
>         require(integer(s['frames']) and s['frames']>0,f'Invalid duration: {s["id"]}')
>         if s.get('art'):require(local(s['art'],True).is_file(),f'Missing artwork: {s["id"]}')
>         if s.get('clip'):
>             path=local(s['clip'],True);require(path.is_file(),f'Missing clip: {s["id"]}')
>             _,v,n=video_info(path)
>             require(Fraction(v['avg_frame_rate'])==fps and n==s['frames'],f'Wrong native fps or frame count: {s["id"]}')
>             require((v['width'],v['height'])==(fmt['width'],fmt['height']),f'Wrong clip dimensions: {s["id"]}')
>         elif stage!='scaffold':raise ValueError(f'Missing footage: {s["id"]}')
>         c=s.get('caption')
>         if c:
>             require(isinstance(c['text'],str) and 0<=c['startFrame']<c['endFrame']<=s['frames'],f'Invalid caption: {s["id"]}')
>             if stage=='final' and any(ord(x)>127 for x in c['text']):require(data.get('fontFile'),'Provide a project-local font for non-ASCII captions')
>         d=s.get('dialogue')
>         if d:
>             require(d.get('sync') in ['on-screen','voiceover'] and isinstance(d.get('text'),str) and d['text'].strip(),f'Invalid dialogue description: {s["id"]}')
>             require(integer(d['fromFrame']) and 0<=d['fromFrame']<s['frames'],f'Invalid dialogue start: {s["id"]}')
>             if not d.get('file'):
>                 require(stage=='scaffold',f'Missing voice: {s["id"]}');continue
>             voice=pcm(local(d['file'],True));spf=Fraction(48000,fps)
>             require(spf.denominator==1,'Dialogue driver requires an integer sample count per frame')
>             offset=d['fromFrame']*int(spf)*4
>             require(offset+len(voice)<=s['frames']*int(spf)*4,f'Voice exceeds selected shot: {s["id"]}')
>             if d.get('driverFile'):
>                 driver_path=local(d['driverFile'],True);driver=pcm(driver_path)
>                 require(sha(driver_path)==d['driverSha256'],'Driver changed; regenerate the speaking clip')
>                 require(driver[:offset]==bytes(offset) and driver[offset:offset+len(voice)]==voice and driver[offset+len(voice):]==bytes(len(driver)-offset-len(voice)),'Driver differs from final voice or timing')
>                 require(sha(local(d['file'],True))==d['voiceSha256'],'Voice changed; create a new driver and speaking take')
>             if d['sync']=='on-screen' and stage!='scaffold':
>                 require(d.get('driverFile') and s.get('generation'),f'Missing A2V provenance: {s["id"]}')
>                 g=json.loads(local(s['generation']).read_text())
>                 require(g['mode']=='a2v' and g['audioSha256']==d['driverSha256'],f'Voice/footage mismatch: {s["id"]}')
>                 raw=local(g['output']);require(sha(raw)==g['outputSha256'],'Raw candidate changed')
>         if stage=='final':
>             require(s.get('status')=='approved' and s.get('review'),f'Visual review pending: {s["id"]}')
>             review=json.loads(local(s['review']).read_text())
>             require(review.get('visualReviewed') is True and review.get('normalSpeedReviewed') is True and review.get('reviewer') and review.get('observations'),f'Incomplete visual record: {s["id"]}')
>             require(review['clipSha256']==sha(local(s['clip'],True)),f'Stale visual review: {s["id"]}')
>     if data.get('fontFile'):require(local(data['fontFile'],True).is_file(),'Missing font')
>     if data.get('background'):
>         b=data['background'];require(local(b['file'],True).is_file() and isinstance(b['volume'],(int,float)) and math.isfinite(b['volume']) and b['volume']>=0,'Invalid background')
>     for e in data.get('sfx',[]):
>         require(integer(e['frame']) and 0<=e['frame']<total and local(e['file'],True).is_file(),'Invalid sound event')
>     return data
> 
> def select(a):
>     data=load();s=get_shot(data,a.shot);source=local(a.source)
>     _,v,n=video_info(source);fmt=data['format'];fps=fmt['fps']
>     require(Fraction(v['avg_frame_rate'])==fps,'Source fps differs; choose native timeline fps or explicitly redesign conversion')
>     require(a.start>=0 and a.start+s['frames']<=n,'Selected interval exceeds source')
>     w,h=v['width'],v['height'];filters=[]
>     if a.crop:
>         x,y,w,h=a.crop;require(min(x,y)>=0 and min(w,h)>0 and x+w<=v['width'] and y+h<=v['height'],'Invalid crop')
>         filters.append(f'crop={w}:{h}:{x}:{y}')
>     require(abs(w/h-fmt['width']/fmt['height'])<.01,'Aspect differs: provide an intentional crop or regenerate the correct framing')
>     filters+= [f'scale={fmt["width"]}:{fmt["height"]}:flags=lanczos','setsar=1']
>     target=ROOT/f'public/video/{s["id"]}-{a.take}.mp4';require(not target.exists(),'Preserving existing selection; use a new take')
>     if a.generation:
>         g=json.loads(local(a.generation).read_text());require(g['outputSha256']==sha(source),'Generation record is for different source footage')
>     call(['ffmpeg','-y','-v','error','-ss',f'{a.start/fps:.12f}','-i',str(source),'-map','0:v:0','-an','-frames:v',str(s['frames']),'-vf',','.join(filters),'-c:v','libx264','-crf','16','-preset','fast','-pix_fmt','yuv420p','-fps_mode','passthrough','-movflags','+faststart',str(target)])
>     record={'source':source.relative_to(ROOT).as_posix(),'sourceSha256':sha(source),'startFrame':a.start,'frames':s['frames'],'fps':fps,'speed':1,'crop':a.crop,'output':target.relative_to(ROOT).as_posix(),'outputSha256':sha(target)}
>     emit(ROOT/f'docs/selections/{s["id"]}-{a.take}.json',record)
>     s.update(clip=target.relative_to(ROOT/'public').as_posix(),status='candidate',review=None,generation=a.generation)
>     save(data);print(json.dumps(record,indent=2))
> 
> def pad(a):
>     data=load();s=get_shot(data,a.shot);fps=data['format']['fps'];d=s.get('dialogue')
>     require(d and d.get('text'),'Fill shot.dialogue text/sync before preparing its voice')
>     require(a.from_frame>=0 and a.source_frames>=s['frames'] and 48000%fps==0,'Invalid source duration or audio offset')
>     path=local(a.voice);voice=pcm(path);spf=48000//fps;offset=a.from_frame*spf*4
>     require(offset+len(voice)<=s['frames']*spf*4,'Voice exceeds adopted shot; revise line/take/timeline at normal speed')
>     target=ROOT/f'public/audio/drivers/{s["id"]}-{a.take}.wav';require(not target.exists(),'Preserving existing driver; use a new take')
>     body=bytes(offset)+voice+bytes(a.source_frames*spf*4-offset-len(voice))
>     with wave.open(str(target),'wb') as f:f.setnchannels(2);f.setsampwidth(2);f.setframerate(48000);f.writeframes(body)
>     d.update(file=path.relative_to(ROOT/'public').as_posix(),fromFrame=a.from_frame,driverFile=target.relative_to(ROOT/'public').as_posix(),driverSha256=sha(target),voiceSha256=sha(path))
>     s.update(status='candidate',review=None);save(data)
>     print(json.dumps({'driver':d['driverFile'],'voiceSha256':d['voiceSha256'],'frames':a.source_frames,'speed':1},indent=2))
> 
> def sheet(a):
>     data=load();s=get_shot(data,a.shot);require(s.get('clip'),'Select footage first')
>     source=local(s['clip'],True);_,_,n=video_info(source)
>     out=ROOT/f'outputs/checks/{s["id"]}/{a.label}';out.mkdir(parents=True,exist_ok=True)
>     call(['ffmpeg','-y','-v','error','-i',str(source),str(out/'frame-%04d.png')])
>     vf=[]
>     if a.crop:
>         x,y,w,h=a.crop;require(min(x,y)>=0 and min(w,h)>0,'Invalid review crop');vf+=[f'crop={w}:{h}:{x}:{y}']
>     vf+=['scale=240:-1',f'tile=5x{math.ceil(n/5)}']
>     call(['ffmpeg','-y','-v','error','-i',str(source),'-vf',','.join(vf),'-frames:v','1','-update','1',str(out/'all-frames.png')])
>     emit(out/'index.json',{'clipSha256':sha(source),'crop':a.crop,'columns':5,'frames':[{'sourceFrame':i,'filename':f'frame-{i+1:04d}.png','tileRow':i//5,'tileColumn':i%5} for i in range(n)],'visualApproval':'not automatically assigned'})
>     print(out/'all-frames.png')
> 
> def fingerprint(data):
>     paths=[ROOT/n for n in ['production.json','package.json','package-lock.json','tsconfig.json','remotion.config.ts']]
>     paths+=list((ROOT/'src').rglob('*.ts'))+list((ROOT/'src').rglob('*.tsx'))
>     media=[]
>     for s in data['shots']:
>         media+=[s.get('clip')]
>         if s.get('dialogue'):media+=[s['dialogue'].get('file')]
>     media+=[data.get('fontFile')]
>     if data.get('background'):media+=[data['background']['file']]
>     media+=[e['file'] for e in data.get('sfx',[])]
>     paths += [local(m,True) for m in media if m]
>     return {p.relative_to(ROOT).as_posix():sha(p) for p in sorted(set(paths))}
> 
> def verify(rendered=False):
>     data=validate();movie=ROOT/'outputs/final.mp4';require(movie.is_file(),'Export final.mp4 first')
>     state=ROOT/'outputs/checks/export.json';inputs=fingerprint(data)
>     if not rendered:
>         require(state.is_file(),'No render provenance; export using npm run render')
>         prior=json.loads(state.read_text());require(prior['inputs']==inputs and prior['sha256']==sha(movie),'Media/source changed after render; export again')
>     meta,v,n=video_info(movie);fmt=data['format']
>     require(v['codec_name']=='h264' and n==fmt['durationFrames'] and Fraction(v['avg_frame_rate'])==fmt['fps'],'Wrong export codec/fps/frames')
>     require((v['width'],v['height'])==(fmt['width'],fmt['height']),'Wrong export dimensions')
>     require(abs(float(v['duration'])-n/fmt['fps'])<.05,'Wrong export duration')
>     if data.get('background') or data.get('sfx') or any(s.get('dialogue') for s in data['shots']):require(any(s['codec_type']=='audio' for s in meta['streams']),'Exported audio missing')
>     call(['ffmpeg','-v','error','-i',str(movie),'-f','null','-'])
>     result={'status':'technical-pass','scope':'Visual status is read from authored reviews, not assessed by these checks.','file':'outputs/final.mp4','sha256':sha(movie),'frames':n,'fps':fmt['fps'],'duration':n/fmt['fps'],'inputs':inputs}
>     emit(state,result);return result
> 
> def package():
>     report=verify();files=[ROOT/n for n in ['package.json','package-lock.json','.npmrc','.gitignore','README.md','tsconfig.json','remotion.config.ts','production.json']]
>     for folder in ['src','public','scripts','docs']:
>         files += [p for p in (ROOT/folder).rglob('*') if p.is_file() and '__pycache__' not in p.parts]
>     files += list(ROOT.glob('requirements*.txt'))+[ROOT/'outputs/final.mp4',ROOT/'outputs/checks/export.json']
>     target=ROOT/'outputs/project.zip'
>     with zipfile.ZipFile(target,'w',zipfile.ZIP_DEFLATED,compresslevel=6) as z:
>         for p in sorted(set(files)):
>             require(p.resolve().is_relative_to(ROOT) and not p.is_symlink(),'Archive path escape');z.write(p,(Path(ROOT.name)/p.relative_to(ROOT)).as_posix())
>     with zipfile.ZipFile(target) as z:
>         require(z.testzip() is None,'ZIP CRC failed')
>         require(hashlib.sha256(z.read((Path(ROOT.name)/'outputs/final.mp4').as_posix())).hexdigest()==report['sha256'],'Archived movie differs')
>     result={'file':'outputs/project.zip','files':len(set(files)),'bytes':target.stat().st_size,'sha256':sha(target),'crc':'passed'}
>     emit(ROOT/'outputs/checks/package.json',result);return result
> 
> def main():
>     p=argparse.ArgumentParser();sub=p.add_subparsers(dest='command',required=True)
>     v=sub.add_parser('validate');v.add_argument('--stage',choices=['scaffold','draft','final'],default='final')
>     s=sub.add_parser('select');s.add_argument('shot');s.add_argument('--source',required=True);s.add_argument('--start',type=int,default=0);s.add_argument('--take',required=True);s.add_argument('--crop',nargs=4,type=int);s.add_argument('--generation')
>     d=sub.add_parser('pad-voice');d.add_argument('shot');d.add_argument('--voice',required=True);d.add_argument('--from-frame',type=int,required=True);d.add_argument('--source-frames',type=int,required=True);d.add_argument('--take',required=True)
>     f=sub.add_parser('sheet');f.add_argument('shot');f.add_argument('--crop',nargs=4,type=int);f.add_argument('--label',default='full')
>     e=sub.add_parser('verify-export');e.add_argument('--rendered',action='store_true')
>     sub.add_parser('package');a=p.parse_args()
>     if hasattr(a,'take'):require(re.fullmatch(r'[a-zA-Z0-9-]+',a.take),'Use a safe take identifier')
>     if hasattr(a,'label'):require(re.fullmatch(r'[a-zA-Z0-9-]+',a.label),'Use a safe label')
>     if a.command=='validate':data=validate(a.stage);print(json.dumps({'status':'passed','stage':a.stage,'frames':data['format']['durationFrames'],'scope':'Technical and provenance checks only'},indent=2))
>     elif a.command=='select':select(a)
>     elif a.command=='pad-voice':pad(a)
>     elif a.command=='sheet':sheet(a)
>     elif a.command=='verify-export':print(json.dumps(verify(a.rendered),indent=2))
>     elif a.command=='package':print(json.dumps(package(),indent=2))
> 
> if __name__=='__main__':
>     try:main()
>     except (ValueError,KeyError,FileNotFoundError,wave.Error,subprocess.CalledProcessError) as e:raise SystemExit(str(e))
> ```

## 完整关键提示词与生成记录

以下保留项目自有提示词、候选参数与版本。存在失败候选和历史参数；最终采用以 selected-takes、edit-decisions 与制作复盘为准，不能将所有候选都写成已采用。

> [!info]- videos/anime-scene-forge-skill-smoke-20261111/docs/prompts/smoke.txt
> ```text
> Technical dry-run fixture only; no character generation requested.
> ```

## 已有产物与检查依据

文件存在、技术检查、画面检查与用户验收分别看待。下表是本次文件盘点，旧版与当前版可能共存。MP4/HTML 按内容计算 SHA-256，ZIP 仅记录文件大小和修改时间供增量判断。

| 本地产物 | 字节数 | SHA-256 |
| --- | ---: | --- |
| `videos/anime-scene-forge-skill-smoke-20261111/outputs/final.mp4` | 295875 | `1c646460e1c459a135f36648e28d2e80d81d986a97717f25fcd85577e8bedb05` |
| `videos/anime-scene-forge-skill-smoke-20261111/outputs/project.zip` | 1015105 | `ZIP：仅元数据` |
| `videos/anime-scene-forge-skill-smoke-20261111/outputs/review-candidate.mp4` | 295875 | `1c646460e1c459a135f36648e28d2e80d81d986a97717f25fcd85577e8bedb05` |

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
>       "duration": "0.500000",
>       "nb_frames": "12",
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
>       "duration": "0.512000",
>       "nb_frames": "26"
>     }
>   ],
>   "format": {
>     "duration": "0.512000",
>     "size": "295875"
>   },
>   "file": "outputs/final.mp4",
>   "scope": "本次只读取媒体头；未全片解码、播放或重新进行视觉/听觉验收。"
> }
> ```


已有检查报告路径（未在本轮重新执行）：

- `videos/anime-scene-forge-skill-smoke-20261111/outputs/checks/export.json`
- `videos/anime-scene-forge-skill-smoke-20261111/outputs/checks/package.json`
- `videos/anime-scene-forge-skill-smoke-20261111/outputs/checks/skill-smoke-report.json`
- `videos/anime-scene-forge-skill-smoke-20261111/outputs/checks/scene-01/full/index.json`
- `videos/anime-scene-forge-skill-smoke-20261111/outputs/checks/scene-01/region/index.json`

## 制作方法与迭代记录选编

下面是项目自有文档的归档摘录，保留原阶段与原检查表述，个人路径已泛化、相对链接已注明源项目位置。它们属于来源陈述，不代表本次重新完成其中的生成或验收。

> [!info]- 来源文档：videos/anime-scene-forge-skill-smoke-20261111/README.md
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
> - Keep complete reference artwork in `public/art`, raw candidates in `public/candidates`, and selected clips in `public/video`. Record prompts and backend versions under `docs`.
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

> [!info]- 来源文档：videos/anime-scene-forge-skill-smoke-20261111/docs/brief.md
> # Technical skill smoke fixture
> 
> 状态：制作规划，未生成成片。
> 
> - 时长：0.5秒，24fps，12帧。
> - 画幅：1280×720。
> - 角色、造型、场景与参考来源：待按用户要求填写。
> - 故事与动作：按叙事修改production.json中的默认等长分段。
> - 对白模式、语言、声线与情绪：待按用户要求填写。
> - 图像与生成后端：mlx；实际能力、版本和预算需记录。
> - 交付：按用户要求提供预览、MP4和独立工程。

> [!info]- 来源文档：videos/anime-scene-forge-skill-smoke-20261111/docs/production-method.md
> # 实际制作记录
> 
> 状态：尚未制作。完成后记录真实版本、候选、失败、修正、检查、耗时与限制。

## 证据清单与缺失信息

盘点的文本/报告文件 31 个，源文本、报告及媒体产物身份摘要为 `495c94703e781f6a74d32d7df6b1d1193a0a599d2932e60d6dfbee10c4fd7cdc`。哈希证明采集内容身份，不代表其中每个断言已验证。模型权重、缓存、依赖、第三方源码树和未指定的历史会话未采集；大型文本仅做哈希并按需要选读。

- 本次整理基于明确引用的 README、制作方法、配置、提示词、关键源码与已有报告。未重新观看全部参考/候选/成片，也未做人工听辨。
- 项目未提供的原始模型内部实现、未披露生成后端、未记录 seed/参数、真实费用、人工总工时和用户最终验收，均不补造。实际日志有记录的耗时见上文，流水线耗时不等于整个制作总耗时。
- 源项目内的路径只用于查证；本档案未复制大型素材或模型，也不改变源项目运行依赖。

| 已盘点证据文件 | SHA-256（前 16 位）/状态 |
| --- | --- |
| `videos/anime-scene-forge-skill-smoke-20261111/README.md` | `89dbc25c78053962` |
| `videos/anime-scene-forge-skill-smoke-20261111/package-lock.json` | `bc31e3654ea5fa81` |
| `videos/anime-scene-forge-skill-smoke-20261111/package.json` | `e256953722fff4a6` |
| `videos/anime-scene-forge-skill-smoke-20261111/production.json` | `f12d87e16feaaca0` |
| `videos/anime-scene-forge-skill-smoke-20261111/remotion.config.ts` | `5ad026e95030791b` |
| `videos/anime-scene-forge-skill-smoke-20261111/requirements-local-video.txt` | `022301641961a33b` |
| `videos/anime-scene-forge-skill-smoke-20261111/requirements-tts.txt` | `1a42a1f23876f2df` |
| `videos/anime-scene-forge-skill-smoke-20261111/tsconfig.json` | `c414c90c7e726c32` |
| `videos/anime-scene-forge-skill-smoke-20261111/docs/brief.md` | `f618877c162a0827` |
| `videos/anime-scene-forge-skill-smoke-20261111/docs/mlx-profile.json` | `ee29a23fd6fc1cc6` |
| `videos/anime-scene-forge-skill-smoke-20261111/docs/production-method.md` | `8e9c4ca8a8e974ac` |
| `videos/anime-scene-forge-skill-smoke-20261111/docs/prompts/smoke.txt` | `1c64d7304dc9ddfc` |
| `videos/anime-scene-forge-skill-smoke-20261111/docs/reviews/smoke-fixture.json` | `a599e79b92f61e3a` |
| `videos/anime-scene-forge-skill-smoke-20261111/docs/selections/scene-01-v1.json` | `8ca4f07bedfba579` |
| `videos/anime-scene-forge-skill-smoke-20261111/outputs/checks/export.json` | `72a31dcecf16d87f` |
| `videos/anime-scene-forge-skill-smoke-20261111/outputs/checks/package.json` | `f55f54b8f76dac98` |
| `videos/anime-scene-forge-skill-smoke-20261111/outputs/checks/skill-smoke-report.json` | `1ef38ba8269e449b` |
| `videos/anime-scene-forge-skill-smoke-20261111/outputs/checks/scene-01/full/index.json` | `80127c620affbe6d` |
| `videos/anime-scene-forge-skill-smoke-20261111/outputs/checks/scene-01/region/index.json` | `e4178782ac5c4273` |
| `videos/anime-scene-forge-skill-smoke-20261111/scripts/fetch_mlx_models.py` | `0a97c44397d549b8` |
| `videos/anime-scene-forge-skill-smoke-20261111/scripts/gpu_lock.py` | `0a0fcf40a6176616` |
| `videos/anime-scene-forge-skill-smoke-20261111/scripts/lowram_prompt.py` | `f8267e06ce389727` |
| `videos/anime-scene-forge-skill-smoke-20261111/scripts/ltx_entry.py` | `6643f955c03c6588` |
| `videos/anime-scene-forge-skill-smoke-20261111/scripts/project_env.py` | `9fa4016b6cce04fe` |
| `videos/anime-scene-forge-skill-smoke-20261111/scripts/project_ops.py` | `9d07bbd3574305df` |
| `videos/anime-scene-forge-skill-smoke-20261111/scripts/run.mjs` | `753d9012ec380054` |
| `videos/anime-scene-forge-skill-smoke-20261111/scripts/run_mlx.py` | `5405992a350ffd3f` |
| `videos/anime-scene-forge-skill-smoke-20261111/scripts/setup_mlx.py` | `734bde5bd1bf89fb` |
| `videos/anime-scene-forge-skill-smoke-20261111/scripts/synthesize_mlx.py` | `175ef3cdaf2b9a0b` |
| `videos/anime-scene-forge-skill-smoke-20261111/src/Root.tsx` | `90bb85ddcbaa652a` |
| `videos/anime-scene-forge-skill-smoke-20261111/src/index.ts` | `f9bc4e836a064aca` |
