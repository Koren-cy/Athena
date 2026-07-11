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