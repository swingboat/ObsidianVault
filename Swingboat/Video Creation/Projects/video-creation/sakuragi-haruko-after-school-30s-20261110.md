---
type: video-production-dossier
project: video-creation
video_id: sakuragi-haruko-after-school-30s-20261110
captured: '2026-11-12'
status: 当前口型修复版已交付；源记录未写用户自然度最终验收
source_fingerprint: 4979832c7734cb57a9dfeee5c91e6b7ab12321dc21f03642aadf5d5bb7a53085
tags:
- video-creation
- production-capture
---

# 樱木与晴子 · 30 秒角色表演、配音与口型修复

<!-- capture-source-sha256:4979832c7734cb57a9dfeee5c91e6b7ab12321dc21f03642aadf5d5bb7a53085 -->

本次为指定项目的材料回溯与归档。制作结论来自项目记录/源码，未找回关闭的制作对话，也未重新执行生成、渲染、音频合成或用户验收。

> 本页的环境日志与模型文件清单已删节。

## 当前结论与制作路线

**状态：当前口型修复版已交付；源记录未写用户自然度最终验收。**

统一原画 → LTX 连续动作 → Qwen3-TTS 配音 → 音轨驱动 A2V 重生成 → 剪辑。

1280×720、24fps、720 帧。初版采用无对白连续动作和字幕，随后加入 6 句国语角色声音，最后因用户指出嘴型不匹配，重生成 6 处讲话镜头。当前 Root 为八个主要 Series 段，早期文档的十切镜表属于旧剪辑阶段，不能混写。

## 关键算法、实现与适用边界

先统一角色身份、机位与光线；LTX 原生 1024×576、24fps 生成，原速选帧放到 720p 时间轴。角色对白单独输出 WAV，以相同语速补足头尾静音，作为 a2v --distilled 的实际驱动；两阶段固定输入音频 latent，联合重生嘴、眉眼和头部。shy-v2 在源第 17/81 帧加闭口参考，抑制提前张嘴/转头变形。重生成后按接触帧重标拍手声。

## 声音与字幕

Qwen3-TTS VoiceDesign 按文字声线/情绪设计国语角色，未用原演员录音；Qwen3-ASR 检查可识别内容。12 小节、96BPM 原创合成配乐对白时 ducking。当前六条对白 WAV 与旧配音逐采样相同，拍手对齐全片第 445 帧；本工具环境的原记录不宣称人工听感或原声相似度验收。

## 失败、修正与验收边界

运球 v1 多球和手型失败被拒；害羞 v1 提前张嘴、眼睛变形被拒；长动作后段舍弃。字幕窗口、ASR、总时长和能量包络均不能替代口型视觉验收。配音版画面流不变只适用于旧阶段，口型修复已改变画面流。

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


> [!info]- 版本证据：docs/voiceover/asr-model-info.json（配置存在与实际使用分开）
> ```json
> {
>   "repo": "mlx-community/Qwen3-ASR-0.6B-4bit",
>   "revision": "313d850181767edf09f00a9c289becca70e58cd0",
>   "local": ".cache/tts-models/qwen3-asr-4bit",
>   "files": [
>     {
>       "path": ".gitattributes",
>       "bytes": 1519
>     },
>     {
>       "path": "README.md",
>       "bytes": 1008
>     },
>     {
>       "path": "chat_template.json",
>       "bytes": 1161
>     },
>     {
>       "path": "config.json",
>       "bytes": 7187
>     },
>     {
>       "path": "generation_config.json",
>       "bytes": 142
>     },
>     {
>       "path": "merges.txt",
>       "bytes": 1671853
>     },
>     {
>       "path": "model.safetensors",
>       "bytes": 708236945
>     },
>     {
>       "path": "model.safetensors.index.json",
>       "bytes": 71814
>     },
>     {
>       "path": "preprocessor_config.json",
>       "bytes": 330
>     },
>     {
>       "path": "vocab.json",
>       "bytes": 2776833
>     }
>   ],
>   "verified_file_sizes": true
> }
> ```


> [!info]- 版本证据：docs/voiceover/model-info.json（配置存在与实际使用分开）
> ```json
> {
>   "repo": "mlx-community/Qwen3-TTS-12Hz-1.7B-VoiceDesign-bf16",
>   "revision": "7d3824abff87e49756bb0f83fb5411de75d160c4",
>   "files": [
>     {
>       "path": ".gitattributes",
>       "bytes": 1519
>     },
>     {
>       "path": "README.md",
>       "bytes": 1068
>     },
>     {
>       "path": "config.json",
>       "bytes": 5232
>     },
>     {
>       "path": "generation_config.json",
>       "bytes": 245
>     },
>     {
>       "path": "merges.txt",
>       "bytes": 1671839
>     },
>     {
>       "path": "model.safetensors",
>       "bytes": 3833402589
>     },
>     {
>       "path": "model.safetensors.index.json",
>       "bytes": 32458
>     },
>     {
>       "path": "preprocessor_config.json",
>       "bytes": 127
>     },
>     {
>       "path": "vocab.json",
>       "bytes": 2776833
>     }
>   ],
>   "local": ".cache/tts-models/qwen3-voicedesign-bf16",
>   "verified_file_sizes": true
> }
> ```


工具底层模型未公开时按“未披露”记录；不能由 imagegen 工具名猜测图像模型。未提供真实推理日志的配置不记作已运行。

## 镜头、时间轴与采用参数

> [!info]- 配置：docs/edit-decisions.json
> ```json
> {
>   "opening": {
>     "source": "opening-a2v-v1.mp4",
>     "start_frame": 0,
>     "frames": 96,
>     "review_status": "approved",
>     "note": "Audio-conditioned replacement. Haruko alone visibly speaks and returns to a closed smile around the end of her phrase; Sakuragi remains a quiet closed-mouth listener. Stable single held ball, hand grip, separated bodies and standing positions. All-frame face, listener and contact sheets plus browser 1x playback reviewed. Wide shot has limited mouth detail. Use native 0–95, no retiming."
>   },
>   "haruko": {
>     "source": "haruko-a2v-v1.mp4",
>     "start_frame": 0,
>     "frames": 96,
>     "review_status": "approved",
>     "note": "Audio-conditioned replacement. Reviewed all-face-frame sheet, full-scene overview, full-resolution speaking frame 34 and browser 1x playback to end. Mouth closes during the inter-phrase pause, reopens for the second phrase and settles into a closed smile after speech. Small nod and stable clasped hands. Initial open smile changes before voice onset; not an exact phoneme-match claim. Native frames 0–95, no retiming."
>   },
>   "shy": {
>     "source": "shy-a2v-v2.mp4",
>     "start_frame": 0,
>     "frames": 52,
>     "review_status": "approved",
>     "note": "Audio-conditioned replacement with extra clean closed-mouth guide images at source frames 17 and 81. Main speaking motion now begins around frame 20, close to voice activity around frame 19; an earlier small toothy smile is an expression rather than the sustained premature open mouth of v1. Stable three-quarter-right face and eyes through the final smile, with naturally connected hand resting behind the neck. All-face and arm-contact frames and 1x playback reviewed. Use continuous native source 0–85, split at 52 only for existing component boundaries; no omitted interval or retiming. Approximate anime articulation, not a phoneme-level certification."
>   },
>   "smile": {
>     "source": "shy-a2v-v2.mp4",
>     "start_frame": 52,
>     "frames": 34,
>     "review_status": "approved",
>     "note": "Audio-conditioned replacement with extra clean closed-mouth guide images at source frames 17 and 81. Main speaking motion now begins around frame 20, close to voice activity around frame 19; an earlier small toothy smile is an expression rather than the sustained premature open mouth of v1. Stable three-quarter-right face and eyes through the final smile, with naturally connected hand resting behind the neck. All-face and arm-contact frames and 1x playback reviewed. Use continuous native source 0–85, split at 52 only for existing component boundaries; no omitted interval or retiming. Approximate anime articulation, not a phoneme-level certification."
>   },
>   "practice": {
>     "source": "practice-native24-v2.mp4",
>     "start_frame": 0,
>     "frames": 96,
>     "review_status": "approved",
>     "note": "Rebuilt from new empty-hand stance keyframe. Overview, all knee/foot frames and 1x playback reviewed. Stable shoes, controlled knee bend and separate characters. Haruko makes a small coaching step but no crossing or contact. Use 0–95 at native speed."
>   },
>   "gather": {
>     "source": "gather-native24-v1.mp4",
>     "start_frame": 0,
>     "frames": 60,
>     "review_status": "approved",
>     "note": "Reviewed full-frame overview, all 73 hand/ball frames and 1x browser playback. Select frames 0–59. Exclude unrequested shooting after about frame 63 and blur near the end. No frame extension."
>   },
>   "cheer": {
>     "source": "cheer-a2v-v1.mp4",
>     "start_frame": 0,
>     "frames": 94,
>     "review_status": "approved",
>     "note": "Audio-conditioned replacement. Warm visible speaking followed by closed smile. Hands make one gentle palm contact at source frame 11, remain together, then lower. No repeated claps; update score to one event at global frame 445. Stable face and connected wrists, with some fine finger-line softness while hands settle. All-frame face/contact sheets, full-resolution speech frame and 1x playback reviewed. Native 0–93, no retiming."
>   },
>   "resolve": {
>     "source": "resolve-a2v-v1.mp4",
>     "start_frame": 0,
>     "frames": 96,
>     "review_status": "approved",
>     "note": "Audio-conditioned replacement. Reviewed full-scene overview, all 97 face frames with source-waveform energy, and 4s browser playback at 1x to end. Stable face, solo identity and fist; visible speaking, round-lip and closed-mouth changes followed by a closed smile. Broad speech timing improved; not a phoneme-level or original-animation match. Use native 0–95 without crop or retiming."
>   },
>   "ending": {
>     "source": "ending-a2v-v1.mp4",
>     "start_frame": 0,
>     "frames": 60,
>     "review_status": "approved",
>     "note": "Audio-conditioned replacement. Sakuragi visibly delivers the line and closes his mouth after the response; Haruko listens quietly with closed mouth. Ball remains singly held at left hip, right fist stays near chest, separate bodies and stable feet. All-frame speaker, listener and contact sheets plus browser 1x playback reviewed. Wide-shot facial lines remain low detail. Native 0–59, no retiming."
>   },
>   "epilogue": {
>     "source": "epilogue-native24-v1.mp4",
>     "start_frame": 0,
>     "frames": 36,
>     "review_status": "approved",
>     "note": "New close portrait keyframe, clean solo expression with complete hair/chin. Overview and 1x native playback checked. Use 0–35, 1.5s at native speed."
>   }
> }
> ```

> [!info]- 配置：docs/lipsync/selected-takes.json
> ```json
> {
>   "resolve": "v1",
>   "haruko": "v1",
>   "opening": "v1",
>   "ending": "v1",
>   "cheer": "v1",
>   "shy": "v2"
> }
> ```

> [!info]- 配置：docs/voiceover/selected-takes.json
> ```json
> {"opening":"v1","haruko":"v1","shy":"v1","cheer":"v1","resolve":"v1","ending":"v1"}
> ```

> [!info]- 配置：docs/voiceover/dialogue.json
> ```json
> {
>  "language":"Chinese",
>  "direction":"Classic Mandarin anime-dub feel, designed character voices. Not original actor recordings or an exact original-cast clone.",
>  "characters":{
>   "sakuragi":{"seed":1110,"voice_description":"年轻男性，十八岁左右的热血少年音色，中低音区，声音厚实有胸腔共鸣，轻微沙哑和鼻腔共鸣，带一点调皮和笨拙的自信。说标准国语，带非常轻微的九十年代台湾动画国语配音语感，咬字清楚，有活力但不是播音腔，不是大叔音，不用气泡音。表演有经典热血篮球动画男主角的夸张起伏，面对喜欢的女生会有一点害羞。自然、爽朗、真诚的角色对白。"},
>   "haruko":{"seed":2210,"voice_description":"年轻女性，十七岁左右温柔明亮的少女音色，中高音区，清澈自然、温暖亲切，带笑意和真诚的鼓励。说标准国语，带非常轻微的九十年代台湾动画国语配音语感，咬字清楚，声音柔和但有精神，不是幼儿音，不嗲，不是客服播音腔，没有气声。像经典篮球动画里开朗善良的女同学在球馆边鼓励朋友。自然连贯的角色对白。"}
>  },
>  "lines":[
>   {"id":"opening","character":"haruko","text":"樱木同学，再试一次吧。","start_frame":14,"end_frame":91,"emotion":"温柔、明亮，轻松地鼓励对方再尝试一次，句尾的吧轻轻扬起。"},
>   {"id":"haruko","character":"haruko","text":"先站稳，别着急。","start_frame":109,"end_frame":184,"emotion":"耐心地给建议，亲切自然，不说教。"},
>   {"id":"shy","character":"sakuragi","text":"交给我吧，晴子小姐！","start_frame":205,"end_frame":273,"emotion":"前半句自信地应下，叫晴子小姐时带一点开心又不好意思的笑意，语气活泼。"},
>   {"id":"cheer","character":"haruko","text":"对，就是这样！","start_frame":444,"end_frame":512,"emotion":"看见朋友有进步，真心高兴地轻快赞许，有笑意但不尖叫。"},
>   {"id":"resolve","character":"sakuragi","text":"我可是天才嘛。","start_frame":544,"end_frame":617,"emotion":"故作得意、俏皮地自夸，胸有成竹，句尾嘛有少年的笑意。"},
>   {"id":"ending","character":"sakuragi","text":"好，再来一遍！","start_frame":631,"end_frame":680,"emotion":"恢复热血，爽朗而干脆，准备再练一次，不拖长音，不是大喊。"}
>  ]
> }
> ```

> [!info]- 配置：docs/audio-events.json
> ```json
> {
>   "dribbles": [],
>   "claps": [
>     18.541666666666668
>   ],
>   "clap_source_frames": [
>     11
>   ],
>   "clap_timeline_frames": [
>     445
>   ],
>   "notes": "Audio-driven cheer replacement has one gentle palm contact at source frame 11, followed by hands together and lowering. Reviewed all-frame hand sheet and enlarged frames 7–14. Replaced the earlier four clap sounds with one contact-aligned event. No dribble event."
> }
> ```

> [!info]- 配置：docs/lipsync/input-layout.json
> ```json
> {
>   "opening": {
>     "frames": 97,
>     "fps": 24,
>     "audio_start_frame": 14,
>     "text": "樱木同学，再试一次吧。",
>     "source": "public/video/opening.mp4",
>     "voice": "public/audio/voices/opening.wav"
>   },
>   "haruko": {
>     "frames": 97,
>     "fps": 24,
>     "audio_start_frame": 13,
>     "text": "先站稳，别着急。",
>     "source": "public/video/haruko.mp4",
>     "voice": "public/audio/voices/haruko.wav"
>   },
>   "shy": {
>     "frames": 89,
>     "fps": 24,
>     "audio_start_frame": 13,
>     "text": "交给我吧，晴子小姐！",
>     "source": "public/video/shy.mp4",
>     "voice": "public/audio/voices/shy.wav"
>   },
>   "cheer": {
>     "frames": 97,
>     "fps": 24,
>     "audio_start_frame": 10,
>     "text": "对，就是这样！",
>     "source": "public/video/cheer.mp4",
>     "voice": "public/audio/voices/cheer.wav"
>   },
>   "ending": {
>     "frames": 65,
>     "fps": 24,
>     "audio_start_frame": 7,
>     "text": "好，再来一遍！",
>     "source": "public/video/ending.mp4",
>     "voice": "public/audio/voices/ending.wav"
>   },
>   "resolve": {
>     "frames": 97,
>     "fps": 24,
>     "audio_start_frame": 16,
>     "text": "我可是天才嘛。",
>     "source": "public/video/resolve.mp4",
>     "voice": "public/audio/voices/resolve.wav"
>   }
> }
> ```

> [!info]- 当前 Composition / 时间轴源码：videos/sakuragi-haruko-after-school-30s-20261110/src/Root.tsx，L1–37
> ```tsx
> import {Audio} from '@remotion/media';
> import {AbsoluteFill,Composition,Folder,Series,interpolate,staticFile,useCurrentFrame,useVideoConfig} from 'remotion';
> import {Opening} from './Opening';import {Haruko} from './Haruko';import {Shy} from './Shy';import {Practice} from './Practice';import {Gather} from './Gather';import {Cheer} from './Cheer';import {Resolve} from './Resolve';import {Ending} from './Ending';
> import {Smile} from './Smile';
> import {Epilogue} from './Epilogue';
> export const SakuragiHaruko=()=>{
>  const {fps,durationInFrames}=useVideoConfig();const frame=useCurrentFrame();
>  return <AbsoluteFill style={{background:'#171916'}}>
>   <Series>
>    <Series.Sequence name="同框鼓励" durationInFrames={4*fps} premountFor={fps}><Opening/></Series.Sequence>
>    <Series.Sequence name="晴子的建议" durationInFrames={4*fps} premountFor={fps}><Haruko/></Series.Sequence>
>    <Series.Sequence name="樱木害羞" durationInFrames={86} premountFor={fps}><Shy/></Series.Sequence>
>    <Series.Sequence name="认真练习" durationInFrames={4*fps} premountFor={fps}><Practice/></Series.Sequence>
>    <Series.Sequence name="收球准备" durationInFrames={2.5*fps} premountFor={fps}><Gather/></Series.Sequence>
>    <Series.Sequence name="晴子拍手" durationInFrames={94} premountFor={fps}><Cheer/></Series.Sequence>
>    <Series.Sequence name="恢复自信" durationInFrames={4*fps} premountFor={fps}><Resolve/></Series.Sequence>
>    <Series.Sequence name="再来一遍" durationInFrames={4*fps} premountFor={fps}><Ending/></Series.Sequence>
>   </Series>
>   <Audio name="对白下的配乐与球馆声" src={staticFile('audio/background-ducked.wav')} premountFor={fps}/>
>   <AbsoluteFill style={{background:'#171916',pointerEvents:'none',opacity:interpolate(frame,[0,.25*fps,durationInFrames-.35*fps,durationInFrames-1],[.6,0,0,.9],{extrapolateLeft:'clamp',extrapolateRight:'clamp'})}}/>
>  </AbsoluteFill>;
> };
> export const RemotionRoot=()=> <>
>  <Composition id="SakuragiHaruko" component={SakuragiHaruko} width={1280} height={720} fps={24} durationInFrames={720}/>
>  <Folder name="Scenes">
>   <Composition id="Opening" component={Opening} width={1280} height={720} fps={24} durationInFrames={96}/>
>   <Composition id="Haruko" component={Haruko} width={1280} height={720} fps={24} durationInFrames={96}/>
>   <Composition id="Shy" component={Shy} width={1280} height={720} fps={24} durationInFrames={86}/>
>   <Composition id="Smile" component={Smile} width={1280} height={720} fps={24} durationInFrames={34}/>
>   <Composition id="Practice" component={Practice} width={1280} height={720} fps={24} durationInFrames={96}/>
>   <Composition id="Gather" component={Gather} width={1280} height={720} fps={24} durationInFrames={60}/>
>   <Composition id="Cheer" component={Cheer} width={1280} height={720} fps={24} durationInFrames={94}/>
>   <Composition id="Resolve" component={Resolve} width={1280} height={720} fps={24} durationInFrames={96}/>
>   <Composition id="Ending" component={Ending} width={1280} height={720} fps={24} durationInFrames={96}/>
>   <Composition id="Epilogue" component={Epilogue} width={1280} height={720} fps={24} durationInFrames={36}/>
>  </Folder>
> </>;
> ```

源时间轴、生成帧数、所用区间与最终时长分别记录；历史文档或候选参数不自动覆盖当前采用配置。

## 参考来源与借鉴说明

在本次盘点的项目自有材料中，未找到可明确归属本片的外部参考视频或 X 帖子链接；这不等于历史上没有参考。

原作人物与场景依据见 `docs/references.md`：东映 DVD/角色资料、集英社试读入口是角色资料来源。项目声明实际视觉输入为自己生成的同框原画，未下载原作页或使用现成动漫片段；历史检索中部分页面不可访问，不将未读页面写成已验证分镜。

### 项目记录的技术、内容或素材来源

这些 URL 来自项目文件，尚不自动证明本片采用；除本节明确标注的访问外，本轮未逐一在线重读。

