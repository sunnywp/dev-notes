# Centos7安装nodejs

### 1、下载nodeJs
下载地址https://nodejs.org/en/download/
```
cd /usr/local/src/
wget https://nodejs.org/dist/v8.12.0/node-v8.12.0-linux-x64.tar.xz
tar -Jxf node-v8.12.0-linux-x64.tar.xz
mv node-v8.12.0-linux-x64 /usr/local/node
```

### 2、安装gcc并设置软链接
```
yum install gcc gcc-c++
ln -s /usr/local/node/bin/node /usr/local/bin/node
ln -s /usr/local/node/bin/npm /usr/local/bin/npm
```

### 3、配置NODE_HOME 
```
vi /etc/profile 
# 在export PATH USER 。。。一行的上面添加如下内容，并将NODE_HOME/bin设置到系统path中 
export NODE_HOME=/usr/local/node
export PATH=$NODE_HOME/bin:$PATH 
# 保存退出后执行如下命令，使刚才的配置生效 
source /etc/profile
# 执行node -h命令验证设置成功
node -h 
```

### 4、npm ERR! Unexpected end of JSON input while parsing near '...YtVN3ZtkL5gmr8yfAyB5B'
```
npm cache clean --force
```
