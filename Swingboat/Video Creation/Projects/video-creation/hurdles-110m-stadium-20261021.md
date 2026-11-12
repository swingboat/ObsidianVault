---
type: video-production-dossier
project: video-creation
video_id: hurdles-110m-stadium-20261021
captured: '2026-11-12'
status: 主验证片 2.75 秒；v6/v7 为静帧外观候选
source_fingerprint: efe218da8b35912e576009556c066adb52febcf91fb8aed0225c388116ad575b
tags:
- video-creation
- production-capture
---

# 110 米栏 · 本地代码动画与人物写实迭代

<!-- capture-source-sha256:efe218da8b35912e576009556c066adb52febcf91fb8aed0225c388116ad575b -->

本次为指定项目的材料回溯与归档。制作结论来自项目记录/源码，未找回关闭的制作对话，也未重新执行生成、渲染、音频合成或用户验收。

## 当前结论与制作路线

**状态：主验证片 2.75 秒；v6/v7 为静帧外观候选。**

程序轨迹与 IK → 骨骼矩阵 → Blender Cycles CPU → MP4/内嵌 HTML。

当前主交付为 66 帧、24fps、1280×720 的正常速度验证片，包含两次过栏与继续跑动。源程序动作导出 163 骨骼矩阵，原数据有 120 帧，实际采用片段与最终长度不同。v6/v7 人物研究没有替换 v5 主动画。

## 关键算法、实现与适用边界

src/app.js 用跑动轨迹、关键姿态和肢体 IK 计算动作；build-offline.py 转为 Blender 原生动画，再由 Cycles 逐帧渲染。角色基础网格/纹理来自 MakeHuman，HDRI 为环境照明资产，不能把动态制作描述为所有素材从零创建。骨骼数据不是专业运动员动捕。

## 声音与字幕

当前主验证片无旁白或音轨。

## 失败、修正与验收边界

Metal 两次测试未正常推进，主路线改 CPU；首版运动模糊与程序步态切换产生抹影，后续关闭并从 60 帧补至 66 帧。v6 扫描面部与身体连接仍有问题；v7 Rocketbox 完整免费人物改善衔接，但未达到真人实拍目标，不能把外观候选称为已验收动作成片。

## 技术与模型版本

以下是本项目声明或锁定的依赖，安装清单本身不证明某项参与了最终画面。具体用途以制作路线、源码和生成记录交叉判断。

| 依赖 | package.json 声明 | 可解析的直接锁定版本 |
| --- | --- | --- |
| `@remotion/cli` | `4.0.361` | `4.0.361` |
| `@remotion/media` | `4.0.361` | `4.0.361` |
| `react` | `19.0.0` | `19.0.0` |
| `react-dom` | `19.0.0` | `19.0.0` |
| `remotion` | `4.0.361` | `4.0.361` |
| `three` | `0.170.0` | `0.170.0` |
| `@types/react` | `19.0.3` | `19.0.3` |
| `esbuild` | `0.25.0` | `0.25.0` |
| `typescript` | `5.7.3` | `5.7.3` |

工具底层模型未公开时按“未披露”记录；不能由 imagegen 工具名猜测图像模型。未提供真实推理日志的配置不记作已运行。

## 镜头、时间轴与采用参数

> [!info]- 当前 Composition / 时间轴源码：videos/hurdles-110m-stadium-20261021/src/remotion.tsx，L1–20
> ```tsx
> import React,{useEffect,useMemo,useRef,useState} from 'react';
> import {AbsoluteFill,Composition,continueRender,delayRender,registerRoot,staticFile,useCurrentFrame,useVideoConfig} from 'remotion';
> import {Video} from '@remotion/media';
> type HurdleWindow=Window & {hurdleApp?:{ready:boolean;setPlaying:(p:boolean)=>void;setTime:(t:number)=>void;setCamera:(m:string)=>void;getState:()=>unknown}};
> const HurdleScene:React.FC=()=>{
>  const frame=useCurrentFrame();const {fps}=useVideoConfig();const ref=useRef<HTMLIFrameElement>(null);const [loaded,setLoaded]=useState(false);
>  const handle=useMemo(()=>delayRender('Rendering stadium frame '+frame),[frame]);
>  useEffect(()=>{if(!loaded)return;const w=ref.current?.contentWindow as HurdleWindow|null;if(!w)return;
>   const draw=()=>{if(!w.hurdleApp?.ready)return false;w.hurdleApp.setPlaying(false);w.hurdleApp.setCamera('cinematic');w.hurdleApp.setTime(frame/fps);w.document.body.classList.add('clean');w.document.querySelector('#loading')?.remove();continueRender(handle);return true;};
>   if(draw())return;const id=setInterval(()=>{if(draw())clearInterval(id);},30);return()=>clearInterval(id);
>  },[frame,fps,loaded,handle]);
>  return <AbsoluteFill style={{backgroundColor:'#c7d8d7'}}><iframe ref={ref} title="110m hurdles" src={staticFile('hurdle-110-3d.html')+'?paused=1&t=0&clean=1&render=1'} onLoad={()=>setLoaded(true)} style={{width:'100%',height:'100%',border:0}}/></AbsoluteFill>;
> };
> const CGScene:React.FC=()=> <AbsoluteFill style={{backgroundColor:'#202b22'}}>
>  <Video src={staticFile('assets/offline/hurdles-cycles.mp4')} playbackRate={1} muted style={{width:'100%',height:'100%',objectFit:'contain'}}/>
>  </AbsoluteFill>;
> const Root:React.FC=()=> <>
>  <Composition id="HurdleCG" component={CGScene} durationInFrames={66} fps={24} width={1280} height={720}/>\n <Composition id="Hurdle110" component={HurdleScene} durationInFrames={660} fps={30} width={1920} height={1080}/>
>  </>;
> registerRoot(Root);
> ```

源时间轴、生成帧数、所用区间与最终时长分别记录；历史文档或候选参数不自动覆盖当前采用配置。

## 参考来源与借鉴说明

