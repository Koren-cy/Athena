## keyframe_001_9.3s (9.3s)

八向射击游戏《冥日芳粥:肿么滴》

---

## keyframe_002_11.5s (11.5s)

我们neta了在《星露谷物语》中的

---

## keyframe_003_22.5s (22.5s)

在我们的教程系列中

---

## keyframe_004_30.9s (30.9s)

## 010 of
把整个开发过程里真正需要掌握的内容

---

## keyframe_005_38.7s (38.7s)

从零开始一步一步把这款属于你的《肿么滴》真正做出来

---

## keyframe_006_44.9s (44.9s)

也最容易真正学会的第一批开发知识串联起来

---

## keyframe_007_48.6s (48.6s)

#玩家移动速度，单位是像素/秒。
@export var move\_speed: float = 120.0
func \_physics\_process(\_delta: float) -> void:
#读取四个方向输入，并得到标准化后的八向输入向量
CharacterB
Motion Mode
Up Direction
Slide on Ceiling
Floor
Moving Platform
Collision
CollisionOb
Disable ModeRer
Collision
Input
Node2
Transform
CanvasI
Visibility
Ordering
Texture
Material
O Node
Process
Physics Interpolatior
Auto Translate
Editor Description
Script
+添加元数

---

## keyframe_008_52.2s (52.2s)

比如怎样用更规范的场景结构来组织玩家敌人子弹和道具

---

## keyframe_009_55.7s (55.7s)

(no text detected)

---

## keyframe_010_59.9s (59.9s)

(no text detected)

---

## keyframe_011_66.8s (66.8s)

## 碰撞管理器
不只是告诉你这里点哪里那里填什么

---

## keyframe_012_73.0s (73.0s)

##
资源组织这些真正会影响你日后开发上限的内容讲清楚

---

## keyframe_013_80.8s (80.8s)

1 extends CharacterBody2D
3#玩家移动速度，单位是像素/秒。
4 @export var move\_speed: float = 120.0
6func \_physics\_process(\_delta: float) -> void:
#读取四个方向输入，并得到标准化后的八向输入向量
8 var move\_input := Input.get\_vector("move\_left", "move\_right", "move\_up", "move\_down")
10 # CharacterBody2D 通过 velocity 配合 move\_and\_slide() 完成移动
11 velocity = move\_input \* move\_speed
12 move\_and\_slide()
往往只会做教程里手把手演示过的内容

---

## keyframe_014_83.0s (83.0s)

(no text detected)

---

## keyframe_015_90.1s (90.1s)

所以我的想法是

---

## keyframe_016_94.7s (94.7s)

1extends CharacterBody2D
3#玩家移动速度，单位是像素/秒。
@export var move\_speed: float = 120.0
func \_physics\_process(\_delta: float) -> void:
#读取四个方向输入，并得到标准化后的八向输入向量
81 var move\_input := Input.get\_vector("move\_left", "move\_right", "move\_up", "move\_down")
10#CharacterBody2D 通过 velocity 配合 move\_and\_slide() 完成移动
11 velocity = move\_input \* move\_speed
12move\_and\_slide()
筛选属性
免 CharacterBody2
Motion Mode
Grounde
历史
0.0
Up Direction
y -1.0
Slide on Ceiling
Moving Platform
启用
Collision
CollisionObject2
Floor
Disable Mode
Remove
Collision
Input
O Node2D
Transform
/ CanvasItem
Visibility
Ordering
Texture
Material
O Node
Process
我会结合我的从业经验
Physics Interpolation
Auto Translate
Editor Description
Script
playe
\+ 添加元数据

---

## keyframe_017_100.0s (100.0s)

1extends CharacterBody2D
3#玩家移动速度，单位是像素/秒。
4 @export var move\_speed: float = 120.0
6func \_physics\_process(\_delta: float) -> void:
7 #读取四个方向输入，并得到标准化后的八向输入向量
8var move\_input := Input.get\_vector("move\_left", "move\_right", "move\_up", "move\_down")
10” # CharacterBody2D 通过 velocity 配合move\_and\_slide() 完成移动
11 velocity = move\_input \* move\_speed
12 S1 move\_and\_slide()

