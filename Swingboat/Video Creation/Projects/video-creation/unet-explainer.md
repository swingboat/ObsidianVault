---
type: video-production-dossier
project: video-creation
video_id: unet-explainer
captured: '2026-11-12'
status: 已有讲解成片、旁白与句级字幕
source_fingerprint: 909129796470deb9be4641f97fa531fb22bc7f3b6a14488f746f14a49394c8db
tags:
- video-creation
- production-capture
---

# U-Net 中文讲解 · 旁白驱动教学动画

<!-- capture-source-sha256:909129796470deb9be4641f97fa531fb22bc7f3b6a14488f746f14a49394c8db -->

本次为指定项目的材料回溯与归档。制作结论来自项目记录/源码，未找回关闭的制作对话，也未重新执行生成、渲染、音频合成或用户验收。

## 当前结论与制作路线

**状态：已有讲解成片、旁白与句级字幕。**

旁白文稿 → 系统 TTS → 实测音频时长 → 七镜头 SVG/React 教学动画。

1920×1080、30fps、4750 帧，约 158.33 秒。七个镜头围绕同一张 U 形图展开。macOS Tingting 按 32 句生成中文旁白，实测 WAV 长度决定字幕和时间轴；calculateMetadata 读取生成的总帧数。

## 关键算法、实现与适用边界

制作算法包括帧驱动图示、固定种子的特征纹理、句级采样长度到帧数的映射、镜头最短教学时长与停顿分配。U-Net 是讲解主题：空间尺寸、通道拼接和分割图为教学示意，项目未训练真实 U-Net，也未使用模型激活生成画面。跳跃连接沿通道拼接，不是特征值逐元素相加。

## 声音与字幕

系统 say/afconvert 逐句合成，缓存未变文稿；48kHz 单声道 PCM WAV。替换 sentence-01 至 sentence-32 后可 --reuse-audio 重测时间轴。无背景音乐。

## 失败、修正与验收边界

为教学采用同尺寸填充，明确省略原论文无填充卷积造成的裁剪；字幕为实测句级时点，未做词级强制对齐或逐句人工听校。发音稿把 U-Net 写成 U net 以改善系统发音。

## 技术与模型版本

以下是本项目声明或锁定的依赖，安装清单本身不证明某项参与了最终画面。具体用途以制作路线、源码和生成记录交叉判断。

| 依赖 | package.json 声明 | 可解析的直接锁定版本 |
| --- | --- | --- |
| `@remotion/bundler` | `4.0.534` | `4.0.534` |
| `@remotion/cli` | `4.0.534` | `4.0.534` |
| `@remotion/renderer` | `4.0.534` | `4.0.534` |
| `react` | `19.2.3` | `19.2.3` |
| `react-dom` | `19.2.3` | `19.2.3` |
| `remotion` | `4.0.534` | `4.0.534` |
| `@remotion/fonts` | `4.0.534` | `4.0.534` |
| `@remotion/media` | `4.0.534` | `4.0.534` |
| `@remotion/captions` | `4.0.534` | `4.0.534` |
| `@types/react` | `19.2.7` | `19.2.7` |
| `@types/react-dom` | `19.2.3` | `19.2.3` |
| `typescript` | `5.9.3` | `5.9.3` |

工具底层模型未公开时按“未披露”记录；不能由 imagegen 工具名猜测图像模型。未提供真实推理日志的配置不记作已运行。

## 镜头、时间轴与采用参数

> [!info]- 当前 Composition / 时间轴源码：videos/unet-explainer/src/Root.tsx，L1–29
> ```tsx
> import {Composition, Folder} from 'remotion';
> import {UnetStoryboard} from './unet/UnetStoryboard';
> import {SkipScene} from './unet/scenes/SkipScene';
> import {UnetExplainer} from './UnetExplainer';
> import {timeline as unetTimeline} from './unet/timeline.generated';
> import {TaskScene} from './unet/scenes/TaskScene';
> import {OverviewScene} from './unet/scenes/OverviewScene';
> import {EncoderScene} from './unet/scenes/EncoderScene';
> import {DecoderScene} from './unet/scenes/DecoderScene';
> import {TrainingScene} from './unet/scenes/TrainingScene';
> import {SummaryScene} from './unet/scenes/SummaryScene';
> 
> export const RemotionRoot = () => (
>   <>
>   <Composition id="UnetExplainer" component={UnetExplainer} durationInFrames={4750} fps={30} width={1920} height={1080}
>     calculateMetadata={() => ({durationInFrames: unetTimeline.durationFrames})} />
>   <Folder name="Unet-Scenes">
>     <Composition id="UnetTask" component={TaskScene} durationInFrames={617} fps={30} width={1920} height={1080} calculateMetadata={() => ({durationInFrames: unetTimeline.scenes[0].durationFrames})} />
>     <Composition id="UnetOverview" component={OverviewScene} durationInFrames={600} fps={30} width={1920} height={1080} calculateMetadata={() => ({durationInFrames: unetTimeline.scenes[1].durationFrames})} />
>     <Composition id="UnetEncoder" component={EncoderScene} durationInFrames={750} fps={30} width={1920} height={1080} calculateMetadata={() => ({durationInFrames: unetTimeline.scenes[2].durationFrames})} />
>     <Composition id="UnetDecoder" component={DecoderScene} durationInFrames={750} fps={30} width={1920} height={1080} calculateMetadata={() => ({durationInFrames: unetTimeline.scenes[3].durationFrames})} />
>     <Composition id="UnetSkip" component={SkipScene} durationInFrames={900} fps={30} width={1920} height={1080} calculateMetadata={() => ({durationInFrames: unetTimeline.scenes[4].durationFrames})} />
>     <Composition id="UnetTraining" component={TrainingScene} durationInFrames={750} fps={30} width={1920} height={1080} calculateMetadata={() => ({durationInFrames: unetTimeline.scenes[5].durationFrames})} />
>     <Composition id="UnetSummary" component={SummaryScene} durationInFrames={383} fps={30} width={1920} height={1080} calculateMetadata={() => ({durationInFrames: unetTimeline.scenes[6].durationFrames})} />
>   </Folder>
>   <Composition id="UnetStoryboard" component={UnetStoryboard} durationInFrames={1} fps={30} width={1920} height={1080} defaultProps={{scene: 0}} />
>   <Composition id="UnetSkipSample" component={SkipScene} durationInFrames={360} fps={30} width={1920} height={1080} />
>   </>
> );
> ```

