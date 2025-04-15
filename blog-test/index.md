# Blog Test

<!--more-->

## 事件发展
{{< mermaid >}}
graph TD;
    A[起因] -->|经过| B(事件发展)
    B --> C{转折}
    C -->|走向1| D[结果1]
    C -->|走向2| E[结果2]
{{< /mermaid >}}

{{< mermaid >}}
gantt
dateFormat  YYYY-MM-DD
title 计划安排

section 计划A
Completed task            :done,    des1, 2023-01-06,2023-01-08
Active task               :active,  des2, 2023-01-09, 3d
Future task               :         des3, after des2, 5d
Future task2              :         des4, after des3, 5d
{{< /mermaid >}}


## content
This is a test blog.  
This is my first blog at GitHub Page website!  

This is a test blog.  
This is my first blog at GitHub Page website!  

This is a test blog.  
This is my first blog at GitHub Page website!  

This is a test blog.  
This is my first blog at GitHub Page website!  

This is a test blog.  
This is my first blog at GitHub Page website!  

## 数学公式

```latex
$$
\begin{aligned}
Q_(n+1)&=\frac{1}{n}\sum_{i=1}^nR_i \\\\
&=Q_n+\frac{1}{n}(R_n-Q_n) 
\end{aligned}
$$
```

$$
\begin{aligned}
Q_(n+1)&=\frac{1}{n}\sum_{i=1}^nR_i \\\\
&=Q_n+\frac{1}{n}(R_n-Q_n) 
\end{aligned}
$$

## 参考
GitHub: [Github Pages + Hugo 搭建个人博客](https://github.com/zz2summer/blog-hugo-theme-LoveIt)  
少数派: [Hugo + GitHub Action，搭建你的博客自动发布系统](https://sspai.com/post/73512)  



