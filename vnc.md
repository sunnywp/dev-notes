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



Ubuntu14.04使用VNC解决gnome-session-is-accelerated: No composite
博客分类： linux
 
Ubuntu14.04目前仍是非常不稳定，使用VNC的时候，只有一个终端，检查日志以后，发现了如下错误：

 

 

Java代码  收藏代码
$ cat /home/brett/.vnc/bt-desktop:1.log  
……  
gnome-session-is-accelerated: No composite extension.  
gnome-session-check-accelerated: Helper exited with code 256  
gnome-session-is-accelerated: No composite extension.  
gnome-session-check-accelerated: Helper exited with code 256  
   
** (process:6694): WARNING **: software acceleration check failed: Child process exited with code 1  
   
** (gnome-session:6694): CRITICAL **: We failed, but the fail whale is dead. Sorry....  
……  
 

简单看了下，可能是gnome-session无法识别到OpenGL硬件加速导致的。该问题普遍存在于DELL大部分系列的服务器上。这个帖子提供了一种解决办法，但我试了下，貌似是无效的。

 

解决办法：

Java代码  收藏代码
$ sudo apt-get install gnome-panel gnome-settings-daemon metacity nautilus gnome-terminal    
   
$ vim ~/.vnc/xstartup  #修改此文件,写入以下内容    
#!/bin/sh    
   
export XKL_XMODMAP_DISABLE=1  
unset SESSION_MANAGER  
unset DBUS_SESSION_BUS_ADDRESS  
     
gnome-panel &  
gnome-settings-daemon &  
metacity &  
nautilus &  
gnome-terminal &  
 
然后重新启动VNC终端即可

也可以通过安装KDE/XFCE来解决：

 
http://ljl-xyf.iteye.com/blog/2231934   
Java代码  收藏代码
$ sudo apt-get install gnome-core xfce4 firefox    
$ vim ~/.vnc/xstartup  #修改此文件,写入以下内容    
#!/bin/sh    
unset SESSION_MANAGER    
unset DBUS_SESSION_BUS_ADDRESS    
startxfce4 &    
#gnome-session --session=gnome-flashback &    
   
[ -x /etc/vnc/xstartup ] && exec /etc/vnc/xstartup    
[ -r $HOME/.Xresources ] && xrdb $HOME/.Xresources    
xsetroot -solid grey    
vncconfig -iconic &   
 

============================================
后续补充：
Ubuntu14.04真是很差劲，使用VNC连接成功上桌面以后，S键、D键与Alt功能键相反了。
解决办法：

#必须在图形界面的终端里操作，不能在远程的终端里操作
gsettings set org.gnome.desktop.wm.keybindings panel-main-menu "['<Alt>F1']"
gsettings set org.gnome.desktop.wm.keybindings switch-applications "['<Alt>Tab']"
gsettings set org.gnome.desktop.wm.keybindings show-desktop "['<Alt>d']"
gsettings set org.gnome.desktop.wm.keybindings maximize "['<Alt>Up']"
gsettings set org.gnome.desktop.wm.keybindings unmaximize "['<Alt>Down']"
#也可以运行dconf-editor在GUI界面中展开指定的项进行修改

2015.03.23补充：
VNC日志里出现如下错误：

error opening security policy file /etc/X11/xserver/SecurityPolicy
Could not init font path element /usr/share/fonts/X11/cyrillic, removing from list!
Could not init font path element /usr/share/fonts/X11/100dpi/:unscaled, removing from list!
Could not init font path element /usr/share/fonts/X11/75dpi/:unscaled, removing from list!
Could not init font path element /usr/share/fonts/X11/100dpi, removing from list!
Could not init font path element /usr/share/fonts/X11/75dpi, removing from list!
Could not init font path element built-ins, removing from list!

解决办法：

sudo aptitude install xfonts-100dpi xfonts-75dpi xfonts-scalable xfonts-cyrillic

2015.03.24，关于windows下无法连接Ubuntu 14.04 VNC的问题，在网上发现了一个解决办法（参考文章），但我试了下貌似不成功：

#必须在图形界面的终端里操作，不能在远程的终端里操作
sudo apt-get install dconf-tools
sudo dconf write /org/gnome/desktop/remote-access/require-encryption false

gsettings set org.gnome.Vino require-encryption false

更改背景图片：

#必须在图形界面的终端里操作，不能在远程的终端里操作
sudo dconf write /org/gnome/desktop/background/picture-uri file:///usr/share/backgrounds/Forever_by_Shady_S.jpg

gsettings set org.gnome.desktop.background draw-background false
gsettings set org.gnome.desktop.background picture-uri file:///usr/share/backgrounds/Forever_by_Shady_S.jpg
gsettings set org.gnome.desktop.background draw-background true
