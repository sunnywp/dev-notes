# Centos7安装nodejs

node下载地址
https://nodejs.org/en/download/

$ wget https://nodejs.org/dist/v6.10.2/node-v6.10.2.tar.gz

1.安装gcc
$ yum install gcc-c++

2.安装libssl-dev
$ yum install openssl-dev

3.确保安装了python，大部分安装失败都是由于python版本过低导致。安装之前，升级python版本。
nodejs 0.8.5需要，请安装python前，先安装此模块。
$ yum install-ybzip2*    
$ tar zxvf node-v0.9.0.tar.gz 

这里安装在了/usr/local/node目录下
$ ./configure --prefix=/usr/local/node
$ make
$ make install
$ ln -s /usr/local/bin/node /usr/bin/node

3).配置NODE_HOME 
$ vi /etc/profile 
在export PATH USER 。。。一行的上面添加如下内容，并将NODE_HOME/bin设置到系统path中 
set for nodejs 
export NODE_HOME=/usr/local/node
export PATH=$NODE_HOME/bin:$PATH 
保存退出后执行如下命令，使刚才的配置生效 
$ source /etc/profile
执行node -h命令验证设置成功
$ node -h 
node,npm 到次安装完成