源时间轴、生成帧数、所用区间与最终时长分别记录；历史文档或候选参数不自动覆盖当前采用配置。

## 参考来源与借鉴说明

创作参考视频或 X 帖子：材料中未找到明确链接。知识依据为原始 U-Net 论文和作者项目，属于教学内容来源；本次未重新在线核读论文。

### 项目记录的技术、内容或素材来源

这些 URL 来自项目文件，尚不自动证明本片采用；除本节明确标注的访问外，本轮未逐一在线重读。

| 链接 | 记录文件 |
| --- | --- |
| [arxiv.org](https://arxiv.org/abs/1505.04597) | `videos/unet-explainer/README.md` |
| [lmb.informatik.uni-freiburg.de](https://lmb.informatik.uni-freiburg.de/people/ronneber/u-net/) | `videos/unet-explainer/README.md` |
| [hyperos.mi.com](https://hyperos.mi.com/font/download) | `videos/unet-explainer/public/fonts/misans-source.json` |
| [hyperos.mi.com](https://hyperos.mi.com/font-download/MiSans.zip) | `videos/unet-explainer/public/fonts/misans-source.json` |

## 重现入口与修改位置

以下是 package.json 中的实际命令映射，供在对应视频目录使用；本轮未执行这些制作命令。系统依赖、前置模型/素材和环境准备见后附 README。

| npm 入口 | 实际命令 |
| --- | --- |
| `npm run dev` | `node scripts/remotion.mjs studio src/index.ts --no-open --port=3210` |
| `npm run compositions` | `node scripts/remotion.mjs compositions src/index.ts` |
| `npm run typecheck` | `tsc --noEmit` |
| `npm run render` | `node scripts/remotion.mjs render src/index.ts UnetExplainer outputs/unet-explainer.mp4 --codec=h264 --audio-codec=aac --pixel-format=yuv420p --concurrency=2` |
| `npm run still` | `node scripts/remotion.mjs still src/index.ts UnetExplainer outputs/checks/current.png --frame=3454` |
| `npm run check` | `npm run typecheck && node scripts/check-isolation.mjs` |
| `npm run pack` | `python3 scripts/package-project.py` |
| `npm run prepare:unet` | `python3 scripts/prepare-unet-audio.py` |
| `npm run render:unet` | `node scripts/remotion.mjs render src/index.ts UnetExplainer outputs/unet-explainer.mp4 --codec=h264 --audio-codec=aac --pixel-format=yuv420p --concurrency=2` |
| `npm run review:unet` | `node scripts/render-unet-review.mjs --sample` |
| `npm run verify:unet` | `node scripts/verify-unet.mjs` |
| `npm run verify` | `node scripts/verify-media.mjs` |

### 算法源码摘录

> [!info]- videos/unet-explainer/scripts/prepare-unet-audio.py，L1–96
> ```python
> """Project-local macOS TTS; --reuse-audio measures replacement sentence WAVs."""
> import argparse, array, hashlib, json, math, pathlib, subprocess, wave
> 
> ROOT = pathlib.Path(__file__).resolve().parent.parent
> AUDIO = ROOT / 'public/unet/audio'
> OUT = ROOT / 'outputs'
> RATE = 48000
> FPS = 30
> TARGET = [15, 20, 25, 25, 30, 25, 10]
> 
> def read_wav(path):
>     with wave.open(str(path)) as w:
>         if (w.getframerate(), w.getnchannels(), w.getsampwidth()) != (RATE, 1, 2):
>             raise ValueError(f'{path}: 需要48kHz、单声道、16bit PCM WAV')
>         return array.array('h', w.readframes(w.getnframes()))
> 
> def write_wav(path, samples):
>     with wave.open(str(path), 'wb') as w:
>         w.setnchannels(1); w.setsampwidth(2); w.setframerate(RATE); w.writeframes(samples.tobytes())
> 
> def trim(samples):
>     block = 480
>     active = [i for i in range(0, len(samples), block) if math.sqrt(sum(v*v for v in samples[i:i+block]) / max(1, len(samples[i:i+block]))) > 80]
>     if not active: raise ValueError('旁白音频没有可检测的声音')
>     x = samples[max(0, active[0] - 3840): min(len(samples), active[-1] + block + 4800)]
>     peak = max(abs(v) for v in x)
>     gain = min(1.6, 0.78 * 32767 / peak)
>     return array.array('h', (round(v * gain) for v in x))
> 
> def wrap_caption(text):
>     if len(text) <= 27: return text
>     cut = text.find('，', max(8, len(text)//2-8), min(len(text)-7, len(text)//2+9))
>     cut = cut+1 if cut >= 0 else len(text)//2
>     return text[:cut] + '\n' + text[cut:]
> 
> def stamp(ms):
>     n = round(ms); hours, n = divmod(n, 3600000); minutes, n = divmod(n, 60000); sec, n = divmod(n, 1000)
>     return f'{hours:02}:{minutes:02}:{sec:02},{n:03}'
> 
> def main():
>     ap = argparse.ArgumentParser(); ap.add_argument('--reuse-audio', action='store_true'); ap.add_argument('--voice', default='Tingting'); ap.add_argument('--rate', type=int, default=175)
>     args = ap.parse_args(); AUDIO.mkdir(parents=True, exist_ok=True); OUT.mkdir(parents=True, exist_ok=True); (OUT/'checks').mkdir(parents=True, exist_ok=True)
>     lines = json.loads((ROOT/'public/unet/narration.json').read_text())
>     spoken = []; records = []
>     for i, line in enumerate(lines):
>         tts = line.get('tts', line['text'].replace('U-Net', 'U net').replace('字母 U', '字母 U'))
>         spoken.append(f'{i+1:02d}. {tts}')
>         base = AUDIO / f'sentence-{i+1:02d}'
>         signature = hashlib.sha256(f'{args.voice}|{args.rate}|{tts}'.encode()).hexdigest()
>         key = base.with_suffix('.sha256')
>         if not args.reuse_audio and (not base.with_suffix('.wav').exists() or not key.exists() or key.read_text() != signature):
>             base.with_suffix('.txt').write_text(tts)
>             subprocess.run(['say','-v',args.voice,'-r',str(args.rate),'-f',str(base.with_suffix('.txt')),'-o',str(base.with_suffix('.aiff'))],check=True)
>             subprocess.run(['afconvert',str(base.with_suffix('.aiff')),str(base.with_suffix('.wav')),'-f','WAVE','-d','LEI16@48000'],check=True)
>             write_wav(base.with_suffix('.wav'), trim(read_wav(base.with_suffix('.wav'))))
>             key.write_text(signature)
>         data = read_wav(base.with_suffix('.wav'))
>         records.append({'scene':line['scene'], 'text':wrap_caption(line['text']), 'samples':data, 'file':str(base.with_suffix('.wav').relative_to(ROOT/'public'))})
>         print(f'句子 {i+1:02d}/{len(lines)}：{len(data)/RATE:.2f}s',flush=True)
>     timeline = {'fps':FPS,'audioAvailable':True,'voice':args.voice,'speechRate':args.rate,'scenes':[],'durationFrames':0}
>     global_frames=0; srt=[]; audio_all=array.array('h'); waveform=[]
>     for scene in range(7):
>         entries=[r for r in records if r['scene']==scene]
>         lengths=[math.ceil(len(r['samples'])/RATE*FPS) for r in entries]
>         minimum=18+sum(lengths)+8*len(entries)+25
>         duration=max(round(TARGET[scene]*FPS),minimum)
>         extra=duration-minimum
>         pauses=[8+extra//len(entries)+(1 if i<extra%len(entries) else 0) for i in range(len(entries))]
>         data=array.array('h',[0]) * (duration*RATE//FPS)
>         cursor=18; captions=[]
>         for j,(r,length) in enumerate(zip(entries,lengths)):
>             sample_start=cursor*RATE//FPS
>             data[sample_start:sample_start+len(r['samples'])]=r['samples']
>             start=cursor/FPS*1000; end=start+len(r['samples'])/RATE*1000
>             cap={'text':r['text'],'startMs':round(start,3),'endMs':round(end,3),'timestampMs':None,'confidence':None}
>             captions.append(cap)
>             srt.append(f'{len(srt)+1}\n{stamp(global_frames/FPS*1000+start)} --> {stamp(global_frames/FPS*1000+end)}\n{r["text"]}\n')
>             waveform.append({'sentenceFile':r['file'],'scene':scene,'startMs':round(global_frames/FPS*1000+start,3),'durationMs':round(len(r['samples'])/RATE*1000,3),
>                 'peak':round(max(abs(x) for x in r['samples'])/32768,4)})
>             cursor+=length+pauses[j]
>         src=f'unet/audio/scene-{scene+1}.wav'
>         write_wav(ROOT/'public'/src,data); audio_all.extend(data)
>         timeline['scenes'].append({'startFrame':global_frames,'durationFrames':duration,'audioSrc':src,'captions':captions})
>         global_frames+=duration
>     timeline['durationFrames']=global_frames
>     (OUT/'timeline.json').write_text(json.dumps(timeline,ensure_ascii=False,indent=2)+'\n')
>     (ROOT/'src/unet/timeline.generated.ts').write_text("// Generated by scripts/prepare-unet-audio.py; do not edit by hand.\nimport type {UnetTimeline} from './types';\nexport const timeline: UnetTimeline = "+json.dumps(timeline,ensure_ascii=False,indent=2)+';\n')
>     (OUT/'unet-explainer.srt').write_text('\n'.join(srt))
>     (OUT/'tts-narration.txt').write_text('\n'.join(spoken)+'\n')
>     (OUT/'narration.txt').write_text('\n\n'.join('镜头 '+str(s+1)+'\n'+''.join(i['text'] for i in lines if i['scene']==s) for s in range(7))+'\n')
>     write_wav(OUT/'narration.wav',audio_all)
>     (OUT/'checks/audio-check.json').write_text(json.dumps({'voice':args.voice,'rate':args.rate,'sampleRate':RATE,'channels':1,'durationSeconds':global_frames/FPS,'sentences':waveform,
>       'alignment':'逐句生成音频，按实际采样长度安排每句字幕；不做词级对齐','ttsPronunciation':'U-Net 在独立 TTS 文稿中写为 U net；屏幕与字幕保留 U-Net'},ensure_ascii=False,indent=2))
>     print(f'配音与时间轴完成：{global_frames/FPS:.3f}s / {global_frames} 帧',flush=True)
> 
> if __name__=='__main__': main()
> ```

> [!info]- videos/unet-explainer/src/unet/ConcatDemo.tsx，L1–26
> ```tsx
> import {FeatureMapStack} from './FeatureMapStack';
> import {palette} from './settings';
> import {move} from './motion';
> 
> export const ConcatDemo = ({progress}: {progress: number}) => {
>   const merge = move(progress, 0.35, 0.62); const fuse = move(progress, 0.73, 0.88);
>   const xA = 140 + merge * 180; const xB = 760 - merge * 380;
>   return <svg width="100%" height="100%" viewBox="0 0 1100 610">
>     <text x="550" y="45" textAnchor="middle" fontSize="39" fontWeight="700" fill={palette.ink}>同样的宽和高，沿通道维度连接</text>
>     <g opacity={1 - fuse}>
>       <FeatureMapStack x={xA} y={165} size={180} color={palette.encoder} layers={4} seed={27} />
>       <FeatureMapStack x={xB} y={165 - merge * 40} size={180} color={palette.decoder} layers={4} seed={89} />
>       <text x="140" y="415" fontSize="31" fill={palette.encoder} opacity={1 - merge}>编码器：64 通道</text>
>       <text x="750" y="415" fontSize="31" fill={palette.decoder} opacity={1 - merge}>解码器：64 通道</text>
>       <path d="M470 280L650 175" fill="none" stroke={palette.skip} strokeWidth="3" opacity={merge} />
>       <text x="670" y="171" fontSize="28" fill={palette.skip} opacity={merge}>通道轴 C</text>
>     </g>
>     <g opacity={fuse}>
>       <FeatureMapStack x={420} y={160} size={180} color={palette.skip} layers={4} seed={143} />
>     </g>
>     <text x="550" y="415" textAnchor="middle" fontSize="36" fontWeight="700" fill={palette.skip} opacity={merge}>{fuse > 0.5 ? '卷积融合 → 64 通道' : '拼接后：128 通道'}</text>
>     <text x="550" y="480" textAnchor="middle" fontSize="38" fontWeight="700" fill={palette.ink}>64 + 64 → 128 通道</text>
>     <text x="550" y="535" textAnchor="middle" fontSize="28" fill={palette.muted}>64×64×64  →  64×64×128  →  64×64×64</text>
>     <text x="550" y="585" textAnchor="middle" fontSize="27" fill={palette.ink}>“+”只表示通道数计数；特征值没有逐元素相加</text>
>   </svg>;
> };
> ```

> [!info]- videos/unet-explainer/src/unet/motion.ts，L1–6
> ```typescript
> import {Easing, interpolate, useCurrentFrame, useVideoConfig} from 'remotion';
> export const move = (p: number, a: number, b: number, x = 0, y = 1) => interpolate(p, [a, b], [x, y], {easing: Easing.bezier(0.3, 0, 0.2, 1), extrapolateLeft: 'clamp', extrapolateRight: 'clamp'});
> export const useSceneProgress = (still = false, sceneFrames?: number) => {
>   const frame = useCurrentFrame(); const {durationInFrames} = useVideoConfig();
>   return still ? 0.83 : Math.max(0, Math.min(1, frame / Math.max(1, (sceneFrames ?? durationInFrames) - 1)));
> };
> ```

## 完整关键提示词与生成记录

以下保留项目自有提示词、候选参数与版本。存在失败候选和历史参数；最终采用以 selected-takes、edit-decisions 与制作复盘为准，不能将所有候选都写成已采用。

未发现实际生成提示词或推理执行记录；不从脚手架、模型安装清单推测生成过程。

## 已有产物与检查依据

文件存在、技术检查、画面检查与用户验收分别看待。下表是本次文件盘点，旧版与当前版可能共存。MP4/HTML 按内容计算 SHA-256，ZIP 仅记录文件大小和修改时间供增量判断。

| 本地产物 | 字节数 | SHA-256 |
| --- | ---: | --- |
| `videos/unet-explainer/outputs/preview.html` | 2301 | `28ee3f1b5114c3b171156f367ca0085dfff1b57e3aee6f8d9c5b6c9926ca1570` |
| `videos/unet-explainer/outputs/skip-connection-sample.mp4` | 552537 | `6b1d7ccb067227cc76bf6a0e45b3cb41095f948b62437fa03d1ee8a72791e7a9` |
| `videos/unet-explainer/outputs/unet-explainer.mp4` | 10671408 | `9d00d7e0cb1f64664dce1cb3fc85aa2a406eb7f2c6ac7f3b7338cfe45e15b0ca` |
| `videos/unet-explainer/outputs/unet-remotion-project.zip` | 59543991 | `ZIP：仅元数据` |

> [!info]- 本次只读 ffprobe 结果
> ```json
> {
>   "programs": [],
>   "stream_groups": [],
>   "streams": [
>     {
>       "codec_name": "h264",
>       "codec_type": "video",
>       "width": 1920,
>       "height": 1080,
>       "r_frame_rate": "30/1",
>       "duration": "158.333333",
>       "nb_frames": "4750"
>     },
>     {
>       "codec_name": "aac",
>       "codec_type": "audio",
>       "sample_rate": "48000",
>       "channels": 2,
>       "r_frame_rate": "0/0",
>       "duration": "158.336000",
>       "nb_frames": "7424"
>     }
>   ],
>   "format": {
>     "duration": "158.336000",
>     "size": "10671408"
>   },
>   "file": "outputs/unet-explainer.mp4",
>   "scope": "本次只读取媒体头；未全片解码、播放或重新进行视觉/听觉验收。"
> }
> ```


已有检查报告路径（未在本轮重新执行）：

- `videos/unet-explainer/outputs/checks/archive-report.json`
- `videos/unet-explainer/outputs/checks/audio-check.json`
- `videos/unet-explainer/outputs/checks/final-media-report.json`
- `videos/unet-explainer/outputs/checks/isolation-report.json`
- `videos/unet-explainer/outputs/checks/keyframes-report.json`
- `videos/unet-explainer/outputs/checks/media-verification.json`
- `videos/unet-explainer/outputs/checks/migration-report.json`
- `videos/unet-explainer/outputs/checks/playback-report.json`
- `videos/unet-explainer/outputs/checks/static-review.json`

## 制作方法与迭代记录选编

下面是项目自有文档的归档摘录，保留原阶段与原检查表述，个人路径已泛化、相对链接已注明源项目位置。它们属于来源陈述，不代表本次重新完成其中的生成或验收。

> [!info]- 来源文档：videos/unet-explainer/README.md
> # U-Net 中文讲解视频
> 
> 本视频已迁移为独立 Remotion 项目。Composition 为 `UnetExplainer`：1920×1080、30 fps、4750 帧，时长 158.33 秒。七个镜头围绕同一张 U 形结构图展开，使用本地 MiSans 字体。中文普通话旁白由本机 macOS Tingting 生成，无背景音乐，无付费 API。
> 
> ## 交付文件
> 
> - `unet-explainer.mp4`：H.264 / yuv420p，含 AAC 旁白与烧录中文字幕。
> - `unet-explainer.srt`：32 条句级字幕，来自逐句音频的实测时间。
> - `narration.txt`：完整旁白；`tts-narration.txt`：发音调整版，U-Net 统一写为 `U net`。
> - `narration.wav`：48 kHz、单声道完整配音。
> - `storyboard.md`、`storyboard/`：七段分镜与静态关键帧。
> - `skip-connection-sample.mp4`：12 秒无声拼接样片。
> - `timeline.json`：实测时间轴。
> - `unet-remotion-project.zip`：可编辑完整项目，包含源代码、本地字体、逐句与逐镜头音频、锁定依赖清单；不包含 node_modules 和渲染缓存。
> - `checks/`：视觉关键帧、音频与时间轴检查记录、最终媒体检查记录。
> - `preview.html`：本地成品播放器，支持跳转章节；可直接打开，或通过本地 HTTP 服务播放。
> 
> ## 安装、预览与重新导出
> 
> 解压项目后进入 `unet-explainer` 目录。需要 Node.js 22 或更高版本；本次使用 Node.js 24.13.1。依赖只安装在项目内：
> 
> ```bash
> npm ci
> npm run dev
> ```
> 
> 在浏览器打开 `http://localhost:3210/UnetExplainer`。逐段 Composition 位于 `Unet-Scenes` 文件夹。
> 
> ```bash
> npm run typecheck
> npm run render:unet
> ```
> 
> 最终视频输出至 `outputs/unet-explainer.mp4`。第一次渲染时 Remotion 可能自动下载官方 Chrome Headless Shell。已附带配音，常规预览和导出不需要 macOS TTS。
> 
> 成品播放器：运行 `python3 scripts/serve-unet.py`，打开 `http://localhost:3211/preview.html`。服务仅绑定本机，并支持视频章节跳转所需的字节范围请求。
> 
> ```bash
> node scripts/render-unet-review.mjs --sample
> node scripts/render-unet-keyframes.mjs
> node scripts/verify-unet.mjs
> ```
> 
> 前两条重新生成分镜/样片与完整关键帧；最后一条检查已导出的 MP4 并完整解码、抽取成品帧。
> 
> ## 修改内容
> 
> - `public/unet/narration.json`：逐句旁白与镜头归属，字幕保留正确术语。
> - `src/unet/settings.ts`：集中管理配色、字号、空间尺寸、通道数和标题。
> - `src/unet/scenes/`：七个独立镜头；`src/UnetExplainer.tsx`：镜头编排。
> - `src/unet/UNetDiagram.tsx`、`FeatureMapStack.tsx`、`ConcatDemo.tsx`、`SegmentationExample.tsx`、`TrainingDemo.tsx`、`CaptionOverlay.tsx`：可复用图形与字幕组件。
> - `scripts/prepare-unet-audio.py`：语音生成、实测音频时间、停顿与镜头时长计算。`TARGET` 为各镜头最短教学时长。
> - `src/unet/timeline.generated.ts`：生成的最终时间轴，请通过脚本更新。Root 的 `calculateMetadata` 自动采用总帧数。
> 
> 所有动画依照帧号计算；镜头内部使用局部帧。特征纹理使用固定的数学种子，同一帧确定。字幕位于单独底部安全区域。
> 
> ## 配音替换与时间轴更新
> 
> 在已安装中文 Tingting 声音的 macOS 上重新生成：
> 
> ```bash
> npm run prepare:unet
> # 可选：指定已安装的声音与语速
> npm run prepare:unet -- --voice Tingting --rate 175
> ```
> 
> 脚本调用系统 `say` / `afconvert`，按句生成，缓存未变的文本；只裁去外部空白并调整电平，不加速。字幕起止取实际采样长度。
> 
> 替换真人或其他旁白时，将 `public/unet/audio/sentence-01.wav` 至 `sentence-32.wav` 替换为对应短句的 **48 kHz、单声道、16 位 PCM WAV**，再执行：
> 
> ```bash
> npm run prepare:unet -- --reuse-audio
> npm run typecheck
> node scripts/render-unet-keyframes.mjs
> npm run render:unet
> node scripts/verify-unet.mjs
> ```
> 
> 脚本重新测量 WAV，生成镜头音频、完整 WAV、SRT 和时间轴；保留必要停顿，不强行压至固定总时长。新增、删除或重排句子时，也应更新 Task/Encoder/Decoder/Skip 场景中的字幕提示索引，让动画继续对应讲解内容。
> 
> ## 教学简化与素材标注
> 
> 空间尺寸为 256→128→64→32→16→32→64→128→256，采用同尺寸填充。通道配置用于教学，不代表原论文精确配置。原始 U-Net 使用不填充卷积，发生尺寸变化，并在跳跃连接前裁剪；视频中明确说明省略这些细节，重点解释信息流。参考原始论文：https://arxiv.org/abs/1505.04597 与作者项目：https://lmb.informatik.uni-freiburg.de/people/ronneber/u-net/。
> 
> 跳跃连接是沿通道维拼接，两路空间尺寸均为 64×64、各 64 通道，拼接为 128 通道，再卷积融合。画面明确“+ 只表示通道数计数；特征值没有逐元素相加”。最后 1×1 卷积映射为前景/背景两类；上采样不保证恢复全部边界信息。
> 
> 细胞图与标注掩码为 React/SVG 合成示意，标注“分割示意”；纹理标注“特征示意”；故意带误差的预测标注“预测示意”，红线强调与标注边界的差异。没有训练真实模型、使用真实激活或声称实际准确率。
> 
> ## 验证与限制
> 
> 已经进行 TypeScript 检查、Remotion 构建、七个静态分镜和拼接样片检查、完整动画关键帧人工视觉检查，以及重复帧一致性和字幕时间范围检查。最终编码、全片解码和播放检查的实测结果见 `checks/final-media-report.json` 与 `checks/playback-report.json`。
> 
> 旁白文件有效、时长与电平均经过检查；U-Net 在 TTS 文稿中统一读法。未进行逐句人工听校或词级强制对齐，字幕采用实测句级时间戳。这是本版保留的质量检查限制；项目、配音、字幕和 MP4 均已制作。
> 
> 独立目录迁移检查：`npm run check`；完整成品参数与解码检查：`npm run verify`；本视频单独打包：`npm run pack`。

## 证据清单与缺失信息

盘点的文本/报告文件 82 个，源文本、报告及媒体产物身份摘要为 `909129796470deb9be4641f97fa531fb22bc7f3b6a14488f746f14a49394c8db`。哈希证明采集内容身份，不代表其中每个断言已验证。模型权重、缓存、依赖、第三方源码树和未指定的历史会话未采集；大型文本仅做哈希并按需要选读。

- 本次整理基于明确引用的 README、制作方法、配置、提示词、关键源码与已有报告。未重新观看全部参考/候选/成片，也未做人工听辨。
- 项目未提供的原始模型内部实现、未披露生成后端、未记录 seed/参数、真实费用、人工总工时和用户最终验收，均不补造。实际日志有记录的耗时见上文，流水线耗时不等于整个制作总耗时。
- 源项目内的路径只用于查证；本档案未复制大型素材或模型，也不改变源项目运行依赖。

| 已盘点证据文件 | SHA-256（前 16 位）/状态 |
| --- | --- |
| `videos/unet-explainer/README.md` | `d2d62c8e39e15db4` |
| `videos/unet-explainer/package-lock.json` | `ffd8e35babad54d2` |
| `videos/unet-explainer/package.json` | `cf8cc020abb2801a` |
| `videos/unet-explainer/project.json` | `a029d3dd411f1ab2` |
| `videos/unet-explainer/remotion.config.ts` | `fe968ee4188f28ba` |
| `videos/unet-explainer/tsconfig.json` | `2cfb14dc584e98ab` |
| `videos/unet-explainer/outputs/timeline.json` | `51d86c3df61a7bf1` |
| `videos/unet-explainer/outputs/checks/archive-report.json` | `95a0fe7b7a0a84bf` |
| `videos/unet-explainer/outputs/checks/audio-check.json` | `c716068d15aa74cf` |
| `videos/unet-explainer/outputs/checks/final-media-report.json` | `fc318f4e009edc0a` |
| `videos/unet-explainer/outputs/checks/isolation-report.json` | `944d6446a36ce0bb` |
| `videos/unet-explainer/outputs/checks/keyframes-report.json` | `b75a02457fe2cf9e` |
| `videos/unet-explainer/outputs/checks/media-verification.json` | `eded007d395e840e` |
| `videos/unet-explainer/outputs/checks/migration-report.json` | `bb770c0e541ddea3` |
| `videos/unet-explainer/outputs/checks/playback-report.json` | `c1e63f5edda1f1e0` |
| `videos/unet-explainer/outputs/checks/static-review.json` | `9557aeaa0777a50b` |
| `videos/unet-explainer/public/fonts/misans-source.json` | `f675e829b466ffa8` |
| `videos/unet-explainer/public/unet/narration.json` | `ea682dfce3c1e65b` |
| `videos/unet-explainer/public/unet/audio/sentence-01.txt` | `a8d47aeea6d1a6bc` |
| `videos/unet-explainer/public/unet/audio/sentence-02.txt` | `670d397c3e09857a` |
| `videos/unet-explainer/public/unet/audio/sentence-03.txt` | `319d94f868772c37` |
| `videos/unet-explainer/public/unet/audio/sentence-04.txt` | `a689c37355702434` |
| `videos/unet-explainer/public/unet/audio/sentence-05.txt` | `6615be69d86b3838` |
| `videos/unet-explainer/public/unet/audio/sentence-06.txt` | `fab0597f85a2474b` |
| `videos/unet-explainer/public/unet/audio/sentence-07.txt` | `1260043c2ef43e83` |
| `videos/unet-explainer/public/unet/audio/sentence-08.txt` | `101a26bd78edaf48` |
| `videos/unet-explainer/public/unet/audio/sentence-09.txt` | `7a293d9ac9dfe69e` |
| `videos/unet-explainer/public/unet/audio/sentence-10.txt` | `2e90890799ff6fed` |
| `videos/unet-explainer/public/unet/audio/sentence-11.txt` | `43278ce8cfb88bed` |
| `videos/unet-explainer/public/unet/audio/sentence-12.txt` | `e721194d4f292b22` |
| `videos/unet-explainer/public/unet/audio/sentence-13.txt` | `b91edce970e8891a` |
| `videos/unet-explainer/public/unet/audio/sentence-14.txt` | `5d38c75469e070b9` |
| `videos/unet-explainer/public/unet/audio/sentence-15.txt` | `89071e68137a5ef5` |
| `videos/unet-explainer/public/unet/audio/sentence-16.txt` | `069d7ce51c976640` |
| `videos/unet-explainer/public/unet/audio/sentence-17.txt` | `538e78ee8c6b8b54` |
| `videos/unet-explainer/public/unet/audio/sentence-18.txt` | `0efcecbe01e0d5c5` |
| `videos/unet-explainer/public/unet/audio/sentence-19.txt` | `f1866ddc2f1953e6` |
| `videos/unet-explainer/public/unet/audio/sentence-20.txt` | `26e285b493d61d88` |
| `videos/unet-explainer/public/unet/audio/sentence-21.txt` | `8d8323f758f65ce8` |
| `videos/unet-explainer/public/unet/audio/sentence-22.txt` | `e51901f758bf6c65` |
| `videos/unet-explainer/public/unet/audio/sentence-23.txt` | `1de002ba6a273256` |
| `videos/unet-explainer/public/unet/audio/sentence-24.txt` | `2dabf71d7f930556` |
| `videos/unet-explainer/public/unet/audio/sentence-25.txt` | `7c3a5b722a5ac28f` |
| `videos/unet-explainer/public/unet/audio/sentence-26.txt` | `faf971ea5e0d78ee` |
| `videos/unet-explainer/public/unet/audio/sentence-27.txt` | `d5c5a73b038eb557` |
| `videos/unet-explainer/public/unet/audio/sentence-28.txt` | `0c9716f00078b435` |
| `videos/unet-explainer/public/unet/audio/sentence-29.txt` | `016ee133e907820c` |
| `videos/unet-explainer/public/unet/audio/sentence-30.txt` | `9ee8867cabe09110` |
| `videos/unet-explainer/public/unet/audio/sentence-31.txt` | `186de19c93582e4a` |
| `videos/unet-explainer/public/unet/audio/sentence-32.txt` | `9d4cb1adf294071c` |
| `videos/unet-explainer/scripts/check-isolation.mjs` | `f7d2e18736bae672` |
| `videos/unet-explainer/scripts/package-project.py` | `62fc9770e7659b40` |
| `videos/unet-explainer/scripts/prepare-unet-audio.py` | `397b30296a4ed34c` |
| `videos/unet-explainer/scripts/remotion.mjs` | `a1a87d7bd574cc39` |
| `videos/unet-explainer/scripts/render-unet-keyframes.mjs` | `dc48655645915333` |
| `videos/unet-explainer/scripts/render-unet-review.mjs` | `b849fbde9581d757` |
| `videos/unet-explainer/scripts/serve-unet.py` | `247f30a151ea2916` |
| `videos/unet-explainer/scripts/verify-media.mjs` | `3ba189cbc90be977` |
| `videos/unet-explainer/scripts/verify-unet.mjs` | `035578487946906a` |
| `videos/unet-explainer/src/Root.tsx` | `74f2bda5ce8de032` |
| `videos/unet-explainer/src/UnetExplainer.tsx` | `d921228b65cb4c96` |
| `videos/unet-explainer/src/fonts.ts` | `13ee436fdd06e832` |
| `videos/unet-explainer/src/index.ts` | `5462d3268136731c` |
| `videos/unet-explainer/src/unet/CaptionOverlay.tsx` | `c8386032c29e41fc` |
| `videos/unet-explainer/src/unet/ConcatDemo.tsx` | `b94b0cf3d03fcf84` |
| `videos/unet-explainer/src/unet/FeatureMapStack.tsx` | `fda28db42bcf1f1b` |
| `videos/unet-explainer/src/unet/Frame.tsx` | `9ad7861a4b04bd65` |
| `videos/unet-explainer/src/unet/SegmentationExample.tsx` | `ae0294a04f0c1c17` |
| `videos/unet-explainer/src/unet/TrainingDemo.tsx` | `f1c396cfe812687d` |
| `videos/unet-explainer/src/unet/UNetDiagram.tsx` | `9d06fc123dcfa523` |
| `videos/unet-explainer/src/unet/UnetStoryboard.tsx` | `2ac05577c471f88a` |
| `videos/unet-explainer/src/unet/motion.ts` | `0653d4a57953a011` |
| `videos/unet-explainer/src/unet/settings.ts` | `a328c8e46806b794` |
| `videos/unet-explainer/src/unet/timeline.generated.ts` | `72656179ea44f190` |
| `videos/unet-explainer/src/unet/types.ts` | `e8c9e11e360a0a81` |
| `videos/unet-explainer/src/unet/scenes/DecoderScene.tsx` | `2d9a53c1db9665b6` |
| `videos/unet-explainer/src/unet/scenes/EncoderScene.tsx` | `0c632198ec3a29a9` |
| `videos/unet-explainer/src/unet/scenes/OverviewScene.tsx` | `9b421b2ed0dfc933` |
| `videos/unet-explainer/src/unet/scenes/SkipScene.tsx` | `4cb2c703876385d7` |
| `videos/unet-explainer/src/unet/scenes/SummaryScene.tsx` | `1c55e4e53deea020` |
| `videos/unet-explainer/src/unet/scenes/TaskScene.tsx` | `c0247e9908ad948d` |
| `videos/unet-explainer/src/unet/scenes/TrainingScene.tsx` | `b64240faeb077ef8` |
