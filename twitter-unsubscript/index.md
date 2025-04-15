# Twitter取关脚本

<!--more-->

<!--kg-card-begin: markdown--><p>由于Twitter页面显示情况较特殊，暂时没有找到更高效的批量取关方法。</p>

## 代码
<pre><code class=\"language-javascript\">let btns = document.querySelectorAll(&quot;[data-testid]&quot;)<br>    let followBtns = Array.from(btns).filter(btn =&gt; {<br>        return btn.getAttribute('data-testid').includes('unfollow')<br>    })<br><br>    for (let i = 1; i &lt;= followBtns.length; i++) {<br>        followBtns[i - 1].click();<br>        let _span = document.getElementsByTagName('span');<br>        for (var j = 0; j &lt; _span.length; j++) {<br>            if (_span[j].textContent == &quot;取消关注&quot;) {<br>                _span[j].click();<br>                console.log(&quot;取关&quot;);<br>            }<br>        }<br>    }<br></code></pre>

## 使用说明
<p>使用网页版Twitter进入<code>个人资料-&gt;正在关注</code>页面，网址例如<code>https://twitter.com/xxxxx/following</code>。</p><p><code>F12</code>调出开发者工具，在<code>console</code>栏中输入上述代码。每次运行能取关几十人，多运行几次就行。</p><!--kg-card-end: markdown-->