---

## keyframe_018_104.7s (104.7s)

## 日

---

## keyframe_019_110.8s (110.8s)

## 举一反三
我会带大家举一反三拓展相关的玩法思路

---

## keyframe_020_122.0s (122.0s)

从零开始玩家移
《冥日芳粥：肿么滴》
Vol.002
## 从零开始 Godot玩家射击系统
《冥日芳粥：肿么滴》
Vol.004
我会把我账号接广卖课收入的部分作为“活动基金”

---

## keyframe_021_129.0s (129.0s)

##
##
也能创作出独属于自己的作品

---

## keyframe_022_140.0s (140.0s)

为国内Godot社区的建设和成长

---

## keyframe_023_145.2s (145.2s)

免费教程永久公开永久免费

---

## keyframe_024_147.4s (147.4s)

##

---

## keyframe_025_150.4s (150.4s)

共42课时
## 课堂
【上新5折】联机游戏开发完全指南
【限时特惠】通关设计模式：游戏开发架构宝典
共54课时
【限时特惠]共36课时
Voidmatrix
投稿
## 【上新5折】联机游戏开发完全指南
共42课时3963
【限时特惠】通关设计模式：游戏开发架
构宝典
共54课时
10.5万
【限时特惠】从零开始的C++双人塔防游
戏开发
共36课时
24.5万
## Voidmatrix DiliDili
关注数 粉丝数 获赞数 播放数2298 12.7万 28.9万 738.9万
我个人主页课堂页面中的付费课程内容

---

## keyframe_026_154.0s (154.0s)

·瓦片地图编辑器
·角色属性编辑器
·关卡波次编辑器
· PIE 调试控制台
·一键打包发布功能
## 项目体量与结构总览
MiniTdGame
引用
外部依都项
头文件
axe bulleth
bulleth
bullet\_type.h
tower
archer\_tower.h
axeman tower.h
gunnec\_towerh
towerh
tower\_typeh
panel
panelh
place\_paneth
upgrade panelh
banner.h
status bar.h
animationh
coin\_prap.h
facing.h
maph
resource.h
routeh
tileh
timerh
vector2h
wave.h
即原文件
cISON
main.cpp
资原文件
iconico
MiniTdGame.rc
TieAmnotator
引用
外部休解项
头文件
resource.h
源文件
main.cpp
民原文件
iconico
TleAnnotator.ec
4000余行核心代码
工程视野：仅聚焦于单个案例项目 从独游到大型网游技术思路从同步和优化到调试与更新，提供贯穿项目生命周期的解决方案
## 四大进阶篇章
40+课时|构建从原理到工业环境的完整路径
1基础篇：联机与核心通信模型直击关键概念完成多人聊天室，快速建立“联机”体感与信心
2核心篇：服务端架构与世界同步技术深入游戏服务端核心架构，构建稳固的多人游戏逻辑基石
3精进篇：优化与专项技术直面延迟抖动，掌握状态/帧同步策略，打造流畅稳定的线上体验
4升华篇：引擎源码解读与工业级架构
深入浅出拆解Godot引擎联机模块实现，获得引擎维度的架构理解工业实践篇：
解读大型网游服务器集群分布式系统设计，建立高屋建瓴的架构师视角
## 附赠课程资料
7个专题配套示例工程 涵盖聊天室、Lua热更等核心概念清晰易懂的代码
齐全图文学习资料：Godot源代码与引擎网络模块技术架构高清图
一站式集成资源包：所有三方库工具链等资料，告别繁琐的资料下载
## 适合人群
体系化学习路径：仅介绍设计模式 从基础原则到模式架构从萌新小白到实战经验，再到大厂面试高频考察点一网打尽
跨领域开发能力：仅关注某语言/领域游戏与通用软件开发并行角色、战斗上层逻辑与场景资源、架构底层性能优化两手抓
## 五大进阶模块
50+课时|贯穿游戏开发全场景
筑基篇：面向对象六大原则+架构思维重塑创世篇：创建型模式×5 更灵活的对象创建机制塑形篇：结构型模式×7 更丰富的对象组织架构赋能篇：行为型模式×11更高效的对象职责沟通5 升华篇：面试专题+ECS/对象池等工业级架构解析
·23个可交互设计模式Demo含6000+行源码的完整项目工程
·自研迷你引擎框架
逐个击破JSON/XML加载解析、场景树编辑、Lua脚本接入等进阶功能
10+简历高光技术点指路代理延迟加载、ECS数据驱动、对象池与数据局部性优化等高热度专题
#舞引学是独立于我们教程项自之外的更进阶的游戏开发专题课程 Java/C++/C#开发者
## 适合人群
新人破局者：被“如何设计代码架构”困扰的入门开发者独立游戏人：想更好地使用引擎工具实现深度定制的创作者

