1extends CharacterBody2D

3#玩家移动速度，单位是像素/秒。

4 @export var move\_speed: float = 120.0

6func \_physics\_process(\_delta: float) -> void:

7 #读取四个方向输入，并得到标准化后的八向输入向量

8var move\_input := Input.get\_vector("move\_left", "move\_right", "move\_up", "move\_down")

10” # CharacterBody2D 通过 velocity 配合move\_and\_slide() 完成移动

11 velocity = move\_input \* move\_speed

12 S1 move\_and\_slide()