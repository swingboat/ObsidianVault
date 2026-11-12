---
type: video-production-dossier
project: video-creation
video_id: hello-remotion
captured: '2026-11-12'
status: 基础示例；已有 MP4
source_fingerprint: b48f74135417a5fdb13e41f2e4fcfdea72a43831465ae0792e1bec9c55986431
tags:
- video-creation
- production-capture
---

# HelloRemotion · 帧驱动合成基础

<!-- capture-source-sha256:b48f74135417a5fdb13e41f2e4fcfdea72a43831465ae0792e1bec9c55986431 -->

本次为指定项目的材料回溯与归档。制作结论来自项目记录/源码，未找回关闭的制作对话，也未重新执行生成、渲染、音频合成或用户验收。

## 当前结论与制作路线

**状态：基础示例；已有 MP4。**

React 布局 → 帧号插值 → Remotion 导出。

以 1280×720、30fps、90 帧建立基础 Composition。组件读取当前帧，计算淡入和进度，再生成每帧的画面。该视频是工程与合成链路示例，资料未显示采用 AI 图像或视频生成。

## 关键算法、实现与适用边界

`src/HelloRemotion.tsx` 将帧 0–20 映射为透明度 0–1，并用 `frame / (durationInFrames - 1)` 得到覆盖首尾帧的归一化进度。相同帧输入对应相同画面，适合验证预览和离线渲染的一致性。

## 声音与字幕

未发现独立配音或配乐制作说明。

## 失败、修正与验收边界

本项目材料未记录具体创作失败或用户审美验收。已有 smoke-test.mp4 只说明存在测试输出。

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
| `@types/react` | `19.2.7` | `19.2.7` |
| `@types/react-dom` | `19.2.3` | `19.2.3` |
| `typescript` | `5.9.3` | `5.9.3` |

工具底层模型未公开时按“未披露”记录；不能由 imagegen 工具名猜测图像模型。未提供真实推理日志的配置不记作已运行。

## 镜头、时间轴与采用参数

> [!info]- 当前 Composition / 时间轴源码：videos/hello-remotion/src/Root.tsx，L1–15
> ```tsx
> import {Composition} from 'remotion';
> import {HelloRemotion} from './HelloRemotion';
> 
> export const RemotionRoot = () => (
>   <>
>   <Composition
>     id="HelloRemotion"
>     component={HelloRemotion}
>     durationInFrames={90}
>     fps={30}
>     width={1280}
>     height={720}
>   />
>   </>
> );
> ```

源时间轴、生成帧数、所用区间与最终时长分别记录；历史文档或候选参数不自动覆盖当前采用配置。

## 参考来源与借鉴说明

在本次盘点的项目自有材料中，未找到可明确归属本片的外部参考视频或 X 帖子链接；这不等于历史上没有参考。


## 重现入口与修改位置

以下是 package.json 中的实际命令映射，供在对应视频目录使用；本轮未执行这些制作命令。系统依赖、前置模型/素材和环境准备见后附 README。

| npm 入口 | 实际命令 |
| --- | --- |
| `npm run dev` | `node scripts/remotion.mjs studio src/index.ts --no-open --port=3212` |
| `npm run compositions` | `node scripts/remotion.mjs compositions src/index.ts` |
| `npm run typecheck` | `tsc --noEmit` |
| `npm run render` | `node scripts/remotion.mjs render src/index.ts HelloRemotion outputs/hello-remotion.mp4 --codec=h264 --audio-codec=aac --pixel-format=yuv420p --concurrency=2` |
| `npm run still` | `node scripts/remotion.mjs still src/index.ts HelloRemotion outputs/checks/current.png --frame=45` |
| `npm run check` | `npm run typecheck && node scripts/check-isolation.mjs` |
| `npm run pack` | `python3 scripts/package-project.py` |
| `npm run test` | `npm run typecheck && node scripts/smoke-test.mjs` |
| `npm run verify` | `node scripts/verify-media.mjs` |

### 算法源码摘录

> [!info]- videos/hello-remotion/src/HelloRemotion.tsx，L1–38
> ```tsx
> import {AbsoluteFill, interpolate, useCurrentFrame, useVideoConfig} from 'remotion';
> 
> export const HelloRemotion = () => {
>   const frame = useCurrentFrame();
>   const {durationInFrames} = useVideoConfig();
>   const opacity = interpolate(frame, [0, 20], [0, 1], {
>     extrapolateLeft: 'clamp',
>     extrapolateRight: 'clamp',
>   });
>   const progress = frame / (durationInFrames - 1);
> 
>   return (
>     <AbsoluteFill
>       style={{
>         backgroundColor: '#111827',
>         color: '#f9fafb',
>         fontFamily: 'Arial, sans-serif',
>         alignItems: 'center',
>         justifyContent: 'center',
>       }}
>     >
>       <div style={{opacity, fontSize: 76, fontWeight: 700}}>Hello, Remotion!</div>
>       <div style={{opacity, marginTop: 24, fontSize: 30, color: '#93c5fd'}}>
>         Project-local video rendering is ready.
>       </div>
>       <div
>         style={{
>           position: 'absolute',
>           bottom: 0,
>           left: 0,
>           height: 12,
>           width: `${progress * 100}%`,
>           backgroundColor: '#60a5fa',
>         }}
>       />
>     </AbsoluteFill>
>   );
> };
> ```

## 完整关键提示词与生成记录

以下保留项目自有提示词、候选参数与版本。存在失败候选和历史参数；最终采用以 selected-takes、edit-decisions 与制作复盘为准，不能将所有候选都写成已采用。

