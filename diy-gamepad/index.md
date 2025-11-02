# 将第三方手柄输出映射到鼠标上

<!--more-->

## 说明
将游戏手柄的摇杆映射为鼠标滚轮操作，
当前只是将向上和向下操作映射为鼠标滚轮的上下滚动。  

通过`pynput`库完全可以更大程度的映射手柄操作，
- 鼠标指针移动
- 鼠标点击
- 键盘快捷键
- ...  

更多`pynpug`库接口，可参看官方文档[^1]。

## 全部代码
```python
# @auth: afeng
# @desc: get gamepad input and output mouse cmd
# @date: 2023/05/21

import pygame
from pynput.mouse import Controller

pygame.init()
clock = pygame.time.Clock()
pygame.joystick.init()
mouse = Controller()
scroll_offset = .2

# gamepad
joystick = pygame.joystick.Joystick(0)
joystick.init()

name = joystick.get_name()
print(f"Joystick name: {name}".format(name))

while True:
    for event in pygame.event.get(): # User did something
        pass
    #     if event.type == pygame.QUIT: # If user clicked close
    #         done=True # Flag that we are done so we exit this loop

    axes = joystick.get_numaxes()
    for i in range( axes ):
        if i==0 or i==3:
            continue
        axis = joystick.get_axis( i )
        # Axis 0: -1 left, 1 right
        # Axis 1: -1 up, 1 down
        # Axis 2: -1 up, 1 down
        # Axis 3: -1 left, 1 right
        # mouse scroll dy: -1 down, 1 up
        mouse.scroll(0, -i*axis*scroll_offset)

    clock.tick(20)
```

[^1]: https://pynput.readthedocs.io/en/latest/ "`pynput`库官方文档"



