# Centos7安装nodejs

### 1、下载nodeJs
下载地址https://nodejs.org/en/download/
```
cd /usr/local/src/
wget https://nodejs.org/dist/v8.12.0/node-v8.12.0-linux-x64.tar.xz
tar -Jxf node-v8.12.0-linux-x64.tar.xz
mv node-v8.12.0-linux-x64 /usr/local/node
```

### 2、安装gcc、ssl-dev和bzip2*
```
yum install gcc-c++
yum install openssl-dev
# 确保安装了python，大部分安装失败都是由于python版本过低导致。安装之前，升级python版本。
# nodejs 0.8.5需要，请安装python前，先安装此模块。
yum install -y bzip2*
./configure --prefix=/usr/local/node
make
make install
ln -s /usr/local/bin/node /usr/bin/node
```

### 3、配置NODE_HOME 
```
vi /etc/profile 
# 在export PATH USER 。。。一行的上面添加如下内容，并将NODE_HOME/bin设置到系统path中 
export NODE_HOME=/usr/local/node
export PATH=$NODE_HOME/bin:$PATH 
# 保存退出后执行如下命令，使刚才的配置生效 
source /etc/profile
ln -s /opt/node-v0.12.10-linux-x86/bin/node /usr/local/bin/node
ln -s /opt/node-v0.12.10-linux-x86/bin/npm /usr/local/bin/npm
# 执行node -h命令验证设置成功
node -h 
```