未发现实际生成提示词或推理执行记录；不从脚手架、模型安装清单推测生成过程。

## 已有产物与检查依据

文件存在、技术检查、画面检查与用户验收分别看待。下表是本次文件盘点，旧版与当前版可能共存。MP4/HTML 按内容计算 SHA-256，ZIP 仅记录文件大小和修改时间供增量判断。

| 本地产物 | 字节数 | SHA-256 |
| --- | ---: | --- |
| `videos/hello-remotion/outputs/hello-remotion-project.zip` | 232617 | `ZIP：仅元数据` |
| `videos/hello-remotion/outputs/hello-remotion.mp4` | 31177 | `e27c7d0a84fd515f8f003f336a1d889f8fcf3e8190eef3e68a8e010029b511b5` |
| `videos/hello-remotion/outputs/smoke-test.mp4` | 31177 | `e27c7d0a84fd515f8f003f336a1d889f8fcf3e8190eef3e68a8e010029b511b5` |

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
>       "duration": "3.000000",
>       "nb_frames": "90"
>     }
>   ],
>   "format": {
>     "duration": "3.000000",
>     "size": "31177"
>   },
>   "file": "outputs/hello-remotion.mp4",
>   "scope": "本次只读取媒体头；未全片解码、播放或重新进行视觉/听觉验收。"
> }
> ```


已有检查报告路径（未在本轮重新执行）：

- `videos/hello-remotion/outputs/checks/archive-report.json`
- `videos/hello-remotion/outputs/checks/isolation-report.json`
- `videos/hello-remotion/outputs/checks/media-verification.json`
- `videos/hello-remotion/outputs/checks/migration-report.json`

## 制作方法与迭代记录选编

下面是项目自有文档的归档摘录，保留原阶段与原检查表述，个人路径已泛化、相对链接已注明源项目位置。它们属于来源陈述，不代表本次重新完成其中的生成或验收。

> [!info]- 来源文档：videos/hello-remotion/README.md
> # HelloRemotion
> 
> 独立 Remotion 视频项目：1280×720，30 fps，3 秒。仅包含本视频的组件、素材与脚本。
> 
> ```bash
> npm ci
> npm run dev
> npm run typecheck
> npm run check
> npm run still
> npm run render
> npm run verify
> npm run pack
> ```
> 
> 预览地址：http://localhost:3212/HelloRemotion。最终视频输出至 `outputs/hello-remotion.mp4`；缓存和临时文件位于 `.cache/`。本目录的 `package-lock.json` 锁定依赖，npm 缓存和浏览器均在本目录内。项目不能依赖其他视频或上层工作区文件。
> 
> 修改 `src/HelloRemotion.tsx` 及本视频的子组件、`public/` 素材；Composition 注册与时长在 `src/Root.tsx`。
> 
> 已保留原有 MP4。

## 证据清单与缺失信息

盘点的文本/报告文件 19 个，源文本、报告及媒体产物身份摘要为 `b48f74135417a5fdb13e41f2e4fcfdea72a43831465ae0792e1bec9c55986431`。哈希证明采集内容身份，不代表其中每个断言已验证。模型权重、缓存、依赖、第三方源码树和未指定的历史会话未采集；大型文本仅做哈希并按需要选读。

- 本次整理基于明确引用的 README、制作方法、配置、提示词、关键源码与已有报告。未重新观看全部参考/候选/成片，也未做人工听辨。
- 项目未提供的原始模型内部实现、未披露生成后端、未记录 seed/参数、真实费用、人工总工时和用户最终验收，均不补造。实际日志有记录的耗时见上文，流水线耗时不等于整个制作总耗时。
- 源项目内的路径只用于查证；本档案未复制大型素材或模型，也不改变源项目运行依赖。

| 已盘点证据文件 | SHA-256（前 16 位）/状态 |
| --- | --- |
| `videos/hello-remotion/README.md` | `58e6122e64f96b2b` |
| `videos/hello-remotion/package-lock.json` | `de72d92bb22cf9d9` |
| `videos/hello-remotion/package.json` | `5181935c13537ba6` |
| `videos/hello-remotion/project.json` | `9fb2918b56707c19` |
| `videos/hello-remotion/remotion.config.ts` | `fe968ee4188f28ba` |
| `videos/hello-remotion/tsconfig.json` | `2cfb14dc584e98ab` |
| `videos/hello-remotion/outputs/smoke-test-report.json` | `1fe69d47378aeaff` |
| `videos/hello-remotion/outputs/checks/archive-report.json` | `4ba0f0fc29297e6d` |
| `videos/hello-remotion/outputs/checks/isolation-report.json` | `e0a5cc3805c7015e` |
| `videos/hello-remotion/outputs/checks/media-verification.json` | `e4270492765416b1` |
| `videos/hello-remotion/outputs/checks/migration-report.json` | `84c8d3fb4203f822` |
| `videos/hello-remotion/scripts/check-isolation.mjs` | `f7d2e18736bae672` |
| `videos/hello-remotion/scripts/package-project.py` | `62fc9770e7659b40` |
| `videos/hello-remotion/scripts/remotion.mjs` | `a1a87d7bd574cc39` |
| `videos/hello-remotion/scripts/smoke-test.mjs` | `01d512f5d3eb6c1e` |
| `videos/hello-remotion/scripts/verify-media.mjs` | `3ba189cbc90be977` |
| `videos/hello-remotion/src/HelloRemotion.tsx` | `9619d7b220a3800b` |
| `videos/hello-remotion/src/Root.tsx` | `d61a53d667394bb2` |
| `videos/hello-remotion/src/index.ts` | `5462d3268136731c` |
