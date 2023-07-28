# Pytorch测试GPU是否可用

<!--more-->

pytorch分为cpu和gpu两个版本，若需要验证gpu版本是否安装成功可以通过以下命令

```python
import torch

# 返回当前设备索引
torch.cuda.current_device()

# 返回gpu数量
torch.cuda.device_count()

# 返回gpu名称，索引从0开始
torch.cuda.get_device_name(0)

# cuda是否可用
torch.cuda.is_available()
```



