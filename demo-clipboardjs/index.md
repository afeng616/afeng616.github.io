# Clipboard.js使用实例

<!--more-->

## 说明
通过`Clipboard.js`可以完成对指定元素原格式内容的复制功能。
使用时需要注意以下两点：
1. 新建`Clipboard`对象时，指定触发对象
2. 触发对象最好使用`<button>`元素

## 代码说明
### 动态添加复制按钮
在网页指定位置添加触发器，即按钮（使用了`JQuery`，也可通过`JavaScript`原生代码编写）
```javascript
// 添加复制按钮
$('.toolbar').append('<button class="button copy-button"><span>Copy</span></button>');
```

### 动态指定复制目标
可以在不修改`html`的情况下，通过编辑`js`代码完成创建`Clipboard`对象，并添加触发器
```javascript
// 创建Clipboard对象并完成初始化
var copyCode = new Clipboard('.copy-button', {
    target: function (trigger) {
        return trigger.parentElement.previousElementSibling;
    }
});
```

### 编写回调函数
```javascript
// Clipboard对象的回调函数（复制成功，复制失败）
copyCode.on('success', function (event) {
    event.clearSelection();
    event.trigger.textContent = 'Copied';
    window.setTimeout(function () {
        event.trigger.textContent = 'Copy';
    }, 2000);
});
copyCode.on('error', function (event) {
    event.trigger.textContent = 'Error';
    window.setTimeout(function () {
        event.trigger.textContent = 'Copy';
    }, 2000);
});
```

## 完整代码
```javascript
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Clipboard.js使用实例</title>

    <style>
        .button {
            display: inline-block;
            height: auto;
            border-radius: 5px;
            padding: 3px 8px;
            border: 2px solid rgb(8, 230, 238);
        }

        .container {
            display: inline-block;
            height: auto;
            padding: 10px 15px;
            border: 3px dotted greenyellow;
            border-radius: 5px;
        }
    </style>

    <script src="https://cdnjs.cloudflare.com/ajax/libs/clipboard.js/1.4.0/clipboard.min.js"></script>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/jquery/3.4.1/jquery.min.js"></script>

</head>

<body>
    <div style="text-align: center;">
        <div class="container">
            <code>Clipboard</code>
            <div class="toolbar">
            </div>
        </div>

        <div class="container">
            <code>使用</code>
            <div class="toolbar">
            </div>
        </div>

        <div class="container">
            <code>实例</code>
            <div class="toolbar">
            </div>
        </div>
    </div>

    <script>
        $('.toolbar').append('<button class="button copy-button"><span>Copy</span></button>');

        var copyCode = new Clipboard('.copy-button', {
            target: function (trigger) {
                return trigger.parentElement.previousElementSibling;
            }
        });
        copyCode.on('success', function (event) {
            event.clearSelection();
            event.trigger.textContent = 'Copied';
            window.setTimeout(function () {
                event.trigger.textContent = 'Copy';
            }, 2000);
        });
        copyCode.on('error', function (event) {
            event.trigger.textContent = 'Error';
            window.setTimeout(function () {
                event.trigger.textContent = 'Copy';
            }, 2000);
        });
    </script>
</body>

</html>
```