| 链接 | 记录文件 |
| --- | --- |
| [www.adobe.com](http://www.adobe.com/) | `videos/sakuragi-haruko-after-school-30s-20261110/docs/NOTO-OFL.txt` |
| [scripts.sil.org](https://scripts.sil.org/OFL) | `videos/sakuragi-haruko-after-school-30s-20261110/docs/NOTO-OFL.txt` |
| [www.toei-anim.co.jp](https://www.toei-anim.co.jp/dvd/slamdunk/) | `videos/sakuragi-haruko-after-school-30s-20261110/docs/references.md` |
| [www.shueisha.co.jp](https://www.shueisha.co.jp/books/reader/main.php?cid=08X10000000062717100) | `videos/sakuragi-haruko-after-school-30s-20261110/docs/references.md` |
| [lineup.toei-anim.co.jp](https://lineup.toei-anim.co.jp/ja/movie/movie_slumdumk_1994/character/) | `videos/sakuragi-haruko-after-school-30s-20261110/docs/references.md` |
| [github.com](https://github.com/dgrauet/ltx-2-mlx/blob/747b30ef10e92a011e85b2aa0f6c4ee86ced5218/docs/PIPELINES.md) | `videos/sakuragi-haruko-after-school-30s-20261110/docs/lipsync/method.md` |
| [ltx.io](https://ltx.io/blog/how-to-generate-video-from-audio) | `videos/sakuragi-haruko-after-school-30s-20261110/docs/lipsync/method.md` |
| [docs.ltx.io](https://docs.ltx.io/open-source-model/feature-guides/audio/dub-it-beta) | `videos/sakuragi-haruko-after-school-30s-20261110/docs/lipsync/method.md` |
| [github.com](https://github.com/Blaizzy/mlx-audio/blob/main/mlx_audio/tts/models/qwen3_tts/README.md) | `videos/sakuragi-haruko-after-school-30s-20261110/docs/voiceover/method.md` |
| [huggingface.co](https://huggingface.co/mlx-community/Qwen3-TTS-12Hz-1.7B-VoiceDesign-bf16) | `videos/sakuragi-haruko-after-school-30s-20261110/docs/voiceover/method.md` |

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

> [!info]- videos/sakuragi-haruko-after-school-30s-20261110/scripts/generate-lipsync.py，L1–62
> ```python
> """Generate an audio-driven candidate, preserving every prior take."""
> import project_env
> import argparse, hashlib, json, os, subprocess, sys, time
> from pathlib import Path
> 
> ROOT = Path(__file__).resolve().parents[1]
> p = argparse.ArgumentParser()
> p.add_argument('scene')
> p.add_argument('--take', default='v1')
> p.add_argument('--seed', type=int, default=11210)
> p.add_argument('--frames', type=int, default=97)
> p.add_argument('--anchors', help='Project-relative JSON list of extra image anchors')
> p.add_argument('--inputs-dir', default='public/lipsync/inputs', help='Use a new local input directory when changing dialogue')
> a = p.parse_args()
> for part in (a.scene, a.take):
>     if not part.replace('-', '').replace('_', '').isalnum():
>         raise SystemExit('Use a simple scene/take identifier')
> inputs=ROOT/a.inputs_dir
> assert inputs.resolve().is_relative_to(ROOT)
> image = inputs / f'{a.scene}.png'
> audio = inputs / f'{a.scene}.wav'
> prompt_path = ROOT / f'docs/lipsync/{a.scene}-{a.take}-prompt.txt'
> target = ROOT / f'public/lipsync/candidates/{a.scene}-{a.take}.mp4'
> metadata = ROOT / f'outputs/checks/lipsync/{a.scene}-{a.take}-generation.json'
> if target.exists():
>     raise SystemExit('Candidate exists; select a new take.')
> prompt = prompt_path.read_text().strip()
> anchors=[]
> if a.anchors:
>     anchor_path=ROOT/a.anchors
>     assert anchor_path.resolve().is_relative_to(ROOT)
>     anchors=json.loads(anchor_path.read_text())
>     for anchor in anchors:
>         path=ROOT/anchor['image'];assert path.resolve().is_relative_to(ROOT) and path.is_file()
> record = {'scene': a.scene, 'take': a.take, 'frames': a.frames, 'fps': 24,
>           'width': 1024, 'height': 576, 'seed': a.seed,
>           'pipeline': 'A2VidDistilledPipeline; LTX-2.3 q4; frozen source audio in both stages',
>           'reference': str(image.relative_to(ROOT)),
>           'audio': str(audio.relative_to(ROOT)), 'prompt': prompt,
>           'inputs_sha256': {str(f.relative_to(ROOT)): hashlib.sha256(f.read_bytes()).hexdigest()
>                             for f in (image, audio, prompt_path)},
>           'output': str(target.relative_to(ROOT))}
> if anchors:
>     record['anchors']=anchors
>     for anchor in anchors:
>         file=ROOT/anchor['image'];record['inputs_sha256'][anchor['image']]=hashlib.sha256(file.read_bytes()).hexdigest()
> metadata.write_text(json.dumps(record, ensure_ascii=False, indent=2))
> cmd = [sys.executable, '-u', 'scripts/ltx-entry.py', 'a2v', '--distilled',
>        '--model', str(ROOT / '.cache/models/ltx-q4'),
>        '--gemma', str(ROOT / '.cache/models/gemma-q4'), '--prompt', prompt,
>        '--image', str(image), '0', '1.0', '--audio', str(audio),
>        '--frames', str(a.frames), '--width', '1024', '--height', '576',
>        '--frame-rate', '24', '--seed', str(a.seed), '--low-ram', '--output', str(target)]
> for anchor in anchors:
>     cmd+=['--image',str(ROOT/anchor['image']),str(anchor['frame']),str(anchor['strength']),anchor.get('binding','guide')]
> start = time.monotonic()
> result = subprocess.run(cmd, cwd=ROOT, env=os.environ)
> record.update(exit_code=result.returncode, wall_seconds=round(time.monotonic()-start, 2),
>               output_exists=target.exists())
> metadata.write_text(json.dumps(record, ensure_ascii=False, indent=2))
> print(json.dumps(record, ensure_ascii=False, indent=2), flush=True)
> raise SystemExit(result.returncode)
> ```

> [!info]- videos/sakuragi-haruko-after-school-30s-20261110/scripts/lowram_prompt.py，L1–44
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

> [!info]- videos/sakuragi-haruko-after-school-30s-20261110/scripts/check-export-dialogue-timing.py，L1–41
> ```python
> """Check actual exported AAC dialogue positions against the frozen voice WAVs.
> 
> 240 Hz energy-envelope correlation checks mix timing, not lip phonemes.
> Uses only the standard library and the system ffmpeg.
> """
> import project_env
> import array,hashlib,json,math,subprocess,sys,wave
> from pathlib import Path
> ROOT=Path(__file__).resolve().parents[1]
> movie=ROOT/'outputs/sakuragi-haruko-30s.mp4'
> decoded=ROOT/'.cache/tmp/lipsync-export-audio.wav'
> subprocess.run(['ffmpeg','-y','-v','error','-i',str(movie),'-vn','-ar','48000','-ac','2','-c:a','pcm_s16le',str(decoded)],check=True)
> def read(path):
>     with wave.open(str(path),'rb') as f:
>         assert (f.getframerate(),f.getnchannels(),f.getsampwidth())==(48000,2,2)
>         a=array.array('h',f.readframes(f.getnframes()))
>     if sys.byteorder!='little':a.byteswap()
>     return a[::2]
> def envelope(a):
>     return [math.sqrt(sum(x*x for x in a[i:i+200])/200) for i in range(0,len(a)-199,200)]
> actual=read(decoded);records=[]
> lines=json.loads((ROOT/'docs/voiceover/dialogue.json').read_text())['lines']
> for line in lines:
>     voice=read(ROOT/f"public/audio/voices/{line['id']}.wav")
>     nominal=line['start_frame']*2000;left=min(nominal,48000)
>     y=envelope(actual[nominal-left:nominal+len(voice)+48000]);x=envelope(voice)
>     mean=sum(x)/len(x);xc=[v-mean for v in x];xx=sum(v*v for v in xc)
>     prefix=[0.];squares=[0.]
>     for v in y:prefix.append(prefix[-1]+v);squares.append(squares[-1]+v*v)
>     best=(-2.,0)
>     for k in range(len(y)-len(x)+1):
>         sy=prefix[k+len(x)]-prefix[k]
>         yy=squares[k+len(x)]-squares[k]-sy*sy/len(x)
>         score=sum(v*y[k+i] for i,v in enumerate(xc))/math.sqrt(max(1e-20,xx*yy))
>         if score>best[0]:best=(score,k)
>     offset=(best[1]*200-left)/48000
>     records.append({'scene':line['id'],'expected_start_frame':line['start_frame'],'offset_seconds':round(offset,6),'offset_frames_at_24fps':round(offset*24,4),'energy_envelope_correlation':round(best[0],6)})
>     assert abs(offset)<=1/24+1e-6 and best[0]>.85,records[-1]
> report={'status':'passed','scope':'Actual AAC mix timing, within one 24fps frame. Energy-envelope correlation is not a phoneme or perceptual lip-sync score.','movie_sha256':hashlib.sha256(movie.read_bytes()).hexdigest(),'lines':records}
> (ROOT/'outputs/checks/lipsync/export-dialogue-timing.json').write_text(json.dumps(report,ensure_ascii=False,indent=2))
> print(json.dumps(report,ensure_ascii=False,indent=2))
> ```

## 完整关键提示词与生成记录

以下保留项目自有提示词、候选参数与版本。存在失败候选和历史参数；最终采用以 selected-takes、edit-decisions 与制作复盘为准，不能将所有候选都写成已采用。

> [!info]- videos/sakuragi-haruko-after-school-30s-20261110/docs/failed-motion-prompts.json
> ```json
> {
>   "practice-v1": "A clean continuous hand drawn 1990s basketball anime scene, natural real-time stationary dribbling, fixed camera. Same tall red pompadour Sakuragi in red SHOHOKU 10 uniform at left and shorter brown haired Haruko in white blouse and navy skirt at far right. Sakuragi remains in his own spot with both feet firmly planted, knees flexed. With his right hand only he steadily dribbles exactly one orange basketball vertically beside his right leg, between knee height and the wooden floor, roughly two controlled bounces per second. Each bounce hits the floor before returning up to the same palm. His right wrist and elbow follow the ball naturally, left hand stays free for balance. He looks down with determined concentration and small balanced torso movement. Haruko remains clearly separate, quietly follows the ball with her eyes and gives a tiny approving nod, hands empty at her waist. No moving across court, no crossover, no ball passing or shooting. Preserve one round ball, correct hands and knees, no merging bodies, stable rigid hoop and background. Natural dribble impacts and light sneaker squeak in quiet gym, no speech, no music."
> }
> ```

> [!info]- videos/sakuragi-haruko-after-school-30s-20261110/docs/image-prompts.json
> ```json
> {
>   "01-two-shot": "Use case: illustration-story.\nAsset type: one complete 16:9 widescreen master animation keyframe, 1536x864 landscape.\nPrimary request: Faithful early-manga SLAM DUNK characters Hanamichi Sakuragi (桜木花道) and Haruko Akagi (赤木晴子), in Shohoku High School's modest old basketball gym after school. An original quiet practice scene, not a poster or collage.\nCharacter designs: Sakuragi is a very tall muscular Japanese teenage boy, signature vivid scarlet-red high swept-back pompadour with short cropped sides, angular broad jaw, thick dark eyebrows, small sharp eyes, recognizable humorous tough face. Red SHOHOKU basketball jersey with black-and-white trim, number 10, red basketball shorts, white socks, white/red high-top basketball shoes. Haruko is much shorter, a Japanese teenage girl with large warm brown eyes, soft rounded youthful face, straight chestnut-brown shoulder-length hair with the familiar neat bangs, white short-sleeved school blouse with small red ribbon at collar, navy pleated skirt, white socks and indoor shoes. Preserve manga-era proportions, natural nonsexual teen depiction.\nScene: polished honey-colored wooden floor, pale upper walls, muted green lower walls, tall rectangular windows high on left, timber roof trusses, distant simple fixed glass backboard and hoop. Warm late-afternoon light enters from screen left, no arena crowd, no other people. Keep geography coherent.\nComposition: full-body two shot, Sakuragi at left center, Haruko at right center, about 1.5m clear gap between silhouettes. Their entire bodies and feet are visible with generous margins, eye-level medium-wide camera. Sakuragi holds exactly one orange basketball securely in both hands near his left waist with anatomically correct grip, feet planted apart, turns his head slightly toward Haruko with a shy proud half-smile. Haruko faces him encouragingly with her relaxed hands lightly together in front of her waist. Her gaze meets his. They are not touching.\nStyle/medium: original 1990s Japanese basketball manga character designs translated into clean hand-drawn 2D cel animation, expressive precise black ink lines, modest line hatching, restrained flat color and simple cel shadows, warm understated watercolor gym background. Serious believable anatomy with appealing expressive faces, not generic modern anime, not 3D, not chibi, not glossy digital airbrushing. Strong facial resemblance to the original manga.\nConstraints: a single continuous finished scene, no panels, no captions, no subtitles, no watermark, no decorative text. No extra balls, no extra limbs, no duplicated people. Readable SHOHOKU and 10 only on jersey. Keep basket small and far behind the characters.",
>   "02-haruko": "Use case: identity-preserve. Image 1 is the reference for the SAME Haruko Akagi from early manga SLAM DUNK, her costume, art style and gym. Generate a new crisp medium close-up animation keyframe, widescreen 16:9, not a poster, not a collage and not an enlarged crop.\nHaruko alone in frame, chest-up portrait on right-center with some space to the left for her eyeline toward offscreen Sakuragi. Exact shoulder-length chestnut brown hair, tidy soft bangs, large warm brown eyes, youthful soft face, natural modest schoolgirl proportions; white short-sleeved blouse with red ribbon at collar. Warm, gentle, encouraging smile, lips lightly parted as if about to encourage a friend. Both empty hands resting lightly together just below chest, separated finger silhouettes, natural wrists. Her head tilts very slightly toward her left.\nSame Shohoku old gym background: green lower wall, pale upper wall and warm wooden floor subtly visible, the windows lie offscreen to left; warm light from screen left. Do not put a new hoop, signage or objects behind her head. Precise early 1990s hand-inked manga facial features and simple cel shading matching reference; restrained colored manga, not generic modern anime. No other characters, no basketball, no text overlays, no watermark.\n",
>   "03-shy": "Use case: identity-preserve. Image 1 is the exact identity, costume, visual style and location reference. Generate a NEW high quality animation keyframe, widescreen 16:9.\nMedium close-up from mid torso upwards of Hanamichi Sakuragi alone, early original SLAM DUNK manga character. He is at left-center looking offscreen to camera right toward Haruko. Keep the unmistakable vivid red swept-back tall pompadour, shaved short sides, thick black eyebrows, angular jaw, muscular neck and shoulders, small sharp expressive eyes. Red SHOHOKU jersey with black and white piping, number 10 partly visible.\nExpression: bashful and comically pleased with faint blush marks, an awkward closed-mouth grin. His RIGHT hand is raised beside the back of his head with a clearly visible natural elbow, hand not yet touching his hair, fingers relaxed and anatomically sound. His left arm is lowered; no basketball is visible in this close framing. He is about to sheepishly rub the back of his neck.\nSame Shohoku old gym with green lower walls and warm window light from screen left. Keep left windows as background, believable camera reverse eyeline, cel shadows and precise 1990s manga ink linework with limited hatching, same warm palette. No other people or objects near his hands, no text overlay, no watermark, no speech bubble. Do not modernize his face, do not give him a buzz cut.\n",
>   "04-practice": "Use case: identity-preserve. Reference image 1 gives exact same Hanamichi Sakuragi and Haruko, their clothes, line style and gym geography. Create a new complete 16:9 animation keyframe, eye-level full body wide scene in the same old Shohoku gym, warm window light from left.\nSakuragi stands at left center, his signature red pompadour and red SHOHOKU 10 uniform exactly as reference, knees gently flexed, feet a comfortable shoulder-width apart. He practices a stationary right-hand dribble. Exactly ONE orange basketball is below his right palm at knee height, palm flat above the top of the ball with a small air gap, free left hand extended only a little for balance. Both hands, both legs and shoe soles clearly readable; ball is outside his right knee on viewer's left, not between legs. Concentrated beginner expression.\nHaruko at far right about two metres away, same chestnut shoulder hair, bangs and white blouse/red ribbon/navy skirt; hands empty and relaxed in front of waist, watching his dribble supportively. Both figures' full feet visible, clear separation, no other people. Keep modest green lower walls, tall left windows and far centered rigid hoop, consistent court lines, same proportions as reference.\nClean colored manga and 1990s hand drawn cel style, no 3D, no glossy illustration, no poster panels, no duplicated basketball, no ball under left hand, no text overlays or watermark.\n",
>   "05-gather": "Use case: identity-preserve. Input image 1 is the identity, clothing, linework, color and exact gym/location reference. Create a NEW full scene animation keyframe for the same original manga-era SLAM DUNK scene, not a crop.\nFrame 16:9 landscape. Medium shot from knees up. Sakuragi dominates the left 60 percent, Haruko smaller at right with clear separation. Match Sakuragi's vivid red pompadour, angular jaw, thick eyebrows, muscular body, red SHOHOKU number 10 jersey and shorts exactly. Match Haruko's shoulder-length chestnut hair and bangs, white blouse red ribbon navy skirt exactly. Same warm window light from left, same green lower wall, honey wood court and modest old gym.\nSakuragi has now turned three-quarter toward the offscreen hoop on camera left. He holds exactly one basketball securely with two hands in front of his lower chest, shooting hand behind/below ball and guiding hand on its side, both arms separate, elbows natural. Knees gently bent, feet firmly planted, focused but slightly awkward beginner expression. Haruko stands still in the background at far right, watching with a gentle encouraging expression, hands apart and relaxed near her waist, not holding anything.\nCrucial: clear correct anatomy and natural hands contacting one round ball, no overlapping characters, no raised shot or ball release yet. Camera level and static, normal proportions. Faithful early original manga faces with clean 1990s ink lines and restrained cel color; match reference drawing texture. No text overlays, no panels, no watermark.\n",
>   "06-cheer": "Use case: identity-preserve. Image 1 is the exact Haruko Akagi face, hair, blouse, drawing style and gym reference. Create a NEW animation keyframe in 16:9 landscape, same character and outfit, same camera side with warm light from screen left.\nMedium shot from waist up of Haruko alone on the right-center. Same shoulder-length chestnut hair with neat bangs, large brown eyes, small natural nose and warm rounded teenage face. White short-sleeve blouse with red ribbon, navy skirt barely visible at bottom. She smiles with delighted supportive pride toward offscreen Sakuragi at camera left. Both forearms raised loosely in front of her chest, both palms facing each other with a clearly visible 15-centimetre gap, fingers together but relaxed and thumbs correct. Hands are prepared for gentle clapping, not already overlapping. Correct natural shoulder, elbow, wrist and finger anatomy.\nSame empty modest Shohoku gym behind her, green lower wall, pale upper wall, warm wooden floor and left window light. Clean ink, slight hatching, restrained cel color matching the reference, no generic modern anime redesign, no hoop behind her head, no other person, no ball, no captions or watermark.\n",
>   "07-resolve": "Use case: identity-preserve. Image 1 is exact Sakuragi face, hair, clothes, style and lighting reference. Create a NEW medium close-up 16:9 animation keyframe from waist up, Sakuragi alone at left-center looking offscreen camera right toward Haruko. Same Shohoku gym and warm left window light.\nHanamichi Sakuragi's vivid red high swept-back pompadour, muscular build, angular broad jaw, thick eyebrows and unmistakable original SLAM DUNK manga eyes. Red SHOHOKU number 10 jersey. Now he has a confident, slightly bashful genuine smile, composed rather than exaggerated. His RIGHT hand forms one relaxed natural fist just in front of his chest, elbow kept close to his body. His left hand supports exactly one basketball low at his left hip, bottom part of the ball may be cropped but the visible supporting hand is natural. His posture is upright, shoulders relaxed, anatomically correct wrists and arms with no overlap.\nPrecise 1990s original manga ink outlines, limited hatching and restrained cel colors, same warm painted old gym background, no modern anime makeover, no chibi. No other people, no props other than one ball, no captions, no watermark.\n",
>   "08-ending": "Use case: identity-preserve. Reference image 1 is the exact character identity, costume, gym geography and art-style reference. Make a NEW complete final-scene 16:9 animation keyframe.\nThe same early SLAM DUNK manga red-pompadour Hanamichi Sakuragi and chestnut shoulder-haired Haruko Akagi share a warm relaxed moment after basic practice, in exactly the same modest Shohoku old gym with windows on left and green lower walls. Same late-afternoon left light. Full body two shot, a little closer than the reference, both full feet visible.\nSakuragi at left, taller and muscular, red SHOHOKU number 10 jersey and shorts, white/red high tops. Holds exactly one orange ball tucked securely against his LEFT hip with his LEFT forearm and hand supporting from below. RIGHT hand relaxed near his chest, not touching the girl. Slightly bashful proud smile, eyes directed toward Haruko.\nHaruko at right, same shoulder length brown hair and bangs, white short sleeve blouse with red ribbon and navy pleated skirt. She looks up at him with an easy sincere smile, both empty hands resting naturally in front of her waist. Keep at least one metre clear air between their silhouettes. Both planted feet, calm balanced posture, anatomically natural hands.\nThe mood is young friendship and encouragement, consistent with the original relationship, not a romantic embrace. Precise 1990s manga ink linework and understated colored cel animation, matching reference textures. Stable backboard in far background, no new props, no other people, no overlapping arms, no extra ball, no text, no panels, no watermark.\n",
>   "04-stance-v2": "Use case: identity-preserve. Reference image 1 defines the exact same early SLAM DUNK manga identities, outfits, style and Shohoku old gym geography. Create a new widescreen 16:9 full-body animation keyframe.\nSakuragi at left-center and Haruko far right, two metres clear separation. Vivid scarlet swept-back pompadour, broad jaw, thick eyebrows, tall muscular body, red SHOHOKU jersey number 10 and matching shorts, white/red shoes exactly as reference. Sakuragi is now practicing ONLY a basic balanced basketball ready stance WITHOUT a basketball: both empty hands are clearly visible held loosely beside his hips with palms down, elbows naturally slightly bent; feet shoulder-width apart and fully on the wooden floor; knees shallowly bent, torso upright with a small forward athletic lean, focused earnest expression. No fist, no crossing limbs, no jumping.\nHaruko watches with a gentle coaching smile, exact shoulder length chestnut hair and neat bangs, white short-sleeved blouse with red ribbon and navy skirt. Her empty hands rest at her own waist. Her stance is relaxed and she is clearly separate.\nNo basketball anywhere in this frame, it is an empty-hand footwork / balance drill. Keep the same green lower walls, tall windows on left, distant fixed hoop, warm left light and wooden court perspective. Both complete bodies and shoe soles visible with margins. Clean original 1990s inked character designs, natural anatomy, restrained cel colors and background texture matching reference. No extra people, no text overlays, no watermark, no panels.\n",
>   "09-epilogue": "Use case: identity-preserve. Image 1 is the exact identity, hair, clothes, line style and warm Shohoku gym reference for Haruko Akagi, early SLAM DUNK original manga era. Create a new crisp single animation keyframe, widescreen 16:9.\nA close portrait from upper chest up of Haruko alone, her face now closer and slightly left of center. Exact shoulder-length chestnut brown hair, familiar neat bangs, warm large brown eyes, gentle youthful rounded face, white school blouse with red ribbon. She looks slightly UP and to camera LEFT with a quiet sincere pleased smile, lips gently closed. Shoulders relaxed. BOTH HANDS ARE BELOW THE FRAME and invisible; the visible drawing consists of her complete head, neck, shoulders and upper blouse. Keep generous headroom, full hair silhouette, full chin and ears, natural original manga proportions. No other person or part of another person is present.\nSame gym from a gently different close camera angle: pale upper wall, muted green lower wall at the bottom, softly drawn warm left-window light, no hoop or door directly behind her head. Clean expressive 1990s manga ink outlines, small restrained hatching, cel colors and subtle painted background texture exactly consistent with the reference. Not 3D, not chibi, not a modern anime redesign. A calm warm ending expression. No basketball, no hands in frame, no panels, captions, logos or watermark.\n"
> }
> ```

> [!info]- videos/sakuragi-haruko-after-school-30s-20261110/docs/video-prompts.json
> ```json
> {
>   "opening": "A warm hand drawn 1990s basketball manga animation, natural real-time speed, locked camera. Preserve the exact red pompadour boy Sakuragi and brown haired girl Haruko and all clothes. The two remain standing in their own places with clear space between them. Haruko turns her face up toward Sakuragi, smiles reassuringly and gives one small supportive nod. Sakuragi responds by turning his head slightly toward her and smiling bashfully, shifting his weight gently without moving his feet. He continuously holds exactly one basketball at his waist with both hands, keeps the ball still and secure. Stable anatomy and unchanged gym, restrained natural acting, not slow motion. Warm window light, quiet roomy gym ambience and soft fabric movement, no speech, no music.",
>   "haruko": "A warm clean 1990s hand inked anime medium close-up of Haruko Akagi, exact same face, shoulder length chestnut hair, bangs and white school blouse with red ribbon. Locked camera. She looks to offscreen left toward Sakuragi, gently nods once and opens a bright encouraging smile, then relaxes her empty hands a little. Small expressive real-time head and shoulder movement and a natural blink. Remain in place, no walking, no large hand gestures. Preserve face proportions, hair length, finger anatomy and gym background. Same afternoon light. Quiet gym room ambience, subtle clothes rustle, no intelligible or unintelligible speech, no music.",
>   "shy": "Natural real-time 1990s hand-drawn basketball anime acting, fixed camera, same red pompadour Sakuragi and red number 10 jersey. In one continuous sheepish gesture his raised right hand reaches to the back of his neck and rubs it briefly, he dips his chin a little with a bashful grin and glances toward offscreen Haruko on the right. His muscular shoulders relax as he smiles, one natural blink. Keep head shape and red pompadour stable, do not change his age or hairstyle. The left arm stays down outside the crop. No ball appears in frame. Correct connected hand, arm and elbow throughout, no additional hands. Stable warm gym background, soft fabric rustle and room tone, no voice, no music.",
>   "practice": "A clean hand-drawn 1990s basketball manga animation at normal real-time speed, locked full-body camera. Preserve the exact red pompadour Sakuragi in red number 10 jersey and brown-haired Haruko in white blouse and navy skirt. There is no basketball in this empty-hand balance drill. Sakuragi keeps both shoes planted shoulder width apart and makes one small smooth knee bend into a balanced ready stance, then straightens slightly and settles. His torso stays balanced above his feet, shoulders and empty hands move naturally with the modest bend. Both empty hands stay loosely beside his hips and clearly separated from his legs, no crossing limbs, no jump or walking. He glances briefly toward Haruko at far right, then looks forward with earnest concentration. Haruko remains standing well apart, keeps her empty hands at her own waist and gives one small approving nod. No new object or ball appears. Natural stable faces, fingers, knees and foot contact. Same warm afternoon light and rigid old gym architecture. Soft sneaker creak and fabric rustle in quiet gym room tone, no voice, no music.",
>   "cheer": "Hand drawn 1990s manga anime, same Haruko Akagi, brown shoulder length hair, white blouse red ribbon, fixed medium close-up. Natural real-time expression. She gives two gentle clear claps in front of her chest, palms come together then separate naturally, elbows stay in a comfortable position. She smiles delightedly toward offscreen Sakuragi to her left, then lowers her empty hands slightly while keeping the warm smile. Correct distinct wrists and fingers, stable original face and hair, restrained movement, remain standing in place. Warm afternoon old gym background does not move. Soft natural hand claps and airy room tone, no speech, no music.",
>   "resolve": "An expressive natural-time 1990s hand drawn basketball anime shot of red pompadour Sakuragi in the same number 10 red jersey. Locked medium close-up, exact same face, hairstyle and athletic body. He lifts his right clenched fist a small distance in a single restrained confident yes gesture, smiles with bashful pride toward offscreen Haruko on the right, then gives one affirmative nod and relaxes his fist. Left forearm keeps exactly one basketball secure at left hip, never releases or dribbles it. Feet stay in place outside crop, shoulders naturally move with the nod. No extra hands or balls, no deformed fingers, preserve jersey 10 and gym geometry. Soft cloth movement, calm gym room tone, no speaking, no music.",
>   "ending": "A warm natural-time ending in a 1990s hand drawn basketball anime, exact same red pompadour Sakuragi in red number 10 uniform and brown haired Haruko in white blouse red ribbon navy skirt. Fixed full body two shot. They remain in their separate standing places, at least a metre gap, feet planted. Sakuragi keeps exactly one basketball safely tucked at left hip with his left forearm, relaxes his raised right hand slightly and turns his head toward Haruko with a bashful confident smile. Haruko looks up, smiles sincerely and gives one small friendly nod, empty hands relaxed together near waist. A subtle gentle breath and natural blink. No approach, touching, embrace, walking or new object. Natural stable hands and feet, unchanged warm gym and backboard. Quiet gym atmosphere and soft fabric rustle, no speech, no music.",
>   "gather": "A clean hand-drawn 1990s Japanese basketball anime scene at natural real-time speed. Locked camera, preserve the exact original drawing style and both character identities and clothing. The tall red pompadour boy Sakuragi keeps both feet planted and holds exactly one round basketball in both hands. In one smooth modest preparation action he bends his knees slightly and brings the ball from his lower abdomen up to his chest, then settles into a balanced ready stance. His hands remain securely on the same basketball throughout. He concentrates toward the offscreen basket on his left, no shot or release, no jump. The shorter brown-haired schoolgirl Haruko stands separately at the right, stays in her own place, follows his movement with her eyes and gives one small supportive nod. Their bodies remain clearly separated. Anatomically stable hands, elbows, knees and faces, no extra ball, no camera motion. Warm afternoon light, stable gym walls and hoop. Quiet airy indoor gym room tone, a soft sneaker squeak and clothes rustle, no speech, no music.",
>   "epilogue": "A single young brown haired schoolgirl in a locked close portrait, the exact same Haruko Akagi face, shoulder length chestnut hair, bangs, white blouse and red ribbon as the input. Hand drawn 1990s colored manga anime. At natural real-time speed she maintains her quiet sincere closed-lip smile, gives one small relaxed nod and a natural blink, looking slightly up and left. Her complete head and chin stay inside the frame and her shoulders remain relaxed. Both hands and arms stay below frame and never appear. She is the sole person and stays in her own place, no one enters. No speaking, no large turn or gesture, no camera motion, no new object. Preserve the exact face proportions and hair shape, warm afternoon gym background and ink line style. Quiet gentle gym room tone, no voices or music."
> }
> ```

> [!info]- videos/sakuragi-haruko-after-school-30s-20261110/docs/lipsync/cheer-v1-prompt.txt
> ```text
> A continuous medium close-up of Haruko Akagi from the reference illustration, traditional hand-drawn 1990s Japanese basketball anime with stable ink lines and flat cel shading. The brown-haired teenage girl in a white short-sleeved blouse and red ribbon says the Mandarin sentence in the supplied audio: "对，就是这样！" Her mouth and jaw visibly articulate the entire line in time with that exact voice. Her eyes brighten with sincere delight at Sakuragi's progress, looking off camera toward the left. She gives two or three small, gentle claps at chest level with naturally formed separated hands that briefly meet palm to palm, then lowers and relaxes her hands. She speaks while smiling, not a broad silent laugh, and closes her mouth into a pleased small smile after the voice ends. Preserve her face, bangs, shoulder-length brown hair, outfit, gym perspective and warm afternoon light. Locked camera, one girl alone, restrained natural animation at normal speed.
> ```

> [!info]- videos/sakuragi-haruko-after-school-30s-20261110/docs/lipsync/ending-v1-prompt.txt
> ```text
> A continuous wide two-person shot from the reference illustration, hand-drawn 1990s Japanese basketball anime, stable ink lines and flat cel colors. On the left, Hanamichi Sakuragi, the red pompadour-haired teenage boy in red Shohoku number 10 uniform, speaks the Mandarin sentence in the supplied audio: "好，再来一遍！" Only Sakuragi speaks. He visibly articulates the words in time with the exact male voice, with bright determined eyes and a cheerful confident expression, then closes his mouth in a satisfied smile when the voice stops. His right fist remains raised near his chest, his left arm steadily holds one basketball against his left hip. On the right, Haruko Akagi in a white blouse, red ribbon and navy skirt quietly listens with her mouth closed and gives a tiny pleased nod. Her hands remain clasped in front. Both keep their original separated standing positions, no steps, no hand or body contact. Preserve the reference identities, proportions, gym architecture, floor perspective and afternoon light. Locked camera, natural conversational speed.
> ```

> [!info]- videos/sakuragi-haruko-after-school-30s-20261110/docs/lipsync/haruko-v1-prompt.txt
> ```text
> A continuous medium close-up of Haruko Akagi from the reference illustration, traditional hand-drawn 1990s Japanese basketball anime, stable ink lines, flat cel shading. The brown-haired teenage girl in a white short-sleeved blouse and red ribbon speaks the Mandarin sentence in the provided audio: "先站稳，别着急。" Her visible mouth articulates the words naturally and in time with that exact audio. She looks gently toward Sakuragi off camera to the left. She delivers a patient, reassuring suggestion with relaxed brows and a warm small smile, a slight encouraging nod, not a broad laugh. Her hands stay loosely clasped near her chest throughout, no new gesture. Her jaw and lips move clearly while speaking and settle into a quiet closed-mouth smile when the voice stops. Preserve the reference face shape, brown bangs, shoulder-length hair, outfit, gym perspective and warm left sunlight. Fixed camera, natural conversational speed. One girl alone in frame.
> ```

> [!info]- videos/sakuragi-haruko-after-school-30s-20261110/docs/lipsync/opening-v1-prompt.txt
> ```text
> A continuous wide two-person shot from the reference illustration, traditional hand-drawn 1990s Japanese basketball anime with clean ink lines and flat cel colors. On the right, Haruko Akagi, the brown-haired girl in a white blouse, red ribbon and navy skirt, speaks the Mandarin sentence heard in the provided audio: "樱木同学，再试一次吧。" Only Haruko speaks. Her mouth articulates naturally in time with the supplied female voice, her eyes and warm encouraging smile aimed toward Sakuragi on the left, with a small reassuring nod. On the left, red pompadour-haired Hanamichi Sakuragi in his red Shohoku number 10 uniform quietly listens with a closed mouth, looking toward her. His two hands hold a single basketball steadily in front of his waist, unchanged grip. Both remain in the same separate positions, no walking or contact. Haruko's hands stay gently clasped in front of her skirt. Her mouth becomes a quiet closed smile after her voice ends. Preserve character identities, clothes, original gym architecture, perspective and sunlight. Fixed camera, natural conversational speed.
> ```

> [!info]- videos/sakuragi-haruko-after-school-30s-20261110/docs/lipsync/resolve-v1-prompt.txt
> ```text
> A continuous medium close-up from the reference illustration, traditional hand-drawn Japanese basketball anime, clean ink lines and flat cel colors. Hanamichi Sakuragi, the red pompadour-haired teenage boy in a red Shohoku basketball jersey, speaks the Mandarin Chinese sentence heard in the provided audio: "我可是天才嘛。" He starts with his mouth closed in a confident small grin, then at the exact onset of his voice he articulates the whole sentence with clearly visible, natural jaw and mouth shapes synchronized to the provided voice, including the brief lip closure on the final "嘛" before opening the vowel. He raises one eyebrow playfully and keeps a boastful yet friendly amused expression, small head motion, gaze toward Haruko off camera on the right. His mouth becomes a closed satisfied smile after his voice finishes. His right hand stays a relaxed fist beside his chest, with five anatomically coherent fingers. Preserve the same red hair, facial proportions, jersey, perspective, warm wooden gym interior and afternoon light. The camera remains locked. One boy alone in frame. Natural conversational speed, restrained 2D animation, stable identity throughout.
> ```

> [!info]- videos/sakuragi-haruko-after-school-30s-20261110/docs/lipsync/shy-v1-prompt.txt
> ```text
> A continuous medium close-up of Hanamichi Sakuragi from the reference illustration, hand-drawn 1990s Japanese basketball anime with stable clean ink lines and flat cel shading. The red pompadour-haired teenage boy in the red Shohoku basketball jersey speaks the Mandarin sentence in the supplied audio: "交给我吧，晴子小姐！" His mouth and jaw visibly articulate the sentence in exact time with that audio. He starts a little proud, then grows bashful and warmly excited while addressing Haruko off camera on the right. A small blush, softened eyebrows and bright eyes convey his happy embarrassment. His right hand gently touches the back of his own neck and stays there, anatomically coherent fingers. Keep his head facing three quarters right, restrained small motions, no large turn. He closes his mouth into a shy satisfied grin after finishing the line. Preserve his face proportions, red hairstyle, jersey and warm wooden gym background. Locked camera, one boy alone, natural speaking speed.
> ```

> [!info]- videos/sakuragi-haruko-after-school-30s-20261110/docs/lipsync/shy-v2-prompt.txt
> ```text
> A continuous medium close-up of Hanamichi Sakuragi from the reference illustration, traditional hand-drawn 1990s Japanese basketball anime, clean ink lines and flat cel shading. He maintains the exact same three-quarter-right head orientation and gaze toward Haruko off camera for the entire shot. The opening is quiet: he holds a small closed-mouth bashful smile until the male voice actually begins in the supplied audio. Then he speaks the Mandarin sentence "交给我吧，晴子小姐！" with visible natural lip and jaw articulation following that exact audio, closing his lips at the short pauses and returning to a closed, softly blushing smile as soon as the phrase ends. His confident delivery gently softens with happy embarrassment when addressing Haruko. His right hand gently settles on the back of his own neck near the beginning, then rests there with a stable elbow and natural connected wrist. His head remains at the same three-quarter-right angle through the final closed-mouth smile, with small natural blinks and breathing. Preserve his red pompadour hairstyle, facial proportions, five-finger hand, red Shohoku jersey and warm wooden gym background from the image anchors. Locked camera, one boy alone, normal conversational speed.
> ```

> [!info]- videos/sakuragi-haruko-after-school-30s-20261110/docs/prompts/02-haruko.txt
> ```text
> Use case: identity-preserve. Image 1 is the reference for the SAME Haruko Akagi from early manga SLAM DUNK, her costume, art style and gym. Generate a new crisp medium close-up animation keyframe, widescreen 16:9, not a poster, not a collage and not an enlarged crop.
> Haruko alone in frame, chest-up portrait on right-center with some space to the left for her eyeline toward offscreen Sakuragi. Exact shoulder-length chestnut brown hair, tidy soft bangs, large warm brown eyes, youthful soft face, natural modest schoolgirl proportions; white short-sleeved blouse with red ribbon at collar. Warm, gentle, encouraging smile, lips lightly parted as if about to encourage a friend. Both empty hands resting lightly together just below chest, separated finger silhouettes, natural wrists. Her head tilts very slightly toward her left.
> Same Shohoku old gym background: green lower wall, pale upper wall and warm wooden floor subtly visible, the windows lie offscreen to left; warm light from screen left. Do not put a new hoop, signage or objects behind her head. Precise early 1990s hand-inked manga facial features and simple cel shading matching reference; restrained colored manga, not generic modern anime. No other characters, no basketball, no text overlays, no watermark.
> ```

> [!info]- videos/sakuragi-haruko-after-school-30s-20261110/docs/prompts/03-shy.txt
> ```text
> Use case: identity-preserve. Image 1 is the exact identity, costume, visual style and location reference. Generate a NEW high quality animation keyframe, widescreen 16:9.
> Medium close-up from mid torso upwards of Hanamichi Sakuragi alone, early original SLAM DUNK manga character. He is at left-center looking offscreen to camera right toward Haruko. Keep the unmistakable vivid red swept-back tall pompadour, shaved short sides, thick black eyebrows, angular jaw, muscular neck and shoulders, small sharp expressive eyes. Red SHOHOKU jersey with black and white piping, number 10 partly visible.
> Expression: bashful and comically pleased with faint blush marks, an awkward closed-mouth grin. His RIGHT hand is raised beside the back of his head with a clearly visible natural elbow, hand not yet touching his hair, fingers relaxed and anatomically sound. His left arm is lowered; no basketball is visible in this close framing. He is about to sheepishly rub the back of his neck.
> Same Shohoku old gym with green lower walls and warm window light from screen left. Keep left windows as background, believable camera reverse eyeline, cel shadows and precise 1990s manga ink linework with limited hatching, same warm palette. No other people or objects near his hands, no text overlay, no watermark, no speech bubble. Do not modernize his face, do not give him a buzz cut.
> ```

> [!info]- videos/sakuragi-haruko-after-school-30s-20261110/docs/prompts/04-practice.txt
> ```text
> Use case: identity-preserve. Reference image 1 gives exact same Hanamichi Sakuragi and Haruko, their clothes, line style and gym geography. Create a new complete 16:9 animation keyframe, eye-level full body wide scene in the same old Shohoku gym, warm window light from left.
> Sakuragi stands at left center, his signature red pompadour and red SHOHOKU 10 uniform exactly as reference, knees gently flexed, feet a comfortable shoulder-width apart. He practices a stationary right-hand dribble. Exactly ONE orange basketball is below his right palm at knee height, palm flat above the top of the ball with a small air gap, free left hand extended only a little for balance. Both hands, both legs and shoe soles clearly readable; ball is outside his right knee on viewer's left, not between legs. Concentrated beginner expression.
> Haruko at far right about two metres away, same chestnut shoulder hair, bangs and white blouse/red ribbon/navy skirt; hands empty and relaxed in front of waist, watching his dribble supportively. Both figures' full feet visible, clear separation, no other people. Keep modest green lower walls, tall left windows and far centered rigid hoop, consistent court lines, same proportions as reference.
> Clean colored manga and 1990s hand drawn cel style, no 3D, no glossy illustration, no poster panels, no duplicated basketball, no ball under left hand, no text overlays or watermark.
> ```

> [!info]- videos/sakuragi-haruko-after-school-30s-20261110/docs/prompts/04-stance-v2.txt
> ```text
> Use case: identity-preserve. Reference image 1 defines the exact same early SLAM DUNK manga identities, outfits, style and Shohoku old gym geography. Create a new widescreen 16:9 full-body animation keyframe.
> Sakuragi at left-center and Haruko far right, two metres clear separation. Vivid scarlet swept-back pompadour, broad jaw, thick eyebrows, tall muscular body, red SHOHOKU jersey number 10 and matching shorts, white/red shoes exactly as reference. Sakuragi is now practicing ONLY a basic balanced basketball ready stance WITHOUT a basketball: both empty hands are clearly visible held loosely beside his hips with palms down, elbows naturally slightly bent; feet shoulder-width apart and fully on the wooden floor; knees shallowly bent, torso upright with a small forward athletic lean, focused earnest expression. No fist, no crossing limbs, no jumping.
> Haruko watches with a gentle coaching smile, exact shoulder length chestnut hair and neat bangs, white short-sleeved blouse with red ribbon and navy skirt. Her empty hands rest at her own waist. Her stance is relaxed and she is clearly separate.
> No basketball anywhere in this frame, it is an empty-hand footwork / balance drill. Keep the same green lower walls, tall windows on left, distant fixed hoop, warm left light and wooden court perspective. Both complete bodies and shoe soles visible with margins. Clean original 1990s inked character designs, natural anatomy, restrained cel colors and background texture matching reference. No extra people, no text overlays, no watermark, no panels.
> ```

> [!info]- videos/sakuragi-haruko-after-school-30s-20261110/docs/prompts/05-gather.txt
> ```text
> Use case: identity-preserve. Input image 1 is the identity, clothing, linework, color and exact gym/location reference. Create a NEW full scene animation keyframe for the same original manga-era SLAM DUNK scene, not a crop.
> Frame 16:9 landscape. Medium shot from knees up. Sakuragi dominates the left 60 percent, Haruko smaller at right with clear separation. Match Sakuragi's vivid red pompadour, angular jaw, thick eyebrows, muscular body, red SHOHOKU number 10 jersey and shorts exactly. Match Haruko's shoulder-length chestnut hair and bangs, white blouse red ribbon navy skirt exactly. Same warm window light from left, same green lower wall, honey wood court and modest old gym.
> Sakuragi has now turned three-quarter toward the offscreen hoop on camera left. He holds exactly one basketball securely with two hands in front of his lower chest, shooting hand behind/below ball and guiding hand on its side, both arms separate, elbows natural. Knees gently bent, feet firmly planted, focused but slightly awkward beginner expression. Haruko stands still in the background at far right, watching with a gentle encouraging expression, hands apart and relaxed near her waist, not holding anything.
> Crucial: clear correct anatomy and natural hands contacting one round ball, no overlapping characters, no raised shot or ball release yet. Camera level and static, normal proportions. Faithful early original manga faces with clean 1990s ink lines and restrained cel color; match reference drawing texture. No text overlays, no panels, no watermark.
> ```

> [!info]- videos/sakuragi-haruko-after-school-30s-20261110/docs/prompts/06-cheer.txt
> ```text
> Use case: identity-preserve. Image 1 is the exact Haruko Akagi face, hair, blouse, drawing style and gym reference. Create a NEW animation keyframe in 16:9 landscape, same character and outfit, same camera side with warm light from screen left.
> Medium shot from waist up of Haruko alone on the right-center. Same shoulder-length chestnut hair with neat bangs, large brown eyes, small natural nose and warm rounded teenage face. White short-sleeve blouse with red ribbon, navy skirt barely visible at bottom. She smiles with delighted supportive pride toward offscreen Sakuragi at camera left. Both forearms raised loosely in front of her chest, both palms facing each other with a clearly visible 15-centimetre gap, fingers together but relaxed and thumbs correct. Hands are prepared for gentle clapping, not already overlapping. Correct natural shoulder, elbow, wrist and finger anatomy.
> Same empty modest Shohoku gym behind her, green lower wall, pale upper wall, warm wooden floor and left window light. Clean ink, slight hatching, restrained cel color matching the reference, no generic modern anime redesign, no hoop behind her head, no other person, no ball, no captions or watermark.
> ```

> [!info]- videos/sakuragi-haruko-after-school-30s-20261110/docs/prompts/07-resolve.txt
> ```text
> Use case: identity-preserve. Image 1 is exact Sakuragi face, hair, clothes, style and lighting reference. Create a NEW medium close-up 16:9 animation keyframe from waist up, Sakuragi alone at left-center looking offscreen camera right toward Haruko. Same Shohoku gym and warm left window light.
> Hanamichi Sakuragi's vivid red high swept-back pompadour, muscular build, angular broad jaw, thick eyebrows and unmistakable original SLAM DUNK manga eyes. Red SHOHOKU number 10 jersey. Now he has a confident, slightly bashful genuine smile, composed rather than exaggerated. His RIGHT hand forms one relaxed natural fist just in front of his chest, elbow kept close to his body. His left hand supports exactly one basketball low at his left hip, bottom part of the ball may be cropped but the visible supporting hand is natural. His posture is upright, shoulders relaxed, anatomically correct wrists and arms with no overlap.
> Precise 1990s original manga ink outlines, limited hatching and restrained cel colors, same warm painted old gym background, no modern anime makeover, no chibi. No other people, no props other than one ball, no captions, no watermark.
> ```

> [!info]- videos/sakuragi-haruko-after-school-30s-20261110/docs/prompts/08-ending.txt
> ```text
> Use case: identity-preserve. Reference image 1 is the exact character identity, costume, gym geography and art-style reference. Make a NEW complete final-scene 16:9 animation keyframe.
> The same early SLAM DUNK manga red-pompadour Hanamichi Sakuragi and chestnut shoulder-haired Haruko Akagi share a warm relaxed moment after basic practice, in exactly the same modest Shohoku old gym with windows on left and green lower walls. Same late-afternoon left light. Full body two shot, a little closer than the reference, both full feet visible.
> Sakuragi at left, taller and muscular, red SHOHOKU number 10 jersey and shorts, white/red high tops. Holds exactly one orange ball tucked securely against his LEFT hip with his LEFT forearm and hand supporting from below. RIGHT hand relaxed near his chest, not touching the girl. Slightly bashful proud smile, eyes directed toward Haruko.
> Haruko at right, same shoulder length brown hair and bangs, white short sleeve blouse with red ribbon and navy pleated skirt. She looks up at him with an easy sincere smile, both empty hands resting naturally in front of her waist. Keep at least one metre clear air between their silhouettes. Both planted feet, calm balanced posture, anatomically natural hands.
> The mood is young friendship and encouragement, consistent with the original relationship, not a romantic embrace. Precise 1990s manga ink linework and understated colored cel animation, matching reference textures. Stable backboard in far background, no new props, no other people, no overlapping arms, no extra ball, no text, no panels, no watermark.
> ```

> [!info]- videos/sakuragi-haruko-after-school-30s-20261110/docs/prompts/09-epilogue.txt
> ```text
> Use case: identity-preserve. Image 1 is the exact identity, hair, clothes, line style and warm Shohoku gym reference for Haruko Akagi, early SLAM DUNK original manga era. Create a new crisp single animation keyframe, widescreen 16:9.
> A close portrait from upper chest up of Haruko alone, her face now closer and slightly left of center. Exact shoulder-length chestnut brown hair, familiar neat bangs, warm large brown eyes, gentle youthful rounded face, white school blouse with red ribbon. She looks slightly UP and to camera LEFT with a quiet sincere pleased smile, lips gently closed. Shoulders relaxed. BOTH HANDS ARE BELOW THE FRAME and invisible; the visible drawing consists of her complete head, neck, shoulders and upper blouse. Keep generous headroom, full hair silhouette, full chin and ears, natural original manga proportions. No other person or part of another person is present.
> Same gym from a gently different close camera angle: pale upper wall, muted green lower wall at the bottom, softly drawn warm left-window light, no hoop or door directly behind her head. Clean expressive 1990s manga ink outlines, small restrained hatching, cel colors and subtle painted background texture exactly consistent with the reference. Not 3D, not chibi, not a modern anime redesign. A calm warm ending expression. No basketball, no hands in frame, no panels, captions, logos or watermark.
> ```

> [!info]- videos/sakuragi-haruko-after-school-30s-20261110/outputs/checks/generation-cheer-v1.json
> ```json
> {
>   "scene": "cheer",
>   "output": "public/video/cheer-native24-v1.mp4",
>   "frames": 97,
>   "width": 1024,
>   "height": 576,
>   "fps": 24,
>   "seed": 11210,
>   "source_image": "public/art/06-cheer.png",
>   "source_image_sha256": "abe0f2ec89dde9441a317ddc9c5630f7c61f334b3980de10b9b5d38c5057718f",
>   "prompt": "Hand drawn 1990s manga anime, same Haruko Akagi, brown shoulder length hair, white blouse red ribbon, fixed medium close-up. Natural real-time expression. She gives two gentle clear claps in front of her chest, palms come together then separate naturally, elbows stay in a comfortable position. She smiles delightedly toward offscreen Sakuragi to her left, then lowers her empty hands slightly while keeping the warm smile. Correct distinct wrists and fingers, stable original face and hair, restrained movement, remain standing in place. Warm afternoon old gym background does not move. Soft natural hand claps and airy room tone, no speech, no music.",
>   "method": "LTX-2.3 q4 / MLX whole-scene I2V, float32 default compute, block streaming, sequential Gemma/connector",
>   "exit_code": 0,
>   "wall_seconds": 423.61,
>   "output_exists": true
> }
> ```

> [!info]- videos/sakuragi-haruko-after-school-30s-20261110/outputs/checks/generation-ending-v1.json
> ```json
> {
>   "scene": "ending",
>   "output": "public/video/ending-native24-v1.mp4",
>   "frames": 97,
>   "width": 1024,
>   "height": 576,
>   "fps": 24,
>   "seed": 11210,
>   "source_image": "public/art/08-ending.png",
>   "source_image_sha256": "37d81a38179249d52ace9f6d569169afd6939ba0ecd94e9cf73c1ef905d1bbe5",
>   "prompt": "A warm natural-time ending in a 1990s hand drawn basketball anime, exact same red pompadour Sakuragi in red number 10 uniform and brown haired Haruko in white blouse red ribbon navy skirt. Fixed full body two shot. They remain in their separate standing places, at least a metre gap, feet planted. Sakuragi keeps exactly one basketball safely tucked at left hip with his left forearm, relaxes his raised right hand slightly and turns his head toward Haruko with a bashful confident smile. Haruko looks up, smiles sincerely and gives one small friendly nod, empty hands relaxed together near waist. A subtle gentle breath and natural blink. No approach, touching, embrace, walking or new object. Natural stable hands and feet, unchanged warm gym and backboard. Quiet gym atmosphere and soft fabric rustle, no speech, no music.",
>   "method": "LTX-2.3 q4 / MLX whole-scene I2V, float32 default compute, block streaming, sequential Gemma/connector",
>   "exit_code": 0,
>   "wall_seconds": 421.17,
>   "output_exists": true
> }
> ```

> [!info]- videos/sakuragi-haruko-after-school-30s-20261110/outputs/checks/generation-epilogue-v1.json
> ```json
> {
>   "scene": "epilogue",
>   "output": "public/video/epilogue-native24-v1.mp4",
>   "frames": 49,
>   "width": 1024,
>   "height": 576,
>   "fps": 24,
>   "seed": 11210,
>   "source_image": "public/art/09-epilogue.png",
>   "source_image_sha256": "b4c6c06f04abc8ab7f25d02c960acb59855a14c67f26381f312e4879ab46f7cd",
>   "prompt": "A single young brown haired schoolgirl in a locked close portrait, the exact same Haruko Akagi face, shoulder length chestnut hair, bangs, white blouse and red ribbon as the input. Hand drawn 1990s colored manga anime. At natural real-time speed she maintains her quiet sincere closed-lip smile, gives one small relaxed nod and a natural blink, looking slightly up and left. Her complete head and chin stay inside the frame and her shoulders remain relaxed. Both hands and arms stay below frame and never appear. She is the sole person and stays in her own place, no one enters. No speaking, no large turn or gesture, no camera motion, no new object. Preserve the exact face proportions and hair shape, warm afternoon gym background and ink line style. Quiet gentle gym room tone, no voices or music.",
>   "method": "LTX-2.3 q4 / MLX whole-scene I2V, float32 default compute, block streaming, sequential Gemma/connector",
>   "exit_code": 0,
>   "wall_seconds": 224.6,
>   "output_exists": true
> }
> ```

> [!info]- videos/sakuragi-haruko-after-school-30s-20261110/outputs/checks/generation-gather-v1.json
> ```json
> {
>   "scene": "gather",
>   "output": "public/video/gather-native24-v1.mp4",
>   "frames": 73,
>   "width": 1024,
>   "height": 576,
>   "fps": 24,
>   "seed": 11210,
>   "source_image": "public/art/05-gather.png",
>   "source_image_sha256": "2e1c326d96aef2b01af07b88bf00d8e6ace82db4c139265edee2096a9ab4b39d",
>   "prompt": "A clean hand-drawn 1990s Japanese basketball anime scene at natural real-time speed. Locked camera, preserve the exact original drawing style and both character identities and clothing. The tall red pompadour boy Sakuragi keeps both feet planted and holds exactly one round basketball in both hands. In one smooth modest preparation action he bends his knees slightly and brings the ball from his lower abdomen up to his chest, then settles into a balanced ready stance. His hands remain securely on the same basketball throughout. He concentrates toward the offscreen basket on his left, no shot or release, no jump. The shorter brown-haired schoolgirl Haruko stands separately at the right, stays in her own place, follows his movement with her eyes and gives one small supportive nod. Their bodies remain clearly separated. Anatomically stable hands, elbows, knees and faces, no extra ball, no camera motion. Warm afternoon light, stable gym walls and hoop. Quiet airy indoor gym room tone, a soft sneaker squeak and clothes rustle, no speech, no music.",
>   "method": "LTX-2.3 q4 / MLX whole-scene I2V, float32 default compute, block streaming, sequential Gemma/connector",
>   "exit_code": 0,
>   "wall_seconds": 373.75,
>   "output_exists": true
> }
> ```

> [!info]- videos/sakuragi-haruko-after-school-30s-20261110/outputs/checks/generation-haruko-v1.json
> ```json
> {
>   "scene": "haruko",
>   "output": "public/video/haruko-native24-v1.mp4",
>   "frames": 97,
>   "width": 1024,
>   "height": 576,
>   "fps": 24,
>   "seed": 11210,
>   "source_image": "public/art/02-haruko.png",
>   "source_image_sha256": "5fdc05637fcff098467d7f1615843f2f8c9be19711fe2e47c5a609db29a69e25",
>   "prompt": "A warm clean 1990s hand inked anime medium close-up of Haruko Akagi, exact same face, shoulder length chestnut hair, bangs and white school blouse with red ribbon. Locked camera. She looks to offscreen left toward Sakuragi, gently nods once and opens a bright encouraging smile, then relaxes her empty hands a little. Small expressive real-time head and shoulder movement and a natural blink. Remain in place, no walking, no large hand gestures. Preserve face proportions, hair length, finger anatomy and gym background. Same afternoon light. Quiet gym room ambience, subtle clothes rustle, no intelligible or unintelligible speech, no music.",
>   "method": "LTX-2.3 q4 / MLX whole-scene I2V, float32 default compute, block streaming, sequential Gemma/connector",
>   "exit_code": 0,
>   "wall_seconds": 405.88,
>   "output_exists": true
> }
> ```

> [!info]- videos/sakuragi-haruko-after-school-30s-20261110/outputs/checks/generation-opening-v1.json
> ```json
> {
>   "scene": "opening",
>   "output": "public/video/opening-native24-v1.mp4",
>   "frames": 97,
>   "width": 1024,
>   "height": 576,
>   "fps": 24,
>   "seed": 11210,
>   "source_image": "public/art/01-two-shot.png",
>   "source_image_sha256": "e423d77f079b31e3fd15da795de92a2e8d37b5c7149781c06cdf98904b26b02e",
>   "prompt": "A warm hand drawn 1990s basketball manga animation, natural real-time speed, locked camera. Preserve the exact red pompadour boy Sakuragi and brown haired girl Haruko and all clothes. The two remain standing in their own places with clear space between them. Haruko turns her face up toward Sakuragi, smiles reassuringly and gives one small supportive nod. Sakuragi responds by turning his head slightly toward her and smiling bashfully, shifting his weight gently without moving his feet. He continuously holds exactly one basketball at his waist with both hands, keeps the ball still and secure. Stable anatomy and unchanged gym, restrained natural acting, not slow motion. Warm window light, quiet roomy gym ambience and soft fabric movement, no speech, no music.",
>   "method": "LTX-2.3 q4 / MLX whole-scene I2V, float32 default compute, block streaming, sequential Gemma/connector",
>   "exit_code": 0,
>   "wall_seconds": 430.63,
>   "output_exists": true
> }
> ```

> [!info]- videos/sakuragi-haruko-after-school-30s-20261110/outputs/checks/generation-practice-v1.json
> ```json
> {
>   "scene": "practice",
>   "output": "public/video/practice-native24-v1.mp4",
>   "frames": 97,
>   "width": 1024,
>   "height": 576,
>   "fps": 24,
>   "seed": 11210,
>   "source_image": "public/art/04-practice.png",
>   "source_image_sha256": "5c644d1341014c809fbed041b1fd70d75ea7aee62e1871e54f8d53f4e8849f7c",
>   "prompt": "A clean continuous hand drawn 1990s basketball anime scene, natural real-time stationary dribbling, fixed camera. Same tall red pompadour Sakuragi in red SHOHOKU 10 uniform at left and shorter brown haired Haruko in white blouse and navy skirt at far right. Sakuragi remains in his own spot with both feet firmly planted, knees flexed. With his right hand only he steadily dribbles exactly one orange basketball vertically beside his right leg, between knee height and the wooden floor, roughly two controlled bounces per second. Each bounce hits the floor before returning up to the same palm. His right wrist and elbow follow the ball naturally, left hand stays free for balance. He looks down with determined concentration and small balanced torso movement. Haruko remains clearly separate, quietly follows the ball with her eyes and gives a tiny approving nod, hands empty at her waist. No moving across court, no crossover, no ball passing or shooting. Preserve one round ball, correct hands and knees, no merging bodies, stable rigid hoop and background. Natural dribble impacts and light sneaker squeak in quiet gym, no speech, no music.",
>   "method": "LTX-2.3 q4 / MLX whole-scene I2V, float32 default compute, block streaming, sequential Gemma/connector",
>   "exit_code": 0,
>   "wall_seconds": 434.2,
>   "output_exists": true
> }
> ```

> [!info]- videos/sakuragi-haruko-after-school-30s-20261110/outputs/checks/generation-practice-v2.json
> ```json
> {
>   "scene": "practice",
>   "output": "public/video/practice-native24-v2.mp4",
>   "frames": 97,
>   "width": 1024,
>   "height": 576,
>   "fps": 24,
>   "seed": 11210,
>   "source_image": "public/art/04-stance-v2.png",
>   "source_image_sha256": "ecb8bb70167b08b96306b04a5148cf22520aaf9bfbf483b017a63548ea25c1f8",
>   "prompt": "A clean hand-drawn 1990s basketball manga animation at normal real-time speed, locked full-body camera. Preserve the exact red pompadour Sakuragi in red number 10 jersey and brown-haired Haruko in white blouse and navy skirt. There is no basketball in this empty-hand balance drill. Sakuragi keeps both shoes planted shoulder width apart and makes one small smooth knee bend into a balanced ready stance, then straightens slightly and settles. His torso stays balanced above his feet, shoulders and empty hands move naturally with the modest bend. Both empty hands stay loosely beside his hips and clearly separated from his legs, no crossing limbs, no jump or walking. He glances briefly toward Haruko at far right, then looks forward with earnest concentration. Haruko remains standing well apart, keeps her empty hands at her own waist and gives one small approving nod. No new object or ball appears. Natural stable faces, fingers, knees and foot contact. Same warm afternoon light and rigid old gym architecture. Soft sneaker creak and fabric rustle in quiet gym room tone, no voice, no music.",
>   "method": "LTX-2.3 q4 / MLX whole-scene I2V, float32 default compute, block streaming, sequential Gemma/connector",
>   "exit_code": 0,
>   "wall_seconds": 423.42,
>   "output_exists": true
> }
> ```

> [!info]- videos/sakuragi-haruko-after-school-30s-20261110/outputs/checks/generation-resolve-v1.json
> ```json
> {
>   "scene": "resolve",
>   "output": "public/video/resolve-native24-v1.mp4",
>   "frames": 97,
>   "width": 1024,
>   "height": 576,
>   "fps": 24,
>   "seed": 11210,
>   "source_image": "public/art/07-resolve.png",
>   "source_image_sha256": "a78a942575405535503121607603808652587260ab84331702eb0e16d3bfd3a1",
>   "prompt": "An expressive natural-time 1990s hand drawn basketball anime shot of red pompadour Sakuragi in the same number 10 red jersey. Locked medium close-up, exact same face, hairstyle and athletic body. He lifts his right clenched fist a small distance in a single restrained confident yes gesture, smiles with bashful pride toward offscreen Haruko on the right, then gives one affirmative nod and relaxes his fist. Left forearm keeps exactly one basketball secure at left hip, never releases or dribbles it. Feet stay in place outside crop, shoulders naturally move with the nod. No extra hands or balls, no deformed fingers, preserve jersey 10 and gym geometry. Soft cloth movement, calm gym room tone, no speaking, no music.",
>   "method": "LTX-2.3 q4 / MLX whole-scene I2V, float32 default compute, block streaming, sequential Gemma/connector",
>   "exit_code": 0,
>   "wall_seconds": 413.68,
>   "output_exists": true
> }
> ```

> [!info]- videos/sakuragi-haruko-after-school-30s-20261110/outputs/checks/generation-shy-v1.json
> ```json
> {
>   "scene": "shy",
>   "output": "public/video/shy-native24-v1.mp4",
>   "frames": 97,
>   "width": 1024,
>   "height": 576,
>   "fps": 24,
>   "seed": 11210,
>   "source_image": "public/art/03-shy.png",
>   "source_image_sha256": "b2da502b3e110553e7eeabd3f4bccd16eb7f20340ae5d6bae9d87f4ac6ebe07e",
>   "prompt": "Natural real-time 1990s hand-drawn basketball anime acting, fixed camera, same red pompadour Sakuragi and red number 10 jersey. In one continuous sheepish gesture his raised right hand reaches to the back of his neck and rubs it briefly, he dips his chin a little with a bashful grin and glances toward offscreen Haruko on the right. His muscular shoulders relax as he smiles, one natural blink. Keep head shape and red pompadour stable, do not change his age or hairstyle. The left arm stays down outside the crop. No ball appears in frame. Correct connected hand, arm and elbow throughout, no additional hands. Stable warm gym background, soft fabric rustle and room tone, no voice, no music.",
>   "method": "LTX-2.3 q4 / MLX whole-scene I2V, float32 default compute, block streaming, sequential Gemma/connector",
>   "exit_code": 0,
>   "wall_seconds": 411.34,
>   "output_exists": true
> }
> ```

> [!info]- videos/sakuragi-haruko-after-school-30s-20261110/outputs/checks/generation-summary.json
> ```json
> {
>   "updated_at": "2026-11-10T15:24:15.084201+08:00",
>   "candidates": [
>     {
>       "candidate": "cheer-v1",
>       "frames": 97,
>       "width": 1024,
>       "height": 576,
>       "fps": 24,
>       "seed": 11210,
>       "seconds": 423.61,
>       "exit_code": 0,
>       "mlx_peak_gib": 10.903
>     },
>     {
>       "candidate": "ending-v1",
>       "frames": 97,
>       "width": 1024,
>       "height": 576,
>       "fps": 24,
>       "seed": 11210,
>       "seconds": 421.17,
>       "exit_code": 0,
>       "mlx_peak_gib": 10.903
>     },
>     {
>       "candidate": "epilogue-v1",
>       "frames": 49,
>       "width": 1024,
>       "height": 576,
>       "fps": 24,
>       "seed": 11210,
>       "seconds": 224.6,
>       "exit_code": 0,
>       "mlx_peak_gib": 8.625
>     },
>     {
>       "candidate": "gather-v1",
>       "frames": 73,
>       "width": 1024,
>       "height": 576,
>       "fps": 24,
>       "seed": 11210,
>       "seconds": 373.75,
>       "exit_code": 0,
>       "mlx_peak_gib": 9.808
>     },
>     {
>       "candidate": "haruko-v1",
>       "frames": 97,
>       "width": 1024,
>       "height": 576,
>       "fps": 24,
>       "seed": 11210,
>       "seconds": 405.88,
>       "exit_code": 0,
>       "mlx_peak_gib": 10.903
>     },
>     {
>       "candidate": "opening-v1",
>       "frames": 97,
>       "width": 1024,
>       "height": 576,
>       "fps": 24,
>       "seed": 11210,
>       "seconds": 430.63,
>       "exit_code": 0,
>       "mlx_peak_gib": 10.903
>     },
>     {
>       "candidate": "practice-v1",
>       "frames": 97,
>       "width": 1024,
>       "height": 576,
>       "fps": 24,
>       "seed": 11210,
>       "seconds": 434.2,
>       "exit_code": 0,
>       "mlx_peak_gib": 10.903
>     },
>     {
>       "candidate": "practice-v2",
>       "frames": 97,
>       "width": 1024,
>       "height": 576,
>       "fps": 24,
>       "seed": 11210,
>       "seconds": 423.42,
>       "exit_code": 0,
>       "mlx_peak_gib": 10.903
>     },
>     {
>       "candidate": "resolve-v1",
>       "frames": 97,
>       "width": 1024,
>       "height": 576,
>       "fps": 24,
>       "seed": 11210,
>       "seconds": 413.68,
>       "exit_code": 0,
>       "mlx_peak_gib": 10.903
>     },
>     {
>       "candidate": "shy-v1",
>       "frames": 97,
>       "width": 1024,
>       "height": 576,
>       "fps": 24,
>       "seed": 11210,
>       "seconds": 411.34,
>       "exit_code": 0,
>       "mlx_peak_gib": 10.903
>     }
>   ],
>   "total_inference_seconds": 3962.28,
>   "note": "Includes prompt encoding, denoising, decoding and muxing. Excludes image generation, setup, visual QA and editing. MLX peak is not total process or system memory."
> }
> ```

> [!info]- videos/sakuragi-haruko-after-school-30s-20261110/outputs/checks/lipsync/cheer-v1-generation.json
> ```json
> {
>   "scene": "cheer",
>   "take": "v1",
>   "frames": 97,
>   "fps": 24,
>   "width": 1024,
>   "height": 576,
>   "seed": 11210,
>   "pipeline": "A2VidDistilledPipeline; LTX-2.3 q4; frozen source audio in both stages",
>   "reference": "public/lipsync/inputs/cheer.png",
>   "audio": "public/lipsync/inputs/cheer.wav",
>   "prompt": "A continuous medium close-up of Haruko Akagi from the reference illustration, traditional hand-drawn 1990s Japanese basketball anime with stable ink lines and flat cel shading. The brown-haired teenage girl in a white short-sleeved blouse and red ribbon says the Mandarin sentence in the supplied audio: \"对，就是这样！\" Her mouth and jaw visibly articulate the entire line in time with that exact voice. Her eyes brighten with sincere delight at Sakuragi's progress, looking off camera toward the left. She gives two or three small, gentle claps at chest level with naturally formed separated hands that briefly meet palm to palm, then lowers and relaxes her hands. She speaks while smiling, not a broad silent laugh, and closes her mouth into a pleased small smile after the voice ends. Preserve her face, bangs, shoulder-length brown hair, outfit, gym perspective and warm afternoon light. Locked camera, one girl alone, restrained natural animation at normal speed.",
>   "inputs_sha256": {
>     "public/lipsync/inputs/cheer.png": "514187d1172f7f39ec2471d817d51751218705960a8e88c19332e52de65556e0",
>     "public/lipsync/inputs/cheer.wav": "f2c027e955b6b727d7408d3f4a9a8391f87aeb06a2c7b32bdcc2a4e1ed9da574",
>     "docs/lipsync/cheer-v1-prompt.txt": "789bc97cc199a9a50f4b5a6b82ed4e1b5f72d841395ff6a358066c1d4bee4d23"
>   },
>   "output": "public/lipsync/candidates/cheer-v1.mp4",
>   "exit_code": 0,
>   "wall_seconds": 430.15,
>   "output_exists": true
> }
> ```

> [!info]- videos/sakuragi-haruko-after-school-30s-20261110/outputs/checks/lipsync/ending-v1-generation.json
> ```json
> {
>   "scene": "ending",
>   "take": "v1",
>   "frames": 65,
>   "fps": 24,
>   "width": 1024,
>   "height": 576,
>   "seed": 11210,
>   "pipeline": "A2VidDistilledPipeline; LTX-2.3 q4; frozen source audio in both stages",
>   "reference": "public/lipsync/inputs/ending.png",
>   "audio": "public/lipsync/inputs/ending.wav",
>   "prompt": "A continuous wide two-person shot from the reference illustration, hand-drawn 1990s Japanese basketball anime, stable ink lines and flat cel colors. On the left, Hanamichi Sakuragi, the red pompadour-haired teenage boy in red Shohoku number 10 uniform, speaks the Mandarin sentence in the supplied audio: \"好，再来一遍！\" Only Sakuragi speaks. He visibly articulates the words in time with the exact male voice, with bright determined eyes and a cheerful confident expression, then closes his mouth in a satisfied smile when the voice stops. His right fist remains raised near his chest, his left arm steadily holds one basketball against his left hip. On the right, Haruko Akagi in a white blouse, red ribbon and navy skirt quietly listens with her mouth closed and gives a tiny pleased nod. Her hands remain clasped in front. Both keep their original separated standing positions, no steps, no hand or body contact. Preserve the reference identities, proportions, gym architecture, floor perspective and afternoon light. Locked camera, natural conversational speed.",
>   "inputs_sha256": {
>     "public/lipsync/inputs/ending.png": "4539451c9bab5841d991aa456ebaa8efc3c487750bf5f067ac920208a1b3e119",
>     "public/lipsync/inputs/ending.wav": "58d3a1a6e395067ca57b9c3228168cfff5a6e1b9733ddd068c746f44c0ce5bf7",
>     "docs/lipsync/ending-v1-prompt.txt": "86b57198eb03abbdc158ee2d3cfdb2884c73ad32f1cb35f1e4543f56a91f463d"
>   },
>   "output": "public/lipsync/candidates/ending-v1.mp4",
>   "exit_code": 0,
>   "wall_seconds": 290.61,
>   "output_exists": true
> }
> ```

> [!info]- videos/sakuragi-haruko-after-school-30s-20261110/outputs/checks/lipsync/haruko-v1-generation.json
> ```json
> {
>   "scene": "haruko",
>   "take": "v1",
>   "frames": 97,
>   "fps": 24,
>   "width": 1024,
>   "height": 576,
>   "seed": 11210,
>   "pipeline": "A2VidDistilledPipeline; LTX-2.3 q4; frozen source audio in both stages",
>   "reference": "public/lipsync/inputs/haruko.png",
>   "audio": "public/lipsync/inputs/haruko.wav",
>   "prompt": "A continuous medium close-up of Haruko Akagi from the reference illustration, traditional hand-drawn 1990s Japanese basketball anime, stable ink lines, flat cel shading. The brown-haired teenage girl in a white short-sleeved blouse and red ribbon speaks the Mandarin sentence in the provided audio: \"先站稳，别着急。\" Her visible mouth articulates the words naturally and in time with that exact audio. She looks gently toward Sakuragi off camera to the left. She delivers a patient, reassuring suggestion with relaxed brows and a warm small smile, a slight encouraging nod, not a broad laugh. Her hands stay loosely clasped near her chest throughout, no new gesture. Her jaw and lips move clearly while speaking and settle into a quiet closed-mouth smile when the voice stops. Preserve the reference face shape, brown bangs, shoulder-length hair, outfit, gym perspective and warm left sunlight. Fixed camera, natural conversational speed. One girl alone in frame.",
>   "inputs_sha256": {
>     "public/lipsync/inputs/haruko.png": "33410ffd6d7b805d5083536bbf313dd1734acea8b2c1814bb80aef4d22ef3289",
>     "public/lipsync/inputs/haruko.wav": "93a254c139e017cf3c0d202bd9f9dc42b168308b1e6aec258ead41cfbe2b887a",
>     "docs/lipsync/haruko-v1-prompt.txt": "526b2a2c2ac9ed733642b948bad308d5f478115273740e9213d3ac90427674e3"
>   },
>   "output": "public/lipsync/candidates/haruko-v1.mp4",
>   "exit_code": 0,
>   "wall_seconds": 407.17,
>   "output_exists": true
> }
> ```

> [!info]- videos/sakuragi-haruko-after-school-30s-20261110/outputs/checks/lipsync/opening-v1-generation.json
> ```json
> {
>   "scene": "opening",
>   "take": "v1",
>   "frames": 97,
>   "fps": 24,
>   "width": 1024,
>   "height": 576,
>   "seed": 11210,
>   "pipeline": "A2VidDistilledPipeline; LTX-2.3 q4; frozen source audio in both stages",
>   "reference": "public/lipsync/inputs/opening.png",
>   "audio": "public/lipsync/inputs/opening.wav",
>   "prompt": "A continuous wide two-person shot from the reference illustration, traditional hand-drawn 1990s Japanese basketball anime with clean ink lines and flat cel colors. On the right, Haruko Akagi, the brown-haired girl in a white blouse, red ribbon and navy skirt, speaks the Mandarin sentence heard in the provided audio: \"樱木同学，再试一次吧。\" Only Haruko speaks. Her mouth articulates naturally in time with the supplied female voice, her eyes and warm encouraging smile aimed toward Sakuragi on the left, with a small reassuring nod. On the left, red pompadour-haired Hanamichi Sakuragi in his red Shohoku number 10 uniform quietly listens with a closed mouth, looking toward her. His two hands hold a single basketball steadily in front of his waist, unchanged grip. Both remain in the same separate positions, no walking or contact. Haruko's hands stay gently clasped in front of her skirt. Her mouth becomes a quiet closed smile after her voice ends. Preserve character identities, clothes, original gym architecture, perspective and sunlight. Fixed camera, natural conversational speed.",
>   "inputs_sha256": {
>     "public/lipsync/inputs/opening.png": "0c99f557df7d97ef5be8f79570cd33590a738c04d646ad694220727573558066",
>     "public/lipsync/inputs/opening.wav": "2508f5540fdb5068f6432cc3eeb5d9a7048c8748f474f8d264c6e80ee928f461",
>     "docs/lipsync/opening-v1-prompt.txt": "f4657a76bd9ff264938f5ed0a26321422e25ccb839c25df885d735a584702493"
>   },
>   "output": "public/lipsync/candidates/opening-v1.mp4",
>   "exit_code": 0,
>   "wall_seconds": 440.96,
>   "output_exists": true
> }
> ```

> [!info]- videos/sakuragi-haruko-after-school-30s-20261110/outputs/checks/lipsync/resolve-v1-generation.json
> ```json
> {
>   "scene": "resolve",
>   "take": "v1",
>   "frames": 97,
>   "fps": 24,
>   "width": 1024,
>   "height": 576,
>   "seed": 11210,
>   "pipeline": "A2VidDistilledPipeline; LTX-2.3 q4; frozen source audio in both stages",
>   "reference": "public/lipsync/inputs/resolve.png",
>   "audio": "public/lipsync/inputs/resolve.wav",
>   "prompt": "A continuous medium close-up from the reference illustration, traditional hand-drawn Japanese basketball anime, clean ink lines and flat cel colors. Hanamichi Sakuragi, the red pompadour-haired teenage boy in a red Shohoku basketball jersey, speaks the Mandarin Chinese sentence heard in the provided audio: \"我可是天才嘛。\" He starts with his mouth closed in a confident small grin, then at the exact onset of his voice he articulates the whole sentence with clearly visible, natural jaw and mouth shapes synchronized to the provided voice, including the brief lip closure on the final \"嘛\" before opening the vowel. He raises one eyebrow playfully and keeps a boastful yet friendly amused expression, small head motion, gaze toward Haruko off camera on the right. His mouth becomes a closed satisfied smile after his voice finishes. His right hand stays a relaxed fist beside his chest, with five anatomically coherent fingers. Preserve the same red hair, facial proportions, jersey, perspective, warm wooden gym interior and afternoon light. The camera remains locked. One boy alone in frame. Natural conversational speed, restrained 2D animation, stable identity throughout.",
>   "inputs_sha256": {
>     "public/lipsync/inputs/resolve.png": "1030c126927678b196f3e1d0dfe83a5ddb861b8ef632f7c34d75091d8f9858e2",
>     "public/lipsync/inputs/resolve.wav": "106971922ff1f3e413a9cf0975c8d7c3ccdf7e09c66ccccc437d835a707a29b5",
>     "docs/lipsync/resolve-v1-prompt.txt": "9fe44d9c36f1e8628980693ee1398ad8227ac65c4a548d667833e47be22b0245"
>   },
>   "output": "public/lipsync/candidates/resolve-v1.mp4",
>   "exit_code": 0,
>   "wall_seconds": 446.91,
>   "output_exists": true
> }
> ```

> [!info]- videos/sakuragi-haruko-after-school-30s-20261110/outputs/checks/lipsync/shy-v1-generation.json
> ```json
> {
>   "scene": "shy",
>   "take": "v1",
>   "frames": 89,
>   "fps": 24,
>   "width": 1024,
>   "height": 576,
>   "seed": 11210,
>   "pipeline": "A2VidDistilledPipeline; LTX-2.3 q4; frozen source audio in both stages",
>   "reference": "public/lipsync/inputs/shy.png",
>   "audio": "public/lipsync/inputs/shy.wav",
>   "prompt": "A continuous medium close-up of Hanamichi Sakuragi from the reference illustration, hand-drawn 1990s Japanese basketball anime with stable clean ink lines and flat cel shading. The red pompadour-haired teenage boy in the red Shohoku basketball jersey speaks the Mandarin sentence in the supplied audio: \"交给我吧，晴子小姐！\" His mouth and jaw visibly articulate the sentence in exact time with that audio. He starts a little proud, then grows bashful and warmly excited while addressing Haruko off camera on the right. A small blush, softened eyebrows and bright eyes convey his happy embarrassment. His right hand gently touches the back of his own neck and stays there, anatomically coherent fingers. Keep his head facing three quarters right, restrained small motions, no large turn. He closes his mouth into a shy satisfied grin after finishing the line. Preserve his face proportions, red hairstyle, jersey and warm wooden gym background. Locked camera, one boy alone, natural speaking speed.",
>   "inputs_sha256": {
>     "public/lipsync/inputs/shy.png": "62ffa64c4e48b5f5561c7f03d255591b415dfda963316a39cc77fab773d3665d",
>     "public/lipsync/inputs/shy.wav": "095bb130a8a2ae3fd78d976aa624122a821a780bef8fc97e659afb4d99ca1c0a",
>     "docs/lipsync/shy-v1-prompt.txt": "9a93bcbd9935d61632795c1365d0d5366a778e39ed206facd049bbdfb75b097d"
>   },
>   "output": "public/lipsync/candidates/shy-v1.mp4",
>   "exit_code": 0,
>   "wall_seconds": 376.8,
>   "output_exists": true
> }
> ```

> [!info]- videos/sakuragi-haruko-after-school-30s-20261110/outputs/checks/lipsync/shy-v2-generation.json
> ```json
> {
>   "scene": "shy",
>   "take": "v2",
>   "frames": 89,
>   "fps": 24,
>   "width": 1024,
>   "height": 576,
>   "seed": 11211,
>   "pipeline": "A2VidDistilledPipeline; LTX-2.3 q4; frozen source audio in both stages",
>   "reference": "public/lipsync/inputs/shy.png",
>   "audio": "public/lipsync/inputs/shy.wav",
>   "prompt": "A continuous medium close-up of Hanamichi Sakuragi from the reference illustration, traditional hand-drawn 1990s Japanese basketball anime, clean ink lines and flat cel shading. He maintains the exact same three-quarter-right head orientation and gaze toward Haruko off camera for the entire shot. The opening is quiet: he holds a small closed-mouth bashful smile until the male voice actually begins in the supplied audio. Then he speaks the Mandarin sentence \"交给我吧，晴子小姐！\" with visible natural lip and jaw articulation following that exact audio, closing his lips at the short pauses and returning to a closed, softly blushing smile as soon as the phrase ends. His confident delivery gently softens with happy embarrassment when addressing Haruko. His right hand gently settles on the back of his own neck near the beginning, then rests there with a stable elbow and natural connected wrist. His head remains at the same three-quarter-right angle through the final closed-mouth smile, with small natural blinks and breathing. Preserve his red pompadour hairstyle, facial proportions, five-finger hand, red Shohoku jersey and warm wooden gym background from the image anchors. Locked camera, one boy alone, normal conversational speed.",
>   "inputs_sha256": {
>     "public/lipsync/inputs/shy.png": "62ffa64c4e48b5f5561c7f03d255591b415dfda963316a39cc77fab773d3665d",
>     "public/lipsync/inputs/shy.wav": "095bb130a8a2ae3fd78d976aa624122a821a780bef8fc97e659afb4d99ca1c0a",
>     "docs/lipsync/shy-v2-prompt.txt": "a9c2be2900dbcd95473755a1c62ba30f71ea47e4585cbea87fb34f5a065a54f7",
>     "public/lipsync/inputs/shy-v2-before-speech.png": "418643802120560d995efeb00b0802a78d9df6f0e702dbf8e79ee3dddfd24765",
>     "public/lipsync/inputs/shy-v2-after-speech.png": "cdf8ce9696103c2c98684dd69a732ee3dab2dd329c3b9d8138924dc3720a8229"
>   },
>   "output": "public/lipsync/candidates/shy-v2.mp4",
>   "anchors": [
>     {
>       "image": "public/lipsync/inputs/shy-v2-before-speech.png",
>       "frame": 17,
>       "strength": 1.0,
>       "binding": "guide"
>     },
>     {
>       "image": "public/lipsync/inputs/shy-v2-after-speech.png",
>       "frame": 81,
>       "strength": 1.0,
>       "binding": "guide"
>     }
>   ],
>   "exit_code": 0,
>   "wall_seconds": 456.17,
>   "output_exists": true
> }
> ```

> [!info]- videos/sakuragi-haruko-after-school-30s-20261110/outputs/checks/voiceover/generation-cheer-v1.json
> ```json
> {
>   "id": "cheer",
>   "character": "haruko",
>   "text": "对，就是这样！",
>   "start_frame": 444,
>   "end_frame": 512,
>   "emotion": "看见朋友有进步，真心高兴地轻快赞许，有笑意但不尖叫。",
>   "take": "v1",
>   "seed": 2210,
>   "instruct": "年轻女性，十七岁左右温柔明亮的少女音色，中高音区，清澈自然、温暖亲切，带笑意和真诚的鼓励。说标准国语，带非常轻微的九十年代台湾动画国语配音语感，咬字清楚，声音柔和但有精神，不是幼儿音，不嗲，不是客服播音腔，没有气声。像经典篮球动画里开朗善良的女同学在球馆边鼓励朋友。自然连贯的角色对白。 本句表演：看见朋友有进步，真心高兴地轻快赞许，有笑意但不尖叫。 自然说出文字，不朗读表演说明。",
>   "model_repo": "mlx-community/Qwen3-TTS-12Hz-1.7B-VoiceDesign-bf16",
>   "model_revision": "7d3824abff87e49756bb0f83fb5411de75d160c4",
>   "mlx_audio_version": "0.6.5",
>   "sample_rate": 24000,
>   "samples": 28800,
>   "raw_seconds": 1.2,
>   "wall_seconds": 1.268,
>   "mlx_peak_gib": 5.29912190977484,
>   "file": "public/audio/voices/raw/cheer-v1.wav",
>   "sha256": "e1c93c430f870720ae124bdef1cd20fe49cdb47035b22c5ae110037384249294"
> }
> ```

> [!info]- videos/sakuragi-haruko-after-school-30s-20261110/outputs/checks/voiceover/generation-ending-v1.json
> ```json
> {
>   "id": "ending",
>   "character": "sakuragi",
>   "text": "好，再来一遍！",
>   "start_frame": 631,
>   "end_frame": 680,
>   "emotion": "恢复热血，爽朗而干脆，准备再练一次，不拖长音，不是大喊。",
>   "take": "v1",
>   "seed": 1110,
>   "instruct": "年轻男性，十八岁左右的热血少年音色，中低音区，声音厚实有胸腔共鸣，轻微沙哑和鼻腔共鸣，带一点调皮和笨拙的自信。说标准国语，带非常轻微的九十年代台湾动画国语配音语感，咬字清楚，有活力但不是播音腔，不是大叔音，不用气泡音。表演有经典热血篮球动画男主角的夸张起伏，面对喜欢的女生会有一点害羞。自然、爽朗、真诚的角色对白。 本句表演：恢复热血，爽朗而干脆，准备再练一次，不拖长音，不是大喊。 自然说出文字，不朗读表演说明。",
>   "model_repo": "mlx-community/Qwen3-TTS-12Hz-1.7B-VoiceDesign-bf16",
>   "model_revision": "7d3824abff87e49756bb0f83fb5411de75d160c4",
>   "mlx_audio_version": "0.6.5",
>   "sample_rate": 24000,
>   "samples": 28800,
>   "raw_seconds": 1.2,
>   "wall_seconds": 1.146,
>   "mlx_peak_gib": 5.834151607938111,
>   "file": "public/audio/voices/raw/ending-v1.wav",
>   "sha256": "c85b83bba6db833aad6ef9dec192e13243670962c4831b08a17f6eabf9346a9f"
> }
> ```

> [!info]- videos/sakuragi-haruko-after-school-30s-20261110/outputs/checks/voiceover/generation-haruko-v1.json
> ```json
> {
>   "id": "haruko",
>   "character": "haruko",
>   "text": "先站稳，别着急。",
>   "start_frame": 109,
>   "end_frame": 184,
>   "emotion": "耐心地给建议，亲切自然，不说教。",
>   "take": "v1",
>   "seed": 2210,
>   "instruct": "年轻女性，十七岁左右温柔明亮的少女音色，中高音区，清澈自然、温暖亲切，带笑意和真诚的鼓励。说标准国语，带非常轻微的九十年代台湾动画国语配音语感，咬字清楚，声音柔和但有精神，不是幼儿音，不嗲，不是客服播音腔，没有气声。像经典篮球动画里开朗善良的女同学在球馆边鼓励朋友。自然连贯的角色对白。 本句表演：耐心地给建议，亲切自然，不说教。 自然说出文字，不朗读表演说明。",
>   "model_repo": "mlx-community/Qwen3-TTS-12Hz-1.7B-VoiceDesign-bf16",
>   "model_revision": "7d3824abff87e49756bb0f83fb5411de75d160c4",
>   "mlx_audio_version": "0.6.5",
>   "sample_rate": 24000,
>   "samples": 30720,
>   "raw_seconds": 1.28,
>   "wall_seconds": 2.286,
>   "mlx_peak_gib": 5.29912190977484,
>   "file": "public/audio/voices/raw/haruko-v1.wav",
>   "sha256": "079c1bf8245131cc9607121695dcc068b1c825e42ab61f541f2e2a3855ee5d14"
> }
> ```

> [!info]- videos/sakuragi-haruko-after-school-30s-20261110/outputs/checks/voiceover/generation-opening-v1.json
> ```json
> {
>   "id": "opening",
>   "character": "haruko",
>   "text": "樱木同学，再试一次吧。",
>   "start_frame": 14,
>   "end_frame": 91,
>   "emotion": "温柔、明亮，轻松地鼓励对方再尝试一次，句尾的吧轻轻扬起。",
>   "take": "v1",
>   "seed": 2210,
>   "instruct": "年轻女性，十七岁左右温柔明亮的少女音色，中高音区，清澈自然、温暖亲切，带笑意和真诚的鼓励。说标准国语，带非常轻微的九十年代台湾动画国语配音语感，咬字清楚，声音柔和但有精神，不是幼儿音，不嗲，不是客服播音腔，没有气声。像经典篮球动画里开朗善良的女同学在球馆边鼓励朋友。自然连贯的角色对白。 本句表演：温柔、明亮，轻松地鼓励对方再尝试一次，句尾的吧轻轻扬起。 自然说出文字，不朗读表演说明。",
>   "model_repo": "mlx-community/Qwen3-TTS-12Hz-1.7B-VoiceDesign-bf16",
>   "model_revision": "7d3824abff87e49756bb0f83fb5411de75d160c4",
>   "mlx_audio_version": "0.6.5",
>   "sample_rate": 24000,
>   "samples": 49920,
>   "raw_seconds": 2.08,
>   "wall_seconds": 7.302,
>   "mlx_peak_gib": 5.771390252746642,
>   "file": "public/audio/voices/raw/opening-v1.wav",
>   "sha256": "4a2c86673adb8e52dcafbf60339300b80858cec34677d9a8a0cf6a3fb2f7c859"
> }
> ```

> [!info]- videos/sakuragi-haruko-after-school-30s-20261110/outputs/checks/voiceover/generation-resolve-v1.json
> ```json
> {
>   "id": "resolve",
>   "character": "sakuragi",
>   "text": "我可是天才嘛。",
>   "start_frame": 544,
>   "end_frame": 617,
>   "emotion": "故作得意、俏皮地自夸，胸有成竹，句尾嘛有少年的笑意。",
>   "take": "v1",
>   "seed": 1110,
>   "instruct": "年轻男性，十八岁左右的热血少年音色，中低音区，声音厚实有胸腔共鸣，轻微沙哑和鼻腔共鸣，带一点调皮和笨拙的自信。说标准国语，带非常轻微的九十年代台湾动画国语配音语感，咬字清楚，有活力但不是播音腔，不是大叔音，不用气泡音。表演有经典热血篮球动画男主角的夸张起伏，面对喜欢的女生会有一点害羞。自然、爽朗、真诚的角色对白。 本句表演：故作得意、俏皮地自夸，胸有成竹，句尾嘛有少年的笑意。 自然说出文字，不朗读表演说明。",
>   "model_repo": "mlx-community/Qwen3-TTS-12Hz-1.7B-VoiceDesign-bf16",
>   "model_revision": "7d3824abff87e49756bb0f83fb5411de75d160c4",
>   "mlx_audio_version": "0.6.5",
>   "sample_rate": 24000,
>   "samples": 51840,
>   "raw_seconds": 2.16,
>   "wall_seconds": 2.101,
>   "mlx_peak_gib": 5.834151607938111,
>   "file": "public/audio/voices/raw/resolve-v1.wav",
>   "sha256": "8bb7be35a720be38aeb1bd7db3606f15d8b07b38ce6fb3fae081c1fc40014631"
> }
> ```

> [!info]- videos/sakuragi-haruko-after-school-30s-20261110/outputs/checks/voiceover/generation-shy-v1.json
> ```json
> {
>   "id": "shy",
>   "character": "sakuragi",
>   "text": "交给我吧，晴子小姐！",
>   "start_frame": 205,
>   "end_frame": 273,
>   "emotion": "前半句自信地应下，叫晴子小姐时带一点开心又不好意思的笑意，语气活泼。",
>   "take": "v1",
>   "seed": 1110,
>   "instruct": "年轻男性，十八岁左右的热血少年音色，中低音区，声音厚实有胸腔共鸣，轻微沙哑和鼻腔共鸣，带一点调皮和笨拙的自信。说标准国语，带非常轻微的九十年代台湾动画国语配音语感，咬字清楚，有活力但不是播音腔，不是大叔音，不用气泡音。表演有经典热血篮球动画男主角的夸张起伏，面对喜欢的女生会有一点害羞。自然、爽朗、真诚的角色对白。 本句表演：前半句自信地应下，叫晴子小姐时带一点开心又不好意思的笑意，语气活泼。 自然说出文字，不朗读表演说明。",
>   "model_repo": "mlx-community/Qwen3-TTS-12Hz-1.7B-VoiceDesign-bf16",
>   "model_revision": "7d3824abff87e49756bb0f83fb5411de75d160c4",
>   "mlx_audio_version": "0.6.5",
>   "sample_rate": 24000,
>   "samples": 59520,
>   "raw_seconds": 2.48,
>   "wall_seconds": 2.527,
>   "mlx_peak_gib": 6.028321311809123,
>   "file": "public/audio/voices/raw/shy-v1.wav",
>   "sha256": "6c922de16f61204491a566d8638697da8650da445630a22bbba88ea6597e5117"
> }
> ```

## 已有产物与检查依据

文件存在、技术检查、画面检查与用户验收分别看待。下表是本次文件盘点，旧版与当前版可能共存。MP4/HTML 按内容计算 SHA-256，ZIP 仅记录文件大小和修改时间供增量判断。

| 本地产物 | 字节数 | SHA-256 |
| --- | ---: | --- |
| `videos/sakuragi-haruko-after-school-30s-20261110/outputs/review-lipsync.html` | 3647 | `32fc873adc200ffe5ab4c0714ff628b24a0df80342de77b227b7eac5e0e1838f` |
| `videos/sakuragi-haruko-after-school-30s-20261110/outputs/review-voiced.html` | 1523 | `b02142bdbf0852b3bd7d0f0ed74589a950a460faf80d126f6747c623aa235047` |
| `videos/sakuragi-haruko-after-school-30s-20261110/outputs/review.html` | 1509 | `eabe8a693725c1b4f3c40358ddf9b3204c4fa0a456ce57265f62bf8f1d44c29a` |
| `videos/sakuragi-haruko-after-school-30s-20261110/outputs/sakuragi-haruko-30s-lipsync-v2.mp4` | 21860484 | `f47930db9afe20734901596509702800034f44380b69fba0ee25d398839ed9cd` |
| `videos/sakuragi-haruko-after-school-30s-20261110/outputs/sakuragi-haruko-30s-voiced.mp4` | 21860484 | `f47930db9afe20734901596509702800034f44380b69fba0ee25d398839ed9cd` |
| `videos/sakuragi-haruko-after-school-30s-20261110/outputs/sakuragi-haruko-30s.mp4` | 21860484 | `f47930db9afe20734901596509702800034f44380b69fba0ee25d398839ed9cd` |
| `videos/sakuragi-haruko-after-school-30s-20261110/outputs/sakuragi-haruko-project.zip` | 257634679 | `ZIP：仅元数据` |

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
>       "duration": "30.000000",
>       "nb_frames": "720",
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
>       "duration": "30.000000",
>       "nb_frames": "1404"
>     }
>   ],
>   "format": {
>     "duration": "30.000000",
>     "size": "21860484"
>   },
>   "file": "outputs/sakuragi-haruko-30s-lipsync-v2.mp4",
>   "scope": "本次只读取媒体头；未全片解码、播放或重新进行视觉/听觉验收。"
> }
> ```


已有检查报告路径（未在本轮重新执行）：

- `videos/sakuragi-haruko-after-school-30s-20261110/outputs/checks/audio-design.json`
- `videos/sakuragi-haruko-after-school-30s-20261110/outputs/checks/audio-normalization.json`
- `videos/sakuragi-haruko-after-school-30s-20261110/outputs/checks/environment-isolation.json`
- `videos/sakuragi-haruko-after-school-30s-20261110/outputs/checks/final-playback.json`
- `videos/sakuragi-haruko-after-school-30s-20261110/outputs/checks/image-isolation.json`
- `videos/sakuragi-haruko-after-school-30s-20261110/outputs/checks/media-metadata.json`
- `videos/sakuragi-haruko-after-school-30s-20261110/outputs/checks/package-report.json`
- `videos/sakuragi-haruko-after-school-30s-20261110/outputs/checks/selections.json`
- `videos/sakuragi-haruko-after-school-30s-20261110/outputs/checks/source-verification.json`
- `videos/sakuragi-haruko-after-school-30s-20261110/outputs/checks/system-tools.json`
- `videos/sakuragi-haruko-after-school-30s-20261110/outputs/checks/verification.json`
- `videos/sakuragi-haruko-after-school-30s-20261110/outputs/checks/cheer-native24-v1/probe.json`
- `videos/sakuragi-haruko-after-school-30s-20261110/outputs/checks/ending-native24-v1/probe.json`
- `videos/sakuragi-haruko-after-school-30s-20261110/outputs/checks/ending-trim-review/probe.json`
- `videos/sakuragi-haruko-after-school-30s-20261110/outputs/checks/epilogue-native24-v1/probe.json`
- `videos/sakuragi-haruko-after-school-30s-20261110/outputs/checks/final-frames/frames.json`
- `videos/sakuragi-haruko-after-school-30s-20261110/outputs/checks/gather-native24-v1/probe.json`
- `videos/sakuragi-haruko-after-school-30s-20261110/outputs/checks/haruko-native24-v1/probe.json`
- `videos/sakuragi-haruko-after-school-30s-20261110/outputs/checks/lipsync/after-audio-rebuild.json`
- `videos/sakuragi-haruko-after-school-30s-20261110/outputs/checks/lipsync/export-dialogue-timing.json`
- `videos/sakuragi-haruko-after-school-30s-20261110/outputs/checks/lipsync/final-playback.json`
- `videos/sakuragi-haruko-after-school-30s-20261110/outputs/checks/lipsync/input-sample-verification.json`
- `videos/sakuragi-haruko-after-school-30s-20261110/outputs/checks/lipsync/queue.json`
- `videos/sakuragi-haruko-after-school-30s-20261110/outputs/checks/lipsync/timing-verification.json`
- `videos/sakuragi-haruko-after-school-30s-20261110/outputs/checks/lipsync/visual-decisions.json`
- `videos/sakuragi-haruko-after-school-30s-20261110/outputs/checks/lipsync/before-voiceover/final-mix-transcript.json`
- `videos/sakuragi-haruko-after-school-30s-20261110/outputs/checks/lipsync/before-voiceover/playback.json`
- `videos/sakuragi-haruko-after-school-30s-20261110/outputs/checks/lipsync/before-voiceover/video-stream-preserved.json`
- `videos/sakuragi-haruko-after-school-30s-20261110/outputs/checks/lipsync/cheer-v1/audio-frame-rms.json`
- `videos/sakuragi-haruko-after-school-30s-20261110/outputs/checks/lipsync/cheer-v1/probe.json`
- `videos/sakuragi-haruko-after-school-30s-20261110/outputs/checks/lipsync/ending-v1/audio-frame-rms.json`
- `videos/sakuragi-haruko-after-school-30s-20261110/outputs/checks/lipsync/ending-v1/probe.json`
- `videos/sakuragi-haruko-after-school-30s-20261110/outputs/checks/lipsync/haruko-v1/audio-frame-rms.json`
- `videos/sakuragi-haruko-after-school-30s-20261110/outputs/checks/lipsync/haruko-v1/probe.json`
- `videos/sakuragi-haruko-after-school-30s-20261110/outputs/checks/lipsync/opening-v1/audio-frame-rms.json`
- `videos/sakuragi-haruko-after-school-30s-20261110/outputs/checks/lipsync/opening-v1/probe.json`
- `videos/sakuragi-haruko-after-school-30s-20261110/outputs/checks/lipsync/resolve-v1/audio-frame-rms.json`
- `videos/sakuragi-haruko-after-school-30s-20261110/outputs/checks/lipsync/resolve-v1/probe.json`
- `videos/sakuragi-haruko-after-school-30s-20261110/outputs/checks/lipsync/shy-v1/audio-frame-rms.json`
- `videos/sakuragi-haruko-after-school-30s-20261110/outputs/checks/lipsync/shy-v1/probe.json`
- `videos/sakuragi-haruko-after-school-30s-20261110/outputs/checks/lipsync/shy-v2/audio-frame-rms.json`
- `videos/sakuragi-haruko-after-school-30s-20261110/outputs/checks/lipsync/shy-v2/probe.json`
- `videos/sakuragi-haruko-after-school-30s-20261110/outputs/checks/opening-native24-v1/probe.json`
- `videos/sakuragi-haruko-after-school-30s-20261110/outputs/checks/practice-native24-v1/probe.json`
- `videos/sakuragi-haruko-after-school-30s-20261110/outputs/checks/practice-native24-v2/probe.json`
- `videos/sakuragi-haruko-after-school-30s-20261110/outputs/checks/resolve-native24-v1/probe.json`
- `videos/sakuragi-haruko-after-school-30s-20261110/outputs/checks/resolve-reframe-review/probe.json`
- `videos/sakuragi-haruko-after-school-30s-20261110/outputs/checks/shy-native24-v1/probe.json`
- `videos/sakuragi-haruko-after-school-30s-20261110/outputs/checks/shy-reframe-review/probe.json`
- `videos/sakuragi-haruko-after-school-30s-20261110/outputs/checks/shy-repair-review/probe.json`
- `videos/sakuragi-haruko-after-school-30s-20261110/outputs/checks/voiceover/alignment.json`
- `videos/sakuragi-haruko-after-school-30s-20261110/outputs/checks/voiceover/asr-transcripts.json`
- `videos/sakuragi-haruko-after-school-30s-20261110/outputs/checks/voiceover/before-audio-normalization.json`
- `videos/sakuragi-haruko-after-school-30s-20261110/outputs/checks/voiceover/before-final-playback.json`
- `videos/sakuragi-haruko-after-school-30s-20261110/outputs/checks/voiceover/before-package-report.json`
- `videos/sakuragi-haruko-after-school-30s-20261110/outputs/checks/voiceover/before-verification.json`
- `videos/sakuragi-haruko-after-school-30s-20261110/outputs/checks/voiceover/final-mix-transcript.json`
- `videos/sakuragi-haruko-after-school-30s-20261110/outputs/checks/voiceover/playback.json`
- `videos/sakuragi-haruko-after-school-30s-20261110/outputs/checks/voiceover/timing-verification.json`
- `videos/sakuragi-haruko-after-school-30s-20261110/outputs/checks/voiceover/video-stream-preserved.json`

## 制作方法与迭代记录选编

下面是项目自有文档的归档摘录，保留原阶段与原检查表述，个人路径已泛化、相对链接已注明源项目位置。它们属于来源陈述，不代表本次重新完成其中的生成或验收。

> [!info]- 来源文档：videos/sakuragi-haruko-after-school-30s-20261110/README.md
> # 樱木花道 × 晴子 · 放学后的练习
> 
> 30秒，1280×720，24fps。以原漫画前期的红色飞机头樱木、长发晴子与湘北旧式球馆为视觉基准的原创同人小片段。
> 
> 完整场景原画 → 本地 LTX-2.3 q4／MLX 连续表演 → 用角色对白驱动讲话镜头的嘴部与表情 → 原速选取 → Remotion 合成。人物表演来自连续生成；没有用冻结、倒放或慢放补足时长。经典国语风格的AI角色对白配合中文字幕、原创轻配乐、球馆环境音和音效。樱木偏低、略沙哑而热血；晴子清亮温柔。音色按角色气质设计，具体记录见角色配音记录（项目文件：`videos/sakuragi-haruko-after-school-30s-20261110/docs/voiceover/method.md`）与口型修复记录（项目文件：`videos/sakuragi-haruko-after-school-30s-20261110/docs/lipsync/method.md`）。
> 
> 制作依据见 前作方法 Review（项目文件：`videos/sakuragi-haruko-after-school-30s-20261110/docs/method-review.md`）、本片制作记录（项目文件：`videos/sakuragi-haruko-after-school-30s-20261110/docs/production-method.md`）、画面验收（项目文件：`videos/sakuragi-haruko-after-school-30s-20261110/docs/visual-review.md`） 与 制作要求（项目文件：`videos/sakuragi-haruko-after-school-30s-20261110/docs/production-brief.md`）。
> 
> 本次“配音与表情、嘴型不匹配”的用户反馈、问题原因、失败候选、修复验证及后续制作准则，集中记录在口型与表情修复复盘（项目文件：`videos/sakuragi-haruko-after-school-30s-20261110/docs/lipsync/method.md`）。后续可见角色对白按“先定配音与情绪 → 实际音轨驱动画面 → 视觉与技术分别检查”执行。
> 
> ## 独立安装与预览
> 
> 需要 Node.js 22或更新版本、npm、Python 3、系统 ffmpeg／ffprobe 与 Chrome。所有命令在本目录执行。
> 
> ```sh
> mkdir -p .cache/tmp .cache/npm
> export TMPDIR="$PWD/.cache/tmp" TMP="$PWD/.cache/tmp" TEMP="$PWD/.cache/tmp"
> npm ci --cache .cache/npm
> npm run dev
> ```
> 
> 打开 `http://localhost:3101/SakuragiHaruko`。未就绪镜头会明确显示“原画预览”，导出脚本拒绝导出未通过画面检查的素材。
> 
> ## 检查、导出与打包
> 
> ```sh
> npm run render
> npm run check
> npm run package
> ```
> 
> 口型修复版为 `outputs/sakuragi-haruko-30s-lipsync-v2.mp4`（与 `outputs/sakuragi-haruko-30s.mp4`、`outputs/sakuragi-haruko-30s-voiced.mp4` 相同），工程包为 `outputs/sakuragi-haruko-project.zip`。检查包含独立路径、锁定版本、720帧／30秒／24fps、H.264／AAC、全片解码、原速选片与六条驱动音频的逐采样时点。打包检查ZIP CRC和包内成片SHA-256。包内包含本片源码、素材、原始视频候选、成片、提示词和制作记录；不打包可重新安装的 node_modules、.venv、模型和缓存。
> 
> ## 修改素材、对白、声音与时间轴
> 
> - `public/art/`：完整同框与匹配机位的原画。`docs/image-prompts.json` 与 `docs/prompts/` 保存内置图像工具提示词；`docs/image-assets.json` 记录来源和哈希。
> - `public/video/`：连续生成候选与经过审核的原速选片。修改选片范围后更新 `docs/edit-decisions.json`，完成画面检查并标记 approved，再运行 `.venv/bin/python scripts/select-clips.py 镜头名`。
> - `src/Root.tsx`：八个独立可编辑叙事段及其时长，合计720帧。害羞段的两个组件可以接入同一连续源片段；收尾段接晴子微笑近景。每个分段另有单独的Composition。
> - `src/Opening.tsx` 等场景文件：中文对白与出现时间。字幕来自本地 `public/fonts/NotoSansSC.ttf`，许可见 `docs/NOTO-OFL.txt`。
> - `scripts/make-audio.py`：原创12小节、96BPM电钢琴配乐与环境音。`docs/audio-events.json` 保存运球和拍手在全片中的接触时刻。修改后运行 `.venv/bin/python scripts/make-audio.py`。配音参数与对白在 `docs/voiceover/dialogue.json`，逐句声音在 `public/audio/voices/`，六个场景内各有独立Audio节点。对白期间配乐自动压低，处理入口为 `scripts/prepare-dialogue.py`。
> - `src/media-status.ts`：选片脚本自动更新的素材就绪状态。
> 
> ## 重新生成连续动作
> 
> 普通预览、导出和打包直接使用已有素材，不需要重新运行模型。重新生成需要 Apple Silicon Mac、Python 3.13、uv、git与约30GB模型空间。模型与Python环境都独立在本目录。
> 
> ```sh
> python3 scripts/setup-generator.py
> .venv/bin/python scripts/fetch-video-model.py
> .venv/bin/python scripts/generate-clip.py gather --suffix=-v2
> .venv/bin/python scripts/review-clip.py gather-native24-v2 --contact 225 220 280 260
> ```
> 
> 先核对逐帧图和正常速度播放，再修改选片记录、重新选择、导出与验收。每次只运行一个GPU推理进程。批量生成入口是 `scripts/generate-queue.py`，只创建候选和检查帧，不自动批准画面质量。
> 
> 固定代码提交、模型修订和依赖见 `docs/local-generator-commit.txt`、`docs/local-models.json`、`requirements-local-video.txt` 与 `package-lock.json`。模型重新下载读取固定修订，不跟随最新版本。本片复用的工具、模型和字体均为当前目录的独立副本。
> 
> ## 重新制作国语角色配音
> 
> 直接预览和导出使用已有WAV，不需要语音模型。重新合成使用本目录独立的`.venv-tts`，模型修订固定在`docs/voiceover/model-info.json`，依赖固定在`requirements-tts.txt`。
> 
> ```sh
> python3 scripts/setup-tts.py
> .venv-tts/bin/python scripts/fetch-tts-model.py
> .venv-tts/bin/python scripts/synthesize-dialogue.py --take=v2
> # 核对候选后，在 docs/voiceover/selected-takes.json 选择对应版本
> .venv-tts/bin/python scripts/prepare-dialogue.py
> python3 scripts/mux-voiceover.py
> npm run check
> npm run package
> ```
> 
> `prepare-dialogue.py`处理头尾静音、逐句响度、轻微球馆反射和配乐压低；当前六句都使用1倍语速。`mux-voiceover.py`复制既有视频流，只替换音轨；需要重新编辑画面时照常运行`npm run render`。
> 
> 先前无角色配音的版本，以及口型修复前的配音版MP4与ZIP，在制作目录的`outputs/versions/`中保留。语音模型按文字设计音色，没有使用原版动画录音或原演员样本。当前对白镜头使用实际配音驱动画面，检查说话起止、开闭口与表情；不把技术检查称为逐音素精确匹配原版动画。
> 
> ## 修改对白后重新驱动口型
> 
> 更换配音后，使用新的输入目录和候选版本，保留旧输入与旧候选。示例只演示“天才”近景；其他对白按各自时长和提示词操作。
> 
> ```sh
> .venv/bin/python scripts/prepare-lipsync-inputs.py --inputs-dir public/lipsync/inputs/v3
> cp docs/lipsync/resolve-v1-prompt.txt docs/lipsync/resolve-v3-prompt.txt
> # 修改提示词中的对白和表演方向
> .venv/bin/python scripts/generate-lipsync.py resolve --take v3 --inputs-dir public/lipsync/inputs/v3 --frames 97
> .venv/bin/python scripts/review-lipsync.py resolve --take v3
> .venv/bin/python scripts/preview-lipsync.py resolve --take v3
> ```
> 
> 完成逐帧检查与1倍速播放，在`outputs/checks/lipsync/visual-decisions.json`记录结果后，运行`.venv/bin/python scripts/adopt-lipsync.py resolve --take v3`选入工程。生成脚本不会自动批准画面。当前选用版本在`docs/lipsync/selected-takes.json`。
> 
> 修改时间轴时，同步修改`src/Root.tsx`、场景中的`Audio from`与字幕时间，以及`scripts/prepare-lipsync-inputs.py`中的`layout`，再制作新的输入与候选。改变拍手动作时也须重新标记`docs/audio-events.json`，生成配乐音效并运行`prepare-dialogue.py`，最后导出和检查。推理一次只运行一个GPU任务。

> [!info]- 来源文档：videos/sakuragi-haruko-after-school-30s-20261110/docs/production-method.md
> # 樱木花道与晴子 · 本片制作记录
> 
> 状态：MP4已完成逐段画面复核、正式导出、完整1倍速播放与技术检查。工程包检查另见交付报告。
> 
> ## 使用的路线
> 
> 用户指定先Review前作的制作复盘，再使用同样技术做30秒短片。Review结论见 method-review.md（项目文件：`videos/sakuragi-haruko-after-school-30s-20261110/docs/method-review.md`）。本片使用内置imagegen生成统一完整同框原画与匹配机位，本地LTX-2.3 q4／MLX生成连续表演，原速选片后由Remotion合成。
> 
> 原漫画前期造型与湘北旧体育馆是视觉基准；故事和中文对白为原创同人情境。原画不是官方扫描图，不能把生成结果描述为完全等同官方原画。
> 
> ## 固定版本与环境
> 
> - `dgrauet/ltx-2-mlx`：`747b30ef10e92a011e85b2aa0f6c4ee86ced5218`。
> - `dgrauet/ltx-2.3-mlx-q4`：`f306259444116d71927188af2fbbf7fdc1a8089a`。
> - `mlx-community/gemma-3-12b-it-4bit`：`86cc6a8dedbc456dd0e4af01a9d09f396f77e558`。
> - Remotion `4.0.541`、React `19.3.1`、TypeScript `5.9.3`；精确安装版本见锁文件和requirements。
> - 量化权重，默认float32计算；没有启用前作曾失败的float16试跑路线。
> - 使用Gemma和连接器顺序加载、提示词NPZ缓存、视频变换器逐块读取。MLX额度11GiB、缓存额度0；一次一个GPU推理进程。
> - 模型通过APFS文件克隆成为本目录独立文件，非硬链接与符号链接；重新建立本片.venv，npm独立安装。模型清单尺寸、独立inode和提交检查见 `outputs/checks/environment-isolation.json`。
> - 提示词、图像输入哈希、seed、帧数、规格和实际耗时保存在 `outputs/checks/generation-*.json`。静态模型重新下载脚本读取固定修订。
> 
> ## 原画与镜头
> 
> 完整同框与匹配机位的原画统一红色飞机头、湘北10号球衣、晴子的长发校服和左侧下午光。首幅是同框身份及场景基准，晴子近景另作拍手机位的参考。图像由内置工具生成后立即移入本目录；原始工具路径与哈希保留在 `image-assets.json`，当前项目不依赖外部生成目录。没有从现成动漫视频截取片段。
> 
> 每个连续片段围绕一个动作，人物保持各自站位和明确间距。原生规格1024×576、24fps，生成73或97帧，放大到1280×720合成。没有光流补帧、倒放、冻结补时和减速。每个可编辑镜头都有独立JSX节点、组件与Composition。
> 
> ## 已发现问题与处理
> 
> | 候选 | 发现 | 处理 |
> |---|---|---|
> | practice-v1 | 中段出现两个篮球，持球峰值手型和落地节奏不可靠 | 拒绝整段；重画空手基本站姿机位，制作practice-v2，不通过裁切掩盖核心问题 |
> | shy-v1 | 后段右边出现额外队员，转头帧有核心重影 | 固定近景完整排除边缘角色；舍弃源52–61帧，0–51与62–95两个原速切镜采用不同取景，重新检查交接 |
> | resolve-v1 | 右侧出现额外队员 | 采用640×360上身近景完整排除边缘角色，逐帧检查握拳动作与脸；球保持画外，不用于可见接触动作 |
> | ending-v1 | 后段人物靠近，并出现身体与手球接触问题 | 只采用干净的0–59帧；补做晴子近景表情epilogue，使用新的连续原速素材补齐1.5秒 |
> | gather-v1 | 提示只要求收球准备，但约2.7秒后自行开始出手，末尾运动模糊 | 逐帧检查全部手球接触后只采用0–59帧，2.5秒原速；排除后段。相应调整其他原速镜头的时长，保持总长30秒 |
> 
> 采用范围及修正都已逐段记录；生成成功与画面通过分开标记。最终完整成片播放和技术检查另行记录。
> 
> ## 声音
> 
> 原始生成音轨保存在候选中，当前剪辑静音这些音轨。使用本片Python脚本合成原创12小节、96BPM电钢琴轻配乐、球馆空气声及按画面对齐的运球和拍手声。初版对白以字幕呈现；现已按用户选择加入经典国语风格AI角色配音，详细过程见角色配音记录（项目文件：`videos/sakuragi-haruko-after-school-30s-20261110/docs/voiceover/method.md`）。配乐没有使用原曲录音。事件位置见 `audio-events.json`，最终响度与技术解码分别记录。
> 
> ## 时间轴
> 
> | 时间（秒） | 镜头 | 帧数 |
> |---|---|---:|
> | 0.000–4.000 | 同框鼓励 | 96 |
> | 4.000–8.000 | 晴子的建议 | 96 |
> | 8.000–10.167 | 樱木挠头 | 52 |
> | 10.167–11.583 | 害羞微笑 | 34 |
> | 11.583–15.583 | 空手基本站姿 | 96 |
> | 15.583–18.083 | 收球准备 | 60 |
> | 18.083–22.000 | 晴子拍手 | 94 |
> | 22.000–26.000 | 恢复自信 | 96 |
> | 26.000–28.500 | 同框回应 | 60 |
> | 28.500–30.000 | 晴子的微笑 | 36 |
> 
> ## 验收与交付
> 
> 画面验收见 `visual-review.md`；技术检查见 `outputs/checks/verification.json`；包检查见 `outputs/checks/package-report.json`。最终以实际已生成报告为准。
> 
> 源码、原画、候选、模型、环境、本地可配置的依赖缓存、检查帧、日志、音频、MP4和ZIP都位于本视频目录。内置图像工具的输出目录无法通过调用参数配置，已返回的PNG均立即移入本目录；不宣称其服务内部日志或缓存的路径已受工程控制。工程包只包含当前工程文件；可安装依赖及可下载模型由本片锁文件与脚本重建。
> 
> ## 实际耗时与限制
> 
> 本片共生成10个连续动画候选（含被拒绝的运球版），累计推理66.04分钟，逐项实际耗时见generation-timings.md（项目文件：`videos/sakuragi-haruko-after-school-30s-20261110/docs/generation-timings.md`）。该数字包含文本编码、去噪、解码与封装，不包含原画生成、安装、画面检查、剪辑与最终导出；这些步骤部分重叠执行。
> 
> 原速选片包含10个独立切镜、8个叙事段，合计720帧。没有光流补帧、冻结补时、倒放循环或放慢源片段。原画与连续动画均为AI制作的原创同人情境；远景脸、运动细线与球衣文字仍可软化，固定seed不能保证逐帧等同官方原画。包含画外角色的提示在本片两次引入了额外队员，未来应优先用不命名画外人物的单人表演提示，并继续按实际画面复核；此经验不当作对所有模型或设备的结论。
> 
> 最终MP4为H.264／AAC、1280×720、24fps、720帧、30秒，完整音视频解码通过；浏览器已1倍速播放到30.0秒结束。当前国语版归一化滤镜报告输出综合响度为-18.10 LUFS，视频流复制保留。检查与哈希见outputs/checks/verification.json、audio-normalization.json和final-playback.json。
> 
> ## 国语配音更新
> 
> 在相同30秒画面上加入6句国语角色对白。使用独立Qwen3-TTS VoiceDesign环境与固定模型修订，具体音色和表演提示在voiceover/dialogue.json。声音按经典国语动画的角色气质设计，未使用原演员录音作为参考，也不称为原声复刻。对白保持自然语速，逐句放入字幕窗口，配乐在对白时降低；画面数据逐包SHA-256与上一版完全一致。转写核对涵盖逐句WAV和实际导出AAC混音，结果及范围见outputs/checks/voiceover/。
> 
> 
> ## 国语口型与表情修复（2026-11-10）
> 
> 触发原因是用户指出配音与人物表情、尤其嘴型不匹配。复盘结论：可见角色对白应先确定声音与情绪，再用实际音轨驱动嘴部和整脸表演，并分别进行视觉和技术检查。用户反馈原句、失败候选、修复细节和后续准则统一保存在口型修复记录（项目文件：`videos/sakuragi-haruko-after-school-30s-20261110/docs/lipsync/method.md`）。
> 
> 在首次配音版之后，六处讲话镜头改为LTX-2.3 q4 / MLX `a2v --distilled`，两阶段固定实际对白音轨，重新生成连续的嘴部、眉眼和头部表演。生成7个候选，采用6个；害羞首版因提前张嘴及转头眼睛变形被拒，追加闭口guide后重做。生成累计约47.48分钟，独立记录与复现入口见口型修复记录（项目文件：`videos/sakuragi-haruko-after-school-30s-20261110/docs/lipsync/method.md`）。
> 
> 晴子赞许改为一次轻拍，音效校准到全片第445帧。角色对白WAV与上一版逐采样相同。实际新成片AAC的六句配音落点与驱动音频一致，另完成脸部/手部逐帧检查、全片1倍速播放、720帧/30秒/24fps检查、独立路径和全片解码。当前画面流已改变；首次配音时的视频流不变记录仅适用于保留的旧版。

> [!info]- 来源文档：videos/sakuragi-haruko-after-school-30s-20261110/docs/method-review.md
> # 对既有制作记录的Review
> 
> 已完整阅读用户指定的production-method.md。该记录有可执行的固定版本、内存策略、动作验收、原速剪辑与独立工程规则，可以复用。
> 
> 1. 核心可复用的是整幅场景原画、连续生成、短动作验证、原速选帧、Remotion合成和视觉/技术分开验收。
> 2. 不直接复用旧片的镜头时长、篮球坐标、遮罩、音效事件、角色提示词与20秒检查参数。新故事重建。
> 3. 原脚本fetch-video-model.py会查询最新修订，与文档固定修订原则不一致；本片复制明确版本的模型，并让重新下载脚本读取固定manifest。
> 4. 锁文件应配合新环境安装；禁止复制旧.venv和带旧绝对路径的可编辑依赖。
> 5. 模型版本不是身份一致性的保证。晴子与樱木的前期外形先在同框原画统一，视频逐段检查脸、发型、衣着、手球接触。
> 6. 同机生成每4秒约7分钟，可据此估算，但最终耗时以本片日志为准。
> 7. 本片没有入网硬性叙事需求，避免借用旧篮筐校准。对单球数量和握持仍逐帧检查。
> 
> 旧文档的原速与自然动作要求将作为硬约束。

> [!info]- 来源文档：videos/sakuragi-haruko-after-school-30s-20261110/docs/generation-timings.md
> # 本片连续动画实际生成记录
> 
> | 候选 | 帧数 | 尺寸 | 实际分钟 | 结果 |
> |---|---:|---|---:|---|
> | cheer-v1 | 97 | 1024×576 | 7.06 | 完成生成，是否采用另见选片记录 |
> | ending-v1 | 97 | 1024×576 | 7.02 | 完成生成，是否采用另见选片记录 |
> | epilogue-v1 | 49 | 1024×576 | 3.74 | 完成生成，是否采用另见选片记录 |
> | gather-v1 | 73 | 1024×576 | 6.23 | 完成生成，是否采用另见选片记录 |
> | haruko-v1 | 97 | 1024×576 | 6.76 | 完成生成，是否采用另见选片记录 |
> | opening-v1 | 97 | 1024×576 | 7.18 | 完成生成，是否采用另见选片记录 |
> | practice-v1 | 97 | 1024×576 | 7.24 | 完成生成，是否采用另见选片记录 |
> | practice-v2 | 97 | 1024×576 | 7.06 | 完成生成，是否采用另见选片记录 |
> | resolve-v1 | 97 | 1024×576 | 6.89 | 完成生成，是否采用另见选片记录 |
> | shy-v1 | 97 | 1024×576 | 6.86 | 完成生成，是否采用另见选片记录 |
> 
> 累计推理时间：66.04分钟。
> 
> Includes prompt encoding, denoising, decoding and muxing. Excludes image generation, setup, visual QA and editing. MLX peak is not total process or system memory.

> [!info]- 来源文档：videos/sakuragi-haruko-after-school-30s-20261110/docs/lipsync/method.md
> # 口型与表情修复记录
> 
> ## 问题定位
> 
> 前一版先生成无对白画面，后按字幕时点加入六句国语配音。声音在字幕窗口内、音视频时长正确，并不能证明嘴型匹配。特别是樱木近景，原素材的闭嘴微笑与持续对白冲突。此次修复需要检查可见的嘴部表演、眉眼情绪与对白起止。
> 
> ### 用户反馈与当前状态
> 
> 2026-11-10，用户指出：“配音和人物的表情，特别是嘴型太不匹配了。”此前用户已选择“经典国语配音的感觉”。本轮问题是角色可见表演与实际对白不匹配，需要同时处理开闭口、眉眼情绪和头部动作。
> 
> 修复版已制作并交付，以下“已选入”“检查通过”指本轮制作中的选片与检查结果。用户随后要求保存问题和修复经验，尚未明确反馈新版自然度是否达到预期，不把记录请求视为用户验收通过。
> 
> ## 本轮试验
> 
> 先验证“我可是天才嘛”四秒近景。使用当前工程独立的 LTX-2.3 q4 模型、Gemma 与固定版本 MLX 代码。输入为已采用镜头的首帧，以及按当前对白时点补足头尾静音的独立角色 WAV，不含配乐。使用 `a2v --distilled`，两轮生成都固定输入音频 latent，重新生成整幅场景连续表演；不是在旧视频上简单附加音轨，也不是根据音量贴一个开合的嘴。
> 
> 生成入口：`scripts/prepare-lipsync-inputs.py`、`scripts/generate-lipsync.py`。逐帧审核入口：`scripts/review-lipsync.py`；同音轨、同语速对比：`scripts/preview-lipsync.py`。候选和提示词均保留版本，不自动批准，也不自动替换成片。
> 
> ```sh
> .venv/bin/python scripts/prepare-lipsync-inputs.py haruko
> .venv/bin/python scripts/generate-lipsync.py resolve --take v1
> .venv/bin/python scripts/review-lipsync.py resolve --take v1
> .venv/bin/python scripts/preview-lipsync.py resolve --take v1
> ```
> 
> 单个 GPU 推理进程；缓存、输入、候选、日志和审核图均在本视频目录内。旧版配音成片与工程包已备份到 `outputs/versions/*before-lipsync*`。
> 
> ## 质量边界
> 
> 该 MLX 的 distilled A2V 功能标为实验性，不能预先保证中文每个音素精确匹配，也可能改变非嘴部动作。必须审核对白前后的开闭口、脸部和手部稳定、人物身份以及 1 倍速整段表现。能量条仅辅助对照声音起止，不构成音素级同步评分。原来的语音识别文字检查只检查对白内容，也不构成口型验收。
> 
> 参考：[MLX A2V 流程](https://github.com/dgrauet/ltx-2-mlx/blob/747b30ef10e92a011e85b2aa0f6c4ee86ced5218/docs/PIPELINES.md)、[Lightricks 音频驱动视频](https://ltx.io/blog/how-to-generate-video-from-audio)。也调查了 [Dub-It](https://docs.ltx.io/open-source-model/feature-guides/audio/dub-it-beta)：它会重生成对白，官方列出的已验证语言未包括中文，因此不将其等同于保持现有国语录音的精确对嘴工具。
> 
> 
> 
> ## 本轮结果（2026-11-10）
> 
> 六处讲话镜头均重新生成并选入，使用既有的同一批国语对白。`shy-v1`未采用；`shy-v2`使用干净的闭口图在第17和81帧作为额外guide，主要讲话动作约从第20帧开始，接近音频活动的第19帧，后段保持稳定侧脸。最终画面仍是连续生成的视频，不是静帧轮换。
> 
> | 候选 | 原生帧数 | 生成分钟 | 结果 |
> |---|---:|---:|---|
> | cheer-v1 | 97 | 7.17 | 已选入 |
> | ending-v1 | 65 | 4.84 | 已选入 |
> | haruko-v1 | 97 | 6.79 | 已选入 |
> | opening-v1 | 97 | 7.35 | 已选入 |
> | resolve-v1 | 97 | 7.45 | 已选入 |
> | shy-v1 | 89 | 6.28 | 未采用：提前张嘴及转头眼睛变形 |
> | shy-v2 | 89 | 7.60 | 已选入 |
> 
> 七次生成共 47.48 分钟GPU流水线墙钟时间，包含一个未采用候选；逐帧审核、合成、打包时间另计。模型、代码提交和精度沿用本片固定环境。
> 
> 选用：开场0–95、晴子建议0–95、害羞0–85（组件边界52，前后连续）、赞许0–93、自夸0–95、收尾0–59。全片720帧、24fps、30秒。没有调慢、倒放、重复或冻结补帧。
> 
> 新版赞许镜头只有一次轻拍：第11源帧手掌接触，对应全片第445帧（18.5417秒）。原四个拍手音效已改为这一处接触，六条对白WAV逐采样保持一致。
> 
> ## 最终检查与交付
> 
> 所有输入音频与工程对白的帧/采样偏移一致。实际导出AAC与各条对白的能量包络相关性为0.979–0.999；在4.17毫秒分析分辨率下，六句均未检出整体偏移。这测量混音时间位置，不测量每个音素与嘴形的匹配。脚本为 `scripts/check-export-dialogue-timing.py`，已纳入 `npm run check`。
> 
> 最终导出逐帧抽取了镜头边界、连续害羞段交界与关键表演画面，结合各源片的逐帧脸部/手部检查；成片在浏览器以1倍速、不静音播放至30秒，媒体错误为空。技术检查另外核验帧数、时长、锁定依赖、独立路径、三个交付文件一致性与音视频全片解码。
> 
> 本工具环境不能直接听辨音频，因此不把播放完成和包络相关性称为人工听感或原演员相似度验收。口型属于AI生成的近似动漫表演，部分预备微笑会露齿，宽景脸部和少量手指线条较软；没有声称达到原动画的逐音素人工对嘴。
> 
> 成片：`outputs/sakuragi-haruko-30s-lipsync-v2.mp4`；浏览器页：`outputs/review-lipsync.html`；SHA-256：`f47930db9afe20734901596509702800034f44380b69fba0ee25d398839ed9cd`。候选、提示词、两张追加参考、逐帧图、真实导出音轨时点与检查JSON均在本片目录。
> 
> ## 后续制作采用的准则
> 
> 1. 对画面中可见的角色对白，先确定对白文本、声线、情绪与正常语速，再用实际角色音轨驱动连续画面。字幕与声音的时点对齐不构成嘴型匹配。
> 2. 先做2至4秒可见嘴部的讲话近景，检查说话前、句中停顿和说话后的开闭口，以及眉眼、视线和情绪。此步骤属于内部质量检查，不增加逐镜头向用户确认的流程。
> 3. 驱动输入使用不含配乐的角色音轨，按实际时间轴补足头尾静音；最终合成使用同一批对白。更换配音或改变语句节奏后，重新生成相应讲话镜头。
> 4. 明确双人镜头中谁说话、谁倾听。检查听者是否意外张嘴，同时检查人物身份、脸部、手势、握持和站位。核心变形应重做；本次闭口guide修复有效，但仍须逐帧核对每次结果。
> 5. 重生成动作后，按新的接触帧重新校准音效。本次四次拍手变为一次轻拍，音效随画面改到全片445帧。
> 6. 分开记录视觉检查与技术检查。音轨包络相关、ASR、总时长和解码只能证明各自检查范围；记录检查分辨率、工具的听辨限制及口型近似程度。保留原版、失败候选、提示词、参考帧、选片范围与实际耗时。

> [!info]- 来源文档：videos/sakuragi-haruko-after-school-30s-20261110/docs/voiceover/method.md
> # 经典国语风格角色配音
> 
> 本页记录首次在既有画面上添加配音的阶段。随后已用同一批对白重新生成讲话镜头，当前成片的画面流已改变，见口型与表情修复记录（项目文件：`videos/sakuragi-haruko-after-school-30s-20261110/docs/lipsync/method.md`）。下文的“视频流保留”与首次成片ASR属于旧版记录，不作为本次口型验收依据。
> 
> 用户选择经典国语配音的感觉。在现有30秒画面上给6句字幕添加中文角色对白。樱木：年轻中低男声，略沙哑、热血自信，面对晴子有害羞感；晴子：清亮温柔、开朗鼓励的少女声。
> 
> 采用本地Qwen3-TTS VoiceDesign与MLX Audio，按照文字描述设计角色声音。没有使用动画原版录音或原演员的音频样本，因此不把结果称为原声复刻。模型、环境、缓存、原始候选、对白和处理结果全部位于本视频目录；保留了上一版无对白MP4与工程包。
> 
> - 模型和修订：`model-info.json`，生成脚本只读本地固定修订。
> - 依赖：独立`.venv-tts/`与`requirements-tts.txt`，不改动原视频推理环境。
> - 角色声音、表演提示、中文对白与帧时点：`dialogue.json`。
> - 采用的候选版本：`selected-takes.json`。
> - 原始音频：`public/audio/voices/raw/`。
> - 逐句生成元数据：`outputs/checks/voiceover/generation-*.json`。
> 
> 技术资料：[MLX Audio语音模型说明](https://github.com/Blaizzy/mlx-audio/blob/main/mlx_audio/tts/models/qwen3_tts/README.md)、[模型卡](https://huggingface.co/mlx-community/Qwen3-TTS-12Hz-1.7B-VoiceDesign-bf16)。
> 
> 配音完成后核对文字、时长、声音层次和字幕时点，检查完整30秒声画与解码。画面采用现有已验收视频流，配音不改变人物动作或总时长。
> 
> ## 本轮实际结果
> 
> 六句原始候选均成功生成，分别为2.08、1.28、2.48、1.20、2.16、1.20秒。去除头尾静音并加入轻微空间反射后仍全部以1倍语速落在字幕窗口内，没有为了配音修改画面节奏。每句音频、字幕和配乐层均可在Remotion中独立调整。
> 
> 采用Qwen3-ASR对逐句音频和最终AAC混音做无提示词转写。六句内容均可识别；人名晴子在识别结果中写作青子，原字幕仍使用晴子。最终混音转写在樱木回应前额外给出“呵呵”，这是识别结果，未将其当作已经主观听辨的笑声。ASR仅核对可识别内容，不用来证明声音与原演员一致。当前执行环境不能直接接收音频作主观听感判断，音色相似度交由用户试听；不将转写或技术通过写成原声相似度验收。
> 
> 完整配音不重做口型，原动作与口型保持。视频流SHA-256前后相同，见outputs/checks/voiceover/video-stream-preserved.json。原版无对白文件在outputs/versions/保留。首次ASR模型Xet下载未推进，改为普通HTTPS完成；两种下载与生成的缓存均设置在本视频目录。
> 
> 配音版已完整播放到30.0秒，浏览器声音未静音且无媒体错误；720帧、24fps、H.264／AAC、全片解码、逐句自然语速时间窗和命名副本一致性检查通过。画面逐包哈希保持相同。

> [!info]- 来源文档：videos/sakuragi-haruko-after-school-30s-20261110/docs/visual-review.md
> # 画面验收记录
> 
> 状态：已完成逐段画面复核、导出切镜复核与最终成片1倍速完整播放。技术检查另存，未以可解码或区域变化替代画面检查。
> 
> ## gather-native24-v1
> 
> - 原始73帧、24fps、3.041667秒。已提取所有帧；全幅每3帧概览与手球区域逐帧图在 `outputs/checks/gather-native24-v1/`。
> - 已在浏览器以1倍速播放原始短片到3.041667秒结束。
> - 前段双手持同一篮球、双臂连接和两人分离可用，晴子留在独立站位。
> - 约第63帧开始转入额外出手，末尾有明显运动模糊，不采用。
> - 采用第0–59帧，2.5秒、原速。此段没有出手或进球叙事，不配入网声音。
> - 球衣文字在运动中有轻微软化，保留原始候选作为限制记录。
> 
> 后续各镜头逐项核对人物身份、发型、衣着、手、重心、单球数量、人物分离、动作节奏与交接。最终成片仍需1倍速完整播放。
> 
> ## practice-native24-v1（不采用）
> 
> 全幅概览和关键帧显示约第36帧出现两个篮球；球在高位时右掌的接触方向也不可靠。该段没有获批，选片脚本不会将它纳入成片。按故事目标重画了无球基本站姿的04-stance-v2.png，连续表演仍待生成与验收。
> 
> ## opening-native24-v1
> 
> 全幅每3帧概览与97帧手球接触图已查看，浏览器1倍速播放至4.041667秒结束。单球握持、双臂、站位间距与双人视线可用；后段晴子小步移动但没有交叠。采用0–95帧，4秒原速。运动中的球衣英文有轻微软化，远景小脸不等同于逐张原作画稿。
> 
> ## haruko-native24-v1
> 
> 全幅每3帧概览、第42／66／95帧原尺寸与1倍速完整播放已检查。脸型、刘海、校服与双腕连接可用。实际增加了展开手掌的解释动作和转头，未完全遵循仅小幅点头的提示，但适合指导站姿的对白；保留该表演。采用0–95帧，4秒原速。
> 
> ## shy-native24-v1：取景与剪辑修正后采用
> 
> 原始97帧中，右侧约2.2秒后出现额外黑发队员；转头附近第52–61帧有明显重影。先尝试完整640×360近景取景，虽然排除了额外角色，但重影仍处于核心脸手动作，因此未采用整段。
> 
> 最终采用两个独立原速镜头：源0–51帧，以640×360、x=80、y=0固定取景；源62–95帧，以576×324、x=140、y=0的稍紧脸部取景。两个取景均完整排除右侧额外人物，保留发型、脸与下巴；中间重影帧全部舍弃。已查看取景概览、关键原尺寸帧及原速连续播放至3.583333秒结束。镜头之间是明确切镜，没有插帧或延长。
> 
> 修正候选保存在public/video/shy-repair-review.mp4，原始视频和最初取景尝试均保留。最终范围以edit-decisions.json为准。
> 
> ## cheer-native24-v1
> 
> 全幅概览、97帧手部图、前36帧放大接触图及1倍速完整播放已检查。实际有四次小幅拍手，接触约在源第5／14／23／32帧，随后双手收拢并自然放下。手腕连接与脸型可用，放下手时有短暂运动软化；没有新增人物或重复手臂。采用0–93帧，94帧原速。对应全片拍手声第439／448／457／466帧。
> 
> ## resolve-native24-v1：固定近景后采用
> 
> 原始镜头右侧再次出现额外队员，不直接采用原始全幅。使用640×360、x=80、y=0固定上身取景，完整排除额外角色，保留拳头抬起、放下与完整脸型。已查看全部96帧取景图、概览，并以1倍速播放修正版至4.0秒结束；没有新增肢体、相互接触或第二个球。篮球在此反应近景的下方画外，未把画外球用于可见关键动作。采用源0–95帧，4秒原速。
> 
> ## ending-native24-v1：只采用前2.5秒
> 
> 后段两人开始靠近，出现不合适的身体与手球接触，不采用。源0–59帧已检查全幅概览、全部手球区域图和1倍速2.5秒播放，双人仍保持清楚分离，只有单个固定持球。保留该范围；新增独立的晴子近景1.5秒作为最终回应，不以拉长或冻结填补时间。
> 
> ## practice-native24-v2：重做后采用
> 
> 以新的空手基本站姿完整原画生成。全幅概览、全部97帧膝盖与脚底区域、正常速度播放均已检查。樱木以站稳为基础做屈膝与小幅起伏，手臂与双腿独立，鞋底接触保持；晴子后段有小步移动但没有穿插与接触。没有篮球或新人物。采用源0–95帧，4秒原速。
> 
> ## epilogue-native24-v1
> 
> 新增清楚的近景原画后生成49帧。全幅概览和1倍速2.041667秒播放已检查。脸型、刘海、完整下巴和肩膀稳定，轻微点头与眨眼可用，没有手臂或额外人物进入。采用0–35帧，1.5秒原速。
> 
> ## 最终30秒成片
> 
> 实际导出的MP4已抽查全部切镜边界、四个拍手接触点和字幕／收尾取样，共33帧，见outputs/checks/final-frames/overview.jpg。无原画预览标签、额外队员、重复篮球或失败运球段进入成片。两个樱木近景取景均排除右侧多余角色；身体接触后段已排除。
> 
> 浏览器以1倍速从头播放至30.0秒结束，观察到rate=1、ended=true、readyState=4、error=null。全片720帧，镜头交接、字幕安全区、拍手事件与结尾已复核。播放记录见outputs/checks/final-playback.json。画面中的细线、远景小脸和运动球衣文字仍可能软化，不宣称与官方手绘动画逐帧相同。
> 
> ## 国语配音更新的播放检查
> 
> 配音版MP4以1倍速完整播放至30.0秒，浏览器muted=false、ended=true、error=null。视频压缩数据逐包哈希与原版完全相同，因此上述画面与切镜检查继续适用。6句对白均按字幕时点放置，全部自然语速，声音与字幕窗口检查见voiceover/timing-verification.json，最终AAC混音转写见voiceover/final-mix-transcript.json。自动转写仅验证文字可识别，不等同于主观音色或原演员相似度验收。
> 
> 
> ## 配音驱动画面的修复版
> 
> 六处对白已更换为实际音轨驱动的连续生成。逐帧脸部与手部检查见 `outputs/checks/lipsync/`，选用与拒绝原因见 `visual-decisions.json`。害羞v1未采用；v2的主讲话动作接近声音起点，后段侧脸保持稳定。开场与收尾分别核对讲话者和倾听者，保持单个篮球、握持与人物分离。晴子赞许只出现一次轻拍，第11源帧接触，音效改到全片445帧。
> 
> 实际导出边界图已更新，害羞段52帧组件边界前后来自同一连续源片，没有跳切或错帧。30秒成片已在浏览器按1倍速播放至结束，无媒体错误。检查不构成逐音素准确度或原演员听感认证；宽景细线与个别手指线条仍有软化。

> [!info]- 来源文档：videos/sakuragi-haruko-after-school-30s-20261110/docs/references.md
> # 人物与场景依据
> 
> 本片用户要求尽量保持原漫画的人物和场景，因此锁定前期造型，不混用电影版与后期寸头樱木。服装、头发、脸型和球馆以新生成的同框原画为跨镜头基准。没有使用现成动漫视频片段。
> 
> 官方资料检索于2026-11-10：
> 
> - [东映SLAM DUNK DVD资料](https://www.toei-anim.co.jp/dvd/slamdunk/)：核对晴子鼓励初学者樱木接触篮球的关系背景。
> - [集英社SLAM DUNK 1试读入口](https://www.shueisha.co.jp/books/reader/main.php?cid=08X10000000062717100)：原漫画入口。
> - [东映角色资料入口](https://lineup.toei-anim.co.jp/ja/movie/movie_slumdumk_1994/character/)：角色名参考。
> 
> 检索取得文字与索引；部分官方网站完整页面返回不可访问或403。本片未下载并使用其原画作为图像输入，也不将未读取的原作页标作已验证分镜。实际生成图像的唯一跨镜头视觉输入是本片自己生成的01-two-shot.png及02-haruko.png；全部提示词和素材位置另有记录。
> 
> 故事与对白为原创同人情境，不宣称是漫画现有章节或官方片段。生成模型仍可能使远景脸、衣服字样和细线轻微软化；实际采用范围逐镜头验收。

## 证据清单与缺失信息

盘点的文本/报告文件 186 个，源文本、报告及媒体产物身份摘要为 `4979832c7734cb57a9dfeee5c91e6b7ab12321dc21f03642aadf5d5bb7a53085`。哈希证明采集内容身份，不代表其中每个断言已验证。模型权重、缓存、依赖、第三方源码树和未指定的历史会话未采集；大型文本仅做哈希并按需要选读。

- 本次整理基于明确引用的 README、制作方法、配置、提示词、关键源码与已有报告。未重新观看全部参考/候选/成片，也未做人工听辨。
- 项目未提供的原始模型内部实现、未披露生成后端、未记录 seed/参数、真实费用、人工总工时和用户最终验收，均不补造。实际日志有记录的耗时见上文，流水线耗时不等于整个制作总耗时。
- 源项目内的路径只用于查证；本档案未复制大型素材或模型，也不改变源项目运行依赖。

| 已盘点证据文件 | SHA-256（前 16 位）/状态 |
| --- | --- |
| `videos/sakuragi-haruko-after-school-30s-20261110/README.md` | `0e18eee3233f0af0` |
| `videos/sakuragi-haruko-after-school-30s-20261110/package-lock.json` | `da6e83823cf6577a` |
| `videos/sakuragi-haruko-after-school-30s-20261110/package.json` | `eae6b5d93511b10c` |
| `videos/sakuragi-haruko-after-school-30s-20261110/remotion.config.ts` | `89558888f253880f` |
| `videos/sakuragi-haruko-after-school-30s-20261110/requirements-local-video.txt` | `022301641961a33b` |
| `videos/sakuragi-haruko-after-school-30s-20261110/requirements-tts.txt` | `1a42a1f23876f2df` |
| `videos/sakuragi-haruko-after-school-30s-20261110/tsconfig.json` | `af262af0dd89b1e2` |
| `videos/sakuragi-haruko-after-school-30s-20261110/docs/NOTO-OFL.txt` | `1c05c68c34f97084` |
| `videos/sakuragi-haruko-after-school-30s-20261110/docs/audio-events.json` | `4cc8a221a7b16177` |
| `videos/sakuragi-haruko-after-school-30s-20261110/docs/edit-decisions.json` | `02c5b4b0bd4c5a8f` |
| `videos/sakuragi-haruko-after-school-30s-20261110/docs/failed-motion-prompts.json` | `0202f847464d73da` |
| `videos/sakuragi-haruko-after-school-30s-20261110/docs/generation-timings.md` | `f9e7c53ca2335d11` |
| `videos/sakuragi-haruko-after-school-30s-20261110/docs/image-assets.json` | `46abf361394785d5` |
| `videos/sakuragi-haruko-after-school-30s-20261110/docs/image-prompts.json` | `1573f6e575f26662` |
| `videos/sakuragi-haruko-after-school-30s-20261110/docs/local-generator-commit.txt` | `cefeb3f58443a4e6` |
| `videos/sakuragi-haruko-after-school-30s-20261110/docs/local-models.json` | `061b2acac26482fc` |
| `videos/sakuragi-haruko-after-school-30s-20261110/docs/method-review.md` | `0787eee0551f2820` |
| `videos/sakuragi-haruko-after-school-30s-20261110/docs/production-brief.md` | `5c2ee17be6b18832` |
| `videos/sakuragi-haruko-after-school-30s-20261110/docs/production-method.md` | `5a396e4fce4a171d` |
| `videos/sakuragi-haruko-after-school-30s-20261110/docs/references.md` | `2bd5f948311f7517` |
| `videos/sakuragi-haruko-after-school-30s-20261110/docs/video-prompts.json` | `cc471a1c587501d0` |
| `videos/sakuragi-haruko-after-school-30s-20261110/docs/visual-review.md` | `edc3684ec6b150c8` |
| `videos/sakuragi-haruko-after-school-30s-20261110/docs/lipsync/before-audio-events.json` | `1e0ca66afa9deb8f` |
| `videos/sakuragi-haruko-after-school-30s-20261110/docs/lipsync/before-edit-decisions.json` | `7a517db6d3d357c4` |
| `videos/sakuragi-haruko-after-school-30s-20261110/docs/lipsync/before-selections.json` | `2d25f2f303ed54fd` |
| `videos/sakuragi-haruko-after-school-30s-20261110/docs/lipsync/cheer-v1-prompt.txt` | `789bc97cc199a9a5` |
| `videos/sakuragi-haruko-after-school-30s-20261110/docs/lipsync/ending-v1-prompt.txt` | `86b57198eb03abbd` |
| `videos/sakuragi-haruko-after-school-30s-20261110/docs/lipsync/haruko-v1-prompt.txt` | `526b2a2c2ac9ed73` |
| `videos/sakuragi-haruko-after-school-30s-20261110/docs/lipsync/input-layout.json` | `7300577ecf176cb5` |
| `videos/sakuragi-haruko-after-school-30s-20261110/docs/lipsync/method.md` | `de26482ab1b34f98` |
| `videos/sakuragi-haruko-after-school-30s-20261110/docs/lipsync/opening-v1-prompt.txt` | `f4657a76bd9ff264` |
| `videos/sakuragi-haruko-after-school-30s-20261110/docs/lipsync/resolve-v1-prompt.txt` | `9fe44d9c36f1e862` |
| `videos/sakuragi-haruko-after-school-30s-20261110/docs/lipsync/selected-takes.json` | `0483fac271a30bf4` |
| `videos/sakuragi-haruko-after-school-30s-20261110/docs/lipsync/shy-v1-prompt.txt` | `9a93bcbd9935d616` |
| `videos/sakuragi-haruko-after-school-30s-20261110/docs/lipsync/shy-v2-anchors.json` | `5fbc3efeb23330cf` |
| `videos/sakuragi-haruko-after-school-30s-20261110/docs/lipsync/shy-v2-prompt.txt` | `a9c2be2900dbcd95` |
| `videos/sakuragi-haruko-after-school-30s-20261110/docs/prompts/02-haruko.txt` | `af6a1a5d5b6ab290` |
| `videos/sakuragi-haruko-after-school-30s-20261110/docs/prompts/03-shy.txt` | `3a01fb12308ca8fe` |
| `videos/sakuragi-haruko-after-school-30s-20261110/docs/prompts/04-practice.txt` | `ff6e57f8e94385f2` |
| `videos/sakuragi-haruko-after-school-30s-20261110/docs/prompts/04-stance-v2.txt` | `595530e257388884` |
| `videos/sakuragi-haruko-after-school-30s-20261110/docs/prompts/05-gather.txt` | `56ac48b0ee82561c` |
| `videos/sakuragi-haruko-after-school-30s-20261110/docs/prompts/06-cheer.txt` | `841867cd2512208c` |
| `videos/sakuragi-haruko-after-school-30s-20261110/docs/prompts/07-resolve.txt` | `ca3d179ac5858c68` |
| `videos/sakuragi-haruko-after-school-30s-20261110/docs/prompts/08-ending.txt` | `a282332e2d8894ef` |
| `videos/sakuragi-haruko-after-school-30s-20261110/docs/prompts/09-epilogue.txt` | `4de2a85f9dacac69` |
| `videos/sakuragi-haruko-after-school-30s-20261110/docs/voiceover/asr-model-info.json` | `dbda4802a63e25d5` |
| `videos/sakuragi-haruko-after-school-30s-20261110/docs/voiceover/dialogue.json` | `2a1c2d73450d301f` |
| `videos/sakuragi-haruko-after-school-30s-20261110/docs/voiceover/method.md` | `5513d13660d59dce` |
| `videos/sakuragi-haruko-after-school-30s-20261110/docs/voiceover/model-info.json` | `0aacf40c973da02b` |
| `videos/sakuragi-haruko-after-school-30s-20261110/docs/voiceover/selected-takes.json` | `c645fa6aaf168e39` |
| `videos/sakuragi-haruko-after-school-30s-20261110/outputs/checks/audio-design.json` | `03b11ec20ca51e0d` |
| `videos/sakuragi-haruko-after-school-30s-20261110/outputs/checks/audio-normalization.json` | `a7c758186193e0da` |
| `videos/sakuragi-haruko-after-school-30s-20261110/outputs/checks/environment-isolation.json` | `9fdbd69995f4674b` |
| `videos/sakuragi-haruko-after-school-30s-20261110/outputs/checks/final-playback.json` | `bd4649af64e6d2c2` |
| `videos/sakuragi-haruko-after-school-30s-20261110/outputs/checks/generation-cheer-v1.json` | `fdbe094559f6b728` |
| `videos/sakuragi-haruko-after-school-30s-20261110/outputs/checks/generation-ending-v1.json` | `9cd6b7e69239624d` |
| `videos/sakuragi-haruko-after-school-30s-20261110/outputs/checks/generation-epilogue-v1.json` | `7cad31ebe25a249b` |
| `videos/sakuragi-haruko-after-school-30s-20261110/outputs/checks/generation-gather-v1.json` | `8385860e323df426` |
| `videos/sakuragi-haruko-after-school-30s-20261110/outputs/checks/generation-haruko-v1.json` | `a02d10897f6f1fcb` |
| `videos/sakuragi-haruko-after-school-30s-20261110/outputs/checks/generation-opening-v1.json` | `4cd8f1002a499d22` |
| `videos/sakuragi-haruko-after-school-30s-20261110/outputs/checks/generation-practice-v1.json` | `59012cbb1186ac09` |
| `videos/sakuragi-haruko-after-school-30s-20261110/outputs/checks/generation-practice-v2.json` | `7f4fa87e17f50bf7` |
| `videos/sakuragi-haruko-after-school-30s-20261110/outputs/checks/generation-resolve-v1.json` | `870ab874ecb24d30` |
| `videos/sakuragi-haruko-after-school-30s-20261110/outputs/checks/generation-shy-v1.json` | `1a1a75df1370dbe0` |
| `videos/sakuragi-haruko-after-school-30s-20261110/outputs/checks/generation-summary.json` | `cfdab67ff3abf6d2` |
| `videos/sakuragi-haruko-after-school-30s-20261110/outputs/checks/image-isolation.json` | `7e8c6343e8c7d475` |
| `videos/sakuragi-haruko-after-school-30s-20261110/outputs/checks/media-metadata.json` | `4c9e39211d93388d` |
| `videos/sakuragi-haruko-after-school-30s-20261110/outputs/checks/package-report.json` | `8828be254b3e4478` |
| `videos/sakuragi-haruko-after-school-30s-20261110/outputs/checks/selections.json` | `c6302e6b585e31f4` |
| `videos/sakuragi-haruko-after-school-30s-20261110/outputs/checks/source-verification.json` | `685cdea2f45c7ca4` |
| `videos/sakuragi-haruko-after-school-30s-20261110/outputs/checks/system-tools.json` | `b5ef69dec0ed2140` |
| `videos/sakuragi-haruko-after-school-30s-20261110/outputs/checks/verification.json` | `412195eeb4f6f00a` |
| `videos/sakuragi-haruko-after-school-30s-20261110/outputs/checks/cheer-native24-v1/probe.json` | `0d6fcf831b1aac65` |
| `videos/sakuragi-haruko-after-school-30s-20261110/outputs/checks/ending-native24-v1/probe.json` | `5480a15c027d3b75` |
| `videos/sakuragi-haruko-after-school-30s-20261110/outputs/checks/ending-trim-review/probe.json` | `dd0021fcfd34eae3` |
| `videos/sakuragi-haruko-after-school-30s-20261110/outputs/checks/epilogue-native24-v1/probe.json` | `6862fcb1b5dc1421` |
| `videos/sakuragi-haruko-after-school-30s-20261110/outputs/checks/final-frames/frames.json` | `38af150a2231ddcf` |
| `videos/sakuragi-haruko-after-school-30s-20261110/outputs/checks/gather-native24-v1/probe.json` | `8676503df8da3116` |
| `videos/sakuragi-haruko-after-school-30s-20261110/outputs/checks/haruko-native24-v1/probe.json` | `b71e21e8366baf80` |
| `videos/sakuragi-haruko-after-school-30s-20261110/outputs/checks/lipsync/after-audio-rebuild.json` | `fafcd32bf742c2fd` |
| `videos/sakuragi-haruko-after-school-30s-20261110/outputs/checks/lipsync/cheer-v1-generation.json` | `5fef609fc3d34d76` |
| `videos/sakuragi-haruko-after-school-30s-20261110/outputs/checks/lipsync/ending-v1-generation.json` | `f6f2ee4c9d6d651a` |
| `videos/sakuragi-haruko-after-school-30s-20261110/outputs/checks/lipsync/export-dialogue-timing.json` | `77fed63c79d03deb` |
| `videos/sakuragi-haruko-after-school-30s-20261110/outputs/checks/lipsync/final-playback.json` | `bd4649af64e6d2c2` |
| `videos/sakuragi-haruko-after-school-30s-20261110/outputs/checks/lipsync/haruko-v1-generation.json` | `bb0202ae4998e982` |
| `videos/sakuragi-haruko-after-school-30s-20261110/outputs/checks/lipsync/input-sample-verification.json` | `b438e3bb7b9e9cf5` |
| `videos/sakuragi-haruko-after-school-30s-20261110/outputs/checks/lipsync/opening-v1-generation.json` | `bfdc377dc5d45a4d` |
| `videos/sakuragi-haruko-after-school-30s-20261110/outputs/checks/lipsync/queue.json` | `4ce9c9364757da5c` |
| `videos/sakuragi-haruko-after-school-30s-20261110/outputs/checks/lipsync/resolve-v1-generation.json` | `e0e80a0dda537a26` |
| `videos/sakuragi-haruko-after-school-30s-20261110/outputs/checks/lipsync/shy-v1-generation.json` | `4365dd77474a8124` |
| `videos/sakuragi-haruko-after-school-30s-20261110/outputs/checks/lipsync/shy-v2-generation.json` | `67c2e6e7f4dc817c` |
| `videos/sakuragi-haruko-after-school-30s-20261110/outputs/checks/lipsync/timing-verification.json` | `3f7a0396d9e64782` |
| `videos/sakuragi-haruko-after-school-30s-20261110/outputs/checks/lipsync/visual-decisions.json` | `68046862577ffd83` |
| `videos/sakuragi-haruko-after-school-30s-20261110/outputs/checks/lipsync/before-voiceover/final-mix-transcript.json` | `326d0e344d10c946` |
| `videos/sakuragi-haruko-after-school-30s-20261110/outputs/checks/lipsync/before-voiceover/playback.json` | `d7f792d5f55b976b` |
| `videos/sakuragi-haruko-after-school-30s-20261110/outputs/checks/lipsync/before-voiceover/video-stream-preserved.json` | `a866bb5af4be85f5` |
| `videos/sakuragi-haruko-after-school-30s-20261110/outputs/checks/lipsync/cheer-v1/audio-frame-rms.json` | `b7ce0eeedbc0828c` |
| `videos/sakuragi-haruko-after-school-30s-20261110/outputs/checks/lipsync/cheer-v1/probe.json` | `f364757ba168e79d` |
| `videos/sakuragi-haruko-after-school-30s-20261110/outputs/checks/lipsync/ending-v1/audio-frame-rms.json` | `f15cb8cda20cd738` |
| `videos/sakuragi-haruko-after-school-30s-20261110/outputs/checks/lipsync/ending-v1/probe.json` | `bed1d722e7127ada` |
| `videos/sakuragi-haruko-after-school-30s-20261110/outputs/checks/lipsync/haruko-v1/audio-frame-rms.json` | `82405ee2f77ab6d8` |
| `videos/sakuragi-haruko-after-school-30s-20261110/outputs/checks/lipsync/haruko-v1/probe.json` | `76aba7dcb8976585` |
| `videos/sakuragi-haruko-after-school-30s-20261110/outputs/checks/lipsync/opening-v1/audio-frame-rms.json` | `f114a80b0df6570a` |
| `videos/sakuragi-haruko-after-school-30s-20261110/outputs/checks/lipsync/opening-v1/probe.json` | `9eaca95e0bdaada7` |
| `videos/sakuragi-haruko-after-school-30s-20261110/outputs/checks/lipsync/resolve-v1/audio-frame-rms.json` | `d04863a775439d20` |
| `videos/sakuragi-haruko-after-school-30s-20261110/outputs/checks/lipsync/resolve-v1/probe.json` | `316dbd9e1a479a4c` |
| `videos/sakuragi-haruko-after-school-30s-20261110/outputs/checks/lipsync/shy-v1/audio-frame-rms.json` | `85c8822f46163694` |
| `videos/sakuragi-haruko-after-school-30s-20261110/outputs/checks/lipsync/shy-v1/probe.json` | `f6173460d5fed41d` |
| `videos/sakuragi-haruko-after-school-30s-20261110/outputs/checks/lipsync/shy-v2/audio-frame-rms.json` | `85c8822f46163694` |
| `videos/sakuragi-haruko-after-school-30s-20261110/outputs/checks/lipsync/shy-v2/probe.json` | `2a6dd01b8a884a4a` |
| `videos/sakuragi-haruko-after-school-30s-20261110/outputs/checks/opening-native24-v1/probe.json` | `bdff309019169175` |
| `videos/sakuragi-haruko-after-school-30s-20261110/outputs/checks/practice-native24-v1/probe.json` | `2b7f5c1eea0a7aed` |
| `videos/sakuragi-haruko-after-school-30s-20261110/outputs/checks/practice-native24-v2/probe.json` | `c18c08c5ed174a7d` |
| `videos/sakuragi-haruko-after-school-30s-20261110/outputs/checks/resolve-native24-v1/probe.json` | `f92d682d88641c98` |
| `videos/sakuragi-haruko-after-school-30s-20261110/outputs/checks/resolve-reframe-review/probe.json` | `ab577feeb01c6548` |
| `videos/sakuragi-haruko-after-school-30s-20261110/outputs/checks/shy-native24-v1/probe.json` | `163abbcbfc6ce88c` |
| `videos/sakuragi-haruko-after-school-30s-20261110/outputs/checks/shy-reframe-review/probe.json` | `4430f2e04feb3468` |
| `videos/sakuragi-haruko-after-school-30s-20261110/outputs/checks/shy-repair-review/probe.json` | `908c77090b45f3b6` |
| `videos/sakuragi-haruko-after-school-30s-20261110/outputs/checks/voiceover/alignment.json` | `49b4903062ab2888` |
| `videos/sakuragi-haruko-after-school-30s-20261110/outputs/checks/voiceover/asr-transcripts.json` | `c516c2a28bc3c193` |
| `videos/sakuragi-haruko-after-school-30s-20261110/outputs/checks/voiceover/before-audio-normalization.json` | `19070a9b08031d98` |
| `videos/sakuragi-haruko-after-school-30s-20261110/outputs/checks/voiceover/before-final-playback.json` | `320e9d25e339474c` |
| `videos/sakuragi-haruko-after-school-30s-20261110/outputs/checks/voiceover/before-package-report.json` | `201271958feb2a4c` |
| `videos/sakuragi-haruko-after-school-30s-20261110/outputs/checks/voiceover/before-verification.json` | `e3fded481e07f71c` |
| `videos/sakuragi-haruko-after-school-30s-20261110/outputs/checks/voiceover/final-mix-transcript.json` | `f96312e7c3c289a8` |
| `videos/sakuragi-haruko-after-school-30s-20261110/outputs/checks/voiceover/generation-cheer-v1.json` | `8bc0eac8b734cec7` |
| `videos/sakuragi-haruko-after-school-30s-20261110/outputs/checks/voiceover/generation-ending-v1.json` | `693583eac7288c31` |
| `videos/sakuragi-haruko-after-school-30s-20261110/outputs/checks/voiceover/generation-haruko-v1.json` | `a8f4c09344f2733f` |
| `videos/sakuragi-haruko-after-school-30s-20261110/outputs/checks/voiceover/generation-opening-v1.json` | `790ff4e8444504a7` |
| `videos/sakuragi-haruko-after-school-30s-20261110/outputs/checks/voiceover/generation-resolve-v1.json` | `e49c93f6163177db` |
| `videos/sakuragi-haruko-after-school-30s-20261110/outputs/checks/voiceover/generation-shy-v1.json` | `9498af291f098162` |
| `videos/sakuragi-haruko-after-school-30s-20261110/outputs/checks/voiceover/playback.json` | `bd4649af64e6d2c2` |
| `videos/sakuragi-haruko-after-school-30s-20261110/outputs/checks/voiceover/timing-verification.json` | `70fd8ab34ee37c3c` |
| `videos/sakuragi-haruko-after-school-30s-20261110/outputs/checks/voiceover/video-stream-preserved.json` | `2aa86ce5500befb1` |
| `videos/sakuragi-haruko-after-school-30s-20261110/scripts/adopt-lipsync.py` | `66aa5919790160cd` |
| `videos/sakuragi-haruko-after-school-30s-20261110/scripts/check-dialogue.py` | `50fe4072032bfbcf` |
| `videos/sakuragi-haruko-after-school-30s-20261110/scripts/check-export-dialogue-timing.py` | `e718190d63d55957` |
| `videos/sakuragi-haruko-after-school-30s-20261110/scripts/check-lipsync.py` | `6e392daaf7b87978` |
| `videos/sakuragi-haruko-after-school-30s-20261110/scripts/check.py` | `f83ec3b50af05328` |
| `videos/sakuragi-haruko-after-school-30s-20261110/scripts/fetch-asr-model.py` | `092576a0be9d9246` |
| `videos/sakuragi-haruko-after-school-30s-20261110/scripts/fetch-tts-model.py` | `ce12ff8b6da425d8` |
| `videos/sakuragi-haruko-after-school-30s-20261110/scripts/fetch-video-model.py` | `c3b59ee9e90c5c74` |
| `videos/sakuragi-haruko-after-school-30s-20261110/scripts/final-audio.py` | `25fd60e6e7535947` |
| `videos/sakuragi-haruko-after-school-30s-20261110/scripts/finish-stance.py` | `34f33e0bf20ba98a` |
| `videos/sakuragi-haruko-after-school-30s-20261110/scripts/generate-after.py` | `c27b66da6f7e2b55` |
| `videos/sakuragi-haruko-after-school-30s-20261110/scripts/generate-clip.py` | `7c36489422306df5` |
| `videos/sakuragi-haruko-after-school-30s-20261110/scripts/generate-lipsync-queue.py` | `aedf40fe08fefcb6` |
| `videos/sakuragi-haruko-after-school-30s-20261110/scripts/generate-lipsync-repair-after.py` | `99edb66acac753fa` |
| `videos/sakuragi-haruko-after-school-30s-20261110/scripts/generate-lipsync.py` | `7ca3da9bb134a290` |
| `videos/sakuragi-haruko-after-school-30s-20261110/scripts/generate-queue.py` | `369aa2265088baf5` |
| `videos/sakuragi-haruko-after-school-30s-20261110/scripts/generation-report.py` | `a2f48b38ca029539` |
| `videos/sakuragi-haruko-after-school-30s-20261110/scripts/ingest-image.py` | `d700b3ff8c70e09d` |
| `videos/sakuragi-haruko-after-school-30s-20261110/scripts/lowram_prompt.py` | `f8267e06ce389727` |
| `videos/sakuragi-haruko-after-school-30s-20261110/scripts/ltx-entry.py` | `6643f955c03c6588` |
| `videos/sakuragi-haruko-after-school-30s-20261110/scripts/make-audio.py` | `564874734c62cc5d` |
| `videos/sakuragi-haruko-after-school-30s-20261110/scripts/mux-voiceover.py` | `9213b8d8002ae6d8` |
| `videos/sakuragi-haruko-after-school-30s-20261110/scripts/package.py` | `a999f352b9529d27` |
| `videos/sakuragi-haruko-after-school-30s-20261110/scripts/prepare-dialogue.py` | `707b664df5a94726` |
| `videos/sakuragi-haruko-after-school-30s-20261110/scripts/prepare-lipsync-before.py` | `29e7dfa2afabe758` |
| `videos/sakuragi-haruko-after-school-30s-20261110/scripts/prepare-lipsync-inputs.py` | `2d558a293400813b` |
| `videos/sakuragi-haruko-after-school-30s-20261110/scripts/preview-lipsync.py` | `4f4c3c2a8b8a8f63` |
| `videos/sakuragi-haruko-after-school-30s-20261110/scripts/project_env.py` | `fce0376cda309a97` |
| `videos/sakuragi-haruko-after-school-30s-20261110/scripts/review-clip.py` | `63e2f16359d199df` |
| `videos/sakuragi-haruko-after-school-30s-20261110/scripts/review-final.py` | `87da1f84655f0dfd` |
| `videos/sakuragi-haruko-after-school-30s-20261110/scripts/review-lipsync.py` | `edf7e4db5b8ac5f4` |
| `videos/sakuragi-haruko-after-school-30s-20261110/scripts/run.mjs` | `7abbc8ab30f694d3` |
| `videos/sakuragi-haruko-after-school-30s-20261110/scripts/select-clips.py` | `6a7d50bd95a12560` |
| `videos/sakuragi-haruko-after-school-30s-20261110/scripts/setup-generator.py` | `b48372860c66d86d` |
| `videos/sakuragi-haruko-after-school-30s-20261110/scripts/setup-tts.py` | `d68bbd047c3ca11c` |
| `videos/sakuragi-haruko-after-school-30s-20261110/scripts/synthesize-dialogue.py` | `4339c1cd045200de` |
| `videos/sakuragi-haruko-after-school-30s-20261110/src/Caption.tsx` | `6abe2b7047216cf9` |
| `videos/sakuragi-haruko-after-school-30s-20261110/src/Cheer.tsx` | `a953dbd2a68d3b42` |
| `videos/sakuragi-haruko-after-school-30s-20261110/src/Ending.tsx` | `b279dbc2fec6551e` |
| `videos/sakuragi-haruko-after-school-30s-20261110/src/Epilogue.tsx` | `138034d04d57accc` |
| `videos/sakuragi-haruko-after-school-30s-20261110/src/Gather.tsx` | `21f84e5dca4b84d5` |
| `videos/sakuragi-haruko-after-school-30s-20261110/src/Haruko.tsx` | `c7294d5758889ff8` |
| `videos/sakuragi-haruko-after-school-30s-20261110/src/Opening.tsx` | `0f7cc168c4ae9694` |
| `videos/sakuragi-haruko-after-school-30s-20261110/src/Practice.tsx` | `333e717825cecf09` |
| `videos/sakuragi-haruko-after-school-30s-20261110/src/Resolve.tsx` | `ea192e0bc1e8e83c` |
| `videos/sakuragi-haruko-after-school-30s-20261110/src/Root.tsx` | `2b7dd4207562ebef` |
| `videos/sakuragi-haruko-after-school-30s-20261110/src/SceneVideo.tsx` | `d37cc771eb356210` |
| `videos/sakuragi-haruko-after-school-30s-20261110/src/Shy.tsx` | `8cee2af13b04042f` |
| `videos/sakuragi-haruko-after-school-30s-20261110/src/Smile.tsx` | `5a6a429da59d082d` |
| `videos/sakuragi-haruko-after-school-30s-20261110/src/fonts.ts` | `4c854768cbef4ad8` |
| `videos/sakuragi-haruko-after-school-30s-20261110/src/index.ts` | `f9bc4e836a064aca` |
| `videos/sakuragi-haruko-after-school-30s-20261110/src/media-status.ts` | `7000e9448eebd484` |
