# 计算机网络参考模型

<!--more-->

<!--kg-card-begin: markdown-->

## OSI和TCP/IP标准
<table style=\"text-align: center;\">    <tr>        <th>OSI</th>        <th>TCP/IP</th>        <th>TCP/IP协议栈</th>    </tr>    <tr>        <td>应用层</td>        <td rowspan=\"3\" style=\"vertical-align: middle;\">应用层</td>        <td rowspan=\"3\" style=\"vertical-align: middle;\">HTTP、FTP、DNS</td>    </tr>    <tr>        <td>表示层</td>    </tr>    <tr>        <td>会话层</td>    </tr>    <tr>        <td>传输层</td>        <td>传输层</td>        <td>TCP、UDP</td>    </tr>    <tr>        <td>网络层</td>        <td>网际层</td>        <td>IP</td>    </tr>    <tr>        <td>数据链路层</td>        <td rowspan=\"2\" style=\"vertical-align: middle;\">网络接口层</td>        <td rowspan=\"2\" style=\"vertical-align: middle;\">Ethernet、ATM</td>    </tr>    <tr>        <td>物理层</td>    </tr></table><p><strong>OSI、TCP/IP标准中传输层和网络（际）层的区别</strong></p><table><thead><tr><th style=\"text-align:center\"></th><th style=\"text-align:center\">OSI</th><th style=\"text-align:center\">TCP/IP</th></tr></thead><tbody><tr><td style=\"text-align:center\">传输层</td><td style=\"text-align:center\">面向连接</td><td style=\"text-align:center\">无连接、面向连接</td></tr><tr><td style=\"text-align:center\">网络（际）层</td><td style=\"text-align:center\">无连接、面向连接</td><td style=\"text-align:center\">无连接</td></tr></tbody></table>

## 综合标准
<p>经过综合考量当前认定的标准是五层结构</p><table><thead><tr><th style=\"text-align:center\">分层</th><th style=\"text-align:center\">单位</th><th style=\"text-align:center\">协议</th></tr></thead><tbody><tr><td style=\"text-align:center\">应用层</td><td style=\"text-align:center\">报文</td><td style=\"text-align:center\">FTP、SMTP、HTTP</td></tr><tr><td style=\"text-align:center\">传输层</td><td style=\"text-align:center\">报文段</td><td style=\"text-align:center\">TCP、UDP</td></tr><tr><td style=\"text-align:center\">网络层</td><td style=\"text-align:center\">数据报</td><td style=\"text-align:center\">IP、ICMP、OSPF</td></tr><tr><td style=\"text-align:center\">数据链路层</td><td style=\"text-align:center\">帧</td><td style=\"text-align:center\">PPP、Ethernet</td></tr><tr><td style=\"text-align:center\">物理层</td><td style=\"text-align:center\">比特</td><td style=\"text-align:center\"></td></tr></tbody></table><!--kg-card-end: markdown-->



