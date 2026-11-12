---
type: video-production-dossier
project: video-creation
video_id: rukawa-continuous-rig-study-20261109
captured: '2026-11-12'
status: 5 秒绑定研究；外观与完整 20 秒故事未完成
source_fingerprint: d7c239a9d1c44fa3e605e33e6a65c16601442d7c3eed4270d168488a69bd62d6
tags:
- video-creation
- production-capture
---

# 流川枫 · 连续蒙皮与 IK 动作研究

<!-- capture-source-sha256:d7c239a9d1c44fa3e605e33e6a65c16601442d7c3eed4270d168488a69bd62d6 -->

本次为指定项目的材料回溯与归档。制作结论来自项目记录/源码，未找回关闭的制作对话，也未重新执行生成、渲染、音频合成或用户验收。

## 当前结论与制作路线

**状态：5 秒绑定研究；外观与完整 20 秒故事未完成。**

MakeHuman 连续网格 → 12 套骨架 → 接触/IK → 原生动画测试。

1280×720、30fps、150 帧。用连续身体/球衣/短裤蒙皮替代姿态图切换，包含六名球员与六名观众。当前只验证动作基础，角色仍是占位外观。

## 关键算法、实现与适用边界

根路径与 foot_plan 决定世界坐标支撑脚，hip_delta 控制起跳/落地，held_ball 给出手掌目标，肘膝 IK 保持连接。相邻四元数统一符号避免插值翻转；弯曲平面连续传递以稳定肘部求解，并以弧形手腕轨迹避开奇异急转。

## 声音与字幕

无配音与配乐。

## 失败、修正与验收边界

曾修正导入骨架旋转模式、出手左腕目标不连续、固定极向量导致肘部翻转、右腕方向切换。微小接触误差和骨骼间隙只证明数值约束，不证明自然表演、漫画造型或用户验收。

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

> [!info]- 当前 Composition / 时间轴源码：videos/rukawa-continuous-rig-study-20261109/src/Root.tsx，L1–26
> ```tsx
> import React, {useEffect,useState} from 'react';
> import {AbsoluteFill,Composition,continueRender,delayRender,staticFile,useCurrentFrame,useVideoConfig} from 'remotion';
> import {Video} from '@remotion/media';
> 
> const Film: React.FC = () => {
>   const frame=useCurrentFrame();
>   const {fps}=useVideoConfig();
>   const [fontHandle]=useState(()=>delayRender('Loading local Chinese font'));
>   useEffect(()=>{
>     const font=new FontFace('StudyChinese',`url(${staticFile('fonts/NotoSansSC.ttf')})`);
>     font.load().then(f=>{document.fonts.add(f);continueRender(fontHandle);}).catch(()=>continueRender(fontHandle));
>   },[fontHandle]);
>   const t=frame/fps;
>   const phase=t<.55?'持球准备':t<1.62?'屈膝蓄力':t<1.94?'起跳举球':t<2.76?'出手与落地':t<3.16?'保持随球动作':'入网与多人反应';
>   return <AbsoluteFill style={{backgroundColor:'#273536',fontFamily:'StudyChinese, sans-serif'}}>
>     <Video name="Continuous mesh animation" src={staticFile('rig-study-picture.mp4')+'?v=continuous-rig-4'} muted premountFor={fps} style={{position:'absolute',width:'100%',height:'100%'}}/>
>     <div style={{position:'absolute',top:24,left:28,color:'#fff',backgroundColor:'#18282be6',padding:'12px 18px',borderLeft:'4px solid #e5bd6a'}}>
>       <div style={{fontSize:26}}>连续蒙皮 · 动作测试</div>
>       <div style={{fontSize:17,color:'#c4d1d1',marginTop:5}}>6 名球员 / 6 名观众 / 固定机位观察</div>
>     </div>
>     <div style={{position:'absolute',top:30,right:28,color:'#edf0e8',fontSize:20,backgroundColor:'#18282bdb',padding:'10px 16px'}}>外观未定稿</div>
>     <div style={{position:'absolute',bottom:26,left:28,color:'#fff',fontSize:23,backgroundColor:'#18282be8',padding:'11px 17px'}}>{phase}</div>
>     <div style={{position:'absolute',bottom:26,right:28,color:'#fff',fontSize:19,fontVariantNumeric:'tabular-nums',backgroundColor:'#18282be8',padding:'11px 17px'}}>{t.toFixed(2)} s / 5.00 s</div>
>   </AbsoluteFill>;
> };
> export const Root:React.FC=()=> <Composition id="ContinuousRigStudy" component={Film} durationInFrames={150} fps={30} width={1280} height={720}/>;
> ```

源时间轴、生成帧数、所用区间与最终时长分别记录；历史文档或候选参数不自动覆盖当前采用配置。

## 参考来源与借鉴说明

在本次盘点的项目自有材料中，未找到可明确归属本片的外部参考视频或 X 帖子链接；这不等于历史上没有参考。

本地造型/参考文件（本轮仅核对存在，不宣称重新看过全部图像）：

- `videos/rukawa-continuous-rig-study-20261109/public/references/rukawa-approved.png`

### 项目记录的技术、内容或素材来源

这些 URL 来自项目文件，尚不自动证明本片采用；除本节明确标注的访问外，本轮未逐一在线重读。

