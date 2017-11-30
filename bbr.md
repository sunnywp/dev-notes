# 给CentOs7使用Google BBR加速
### 1, 查看当前系统核心
```
$: uname -r
# 这里我们看到当前CENTOS7核心是3.10.0-514.2.2.el7.x86_64，这个核心是不可以安装BBR的, BBR目前只支持4.9.0以上的内核。
```

### 2, 更新内核
```
$: rpm --import https://www.elrepo.org/RPM-GPG-KEY-elrepo.org
$: rpm -Uvh http://www.elrepo.org/elrepo-release-7.0-2.el7.elrepo.noarch.rpm
$: yum --enablerepo=elrepo-kernel install kernel-ml -y
```

### 3, 检查内核是否更新并设置上并重启
```
$: rpm -qa | grep kernel
$: grub2-set-default 1
$: shutdown -r now
```

### 4, 开机，检查内核是否生效
```
$: uname -r
# 检查当前内核是不是4.9.4-1.el7.elrepo.x86_64或以上版本。
```

### 5, 安装Google BBR
```
$: echo 'net.core.default_qdisc=fq' | sudo tee -a /etc/sysctl.conf
$: echo 'net.ipv4.tcp_congestion_control=bbr' | sudo tee -a /etc/sysctl.conf
$: sysctl -p
```

### 6, 检查BBR是否成功
```
$: sysctl net.ipv4.tcp_available_congestion_control
# 执行命令，看看是否是提示"net.ipv4.tcp_available_congestion_control = bbr cubic reno"
$: sysctl -n net.ipv4.tcp_congestion_control
# 执行命令，是否提示bbr
$: lsmod | grep bbr
# 执行命令，是否看到BBR提示。

#能看到上面提示，就说明BBR安装成功。后面，我们再去安装需要的工具，比如SS或者其他项目，速度上是有明显提升的。
```


