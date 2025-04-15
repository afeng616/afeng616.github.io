# 论文那些事儿

<!--more-->


## 基础知识 Base  

Softmax和Sigmoid，详解[^1] [^2]

{{< admonition tip "小结" false >}}
都可用于多分类问题中，Sigmoid函数常用于有多个正确答案的分类问题，Softmax函数常用于有一个正确答案的分类问题。 
{{< /admonition >}}


tanh，详解[^3]

torch.gather函数详解[^15]，见汤胤评论


## 深度学习 Deep Learning  

LSTM，详解[^4] [^5]
{{< admonition tip "小结" false >}}
LSTM通过门控状态来控制传输状态，忘记+选择性记忆=输出。
{{< /admonition >}}

GRU，详解[^6]，论文[^7]
{{< admonition tip "小结" false >}}
GRU效果与LSTM相近，减少了计算量。
{{< /admonition >}}

Attention，详解[^8]，视频[^9] [^11]  
Transformer，详解[^10]  

## 强化学习 Reinforcement Learning

SARSA，详解[^12]

Q-Learning，详解[^13]
{{< admonition tip "小结" false >}}
{{< raw >}}
\[ 
    \forall s, a: Q^{*}(s, a)=\sum_{s^{\prime}} P_{s a}^{s^{\prime}}\left(R_{s a}^{s^{\prime}}+\gamma \max _{a} Q^{*}\left(s^{\prime}, a\right)\right) \\
    Q_{t+1}\left(s_{t}, a_{t}\right)=Q_{t}\left(s_{t}, a_{t}\right)+\alpha_{t}\left(s_{t}, a_{t}\right)\left(r_{t}+\gamma \max _{a} Q_{t}\left(s_{t+1}, a\right)-Q_{t}\left(s_{t}, a_{t}\right)\right)
\]
{{< /raw >}}
{{< /admonition >}}

DQN，详解[^14]

## 参考资料 Reference

[^1]: https://www.jianshu.com/p/037bf733713f "简书: 分类问题中Sigmoid与Softmax区别"
[^2]: https://devpress.csdn.net/xian/64a6246db1e197348be16c6a.html "论坛: 三分钟认知Softmax和Sigmoid的详细区别"
[^3]: https://www.jianshu.com/p/7409c8f1cdca "简书: 神经网络中的激活函数-tanh"
[^4]: https://zhuanlan.zhihu.com/p/32085405 "知乎: 人人都能看懂的LSTM"
[^5]: https://zhuanlan.zhihu.com/p/42717426 "知乎: 详解LSTM"
[^6]: https://zhuanlan.zhihu.com/p/32481747 "知乎: 人人都能看懂的GRU"
[^7]: https://arxiv.org/pdf/1412.3555.pdf "论文: Empirical Evaluation of Gated Recurrent Neural Networks on Sequence Modeling"
[^8]: https://zhuanlan.zhihu.com/p/46313756 "知乎: Attention机制简单总结"
[^9]: https://www.bilibili.com/video/BV1q3411U7Hi "Bili: Attention、Transformer公式推导和矩阵变化"
[^10]: https://zhuanlan.zhihu.com/p/166608727 "知乎: 举个例子讲下transformer的输入输出细节及其他"
[^11]: https://zhuanlan.zhihu.com/p/46313756 "知乎: Attention机制简单总结"
[^12]: ./RL/common_alg/SARSA.md "文档: SARSA"
[^13]: ./RL/common_alg/Q-Learning.md "文档: Q-Learning"
[^14]: https://paddlepedia.readthedocs.io/en/latest/tutorials/reinforcement_learning/DQN.html "文档: DQN"
[^15]: https://zhuanlan.zhihu.com/p/352877584 "知乎: 图解PyTorch中的torch.gather函数"



