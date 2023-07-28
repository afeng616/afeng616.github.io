# 重装CentOS环境

<!--more-->

<!--kg-card-begin: markdown--><blockquote><p>没事千万别拿环境开玩笑！</p><p>​——沃兹基·硕德</p></blockquote><br><!--kg-card-end: markdown--><p><em><strong>轻易不要尝试</strong></em></p><p><em><strong>轻易不要尝试</strong></em></p><p><em><strong>轻易不要尝试</strong></em></p><p>由于之前使用的python版本过低，无法使用很多模块的新特性。然而在尝试更新python版本时遇到了很多坑，不仅把原python3更新为新版本后，原yum不能用了，而且pip也无法正常工作。且卸载python3老版本的时候不小心吧python2给卸载了，yum又需要python2。几番尝试之后无果，干脆全部重装算了！</p>
## 卸载原python、yum
<p>卸载现有python（所有版本及模块）。</p><pre><code class=\"language-shell\">rpm -qa|grep python|xargs rpm -ev --allmatches --nodeps ##强制删除已安装程序及其关联<br>whereis python |xargs rm -frv  ##删除所有残余文件 ##xargs，允许你对输出执行其他某些命令<br>whereis python ##验证删除，返回无结果<br></code></pre><p>卸载现有yum。</p><pre><code class=\"language-shell\">rpm -qa|grep yum|xargs rpm -ev --allmatches --nodeps<br>whereis yum |xargs rm -frv<br></code></pre>

## 安装python2

### 安装python2.7.15
<p>使用python.org下载太慢，这里使用淘宝镜像下载。</p><pre><code>https://registry.npmmirror.com/-/binary/python/2.7.15/Python-2.7.15.tgz<br></code></pre><pre><code class=\"language-shell\">cd /usr/local/src<br>wget https://registry.npmmirror.com/-/binary/python/2.7.15/Python-2.7.15.tgz<br>tar -zxvf Python-2.7.15.tgz<br>cd Python-2.7.15<br>./configure<br>make &amp;&amp; make install<br></code></pre><p>创建软链接，将python2.7.15版本命名为python2，将python留给python3版本使用。</p><pre><code class=\"language-shell\">ln /usr/local/bin/python /usr/local/bin/python2<br></code></pre><p>将涉及<code>/usr/bin/python</code>的设置更新为<code>/usr/bin/python2</code>，毕竟python2.7的软链接已经被改成了python2。</p>

### 安装pip
<p>查找网上的教程都是通过以下yum命令安装的，然而我却总是显示没有匹配包，我也是挺无奈的。</p><pre><code class=\"language-shell\">yum -y install python-pip<br></code></pre><p>干脆就直接使用源码安装算了，反正也不复杂。</p><pre><code class=\"language-shell\"># 1.下载python-pip包<br>wget https://pypi.python.org/packages/source/p/pip/pip-1.3.1.tar.gz --no-check-certificate<br># 2.安装pip之前需要先安装setuptools<br>wget https://pypi.python.org/packages/2.7/s/setuptools/setuptools-0.6c11-py2.7.egg --no-check-certificate<br>chmod +x setuptools-0.6c11-py2.7.egg<br>sh setuptools-0.6c11-py2.7.egg<br># 3.安装pip<br>chmod +x pip-1.3.1.tar.gz<br>tar xzvf pip-1.3.1.tar.gz<br>cd pip-1.3.1<br>python setup.py install<br></code></pre><p>可以愉快的使用pip了。</p>

## 安装yum
<p>据说yum的使用需要用到python2，具体我也没有细究，反正至少先安装好python2就行了。</p><p>以下链接与CentOS版本相关，具体链接可在网易镜像中依照具体系统查找。</p><pre><code class=\"language-shell\">cat /etc/centos-release  # 查看系统版本号<br># CentOS Linux release 7.6.1810 (Core)<br></code></pre>

### 下载相关文件
<pre><code class=\"language-shell\">wget http://mirrors.163.com/centos/7/os/x86_64/Packages/python-urlgrabber-3.10-8.el7.noarch.rpm<br>wget http://mirrors.163.com/centos/7/os/x86_64/Packages/python-iniparse-0.4-9.el7.noarch.rpm <br>wget http://mirrors.163.com/centos/7/os/x86_64/Packages/yum-plugin-fastestmirror-1.1.31-45.el7.noarch.rpm<br>wget http://mirrors.163.com/centos/7/os/x86_64/Packages/yum-metadata-parser-1.1.4-10.el7.x86_64.rpm<br>wget http://mirrors.163.com/centos/7/os/x86_64/Packages/yum-3.4.3-158.el7.centos.noarch.rpm<br></code></pre>