---

## keyframe_027_157.9s (157.9s)

animationh coin\_prop.h facing.h map.h resource.h routeh tile.h timer.h vector2h wave.h
manager bullet\_manager.h coin\_manager.h èconfig manager.h debug\_manager.h enemy\_manager.h game\_manager.h home manager.h àmanager.h player\_manager.h 0 resources manager. tower\_manager.h wave manager.h
4000余行核心代码讲解成熟游戏项目代码组织结构实践从引擎层到玩法核心全部内容
## 适合人群
·C++编程初学者 希望有体系完备的教学项目手把手跟随实践
·游戏开发爱好者
希望了解游戏引擎运作机理和更底层的程序设计
· 相关方向求职者
希望拥有足够竞争力的项目提升自身简历含金量
## 一站式集成资源包：所有三方库工具链等资料，告别繁琐的资料下载
## 适合人群
✓ 求职冲击者瞄准游戏开发岗位，急需系统化联机知识的应届生
V独立/小团队开发者希望实现多人联机玩法，寻求有限资源下高效方案的创作者
后端转型者
具备一定开发经验希望转向游戏领域的后端技术工程师
技术深研爱好者不满足黑盒调用，渴望深入理解引擎底层实现的技术极客
## 为什么选择这套课程？
真正源自长线运营网游的实战经验课程内容植根于大型游戏公司联机项目的开发经验与技术复盘，绝非纸上谈兵历史课程口碑见证品质延续：《从零开始的游戏开发》及《游戏架构宝典》等课程全网千万播放，长期霸榜
真正用心打磨的浓缩课程：课程规划与制作历时半年有余，力求在最短时间内交付最密集、最准确的知识
## 注意提醒
本课程信息密度高、针对性强，不适合零编程/游戏开发基础的同学。新手同学建议先通过《从零开始的游戏开发》《双人塔防游戏开发》《游戏架构宝典》等系列系列夯实基础，再参与到本套课程的学习中。
## Voidmatrix Dilibili适合人群
新人破局者：被“如何设计代码架构”困扰的入门开发者独立游戏人：想更好地使用引擎工具实现深度定制的创作者引擎爱好者：挑战自研引擎专注体系结构设计的技术极客求职冲刺党：需要设计模式+游戏优化双重面试筹码的应届生软件工程师：需要构建高扩展性系统的Java/C++/C#开发者
## 为什么选择这套课程？
《从零开始游戏开发》系列作者：
仅B站播放近400万，被学员称为“最透彻易懂的C++游戏编程教程”
历史课程口碑认证：项目实战课程《代号：村庄保卫战！》双人塔防游戏开发广受好评，长期霸榜
开发经验加持深化内容：
课程中融入个人理解与经验传输，从基础到实战，重塑触手可及的架构认知
对于大部分需要跟随本项目学习的同学是暂时用不上的

---

## keyframe_028_162.3s (162.3s)

##

---
