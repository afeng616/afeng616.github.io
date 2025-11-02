# 远程开机的实现分析


<!--more-->

<!--kg-card-begin: markdown--><p>这次开发的需求很简单，就是想使用移动端通过互联网实现对主机的开机、关机、休眠、睡眠以及重启的控制。</p><p>明确了需求后就开始构思解决方案，在此过程中，通过互联网了解到市场上存在能完成该任务的产品。于是我就开始对其中的<code>HiPC</code><sup class=footnote-ref><a href=#fn1 id=fnref1>[1]</a></sup>进行了了解，发现该产品由软件端（小程序及pc端应用）和硬件端（类似二次开发的网卡）组成，再结合搜索到的资料，我大致了解整个过程的实现思路。</p>

## HiPC的实现思路
<p><em><strong>该实现思路为本人摸索得出，仅供参考，侵权删！</strong></em></p><p>下载并使用<code>HiPC</code>的PC端应用程序后不难发现，仅使用该程序就可以配合小程序<strong>经过互联网</strong>完成对PC的远程<strong>关机、休眠、睡眠以及重启</strong>控制，而想使用<strong>开机</strong>功能（无论通过局域网还是广域网）则需要购买其硬件产品。将焦点放在PC端程序还发现，若想完成上述操作，该程序还必须在PC上正常运行。</p>

### 除开机外其他控制的实现
<p>经过简单的分析发现，此应用与云服务器建立了<code>WebSocket</code>连接，使得该应用能够将PC的实时状态上传至服务器，同时使小程序能及时更新PC数据，并完成对PC的操作控制。</p><ol><li>HiPC.exe后台监控PC状态，并通过<code>WebSocket</code>传输数据给服务器</li><li>小程序发送指令至服务器，服务器通过<code>WebSocket</code>将指令转发给HiPC.exe使其执行相应指令</li></ol><p>
![HiPC实现逻辑][hipc-process]
</p><p>通过以上的方式使得用户能脱离局域网完成对PC除开机以外的其他控制。</p>

### 开机功能的实现
<p>开机功能实质是唤醒PC，在PC进入关机、休眠、睡眠状态下，系统中原有的运行程序将停止正常工作。因此，无法再通过上述的逻辑实现远程唤醒操作。</p><p>这时候再将关注点放在硬件设备上，即<code>HiPC</code>售卖的PCI-E设备。经过简单分析，初步判断该设备为经过特定编程开发的网卡，实质通过WoL<sup class=footnote-ref><a href=#fn2 id=fnref2>[2]</a></sup>完成对主机的唤醒。使用WoL需要有两个前提：1.主板支持；2.网卡支持。实现简单的局域网唤醒<sup class=footnote-ref><a href=#fn3 id=fnref3>[3]</a></sup>满足以上两个条件经过简单设置就能实现，简单的原理就是处在同一局域网内的某设备向目标PC的网卡地址发送UDP数据包，网卡接收到指定格式数据后通过主板唤醒主机。</p><p>

![本地唤醒][local-startup]
</p><p>然而想完成远程唤醒，还需要满足一个较苛刻的条件，即实现外网向内网指定网卡发包。绝大部分家庭使用的网络IP都是动态的，大部分家庭网络IP更是经过多层路由转发，因此其网关WAN IP都有可能是内网IP，而内网对于外网而言是透明的。由此，实现精准发包就需要下一点功夫了，这也是<code>HiPC</code>配合硬件端完成的工作。</p><p>在已经实现局域网内开机功能后，经过了几天的探索，最终确立了能够实现远程控制的方案。</p>

## 方案
<ol><li><p>局域网实现对PC的控制</p><p>需要：主板支持；网卡支持；</p><ul><li>局域网开机可以根据WoL网络唤醒实现</li><li>其他操作完全可以下载<code>HiPC.exe</code>完成</li></ul></li><li><p>广域网实现对PC的控制</p><p>需要：主板支持；网卡支持；路由器支持；服务器；公网IP；</p><ul><li>依照WoL原理，从服务器发送udp数据包，经过互联网传输到有公网IP的网关WAN口，通过端口映射进入内网，通过路由器将数据包转发给指定网卡</li></ul><p>