| 链接 | 记录文件 |
| --- | --- |
| [www.adobe.com](http://www.adobe.com/) | `videos/rukawa-continuous-rig-study-20261109/docs/NOTO-OFL.txt` |
| [scripts.sil.org](https://scripts.sil.org/OFL) | `videos/rukawa-continuous-rig-study-20261109/docs/NOTO-OFL.txt` |
| [static.makehumancommunity.org](https://static.makehumancommunity.org/about/license.html) | `videos/rukawa-continuous-rig-study-20261109/docs/provenance.md` |
| [github.com](https://github.com/google/fonts/tree/main/ofl/notosanssc) | `videos/rukawa-continuous-rig-study-20261109/docs/provenance.md` |

## 重现入口与修改位置

以下是 package.json 中的实际命令映射，供在对应视频目录使用；本轮未执行这些制作命令。系统依赖、前置模型/素材和环境准备见后附 README。

| npm 入口 | 实际命令 |
| --- | --- |
| `npm run dev` | `node scripts/run.mjs dev` |
| `npm run scene` | `node scripts/run.mjs scene` |
| `npm run render:scene` | `node scripts/run.mjs frames` |
| `npm run encode` | `node scripts/run.mjs encode` |
| `npm run render` | `node scripts/run.mjs render` |
| `npm run check` | `node scripts/run.mjs check` |
| `npm run package` | `node scripts/run.mjs package` |

### 算法源码摘录

> [!info]- videos/rukawa-continuous-rig-study-20261109/scripts/build_rig_study.py，L1–230
> ```python
> """Continuous skeletal deformation study. Real weighted human mesh, no sprite swaps."""
> import bpy,bmesh,math,json,sys,time
> from pathlib import Path
> from mathutils import Vector,Matrix,Quaternion
> R=Path(__file__).resolve().parents[1];FPS=30;COUNT=150
> bpy.ops.object.select_all(action='SELECT');bpy.ops.object.delete(use_global=False)
> bpy.ops.import_scene.gltf(filepath=str(R/'public/models/human-base.glb'))
> scene=bpy.context.scene
> source_arm=next(o for o in scene.objects if o.type=='ARMATURE')
> source_body=bpy.data.objects['AthleteBody'];source_eye=bpy.data.objects['Athletehigh-poly'];source_shoes=bpy.data.objects['Athleteshoes05']
> for o in list(scene.objects):
>  if o not in [source_arm,source_body,source_eye,source_shoes] and o.type=='MESH':bpy.data.objects.remove(o,do_unlink=True)
> rest={b.name:b.matrix_local.copy() for b in source_arm.data.bones}
> rest_inv={n:m.inverted() for n,m in rest.items()}
> bones=list(source_arm.data.bones)
> head={b.name:b.head_local.copy() for b in bones};tail={b.name:b.tail_local.copy() for b in bones}
> parent={b.name:b.parent.name if b.parent else None for b in bones}
> 
> def sat(x):return max(0.,min(1.,x))
> def smooth(a,b,t):u=sat((t-a)/(b-a));return u*u*u*(10+u*(-15+6*u))
> def mix(a,b,u):return Vector(a).lerp(Vector(b),u)
> def hermite(t,a,b,va,vb,sa=0,sb=0):
>  u=sat((t-a)/(b-a));d=b-a
>  return (2*u**3-3*u*u+1)*va+(u**3-2*u*u+u)*d*sa+(-2*u**3+3*u*u)*vb+(u**3-u*u)*d*sb
> 
> def ballistic(t,start,height):
>  duration=math.sqrt(8*height/9.81);u=t-start
>  return (4*height/duration*u-4*height/duration**2*u*u) if 0<u<duration else 0.
> 
> def hip_delta(t,kind):
>  if kind=='shooter':
>   if t<.55:return 0.
>   if t<1.25:return hermite(t,.55,1.25,0,-.20)
>   if t<1.62:return hermite(t,1.25,1.62,-.20,0,0,math.sqrt(2*9.81*.41))
>   land=1.62+math.sqrt(8*.41/9.81)
>   if t<land:return ballistic(t,1.62,.41)
>   if t<land+.17:return hermite(t,land,land+.17,0,-.18,-math.sqrt(2*9.81*.41),0)
>   return hermite(t,land+.17,land+.68,-.18,0)
>  if kind=='defender':
>   start=1.88;height=.23;land=start+math.sqrt(8*height/9.81)
>   if t<1.36:return -.05
>   if t<start:return hermite(t,1.36,start,-.05,0,0,math.sqrt(2*9.81*height))-.09*math.sin(math.pi*sat((t-1.36)/(start-1.36)))**2
>   if t<land:return ballistic(t,start,height)
>   if t<land+.16:return hermite(t,land,land+.16,0,-.12,-math.sqrt(2*9.81*height),0)
>   return hermite(t,land+.16,land+.56,-.12,-.04)
>  return -.035-.016*math.sin(t*2.1)**2
> 
> def mat(n,c):
>  m=bpy.data.materials.new(n);m.diffuse_color=(*c,1);m.use_nodes=True;m.node_tree.nodes['Principled BSDF'].inputs['Base Color'].default_value=(*c,1);return m
> skin=mat('Study / skin',(.72,.47,.32));ivory=mat('Study / ivory',(.86,.84,.75));black=mat('Study / ink',(.025,.032,.045));red=mat('Study / Shohoku red',(.62,.024,.036));navy=mat('Study / opponent navy',(.03,.07,.16));eyewhite=mat('Study / eye white',(.85,.84,.77));iris=mat('Study / iris',(.015,.025,.04))
> source_body.data.materials.clear();source_body.data.materials.append(skin)
> source_eye.data.materials.clear();source_eye.data.materials.append(eyewhite)
> source_shoes.data.materials.clear();source_shoes.data.materials.append(ivory)
> for p in source_body.data.polygons:p.use_smooth=True
> 
> def group_copy(src,dst):
>  for g in src.vertex_groups:
>   ng=dst.vertex_groups.new(name=g.name)
>   for v in src.data.vertices:
>    for gg in v.groups:
>     if gg.group==g.index and gg.weight>0:ng.add([v.index],gg.weight,'REPLACE')
> 
> def shirt_cond(v):
>  x=abs(v.x);u=smooth(.055,.115,x);upper=1.495+.091*u;return 1.09<v.z<upper and x<.235-.073*smooth(1.34,1.54,v.z)
> def shorts_cond(v):return .775<v.z<1.10 and abs(v.x)<.34
> 
> def garment(name,cond,pad,material):
>  fs=[f for f in source_body.data.polygons if cond(f.center)];used=sorted({i for f in fs for i in f.vertices});mp={i:j for j,i in enumerate(used)}
>  d=bpy.data.meshes.new(name);d.from_pydata([source_body.data.vertices[i].co+source_body.data.vertices[i].normal*pad for i in used],[],[[mp[i] for i in f.vertices] for f in fs]);d.update();o=bpy.data.objects.new(name,d);scene.collection.objects.link(o);d.materials.append(material)
>  for g in source_body.vertex_groups:
>   vg=o.vertex_groups.new(name=g.name)
>   for old,new in mp.items():
>    for item in source_body.data.vertices[old].groups:
>     if item.group==g.index and item.weight>0:vg.add([new],item.weight,'REPLACE')
>  bm=bmesh.new();bm.from_mesh(d);bmesh.ops.remove_doubles(bm,verts=list(bm.verts),dist=.00002);bmesh.ops.recalc_face_normals(bm,faces=list(bm.faces));bm.to_mesh(d);bm.free()
>  for p in d.polygons:p.use_smooth=True
>  am=o.modifiers.new('Continuous cloth skinning','ARMATURE');am.object=source_arm;am.use_deform_preserve_volume=True
>  solid=o.modifiers.new('Cloth thickness','SOLIDIFY');solid.thickness=.002;solid.offset=0
>  o.parent=source_arm;return o
> shirt=garment('Continuous jersey',shirt_cond,.014,red);shorts=garment('Continuous shorts',shorts_cond,.022,red)
> # Remove only the hidden body faces; keep each deform layer with the mesh.
> bm=bmesh.new();bm.from_mesh(source_body.data)
> bmesh.ops.delete(bm,geom=[f for f in bm.faces if shirt_cond(f.calc_center_median()) or shorts_cond(f.calc_center_median())],context='FACES')
> bm.to_mesh(source_body.data);bm.free();source_body.data.update()
> for o in [source_body,source_eye,source_shoes]:
>  for mod in o.modifiers:
>   if mod.type=='ARMATURE':mod.use_deform_preserve_volume=True
> # A simple scalp and eyes keep this motion-study model readable; appearance is not final.
> scalp=garment('Study scalp',lambda v:v.z>1.825 or (v.z>1.75 and v.y>-.025),.006,black)
> # scalp uses a rigid head weight to avoid deforming with unrelated nearby bones.
> scalp.vertex_groups.clear();g=scalp.vertex_groups.new(name='head');g.add(list(range(len(scalp.data.vertices))),1,'REPLACE')
> for side in ['L','R']:
>  center=head['eye.'+side]+Vector((0,-.011,0))
>  bpy.ops.mesh.primitive_uv_sphere_add(segments=12,ring_count=8,location=center)
>  o=bpy.context.object;o.name='Study iris '+side;o.scale=(.006,.003,.006);bpy.ops.object.transform_apply(location=True,rotation=False,scale=True);o.data.materials.append(iris);o.parent=source_arm
>  vg=o.vertex_groups.new(name='eye.'+side);vg.add(list(range(len(o.data.vertices))),1,'REPLACE');mod=o.modifiers.new('Eye bone','ARMATURE');mod.object=source_arm
> source_meshes=[o for o in scene.objects if o.type=='MESH']
> # Shoe contact data is read from the actual source mesh, separately for each foot.
> foot_data={}
> for side,sign in [('L',1),('R',-1)]:
>  vs=[v.co.copy() for v in source_shoes.data.vertices if v.co.x*sign>0]
>  zmin=min(v.z for v in vs);ymin=min(v.y for v in vs)
>  pivot=Vector((head['foot.'+side].x,ymin,zmin))
>  foot_data[side]={'sole_min':zmin,'pivot':pivot}
> 
> def cube(n,loc,dims,ma):
>  bpy.ops.mesh.primitive_cube_add(size=1,location=loc);o=bpy.context.object;o.name=n;o.dimensions=dims;bpy.ops.object.transform_apply(location=False,rotation=False,scale=True);o.data.materials.append(ma);return o
> 
> def curve(n,pts,r,ma):
>  d=bpy.data.curves.new(n,'CURVE');d.dimensions='3D';d.bevel_depth=r;d.bevel_resolution=1;s=d.splines.new('POLY');s.points.add(len(pts)-1)
>  for p,v in zip(s.points,pts):p.co=(*v,1)
>  o=bpy.data.objects.new(n,d);scene.collection.objects.link(o);d.materials.append(ma);return o
> wood=mat('Court warm maple',(.55,.38,.21));green=mat('Court deep teal',(.08,.24,.25));gray=mat('Study wall',(.55,.61,.62));line=mat('Court line',(.89,.85,.72));orange=mat('Basketball',(.78,.27,.055))
> cube('Court floor',(0,-1,-.05),(14,16,.1),wood);cube('Paint',(0,-4,.003),(4.9,4,.006),green)
> for x in [-6.5,6.5]:curve('Sideline',[(x,-8,.012),(x,6,.012)],.022,line)
> curve('Three point arc',[(6.75*math.cos(a),-5.3+6.75*math.sin(a),.012) for a in [i*math.pi/120 for i in range(121)]],.025,line)
> curve('Key outline',[(-2.45,-6,.015),(-2.45,-2,.015),(2.45,-2,.015),(2.45,-6,.015)],.024,line)
> for y in [-7,-6,-5,-4,-3,-2,-1,0,1,2,3,4,5]:curve('Floor board seam',[(-6.5,y,.008),(6.5,y,.008)],.002,line)
> cube('Back wall',(-5.6,-.7,1.8),(.15,14,3.6),gray)
> for i in range(2):cube('Study stand',(-4.3-i*.7,-.4,.18+i*.4),(.8,11,.35),green)
> # Native ring and a simple readable backboard in the same court space.
> cube('Backboard',(0,-5.7,3.48),(1.83,.045,1.06),ivory)
> curve('Orange rim',[(.225*math.cos(a),-5.3+.225*math.sin(a),3.05) for a in [i*math.tau/64 for i in range(65)]],.018,orange)
> for i in range(12):
>  a=i*math.tau/12;curve('Net cord',[(.225*math.cos(a),-5.3+.225*math.sin(a),3.05),(.14*math.cos(a+.3),-5.3+.14*math.sin(a+.3),2.64)],.005,ivory)
> 
> specs=[('Shooter 11','shooter',(-.8,2.4,0),0,red,11),('Defender 6','defender',(-.65,.85,0),math.pi,ivory,6),('Rebound 10','rebound',(-2.2,-1.0,0),-.18,red,10),('Guard 7','guard',(2.3,-.3,0),.22,red,7),('Center 4','center',(-.6,-3.5,0),.15,red,4),('Opponent 5','opponent',(.95,-3.0,0),-.45,ivory,5)]
> for i in range(6):specs.append(('Spectator '+str(i+1),'crowd',(-4.25,-3.6+i*1.3,.45),math.pi/2,navy if i%2 else green,20+i))
> font=bpy.data.fonts.load(str(R/'public/fonts/NotoSansSC.ttf'))
> def number(arm,body,loc,rotation,size,material):
>  d=bpy.data.curves.new('Uniform label','FONT');d.body=body;d.align_x='CENTER';d.size=size;d.font=font;d.extrude=.0004;o=bpy.data.objects.new('Uniform '+body,d);scene.collection.objects.link(o);o.location=loc;o.rotation_euler=rotation;d.materials.append(material)
>  bpy.ops.object.select_all(action='DESELECT');bpy.context.view_layer.objects.active=o;o.select_set(True);bpy.ops.object.convert(target='MESH');o=bpy.context.object;bpy.ops.object.transform_apply(location=True,rotation=True,scale=True);o.select_set(False);o.parent=arm
>  vg=o.vertex_groups.new(name='spine02');vg.add(list(range(len(o.data.vertices))),1,'REPLACE');mod=o.modifiers.new('Chest label binding','ARMATURE');mod.object=arm
>  return o
> actors=[]
> for idx,(name,kind,origin,angle,col,num) in enumerate(specs):
>  arm=source_arm.copy();arm.data=source_arm.data.copy();arm.name=name+' / deform rig';scene.collection.objects.link(arm);arm.animation_data_clear();arm.rotation_mode='XYZ'
>  meshes=[]
>  for src in source_meshes:
>   ob=src.copy();ob.data=src.data;ob.name=name+' / '+src.name;scene.collection.objects.link(ob);ob.parent=arm;ob.animation_data_clear()
>   for mod in ob.modifiers:
>    if mod.type=='ARMATURE':mod.object=arm
>   if src in [shirt,shorts]:
>    for slot in ob.material_slots:slot.link='OBJECT';slot.material=col
>   meshes.append(ob)
>  labelmat=ivory if col==red else navy
>  number(arm,str(num),(0,-.169,1.20),(math.pi/2,0,0),.15,labelmat)
>  number(arm,str(num),(0,.145,1.20),(math.pi/2,0,math.pi),.17,labelmat)
>  if kind!='crowd':number(arm,'SHOHOKU' if col==red else 'GUEST',(0,-.159,1.40),(math.pi/2,0,0),.036,labelmat)
>  actors.append({'name':name,'kind':kind,'arm':arm,'origin':Vector(origin),'angle':angle,'meshes':meshes,'idx':idx})
> for ob in source_meshes:bpy.data.objects.remove(ob,do_unlink=True)
> bpy.data.objects.remove(source_arm,do_unlink=True)
> 
> # Motion helpers: planted world-space contacts, two-segment IK, skinning matrices.
> ik_state={}
> def ik(a,b,l1,l2,pole,key=None):
>  a,b=Vector(a),Vector(b);dvec=b-a;dist=dvec.length;d=min(max(dist,.00001),l1+l2-.0001);v=dvec.normalized();h=(l1*l1-l2*l2+d*d)/(2*d);p=Vector(pole);p=(p-v*p.dot(v)).normalized()
>  if key is not None:
>   if key in ik_state:
>    old=ik_state[key];old=(old-v*old.dot(v)).normalized();angle=math.atan2(v.dot(old.cross(p)),max(-1,min(1,old.dot(p))))
>    # Transport the bend plane continuously; a preferred pole crossing the limb axis cannot flip the elbow.
>    p=Quaternion(v,max(-.10,min(.10,angle*.35)))@old
>   ik_state[key]=p.copy()
>  return a+v*h+p*math.sqrt(max(0,l1*l1-h*h)),max(0,dist-l1-l2)
> 
> def segment(over,names,old_a,old_b,new_a,new_b,old_normal=None,new_normal=None):
>  if old_normal is not None:
>   oa=(old_b-old_a).normalized();na=(new_b-new_a).normalized();on=Vector(old_normal).normalized();nn=Vector(new_normal).normalized()
>   of=Matrix((oa,on.cross(oa).normalized(),on)).transposed();nf=Matrix((na,nn.cross(na).normalized(),nn)).transposed();q=(nf@of.transposed()).to_quaternion()
>  else:q=(old_b-old_a).rotation_difference(new_b-new_a)
>  for n in names:
>   m=q.to_matrix().to_4x4()@rest[n];m.translation=new_a+q@(head[n]-old_a);over[n]=m
> 
> def swing(t,a,b):u=sat((t-a)/(b-a));return smooth(a,b,t),math.sin(math.pi*u)**2 if a<t<b else 0.
> 
> def root_position(actor,t):
>  o=actor['origin'].copy();k=actor['kind']
>  if k=='defender':o.y+=.54*smooth(.25,1.48,t)
>  if k=='rebound':o.x+=.30*smooth(.6,1.6,t);o.y-=.18*smooth(1.4,2.7,t)
>  if k=='guard':o.x-=.25*smooth(.4,1.6,t)
>  if k=='center':o.x+=.15*smooth(1.4,2.6,t)
>  if k=='opponent':o.x-=.15*smooth(1.3,2.5,t)
>  return o
> 
> def foot_plan(actor,side,t):
>  o=actor['origin'];k=actor['kind'];rot=Matrix.Rotation(actor['angle'],3,'Z')
>  offset=head['foot.'+side].copy();offset.x=.165 if side=='L' else -.165
>  if k in ['center','opponent']:offset.x*=1.22
>  offset.y=.035 if side=='L' else -.07;offset.z-=foot_data[side]['sole_min'];world=o+rot@offset;lift=0.;plant=True;pitch=0.
>  moves=[]
>  if k=='defender':moves=[(.25,.65,(0,.30,0))] if side=='R' else [(.72,1.15,(0,.38,0))]
>  elif k=='rebound':moves=[(.65,1.10,(.25,-.10,0))] if side=='R' else [(1.22,1.72,(.32,-.15,0))]
>  elif k=='guard':moves=[(.45,.90,(-.22,0,0))] if side=='L' else [(1.0,1.45,(-.25,0,0))]
>  elif k=='center':moves=[(1.50,1.95,(.18,0,0))] if side=='L' else [(2.02,2.45,(.15,0,0))]
>  elif k=='opponent':moves=[(1.4,1.85,(-.18,0,0))] if side=='R' else [(1.98,2.4,(-.15,0,0))]
>  for a,b,delta in moves:
>   u,arch=swing(t,a,b);world+=Vector(delta)*u
>   if a<t<b:lift+=.085*arch;pitch=.10*math.sin(math.pi*sat((t-a)/(b-a)));plant=False
>  if k in ['shooter','defender']:
>   start,height=(1.62,.41) if k=='shooter' else (1.88,.23);end=start+math.sqrt(8*height/9.81)
>   if start<t<end:lift+=ballistic(t,start,height);plant=False;pitch=.20*(1-smooth(start+.04,end-.04,t))
>   elif start-.18<t<=start:pitch=.20*smooth(start-.18,start,t);plant=False
>  world.z+=lift
>  pivot=foot_data[side]['pivot']-head['foot.'+side]
>  mapping=Matrix.Translation(world)@Matrix.Rotation(actor['angle'],4,'Z')@Matrix.Translation(pivot)@Matrix.Rotation(pitch,4,'X')@Matrix.Translation(-pivot)@Matrix.Translation(-head['foot.'+side])
>  return mapping,plant
> 
> RELEASE=1.94
> 
> def held_ball(t):
>  lift=smooth(1.13,1.84,t);z=1.30+.80*lift+hip_delta(t,'shooter');return Vector((-.08,-.34,z))
> release_local=held_ball(RELEASE);shooter=actors[0];release_world=shooter['origin']+release_local
> flight=1.22;rim=Vector((0,-5.3,3.05));swish=RELEASE+flight
> 
> def ball_world(t):
>  if t<=RELEASE:return shooter['origin']+held_ball(t)
>  u=(t-RELEASE)/flight
>  if u<=1:
>   p=release_world.lerp(rim,u);p.z+=4.905*flight*flight*u*(1-u);return p
>  dt=t-swish;v=(rim-release_world)/flight;v.z-=4.905*flight;impact=(v.z+math.sqrt(v.z*v.z+19.62*(3.05-.12)))/9.81
>  # Ring/net drag slows horizontal motion continuously; vertical velocity carries through the rim.
>  xy=Vector((v.x,v.y,0))*((1-math.exp(-25*dt))/25)
>  if dt<impact:return rim+xy+Vector((0,0,v.z*dt-4.905*dt*dt))
>  age=dt-impact;dur=.54;hh=.35
>  while age>dur and dur>.06:age-=dur;dur*=.62;hh*=.3844
>  return Vector((xy.x,-5.3+xy.y,.12+4*hh*min(1,age/dur)*(1-min(1,age/dur))))
> 
> bpy.ops.mesh.primitive_uv_sphere_add(segments=24,ring_count=12,radius=.12);ball=bpy.context.object;ball.name='Single continuous basketball';ball.data.materials.append(orange)
> for poly in ball.data.polygons:poly.use_smooth=True
> for axis in range(3):
> ```
> 
> 本摘录到第 230 行；完整文件共 352 行，余下实现请按源文件继续阅读。

## 完整关键提示词与生成记录

以下保留项目自有提示词、候选参数与版本。存在失败候选和历史参数；最终采用以 selected-takes、edit-decisions 与制作复盘为准，不能将所有候选都写成已采用。

未发现实际生成提示词或推理执行记录；不从脚手架、模型安装清单推测生成过程。

## 已有产物与检查依据

文件存在、技术检查、画面检查与用户验收分别看待。下表是本次文件盘点，旧版与当前版可能共存。MP4/HTML 按内容计算 SHA-256，ZIP 仅记录文件大小和修改时间供增量判断。

| 本地产物 | 字节数 | SHA-256 |
| --- | ---: | --- |
| `videos/rukawa-continuous-rig-study-20261109/outputs/continuous-rig-study-5s.mp4` | 1040819 | `c1cd7e160182815e379a1db2e438fe6747796bf2f5d8f0daf1dd518b347d8f73` |
| `videos/rukawa-continuous-rig-study-20261109/outputs/continuous-rig-study-project.zip` | 57507694 | `ZIP：仅元数据` |

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
>       "nb_frames": "150"
>     }
>   ],
>   "format": {
>     "duration": "5.000000",
>     "size": "1040819"
>   },
>   "file": "outputs/continuous-rig-study-5s.mp4",
>   "scope": "本次只读取媒体头；未全片解码、播放或重新进行视觉/听觉验收。"
> }
> ```


已有检查报告路径（未在本轮重新执行）：

- `videos/rukawa-continuous-rig-study-20261109/outputs/checks/project-check.json`
- `videos/rukawa-continuous-rig-study-20261109/outputs/checks/rig-audit.json`
- `videos/rukawa-continuous-rig-study-20261109/outputs/checks/rotation-audit.json`
- `videos/rukawa-continuous-rig-study-20261109/outputs/checks/source-rig.json`

## 制作方法与迭代记录选编

下面是项目自有文档的归档摘录，保留原阶段与原检查表述，个人路径已泛化、相对链接已注明源项目位置。它们属于来源陈述，不代表本次重新完成其中的生成或验收。

> [!info]- 来源文档：videos/rukawa-continuous-rig-study-20261109/README.md
> # 篮球连续蒙皮动作测试
> 
> 当前交付是 5 秒、1280×720、30 fps 的本地绑定验证，包含 6 名球员与 6 名观众。它用于检查持球、屈膝、起跳、出手、落地及多人反应的衔接；尚未完成流川枫的动漫脸型、发型、宽松球服，也不是最终的 20 秒成片。已确认的造型图保存在 `public/references/rukawa-approved.png`。
> 
> 每个人物使用同一个连续人体网格及原生 163 骨骼权重，衣服也绑定到骨骼；动作使用世界坐标脚底约束、双段 IK 和连续轨迹烘焙。没有切换角色姿态图片。两支球队、看台与机位均为动作检查占位。
> 
> ## 独立安装与预览
> 
> 需要系统已安装 Node.js、Blender 5.2、FFmpeg 和 Google Chrome。依赖版本锁定在本目录。解压工程后，在其父目录执行以下命令；当前工作区也可直接进入这个视频目录。
> 
> ```sh
> cd rukawa-continuous-rig-study-20261109
> mkdir -p .cache/tmp .cache/npm
> TMPDIR="$PWD/.cache/tmp" TMP="$PWD/.cache/tmp" TEMP="$PWD/.cache/tmp" npm ci --cache .cache/npm
> npm run dev
> ```
> 
> 打开 http://localhost:3094/ContinuousRigStudy 。现成的本地画面素材已包含在 `public/rig-study-picture.mp4`，预览不需要重新渲染 Blender。
> 
> ## 重建、检查与导出
> 
> ```sh
> npm run scene                         # 重建绑定与 150 帧动作，约 30 MB 的 .blend
> node scripts/run.mjs preview           # 关键姿态 PNG
> node scripts/run.mjs audit             # 从实际骨骼、蒙皮鞋面采样接触误差
> npm run render:scene                   # 本地 Blender 渲染 150 帧
> npm run encode                         # 将帧序列合成画面素材
> npm run render                         # Remotion 导出带测试标签的 MP4
> npm run check                          # 类型、路径隔离、视频解码与接触误差检查
> npm run package                        # 当前工程的独立 ZIP
> ```
> 
> `run.mjs` 将工作目录、临时文件、npm 缓存、XDG 缓存及 Blender 用户配置限定在本视频目录；使用系统已安装 Chrome，不下载浏览器。直接运行 npm 安装时使用上面的环境变量。所有生成文件和日志保留在本目录。压缩包排除可重建的 `node_modules`、缓存和 PNG 全帧序列，保留锁文件、源素材、绑定工程、成片及检查报告。
> 
> ## 修改素材、动作和时间轴
> 
> - `scripts/build_rig_study.py`：`specs` 定义角色、球队与位置；`root_position`/`foot_plan` 定义支撑脚和移动；`hip_delta` 定义起跳与落地；`held_ball`/`ball_world` 定义持球和抛物线；逐帧绑定段定义手腕和肘膝 IK。动作曲线使用连续插值，并修正相邻四元数的符号以避免插值翻转。
> - `public/models/human-base.glb`：独立复制的人体基础网格与蒙皮权重。重新生成场景不读取任何其他视频。
> - `src/Root.tsx`：测试标签、阶段说明、Remotion 5 秒时间轴。改变长度时须同步 Blender 帧数、渲染脚本和编码帧数。
> - `public/fonts/NotoSansSC.ttf`：本地测试标签字体。
> - 本片没有配音或配乐。需要音频时将有权使用的文件放入本目录 `public/audio/`，在 Root 中通过 `@remotion/media` 的 Audio 引用，并按新时间轴调整。
> 
> ## 验证边界
> 
> `outputs/checks/rig-audit.json` 采样真实绑定结果，检查脚底在支撑期的滑动、手掌目标、骨骼连接和尺度稳定性。它不能证明动作已经达到成片的自然程度。需要结合视频确认肩肘姿态、着地节奏、手指和球的关系。该测试保留占位外观，不能作为已完成动漫造型或 20 秒故事的证明。
> 
> 基础人体来源与许可见 `docs/provenance.md`。已确认的动漫参考仅作为本项目制作参考。

> [!info]- 来源文档：videos/rukawa-continuous-rig-study-20261109/docs/production-status.md
> # 本轮制作范围
> 
> 用户选择：坚持本地制作，重建可连续变形的人物绑定。
> 
> 本轮可检查结果：5 秒连续蒙皮动作测试与可编辑 Blender/Remotion 工程。原 2D 九姿态和三姿态切图路线不再用于此测试。
> 
> 已实现：连续身体/球衣/短裤蒙皮；12 个独立骨架；世界坐标支撑脚；膝肘 IK；持球手掌目标；起跳、落地和随球动作；不同相位的观众鼓掌；背景球员移动、举手和反应。
> 
> 制作中发现并修正：导入骨架的旋转模式使角色转向未生效；出手瞬间左腕的目标位置不衔接；固定肘部极向量导致抬手途中翻转。检查真实骨骼与蒙皮后修正，未使用图像区域变化率替代动作质量检查。
> 
> 仍需完成：匹配已确认的流川枫脸型、发型与漫画球服，区分樱木等配角造型，完成 10 人正式场上站位、完整看台、20 秒分镜、镜头和声音。数值接触通过不代表上述外观和自然动作已获用户认可。
> 
> 补充修正：持球到随球手势的方向参考导致右腕瞬间翻转，现用稳定四元数插值连接；肘部弯曲平面在逐帧求解时连续传递，骨骼朝向由整条肢体平面确定。收臂和举手加入向前弧线，避免腕部轨迹过于贴近肩部，引发 IK 急转。

> [!info]- 来源文档：videos/rukawa-continuous-rig-study-20261109/docs/validation.md
> # 本地绑定验证记录
> 
> 本轮结果：5 秒 / 150 帧 / 30 fps。仅验证连续绑定与动作基础，外观未定稿，完整 20 秒视频未完成。
> 
> ## 真实变形结果采样
> 
> - 支撑期鞋底顶点最大单帧位移：0.000987 毫米。
> - 支撑期鞋底最低点最大离地绝对误差：0.000685 毫米。
> - 手掌持球目标最大误差：0.000961 毫米。
> - 相邻骨骼连接最大间隙：0.000565 毫米。
> - 最大局部骨骼单帧转角：27.58 度，发生于跳投落地；相邻四元数符号一致。
> - 角色为同一拓扑连续蒙皮，12 套独立骨骼；没有更换姿态图。
> - 没有链接外部 Blender 库；字体与使用到的图片已打包在工程。
> 
> ## 视觉检查范围
> 
> 固定镜头检查持球、蹬地、出手、落地及抬手的关键帧，修正了遮挡、腕部方向翻转与肘部极向量翻转。肩肘自然程度、手指接触、群演表演和漫画造型仍需结合播放结果继续评估。测试没有以图像变化率或接触误差替代人物动作的审美验收。

> [!info]- 来源文档：videos/rukawa-continuous-rig-study-20261109/docs/provenance.md
> # 素材来源
> 
> - `public/models/human-base.glb`：本项目已有 MakeHuman 导出人体的独立副本（复制于 2026-11-09）。保留连续身体、眼球、鞋和 163 骨骼权重。原发型未使用；测试中的头皮、球服、场地、篮球、文字和动作由本工程脚本创建。
> - MakeHuman 核心素材及导出模型的 CC0 许可依据：<https://static.makehumancommunity.org/about/license.html>；许可文本保留在 `MAKEHUMAN-ASSETS-LICENSE.md`。本工程没有引用 Renderpeople 或 Rocketbox 研究素材。
> - `public/references/rukawa-approved.png`：本会话已确认的 AI 动漫原型图的独立副本，仅作后续外观适配参考。
> - `public/fonts/NotoSansSC.ttf`：Google Fonts 官方仓库的 Noto Sans SC 字体，SIL Open Font License 1.1，许可文本保存在 `NOTO-OFL.txt`。来源：<https://github.com/google/fonts/tree/main/ofl/notosanssc>。
> 
> 上述复制完成后，本工程不在运行时读取原视频路径。

## 证据清单与缺失信息

盘点的文本/报告文件 27 个，源文本、报告及媒体产物身份摘要为 `d7c239a9d1c44fa3e605e33e6a65c16601442d7c3eed4270d168488a69bd62d6`。哈希证明采集内容身份，不代表其中每个断言已验证。模型权重、缓存、依赖、第三方源码树和未指定的历史会话未采集；大型文本仅做哈希并按需要选读。

- 本次整理基于明确引用的 README、制作方法、配置、提示词、关键源码与已有报告。未重新观看全部参考/候选/成片，也未做人工听辨。
- 项目未提供的原始模型内部实现、未披露生成后端、未记录 seed/参数、真实费用、人工总工时和用户最终验收，均不补造。实际日志有记录的耗时见上文，流水线耗时不等于整个制作总耗时。
- 源项目内的路径只用于查证；本档案未复制大型素材或模型，也不改变源项目运行依赖。

| 已盘点证据文件 | SHA-256（前 16 位）/状态 |
| --- | --- |
| `videos/rukawa-continuous-rig-study-20261109/README.md` | `d1e76770cba5c4eb` |
| `videos/rukawa-continuous-rig-study-20261109/package-lock.json` | `1a183d17805b8416` |
| `videos/rukawa-continuous-rig-study-20261109/package.json` | `1f96f45762b39570` |
| `videos/rukawa-continuous-rig-study-20261109/remotion.config.ts` | `ec70b0acd4585a5d` |
| `videos/rukawa-continuous-rig-study-20261109/tsconfig.json` | `af262af0dd89b1e2` |
| `videos/rukawa-continuous-rig-study-20261109/docs/MAKEHUMAN-ASSETS-LICENSE.md` | `f6089cba01cb570a` |
| `videos/rukawa-continuous-rig-study-20261109/docs/NOTO-OFL.txt` | `1c05c68c34f97084` |
| `videos/rukawa-continuous-rig-study-20261109/docs/motion-trace.json` | `15595606e48c68ae`；内容超限未展开 |
| `videos/rukawa-continuous-rig-study-20261109/docs/production-status.md` | `b87955397e2c0efc` |
| `videos/rukawa-continuous-rig-study-20261109/docs/provenance.md` | `6fb5cf0989b6f86a` |
| `videos/rukawa-continuous-rig-study-20261109/docs/rig-manifest.json` | `c4d3402b14fc79d6` |
| `videos/rukawa-continuous-rig-study-20261109/docs/validation.md` | `e162420fef98242b` |
| `videos/rukawa-continuous-rig-study-20261109/outputs/checks/evaluated-joints.json` | `45f77e573a28d281`；内容超限未展开 |
| `videos/rukawa-continuous-rig-study-20261109/outputs/checks/project-check.json` | `b564123776e63b7a` |
| `videos/rukawa-continuous-rig-study-20261109/outputs/checks/rig-audit.json` | `243f45c523fd5b7e` |
| `videos/rukawa-continuous-rig-study-20261109/outputs/checks/rotation-audit.json` | `401fb9cb775c196c` |
| `videos/rukawa-continuous-rig-study-20261109/outputs/checks/source-rig.json` | `87f86af1e3952055` |
| `videos/rukawa-continuous-rig-study-20261109/scripts/audit_rig.py` | `4c46284aced02ffd` |
| `videos/rukawa-continuous-rig-study-20261109/scripts/audit_rotations.py` | `84c211e15dd3efe1` |
| `videos/rukawa-continuous-rig-study-20261109/scripts/build_rig_study.py` | `f35d0644c7e090d2` |
| `videos/rukawa-continuous-rig-study-20261109/scripts/check.py` | `ff8bf0cea5a16b5b` |
| `videos/rukawa-continuous-rig-study-20261109/scripts/inspect_rig.py` | `37f90d5d5ecb45fe` |
| `videos/rukawa-continuous-rig-study-20261109/scripts/package.py` | `24e6d87977908421` |
| `videos/rukawa-continuous-rig-study-20261109/scripts/render_scene.py` | `e33d39a8f47e49bb` |
| `videos/rukawa-continuous-rig-study-20261109/scripts/run.mjs` | `2e3aabb67febc7e1` |
| `videos/rukawa-continuous-rig-study-20261109/src/Root.tsx` | `965a2ac29d1995ed` |
| `videos/rukawa-continuous-rig-study-20261109/src/index.ts` | `309d14a1bb4ad80b` |
