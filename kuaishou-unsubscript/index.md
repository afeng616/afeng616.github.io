# 快手取关脚本

<!--more-->

<!--kg-card-begin: markdown--><p>简单的元素筛选，模拟点击即可！</p>
## 代码
<pre><code class=\"language-javascript\">let num = 0<br>for (let tmp of document.getElementsByClassName('container')){<br>    tmp.children[2].children[0].click()<br>    console.log('取关', ++num)<br>}<br></code></pre>

## 使用方法
<p>进入快手网页版<code>个人页面-&gt;关注一栏</code>，如<code>https://www.kuaishou.com/profile/kuaishou_id</code>，多次按下<kbd>Ctrl</kbd><kbd>End</kbd>组合键使页面滚动到最下端（方便一次运行完毕），<kbd>F12</kbd>打开开发者工具，输入上述命令运行即可。</p><!--kg-card-end: markdown-->


