# OpenCV绘制圆弧的坑

<!--more-->

使用函数`cv2.ellipse`可以进行圆弧绘制，该函数传入的主要参数为：弧中心、弧半径、弧起始角度、弧终止角度。  
但这个函数在使用时着实有些反人类！！！

首先在cv中取向下为y轴正方向，因此它的角度表示与笛卡尔系是相反的；  
其次绘制时并非是从angle1->angle2按规定方向绘制，而是在传入的两个angle中从小angle->大angle按顺时针绘制的，即angle传入顺序先后并不代表着起始和终止。

在用的时候真是无语住了，直到我单独测试才发现这个问题！！！

![OpenCV画弧](/images/blog/20240625-cv-arc-problem.png)

```
numpy==1.23.2
opencv_python==4.6.0.66
```

```python
import cv2
import numpy as np

if __name__ == '__main__':
    img = np.zeros((100, 100, 3), dtype=np.uint8)

    cv2.ellipse(img, (10, 10), (10,10), 0, 45, -135, (0, 255, 255), -1)
    cv2.ellipse(img, (40, 10), (10,10), 0, 135, 0,  (255, 255, 255), -1)
    cv2.ellipse(img, (80, 10), (10,10), 0, 0, 135,  (255, 255, 255), -1)
    cv2.imshow('image', img)
    
    cv2.waitKey(0)
```

这里刻意采用图形填充来显示弧的角度范围。  
弧1可以看出在cv世界中，向右为0°，顺时针转动为正。  
弧2、弧3的函数使用则可以看出angle传入的顺序对圆弧绘制没有影响。

