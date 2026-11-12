---
type: video-production-dossier
project: video-creation
video_id: bottle-storm-recreation-20261106
captured: '2026-11-12'
status: 20 秒成片已存在；正式音轨沿用 14 秒文件
source_fingerprint: 89ef8d5ecb7ee12b5652e4d288c6ce32c3c99e992b203531fc80c56095db2aee
tags:
- video-creation
- production-capture
---

# 瓶中风暴 · 小兔搬积木与鱼群游动

<!-- capture-source-sha256:89ef8d5ecb7ee12b5652e4d288c6ce32c3c99e992b203531fc80c56095db2aee -->

本次为指定项目的材料回溯与归档。制作结论来自项目记录/源码，未找回关闭的制作对话，也未重新执行生成、渲染、音频合成或用户验收。

## 当前结论与制作路线

**状态：20 秒成片已存在；正式音轨沿用 14 秒文件。**

Python 场景与约束动画 → Blender Cycles/Metal → FFmpeg → Remotion 合成。

1280×720、24fps、480 帧。由 14 秒瓶中风暴扩展为 20 秒小兔搬积木、拿笔画花，再优化九条鱼的形体与运动。房间、玻璃瓶、水面、船、鱼在同一 Blender Scene 中，由统一光照、折射和遮挡形成空间关系。

## 关键算法、实现与适用边界

小兔：世界坐标支撑脚、双段 IK、单一物体轨迹维持抓握，画线与笔尖共用路径。鱼群：周期 CubicSpline 路线，弧长查找表将距离映射为曲线参数，朝向跟随切线；身体由前向后的波形弯曲，20 个 shape keys 连续混合，九鱼独立相位。海浪：解析波面，在船底四点取样驱动升沉/俯仰/横摇。闪电强度同步辅助灯，雷声滞后约 90ms。动作全部烘焙为原生关键帧，不能等同完整流体/刚体物理模拟。

## 声音与字幕

NumPy/SciPy 合成 48kHz 双声道环境与拟音；三阶 Butterworth 滤波区分风雨/海浪/雷声，脚步读动作审计时点，积木和画笔按事件触发。音频采样率、视频 fps 与 Cycles samples 为三个独立参数。

## 失败、修正与验收边界

旧鱼刚性漂移、全屋闪光过强、Metal 二进制归档编译崩溃分别通过曲线/形态重建、降低辅助闪光、兼容环境设置处理。正式 public/storm-soundscape.wav 只有 14 秒，制作手册记载成片第 15–20 秒静音。完整 20 秒声音分支已独立重建，但尚未应用到正式成片；本轮仅记录，不替换文件。

## 技术与模型版本

以下是本项目声明或锁定的依赖，安装清单本身不证明某项参与了最终画面。具体用途以制作路线、源码和生成记录交叉判断。

| 依赖 | package.json 声明 | 可解析的直接锁定版本 |
| --- | --- | --- |
| `@remotion/cli` | `4.0.541` | `4.0.541` |
| `@remotion/media` | `4.0.541` | `4.0.541` |
| `react` | `19.3.1` | `19.3.1` |
| `react-dom` | `19.3.1` | `19.3.1` |
| `remotion` | `4.0.541` | `4.0.541` |
| `@remotion/eslint-config-flat` | `4.0.541` | `4.0.541` |
| `@types/react` | `19.3.0` | `19.3.0` |
| `@types/web` | `0.0.166` | `0.0.166` |
| `eslint` | `10.12.0` | `10.12.0` |
| `prettier` | `3.8.1` | `3.8.1` |
| `typescript` | `5.9.3` | `5.9.3` |

> [!info]- requirements.txt
> ```text
> numpy==2.5.4
> scipy==1.18.1
> pillow==13.0.0
> ```


工具底层模型未公开时按“未披露”记录；不能由 imagegen 工具名猜测图像模型。未提供真实推理日志的配置不记作已运行。

## 镜头、时间轴与采用参数

> [!info]- 配置：docs/timeline.json
> ```json
> {
>   "fps": 24,
>   "duration": 20,
>   "frames": 480,
>   "lightning_seconds": [
>     2.05,
>     3.84,
>     5.02,
>     6.92,
>     8.27,
>     9.48,
>     10.84,
>     12.02
>   ],
>   "thunder_delay_seconds": 0.09,
>   "modes": [
>     [
>       0.17,
>       2.5,
>       0.38,
>       2.6
>     ],
>     [
>       0.08,
>       4.2,
>       -1.2,
>       3.7
>     ],
>     [
>       0.045,
>       7.3,
>       2.0,
>       5.1
>     ]
>   ],
>   "version": "bunny-artist",
>   "room_flash_scale": 0.3,
>   "bottle_flash_scale": 0.72,
>   "fish_revision": "v3",
>   "frame_dir": "outputs/frames-fish",
>   "picture": "bunny-fish-picture.mp4",
>   "final_video": "outputs/bottle-storm-bunny-fish-20s.mp4"
> }
> ```

> [!info]- 当前 Composition / 时间轴源码：videos/bottle-storm-recreation-20261106/src/Root.tsx，L1–9
> ```tsx
> import { MyComposition } from "./Composition";
> 
> export const RemotionRoot: React.FC = () => {
>   return (
>     <>
>       <MyComposition />
>     </>
>   );
> };
> ```

源时间轴、生成帧数、所用区间与最终时长分别记录；历史文档或候选参数不自动覆盖当前采用配置。

## 参考来源与借鉴说明

