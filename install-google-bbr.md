本文原创来自：https://www.vultrclub.com/174.html

一般而言，服务器本身的速度是决定我们项目打开速度、下载速度的关键，但是我们也可以借助第三方软件工具等提高加速效果，比如我们肯定很多人都熟悉的锐速、Net-Speeder可以双倍发包流量，可以减少超时和提高下载速度。这不在前一段时间，来自大名鼎鼎的谷歌发布开源Google BBR工具，可以提高发包数据量，起到加速作用。

这里，我们也在Vultr VPS中安装Google BBR工具，因为是支持KVM和XEN架构的，我们的VULTR都是KVM架构所以肯定支持，但是由于内核的问题，我们需要调试和安装必备的内核和组件才 可以使用，我们一起安装试试吧。

第一、准备工作

这里我选择使用Vultr美国洛杉矶机房5美金月付方案，系统采用CENTOS7 64BIT。很多人要问为什么不选择速度较好的日本机房，因为日本机房虽然目前用NTT线路，PING速度看着还可以，但是稳定性不行，所以我不选择，尤其是晚上速度很差。

第二、查看当前核心

uname -r

这里我们看到当前CENTOS7核心是3.10.0-514.2.2.el7.x86_64，这个核心是不可以安装BBR的。

第三、更新内核

rpm --import https://www.elrepo.org/RPM-GPG-KEY-elrepo.org

rpm -Uvh http://www.elrepo.org/elrepo-release-7.0-2.el7.elrepo.noarch.rpm

安装4.9.0内核

yum --enablerepo=elrepo-kernel install kernel-ml -y

我们要知道，BBR目前只支持4.9.0内核，其他内核是不行的，需要更换内核才可以。

第四、检查内核是否更新

rpm -qa | grep kernel

我们看到了有4.9.0内核，需要启动才可以。

grub2-set-default 1

然后重启

shutdown -r now

第五、检查是否生效

uname -r

检查当前内核是不是4.9.4-1.el7.elrepo.x86_64.

看来内核是搞定了，我们那就开始安装BBR了。

第六、安装Google BBR

echo 'net.core.default_qdisc=fq' | sudo tee -a /etc/sysctl.conf

echo 'net.ipv4.tcp_congestion_control=bbr' | sudo tee -a /etc/sysctl.conf

sysctl -p

第七、检查BBR是否成功

sysctl net.ipv4.tcp_available_congestion_control

执行命令，看看是否是提示"net.ipv4.tcp_available_congestion_control = bbr cubic reno"

sysctl -n net.ipv4.tcp_congestion_control

执行命令，是否提示bbr

lsmod | grep bbr

执行命令，是否看到BBR提示。

能看到上面提示，就说明BBR安装成功。后面，我们再去安装需要的工具，比如SS或者其他项目，速度上是有明显提升的。
