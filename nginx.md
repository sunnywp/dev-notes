# CentOS 7上Nginx的安装和启动

### 1，加centos7的nginx源
```
$: rpm -Uvh http://nginx.org/packages/centos/7/noarch/RPMS/nginx-release-centos-7-0.el7.ngx.noarch.rpm
```

### 2，安装nginx
```
$: yum install nginx
```

### 3,防火墙设置
```
$: firewall-cmd --permanent --zone=public --add-service=http
$: firewall-cmd --permanent --zone=public --add-service=https
$: firewall-cmd --reload
# 查看防火墙状态和打开防火墙(有时候防火墙关闭时需要此操作)
$: systemctl status firewalld
$: systemctl start firewalld
```

### 4，配置Nginx
```
$: vi /etc/nginx/nginx.conf
```

### 5，开启Nginx
```
$: systemctl start nginx
$: systemctl enable nginx
```

### 6, 关闭nginx
```
$: systemctl stop nginx
>or
$: ps -ef | grep nginx
$: pkill -9 pid
```

