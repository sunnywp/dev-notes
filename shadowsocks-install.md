# 给Linux系统安装ShadowSocks服务器

主要有两种方法：1，脚本自动安装 2，手动安装

## 方法一 (脚本快速安装)

### 【脚本功能】
自定义端口号和密码
全过程静默安装，不会打扰用户，你所要做的就是去听一首音乐或者去喝杯咖啡
一次只允许运行一个shadowsocks进程，脚本会自动检测原来已经运行的进程并杀死
安装防火墙并开放需要的端口

### 【操作步骤】
```
# 下载脚本
$ wget -O ss.sh http://cntower001.com/file/ss.sh
# 下载脚本
$ bash ss.sh
```

### 【运行步骤】
设置端口号并回车，直接回车是设置为1225
Please enter PORT(1225 default):
设置密码并回车，直接回车是设置为123456
Please enter PASSWORD(123456 default):
等待一会……就完成了（初次执行约2-5min）

### 【脚本源码】
```
#! /bin/bash
# log路径
export log_path=/etc/ss.log
# 设置端口号
echo -n -e '\033[36mPlease enter PORT(1225 default): \033[0m'
# echo -n "please enter port(1225 default):"
read port
if [ ! -n "$port" ];then
        echo "port will be set to 1225"
        port=1225
else
        echo "port will be set to $port"
fi
# 设置密码
echo -n -e '\033[36mPlease enter PASSWORD(123456 default): \033[0m'
# echo -n "please enter password(123456 default):"
read pwd
if [ ! -n "$pwd" ];then
        echo "password will be set to 123456"
        pwd=123456
else
        echo "password will be set to $pwd"
fi
# 写shadowsocks.json配置文件
cat>/etc/shadowsocks.json<<EOF
{
    "server":"0.0.0.0",
    "server_port":$port,
    "local_address": "127.0.0.1",
    "local_port":1080,
    "password":"$pwd",
    "timeout":300,
    "method":"aes-256-cfb",
    "fast_open": false
}
EOF
# 安装 shadowsocks 防火墙等
ret=`yum install -y m2crypto python-setuptools >> ${log_path} 2>&1`
ret=`easy_install pip >> ${log_path} 2>&1`
ret=`pip install shadowsocks >> ${log_path} 2>&1`
ret=`yum install -y firewalld >> ${log_path} 2>&1`
ret=`systemctl start firewalld >> ${log_path} 2>&1`
# 开启端口
ret=`firewall-cmd --permanent --zone=public --add-port=22/tcp >> ${log_path} 2>&1`
ret=`firewall-cmd --permanent --zone=public --add-port=$port/tcp >> ${log_path} 2>&1`
ret=`firewall-cmd --reload >> ${log_path} 2>&1`
# 如果有相同功能的进程则杀死
ps -ef|grep ssserver|grep shadowsocks|awk '{ print $2 }'|xargs kill -9
nohup /usr/bin/ssserver -c /etc/shadowsocks.json &
# 成功
if [ $? -eq 0 ];then
clear
cat<<EOF
***************Congratulation!*************
Shadowsocks installed successfully!

PORT: $port
PASSWORD: $pwd
METHOD: aes-256-cfb

***************JUST ENJOY IT!**************
EOF
# 失败
else
clear
cat<<EOF
************Failed,retry please!***********

cat /etc/ss.log to get something you need…

************Failed,retry please!***********
EOF
fi
```

## 方法二 (独立动手搭建)

### 【搭建 Shadowsocks 服务】
```
注意：
Python版本要求2.7以上
# 安装组件
$ yum install m2crypto python-setuptools
$ easy_install pip
$ pip install shadowsocks

# 安装完成后配置服务器参数
$ vi  /etc/shadowsocks.json
# 写入如下配置:
{
    "server":"0.0.0.0",
    "server_port":443,
    "local_address": "127.0.0.1",
    "local_port":1080,
    "password":"123456",
    "timeout":300,
    "method":"aes-256-cfb",
    "fast_open": false
}
# 多端口的如下：
{
    "server":"0.0.0.0",
    "local_address": "127.0.0.1",
    "local_port":1080,
    "port_password": {
         "443": "443",
         "8888": "8888"
     },
    "timeout":300,
    "method":"aes-256-cfb",
    "fast_open": false
}
# 其中server字段与local_address填写之前的IP Address. password是自己用于连接这个shadow socks的密码，自定义就好，其他的不需要更改。
```

### 【配置防火墙】
```
# 安装防火墙
$ yum install firewalld
# 启动防火墙
$ systemctl start firewalld
# 开启防火墙相应的端口(端口号是你自己设置的端口)（这里需要注意同时暴露ssh端口）
$ firewall-cmd --permanent --zone=public --add-port=443/tcp
$ firewall-cmd --reload
```

### 【启动 Shadowsocks 服务】
```
$ ssserver -c /etc/shadowsocks.json
如果想干点其他的实现后台运行，使用
$ nohup ssserver -c /etc/shadowsocks.json &
```

### 【连接】
下载Shadow Socks客户端。SS加速器客户端下载 
选择适合的版本，下载并解压运行。
填写信息:服务器地址，端口号，密码，加密方式与代理端口默认即可
iOS
在App Store下载Wingy。
填写信息:服务器，端口，密码，代理模式，加密方式默认即可。


