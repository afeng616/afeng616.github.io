# 数据结构学习

<!--more-->

在[IO Wiki](https://oi-wiki.org/ds/stack/)上看到一个用数组实现栈的例子，真是被妙到了，值得记录一下。
```c++
int stack[10] = {0};

// 入栈
stack[++*stack] = 18;
stack[++*stack] = 22;

cout << "test" << endl;
for (auto n: stack)
    cout << n << " ";

// 出栈
int num = stack[*stack--];
// 清空
*stack = 0;
```
使用数组的首元素作为栈顶指针，这样就可以用`*stack`来表示栈顶元素的下标。栈顶元素的值为`0`，表示栈为空。当然，这里的清空是假性清空并未真正删除存储值，但并不影响下次入栈操作；还需要注意的是，应当加入适当的边界检查，避免栈溢出，如超出数组长度、首元素减至小于0。

