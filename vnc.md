最近一直在用putty或Xshell登录linux服务器，今天用ultraVNC Viewer登录服务器时发现自己的vnc服务器挂掉了。在此，整理下vnc操作的基本命令。

查看vnc进程：

ps -ef | grep vnc 
该命令可以列出当前系统上所有用户vnc界面的端口号，分辨率等信息。 
我们要用到的只是端口号。

杀掉自己vnc端口对应的进程

vncserver -kill :43 
我的端口号是43

启动vnc服务器

vncserver :43 
启动后发现，与自己的电脑桌面相比界面比较小，点击全屏后，原来多余的部分都变成黑色的了。 
通过上面说的查看vnc进程命令后发现有个信息-geometry 1024x768，这说明vnc的默认分辨率是1024x768，而我的笔记本分辨率是1366x768。所以要全屏，只需要修改分辨率即可。

修改vnc分辨率

修改分辨率的命令为： 
vncserver -geometry 1366x768 :43 
在这里要注意一点，在你的vnc server运行期间，使用该命令是会报错的：‘A VNC server is already running as :43’ 
所以，在修改vnc分辨率之前，我们需要先将vnc server关闭。用上面说过的-kill命令杀掉vnc端口对应的进程即可。 
然后再运行命令修改分辨率即可。之后登录vnc界面就可以看到界面的大小已经改变了。