- 历史真人视频：[Male Athlete Hurdling on Outdoor Track](https://www.pexels.com/video/male-athlete-hurdling-on-outdoor-track-30997018/)，作者 Mharee Lynn Eulanda Guillena，Pexels。2026-11-12 本次打开官方页面可读取作者署名，页面现标题概述为运动员在室外跑道跨栏；未下载或重新观看完整媒体。
- 本地来源：`docs/LIVE-ACTION.md`、`docs/ASSET-CREDITS.md`，历史原片路径 `sources/live-action/hurdle-30997018-original.mp4`。该来源在旧版被直接剪辑使用，后来退出主交付；不能将旧真人画面记成本项目生成动画。
- 项目历史记录规格：源片 1080×1920、约 12.86 秒、27.92fps；采用约 6.15–12.35 秒，输出 6.20 秒、30fps，保持竖屏且不作生成式补帧。这些区间与规格来自本地编辑记录，并非本次新测量。
- 当前主路线为 v5 本地代码/Cycles 动画，v6/v7 仅人物外观研究；旧真人参考入口保留在备份/对照文件。许可按项目保存的来源记录查阅，本次只核对署名，不重新作许可判断。

### 项目记录的技术、内容或素材来源

这些 URL 来自项目文件，尚不自动证明本片采用；除本节明确标注的访问外，本轮未逐一在线重读。

| 链接 | 记录文件 |
| --- | --- |
| [github.com](https://github.com/microsoft/Microsoft-Rocketbox/tree/master/Assets/Avatars/Professions/Sports_Male_04) | `videos/hurdles-110m-stadium-20261021/docs/ASSET-CREDITS.md` |
| [renderpeople.com](https://renderpeople.com/general-terms-and-conditions/) | `videos/hurdles-110m-stadium-20261021/docs/ASSET-CREDITS.md` |
| [renderpeople.com](https://renderpeople.com/free-3d-people/) | `videos/hurdles-110m-stadium-20261021/docs/ASSET-CREDITS.md` |
| [polyhaven.com](https://polyhaven.com/a/stadium_01) | `videos/hurdles-110m-stadium-20261021/docs/ASSET-CREDITS.md` |
| [polyhaven.com](https://polyhaven.com/license) | `videos/hurdles-110m-stadium-20261021/docs/ASSET-CREDITS.md` |
| [www.pexels.com](https://www.pexels.com/video/male-athlete-hurdling-on-outdoor-track-30997018/) | `videos/hurdles-110m-stadium-20261021/docs/ASSET-CREDITS.md` |
| [www.pexels.com](https://www.pexels.com/license/) | `videos/hurdles-110m-stadium-20261021/docs/ASSET-CREDITS.md` |
| [static.makehumancommunity.org](https://static.makehumancommunity.org/assets/assetpacks/makehuman_system_assets.html) | `videos/hurdles-110m-stadium-20261021/docs/ASSET-CREDITS.md` |
| [files.makehumancommunity.org](https://files.makehumancommunity.org/asset_packs/makehuman_system_assets/makehuman_system_assets_cc0.zip) | `videos/hurdles-110m-stadium-20261021/docs/ASSET-CREDITS.md` |
| [github.com](https://github.com/makehumancommunity/makehuman) | `videos/hurdles-110m-stadium-20261021/docs/ASSET-CREDITS.md` |
| [github.com](https://github.com/mrdoob/three.js/tree/r170/examples/models/gltf/LeePerrySmith) | `videos/hurdles-110m-stadium-20261021/docs/ASSET-CREDITS.md` |
| [casual-effects.com](https://casual-effects.com/data/) | `videos/hurdles-110m-stadium-20261021/docs/ASSET-CREDITS.md` |
| [creativecommons.org](https://creativecommons.org/licenses/by/3.0/) | `videos/hurdles-110m-stadium-20261021/docs/ASSET-CREDITS.md` |
| [worldathletics.org](https://worldathletics.org/responsive/download/downloaddirect?filename=7d39b677-8043-45f4-89f4-6c8814023057.pdf&urlslug=Track%2Band%2BField%2BFacilities%2BManual%2B2019%2BEdition%2B-%2BChapters%2B1-3) | `videos/hurdles-110m-stadium-20261021/docs/ASSET-CREDITS.md` |
| [videos.pexels.com](https://videos.pexels.com/video-files/30997018/13249548_1080_1920_28fps.mp4) | `videos/hurdles-110m-stadium-20261021/docs/LIVE-ACTION.md` |
| [help.runwayml.com](https://help.runwayml.com/hc/en-us/articles/46974685288467-Creating-with-Gen-4-5) | `videos/hurdles-110m-stadium-20261021/docs/ORIGINAL-AI-VIDEO.md` |
| [academy.runwayml.com](https://academy.runwayml.com/guides/prompting-guide) | `videos/hurdles-110m-stadium-20261021/docs/ORIGINAL-AI-VIDEO.md` |
| [docs.dev.runwayml.com](https://docs.dev.runwayml.com/guides/using-the-api/) | `videos/hurdles-110m-stadium-20261021/docs/ORIGINAL-AI-VIDEO.md` |
| [renderpeople.com](https://renderpeople.com/sample/free/renderpeople_free_rigged_people_FBX.zip) | `videos/hurdles-110m-stadium-20261021/docs/REALISM-V6.md` |
| [renderpeople.com](https://renderpeople.com/3d-people/mason-rigged-007/) | `videos/hurdles-110m-stadium-20261021/docs/REALISM-V6.md` |
| [github.com](https://github.com/microsoft/Microsoft-Rocketbox) | `videos/hurdles-110m-stadium-20261021/docs/REALISM-V7-FREE.md` |

## 重现入口与修改位置

以下是 package.json 中的实际命令映射，供在对应视频目录使用；本轮未执行这些制作命令。系统依赖、前置模型/素材和环境准备见后附 README。

| npm 入口 | 实际命令 |
| --- | --- |
| `npm run build` | `node scripts/task.mjs build` |
| `npm run preview` | `node scripts/task.mjs preview` |
| `npm run check` | `node scripts/task.mjs check` |
| `npm run studio` | `node scripts/task.mjs studio` |
| `npm run render` | `node scripts/task.mjs render` |
| `npm run render:3d` | `node scripts/task.mjs render:3d` |
| `npm run build:scene` | `node scripts/run-offline.mjs build` |
| `npm run render:scene` | `node scripts/run-offline.mjs frames 1 66 24` |
| `npm run encode:scene` | `python3 scripts/encode-offline.py` |
| `npm run assets:lookdev` | `python3 scripts/fetch-scan-v6.py` |
| `npm run build:lookdev` | `node scripts/run-realism-v6.mjs build` |
| `npm run render:lookdev` | `node scripts/render-lookdev-v6.mjs` |
| `npm run review:lookdev` | `node scripts/build-lookdev-v6.mjs` |
| `npm run check:lookdev` | `node scripts/check-lookdev-v6.mjs` |
| `npm run assets:free` | `python3 scripts/fetch-free-v7.py` |
| `npm run build:free` | `node scripts/run-free-v7.mjs build` |
| `npm run render:free` | `node scripts/render-free-v7.mjs` |
| `npm run review:free` | `node scripts/build-review-v7.mjs` |
| `npm run check:free` | `node scripts/check-free-v7.mjs` |

### 算法源码摘录

> [!info]- videos/hurdles-110m-stadium-20261021/scripts/encode-offline.py，L1–15
> ```python
> from pathlib import Path
> import os,subprocess,json,hashlib,shutil
> root=Path.cwd();tmp=root/'.cache/tmp';tmp.mkdir(parents=True,exist_ok=True);os.environ.update(TMPDIR=str(tmp),TMP=str(tmp),TMP_DIR=str(tmp),TEMP=str(tmp))
> frames=root/'outputs/offline/frames';out=root/'public/assets/offline';out.mkdir(parents=True,exist_ok=True)
> expected=[frames/f'frame_{i:04d}.png' for i in range(1,67)]
> missing=[str(p) for p in expected if not p.exists()]
> if missing:raise SystemExit(f'{len(missing)} rendered frames are missing. Run npm run render:scene first.')
> def run(*args):subprocess.run(['ffmpeg','-hide_banner','-loglevel','error',*map(str,args),'-y'],check=True)
> run('-framerate',24,'-start_number',1,'-i',frames/'frame_%04d.png','-frames:v',66,'-an','-c:v','libx264','-preset','slow','-crf',17,'-pix_fmt','yuv420p','-movflags','+faststart',out/'hurdles-cycles.mp4')
> for name,f in [('poster',35),('approach',28),('flight',35),('landing',40)]:
>  run('-i',frames/f'frame_{f:04d}.png','-frames:v',1,'-q:v',2,out/f'{name}.jpg')
> shutil.copyfile(out/'hurdles-cycles.mp4',root/'outputs/hurdle-110-code.mp4')
> metadata={'renderSamples':24,'motionBlur':False,'backend':'CPU','frameHashes':{p.name:hashlib.sha256(p.read_bytes()).hexdigest() for p in expected},'buildScriptHash':hashlib.sha256((root/'scripts/build-offline.py').read_bytes()).hexdigest(),'source':'Blender Cycles local code render','frames':66,'fps':24,'duration':2.75,'size':[1280,720],'videoSource':'No stock footage; no AI video generation service','motion':'Authored trajectory and two-segment IK','ingredients':['MakeHuman CC0 body and photo textures','Procedural garments, short hair, track and hurdles','Poly Haven Stadium 01 CC0 environment panorama'],'limitations':'Programmed movement and a generic base mesh still look computer-generated; no claim of live-action parity.'}
> (out/'provenance.json').write_text(json.dumps(metadata,ensure_ascii=False,indent=2))
> print('Encoded 66 locally rendered frames into a 2.75-second clip.')
> ```

> [!info]- videos/hurdles-110m-stadium-20261021/scripts/build-free-v7.py，L1–117
> ```python
> """A complete free athlete candidate, preserving the authored head/body/clothes rig."""
> import bpy,math,json,sys,random,bisect
> from pathlib import Path
> from mathutils import Matrix,Vector,Quaternion
> P=Path.cwd();O=P/'outputs/realism-v7';O.mkdir(parents=True,exist_ok=True)
> C=P/'outputs/checks/realism-v7';C.mkdir(parents=True,exist_ok=True)
> S=P/'sources/research/realism-v7/rocketbox'
> args=sys.argv[sys.argv.index('--')+1:] if '--' in sys.argv else [];mode=args[0] if args else 'build'
> 
> def look(o,target):o.rotation_euler=(Vector(target)-o.location).to_track_quat('-Z','Y').to_euler()
> def rotate(arm,name,axis,degrees):
>  pb=arm.pose.bones[name];local=(arm.matrix_world.to_3x3()@pb.bone.matrix_local.to_3x3()).inverted()@Vector(axis);pb.rotation_mode='QUATERNION';pb.rotation_quaternion=Quaternion(local.normalized(),math.radians(degrees))
> def camera(s,kind):
>  c=s.camera;c.animation_data_clear();c.data.dof.use_dof=True;c.data.dof.aperture_fstop=5.6;c.data.sensor_fit='VERTICAL';c.data.sensor_height=36
>  f=bpy.data.objects['Athlete focus'];f.animation_data_clear()
>  if kind=='portrait':f.location=(0,-4.30,1.69);c.location=(.30,-6.02,1.74);c.data.lens=128
>  elif kind=='half':f.location=(0,-4.30,1.37);c.location=(.82,-7.54,1.61);c.data.lens=94
>  else:f.location=(0,-4.30,1.00);c.location=(1.5,-9.95,1.53);c.data.lens=89
>  c.data.dof.focus_object=f;look(c,f.location)
>  s.render.resolution_x=1200;s.render.resolution_y=1500;s.render.resolution_percentage=int(args[2]) if len(args)>2 else 100
> 
> if mode!='build':
>  bpy.ops.wm.open_mainfile(filepath=str(O/'complete-free-athlete.blend'));s=bpy.context.scene;s.cycles.device='CPU'
>  if mode=='flex':
>   arm=next(o for o in s.objects if o.type=='ARMATURE')
>   rotate(arm,'Bip01 L Thigh',(1,0,0),-48);rotate(arm,'Bip01 L Calf',(1,0,0),86)
>   rotate(arm,'Bip01 L UpperArm',(0,1,0),31);rotate(arm,'Bip01 L Forearm',(1,0,0),-45)
>   rotate(arm,'Bip01 R UpperArm',(0,1,0),-31);rotate(arm,'Bip01 R Forearm',(1,0,0),-45)
>   bpy.context.view_layer.update()
>  camera(s,mode);s.cycles.samples=int(args[1]) if len(args)>1 else 64
>  s.render.filepath=str(O/(mode+'.png'));bpy.ops.render.render(write_still=True);sys.exit(0)
> 
> # Retain the established physical track, HDRI, sun, and camera; replace all old character objects.
> bpy.ops.wm.open_mainfile(filepath=str(P/'outputs/realism-v6/baseline.blend'))
> s=bpy.context.scene;s.frame_set(1);s.cycles.device='CPU';s.render.use_motion_blur=False
> oldstage=bpy.data.objects['Character stage'];remove=set(oldstage.children_recursive)|{oldstage}
> for o in list(remove):bpy.data.objects.remove(o,do_unlink=True)
> assert not any(o.type=='ARMATURE' for o in s.objects),'Old armature remains'
> before=set(bpy.data.objects);bpy.ops.import_scene.fbx(filepath=str(S/'Sports_Male_04.fbx'))
> added=[o for o in bpy.data.objects if o not in before];arm=next(o for o in added if o.type=='ARMATURE');mesh=next(o for o in added if o.type=='MESH')
> arm.animation_data_clear()
> for pb in arm.pose.bones:pb.matrix_basis=Matrix.Identity(4)
> bpy.context.view_layer.update()
> # Uniformly normalize the complete asset without changing its proportions or internal transforms.
> corners=[mesh.matrix_world@Vector(v) for v in mesh.bound_box];low=min(v.z for v in corners);height=max(v.z for v in corners)-low
> stage=bpy.data.objects.new('Complete free athlete stage',None);bpy.context.collection.objects.link(stage)
> for o in added:
>  if o.parent in added:continue
>  old=o.matrix_world.copy();o.parent=stage;o.matrix_world=old
> stage.scale=(1.88/height,)*3;stage.location=(0,-4.27,-low*stage.scale.z+.002)
> rotate(arm,'Bip01 L UpperArm',(0,1,0),23);rotate(arm,'Bip01 R UpperArm',(0,1,0),-23)
> rotate(arm,'Bip01 L Forearm',(1,0,0),-6);rotate(arm,'Bip01 R Forearm',(1,0,0),-6)
> def shift_face(name,delta):
>  pb=arm.pose.bones[name];pb.location=(arm.matrix_world.to_3x3()@pb.bone.matrix_local.to_3x3()).inverted()@Vector(delta)
> for side in ['L','R']:shift_face('Bip01 '+side+'EyeBlinkTop',(0,0,-.0011))
> for name in ['Bip01 MBottomLip','Bip01 LMouthBottom','Bip01 RMouthBottom']:shift_face(name,(0,0,.0008))
> bpy.context.view_layer.update()
> 
> def texture(nt,part,kind):
>  t=nt.nodes.new('ShaderNodeTexImage');t.image=bpy.data.images.load(str(S/f'm026_{part}_{kind}.tga'),check_existing=True)
>  if kind!='color':t.image.colorspace_settings.name='Non-Color'
>  return t
> for slot,part in zip(mesh.material_slots,['body','head']):
>  m=bpy.data.materials.new('V7 native '+part);m.use_nodes=True;slot.material=m
>  nt=m.node_tree;n=nt.nodes;l=nt.links;p=n.get('Principled BSDF')
>  col=texture(nt,part,'color');l.new(col.outputs['Color'],p.inputs['Base Color'])
>  norm=texture(nt,part,'normal');nm=n.new('ShaderNodeNormalMap');nm.inputs['Strength'].default_value=.7;l.new(norm.outputs['Color'],nm.inputs['Color']);l.new(nm.outputs['Normal'],p.inputs['Normal'])
>  spec=texture(nt,part,'specular');r=n.new('ShaderNodeMapRange');r.inputs['To Min'].default_value=.82 if part=='body' else .69;r.inputs['To Max'].default_value=.38 if part=='body' else .32;l.new(spec.outputs['Color'],r.inputs['Value']);l.new(r.outputs['Result'],p.inputs['Roughness'])
>  p.inputs['Specular IOR Level'].default_value=.25;p.inputs['IOR'].default_value=1.4
>  if part=='head':p.inputs['Subsurface Weight'].default_value=.055;p.inputs['Subsurface Radius'].default_value=(1,.42,.22);p.inputs['Subsurface Scale'].default_value=.0015
>  else:p.inputs['Sheen Weight'].default_value=.12;p.inputs['Sheen Roughness'].default_value=.8
> eye_mat=mesh.material_slots[1].material.copy();eye_mat.name='V7 native wet eyes';ep=eye_mat.node_tree.nodes.get('Principled BSDF')
> for link in list(ep.inputs['Roughness'].links):eye_mat.node_tree.links.remove(link)
> ep.inputs['Roughness'].default_value=.09;ep.inputs['Coat Weight'].default_value=.75;ep.inputs['Coat Roughness'].default_value=.03;ep.inputs['Subsurface Weight'].default_value=0
> mesh.data.materials.append(eye_mat);eye_ids={g.index for g in mesh.vertex_groups if g.name in ['Bip01 LEye','Bip01 REye']};eye_faces=0
> for f in mesh.data.polygons:
>  weight=sum(sum(g.weight for g in mesh.data.vertices[i].groups if g.group in eye_ids) for i in f.vertices)/len(f.vertices)
>  if weight>.55:f.material_index=2;eye_faces+=1
>  f.use_smooth=True
> sub=mesh.modifiers.new('Smooth authored surface','SUBSURF');sub.levels=2;sub.render_levels=2
> 
> # Short fibers follow the original scalp rather than a generic helmet mesh.
> # Texture/position sampling excludes eyebrows and retains the authored native haircut.
> head_img=bpy.data.images.load(str(S/'m026_head_color.tga'),check_existing=True);W,H=head_img.size;pixels=list(head_img.pixels);uv=mesh.data.uv_layers.active.data
> def lum(u,v):
>  x=min(W-1,max(0,int(u*W)));y=min(H-1,max(0,int(v*H)));i=(y*W+x)*4;return sum(pixels[i:i+3])/3
> triangles=[];total=0
> for poly in mesh.data.polygons:
>  if poly.material_index!=1:continue
>  ids=list(poly.vertices);loops=list(poly.loop_indices)
>  for j in range(1,len(ids)-1):
>   vi=[ids[0],ids[j],ids[j+1]];li=[loops[0],loops[j],loops[j+1]];vs=[mesh.matrix_world@mesh.data.vertices[i].co for i in vi];us=[uv[i].uv.copy() for i in li]
>   # World surface before stage scaling: scalp back/top, no eyebrows.
>   if min(v.z for v in vs)<1.735*1.88/1.804-0.025:continue
>   if lum(sum(u.x for u in us)/3,sum(u.y for u in us)/3)>.22:continue
>   area=(vs[1]-vs[0]).cross(vs[2]-vs[0]).length/2
>   if area>0:total+=area;triangles.append((total,vs,us))
> cur=bpy.data.curves.new('V7 short hair fibers','CURVE');cur.dimensions='3D';cur.resolution_u=1;cur.bevel_depth=.00007;cur.bevel_resolution=0
> hm=bpy.data.materials.new('V7 matte dark brown hair');hm.use_nodes=True;hp=hm.node_tree.nodes.get('Principled BSDF');hp.inputs['Base Color'].default_value=(.026,.016,.011,1);hp.inputs['Roughness'].default_value=.64;cur.materials.append(hm)
> rng=random.Random(1107);count=0
> if triangles:
>  weights=[t[0] for t in triangles]
>  for _ in range(7000):
>   _,vs,us=triangles[bisect.bisect_left(weights,rng.random()*total)];a=rng.random();b=rng.random()
>   if a+b>1:a=1-a;b=1-b
>   root=vs[0]*(1-a-b)+vs[1]*a+vs[2]*b;tex=us[0]*(1-a-b)+us[1]*a+us[2]*b
>   if lum(tex.x,tex.y)>.24:continue
>   normal=(vs[1]-vs[0]).cross(vs[2]-vs[0]).normalized();length=rng.uniform(.001,.003);sp=cur.splines.new('POLY');sp.points.add(2)
>   for j,p in enumerate(sp.points):
>    t=j/2;v=root+normal*(.0003+length*t)+Vector((.001*t*t,.004*t*t,0));p.co=(*v,1)
>   count+=1
> hair=bpy.data.objects.new('V7 original scalp fine hair',cur);bpy.context.collection.objects.link(hair)
> # Keep fibers in world space for static look development. Dynamic hair is not claimed validated.
> s.cycles.samples=64;s.cycles.adaptive_threshold=.025;s.cycles.max_bounces=8;s.cycles.use_denoising=True
> camera(s,'portrait');bpy.ops.file.pack_all();bpy.ops.wm.save_as_mainfile(filepath=str(O/'complete-free-athlete.blend'))
> report={'asset':'Microsoft Rocketbox Sports_Male_04','license':'MIT','cost':0,'completeMatchedCharacter':True,'vertices':len(mesh.data.vertices),'bones':len(arm.data.bones),'textureSize':list(head_img.size),'eyeFaces':eye_faces,'hairFibers':count,'sourceHeight':height,'renderHeight':1.88,'motionValidated':False,'photorealismAccepted':False}
> (C/'build-report.json').write_text(json.dumps(report,indent=2));print('FREE_ATHLETE_BUILT',json.dumps(report))
> ```

## 完整关键提示词与生成记录

以下保留项目自有提示词、候选参数与版本。存在失败候选和历史参数；最终采用以 selected-takes、edit-decisions 与制作复盘为准，不能将所有候选都写成已采用。

未发现实际生成提示词或推理执行记录；不从脚手架、模型安装清单推测生成过程。

## 已有产物与检查依据

文件存在、技术检查、画面检查与用户验收分别看待。下表是本次文件盘点，旧版与当前版可能共存。MP4/HTML 按内容计算 SHA-256，ZIP 仅记录文件大小和修改时间供增量判断。

| 本地产物 | 字节数 | SHA-256 |
| --- | ---: | --- |
| `videos/hurdles-110m-stadium-20261021/outputs/hurdle-110-3d.html` | 11451476 | `11659b2eb8fd1885ceb8e54fd7193e1bc89f41ff1c5c549bf74d38fc61bcdb7f` |
| `videos/hurdles-110m-stadium-20261021/outputs/hurdle-110-code.mp4` | 1307066 | `e5f7ea58b9c660ab88dd6d66318239eeda879b451b5bb87d08ccb5587a8b664f` |
| `videos/hurdles-110m-stadium-20261021/outputs/hurdle-110-reference.html` | 10179384 | `a132302037a6cd455405ec22ec1e66790fc422ee3109eda49c065bac6e3725d8` |
| `videos/hurdles-110m-stadium-20261021/outputs/hurdle-110.html` | 2512940 | `1135dc19f1debfd6f1b9d11a5a0fdbfa3be2f1fbfff943b4447e947839f32f01` |
| `videos/hurdles-110m-stadium-20261021/outputs/hurdle-character-v6.html` | 776691 | `5c57f58a8422f9f1ed1115bf153a05e20ba8c939758f22ce618d6b40eb330b08` |
| `videos/hurdles-110m-stadium-20261021/outputs/hurdle-character-v7.html` | 1387033 | `ed5155a001ad606d422bfb00083193753755fff41d9d2e05553a0943262214da` |

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
>       "duration": "2.750000",
>       "nb_frames": "66"
>     }
>   ],
>   "format": {
>     "duration": "2.750000",
>     "size": "1307066"
>   },
>   "file": "outputs/hurdle-110-code.mp4",
>   "scope": "本次只读取媒体头；未全片解码、播放或重新进行视觉/听觉验收。"
> }
> ```


已有检查报告路径（未在本轮重新执行）：

- `videos/hurdles-110m-stadium-20261021/outputs/checks/athlete-v2-receipt.json`
- `videos/hurdles-110m-stadium-20261021/outputs/checks/browser-interactions.json`
- `videos/hurdles-110m-stadium-20261021/outputs/checks/continuous-portrait-receipt.json`
- `videos/hurdles-110m-stadium-20261021/outputs/checks/face-local-receipt.json`
- `videos/hurdles-110m-stadium-20261021/outputs/checks/final-layout-receipt.json`
- `videos/hurdles-110m-stadium-20261021/outputs/checks/final-motion-receipt.json`
- `videos/hurdles-110m-stadium-20261021/outputs/checks/live-human-receipt.json`
- `videos/hurdles-110m-stadium-20261021/outputs/checks/live-initial-receipt.json`
- `videos/hurdles-110m-stadium-20261021/outputs/checks/live-playback-receipt.json`
- `videos/hurdles-110m-stadium-20261021/outputs/checks/live-verification-receipt.json`
- `videos/hurdles-110m-stadium-20261021/outputs/checks/live-verification.json`
- `videos/hurdles-110m-stadium-20261021/outputs/checks/normal-speed-browser.json`
- `videos/hurdles-110m-stadium-20261021/outputs/checks/portrait-fixed-receipt.json`
- `videos/hurdles-110m-stadium-20261021/outputs/checks/portrait-receipt.json`
- `videos/hurdles-110m-stadium-20261021/outputs/checks/rig-rest-blender.json`
- `videos/hurdles-110m-stadium-20261021/outputs/checks/rocketbox-inspection.json`
- `videos/hurdles-110m-stadium-20261021/outputs/checks/static-check.json`
- `videos/hurdles-110m-stadium-20261021/outputs/checks/v2-cinematic-receipt.json`
- `videos/hurdles-110m-stadium-20261021/outputs/checks/v2-final-browser.json`
- `videos/hurdles-110m-stadium-20261021/outputs/checks/v2-portrait-receipt.json`
- `videos/hurdles-110m-stadium-20261021/outputs/checks/v2-poses-receipt.json`
- `videos/hurdles-110m-stadium-20261021/outputs/checks/v2-verification.json`
- `videos/hurdles-110m-stadium-20261021/outputs/checks/verification.json`
- `videos/hurdles-110m-stadium-20261021/outputs/checks/offline/browser-final.json`
- `videos/hurdles-110m-stadium-20261021/outputs/checks/offline/browser-initial.json`
- `videos/hurdles-110m-stadium-20261021/outputs/checks/offline/final-browser.json`
- `videos/hurdles-110m-stadium-20261021/outputs/checks/offline/source-check.json`
- `videos/hurdles-110m-stadium-20261021/outputs/checks/offline/verification.json`
- `videos/hurdles-110m-stadium-20261021/outputs/checks/offline/verified-browser.json`
- `videos/hurdles-110m-stadium-20261021/outputs/checks/realism-v6/browser-check.json`
- `videos/hurdles-110m-stadium-20261021/outputs/checks/realism-v6/build-report.json`
- `videos/hurdles-110m-stadium-20261021/outputs/checks/realism-v6/scan-inspect.json`
- `videos/hurdles-110m-stadium-20261021/outputs/checks/realism-v6/static-check.json`
- `videos/hurdles-110m-stadium-20261021/outputs/checks/realism-v7/build-report.json`
- `videos/hurdles-110m-stadium-20261021/outputs/checks/realism-v7/inspect.json`
- `videos/hurdles-110m-stadium-20261021/outputs/checks/realism-v7/static-check.json`

## 制作方法与迭代记录选编

下面是项目自有文档的归档摘录，保留原阶段与原检查表述，个人路径已泛化、相对链接已注明源项目位置。它们属于来源陈述，不代表本次重新完成其中的生成或验收。

> [!info]- 来源文档：videos/hurdles-110m-stadium-20261021/README.md
> # 110 米栏 · 代码生成 3D 动画
> 
> ## 本轮 v7：只用免费资源的完整人物测试
> 
> 打开 outputs/hurdle-character-v7.html，或 http://localhost:8110/outputs/hurdle-character-v7.html 。页面包含上一轮与完整人物的全身、面部对照，以及半身和骨骼弯曲检查。六张图片全部嵌入，可离线查看。
> 
> 本轮采用 MIT 许可的 Microsoft Rocketbox Sports_Male_04，保留配套的头部、身体、蓝色无袖运动服、白色短裤、跑鞋与 80 骨骼，使用原生 2048×2048 贴图、本地 Cycles 渲染。素材支出为零。全身衔接和服装比 v6 更自然，但脸部细节较少，仍有游戏角色感，未通过真人实拍质感目标。不替换 v5 主动画，抬腿图仅检查骨骼，不是专业跨栏动作。
> 
> 在本视频目录重建（需要本项目既有 v6 的 baseline.blend 场景；如缺失，先按下节运行 build:lookdev）：
> 
>     npm ci --cache .cache/npm
>     npm run assets:free
>     npm run build:free
>     npm run render:free
>     npm run review:free
>     npm run check:free
>     npm run preview
> 
> 制作提示词和筛选结论见 docs/REALISM-V7-FREE.md。scripts/build-free-v7.py 修改人物材质、站姿、骨骼检查姿势与镜头；scripts/render-free-v7.mjs 修改采样与导出；src/lookdev-v7.html 修改对照页面。可编辑场景 outputs/realism-v7/complete-free-athlete.blend；渲染图同目录；日志与草稿位于 outputs/checks/realism-v7/。这一步没有新增动画时间轴或配音，主动画的修改方式仍按下文执行。
> 
> ## 本轮 v6：人物静帧验证
> 
> 打开 outputs/hurdle-character-v6.html，或预览服务中的 http://localhost:8110/outputs/hurdle-character-v6.html 。页面包含同机位的新旧人物面部对照、候选人物半身和全身，并支持查看原尺寸。四张画面均为本地 Cycles 渲染，页面内嵌图片，可离线查看。
> 
> 这一阶段使用 Renderpeople 免费 Eric 扫描面部，保留面部曲面和 8K UV 贴图，适配本项目 MakeHuman 身体、短裤和跑鞋，制作红黑高领无袖训练上衣，添加眼球反射与短发细丝。它是新的候选人物，身份与旧人物不同。高领用于覆盖当前未完全解决的头颈皮肤连接；造型仍需要视觉验收，不能称为真人实拍级。专业跨栏动作与动作中的变形尚未验证。v5 动画未被替换。
> 
> 本轮视觉结论：面部细节改善，全身与程序服装仍未达到真人实拍目标，未将候选接入主动画。用户已明确要求后续只使用免费资源，付费候选不再作为当前实施方案，未购买任何付费资产。筛选记录见 docs/REALISM-V6.md。
> 
> 在本目录独立重建静帧（系统需已安装 Blender 及 FFmpeg）：
> 
>     npm ci --cache .cache/npm
>     npm run assets:lookdev
>     npm run build:lookdev
>     npm run render:lookdev
>     npm run review:lookdev
>     npm run check:lookdev
>     npm run preview
> 
> 提示词与来源见 docs/REALISM-V6.md；人物场景为 outputs/realism-v6/athlete-lookdev.blend，旧人物同机位场景为 baseline.blend，PNG/JPG 同目录。scripts/build-realism-v6.py 修改造型与机位；src/lookdev-v6.html 修改审阅页面；scripts/render-lookdev-v6.mjs 修改采样和导出。此阶段没有新配音和动画时间轴，v5 时间轴仍按下文修改。
> 
> Renderpeople 免费资产可用于渲染，仍受其官方许可约束；原始模型、纹理与打包场景仅供本地制作，不应随公开页面或项目压缩包再次分发。公开静帧页面只包含渲染图片，不暴露 3D 资产。下载脚本、原始资料、日志和中间图均保存在本视频目录。
> 
> 当前主交付 outputs/hurdle-110.html：通过本地代码构建与驱动三维场景，在 Blender Cycles 中逐帧渲染，再嵌入单个 HTML 播放。没有使用现成拍摄视频，也没有调用 AI 视频生成服务，不需要视频模型账号。
> 
> 这是2.75 秒、24fps、1280×720、默认正常速度的验证样片，含加速跑、两次过栏与落地继续跑动。人物基础网格、皮肤照片纹理和环境全景贴图使用授权资产，不能声称所有原材料都从零生成。人物与动作仍有 CG 感，尚未达到真人实拍级。详见 docs/CODE-RENDER.md。
> 
> ## 打开成品
> 
> 直接用 Chrome 或 Edge 打开 outputs/hurdle-110.html。视频、封面、动作定格、脚本和样式全部内嵌，断网也能播放。也可在本目录运行：
> 
>     npm run preview
>     # http://localhost:8110/outputs/hurdle-110.html
> 
> 支持暂停、重播、时间轴、¼ / ½ / 1 倍速、逐帧、动作定格和沉浸观看。默认 1× 正常速度。空格播放/暂停，左右方向键逐帧，R 重播，H 沉浸，Esc 退出。无旁白或音轨。
> 
> ## 独立安装、构建与检查
> 
> 所有命令在本 README 所在目录执行，不引用其他视频目录或根目录的依赖。
> 
>     mkdir -p .cache/tmp .cache/npm
>     export TMPDIR="$PWD/.cache/tmp" TMP="$PWD/.cache/tmp" TMP_DIR="$PWD/.cache/tmp" TEMP="$PWD/.cache/tmp"
>     export BLENDER_USER_RESOURCES="$PWD/.cache/blender"
>     export PLAYWRIGHT_BROWSERS_PATH="$PWD/.cache/browsers"
>     npm ci --cache .cache/npm
>     npm run build
>     npm run check
>     npx tsc --noEmit
> 
> 构建使用已经渲染的本地 MP4，无需再次渲染 Blender。依赖由 package-lock.json 锁定，npm 缓存在 .cache/npm/。静态检查验证视频和 3D 模型内嵌、来源标注、导入边界、文件真实路径与 public/ 副本一致性。浏览器检查结果在 outputs/checks/。
> 
> ## 从代码重建三维动画
> 
> 重渲染需要系统 Blender 和 FFmpeg，本机验证为 Blender 5.2.2。
> 
>     npm run build:scene
>     # outputs/offline/hurdles-cycles.blend，可在 Blender 中编辑
>     npm run render:scene
>     # outputs/offline/frames/frame_0001.png … frame_0066.png
>     npm run encode:scene
>     # public/assets/offline/hurdles-cycles.mp4 和定格 JPG
>     npm run build
> 
> 脚本默认 CPU 渲染。本机 Metal 后端两次测试没有正常推进；HURDLE_CPU=0 可尝试硬件后端，但不保证可用。重渲染耗时明显高于实时 WebGL。
> 
> 场景文件已打包纹理，可独立打开。scripts/run-offline.mjs 将 Blender 用户资源和临时目录指向项目内，不保存系统偏好。系统图形驱动自身的缓存不属于项目可完整控制的路径，不能声称所有操作系统缓存都已隔离。
> 
> ## 修改人物、素材、配音与时间轴
> 
> - scripts/build-offline.py：材质、独立服装网格、发丝、跑道、栏架、灯光、镜头、景深、运动模糊与骨骼动画转换。
> - sources/offline/poses.json：由本项目原实时动画导出的 163 骨骼矩阵、24fps、120 帧，源模拟时间 3.6–8.558 秒。动作来自 src/app.js 的轨迹、关键姿态与 IK，不是专业运动员动捕。修改动作后需重新导出骨骼数据并重建场景。
> - public/assets/athlete-v2.glb 和 sources/makehuman-system/：MakeHuman 基础人物；换模型时需重新匹配权重和骨骼。
> - public/assets/offline/stadium_01_2k.hdr：Poly Haven CC0 环境照明及远处背景图片。
> - scripts/encode-offline.py：66 张 PNG 合成为2.75 秒正常速度视频，并导出封面和动作定格。
> - src/cg-shell.html、src/style-cg.css、src/cg-player.js：HTML 播放器。动作按钮时码应与定格图同步。
> - src/remotion.tsx：HurdleCG 使用本地代码渲染的视频。修改时长或帧率后同步更新合成元数据。
> - 无旁白。新增录音放在本目录 public/，在 Remotion 中加独立音轨，不引用其他视频目录。
> 
> ## Remotion 预览与导出
> 
>     npm run studio
>     # 选择 HurdleCG，2.75 秒代码渲染版
>     npm run render
>     # outputs/hurdle-110.mp4
> 
> Remotion 读取本地 Blender 成片。remotion.config.ts 使用本机安装的 Chrome，其他系统改为本机浏览器路径；需下载浏览器时放在当前项目 .cache/。
> 
> 原实时 3D 版保留为 Hurdle110。曾使用的库存参考成品与入口源码保留在 outputs/backups/，不是当前生成成果，主构建不依赖该拍摄视频。
> 
> ## 旧版、来源与隔离
> 
> outputs/hurdle-110-3d.html 是原实时 3D 对照版；outputs/hurdle-110-reference.html 是之前的实拍素材参考，不满足当前目标；outputs/backups/ 保存历史备份。提示词见 docs/prompt.md，资产来源见 docs/ASSET-CREDITS.md。
> 
> 本次源码、素材、动画数据、场景、依赖、缓存、日志、检查图和成品均在当前视频目录内。Tabbit 截图工具固定先写入其系统目录，之后立即移入 outputs/checks/ 并清理对应临时副本。

> [!info]- 来源文档：videos/hurdles-110m-stadium-20261021/docs/CODE-RENDER.md
> # 代码生成 3D 动画：当前执行方案
> 
> 2026-10-21 用户确认继续：不使用库存视频，不需要 AI 视频模型账号。通过本地 Python、Blender、骨骼动画和 Cycles 逐帧渲染新的画面，再由 HTML 播放。
> 
> ## 制作提示词
> 
> 制作一段正常速度的男子 110 米栏训练镜头。一名成年运动员穿红色田径背心、深色短裤与跑鞋，在体育场红色塑胶跑道上加速、过栏和继续跑动。人物保持同一张脸、自然比例、真实皮肤纹理和短发；服装具有独立网格厚度、针织纹理及细小褶皱。加强皮肤的次表面散射、眼球反射、发丝、接触阴影与跑道颗粒，使用物理光照、克制景深和运动模糊。镜头侧前方跟拍，完整保留头、脚、栏架和落地。先输出五秒验证片段，逐帧检查起跨、越栏和接地；不以“照片级”等提示词代替视觉验收。
> 
> ## 实际来源边界
> 
> - 人物基础网格、权重和照片皮肤纹理使用本项目已有的 MakeHuman CC0 资产。
> - 运动由本项目的轨迹、关键姿态与肢体 IK 计算，不是现成拍摄视频，也暂不是专业运动员的动作捕捉。
> - 服装、发丝、跑道、栏架、镜头和灯光由脚本构建。
> - Poly Haven 的 Stadium 01 HDRI 用于环境照明与远处背景，作者 Sergej Majboroda，CC0：https://polyhaven.com/a/stadium_01 。这是照明用全景图片素材，不是动态视频。生成画面含照片纹理与环境贴图，不能声称所有原材料都是从零生成的。
> - 离线渲染改善材质与照明，但仍需如实检查和说明人物、动作与真人之间的差距。
> 
> ## 路径
> 
> 源码、输入动画数据、场景文件、逐帧 PNG、MP4 和 HTML 全部在本视频目录内。后续命令和完成检查将随实现更新。
> 
> ## 可复现命令
> 
> 在本目录运行 npm run build:scene、npm run render:scene、npm run encode:scene、npm run build。场景：outputs/offline/hurdles-cycles.blend；逐帧：outputs/offline/frames/；成片：public/assets/offline/hurdles-cycles.mp4；单文件：outputs/hurdle-110.html。骨骼数据 sources/offline/poses.json 为本项目 src/app.js 的程序动作导出，源时段 3.6–8.558 秒，24fps，共 120 帧，不是运动员动捕。
> 
> 本机 Metal 渲染两次测试未正常推进，终止后使用 CPU；不声称 GPU 加速已验证。用户资源和临时目录指向本项目 .cache；系统驱动缓存无法完全控制，详见 README。
> 
> ## 验证片段调整
> 
> 检查发现原程序步态切换配合运动模糊会产生不自然的抹影，因此首版关闭运动模糊，并聚焦首个完整过栏动作：60 帧、24fps、2.5 秒、正常速度。骨骼源数据仍保留五秒 120 帧，场景可继续扩展。原有带模糊的中间帧另存为草稿，未作为最终视频输入。
> 
> 片尾顺序检查后补齐第二次落地，最终验证片段为 66 帧、24fps、2.75 秒。关闭运动模糊，正常速度播放。

> [!info]- 来源文档：videos/hurdles-110m-stadium-20261021/docs/REALISM-V6.md
> # 人物静帧验证 v6
> 
> ## 制作提示词
> 
> 为原创男子 110 米栏动画先制作可检验的人物静帧。运动员为成年男子，短发，精瘦的田径运动员体型，穿红黑配色高领无袖技术训练上衣、深色短裤和跑鞋。使用许可允许的真人扫描人物资产保留真实面部比例、皮肤微细节和自然色差；使用独立服装网格、合理的布料厚度、细微褶皱和明确的缝线。不要以增加皮肤噪声代替真实扫描细节。人物在同一体育场自然光下，分别拍摄脸部、半身和全身，镜头保持真实透视与克制景深。使用本地 Blender/Cycles 渲染，保存可编辑场景和素材溯源。先验证人物静帧，再处理专业跨栏动作；静帧不代表动作已经通过验收。
> 
> ## 工作边界
> 
> - 在现有视频目录中继续，不覆盖 v5 动画。
> - 本轮先测试免费且许可允许渲染的扫描资产，不购买或注册服务。
> - 如果候选资产穿着不适合跨栏，须明确说明哪些部件能复用，不能将休闲服装模型宣称为运动员成品。
> - 通过静帧检查后，提供可直接查看的对比页面与本地源场景。
> 
> ## 候选来源
> 
> - Renderpeople 免费 rigged Eric，官方页面：https://renderpeople.com/free-3d-people/ 。官方标注允许免费商用渲染且无需注册；原模型和贴图的进一步使用仍按包内许可执行。
> - 下载链接来自该官方页面：https://renderpeople.com/sample/free/renderpeople_free_rigged_people_FBX.zip 。下载原包和解压文件保存在 sources/research/realism-v6/。
> - Human Generator 试用需要邮件领取，且贴图带水印，本轮未注册或使用。
> 
> ## 实际制作
> 
> - 保留 Eric 扫描头部的原曲面、UV、8192×8192 diffuse / gloss / normal 贴图；颈部切割后适配到原 MakeHuman 身体，没有把面部投射到旧头部上。候选人物是新身份。
> - 正装身体没有用于运动员；身体重新使用本项目完整 MakeHuman 模型，再调整头颈边界、肤色及服装厚度。移除训练造型不需要的号码布。
> - 眼球增加角膜反射，扫描头发上添加 8,500 条短细丝；仍不是完整的专业毛发资产。
> - 服装重新平滑边界，恢复衣领下的皮肤网格，避免旧版删面在调整衣领后形成缺口。
> - 开放领口方案在近景中仍有明显头颈接缝，保留在检查草稿中，未宣称通过。最终静帧候选改穿红黑高领无袖训练上衣，使用完整新服装网格和罗纹领口区域，隐藏领内皮肤防止穿透。这是服装方案的调整，不代表开放领口下的皮肤融合已经解决。
> - 光照、体育场 HDRI 和镜头保持同一设置。旧人物基于 v5 场景，用相同机位重新渲染作对照。
> - 本轮输出脸部、半身、全身三个新人物静帧和一个旧人物脸部对照；不更新 v5 动画，尚未验证动作中的颈部、服装或发丝变形。
> 
> ## 使用许可及发布边界
> 
> 官方条款：https://renderpeople.com/general-terms-and-conditions/ 。条款允许个人或商业静帧/动画渲染，也限制原始、局部或修改后的 3D 数据对第三方分发。原始下载、扫描纹理和打包 Blender 场景仅留在本地制作目录；审阅 HTML 只嵌入最终渲染 JPG，不包含扫描网格、FBX、贴图或可编辑场景。
> 
> 页面与条款快照位于 sources/research/realism-v6/，下载 URL 与 SHA256 见同目录 download.json。此资产不是 CC0，也不能混用 MakeHuman 的 CC0 标签描述它。
> 
> ## 复现
> 
> 在本视频目录按 README 的 assets:lookdev → build:lookdev → render:lookdev → review:lookdev → check:lookdev 顺序执行。预览入口 outputs/hurdle-character-v6.html；可编辑场景 outputs/realism-v6/athlete-lookdev.blend；所有静帧 PNG/JPG 在同一目录；日志与迭代草稿位于 outputs/checks/realism-v6/。
> 
> ## 验收范围
> 
> 目标是提供可比较的造型候选，并未宣称实现真人实拍。重点检查脸部比例、眼睛反光、头颈色差、衣领贴合与全身比例。下一阶段需要单独验证专业跨栏动作和动态变形，不能用本轮静帧替代动作验收。
> 
> ## 本轮视觉结论及下一阶段候选
> 
> 2026-10-21：面部相较 v5 具备更多自然细节，但身体、衣服与整体材质仍明显像 CG，未通过真人实拍质感目标。不将本轮候选直接接入主动画。把扫描头、通用身体和程序服装拼接在一起，没有解决完整人物的一致性。
> 
> 下一步优先评估配套的完整扫描人物，而非继续扩展当前拼接方案。具体候选：[Mason Rigged 007](https://renderpeople.com/3d-people/mason-rigged-007/)，年轻成年男性、运动 T 恤和短裤，官方页面提供 FBX，标注约 10k 四边面与 8K diffuse/normal/gloss 贴图。2026-10-21 页面及浏览器显示单模型 79 欧元；不同地区的币种、税费及最终结算价格以官网为准。厂商原始展示图的浏览器截图为 outputs/checks/realism-v6/vendor-mason-reference.png，不是本项目渲染结果。
> 
> 该候选尚未购买、下载或验证动态变形。现阶段只确认供应商规格、服装外观和价格；购买完整资产也不等于已经达到实拍质感，仍需静帧与动作验收。
> 
> 用户已明确选择“继续只用免费资源”。付费候选不再作为当前实施方案；不购买、不进入结算、不要求用户再次确认预算。后续仅筛选和测试许可允许使用的免费资产。

> [!info]- 来源文档：videos/hurdles-110m-stadium-20261021/docs/REALISM-V7-FREE.md
> # 免费完整运动人物验证 v7
> 
> ## 制作提示词（实施前确定）
> 
> 为原创 110 米栏动画验证一个完整、配套的成年男性运动人物。只用免费且许可允许的资源，不购买、不注册付费服务，不使用别人拍好的视频。保留人物原本匹配的头部、身体、蓝色无袖运动上衣、白色短裤、跑鞋、UV 和骨骼，不再拼接不同人物的头身。使用 Microsoft Rocketbox Sports_Male_04 的最高细节网格及原生颜色、法线、反射贴图；在 Blender/Cycles 体育场自然光下渲染。皮肤、眼睛、布料和鞋面分别处理反光，衣服保持哑光，头发添加克制的短细丝。输出与 v5 相同机位的脸部、半身、全身静帧，再输出一个骨骼弯曲检查姿势。忠实呈现实际质量，不将旧游戏人物宣称为实拍级；静态外观和运动变形分别验收。
> 
> ## 选择与限制
> 
> - 采用 [Microsoft Rocketbox](https://github.com/microsoft/Microsoft-Rocketbox) 的 Sports_Male_04。官方仓库按 MIT 许可发布，许可原文与下载清单保存在 sources/research/realism-v7/rocketbox/。
> - 该模型是配套的完整人物，不是本项目从零雕刻，也不是完整真人扫描。原网格约 3,804 顶点，原生贴图 2048×2048，近景写实存在明显上限。细分和光线追踪不能补出不存在的扫描细节。
> - Deep3DStudio 免费 Athletic African man walking 223 具有完整扫描外观，但 Sketchfab 下载弹出登录页，且官网说明为静态行走姿势。本轮未登录、未取得该文件，也未将其当作可直接跨栏的绑定角色。
> - 付费 Mason 候选停用，素材支出为零。
> 
> ## 验收
> 
> 先看实际渲染，再记录是否值得接入主动画。v5 主动画和 v6 拼接人物实验均保留；本轮静帧不能证明专业跨栏动作已完成。
> 
> ## 实际测试与结论
> 
> 保留原配身体、运动服、跑鞋和 80 骨骼；最高网格细分两级、重建原生颜色/法线/反射材质，添加约 6,200 条克制短发丝，轻微放松眼睑和唇部。按项目既有体育场的同机位渲染脸部、半身与全身，并通过骨骼旋转生成抬腿/曲肘检查图。
> 
> 全身的服装与颈部衔接比 v6 更自然，不再需要高领遮接缝。面部纹理细节却少于 v6 的 Eric 扫描头，眼睛与头发仍可辨认为 CG。此候选未通过真人感目标，不把它当作最终人物，不替换现有主动画。检查姿势只能说明骨骼可驱动局部网格，不能证明跨栏动作、衣物碰撞或动态发丝已经验证。
> 
> 输出：outputs/hurdle-character-v7.html（内嵌六张静帧，可离线查看）；outputs/realism-v7/complete-free-athlete.blend（本地可编辑）；同目录四张新人物 PNG/JPG。所有下载、脚本、日志和检查图均在本视频目录内。
> 
> 更高精度的完整免费扫描仍值得筛选。当前确认的 Deep3DStudio 223 要求免费登录下载，并且尚需重新绑定；它不是当前成果，也不能承诺取得后即可满足动态写实效果。

> [!info]- 来源文档：videos/hurdles-110m-stadium-20261021/docs/ASSET-CREDITS.md
> # v7 免费完整人物测试（2026-10-21）
> 
> - 人物：[Microsoft Rocketbox / Sports_Male_04](https://github.com/microsoft/Microsoft-Rocketbox/tree/master/Assets/Avatars/Professions/Sports_Male_04)，Copyright (c) 2020 Microsoft，MIT。完整许可位于 sources/research/realism-v7/rocketbox/LICENSE.md；官方来源 URL、字节数、SHA256 位于同目录 download.json。保留原配头部、身体、运动服、跑鞋、骨骼、颜色/法线/反射贴图。项目新增两级细分、材质参数、站姿、骨骼检查姿势、短发细丝和本地渲染。
> - 灯光及体育场远景继续使用项目已有 Poly Haven Stadium 01（CC0）；跑道与场景继续由本项目代码构建。
> - v7 对照页内的 v6 旧图仍按下文 Renderpeople / MakeHuman 许可描述；新人物的 MIT 许可不改变旧图及旧模型的许可。
> - Deep3DStudio Athletic African man walking 223 只核对了官方描述与下载登录要求，没有取得原始文件或用于渲染。不将供应商预览或静态扫描宣称为本项目原创动画。
> - 当前预算约束：用户明确要求只用免费资源；未购买素材，新增人物费用为零。
> 
> # v6 人物静帧候选
> 
> 面部及发型扫描：Renderpeople，Eric rigged 001 免费样本；原始 UV 和 8K diffuse/gloss/normal 贴图保留，切割并适配颈部。身体与鞋型为本项目既有 MakeHuman CC0 资产，服装与额外发丝由脚本调整或生成。扫描人物并非 CC0；使用遵循 https://renderpeople.com/general-terms-and-conditions/ ，只在审阅 HTML 中嵌入渲染图片，不分发原始 3D 数据。下载来源 https://renderpeople.com/free-3d-people/ 。详情见 REALISM-V6.md。
> 
> # v5 主动画：本地代码渲染的 3D 动画
> 
> 人物使用本项目已有 MakeHuman CC0 网格、照片皮肤纹理、权重、眼睛、头发底层与跑鞋。新增服装网格、发丝、跑道、栏架、灯光、骨骼驱动和镜头由代码构建。环境照明与远景：[Stadium 01 / Sergej Majboroda / Poly Haven](https://polyhaven.com/a/stadium_01)，[CC0](https://polyhaven.com/license)。不使用现成动态视频；纹理和环境贴图包含摄影原料。详见 CODE-RENDER.md。下面是旧版历史记录。
> 
> # 旧版历史记录：真人训练影像（已停用）
> 
> 2026-10-21，用户确认真人感优先。主页面使用 Mharee Lynn Eulanda Guillena 的 [Male Athlete Hurdling on Outdoor Track](https://www.pexels.com/video/male-athlete-hurdling-on-outdoor-track-30997018/)，遵循 [Pexels License](https://www.pexels.com/license/)。详情、剪辑范围和素材路径见 LIVE-ACTION.md（项目文件：`videos/hurdles-110m-stadium-20261021/docs/LIVE-ACTION.md`）。下文的 MakeHuman 资产继续用于保留的 3D 版本，未混入真人画面。
> 
> # 人物美化 v2 / v5 基础素材
> 
> 当前运行模型为 `public/assets/athlete-v2.glb`。保留 MakeHuman 原生头颈拓扑和骨骼，已完全移除扫描头部投射。下列资产都来自官方系统资产包并共享 CC0：
> 
> - 官方说明与逐项许可：https://static.makehumancommunity.org/assets/assetpacks/makehuman_system_assets.html
> - 下载：https://files.makehumancommunity.org/asset_packs/makehuman_system_assets/makehuman_system_assets_cc0.zip
> - 下载包位于 `.cache/makehuman-system-assets.zip`；本项目使用的副本位于 `sources/makehuman-system/`。
> - 皮肤：`skins/young_caucasian_male2`，原生全身 UV、照片纹理；运行副本转换为高质量 JPEG，原 PNG 保留。
> - 头发：`hair/short04`，原生网格和带 alpha 的发丝纹理。
> - 眼睛：`eyes/high-poly`、`eyes/materials/brown_eye.png`，保留透明通道，避免外层角膜遮盖虹膜。
> - 跑鞋：`clothes/shoes05`，原生网格、鞋带、纹理与权重。
> - 人体基础网格、男性形态和权重：同下文 MakeHuman 说明；本版不再对头部顶点进行扫描投射。
> - 运动员服装外观、运动轨迹、IK、镜头与景深由本项目实现。
> - 完整旧版 HTML、模型、脚本和模型构建器保存在 `outputs/backups/initial/`。
> 
> 早期 Microsoft Rocketbox Male_Adult_01 候选保存在 `sources/research/rocketbox/`，未被 v5 主模型使用；本轮 v7 另采用 Sports_Male_04，来源与许可见本文顶部。
> 
> ---
> 
> 下面保留旧版素材溯源记录，供回溯使用；Lee Perry-Smith 扫描不再出现在当前版本。
> 
> # 素材来源与修改记录
> 
> ## MakeHuman 人体
> 
> - 来源：https://github.com/makehumancommunity/makehuman
> - 路径：makehuman/data/3dobjs/base.obj、data/rigs/default.mhskel、default_weights.mhw，以及本目录保存的体型形态 target。
> - 许可：CC0，详见 MAKEHUMAN-ASSETS-LICENSE.md 与源文件头部。
> - 修改：成年男性、精瘦肌肉形态；按 1.88 米规范化；保留连续人体拓扑与 163 个骨骼；平滑细分；田径服装使用局部坐标着色；增加跑鞋。用扫描头部对面部曲面及 UV 做近邻转移，保留连续颈部与身体权重。
> 
> ## Lee Perry-Smith 面部扫描
> 
> - 作者：Lee Perry-Smith / Infinite-Realities，© I-R Entertainment Ltd.
> - 下载来源：https://github.com/mrdoob/three.js/tree/r170/examples/models/gltf/LeePerrySmith
> - 使用文件：LeePerrySmith.glb、Map-COL.jpg、Infinite-Level_02_Tangent_SmoothUV.jpg。
> - 原始档案：https://casual-effects.com/data/ （Morgan McGuire Computer Graphics Archive）。档案的原始署名与许可信息保存在 LEE-PERRY-SMITH-SOURCE.js。
> - 许可：CC BY 3.0，https://creativecommons.org/licenses/by/3.0/
> - 修改：头部表面形状与 UV 转移至 MakeHuman 连续人体；调整材质肤色、粗糙度及法线强度，颈部颜色渐变。最终 HTML 内置可打开的素材署名窗口。
> 
> ## 程序与原创部分
> 
> - Three.js 0.170.0，MIT，见 THREE-LICENSE.txt。
> - 体育场、跑道、栏架、看台、灯柱、旗帜、草地和跑道纹理、号码布、服装着色、动作轨迹、IK 与界面：本项目实现。
> - 声音：Web Audio 合成风声、脚步，不含外部录音。
> - 字体：系统字体，不需要外部字体服务。
> 
> ## 尺度参考
> 
> World Athletics《Track and Field Facilities Manual 2019》110m Men：10 栏、1.067 m 栏高、13.72 m 首栏距离、9.14 m 栏间距、14.02 m 末栏至终点。
> https://worldathletics.org/responsive/download/downloaddirect?filename=7d39b677-8043-45f4-89f4-6c8814023057.pdf&urlslug=Track%2Band%2BField%2BFacilities%2BManual%2B2019%2BEdition%2B-%2BChapters%2B1-3

> [!info]- 来源文档：videos/hurdles-110m-stadium-20261021/docs/LIVE-ACTION.md
> # 真人感优先版本 / 2026-10-21
> 
> 用户确认：接受将 HTML 主画面改为授权真人跨栏影像，保留播放、慢动作与时间轴，原 3D 为可选模式。
> 
> ## 素材与来源
> 
> - 名称：[Male Athlete Hurdling on Outdoor Track](https://www.pexels.com/video/male-athlete-hurdling-on-outdoor-track-30997018/)
> - 作者：Mharee Lynn Eulanda Guillena，Pexels。
> - 官方许可：[Pexels License](https://www.pexels.com/license/)，2026-10-21 核对；允许免费下载、使用和修改。不可暗示画面中人物为本项目背书。
> - 原片下载地址：`https://videos.pexels.com/video-files/30997018/13249548_1080_1920_28fps.mp4`。
> - 原片保存在本视频目录的 `sources/live-action/hurdle-30997018-original.mp4`。
> - 源片 1080×1920，约 12.86 秒、27.92 fps；保留原始竖屏比例，无横向拉伸。
> 
> ## 编辑
> 
> 选取源片约 6.15–12.35 秒，输出 6.20 秒的 30fps H.264 MP4。帧率整理仅丢弃或重复原帧，不使用生成式补帧、变脸、身体重绘或虚构动作。默认正常速度（1×）播放，观看时长为 6.20 秒；¼× 和 ½× 可手动切换。慢放不会增加源片细节。
> 
> 这是真实训练素材的局部画面，不能确认画面涵盖完整 110 米、标准成年男子栏高或实测比赛成绩，因此页面不显示模拟距离、速度、过栏数量和虚构成绩。标题保留 110 米栏训练的作品主题。
> 
> 原片没有使用音轨，页面不合成假现场声音；未用未经授权的赛事转播素材。画面中品牌与人物均不表示背书。
> 
> ## 文件
> 
> - `src/live-action.html`、`src/live-action.css`、`src/live-action.js`：主页面与播放器。
> - `scripts/prepare-live-action.py`：可重建剪辑与四张定格图。
> - `scripts/build-live-action.mjs`：将视频与定格图作为 data URI 打包。播放器创建本地 Blob URL，支持 file:// 离线 seek。
> - `outputs/hurdle-110.html`：新版单文件主交付。
> - `outputs/hurdle-110-3d.html`：保留的可环绕实时 3D 版。
> - `src/remotion.tsx`：真人版和原 3D 的独立导出入口。
> 
> 全部运行资源内嵌。主页面中的旧版入口和署名外链属于导航，关闭联网不会影响真人影像播放。

> [!info]- 来源文档：videos/hurdles-110m-stadium-20261021/docs/ORIGINAL-AI-VIDEO.md
> > 历史方案，已被用户后续确认的本地代码渲染方案取代。未执行 imagegen，也未生成 AI 视频。当前要求见 CODE-RENDER.md。
> 
> # 当前目标：提示词生成原创写实动画
> 
> 2026-10-21 用户澄清并确认：使用 AI 视频模型根据提示词生成全新的人物、场景与跨栏动作，再通过 HTML 播放。不得把已拍好的库存视频当作生成成果。默认正常速度（1×）。之前的实拍页面仅为旧版参考，不满足此次生成目标。
> 
> ## 执行状态
> 
> - 原创人物参考首帧：准备通过内置 imagegen 生成。
> - 原创动态视频：尚未生成。
> - 当前会话发现的 Runway、Higgsfield 等视频插件均为管理员禁用，不能调用。等待用户确认可使用的视频平台或生成通道。
> - 参考图与分镜不能替代动态视频；对图片平移缩放也不能算跨栏动作生成。
> 
> ## 首帧生成提示词
> 
> Use case: photorealistic-natural.
> Asset type: original reference first frame for an AI-generated sports film, landscape 16:9.
> Create a completely original fictional adult East Asian male hurdler, approximately 25 years old, with a lean powerful sprinter's physique and anatomically natural proportions. He has short textured black hair, a focused natural face, realistic skin pores, slight perspiration, subtle skin tone variation and natural muscle definition. He wears a red technical running singlet, fitted black track shorts, a small white bib reading "04", and realistic track spikes, with natural fabric folds.
> An outdoor athletics stadium in soft morning sidelight: textured red rubber track with accurate straight white lane markings, a green infield and distant stadium seating. The athlete is in the left half of the frame running from left to right, about three metres before a single white-and-dark-metal men's hurdle in the right half. He is one stride before his approach to the hurdle, running at normal speed, one forefoot pushing against the track, the opposite knee driving forward, arms naturally counterbalancing. The hurdle is physically planted on the track and has no brand name. Show his entire body, both feet, head and the full hurdle with comfortable margins. Track, person and hurdle share a consistent perspective and natural contact shadows.
> Camera at waist height, a three-quarter side view that reveals his face and body, 50 mm sports cinema lens, restrained depth of field with the entire athlete and hurdle sharp. Convincing documentary sports photography, unretouched skin and believable outdoor exposure. Natural motion blur only at extremities, not the face. Realistic materials and anatomy; the scene should look like a frame from a high-end live-action sports film. No collage, UI, watermark, stylized rendering, waxy skin, glamour retouching, duplicated limbs, deformed hands or identity of any real athlete. Only the bib has text.
> 
> ## 动态生成主提示词（图生视频）
> 
> Use the supplied first frame as the visual reference. A single continuous photorealistic sports-film shot. The same adult hurdler runs left to right at natural real-time speed, approaches the hurdle, clears it once with an efficient low trajectory, lands and continues sprinting. His lead knee drives up before the lower leg extends over the hurdle; his bent trail leg sweeps sideways and recovers forward, his torso inclines naturally, and his arms balance the movement. His lead foot snaps down to contact the track and he continues with grounded, rhythmic strides. The camera tracks smoothly alongside him at waist height, keeping his whole body and the hurdle visible during clearance. Natural muscle motion, subtle facial effort, believable cloth movement, consistent sunlight and contact shadows. Preserve his face, red singlet, black shorts and shoes throughout the shot. The hurdle stays physically stable in the same lane. Normal speed, continuous forward movement, documentary sports cinematography.
> 
> ## 首次生成设置与验收
> 
> - 第一轮只生成一个完整过栏动作，建议约 6–8 秒。实际时长与分辨率使用所选平台支持的设置。
> - 画幅优先 16:9。先验证动作，再决定是否需要竖屏构图；禁止拉伸画面。
> - 正常速度，关闭慢动作或自动变速；不添加旁白、字幕与片内 UI。
> - 人物面容、服装、手脚和身材在整段中一致。
> - 起跨、越栏、落地必须连续；栏架不融化，手脚不穿栏，接地不滑行，不漂浮。
> - 动态视频生成后逐帧检查过栏与落地，再集成 HTML。无法通过的样片记录问题并迭代，不以提示词中的“4K”“超写实”等词代替验收。
> 
> ## 后续镜头
> 
> 在主过栏镜头通过后，再以同一角色参考图制作短暂起跑准备、连续栏间跑、冲线减速等镜头。每个独立动作单独生成和检查，再剪辑；不要求一个短镜头凭空覆盖完整 110 米或十个栏。
> 
> ## 官方参考
> 
> - Runway 的图生视频说明要求提示词重点描述运动：[Creating with Gen-4.5](https://help.runwayml.com/hc/en-us/articles/46974685288467-Creating-with-Gen-4-5)。
> - 提示词与迭代：[Runway Academy Prompting Guide](https://academy.runwayml.com/guides/prompting-guide)。
> - API 接入需要服务账户与凭据：[Runway API Getting Started](https://docs.dev.runwayml.com/guides/using-the-api/)。不要把凭据写入前端 HTML 或提交到项目源码。

## 证据清单与缺失信息

盘点的文本/报告文件 85 个，源文本、报告及媒体产物身份摘要为 `efe218da8b35912e576009556c066adb52febcf91fb8aed0225c388116ad575b`。哈希证明采集内容身份，不代表其中每个断言已验证。模型权重、缓存、依赖、第三方源码树和未指定的历史会话未采集；大型文本仅做哈希并按需要选读。

- 本次整理基于明确引用的 README、制作方法、配置、提示词、关键源码与已有报告。未重新观看全部参考/候选/成片，也未做人工听辨。
- 项目未提供的原始模型内部实现、未披露生成后端、未记录 seed/参数、真实费用、人工总工时和用户最终验收，均不补造。实际日志有记录的耗时见上文，流水线耗时不等于整个制作总耗时。
- 源项目内的路径只用于查证；本档案未复制大型素材或模型，也不改变源项目运行依赖。

| 已盘点证据文件 | SHA-256（前 16 位）/状态 |
| --- | --- |
| `videos/hurdles-110m-stadium-20261021/README.md` | `ec3adcc46fed0aae` |
| `videos/hurdles-110m-stadium-20261021/package-lock.json` | `409a9dccd14f2170` |
| `videos/hurdles-110m-stadium-20261021/package.json` | `f3184ea6d4dc2cec` |
| `videos/hurdles-110m-stadium-20261021/remotion.config.ts` | `befde50cbac5d989` |
| `videos/hurdles-110m-stadium-20261021/tsconfig.json` | `27a5ef38c44c9375` |
| `videos/hurdles-110m-stadium-20261021/docs/ASSET-CREDITS.md` | `177f0f7a1815eddc` |
| `videos/hurdles-110m-stadium-20261021/docs/CODE-RENDER.md` | `1d12698db250aa28` |
| `videos/hurdles-110m-stadium-20261021/docs/LEE-PERRY-SMITH-SOURCE.js` | `ce6f25ec33d236af` |
| `videos/hurdles-110m-stadium-20261021/docs/LIVE-ACTION.md` | `3403db580e6182a2` |
| `videos/hurdles-110m-stadium-20261021/docs/MAKEHUMAN-ASSETS-LICENSE.md` | `f6089cba01cb570a` |
| `videos/hurdles-110m-stadium-20261021/docs/ORIGINAL-AI-VIDEO.md` | `def1ddeebe0976e8` |
| `videos/hurdles-110m-stadium-20261021/docs/REALISM-V6.md` | `9435dc4a451e5194` |
| `videos/hurdles-110m-stadium-20261021/docs/REALISM-V7-FREE.md` | `e4d0f28b66dc3727` |
| `videos/hurdles-110m-stadium-20261021/docs/THREE-LICENSE.txt` | `4c40a1ef62450b85` |
| `videos/hurdles-110m-stadium-20261021/docs/prompt.md` | `d448a76ddae11639` |
| `videos/hurdles-110m-stadium-20261021/outputs/checks/athlete-v2-receipt.json` | `fd8ed22c37e128ed` |
| `videos/hurdles-110m-stadium-20261021/outputs/checks/browser-interactions.json` | `9916a650e5597c2c` |
| `videos/hurdles-110m-stadium-20261021/outputs/checks/continuous-portrait-receipt.json` | `3a3ba26fff051c3e` |
| `videos/hurdles-110m-stadium-20261021/outputs/checks/face-local-receipt.json` | `3a6c10c9232bee79` |
| `videos/hurdles-110m-stadium-20261021/outputs/checks/final-layout-receipt.json` | `bc6bc3555b5f45ff` |
| `videos/hurdles-110m-stadium-20261021/outputs/checks/final-motion-receipt.json` | `1b717e74b46396d5` |
| `videos/hurdles-110m-stadium-20261021/outputs/checks/live-human-receipt.json` | `e272db680b578aea` |
| `videos/hurdles-110m-stadium-20261021/outputs/checks/live-initial-receipt.json` | `a6e2b14f5de1c1a0` |
| `videos/hurdles-110m-stadium-20261021/outputs/checks/live-playback-receipt.json` | `aecfe2bb6f003d00` |
| `videos/hurdles-110m-stadium-20261021/outputs/checks/live-verification-receipt.json` | `73ae3ec3e2f12f56` |
| `videos/hurdles-110m-stadium-20261021/outputs/checks/live-verification.json` | `6a8f3e035fb82b8d` |
| `videos/hurdles-110m-stadium-20261021/outputs/checks/normal-speed-browser.json` | `e09a88490f28905d` |
| `videos/hurdles-110m-stadium-20261021/outputs/checks/portrait-fixed-receipt.json` | `0881c10c958e285b` |
| `videos/hurdles-110m-stadium-20261021/outputs/checks/portrait-receipt.json` | `2cb8a35067e60657` |
| `videos/hurdles-110m-stadium-20261021/outputs/checks/rig-rest-blender.json` | `07610b12498c75f3` |
| `videos/hurdles-110m-stadium-20261021/outputs/checks/rocketbox-inspection.json` | `00e6388e752a2feb` |
| `videos/hurdles-110m-stadium-20261021/outputs/checks/static-check.json` | `c62952dfe215da5e` |
| `videos/hurdles-110m-stadium-20261021/outputs/checks/v2-cinematic-receipt.json` | `7ce1427056713794` |
| `videos/hurdles-110m-stadium-20261021/outputs/checks/v2-final-browser.json` | `ff974fe987574300` |
| `videos/hurdles-110m-stadium-20261021/outputs/checks/v2-native-receipt.json` | `e3b0c44298fc1c14` |
| `videos/hurdles-110m-stadium-20261021/outputs/checks/v2-portrait-receipt.json` | `90b1fec8795e1be1` |
| `videos/hurdles-110m-stadium-20261021/outputs/checks/v2-poses-receipt.json` | `1c43058306b6f460` |
| `videos/hurdles-110m-stadium-20261021/outputs/checks/v2-verification.json` | `0ef2de0850f48080` |
| `videos/hurdles-110m-stadium-20261021/outputs/checks/verification.json` | `d2c03358328059e7` |
| `videos/hurdles-110m-stadium-20261021/outputs/checks/offline/browser-final.json` | `7a4f6e54704bf57c` |
| `videos/hurdles-110m-stadium-20261021/outputs/checks/offline/browser-initial.json` | `a0491c62d16812d8` |
| `videos/hurdles-110m-stadium-20261021/outputs/checks/offline/final-browser.json` | `306d42b85d7b3009` |
| `videos/hurdles-110m-stadium-20261021/outputs/checks/offline/source-check.json` | `af73eab1c48cc05b` |
| `videos/hurdles-110m-stadium-20261021/outputs/checks/offline/verification.json` | `09f0935063493945` |
| `videos/hurdles-110m-stadium-20261021/outputs/checks/offline/verified-browser.json` | `5ad08881f313a355` |
| `videos/hurdles-110m-stadium-20261021/outputs/checks/realism-v6/browser-check.json` | `03c59683144581d0` |
| `videos/hurdles-110m-stadium-20261021/outputs/checks/realism-v6/build-report.json` | `9e638eb1117060f3` |
| `videos/hurdles-110m-stadium-20261021/outputs/checks/realism-v6/scan-inspect.json` | `a5611fce3bae8f7a` |
| `videos/hurdles-110m-stadium-20261021/outputs/checks/realism-v6/static-check.json` | `f31ea569f854a395` |
| `videos/hurdles-110m-stadium-20261021/outputs/checks/realism-v7/build-report.json` | `818af1577f9523ad` |
| `videos/hurdles-110m-stadium-20261021/outputs/checks/realism-v7/inspect.json` | `eb34d1da8d0d3c24` |
| `videos/hurdles-110m-stadium-20261021/outputs/checks/realism-v7/static-check.json` | `911004e08647c6eb` |
| `videos/hurdles-110m-stadium-20261021/public/assets/offline/provenance.json` | `531b6ae6b51ee935` |
| `videos/hurdles-110m-stadium-20261021/scripts/build-athlete-v2.py` | `90a9316280e3965f` |
| `videos/hurdles-110m-stadium-20261021/scripts/build-athlete.py` | `e50210f2042c8607` |
| `videos/hurdles-110m-stadium-20261021/scripts/build-cg.mjs` | `0558d7897ca563e1` |
| `videos/hurdles-110m-stadium-20261021/scripts/build-free-v7.py` | `5025f6987a5755df` |
| `videos/hurdles-110m-stadium-20261021/scripts/build-live-action.mjs` | `08ae6b032088ca0e` |
| `videos/hurdles-110m-stadium-20261021/scripts/build-lookdev-v6.mjs` | `c62dd1cb473936e6` |
| `videos/hurdles-110m-stadium-20261021/scripts/build-offline.py` | `0e67fede53388285` |
| `videos/hurdles-110m-stadium-20261021/scripts/build-realism-v6.py` | `8b40025ca459d500` |
| `videos/hurdles-110m-stadium-20261021/scripts/build-review-v7.mjs` | `097408ba311a2b13` |
| `videos/hurdles-110m-stadium-20261021/scripts/build.mjs` | `5a3c4f88c07d672f` |
| `videos/hurdles-110m-stadium-20261021/scripts/check-free-v7.mjs` | `8b85cf16e2a82c89` |
| `videos/hurdles-110m-stadium-20261021/scripts/check-lookdev-v6.mjs` | `907a66780981128a` |
| `videos/hurdles-110m-stadium-20261021/scripts/check.mjs` | `e65a94daf70852e6` |
| `videos/hurdles-110m-stadium-20261021/scripts/collect-receipt.py` | `3d5fc773e78d2350` |
| `videos/hurdles-110m-stadium-20261021/scripts/convert-rocketbox.py` | `4e189c1d12cb1a06` |
| `videos/hurdles-110m-stadium-20261021/scripts/encode-offline.py` | `519e6a0a1d764502` |
| `videos/hurdles-110m-stadium-20261021/scripts/fetch-free-v7.py` | `acb09b09db4fc384` |
| `videos/hurdles-110m-stadium-20261021/scripts/fetch-scan-v6.py` | `30e948323b58b0e8` |
| `videos/hurdles-110m-stadium-20261021/scripts/inspect-rocketbox.py` | `c804fda25400f756` |
| `videos/hurdles-110m-stadium-20261021/scripts/inspect-scan-v6.py` | `8ede1ef0f615cb56` |
| `videos/hurdles-110m-stadium-20261021/scripts/prepare-live-action.py` | `ac8cefd33526bc88` |
| `videos/hurdles-110m-stadium-20261021/scripts/render-free-v7.mjs` | `c152a670d34720b2` |
| `videos/hurdles-110m-stadium-20261021/scripts/render-lookdev-v6.mjs` | `733644980825da22` |
| `videos/hurdles-110m-stadium-20261021/scripts/run-free-v7.mjs` | `afec813a6389545f` |
| `videos/hurdles-110m-stadium-20261021/scripts/run-offline.mjs` | `62d5945a02e0359e` |
| `videos/hurdles-110m-stadium-20261021/scripts/run-realism-v6.mjs` | `fc3346b5fbaa210f` |
| `videos/hurdles-110m-stadium-20261021/scripts/serve.py` | `b068baceaaf0454b` |
| `videos/hurdles-110m-stadium-20261021/scripts/task.mjs` | `8da72f81f6b1eff5` |
| `videos/hurdles-110m-stadium-20261021/src/app.js` | `77eed43ce80a8dfa` |
| `videos/hurdles-110m-stadium-20261021/src/cg-player.js` | `56d444c68d42cb11` |
| `videos/hurdles-110m-stadium-20261021/src/live-action.js` | `70a838be153bf8b6` |
| `videos/hurdles-110m-stadium-20261021/src/remotion.tsx` | `54cf1c52e36c1947` |
