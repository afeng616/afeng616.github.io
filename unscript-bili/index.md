# B站取关脚本分析

<!--more-->

## 需求分析
由于关注UP数量过多不便管理，可把我这个强迫症给急坏了。不如直接取关所有UP，一切从零开始，从头再来，至于还能不能遇见咱们各凭本事。

***V1.0 就目前来说，该脚本对路人不友好，使用时需要一定的计算机知识***

## 技术分析
1. 获取关注列表
    通过GET请求`https://api.bilibili.com/x/relation/followings?vmid=xxxxxx`
    header携带数据`referer:https://space.bilibili.com/xxxxxx/fans/follow`
2. 取关操作
    携带数据`fid=yyyyyy&act=2&csrf=zzzzzz`
    发送POST请求`https://api.bilibili.com/x/relation/modify`

## 代码分析
经过抓包分析API接口后得知，取关一个UP只需要进行最多两次HTTP请求，可以通过爬虫进行模拟操作。通过python的`requests`库既可胜任这个工作，使用简单的`requests.get()`、`requests.post()`就行。
- 在代码基本逻辑理清后，又对B站相关API进行了请求测试，发现完全可行。又考虑到使用的便捷性，决定将变量参数写入`config.ini`文件中。
    ```ini
    [config]
    # 使用者的B站uid
    uid = xxxx
    # 已登录B站的浏览器cookie
    cookie = yyyy
    # 跨域请求标识
    csrf = zzzz
    ```
- 脚本进行取关操作的前提是知晓该UP的uid，故需要先获取到本人的关注列表数据。
    ```python
    def get_relation():
        """
        获取b站关注列表(每次获取50个)
        https://api.bilibili.com/x/relation/followings?vmid='UID'
        :return: 关注up列表（50个）
        """
        return requests.get(f'https://api.bilibili.com/'
                            f'x/relation/followings?vmid='
                            f'{config.get("config", "uid")}').json()['data']['list']
    ```
    ```python
    def unsubscript(up_mid: str):
    """
    取关该up
    https://api.bilibili.com/x/relation/modify
    携带数据
    :return: 请求状态
    """
    header = {
        'cookie': config.get('config', 'cookie')
    }

    data = {
        'fid': up_mid,
        'act': 2,
        'csrf': config.get('config', 'csrf')
    }
    return requests.post(
        'https://api.bilibili.com/x/relation/modify',
        data=data,
        headers=header).json()
    ```