### 按照顺序安装
<pre><code class=\"language-shell\">rpm -ivh python-iniparse-0.4-9.el7.noarch.rpm<br>rpm -ivh python-urlgrabber-3.10-8.el7.noarch.rpm<br>rpm -ivh yum-3.4.3-158.el7.centos.noarch.rpm yum-metadata-parser-1.1.4-10.el7.x86_64.rpm yum-plugin-fastestmirror-1.1.31-45.el7.noarch.rpm<br></code></pre><p><strong>注意：yum和yum-plugin-xxx一定要同时安装，因为他们彼此依赖，先安装谁都会报错。</strong></p><p>安装中途很可能会出现依赖错误导致安装失败，如<code>python(abi) = 2.7 is needed by xxx</code>。</p><p>该错误则需要安装以下几个包：</p><p>python-libs-2.7.5-68.el7.x86_64.rpm</p><p>python-2.7.5-68.el7.x86_64.rpm</p><p>python-devel-2.7.5-68.el7.x86_64.rpm</p><p><strong>注意：在安装时仍然会遇到依赖错误，那么安装提示在网易镜像中查找对应版本文件，使用wget下载后安装即可。</strong></p>

### 修改yum源
<p>在使用yum安装时出现http404之类的错误，很有可能就是yum源有问题。</p><pre><code class=\"language-shell\"># 1.进入系统yum源目录<br>cd /etc/yum.repos.d<br># 2.备份原来的yum源<br>mkdir bak &amp;&amp; mv *.repo bak<br># 3.下载yum源<br>wget http://mirrors.163.com/.help/CentOS7-Base-163.repo<br># 4.改名成默认repo<br>mv CentOS7-Base-163.repo CentOS-Base.repo<br># 5.生成缓存，是配置生效<br>yum makecache<br># 6.验证配置源<br>yum repolist  # 查看是否有163的标识<br># 7.更新yum文件<br>yum update -y<br></code></pre><p>接下来就可以正常使用yum了。</p><p><strong>注意：修改时可能遇到<code>/usr/bin/applydeltarpm not installed</code>的情况</strong></p><p>缺什么安装什么就完事了！</p><pre><code class=\"language-shell\">yum provides '*/applydeltarpm'  <br>yum install deltarpm -y<br></code></pre>

## 安装python3

### 安装python3.8
<p>现在使用python开发基本都是使用python3了，自然不能少。python不能装太老的，因为以后可能官方就不支持了，也不能太新，因为还不稳定，资料也不够多，所以综合考虑python3.8就差不多了。</p><pre><code class=\"language-shell\">cd /usr/local/src<br>wget https://registry.npmmirror.com/-/binary/python/3.8.8/Python-3.8.8.tgz<br>tar -zxvf Python-3.8.8.tgz<br>cd Python-3.8.8<br>./configure<br>make &amp;&amp; make install<br></code></pre>

### 修改python3为默认
<p>安装后通过<code>python3</code>就能使用该版本，不过一般都习惯只键入<code>python</code>，所以在这里创建软链接即可</p><pre><code class=\"language-shell\">which python3  # 查看python3位置<br># /usr/local/bin/python3<br>ln /usr/local/bin/python3 /usr/local/bin/python  # 创建软链接<br>python -V  # 查看版本<br># Python 3.8.8<br></code></pre>

### 配置pip3
<p>python3.8安装成功后，pip3就已经安装完毕了！键入<code>pip3</code>就能正常使用，如果想将它设为默认的话，可以修改软链接。当然这一步其实可以不配置</p><pre><code class=\"language-shell\">which pip  # 查看原pip<br># /usr/local/bin/pip<br>mv /usr/local/bin/pip /usr/local/bin/pip_bak  # 备份<br>which pip3  # 查看pip3位置<br># /usr/local/bin/pip3<br>ln /usr/local/bin/pip3 /usr/local/bin/pip  # 创建软链接<br>pip -V  查看当前pip版本<br>pip 20.2.3 from /usr/local/lib/python3.8/site-packages/pip (python 3.8)<br></code></pre><p>完事！还是python3安装最简单。</p><p>至此终于把整个重装工作完成了！</p><p>网上的资料真是太坑了，要么不详细，要么根本就是直接偷别人的。fo了！</p>