![远程唤醒][remote-startup]
</p><ul><li>其他操作依赖<code>HiPC.exe</code></li></ul></li></ol><p>对于除开机外的其他操作，虽然可以完全手写，但个人觉得完全没必要。</p>

## 远程开机实现细节
<p><em>该实现细节建立在方案1完成的前提下，依照【局域网使用WoL网络唤醒】可完成方案1实施</em></p><p>依照上述方案2实现通过互联网远程唤醒主机，需要考虑的问题如下：</p><p><em><strong>Q</strong></em>：为什么不使用花生壳做内网穿透？</p><p><em><strong>A</strong></em>：在进行方案尝试时，是存在使用花生壳的想法的，但是在具体实施时发现了问题：计算机需要<strong>保持花生壳客户端的运行才能使内网穿透正常</strong>，对于待唤醒的PC，这点自然无法满足。个人认为<strong>花生壳适合用于搭建服务器，使用户能通过互联网访问到本地服务器资源，与本需求相悖，同时也确实没有必要</strong>。在路由器上使用花生壳提供的DDNS服务时，总是会有莫名的错误导致无法达到预期效果，最终放弃了这个方案。</p><p><em><strong>Q</strong></em>：为什么要进行网关和路由器的端口映射？</p><p><em><strong>A</strong></em>：本需求只<strong>要求某PC网卡能接收到来自公网的数据包</strong>。拥有公网IP的WAN口可以直接收到来自互联网的数据包，这时对网关进行端口映射使指定外部端口映射到内网某IP的端口，使得来自公网的数据能够被内网的设备接收到。若目标PC与网关间存在其他路由器，则相应路由器中也应该做好端口映射，以保证目标设备能接收到转发的数据包。</p><p><em><strong>Q</strong></em>：即便申请到了公网IP，家庭用网的公网IP是动态的，如何确保发包目的地址为目标PC网卡？</p><p><em><strong>A</strong></em>：网络运营商虽然可以接受个人申请使用公网IP，但是非专线（固定公网IP）的用户使用的公网IP是动态的。由于同一网关下的所有连接设备都共用一个公网IP，所以一开始设想让目标PC定时发送公网IP给服务器供其更新。随后觉得这个想法不合理，如果公网IP在PC未启动时更新，奈何？所以想到使用移动端代替原设想中PC的功能。<strong>移动设备设置自动化任务，通过指定API获取最新的公网IP发送至服务器供其更新。</strong></p>

## 总结
<p>在网卡和主板支持的前提下，基本可以无压力的实现局域网内的开机控制。而仅使用<code>HiPC</code>开发的应用程序就能实现远程（通过互联网也可）控制PC关机、休眠、重启等操作。若在不购置开机卡的前提下想通过互联网完成开机控制，就多少需要花些心思了。</p>

## References
<hr class=footnotes-sep><section class=footnotes><ol class=footnotes-list><li id=fn1 class=footnote-item><p><a href=https://hipc.cn>HiPC</a> &quot;HiPC官网&quot; <a href=#fnref1 class=footnote-backref>↩︎</a></p></li><li id=fn2 class=footnote-item><p><a href=https://zhuanlan.zhihu.com/p/29100480>Wake on Lan原理</a> &quot;向指定网卡发送指定格式的UDP数据包&quot; <a href=#fnref2 class=footnote-backref>↩︎</a></p></li><li id=fn3 class=footnote-item><p><a href=https://sspai.com/post/67003>局域网使用WoL网络唤醒</a> &quot;局域网某设备向目标PC网卡发送WoL包&quot; <a href=#fnref3 class=footnote-backref>↩︎</a></p></li></ol></section><!--kg-card-end: markdown-->

[hipc-process]: /images/blog/20220125-hipc-process.png "HiPC实现逻辑"
[local-startup]: /images/blog/20220125-local-startup.png "本地唤醒"
[remote-startup]: /images/blog/20220125-remote-startup.png "远程唤醒"