- 原帖：[guanmo_ai 的瓶中风暴帖子](https://x.com/guanmo_ai/status/2118258208045494688)。账号来自 URL；公开名称、原始发布时间在已读快照中未提供，不能将抓取日期当发布时间。
- 本地快照：`docs/source-post.json`，记录抓取日 2026-11-06；参考图位于 `public/reference/`，鱼群对照在 `public/reference/fish-motion/`。
- 本次联网核对：2026-11-12 原帖返回 HTTP 403，未重新观看原帖视频；以下借鉴关系来自本地制作记录。403 不证明帖子已删除。
- 借鉴：瓶内风雨、闪电与卧室光照关系、船随波面运动；鱼群修订逐半秒观察参考中的游动距离、双向穿行、纵深遮挡和侧身转向。具体修改为九条鱼的周期样条、弧长推进和身体波形；未导入原帖模型或运动数据。
- 原快照描述 14 秒参考；本项目后续扩为 20 秒小兔和鱼群版。原帖精确媒体 URL、时长/编码与逐镜头来源时间段未在快照完整提供，标记为未确认。

### 项目记录的技术、内容或素材来源

这些 URL 来自项目文件，尚不自动证明本片采用；除本节明确标注的访问外，本轮未逐一在线重读。

| 链接 | 记录文件 |
| --- | --- |
| [docs.blender.org](https://docs.blender.org/manual/en/4.4/animation/shape_keys/shape_keys_panel.html) | `videos/bottle-storm-recreation-20261106/docs/fish-revision.md` |
| [pmc.ncbi.nlm.nih.gov](https://pmc.ncbi.nlm.nih.gov/articles/PMC6748450/) | `videos/bottle-storm-recreation-20261106/docs/fish-revision.md` |
| [pubmed.ncbi.nlm.nih.gov](https://pubmed.ncbi.nlm.nih.gov/34847768/) | `videos/bottle-storm-recreation-20261106/docs/fish-revision.md` |

## 重现入口与修改位置

以下是 package.json 中的实际命令映射，供在对应视频目录使用；本轮未执行这些制作命令。系统依赖、前置模型/素材和环境准备见后附 README。

| npm 入口 | 实际命令 |
| --- | --- |
| `npm run dev` | `node scripts/run.mjs dev` |
| `npm run build` | `node scripts/run.mjs bundle` |
| `npm run scene` | `node scripts/run.mjs rebuild-scene` |
| `npm run preview:scene` | `node scripts/run.mjs preview-scene` |
| `npm run render:scene` | `node scripts/run.mjs render-scene` |
| `npm run audio` | `node scripts/run.mjs audio` |
| `npm run encode` | `node scripts/run.mjs encode` |
| `npm run render` | `node scripts/run.mjs render` |
| `npm run check` | `node scripts/run.mjs check` |
| `npm run upgrade` | `remotion upgrade` |
| `npm run lint` | `eslint src && tsc` |
| `npm run base` | `node scripts/run.mjs build-scene` |
| `npm run blocking` | `node scripts/run.mjs blocking` |

### 算法源码摘录

> [!info]- videos/bottle-storm-recreation-20261106/scripts/fish_motion.py，L1–77
> ```python
> """Deterministic smooth schooling paths. No Blender callbacks are needed at playback."""
> import math
> import json
> from pathlib import Path
> import numpy as np
> from scipy.interpolate import CubicSpline
> 
> ROOT=Path(__file__).resolve().parents[1]
> FPS=24
> FRAMES=480
> 
> def make_route(radius, half, height, phase):
>     # A rounded racetrack: long swimming passes and broad, continuous turns.
>     pts=[]
>     for a in np.linspace(-math.pi/2,math.pi/2,7):
>         pts.append([-.45+half+radius*math.cos(a),radius*math.sin(a)])
>     for x in np.linspace(half,-half,5)[1:-1]:pts.append([-.45+x,radius])
>     for a in np.linspace(math.pi/2,3*math.pi/2,7):
>         pts.append([-.45-half+radius*math.cos(a),radius*math.sin(a)])
>     for x in np.linspace(-half,half,5)[1:-1]:pts.append([-.45+x,-radius])
>     pts.append(pts[0])
>     u=np.r_[0,np.cumsum(np.linalg.norm(np.diff(np.array(pts),axis=0),axis=1))]
>     spline=CubicSpline(u,np.array(pts),bc_type='periodic')
>     dense=np.linspace(0,u[-1],24001)
>     xy=spline(dense)
>     ds=np.linalg.norm(np.diff(xy,axis=0),axis=1)
>     arc=np.r_[0,np.cumsum(ds)]
>     def at(distance):
>         q=np.interp(distance%arc[-1],arc,dense)
>         xy=spline(q)
>         # Small vertical exploration, with a separate phase for each fish.
>         a=q/u[-1]*math.tau
>         z=height+.023*math.sin(a+phase)+.009*math.sin(2*a-phase)
>         return np.r_[xy,z]
>     return at,float(arc[-1])
> 
> out={'fps':FPS,'frames':FRAMES,'model':'arc-length rounded schooling routes; two depths; opposite directions','fish':[]}
> for i in range(9):
>     upper=i>=6
>     group=i-6 if upper else i
>     direction=-1 if upper else 1
>     radius=(.64,.69,.60,.71,.66,.62,.57,.67,.72)[i]
>     height=(.805,.825,.795,.815,.805,.83,1.09,1.10,1.08)[i]
>     scale=(1.0,.83,.94,.86,1.04,.89,.86,.93,.79)[i]
>     at,length=make_route(radius,1.67-(i%3)*.055,height,i*1.19)
>     offset=(group/(3 if upper else 6)+(.055 if upper else .012))*length
>     speed0=.98 if upper else .83
>     def dist(t):
>         # Integrating speed makes acceleration smooth and prevents framewise jitter.
>         return offset+direction*length/11.3*(speed0*t+.18/.62*(math.cos(.3)-math.cos(.62*t+.3))+.025/.91*(math.cos(i*1.3)-math.cos(.91*t+i*1.3)))
>     samples=[];phase=i*2.399
>     for fr in range(FRAMES):
>         t=fr/FPS;p=at(dist(t));p0=at(dist(t-.01));p1=at(dist(t+.01))
>         velocity=(p1-p0)/.02;speed=float(np.linalg.norm(velocity));v=velocity/speed
>         acc=(p1-2*p+p0)/.0001
>         yaw=math.atan2(v[1],v[0]);pitch=-math.asin(v[2])
>         curvature=float((velocity[0]*acc[1]-velocity[1]*acc[0])/(np.linalg.norm(velocity[:2])**3))
>         bank=max(-.20,min(.20,curvature*speed*speed*.10))
>         # Faster swimming increases tailbeat frequency; phase is integrated, never reset.
>         frequency=1.35+.78*speed/(scale*.68)
>         phase+=math.tau*frequency/FPS
>         samples.append({'p':[round(float(x),7) for x in p],'yaw':yaw,'pitch':pitch,'bank':bank,'speed':speed,'phase':phase,'turn':max(-1.6,min(1.6,curvature))})
>     out['fish'].append({'id':i,'scale':scale,'radius':radius,'height':height,'direction':direction,'route_length':length,'samples':samples})
> # Body-volume proximity is checked independently from screen-space overlap.
> minimum=10.;closest=None
> for fr in range(FRAMES):
>     for i in range(9):
>         a=out['fish'][i];sa=a['samples'][fr];pa=np.array(sa['p'])
>         for j in range(i):
>             b=out['fish'][j];sb=b['samples'][fr];pb=np.array(sb['p'])
>             delta=pa-pb
>             # Ellipsoid broad phase; vertical radii exclude transparent fin membranes.
>             normalized=float(np.linalg.norm(delta/np.array([.50,.20,.105*(a['scale']+b['scale'])])))
>             if normalized<minimum:minimum=normalized;closest=[fr+1,j,i]
> out['spacing']={'min_normalized_center_distance':minimum,'closest':closest}
> (ROOT/'docs/fish-paths.json').write_text(json.dumps(out,separators=(',',':')))
> print(json.dumps({'fish':9,'frames':FRAMES,'spacing':out['spacing']}))
> ```

> [!info]- videos/bottle-storm-recreation-20261106/scripts/bunny_motion.py，L1–172
> ```python
> """Continuous character choreography and world-space contact targets.
> 
> All distances use the existing scene's units. Feet stay at world-space anchors
> through stance. The box and pencil each have one continuous transform trajectory.
> """
> import math
> from mathutils import Vector
> 
> FPS=24
> DURATION=20
> FRAMES=FPS*DURATION
> PEN_SCALE=.61
> PEN_GRIP=.19
> PEN_PALM_Z=.040
> PICK_YAW=-2.34
> PLACE_YAW=-2.40
> DRAW_YAW=-3.68
> 
> def sat(x):return min(1.0,max(0.0,x))
> def ease(a,b,t):
>     u=sat((t-a)/(b-a));return u*u*u*(10+u*(-15+6*u))
> def mix(a,b,u):return a*(1-u)+b*u
> def vec(x):return Vector(x)
> def R(yaw):return Vector((math.cos(yaw),math.sin(yaw),0))
> def F(yaw):return Vector((math.sin(yaw),-math.cos(yaw),0))
> def blend_pos(a,b,a_t,b_t,t):return vec(a).lerp(vec(b),ease(a_t,b_t,t))
> 
> def cruise(a,b,t,ramp=.40):
>     """Distance fraction with cosine velocity ramps and a constant-speed middle."""
>     d=b-a;u=max(0,min(d,t-a));r=min(ramp,d/3)
>     if u<r:return .5*(u-r/math.pi*math.sin(math.pi*u/r))/(d-r)
>     if u>d-r:
>         v=d-u;return 1-.5*(v-r/math.pi*math.sin(math.pi*v/r))/(d-r)
>     return (u-r/2)/(d-r)
> 
> def root_at(t):
>     if t<.60:return vec((5.27,-.25,0))
>     if t<2.10:return blend_pos((5.27,-.25,0),(5.13,-1.30,0),.60,2.10,t)
>     if t<4.05:return vec((5.13,-1.30,0))
>     if t<5.30:return vec((5.13,-1.30,0)).lerp(vec((4.42,-2.17,0)),cruise(4.05,5.30,t,.20))
>     if t<12.70:return vec((4.42,-2.17,0)).lerp(vec((-5.72,-2.17,0)),cruise(5.30,12.70,t))
>     if t<14.35:return vec((-5.72,-2.17,0))
>     return blend_pos((-5.72,-2.17,0),(-5.26,-2.25,0),14.35,15.25,t)
> 
> def yaw_at(t):
>     if t<1.60:return mix(0,PICK_YAW,ease(.55,1.60,t))
>     if t<3.90:return PICK_YAW
>     if t<5.30:return mix(PICK_YAW,-math.pi/2,ease(3.90,5.30,t))
>     if t<12.55:return -math.pi/2
>     if t<13.20:return mix(-math.pi/2,PLACE_YAW,ease(12.55,13.20,t))
>     if t<14.25:return PLACE_YAW
>     return mix(PLACE_YAW,DRAW_YAW,ease(14.25,15.35,t))
> 
> def moving(t):
>     return (ease(.55,.85,t)*(1-ease(2.03,2.30,t))+
>             ease(4.00,4.40,t)*(1-ease(12.80,13.25,t))+
>             ease(14.30,14.55,t)*(1-ease(15.20,15.50,t)))
> 
> def posture(t):
>     pick=ease(2.12,2.92,t)*(1-ease(3.20,4.08,t))
>     place=ease(13.08,13.80,t)*(1-ease(14.15,14.55,t))
>     draw=ease(15.20,15.90,t)
>     walk=moving(t)
>     phase=(t-4.03)/.62
>     z=.43-.23*pick-.23*place-.27*draw-.050*walk
>     bob=.010*walk*math.cos(phase*math.tau*2)
>     sway=.018*walk*math.sin(phase*math.tau)
>     drawing_reach=.075*ease(17.75,18.05,t)*(1-ease(19.65,20.0,t))
>     lean=.20*pick+.21*place+.16*draw+.05*walk+drawing_reach
>     return {'pelvis_z':z+bob,'lean':lean,'sway':sway,'walk':walk,'phase':phase,'draw':draw}
> 
> def foot_default(t,side,lead=0):
>     return root_at(t+lead)+R(yaw_at(t))*side*.14+F(yaw_at(t))*.055+vec((0,0,.105))
> 
> # Explicit alternating swing intervals; world-space anchors between intervals.
> SWINGS={-1:[],1:[]}
> for k in range(5):
>     start=.65+k*.34;side=1 if k%2==0 else -1
>     SWINGS[side].append((start,start+.25,2.30))
> k=0
> while 4.03+k*.31<13.02:
>     start=4.03+k*.31;side=1 if k%2==0 else -1
>     SWINGS[side].append((start,start+.24,13.30));k+=1
> for k in range(4):
>     start=14.35+k*.29;side=1 if k%2==0 else -1
>     SWINGS[side].append((start,start+.23,15.50))
> 
> FOOT_EVENTS={}
> for side in [-1,1]:
>     events=[];anchor=foot_default(0,side);anchor_yaw=0
>     for idx,(start,end,group_end) in enumerate(SWINGS[side]):
>         later=[x for x in SWINGS[side][idx+1:] if x[0]<group_end]
>         landing_t=group_end if not later else end
>         landing=foot_default(landing_t,side,.105 if later else 0)
>         events.append({'start':start,'end':end,'from':anchor.copy(),'to':landing.copy(),'yaw0':anchor_yaw,'yaw1':yaw_at(landing_t)})
>         anchor=landing;anchor_yaw=yaw_at(landing_t)
>     FOOT_EVENTS[side]=events
> 
> def foot_at(t,side):
>     pos=foot_default(0,side);yaw=0
>     for e in FOOT_EVENTS[side]:
>         if t<e['start']:break
>         if t<=e['end']:
>             u=sat((t-e['start'])/(e['end']-e['start']))
>             p=e['from'].lerp(e['to'],ease(0,1,u))
>             p.z+=.135*math.sin(math.pi*u)**1.4+.008*abs(math.sin(math.tau*u))
>             yy=mix(e['yaw0'],e['yaw1'],ease(0,1,u))
>             return p,yy,.18*math.sin(math.tau*u),False
>         pos=e['to'];yaw=e['yaw1']
>     return pos.copy(),yaw,0,True
> 
> def box_at(t):
>     initial=vec((4.75,-.92,.16))
>     target=vec((-6.07,-1.73,.16))
>     carry=root_at(t)+F(yaw_at(t))*.39+vec((0,0,.69+.012*moving(t)*math.cos((t-4.3)/.70*math.tau*2)))
>     if t<3.0:return initial,PICK_YAW
>     if t<3.85:return initial.lerp(carry,ease(3.0,3.85,t)),mix(PICK_YAW,yaw_at(t),ease(3,3.85,t))
>     if t<13.20:return carry,yaw_at(t)
>     if t<14.15:return carry.lerp(target,ease(13.20,14.15,t)),mix(yaw_at(t),PLACE_YAW,ease(13.20,14.15,t))
>     return target,PLACE_YAW
> 
> def resample(points,count):
>     lengths=[0]
>     for a,b in zip(points,points[1:]):lengths.append(lengths[-1]+(b-a).length)
>     result=[];j=0
>     for i in range(count):
>         d=lengths[-1]*i/(count-1)
>         while j<len(lengths)-2 and lengths[j+1]<d:j+=1
>         u=(d-lengths[j])/max(1e-9,lengths[j+1]-lengths[j]);result.append(points[j].lerp(points[j+1],u))
>     return result
> 
> STEM=[vec((-5.08,-1.98,.043)),vec((-5.08,-1.84,.043))]
> FLOWER=resample([vec((-5.08+(.09+.025*math.sin(5*(a+math.pi/2)))*math.cos(a),-1.75+(.09+.025*math.sin(5*(a+math.pi/2)))*math.sin(a),.043)) for a in [-math.pi/2+math.tau*j/800 for j in range(801)]],241)
> 
> def on_path(points,u):
>     x=sat(u)*(len(points)-1);i=min(len(points)-2,int(x));return points[i].lerp(points[i+1],x-i)
> 
> def pen_at(t):
>     initial=vec((-4.97,-1.92,.052));axis0=vec((-1,0,0));axis1=vec((-.18,.24,.40)).normalized()
>     if t<15.80:return initial,axis0
>     if t<16.85:
>         u=ease(15.80,16.85,t);tip=initial.lerp(STEM[0],u);tip.y+=.22*math.sin(math.pi*u);tip.z+=.12*math.sin(math.pi*u)
>         axis=axis0.lerp(axis1,u).normalized();return tip,axis
>     if t<17.75:return on_path(STEM,ease(16.85,17.75,t)),axis1
>     if t<19.75:return on_path(FLOWER,ease(17.75,19.75,t)),axis1
>     tip=FLOWER[-1].copy();tip.z+=.035*ease(19.75,20,t);return tip,axis1
> 
> def hand_targets(t):
>     root=root_at(t);yaw=yaw_at(t);post=posture(t)
>     free_swing=post['walk']*(1-ease(2.0,2.3,t) if t<3 else ease(14.45,14.70,t)*(1-ease(15.1,15.35,t)))
>     rest={s:root+R(yaw)*s*.29+F(yaw)*(.03+s*.075*free_swing*math.sin(post['phase']*math.tau))+vec((0,0,post['pelvis_z']+.12)) for s in [-1,1]}
>     box,byaw=box_at(t)
>     acquire=ease(2.30,3.0,t)*(1-ease(14.15,14.45,t))
>     hands={s:rest[s].lerp(box+R(byaw)*s*.225+vec((0,0,.015)),acquire) for s in [-1,1]}
>     tip,axis=pen_at(t);grip=tip+axis*PEN_GRIP+vec((0,0,PEN_PALM_Z))
>     hands[1]=hands[1].lerp(grip,ease(15.25,15.80,t))
>     hands[-1]=hands[-1].lerp(vec((-5.16,-2.16,.105)),ease(15.30,16.40,t))
>     return hands
> 
> def blink(t):
>     amount=0
>     for center in [.35,2.85,4.03,7.35,10.6,13.1,15.8,18.7]:
>         u=abs(t-center)/.095
>         if u<1:amount=max(amount,(1-u*u)**2)
>     return max(.055,1-.96*amount)
> 
> def two_bone(shoulder,wrist,pole,a,b):
>     delta=wrist-shoulder;actual=delta.length;d=max(.001,min(a+b-.001,actual));n=delta.normalized()
>     toward=pole-shoulder;perp=toward-n*toward.dot(n)
>     if perp.length<1e-6:perp=n.cross(Vector((0,0,1)))
>     perp.normalize();along=(a*a-b*b+d*d)/(2*d);height=math.sqrt(max(.000001,a*a-along*along))
>     return shoulder+n*along+perp*height,max(0,actual-(a+b))
> ```

## 完整关键提示词与生成记录

以下保留项目自有提示词、候选参数与版本。存在失败候选和历史参数；最终采用以 selected-takes、edit-decisions 与制作复盘为准，不能将所有候选都写成已采用。

未发现实际生成提示词或推理执行记录；不从脚手架、模型安装清单推测生成过程。

## 已有产物与检查依据

文件存在、技术检查、画面检查与用户验收分别看待。下表是本次文件盘点，旧版与当前版可能共存。MP4/HTML 按内容计算 SHA-256，ZIP 仅记录文件大小和修改时间供增量判断。

| 本地产物 | 字节数 | SHA-256 |
| --- | ---: | --- |
| `videos/bottle-storm-recreation-20261106/outputs/bottle-storm-14s.mp4` | 7045596 | `25d0e1b1965b329353fc52a1ab7672ca718bf5afdfc0bbb2e13cf539b4036918` |
| `videos/bottle-storm-recreation-20261106/outputs/bottle-storm-bunny-20s.mp4` | 9523624 | `596f047c1a50c31c22b610923b796dd3f2b544ed09217af4c0c1f7be6e74ef1b` |
| `videos/bottle-storm-recreation-20261106/outputs/bottle-storm-bunny-fish-20s.mp4` | 9895628 | `c902580cf75e88e04193e2c9c043c787261bf5c63434b436f12fd57437d4fe6c` |
| `videos/bottle-storm-recreation-20261106/outputs/bottle-storm-bunny-fish-project.zip` | 117047282 | `ZIP：仅元数据` |
| `videos/bottle-storm-recreation-20261106/outputs/bottle-storm-bunny-project.zip` | 58810813 | `ZIP：仅元数据` |
| `videos/bottle-storm-recreation-20261106/outputs/bottle-storm-project.zip` | 31890857 | `ZIP：仅元数据` |

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
>       "nb_frames": "480"
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
>     "size": "9895628"
>   },
>   "file": "outputs/bottle-storm-bunny-fish-20s.mp4",
>   "scope": "本次只读取媒体头；未全片解码、播放或重新进行视觉/听觉验收。"
> }
> ```


已有检查报告路径（未在本轮重新执行）：

- `videos/bottle-storm-recreation-20261106/outputs/checks/archive-check.json`
- `videos/bottle-storm-recreation-20261106/outputs/checks/audio.json`
- `videos/bottle-storm-recreation-20261106/outputs/checks/final-check.json`
- `videos/bottle-storm-recreation-20261106/outputs/checks/frame-qa.json`
- `videos/bottle-storm-recreation-20261106/outputs/checks/package-check.json`
- `videos/bottle-storm-recreation-20261106/outputs/checks/render-blocking.json`
- `videos/bottle-storm-recreation-20261106/outputs/checks/render-final.json`
- `videos/bottle-storm-recreation-20261106/outputs/checks/render-fish-preview.json`
- `videos/bottle-storm-recreation-20261106/outputs/checks/render-preview.json`
- `videos/bottle-storm-recreation-20261106/outputs/checks/render-test.json`
- `videos/bottle-storm-recreation-20261106/outputs/checks/scene-audit.json`
- `videos/bottle-storm-recreation-20261106/outputs/checks/bunny/archive-check.json`
- `videos/bottle-storm-recreation-20261106/outputs/checks/bunny/contact-audit.json`
- `videos/bottle-storm-recreation-20261106/outputs/checks/bunny/flash-diagnosis.json`
- `videos/bottle-storm-recreation-20261106/outputs/checks/bunny/package-check.json`
- `videos/bottle-storm-recreation-20261106/outputs/checks/documentation/audio-rebuild-check.json`
- `videos/bottle-storm-recreation-20261106/outputs/checks/documentation/document-check.json`
- `videos/bottle-storm-recreation-20261106/outputs/checks/documentation/rebuild-audio/docs/timeline.json`
- `videos/bottle-storm-recreation-20261106/outputs/checks/documentation/rebuild-audio/outputs/checks/audio.json`
- `videos/bottle-storm-recreation-20261106/outputs/checks/fish/archive-check.json`
- `videos/bottle-storm-recreation-20261106/outputs/checks/fish/build.json`
- `videos/bottle-storm-recreation-20261106/outputs/checks/fish/cache-cleanup.json`
- `videos/bottle-storm-recreation-20261106/outputs/checks/fish/concurrency-decision.json`
- `videos/bottle-storm-recreation-20261106/outputs/checks/fish/motion-audit.json`
- `videos/bottle-storm-recreation-20261106/outputs/checks/fish/reference-timestamps.json`
- `videos/bottle-storm-recreation-20261106/outputs/checks/fish/subframe-check.json`

## 制作方法与迭代记录选编

下面是项目自有文档的归档摘录，保留原阶段与原检查表述，个人路径已泛化、相对链接已注明源项目位置。它们属于来源陈述，不代表本次重新完成其中的生成或验收。

> [!info]- 来源文档：videos/bottle-storm-recreation-20261106/README.md
> # 瓶中风暴 · 小兔与鱼群（游动优化版）
> 
> 完整制作方法见 制作手册（项目文件：`videos/bottle-storm-recreation-20261106/docs/production-method.md`），涵盖模型与场景关系、动作约束、鱼群、水面、渲染、声音、检查与重建。另有 交互式制作流程图（项目文件：`videos/bottle-storm-recreation-20261106/docs/.archify/architecture-bottle-storm-20261109/production-flow.html`） 和 实际场景清单（项目文件：`videos/bottle-storm-recreation-20261106/docs/scene-inventory.json`）。
> 
> 音频核验：当前正式 WAV 是 14 秒初版，现有成片第 15 至 20 秒为静音。完整 20 秒拟音分支已补齐缺失导入并独立重建验证；详情及应用方法见制作手册第 16、20 节。
> 
> 20 秒 / 24fps / 1280×720 / 480 帧。Blender Cycles 逐帧渲染；全部模型、关节运动、玻璃折射、风雨与小型拟音均在本项目内制作。
> 
> 鱼群 v3 使用有厚度的流线鱼身、珠光鳞片、双侧眼睛、鳃线、分叉尾鳍及细鳍条。每条鱼有 20 个循环形态，摆动从前身传到尾端；尾鳍、胸鳍和背鳍柔软跟随。九条鱼分两层反向游动，沿按弧长推进的圆滑路线连续游过瓶内。朝向跟随速度切线，转弯带倾斜和身体弯曲，摆尾相位独立，频率随游速变化。全部烘焙为原生关键帧，不依赖播放回调。
> 
> 在原有瓶中风暴场景中，小兔看向画纸、走近积木、蹲下双手抱起、绕到瓶前搬到左侧、放稳、拿笔，再画出新的花朵。连续镜头跟随搬运，并在画画时推近。角色沿用原来暖白玩偶与蓝色领结的造型。
> 
> 上一版小兔视频与工程包保留在 outputs/bottle-storm-bunny-20s.mp4 和 outputs/bottle-storm-bunny-project.zip；鱼群修改前的原生场景快照在 outputs/versions/bunny-v2/。
> 
> 初版 14 秒视频与工程包仍保留在 outputs/bottle-storm-14s.mp4 和 outputs/bottle-storm-project.zip。旧视频和旧工程包不随新工程包重复打包。原始 Blender 场景快照在 outputs/versions/v1/；角色版本从本目录内的快照加载，不引用其他视频。
> 
> ## 独立安装
> 
> 系统工具：Node.js 20.19+ / 22.13+ / 24+、Blender 5.2、FFmpeg、Python 3.13。制作机使用 Blender 5.2.2、M3 Pro Metal GPU 与系统 Chrome。
> 
> 在本视频目录执行：
> 
> ```bash
> mkdir -p .cache/tmp .cache/npm .cache/python
> export TMPDIR="$PWD/.cache/tmp"
> export TMP="$PWD/.cache/tmp"
> export TEMP="$PWD/.cache/tmp"
> export PIP_CACHE_DIR="$PWD/.cache/python"
> npm ci
> python3 -m venv .venv
> .venv/bin/python -m pip install -r requirements.txt
> ```
> 
> ## 预览、检查、导出
> 
> ```bash
> npm run dev
> npm run check
> npm run render
> ```
> 
> 预览选择 BottleStorm，默认 http://localhost:3076/BottleStorm 。成片为 outputs/bottle-storm-bunny-fish-20s.mp4。正式画面 public/bunny-fish-picture.mp4；音轨 public/storm-soundscape.wav。压缩包含已生成素材，可独立安装、预览与导出，不必重渲三维。
> 
> Remotion 优先使用系统 Chrome。其他平台在 remotion.config.ts 指定本机 Chrome；下载浏览器时须下载到本视频自己的目录。npm run build 输出 outputs/remotion-bundle/。
> 
> ## 修改动作并重建
> 
> ```bash
> npm run scene
> node scripts/run.mjs audit-bunny
> node scripts/run.mjs audit-fish
> node scripts/run.mjs audit-scene
> npm run preview:scene -- 73 193 313 397 445
> npm run render:scene
> npm run encode
> npm run audio
> npm run render
> ```
> 
> - scripts/fish_motion.py：九条鱼的巡游路线、空间层次、速度与摆尾相位，输出 docs/fish-paths.json。改变鱼速、尺寸、方向和轨迹后，运行 `node scripts/run.mjs fish-paths` 再运行 `node scripts/run.mjs upgrade-fish`，即可保留现有小兔动作而重新生成鱼群。
> - scripts/upgrade_fish.py：从本目录 outputs/versions/bunny-v2/bottle-storm.blend 加载场景，移除旧鱼，制作鱼身、鳍膜和鳍条，烘焙连续游动与身体变形。
> - `npm run scene`：依次从 v1 快照重建小兔、更新本目录兔子场景快照、再生成新版鱼群。
> - scripts/bunny_motion.py：行动时间、路线、脚步接触点、重心、抓握、积木与笔轨迹。站立期足底锁在世界坐标，抬脚期使用连续弧线；手臂与双腿由双段 IK 计算后烘焙为原生关键帧。
> - scripts/animate_bunny.py：加载本目录内原始场景，替换静态兔子，生成关节部件、眨眼与耳朵跟随，烘焙全部动作，并保存 public/bottle-storm.blend。积木和笔各使用单一连续变换轨迹，手跟随物体接触点；新画线随笔尖进度出现。
> - 相机：先保留全景，再跟随左移，最后平缓进入画画近景；景深随推近渐变。
> - scripts/render_scene.py：预览低采样；正式版本 32 samples、固定噪声种子、降噪、适量运动模糊，输出 outputs/frames-fish/。已有有效帧可续渲；渲染报告带场景 SHA-256 指纹。修改动作后应先将旧帧移入本项目内的版本目录，场景与旧帧不一致时脚本会拒绝续渲，防止新旧混用。
> - scripts/synthesize_audio.py：NumPy/SciPy 合成 48kHz 立体声风雨、海浪、雷声、足音、积木落桌和画笔摩擦。没有旁白、字幕和配乐。需要旁白时，将本视频自己的录音放进 public/ 并在 src/Composition.tsx 添加 Audio 层。
> - src/Composition.tsx：最终视频与声音的时间轴、音量；画面与音轨为独立 JSX 节点。
> 
> 更改时长或 fps 时，需同步 bunny_motion.py、Remotion Composition、run.mjs 编码帧数与 docs/timeline.json，并重新生成声音和三维帧。
> 
> ## 闪光说明与检查
> 
> 旧版明显的整屋亮暗变化由瓶内闪电及同步辅助光引起，亮度与闪电强度相关系数约 0.99，并非窗外闪电。角色版本将全屋闪光降到原来的 30%，瓶内补光降到 72%，保留雷雨气氛。采样由 16 提高到 32，以减少细碎的渲染噪声闪烁。
> 
> outputs/checks/bunny/contact-audit.json 记录手脚可达性、站立脚滑动、抓握偏差、笔杆与头/身体的间距、角色及手持积木的镜头范围。积木放下后成为布景道具，镜头转向画纸时允许短暂裁切；最终画画近景能看见积木与画纸。
> 
> outputs/checks/fish/before-after-8s.mp4 提供前 8 秒鱼群运动的同步放大对比，便于观察旧版原地漂移与新版游动的区别。
> 
> outputs/checks/fish/motion-audit.json 检查全部 480 帧的鱼身及鱼鳍与玻璃、砂床、水面的距离、鱼身之间的相交、形态混合权重和朝向连续性。屏幕上前后鱼的遮挡属于不同深度的正常投影。
> 
> outputs/checks/scene-audit.json 检查原生波浪与船体耦合及外部素材依赖；frame-qa.json 检查 480 帧完整性、空帧与重复帧；final-check.json 检查代码、路径与最终视频元数据。
> 
> 动作预演 public/previews/bunny-blocking.mp4 使用简化的 Workbench，隐藏玻璃和水面以检查四肢与地面接触。正式成片使用完整 Cycles 材质与光照。
> 
> ## 隔离与打包
> 
> scripts/run.mjs 把受控临时目录、缓存、Blender 配置和输出指向本视频目录。没有跨视频运行依赖或素材链接。系统 Blender、FFmpeg、Python、Chrome 属于允许的系统工具。
> 
> 制作机发生过 Metal 二进制编译归档崩溃；run.mjs 设置 CYCLES_METAL_DISABLE_BINARY_ARCHIVES=1 绕过该归档路径，仍使用 Metal GPU 的 Cycles。相关崩溃报告已移入 outputs/checks/fish/ 并清理系统日志副本。
> 
> macOS Metal 驱动的系统级编译缓存、既有浏览器自身缓存不接受本项目路径控制，未声称它们已隔离。初版制作时的 Metal 编译崩溃报告已移入本项目输出目录并清理原副本。
> 
> ```bash
> .venv/bin/python scripts/package_project.py
> ```
> 
> 工程包 outputs/bottle-storm-bunny-fish-project.zip 包含源码、锁文件、可编辑 Blender 工程、生成素材、制作说明、检查报告、成片及重建所需的原场景快照；不包含可重建的依赖、缓存、逐帧 PNG 和旧动作预演帧。

> [!info]- 来源文档：videos/bottle-storm-recreation-20261106/docs/production-method.md
> # 瓶中风暴视频制作方法
> 
> 本片采用程序化三维动画流程：Python 创建模型、材质和场景，计算小兔、鱼群、水面及相机的运动，将结果保存成 Blender 原生关键帧；Cycles 计算光照与画面，FFmpeg 编码逐帧图片，Remotion 组合画面与声音并导出 MP4。
> 
> 制作记录日期为 2026 年 11 月 9 日。以下以小兔搬积木画画、鱼群 v3 优化后的工程为准，既解释概念，也记录具体参数、文件依赖、操作步骤和检查范围。
> 
> 当前画面为 20 秒、24fps、1280×720，共 480 帧。当前成片配用的声音源仍是 14 秒初版 WAV，解码确认第 15 至 20 秒为静音。20 秒完整拟音分支已补齐缺失的 `math` 导入，并在独立检查目录生成验证音轨；本次记录保留现有成片和正式音频源。完整声音的生成与应用步骤见第 16 和第 20 节。
> 
> 交互式总览：从模型到成片的制作流程（项目文件：`videos/bottle-storm-recreation-20261106/docs/.archify/architecture-bottle-storm-20261109/production-flow.html`）。
> 
> 实际场景清单：scene-inventory.json（项目文件：`videos/bottle-storm-recreation-20261106/docs/scene-inventory.json`）。清单从现有 `.blend` 读取，记录物体类型、父级、坐标、材质和形态关键帧。
> 
> ## 阅读路线
> 
> - 理解基本概念与软件分工：第 1 至 3 节。
> - 理解模型和场景怎样关联：第 4 至 7 节。
> - 理解动作怎样制作：第 8 至 14 节。
> - 理解画质、声音和视频导出：第 15 至 18 节。
> - 检查、重建和修改工程：第 19 至 24 节。
> 
> ## 1 成片规格与主要产物
> 
> | 项目 | 本工程的实际值 |
> |---|---|
> | 动画时长 | 20 秒 |
> | 帧率 | 24fps |
> | 分辨率 | 1280×720，即 720p |
> | 渲染帧 | Blender 第 1 至 480 帧 |
> | 合成时间轴 | Remotion 第 0 至 479 帧 |
> | 三维工程 | `public/bottle-storm.blend` |
> | 渲染图像 | `outputs/frames-fish/frame_0001.png` 至 `frame_0480.png` |
> | 无声画面 | `public/bunny-fish-picture.mp4` |
> | 当前正式音频源 | `public/storm-soundscape.wav`，14 秒、48kHz、双声道、16 位 PCM |
> | 完整音频重建验证 | `outputs/checks/documentation/rebuild-audio/public/storm-soundscape.wav`，20 秒 |
> | 当前最终视频 | `outputs/bottle-storm-bunny-fish-20s.mp4` |
> | 当前视频编码 | H.264 画面、AAC 声音 |
> | 视频文件容器时长 | 约 20.011 秒；AAC 封装存在少量时长差异 |
> | 制作设备 | Apple M3 Pro，18 核 GPU |
> | 原生场景对象数量 | 534 个 Object，详见第 5 节 |
> 
> 分辨率、fps、渲染采样数是三个独立参数。分辨率决定每张图有多少像素；fps 决定每秒有多少张画面；Cycles samples 决定单张画面进行多少光线采样。
> 
> ## 2 基本概念
> 
> | 概念 | 含义 | 本片中的例子 |
> |---|---|---|
> | 三维模型 | 用几何数据表达物体形状 | 鱼身、瓶壁、兔头 |
> | Mesh | 顶点、边、面的集合 | 玻璃瓶壳、鱼身表面 |
> | Curve | 空间曲线，可添加粗细 | 绳索、水草、闪电、画线 |
> | Object | 几何或其他数据在场景里的实例，带位置、旋转、缩放 | 某一条鱼、某一块积木 |
> | Material | 决定表面怎样响应光线的数据 | 玻璃、木头、陶瓷 |
> | Shader nodes | 组合纹理、颜色、反射等效果的节点网络 | 木纹噪声、鱼鳞凹凸 |
> | Scene | 完整拍摄空间，包含物体、相机、环境和渲染设置 | 桌面上的整个瓶中风暴画面 |
> | Collection | 组织物体的集合 | `School • articulated fish` 鱼群集合 |
> | Empty | 没有可见几何的控制物体 | 鱼的运动根节点、小兔头部控制点 |
> | Parent | 把一个物体的变换传给子物体的层级关系 | 兔头控制点带动眼睛、耳朵 |
> | Keyframe | 在指定时间记录一个属性值 | 第 100 帧手腕的位置 |
> | Shape Key | 为同一网格保存不同形状，通过权重混合 | 鱼的连续摆尾形态 |
> | IK | 从手脚目标位置反推肘、膝等关节位置 | 让手贴住积木 |
> | Render | 根据当前三维状态计算图片 | Cycles 渲染一张 PNG |
> | Encode | 将画面和声音压缩保存成媒体文件 | FFmpeg 输出 H.264 MP4 |
> 
> 模型和物体要分开理解。模型描述形状；Object 决定这个形状在场景哪里、朝哪个方向、有多大。一个 Object 还可以引用材质、动画、父级及修改器。
> 
> 场景是完整世界，背景只是其中一部分。玻璃瓶、鱼、小兔、画纸、灯和相机都处于同一个场景中。
> 
> ## 3 技术栈及分工
> 
> | 技术或库 | 版本 | 实际工作 |
> |---|---|---|
> | Python | 制作环境使用 3.13 | 编写规则、计算轨迹、生成声音、编排工具和检查结果 |
> | Blender | 5.2.2 LTS | 存储、编辑和播放完整三维工程 |
> | `bpy` | Blender 内置 | 创建网格、物体、材质、灯光、相机和关键帧 |
> | `mathutils` | Blender 内置 | 向量、矩阵、欧拉角、四元数及坐标变换 |
> | Cycles | Blender 内置 | 路径追踪渲染、反射、折射、阴影和镜头效果 |
> | Apple Metal | 系统 GPU 接口 | 在本机 GPU 上执行 Cycles 渲染 |
> | NumPy | 2.5.4 | 批量坐标与距离计算、采样、声音波形生成 |
> | SciPy | 1.18.1 | 三次样条插值、信号滤波、WAV 输出 |
> | Pillow | 13.0.0 | 图片读取、联系表、检查图和对比图制作 |
> | Node.js | 制作环境使用 24 | 运行 Remotion 与项目命令编排脚本 |
> | React 和 React DOM | 19.3.1 | 表达最终视频的 JSX 组件树 |
> | TypeScript | 5.9.3 | 对合成代码进行类型检查 |
> | Remotion | 4.0.541 | 时间轴、浏览器预览、画面和声音合成 |
> | `@remotion/media` | 4.0.541 | 读取画面视频及音频素材 |
> | `@remotion/cli` | 4.0.541 | 启动 Studio、合成、导出 |
> | FFmpeg | 系统工具 | 编码图像序列、制作样片、解码检查 |
> | ffprobe | 系统工具 | 读取媒体编码、时长、尺寸、采样率等信息 |
> | ESLint 和 Prettier | 10.12.0 和 3.8.1 | JavaScript/TypeScript 代码规范与格式工具 |
> 
> 版本锁定分别保存在 package.json（项目文件：`videos/bottle-storm-recreation-20261106/package.json`）、package-lock.json（项目文件：`videos/bottle-storm-recreation-20261106/package-lock.json`） 和 requirements.txt（项目文件：`videos/bottle-storm-recreation-20261106/requirements.txt`）。`bpy` 与 `mathutils` 随 Blender 运行，普通 Python 虚拟环境主要承担 NumPy、SciPy、Pillow 相关工作。
> 
> NumPy 擅长处理很多数字；SciPy 提供插值、滤波等成熟算法。Blender 将数字变成三维状态，Cycles 将三维状态变成画面。Remotion 在本片中读取已经渲染好的画面视频，并组合独立音轨。
> 
> ## 4 制作流程与数据流
> 
> ```text
> 制作要求与时间安排
>         │
>         ├─ Python / bpy 创建几何、材质、灯光和相机
>         ├─ 小兔动作函数计算手脚和道具接触
>         ├─ NumPy / SciPy 计算鱼的路径
>         └─ 波浪函数同时计算水面和船的姿态
>                          │
>               烘焙成 Blender 原生关键帧
>                          │
>               public/bottle-storm.blend
>                          │
>                  Cycles 逐帧渲染
>                          │
>                     480 张 PNG
>                          │
>                    FFmpeg 编码
>                          │
>                  无声画面 MP4 ─────────┐
>                                       ├─ Remotion 合成 → 最终 MP4
> 时间表、脚步接触时刻 → 声音合成 → WAV ─┘
> ```
> 
> 这条流程包含三个不同层次：
> 
> 1. 动作层计算“现在是什么姿态”。
> 2. 渲染层计算“这个姿态在光线和相机下长什么样”。
> 3. 合成层安排“画面和声音如何在同一时间轴出现”。
> 
> 先将动作烘焙到 `.blend`，就能打开工程直接检查关键帧。重新渲染时不必再次运行建模和动作生成脚本。
> 
> ## 5 模型与场景的实际组织
> 
> ### 5.1 场景清单
> 
> 当前原生场景包含 534 个 Object：247 个 Mesh、252 个 Curve、12 个 Light、22 个 Empty 和 1 个 Camera。Mesh 物体共引用 33 种材质；曲线物体的材质同样记录在场景清单中。这里统计的是场景 Object 和基础数据，不等于屏幕上能数出的道具数量；一只兔子或一艘船会拆成多个部件。
> 
> | 对象 | 数据规模或组成 | 主要关系 |
> |---|---|---|
> | `Bottle • solid optical shell` | 30,912 个基础顶点、30,912 个面 | 独立世界坐标物体，绑定玻璃材质 |
> | `Ocean • closed volume and analytic waves` | 8,214 个顶点、8,212 个面 | 使用 6 个波浪形态关键帧和两种水材质 |
> | `Schooner • four-point baked buoyancy` | Empty，54 个直接子物体 | 统一带动船壳、甲板、帆和绳索 |
> | `Rabbit rig • chest` | Empty | 带动身体、肚子、尾巴和领结 |
> | `Rabbit rig • head` | Empty | 带动头、眼睛、嘴部与耳朵控制点 |
> | `Rabbit wrist -1 / 1` | 两个 Empty | 分别带动左右手掌与拇指 |
> | `Rabbit ankle -1 / 1` | 两个 Empty | 分别带动脚掌，位置由步态函数决定 |
> | `Rabbit carried pink block` | 积木 Mesh | 使用独立、连续的道具轨迹 |
> | `Rabbit pencil • grip constrained` | Empty 和笔的子部件 | 控制整支笔的位置与朝向 |
> | 每条 `Fish v3` | 6,003 个顶点、5,544 个面 | 网格挂在该鱼的运动 Empty 下 |
> | `Drawing paper` | 基础立方体网格和倒角 | 静态世界坐标物体 |
> 
> 完整清单见 scene-inventory.json（项目文件：`videos/bottle-storm-recreation-20261106/docs/scene-inventory.json`）。
> 
> ### 5.2 Collection 与父子层级
> 
> Collection 负责组织。例如鱼群的根节点和网格都放进 `School • articulated fish`。将物体放在同一 Collection 中便于查找、隐藏和管理，具体运动仍由各物体的变换、父级或动画决定。
> 
> 父子层级用于传递变换。本片的一些实际关系如下：
> 
> ```text
> Rabbit rig • head
> ├─ Rabbit head
> ├─ 左右眼睛及眼睛高光
> ├─ 嘴部与鼻子
> └─ Rabbit ear base -1 / 1
>    ├─ 耳朵外形
>    └─ 耳朵内侧
> 
> Rabbit rig • chest
> ├─ Rabbit soft body
> ├─ Rabbit tummy
> ├─ Rabbit cotton tail
> └─ 领结部件
> 
> Fish v3 00 • tangent swim rig
> └─ Fish v3 00 • flexible body and fins
>    ├─ 网格中的鱼身与鱼鳍
>    ├─ 材质槽与顶点颜色
>    └─ Basis + 20 个摆尾形态 + 1 个转弯形态
> 
> Schooner • four-point baked buoyancy
> └─ 船体、甲板、桅杆、帆面和绳索等 54 个直接子物体
> ```
> 
> 小兔的 root、pelvis、chest、head、wrist、ankle 控制点本身多数是独立的世界坐标 Empty。脚本根据共同的身体轨迹分别计算它们。它们没有组成一棵“只移动 root 就自动带动全部部件”的完整父子树。可见头部、身体和手脚等局部部件，则挂在相应控制点下面。
> 
> ### 5.3 六种关联方式
> 
> | 关联方式 | 本片例子 | 修改时的含义 |
> |---|---|---|
> | 组织关系 | 鱼群放在同一 Collection | 便于选择和管理 |
> | 父子变换 | 鱼网格挂在运动 Empty 下 | 移动 Empty，整条鱼一起移动 |
> | 材质引用 | 多个兔子部件使用同一陶瓷材质 | 修改共享材质，会影响所有引用者 |
> | 共同坐标 | 鱼、水、瓶壁、砂床位于同一空间 | 依靠位置与尺寸对齐 |
> | 运动数据依赖 | 双手接触点由积木位置计算 | 积木轨迹变化后，需要重新计算手臂 |
> | 渲染关系 | 灯照亮鱼，鱼经过水和瓶壁折射后被相机看到 | 外观由几何、光线和材质共同决定 |
> 
> 鱼位于瓶子内部，来自正确的空间位置和边界约束；瓶子不是鱼群的运动父级。船随水面运动，来自共享波浪高度函数。手抱住积木，来自接触目标和 IK 计算。这些是不同的关联方式，修改时需要分别处理。
> 
> ### 5.4 房间与瓶内世界的关系
> 
> 房间、桌面、瓶壁、水、船和鱼都直接放在同一个 Blender Scene 中。相机发出的观察路径经过瓶壁和水等表面，Cycles 在完整场景中计算遮挡、反射、折射和阴影，因此鱼与瓶壁、船与水面能形成统一的空间外观。
> 
> `v1`、`bunny-v2` 和鱼群 v3 表示同一作品的不同制作阶段。重建时依次读取阶段快照，正式渲染则读取包含完整物体与关键帧的 `public/bottle-storm.blend`。
> 
> 动作计算的依赖关系可以表示为：
> 
> ```text
> 角色根路线与朝向 → 胸部、头部及肩胯位置
> 积木路线 → 双手接触目标 → 肘部求解 → 手臂部件
> 脚步事件 → 世界坐标脚点 → 膝盖求解 → 腿部部件
> 花茎与花朵路径 → 笔尖路线 → 手腕目标
> 花茎与花朵路径 → 画线显现进度
> 鱼的巡游路线 → 运动根节点 → 整条鱼的位移和朝向
> 摆尾相位与转弯曲率 → 网格形态权重 → 鱼身局部变形
> 波浪高度函数 → 水面形态权重
> 波浪高度函数 → 四点水位 → 船根节点 → 船的所有子部件
> ```
> 
> 这里既有父级带动子级的变换，也有一组计算结果驱动另一组目标的逻辑依赖。把这两类关系分开，才能知道改动一个参数后哪些部分需要重新生成。
> 
> ## 6 坐标与变换
> 
> 场景使用 X、Y、Z 三个坐标轴，Z 向上。初始全景中，X 大致对应左右，Y 大致对应前后。世界坐标轴不会随镜头移动而旋转；相机转到侧面后，屏幕上的左右与世界 X 方向就不再完全相同。
> 
> 桌面接触高度约为 Z=0；瓶子长轴沿 X，瓶体中心高度为 Z=1.75，平均水面 Z=1.60。小兔初始根位置约为 `(5.27, -0.25, 0)`，画纸中心约为 `(-4.75, -1.55, 0.022)`。
> 
> `.blend` 的单位设置为 METRIC、scale_length=1。制作参数主要作为统一场景单位使用；微缩感来自构图、比例、材质和镜头，不能将所有物体数值直接解释为真实玩具的厘米尺寸。
> 
> Object 的三个基础变换为：
> 
> ```python
> obj.location = (x, y, z)
> obj.rotation_euler = (rx, ry, rz)  # 弧度
> obj.scale = (sx, sy, sz)
> ```
> 
> 没有父级时，Object 的 location 通常直接描述其世界位置。有父级时，局部坐标会经过父级变换。完整数学形式还包括父级逆矩阵等 Blender 变换状态；本片用新建、明确设定的局部部件和独立世界控制点组织这些关系。
> 
> 例如兔头控制点在世界位置 `(5.27, -0.265, 1.08)` 附近，可见 `Rabbit head` 在控制点局部原点。眼睛和耳朵的位置相对头部定义，因此头转动时会一起转动。
> 
> ## 7 程序化建模与材质
> 
> ### 7.1 基础几何函数
> 
> build_scene.py（项目文件：`videos/bottle-storm-recreation-20261106/scripts/build_scene.py`） 封装了 `cube`、`sphere`、`cyl`、`mesh`、`curve`、`empty` 和 `lamp` 等函数。调用时传入名称、位置、尺寸、材质和可选父级，就能生成对象。
> 
> 下面是教学简化例子，需在 Blender 的 Python 环境中运行：
> 
> ```python
> bpy.ops.mesh.primitive_cube_add(size=1, location=(2, 0, 0.5))
> block = bpy.context.object
> block.name = '示例积木'
> block.dimensions = (1, 1, 1)
> ```
> 
> 本片的积木还加了倒角，避免直角边缘显得过于锋利。书本分别生成封面、书页和书脊；兔子由椭球部件组成；曲线增加 bevel_depth 后形成具有粗细的绳索、水草和画线。
> 
> ### 7.2 瓶子的生成
> 
> 先用一组 `(x, radius)` 定义瓶底、瓶身、肩部、瓶颈、瓶口的半径。沿 X 方向平滑插值，再对每个截面围绕 X 轴采样一圈顶点。
> 
> ```text
> x = 轴向位置
> r = 该位置的半径
> θ = 圆周角度
> 顶点 = (x, r·cosθ, 1.75 + r·sinθ)
> ```
> 
> 脚本生成外层和内层瓶壁。内层半径比外层小约 0.047 场景单位，并连接端部，形成有厚度的玻璃壳。正式瓶体采用 161 个轴向截面、每圈 96 点，再平滑表面并整理法线。
> 
> 瓶壁有厚度且法线方向正确，才能让 Cycles 得到稳定、合理的折射边界。
> 
> ### 7.3 水体和船
> 
> 水体使用顶面、底面和侧面闭合网格。顶面分为 110×36 个网格单元，再通过形态关键帧改变顶面高度。砂床沿瓶底曲面生成，使底部形状与瓶壳相适应。
> 
> 船壳由多个船体截面连接，甲板、桅杆、帆、栏杆和绳索分别生成。它们统一挂在船的根 Empty 下；改变根的姿态就能让整船保持部件关系一起摆动。
> 
> ### 7.4 材质节点
> 
> 几何决定轮廓，材质决定它怎样响应光线。脚本使用 Principled BSDF，并设置 Base Color、Roughness、Metallic、Transmission Weight、IOR 等参数。
> 
> | 材质 | 实现要点 |
> |---|---|
> | 玻璃 | IOR=1.46，透射权重 1，粗糙度约 0.035 |
> | 水体 | IOR=1.333，主体透射约 0.98；顶面另有泡沫混合 |
> | 陶瓷兔子 | 暖白底色、适度粗糙度、少量次表面效果 |
> | 木头 | Noise 节点与不同方向缩放，形成条纹和轻微凹凸 |
> | 鱼身 | 顶点颜色形成暗背亮腹，Voronoi 纹理生成细鳞片凹凸 |
> | 鱼鳍 | 较薄的膜面与细鳍条，带少量透射和反射 |
> 
> 水材质中创建过体积吸收节点，但没有连接到最终体积输出。正式清水效果主要由表面透射、颜色和折射实现。这里的“水体闭合”描述几何形状，不代表启用了完整参与介质体积渲染。
> 
> ## 8 帧 时间和关键帧
> 
> 帧是某一时刻的静止画面。24fps 表示每秒播放 24 张画面。Blender 本片从第 1 帧开始，换算为时间：
> 
> ```text
> t = (frame - 1) / 24
> ```
> 
> | Blender 帧 | 时间 |
> |---|---|
> | 1 | 0 秒 |
> | 13 | 0.5 秒 |
> | 25 | 1 秒 |
> | 481 | 20 秒的理论端点，不在本片 480 张输出内 |
> 
> 最后一张输出是第 480 帧，采样时刻约为 19.9583 秒；以 24fps 播放 480 张画面，视频时长正好为 20 秒。
> 
> 关键帧保存的是属性值，例如位置、角度、缩放、灯光能量或形态权重。渲染完成的 PNG 则是这些属性共同形成的最终图片。
> 
> 对于简单移动，可以只保存起点和终点，交给插值计算中间状态。本片动作更复杂，Python 先按时间函数计算每一帧，再将结果烘焙为密集的原生关键帧。鱼群运动及其形态权重使用线性插值，帧内连续性还用于运动模糊采样。
> 
> ## 9 小兔的动作时间表
> 
> | 时间范围 | 主要动作 | 主要计算 |
> |---|---|---|
> | 0 至 2.10 秒 | 看向画纸，转向并靠近积木 | 根位置、朝向、迈步、视线 |
> | 2.12 至 3.85 秒 | 蹲下、接触、抱起 | 骨盆降低、手部获取接触、积木抬升 |
> | 4.05 至 5.30 秒 | 转到瓶前搬运路线 | 加速、转身、脚步对齐 |
> | 5.30 至 12.70 秒 | 沿桌前搬到左边 | 连续位移、交替支撑脚、双手抓握 |
> | 13.20 至 14.15 秒 | 放下积木 | 下蹲、减速落地、道具姿态变化 |
> | 14.15 至 14.45 秒 | 松手 | 抓握权重平滑降低 |
> | 14.35 至 15.25 秒 | 移到画纸旁 | 侧移和身体转向 |
> | 15.80 至 16.85 秒 | 提笔并落到起笔点 | 笔尖弧线、笔轴方向、手腕跟随 |
> | 16.85 至 17.75 秒 | 画花茎 | 笔尖沿线段移动，画线逐步显现 |
> | 17.75 至 19.75 秒 | 画花朵 | 沿花瓣曲线移动和显线 |
> | 19.75 至 20 秒 | 轻微抬笔结束 | 笔尖离纸，保留收尾姿态 |
> 
> 动作之间存在重叠。头部先看向目标、手先获得接触、身体再移动，能让动作的因果关系更清楚。
> 
> ## 10 小兔的身体与步态
> 
> ### 10.1 根位置 朝向和缓动
> 
> bunny_motion.py（项目文件：`videos/bottle-storm-recreation-20261106/scripts/bunny_motion.py`） 定义 `root_at(t)`、`yaw_at(t)`、`posture(t)`、`foot_at(t, side)`、`box_at(t)`、`pen_at(t)` 和 `hand_targets(t)`。
> 
> `root_at(t)` 给出角色整体路线。身体朝向 yaw 决定局部前方和右方：
> 
> ```text
> R(yaw) = (cos(yaw), sin(yaw), 0)
> F(yaw) = (sin(yaw), -cos(yaw), 0)
> ```
> 
> 用 `root + R·左右偏移 + F·前后偏移 + 高度`，就可以根据同一根位置构造肩、胯、手部和道具的目标位置。
> 
> 位置过渡使用五次平滑函数：
> 
> ```text
> u = clamp((t-a)/(b-a), 0, 1)
> ease(u) = 6u⁵ - 15u⁴ + 10u³
> ```
> 
> 它让过渡起止的速度和加速度平缓。长距离搬运还使用带余弦速度起止段的 `cruise`，中间保持较稳定速度。
> 
> ### 10.2 脚底接触
> 
> 每只脚有一组明确的抬脚和落脚时间。支撑期保持世界位置固定；摆动期从上一个接触点移动到下一个接触点，并叠加抬脚弧线。
> 
> ```text
> 水平位置 = 起点与落脚点之间的平滑插值
> 抬脚高度 ≈ 0.135 × sin(πu)^1.4
> ```
> 
> 身体向前走时，支撑脚仍留在桌面原处，直到轮到它抬起。这是避免“脚在地面上滑着走”的关键。
> 
> ### 10.3 双段 IK
> 
> 先确定肩膀和手腕的目标位置，再根据上臂长度 a、前臂长度 b 计算肘部。腿部用相同思想，从胯和脚踝反推膝盖。
> 
> ```text
> d = 肩膀到手腕的距离
> along = (a² - b² + d²) / (2d)
> height = sqrt(a² - along²)
> ```
> 
> 肘部位于朝向目标的轴线上前进 along，再沿选定的弯曲方向偏移 height。这个弯曲方向由 pole 点控制，用于指定肘、膝应向哪边弯。
> 
> 本片上臂与前臂的计算长度为 0.28 和 0.27，腿部两段为 0.255 和 0.265。超过可达长度时，函数会限制求解距离并记录超伸误差。
> 
> 实际几何是分段的玩偶部件和控制 Empty。脚本把肢体部件放到两关节之间，并根据两端距离设置长度和朝向；工程没有使用一套连续皮肤网格的 Armature 蒙皮工作流。
> 
> ### 10.4 次级动作
> 
> 身体叠加轻微上下起伏、左右重心变化；耳朵加入低幅摆动和步态跟随；眨眼使用几个确定的短时间窗。视线根据阶段在画纸、积木、行进方向和笔尖之间过渡。
> 
> 这些动作幅度较小，主要用于保持玩偶的重量感和注意力方向。
> 
> ## 11 积木 抓握和画画的关联
> 
> ### 11.1 积木使用单一连续轨迹
> 
> `box_at(t)` 将积木分成原位、抬升、搬运、放下和落稳阶段。搬运阶段的中心位置由角色根位置、前方偏移和高度计算。
> 
> 双手接触点由积木的位置和朝向决定：
> 
> ```text
> 左手或右手目标 = 积木中心 + 积木右方向 × 左右偏移 + 小幅高度偏移
> ```
> 
> 获取接触时，手从自然姿态平滑靠近积木；搬运时保持接触；积木落稳后，接触权重逐渐降低。积木本身保持独立 Object，整个过程不需要切换它的父级。
> 
> 这样可以使放下时的位姿连续，并且让“手在哪里”依赖于“积木在哪里”，减少两套独立运动对不齐的问题。
> 
> ### 11.2 笔的位置和手的目标
> 
> 笔有自己的 Empty，笔尖、笔杆和笔帽是子物体。`pen_at(t)` 输出笔尖位置和笔轴方向。手的抓握目标由笔尖、笔轴以及抓握距离共同计算，再通过手臂 IK 得到肘部姿态。
> 
> 取得画笔时，笔尖加入一个抬起和绕行弧线，避免直接穿过身体。手腕旋转用四元数球面插值 slerp 平滑过渡到写画姿态。
> 
> ### 11.3 画线和笔尖共享路径
> 
> 花茎路径 `STEM` 是线段，花朵路径 `FLOWER` 根据带五瓣变化的极坐标曲线生成，并按长度重新采样。笔尖沿这条路径移动，画线的 `bevel_factor_end` 使用相同阶段的时间进度逐渐增大。
> 
> 画线和笔尖共享空间路径与进度，才能让纸上新增的线出现在笔尖经过的位置。
> 
> 这些画线目前以世界坐标定义，并未作为画纸的子物体自动随纸移动。修改画纸的位置、角度或尺寸时，需要同步更新 `STEM`、`FLOWER` 和笔尖路线。
> 
> ## 12 鱼群的路线和身体变形
> 
> ### 12.1 鱼模型的组成
> 
> 每条鱼由同一个网格包含鱼身、尾鳍、背鳍、胸鳍、腹鳍、鳍条、眼睛、鳃线和嘴部。不同区域使用材质槽及顶点颜色区分外观。
> 
> 鱼身沿长度方向定义截面宽度和高度，前身较饱满，尾柄逐渐变细。尾鳍是带弧度的分叉膜面，膜面上还有细鳍条。
> 
> ### 12.2 平滑路线和按弧长推进
> 
> fish_motion.py（项目文件：`videos/bottle-storm-recreation-20261106/scripts/fish_motion.py`） 用 `scipy.interpolate.CubicSpline` 构造周期闭合曲线。控制点组成两端圆滑回转、中段较长的巡游路线。
> 
> 直接均匀增加曲线参数，可能在不同区段得到不同速度。本片先密集采样 24,001 个参数点，累计相邻点距离，建立“已游距离到曲线参数”的查找关系，再按所需速度推进距离。
> 
> 速度变化先积分成距离，让加速和减速连续。九条鱼分为下层六条和上层三条，方向相反；尺寸、路线宽度、起始相位和细微高度变化各不相同。
> 
> 高度变化使用完整周期角度的正弦函数，路线闭合处也能连续衔接。
> 
> ### 12.3 让鱼头跟随运动方向
> 
> 脚本用前后很短时间的位置差计算速度，再计算鱼的 yaw、pitch。根据速度和加速度得到平面曲率，并用于转弯倾斜和身体弯曲。
> 
> ```text
> 位置 p(t)
> 速度 v(t) ≈ [p(t+Δt)-p(t-Δt)] / (2Δt)
> 朝向 = 速度方向
> 倾斜 = 根据曲率和速度得到的小幅角度
> ```
> 
> 姿态保存为四元数。相邻四元数若符号相反，会统一符号表示，避免相同姿态的数值表示影响插值连续性。
> 
> ### 12.4 让弯曲从身体前部传向尾端
> 
> 用 u 表示由前身到尾端的位置，侧向摆动近似为：
> 
> ```text
> A(u) = 0.084 × (0.025 + 0.975u²)
> y(u,t) = A(u) × sin(phase(t) - 2π × 0.92u)
> ```
> 
> 振幅向尾端增大，空间相位沿身体变化，从而形成向尾部传播的弯曲波。横截面会随中心线斜率转动，避免只把网格横向剪切。
> 
> 鱼鳍增加小幅滞后运动；转弯另加一个身体弯曲形态。摆尾频率根据鱼速和体型估算，并持续积累 phase，避免每段动作重新启动摆尾周期。
> 
> 这是一套用于动画的运动学模型。路线预先设计并检查，未使用流体求解或鱼群自主避障仿真。
> 
> ### 12.5 二十个形态的混合
> 
> upgrade_fish.py（项目文件：`videos/bottle-storm-recreation-20261106/scripts/upgrade_fish.py`） 为每条鱼生成 20 个摆尾形态，并保存 Basis 和额外转弯形态，共 22 个 Shape Key 数据块。
> 
> 每一帧把摆尾 phase 映射到两个相邻形态，令权重为 `1-w` 和 `w`，其余摆尾形态为 0。摆尾权重和保持为 1，转弯形态则单独叠加。
> 
> 整体运动根节点控制位置和朝向，网格形态关键帧控制局部弯曲。两者结合，形成“游过去、转过身、同时摆尾”的动作。
> 
> ## 13 海浪与船体的关联
> 
> 水面高度使用三个波叠加：
> 
> ```text
> h(x,y,t) = 1.60 + fade(x,y) × envelope(t)
>                      × Σ[A·sin(kx·x + ky·y - ω·t)]
> ```
> 
> | 波 | A | kx | ky | ω |
> |---|---|---|---|---|
> | 主波 | 0.17 | 2.5 | 0.38 | 2.6 |
> | 次波 | 0.08 | 4.2 | -1.2 | 3.7 |
> | 细波 | 0.045 | 7.3 | 2.0 | 5.1 |
> 
> `fade` 降低靠近瓶底端、瓶颈和横向边缘的波幅；`envelope` 让风暴随时间增强和回落，14 秒以后保持较低幅度。
> 
> 通过三角恒等式把每个波分解成空间 sin/cos 与时间系数，水面只需要 6 个波浪 Shape Key，就能表现随时间移动的波峰。
> 
> 船体用同一函数采样四点水位：前后相距 1.46、左右相距 0.46 场景单位。四点平均值决定高度，前后水位差决定俯仰，左右水位差决定侧倾，并加入适度幅度系数和小偏航。
> 
> 船姿态再传给根 Empty 下的所有子部件，船帆和绳索就会与船壳保持一致。该方法属于解析高度场驱动的浮动动画，重量、惯性和流体作用没有交给完整物理求解器计算。
> 
> ## 14 闪电 灯光和镜头
> 
> ### 14.1 闪电与辅助光
> 
> 闪电使用带随机折点的细曲线，添加分支和发光材质。八个触发时刻是 2.05、3.84、5.02、6.92、8.27、9.48、10.84、12.02 秒。
> 
> 触发时，曲线快速显现并带二次闪动；同时驱动瓶内散射辅助光和全屋反弹辅助光的能量。初版较明显的整屋亮暗变化由这组同步光产生。
> 
> 小兔版本把全屋辅助闪光降到初版的 30%，瓶内辅助闪光降到 72%。鱼群版本将稳定的水下可见性补光提高到 32，帮助鱼身保留体积和轮廓。
> 
> 有意设计的闪电亮度变化与采样噪声闪烁需要分别处理。前者调整光照时间曲线和幅度；后者通过采样、降噪、种子及材质设置控制。
> 
> ### 14.2 日常照明
> 
> 暖色主光塑造房间和玩偶，冷色窗边光提供边缘分离，瓶边补光帮助读出玻璃形体。细长区域光控制瓶身高光；灯塔和串灯提供局部暖光。
> 
> 部分补光在光泽或透射可见性上做了艺术控制，帮助保留玻璃内部物体的可读性。
> 
> ### 14.3 连续镜头
> 
> | 时间 | 相机行为 |
> |---|---|
> | 开头至 8 秒附近 | 保持全景，清楚交代小兔、积木、画纸和玻璃瓶 |
> | 8 至 12.80 秒 | 平缓向左跟随搬运 |
> | 13.15 至 16.55 秒 | 转入画纸近景，同时改变位置、拍摄方向和焦距 |
> | 16.55 至 20 秒 | 保持近景，观看笔尖和画线 |
> 
> 相机位置和观察目标分别插值，再用目标方向计算相机旋转。焦距从约 52mm 的全景逐步过渡到 34mm 附近，中间还有平缓的焦距调整。
> 
> 对焦距离跟随目标点，光圈从 f/64 逐步过渡到 f/8，让近景的背景自然虚化。相机路径、对焦和角色动作使用同一个时间 t，保证镜头与动作阶段对齐。
> 
> ## 15 Cycles 怎样把场景变成图片
> 
> Cycles 读取某一时刻的几何、材质、灯光和相机，使用路径追踪计算每个像素的颜色。鱼在什么位置由动作脚本决定；鱼经过光照、水面和瓶壁折射后在镜头里呈现的外观，由渲染器计算。
> 
> | 正式画面参数 | 值与作用 |
> |---|---|
> | 引擎 | Cycles |
> | 设备 | Apple Metal GPU |
> | 分辨率 | 1280×720 |
> | samples | 32，配合自适应采样和降噪 |
> | 自适应阈值 | 0.035 |
> | 随机种子 | 20261106，关闭每帧更换种子 |
> | 总反弹上限 | 8 |
> | 透射反弹上限 | 6，支持玻璃与水的多层透射 |
> | 色彩管理 | AgX，Medium High Contrast，曝光约 0.1 |
> | 运动模糊快门 | 0.3 帧，24fps 时约为 0.0125 秒 |
> | 持久数据 | 开启，减少连续帧重复准备的开销 |
> | 渲染 CPU 线程设置 | 8；主要追踪工作在 GPU 执行 |
> 
> 32 samples 是单张图的采样参数，24fps 是播放速度。将 samples 提高，并不会自动增加视频帧数。
> 
> ### 15.1 三种检查画面
> 
> - Workbench 动作预演：隐藏玻璃和水，便于检查手脚、积木与地面关系。它用于检查动作，不能代表最终光照和折射。
> - Cycles 低采样预览：50% 尺寸、12 samples，快速观察构图和运动。
> - 正式 Cycles：100% 尺寸、32 samples，输出完整 PNG 序列。
> 
> ### 15.2 逐帧输出和续渲
> 
> render_scene.py（项目文件：`videos/bottle-storm-recreation-20261106/scripts/render_scene.py`） 在每一帧调用 `scene.frame_set(frame)`，设置 PNG 路径，再执行 `bpy.ops.render.render(write_still=True)`。
> 
> 每帧完成后更新 `outputs/checks/render-final.json`。报告保存场景 SHA-256、采样数、帧目录、已完成帧和单帧耗时。
> 
> 续渲时会检查已有 PNG 的完整结束标记。场景指纹或采样数与旧报告不一致时，脚本拒绝混用旧帧。该机制检查当前规则中的场景和采样身份；修改渲染脚本内的其他参数时，也应主动归档旧帧后重新渲染。
> 
> 渲染时长会随玻璃覆盖面积、光线路径、硬件负载和系统睡眠变化。不能把包含等待或休眠的日志总时长直接当作纯 GPU 渲染成本。
> 
> ## 16 NumPy 和 SciPy 怎样合成声音
> 
> ### 16.1 声音也是数字序列
> 
> 48kHz 表示每秒记录 48,000 个声音采样值。20 秒单声道需要 960,000 个采样，立体声保存左右两份。这里的声音采样率与视频 fps 是不同概念。
> 
> NumPy 生成时间数组、随机噪声、正弦波和音量包络；SciPy 使用滤波器保留特定频段，再将结果保存为 WAV。
> 
> ### 16.2 环境声
> 
> synthesize_audio.py（项目文件：`videos/bottle-storm-recreation-20261106/scripts/synthesize_audio.py`） 使用固定随机种子 20261106，并将不同频段的噪声叠加：
> 
> | 声音层 | 大致频段 | 时间变化 |
> |---|---|---|
> | 海浪 | 45 至 430Hz | 缓慢起伏 |
> | 风声 | 240 至 1,800Hz | 随风暴强度和低频包络变化 |
> | 雨声 | 2,100 至 12,500Hz | 较细密的高频噪声 |
> | 雷声主体 | 28 至 230Hz | 触发后快速升起、逐步衰减 |
> | 雷声脆响 | 900 至 6,500Hz | 很短的尖锐起音 |
> 
> 滤波使用 `scipy.signal.butter` 生成三阶 Butterworth 滤波器，再用 `sosfilt` 处理噪声。雷声叠加延迟回声、左右声道差异和幅度变化，声音触发比闪电晚 0.09 秒。
> 
> ### 16.3 小兔版本的拟音分支
> 
> 当时间表版本为 `bunny-artist` 时，脚本读取 `outputs/checks/bunny/motion-audit.json` 中的落脚时刻，生成短促脚步声；14.15 秒加入积木落桌声；16.85 至 19.75 秒加入画笔摩擦声。
> 
> 越接近画画近景，环境声会适度降低，让小型拟音更容易听见。最终声音进行淡入淡出、软限幅和峰值归一化，再转换为 16 位双声道 PCM。
> 
> ### 16.4 当前文件与重建结果
> 
> 本次记录按实际媒体核验了两个文件：
> 
> | 文件 | 核验结果 |
> |---|---|
> | `public/storm-soundscape.wav` | 14.000 秒、672,000 个采样/声道、48kHz、双声道 |
> | `outputs/checks/documentation/rebuild-audio/public/storm-soundscape.wav` | 20.000 秒、960,000 个采样/声道、48kHz、双声道 |
> 
> 当前正式成片的 AAC 流封装为约 20.011 秒，但解码后第 15 至 20 秒为静音。容器和流的时长足够长，并不证明每一段都包含实际声音。
> 
> 20 秒分支调用了 `math.tau`，原脚本缺少 `import math`。本次已补齐导入，在独立检查目录使用当前时间表和脚步数据成功生成 20 秒 WAV。证据见 audio-rebuild-check.json（项目文件：`videos/bottle-storm-recreation-20261106/outputs/checks/documentation/audio-rebuild-check.json`）。现有 MP4、正式 WAV 和 `.blend` 保留原内容。
> 
> 要将完整声音应用到视频，需要先备份正式 WAV 和 MP4，再运行 `npm run audio` 与 `npm run render`。这一操作只涉及声音与合成，画面没有变化时可复用现有 `bunny-fish-picture.mp4`，不必重新计算 480 张三维图像。
> 
> ## 17 FFmpeg 和 Remotion 怎样组成视频
> 
> ### 17.1 PNG 编码为无声画面
> 
> FFmpeg 读取 `outputs/frames-fish/frame_%04d.png`，以 24fps 编码 480 张图像，使用 libx264、CRF 17、yuv420p 和 faststart 输出 `public/bunny-fish-picture.mp4`。
> 
> 项目命令是：
> 
> ```bash
> npm run encode
> ```
> 
> `faststart` 将 MP4 索引前置，便于播放器更快开始读取。yuv420p 有助于通用播放器兼容。
> 
> ### 17.2 React 表达时间轴
> 
> src/Composition.tsx（项目文件：`videos/bottle-storm-recreation-20261106/src/Composition.tsx`） 定义 `BottleStorm` 合成：
> 
> ```tsx
> <Composition
>   id="BottleStorm"
>   component={MyComponent}
>   durationInFrames={480}
>   fps={24}
>   width={1280}
>   height={720}
>   defaultProps={{ source: "bunny-fish-picture.mp4" }}
> />
> ```
> 
> 组件内分开读取画面与声音：
> 
> ```tsx
> <Video src={staticFile(source)} muted />
> <Audio src={staticFile("storm-soundscape.wav")} volume={0.8} />
> ```
> 
> 这是结构简化示意，正式组件还设置了 durationInFrames、premountFor、尺寸和 objectFit。
> 
> 视频轨静音，声音只由单独的 Audio 轨提供，便于更换音轨和调整音量。React 在本片中负责合成层组织；三维模型与鱼身变形已经包含在 Cycles 渲染的视频画面中。
> 
> ### 17.3 两套帧编号
> 
> Blender 第 1 帧对应 Remotion 第 0 帧，时间都为 0 秒。转换关系为：
> 
> ```text
> Remotion frame = Blender frame - 1
> ```
> 
> 最终导出使用：
> 
> ```bash
> npm run render
> ```
> 
> 项目脚本指定源画面、H.264 编码、CRF 17 和并发数 2，并优先使用系统 Chrome 进行浏览器渲染。
> 
> ## 18 可编辑工程与媒体文件的区别
> 
> | 文件 | 保存什么 | 适合怎样修改 |
> |---|---|---|
> | `.blend` | 几何、材质、灯光、相机、关键帧 | 改鱼、小兔、瓶子、镜头、光照 |
> | Python 脚本 | 生成规则和动作计算方法 | 成批改变尺寸、路径、节奏及约束 |
> | `fish-paths.json` | 九条鱼每帧的路径、朝向和摆尾相位等数值 | 中间运动数据，通常由脚本重建 |
> | PNG 序列 | 每一帧的最终画面 | 抽帧检查、重新编码 |
> | 无声 MP4 | 已渲染的连续画面 | 组合声音、剪辑、发布 |
> | WAV | 声音波形 | 调音量、替换音效、重新合成 |
> | 最终 MP4 | 编码压缩后的画面与声音 | 播放和分享 |
> 
> 改变鱼的路线后，需要重建关键帧并重新渲染三维画面。改变音轨音量，通常只需调整合成参数并重新导出。改变字体字幕等额外二维层时，可以在 Remotion 合成层处理。
> 
> ## 19 质量检查的实际内容和范围
> 
> | 检查 | 方法 | 本版本记录 |
> |---|---|---|
> | 原生场景外部图片依赖 | 检查未打包图像的路径 | 未发现视频目录之外的图像依赖 |
> | 船与水面 | 在部分关键帧对实际求值水面做四点射线采样 | 最大高度误差约 0.001501 场景单位 |
> | 小兔站立脚 | 连续支撑期世界坐标比较 | 最大滑动为 0 |
> | 手与积木接触 | 实际手腕位置与积木接触目标比较 | 最大偏差约 2.38×10⁻⁷ 场景单位 |
> | 四肢可达性 | 比较目标距离与肢体长度 | 记录的手脚超伸为 0 |
> | 笔与头/身体 | 在笔杆上取点，检测包围椭球的归一化距离 | 最小约 1.092，未检测到穿入 |
> | 相机裁切 | 检查角色包围盒投影范围 | 未发现角色和搬运中的积木越界 |
> | 鱼与瓶壁 | 所有 480 帧求值顶点对瓶体半径边界计算 | 最小间距约 0.1670 场景单位 |
> | 鱼与砂床 | 所有 480 帧求值顶点对砂床模型计算 | 最小间距约 0.1154 场景单位 |
> | 鱼与水面 | 所有 480 帧求值顶点对解析波面计算 | 最小间距约 0.05682 场景单位 |
> | 鱼身之间 | 包围盒筛选，结合鱼身网格 BVH 近邻法线检测 | 未检测到达到判定阈值的鱼身穿插 |
> | 鱼的形态混合 | 在每四帧抽取三个子帧检查权重 | 最大权重和误差小于 9×10⁻⁸ |
> | PNG 完整性 | 尺寸、次序、亮度、文件数量和哈希 | 480 帧，未发现重复哈希或空帧 |
> | 最终媒体 | ffprobe 检查尺寸、帧率、时长、音频流 | 视频规格通过 |
> | 解码 | FFmpeg 完整解码 | 未报告解码错误 |
> | 工程归档 | 相对路径检查、文件 SHA-256 与 ZIP CRC | 制作交付时归档检查通过 |
> 
> 这些检查有明确范围。鱼鳍参与瓶壁、水面和砂床边界检查；鱼与鱼的网格检查主要针对鱼身，不是所有薄鳍膜与眼睛部件的精确布尔相交检查，也没有对每根水草做碰撞求解。小兔笔杆检查使用包围椭球。船的水面射线检查是若干代表帧抽样。
> 
> 积木放下后变成布景道具，近景相机转向画纸时允许它短暂裁切；搬运过程的手持积木和角色应保持在画面中。
> 
> 已有 `check.mjs` 检查音频流存在，但没有验证正式 WAV 的完整时长和成片尾段的有效声能，因此初版音轨的问题未被旧检查发现。以后更换声音或修改时长，应同时检查源音轨时长、抽听结尾，并检查最终文件分段 RMS 或峰值。
> 
> 主要报告：
> 
> - 原生场景检查（项目文件：`videos/bottle-storm-recreation-20261106/outputs/checks/scene-audit.json`）
> - 小兔接触检查（项目文件：`videos/bottle-storm-recreation-20261106/outputs/checks/bunny/contact-audit.json`）
> - 鱼群运动检查（项目文件：`videos/bottle-storm-recreation-20261106/outputs/checks/fish/motion-audit.json`）
> - 鱼群子帧检查（项目文件：`videos/bottle-storm-recreation-20261106/outputs/checks/fish/subframe-check.json`）
> - PNG 检查（项目文件：`videos/bottle-storm-recreation-20261106/outputs/checks/frame-qa.json`）
> - 最终格式与项目检查（项目文件：`videos/bottle-storm-recreation-20261106/outputs/checks/final-check.json`）
> - 本次音频核验与重建（项目文件：`videos/bottle-storm-recreation-20261106/outputs/checks/documentation/audio-rebuild-check.json`）
> 
> ## 20 安装 预览和重建
> 
> 所有命令以本视频目录为工作目录。不要从项目根目录借用其他视频的脚本、素材、依赖或缓存。
> 
> ### 20.1 独立安装
> 
> 系统准备 Node.js、Python 3.13、Blender 5.2 和 FFmpeg。当前版本的依赖清单和锁文件属于本视频自己。
> 
> ```bash
> mkdir -p .cache/tmp .cache/npm .cache/python
> export TMPDIR="$PWD/.cache/tmp"
> export TMP="$PWD/.cache/tmp"
> export TEMP="$PWD/.cache/tmp"
> export npm_config_cache="$PWD/.cache/npm"
> export PIP_CACHE_DIR="$PWD/.cache/python"
> 
> npm ci
> python3 -m venv .venv
> .venv/bin/python -m pip install -r requirements.txt
> ```
> 
> Blender 脚本通过系统 `blender` 命令运行；NumPy/SciPy/Pillow 脚本通过本目录 `.venv` 运行。
> 
> ### 20.2 直接预览或重导出现有画面
> 
> ```bash
> npm run dev
> npm run check
> ```
> 
> Studio 地址为 `http://localhost:3076/BottleStorm`。预览直接读取本视频已有的画面 MP4 和 WAV。
> 
> 需要重新合成现有素材时执行 `npm run render`。它不会重新计算三维模型的光照，输出路径由 `docs/timeline.json` 和 `run.mjs` 决定。
> 
> ### 20.3 从快照重建小兔和鱼群
> 
> `npm run scene` 调用 `rebuild_scene.py`，实际顺序是：
> 
> ```text
> 读取 outputs/versions/v1/bottle-storm.blend
>         ↓
> animate_bunny.py 生成小兔、道具、相机和 20 秒时间轴
>         ↓
> 将结果复制到 outputs/versions/bunny-v2/ 作为鱼群升级的输入
>         ↓
> fish_motion.py 生成 docs/fish-paths.json
>         ↓
> upgrade_fish.py 读取 bunny-v2 快照并替换鱼群
>         ↓
> 保存 public/bottle-storm.blend
> ```
> 
> `bunny-v2` 是当前视频内的制作阶段快照，重建脚本会更新其中的中间场景。需要保留手工编辑版本时，应先复制到本视频的另一个版本目录。
> 
> 完整重建流程：
> 
> ```bash
> # 先把需要保留的 .blend、PNG、报告、WAV 和 MP4 归档到本视频 outputs/versions/
> npm run scene
> node scripts/run.mjs audit-bunny
> node scripts/run.mjs audit-fish
> node scripts/run.mjs audit-scene
> npm run preview:scene -- 73 193 313 397 445
> 
> # 将上一轮 outputs/frames-fish/ 和对应 render-final.json 归档后，再正式渲染
> npm run render:scene
> .venv/bin/python scripts/qa_frames.py
> npm run encode
> npm run audio
> npm run render
> npm run check
> ```
> 
> `npm run audio` 现在可以运行完整的 20 秒拟音分支，会写入正式 WAV。原成片及其音轨需要保留时，应在运行前将它们复制到本视频版本目录。
> 
> ### 20.4 只修改鱼群
> 
> 1. 修改 `fish_motion.py` 中的路线、尺寸、层次、速度等参数。
> 2. 若要修改外形、鱼鳍或摆尾形态，修改 `upgrade_fish.py`。
> 3. 执行下列命令：
> 
> ```bash
> node scripts/run.mjs fish-paths
> node scripts/run.mjs upgrade-fish
> node scripts/run.mjs audit-fish
> node scripts/run.mjs audit-bunny
> node scripts/run.mjs audit-scene
> ```
> 
> `upgrade-fish` 从 `bunny-v2` 快照读取场景。若此前在正式 `.blend` 手工改了兔子或镜头，必须先将这些修改纳入输入快照，否则升级鱼群会回到快照中的兔子和镜头版本。
> 
> 检查通过后归档旧 PNG，重新执行渲染、编码和合成。
> 
> ### 20.5 修改初始房间或瓶身
> 
> 房间和原始瓶中场景由 `build_scene.py` 创建，`npm run base` 会把结果写到 `public/bottle-storm.blend`。而小兔重建读取的是 `outputs/versions/v1/bottle-storm.blend`。
> 
> 因此修改基础建模后，需要先备份现有 v1 快照，再将新的基础 `.blend` 和对应基础时间表作为新的 v1 输入保存，然后运行 `npm run scene`。只修改 `build_scene.py` 并执行小兔重建，不会自动让旧 v1 快照更新。
> 
> ### 20.6 只更换或补齐声音
> 
> 画面没有变化时，可复用现有无声 MP4。先备份 WAV 和最终 MP4，再执行：
> 
> ```bash
> npm run audio
> npm run render
> npm run check
> ffprobe -v error -show_entries format=duration public/storm-soundscape.wav
> ```
> 
> 还应抽听或检测成片的最后几秒，确认声音内容确实覆盖到结尾。
> 
> ### 20.7 修改时长或帧率
> 
> 本片时长参数分布在多处。需要同步修改：
> 
> - `bunny_motion.py` 的 FPS、DURATION、阶段时间和步态事件。
> - `fish_motion.py` 的 FPS、FRAMES、采样与摆尾积分。
> - 波浪、闪电、镜头及声音的时间安排。
> - `docs/timeline.json`。
> - `src/Composition.tsx` 的 durationInFrames、fps 和轨道时长。
> - `run.mjs` 的 FFmpeg 帧率、帧数。
> - 固定抽帧、检查和形态子帧检查脚本中的范围。
> 
> 提高 fps 后，需要生成更密的运动状态并重新渲染对应数量的图像。只给旧视频改一个 fps 元数据，不能获得额外的真实动作画面。
> 
> ## 21 常见问题与修改方向
> 
> | 现象 | 主要检查点 | 本片的处理方法 |
> |---|---|---|
> | 鱼像贴纸滑动 | 只有整体位移，身体和尾鳍缺少变形 | 形态关键帧、尾端增幅、鳍的跟随 |
> | 鱼侧着滑或瞬间掉头 | 朝向未跟随速度，旋转表示有跳变 | 路径切线朝向、四元数连续性 |
> | 曲线路段忽快忽慢 | 曲线参数与游过距离不等价 | 弧长查找表 |
> | 闭合路线接缝突然抬升 | 高度函数没有按整圈周期闭合 | 使用周期角度生成高度 |
> | 小兔脚滑 | 支撑脚随身体一起平移 | 世界坐标支撑点和交替抬脚 |
> | 手离开积木 | 手和道具各有独立轨迹 | 由积木计算接触目标，再解 IK |
> | 放下道具时跳动 | 位姿或父级切换不连续 | 单一道具轨迹、渐变接触权重 |
> | 笔穿过头部 | 取笔路径或笔轴方向不合理 | 绕行弧线、包围椭球间距检查 |
> | 画线与笔尖脱节 | 两者使用不同路线或进度 | 共享路径和阶段进度 |
> | 船浮在波浪上方 | 船姿态与水面计算不一致 | 四点采样同一个高度函数 |
> | 整屋明显闪动 | 同步辅助闪光过强 | 降低房间与瓶内闪光幅度 |
> | 细碎光噪声变化 | 采样、种子、反射细节或降噪 | 增加采样，固定种子，检查材质 |
> | 修改后导出仍像旧版 | 旧 PNG 或阶段快照被复用 | 检查输入快照、场景指纹并归档旧帧 |
> | 结尾没有声音 | WAV 比合成时间轴短 | 核验源时长、重建完整音轨、检查尾段 |
> 
> ## 22 渲染环境与文件隔离
> 
> 一个视频的代码、素材、依赖、快照、检查和输出全部位于当前目录。`run.mjs` 设置本地 TMPDIR、TMP、TEMP、npm/Python 缓存、Blender 配置及脚本目录，保持不同视频之间没有运行依赖。
> 
> 本机曾遇到 Metal 二进制归档编译崩溃。运行脚本设置 `CYCLES_METAL_DISABLE_BINARY_ARCHIVES=1` 绕过这条缓存路径，仍由 Metal GPU 执行 Cycles。系统驱动自身的编译缓存和已有浏览器缓存无法由项目完全指定路径，因此不能声称所有系统缓存都已隔离。
> 
> 系统 Blender、FFmpeg、Python 和 Chrome 可以使用；本视频的脚本、素材和依赖不能从另一个视频目录导入。需要复用的文件应复制到当前视频目录，并声明自己的依赖版本。
> 
> 版本归档也必须保留在本视频目录内。不要通过符号链接、硬链接或跨视频相对路径借用其他作品。
> 
> ## 23 源码阅读顺序与文件职责
> 
> | 文件 | 输入 | 输出或作用 |
> |---|---|---|
> | `scripts/run.mjs` | 命令、时间表 | 设置路径环境，调用 Blender、Python、FFmpeg 和 Remotion |
> | `scripts/build_scene.py` | 建模与材质参数 | 基础卧室、瓶子、水、船和原始动画 |
> | `scripts/bunny_motion.py` | 时间 t | 根位置、朝向、姿态、脚点、道具轨迹及手部目标 |
> | `scripts/animate_bunny.py` | v1 场景和动作函数 | 小兔部件、道具、镜头及原生关键帧 |
> | `scripts/fish_motion.py` | 路线与体型参数 | `docs/fish-paths.json` |
> | `scripts/upgrade_fish.py` | bunny-v2 场景和鱼运动数据 | 完整鱼模型、变形和运动关键帧 |
> | `scripts/rebuild_scene.py` | 当前视频的阶段快照 | 顺序重建小兔和鱼群 |
> | `scripts/render_scene.py` | 已保存 `.blend` 与时间表 | 预览或正式 PNG、渲染进度报告 |
> | `scripts/synthesize_audio.py` | 时间表、脚步接触数据 | WAV 和音频参数报告 |
> | `src/Composition.tsx` | 无声 MP4 与 WAV | 20 秒 Remotion 合成 |
> | `scripts/audit_scene.py` | 实际 `.blend` | 水面与船体、依赖检查 |
> | `scripts/audit_bunny.py` | 实际 `.blend`、动作记录 | 接触、可达性、镜头及笔杆检查 |
> | `scripts/audit_fish.py` | 实际 `.blend`、鱼轨迹 | 鱼群边界、形态与鱼身穿插检查 |
> | `scripts/inspect_fish_cycles.py` | 实际 `.blend` | 摆尾子帧权重检查 |
> | `scripts/qa_frames.py` | PNG 序列 | 完整性、重复帧和联系表 |
> | `scripts/check.mjs` | 代码、报告和成片 | 类型、路径和媒体格式检查 |
> | `scripts/package_project.py` | 当前视频的交付文件 | 可移植工程 ZIP 与文件哈希清单 |
> | `scripts/verify_archive.py` | 工程 ZIP | 路径、必要文件、哈希和 CRC 检查 |
> | `scripts/dump_scene_inventory.py` | 实际 `.blend` | 本次制作记录使用的场景清单 |
> 
> 先阅读 `run.mjs` 可看清命令入口，再阅读 `bunny_motion.py` 和 `fish_motion.py` 理解动作规则，最后对照建模和渲染脚本查看这些数字如何写入 Blender。
> 
> ## 24 方法边界与后续扩展
> 
> 本片的动作主要来自解析公式、预设路径、接触约束和程序化关键帧。Cycles 提供基于光线传播的渲染，但运动本身仍按设计的动画规则计算。
> 
> - 水面使用叠加波，船由高度场驱动；增加真实碰撞、液体飞溅或惯性时，需要引入相应物理求解和新的检查方法。
> - 小兔使用分段玩偶部件和 IK。若制作复杂皮肤形变、布料和肌肉运动，可改用连续网格、骨骼蒙皮及相关模拟。
> - 鱼使用设计好的路线和运动学摆尾。增加环境交互或自主避障，需要另外实现行为和碰撞响应。
> - 光滑轨迹和通过检查有助于提高连贯性，视觉上的重量感、节奏和精致程度仍需播放样片判断。
> - 原帖作为造型、构图和运动参考。当前工程独立重建了几何、材质和动作；没有原帖作者的原始工程参数，不能承诺逐像素一致。
> 
> ## 附录 A 参考与可核验信息
> 
> 参考帖子：<https://x.com/guanmo_ai/status/2118258208045494688>。参考帧位于 `public/reference/`，用于观察和对照，没有用作最终渲染背景或成片帧。
> 
> 制作代码快照：Git 提交 `0e5b7e6abe4e97a74f8002894881332e5cd396a3`。流程关系图的源码链接固定到这一提交。本次文档整理补齐了音频脚本的 `math` 导入，并新增场景清单与制作手册。
> 
> 当前原生场景 SHA-256：
> 
> ```text
> 313a9a224daea1937789eb6483136f76c8bc9884f243111885e0494a3413d111
> ```
> 
> 相关资料：
> 
> - [Blender 形态关键帧说明](https://docs.blender.org/manual/en/4.4/animation/shape_keys/shape_keys_panel.html)
> - [鱼类身体波运动学研究](https://pmc.ncbi.nlm.nih.gov/articles/PMC6748450/)
> - [摆尾频率与游速研究](https://pubmed.ncbi.nlm.nih.gov/34847768/)
> 
> ## 附录 B 两个最小教学例子
> 
> 下面示意如何把规则写成动画属性。代码需在 Blender Python 环境运行，其中辅助函数需自行提供；这些短例子用于说明结构，完整项目还包含接触、姿态、形态和检查逻辑。
> 
> 给积木记录起点和终点：
> 
> ```python
> block.location = (0, 0, 0.5)
> block.keyframe_insert(data_path='location', frame=1)
> 
> block.location = (2, 0, 0.5)
> block.keyframe_insert(data_path='location', frame=25)
> ```
> 
> 逐帧计算复杂动作：
> 
> ```python
> for frame in range(1, 481):
>     t = (frame - 1) / 24
>     rabbit_root.location = root_at(t)
>     rabbit_root.keyframe_insert(data_path='location', frame=frame)
> 
>     # 同一个 t 继续计算道具、手腕、脚点、肘膝、相机和灯光
>     # 保存几何状态以后，再交给 Cycles 渲染该时刻的画面
> ```
> 
> 本片把一个动作拆成可计算的时间、坐标、形状和接触关系，再把这些关系统一到同一条时间轴。这使场景能够保存、重建、检查，并针对单个环节修改。

> [!info]- 来源文档：videos/bottle-storm-recreation-20261106/docs/bunny-direction.md
> # 小兔搬积木与画画 · 动作要求
> 
> 用户希望小兔把积木搬到左侧画纸旁，再开始画画；动作连贯、可信，保留现有三维画面的精致感。
> 
> 20 秒连续镜头，24fps：
> 
> - 0–2.1 秒：注意画纸，转身走近积木。
> - 2.1–3.85 秒：屈膝下蹲、双手接触、抱起并起身。
> - 4.05–12.7 秒：绕至玻璃瓶前，沿桌面搬到左侧，耳朵与身体保留轻微跟随。
> - 13.2–14.15 秒：下蹲、积木减速落稳，手再松开。
> - 14.35–16.85 秒：移至画纸旁，拿起适合玩偶尺寸的小蜡笔，绕开身体后落笔。
> - 16.85–20 秒：一手支撑，另一手画出花茎与花朵；画线与笔尖共同使用同一条轨迹。
> 
> 接触约束：站立脚用世界坐标固定；双段 IK 使上下肢长度保持一致；积木/笔各使用单一连续变换，避免切换父级造成跳变；双手抓握跟随积木接触点；笔杆与头、身体作包围椭球间距检查。
> 
> 镜头：先保持原全景，从 8 秒开始平缓跟随左移；13.15–16.55 秒转入纸面近景。正式版使用原有 Cycles 场景、32 采样、固定种子、降噪、轻量运动模糊和渐变景深。
> 
> 闪电：保留瓶内雷雨；全屋辅助闪光降至原来的 30%，瓶内补光降至 72%。旧版大幅亮暗变化与脚本闪电强度的相关系数约 0.99。
> 
> 原始 14 秒版本及工程包保留；全部新文件均位于当前视频目录。

> [!info]- 来源文档：videos/bottle-storm-recreation-20261106/docs/fish-revision.md
> # 鱼群优化记录
> 
> 参考：<https://x.com/guanmo_ai/status/2118258208045494688>。逐半秒观察原帖画面，重点对照游动距离、双向穿行、纵深遮挡和侧身转向。参考帧在 public/reference/fish-motion/，未导入原帖三维模型或运动数据。
> 
> 旧版使用刚性椭球鱼身和单片三角尾鳍，身体固定朝向，位置只在各自的窄区域内摆动。新版保留小兔、相机和雷雨时间轴，替换九条鱼的形体和运动。
> 
> - 有厚度的鱼身带暗背、亮腹、细鳞片凹凸；补全双侧眼睛、鳃线、嘴、分叉尾鳍、背鳍、胸鳍与鳍条。使用反射和柔和补光塑造体积，鱼身没有自发光。
> - 身体按由前向后传播的弯曲波变形，尾部振幅更明显，横截面随曲线转动。20 个循环形态混合为连续动作；胸鳍和背鳍带轻微滞后摆动。
> - 两层路线反向穿行。路径采用圆滑回转和长距离游过，按弧长推进；朝向沿速度方向，转弯时带身体弯曲和倾斜。速度平滑变化、摆尾频率随游速调整，九条鱼相位不同。
> - 全部保存为 Blender 原生形态关键帧和变换关键帧，打开 .blend 即可播放，不依赖 Python 帧回调。
> 
> 运动检查覆盖 480 帧的真实求值网格，包含鱼鳍：与玻璃、砂床、水面保持间距；鱼身网格之间没有检测到相交；形态混合权重有效；朝向与运动切线一致。检查报告为 outputs/checks/fish/motion-audit.json。帧间形态权重另见 outputs/checks/fish/subframe-check.json，九条鱼均为线性混合，权重和的最大数值误差小于 9×10⁻⁸。
> 
> outputs/checks/fish/before-after-8s.mp4 是同一时刻、同一画面区域的放大对比。正式成片采用 1280×720、24fps、Cycles 32 samples、固定噪声种子、降噪和 0.3 快门运动模糊，与小兔版本保持同一套画面设置。
> 
> 技术资料：
> 
> - Blender relative shape keys: <https://docs.blender.org/manual/en/4.4/animation/shape_keys/shape_keys_panel.html>
> - Body-wave kinematic model: <https://pmc.ncbi.nlm.nih.gov/articles/PMC6748450/>
> - Tail-beat frequency and speed: <https://pubmed.ncbi.nlm.nih.gov/34847768/>

> [!info]- 来源文档：videos/bottle-storm-recreation-20261106/docs/visual-review.md
> 当前角色版为 20 秒；下面的 reference-comparison.jpg 对应初版参考重建。新版本的动作与接触检查见 outputs/checks/bunny/，最终缩略图见 outputs/checks/contact-sheet.jpg。
> 
> # 画面对照说明
> 
> 参考与重建并排图：outputs/checks/reference-comparison.jpg。
> 
> 已重建的主要关系：横置玻璃瓶与木桌；瓶内水上帆船/灯塔和水下鱼草；粉色卧室、窗户、暖灯、兔子、书本与积木；波浪驱动船体；闪电同步照亮瓶内与房间；缓慢推近镜头。
> 
> 本版不是逐像素复刻。可见差异包括瓶肩曲面、船帆轮廓与船壳细节、鱼群造型和密度、植物形态、床铺及灯具比例、玻璃高光与水面泡沫纹理。没有原始工程，因此参数和随机种子由本项目自行设计，不给出没有依据的相似度百分比。
> 
> 制作验证：原生 Blender 工程无外部贴图依赖；水面真实网格与船体四点采样在九个时间点对照；所有最终帧单独用 Cycles 渲染；Remotion 组合画面和合成音轨；具体检查结果见 outputs/checks/。

## 证据清单与缺失信息

盘点的文本/报告文件 68 个，源文本、报告及媒体产物身份摘要为 `89ef8d5ecb7ee12b5652e4d288c6ce32c3c99e992b203531fc80c56095db2aee`。哈希证明采集内容身份，不代表其中每个断言已验证。模型权重、缓存、依赖、第三方源码树和未指定的历史会话未采集；大型文本仅做哈希并按需要选读。

- 本次整理基于明确引用的 README、制作方法、配置、提示词、关键源码与已有报告。未重新观看全部参考/候选/成片，也未做人工听辨。
- 项目未提供的原始模型内部实现、未披露生成后端、未记录 seed/参数、真实费用、人工总工时和用户最终验收，均不补造。实际日志有记录的耗时见上文，流水线耗时不等于整个制作总耗时。
- 源项目内的路径只用于查证；本档案未复制大型素材或模型，也不改变源项目运行依赖。

| 已盘点证据文件 | SHA-256（前 16 位）/状态 |
| --- | --- |
| `videos/bottle-storm-recreation-20261106/README.md` | `009b2b829e8f40c4` |
| `videos/bottle-storm-recreation-20261106/eslint.config.mjs` | `7f2ca640409fd597` |
| `videos/bottle-storm-recreation-20261106/package-lock.json` | `e892d1bdc137fa0e` |
| `videos/bottle-storm-recreation-20261106/package.json` | `dc5c6de00268e746` |
| `videos/bottle-storm-recreation-20261106/remotion.config.ts` | `393bd458f71a50df` |
| `videos/bottle-storm-recreation-20261106/requirements.txt` | `4fda4e054773f608` |
| `videos/bottle-storm-recreation-20261106/tsconfig.json` | `a6fbcf514dbfd3d1` |
| `videos/bottle-storm-recreation-20261106/docs/bunny-direction.md` | `ba696a646c13df48` |
| `videos/bottle-storm-recreation-20261106/docs/fish-paths.json` | `2c44908a72ea5770`；内容超限未展开 |
| `videos/bottle-storm-recreation-20261106/docs/fish-revision.md` | `a124917698641053` |
| `videos/bottle-storm-recreation-20261106/docs/package-manifest.json` | `eb0f5621753c4e32` |
| `videos/bottle-storm-recreation-20261106/docs/production-brief.md` | `cabecbb22c76e7ec` |
| `videos/bottle-storm-recreation-20261106/docs/production-method.md` | `cc0fa19dce546e10` |
| `videos/bottle-storm-recreation-20261106/docs/scene-inventory.json` | `6ead167904f464be`；内容超限未展开 |
| `videos/bottle-storm-recreation-20261106/docs/source-post.json` | `223e147835239a64` |
| `videos/bottle-storm-recreation-20261106/docs/timeline.json` | `8bdeeb155bbb500d` |
| `videos/bottle-storm-recreation-20261106/docs/visual-review.md` | `48b0a70418687dec` |
| `videos/bottle-storm-recreation-20261106/outputs/checks/archive-check.json` | `2af0b90e73507e57` |
| `videos/bottle-storm-recreation-20261106/outputs/checks/audio.json` | `daed8cd0cc055d01` |
| `videos/bottle-storm-recreation-20261106/outputs/checks/final-check.json` | `869989f4329b85bd` |
| `videos/bottle-storm-recreation-20261106/outputs/checks/frame-qa.json` | `59e970a66c52eee5` |
| `videos/bottle-storm-recreation-20261106/outputs/checks/package-check.json` | `2efc17de2b5aca9e` |
| `videos/bottle-storm-recreation-20261106/outputs/checks/render-blocking.json` | `23f872f161cbd2ff` |
| `videos/bottle-storm-recreation-20261106/outputs/checks/render-final.json` | `cf34c72538a26187` |
| `videos/bottle-storm-recreation-20261106/outputs/checks/render-fish-preview.json` | `d6e5cf6cccd8d0ba` |
| `videos/bottle-storm-recreation-20261106/outputs/checks/render-preview.json` | `e8dcb64b21ae2aca` |
| `videos/bottle-storm-recreation-20261106/outputs/checks/render-test.json` | `0d00b6a4b843037d` |
| `videos/bottle-storm-recreation-20261106/outputs/checks/scene-audit.json` | `da5d9dd1998b6558` |
| `videos/bottle-storm-recreation-20261106/outputs/checks/bunny/archive-check.json` | `c13c0351ab59ae30` |
| `videos/bottle-storm-recreation-20261106/outputs/checks/bunny/contact-audit.json` | `dabfe7115647989c` |
| `videos/bottle-storm-recreation-20261106/outputs/checks/bunny/flash-diagnosis.json` | `837efdc5309933b8` |
| `videos/bottle-storm-recreation-20261106/outputs/checks/bunny/motion-audit.json` | `cf0a4536e1f364cc`；内容超限未展开 |
| `videos/bottle-storm-recreation-20261106/outputs/checks/bunny/package-check.json` | `3843ec6443d7b6ee` |
| `videos/bottle-storm-recreation-20261106/outputs/checks/documentation/audio-rebuild-check.json` | `4a781ce5cb49bf86` |
| `videos/bottle-storm-recreation-20261106/outputs/checks/documentation/document-check.json` | `49535839922535dd` |
| `videos/bottle-storm-recreation-20261106/outputs/checks/documentation/rebuild-audio/docs/timeline.json` | `8bdeeb155bbb500d` |
| `videos/bottle-storm-recreation-20261106/outputs/checks/documentation/rebuild-audio/outputs/checks/audio.json` | `31c803286994dbf3` |
| `videos/bottle-storm-recreation-20261106/outputs/checks/documentation/rebuild-audio/outputs/checks/bunny/motion-audit.json` | `cf0a4536e1f364cc`；内容超限未展开 |
| `videos/bottle-storm-recreation-20261106/outputs/checks/fish/archive-check.json` | `1d697b83b11dc40e` |
| `videos/bottle-storm-recreation-20261106/outputs/checks/fish/build.json` | `1aeb9fb3ed5dcc7c` |
| `videos/bottle-storm-recreation-20261106/outputs/checks/fish/cache-cleanup.json` | `a950746c9834c348` |
| `videos/bottle-storm-recreation-20261106/outputs/checks/fish/concurrency-decision.json` | `ef1d57a1f8f8f092` |
| `videos/bottle-storm-recreation-20261106/outputs/checks/fish/motion-audit.json` | `fce16399de3e8f5a` |
| `videos/bottle-storm-recreation-20261106/outputs/checks/fish/reference-timestamps.json` | `c0a77fd6d82d1755` |
| `videos/bottle-storm-recreation-20261106/outputs/checks/fish/subframe-check.json` | `f6c5b583563cfe70` |
| `videos/bottle-storm-recreation-20261106/scripts/animate_bunny.py` | `a7d548616966e917` |
| `videos/bottle-storm-recreation-20261106/scripts/audit_bunny.py` | `eb36257a0b692f16` |
| `videos/bottle-storm-recreation-20261106/scripts/audit_fish.py` | `e6c5aaa55bd0d75c` |
| `videos/bottle-storm-recreation-20261106/scripts/audit_scene.py` | `45647e0dde81145c` |
| `videos/bottle-storm-recreation-20261106/scripts/build_scene.py` | `67d9f3702b581883` |
| `videos/bottle-storm-recreation-20261106/scripts/bunny_motion.py` | `da0112e7ace9f39e` |
| `videos/bottle-storm-recreation-20261106/scripts/check.mjs` | `d263e85aab60cfae` |
| `videos/bottle-storm-recreation-20261106/scripts/compare_fish.py` | `d518393171993411` |
| `videos/bottle-storm-recreation-20261106/scripts/dump_scene_inventory.py` | `8943893b893464f4` |
| `videos/bottle-storm-recreation-20261106/scripts/finish_render.py` | `33da5fcfd8696f4c` |
| `videos/bottle-storm-recreation-20261106/scripts/fish_motion.py` | `e17bb61ace6c7ef7` |
| `videos/bottle-storm-recreation-20261106/scripts/inspect_fish_cycles.py` | `ecca1c08ceb9ccec` |
| `videos/bottle-storm-recreation-20261106/scripts/package_project.py` | `7fb66d245550d562` |
| `videos/bottle-storm-recreation-20261106/scripts/qa_frames.py` | `86809344e9d0d9eb` |
| `videos/bottle-storm-recreation-20261106/scripts/rebuild_scene.py` | `326bfa2ed3c7bfa0` |
| `videos/bottle-storm-recreation-20261106/scripts/render_scene.py` | `fecdf72b09cc52e4` |
| `videos/bottle-storm-recreation-20261106/scripts/run.mjs` | `b007b909475814d7` |
| `videos/bottle-storm-recreation-20261106/scripts/synthesize_audio.py` | `2e41a908215ed852` |
| `videos/bottle-storm-recreation-20261106/scripts/upgrade_fish.py` | `271310e3f98e5719` |
| `videos/bottle-storm-recreation-20261106/scripts/verify_archive.py` | `094560e80ef06b3e` |
| `videos/bottle-storm-recreation-20261106/src/Composition.tsx` | `c5c229f46ae60e83` |
| `videos/bottle-storm-recreation-20261106/src/Root.tsx` | `7989669de8801ac9` |
| `videos/bottle-storm-recreation-20261106/src/index.ts` | `957e481dd33a29b4` |
