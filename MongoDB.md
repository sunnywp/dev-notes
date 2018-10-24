# MongoDB安装和配置详解

### 1、创建安装目录
```
cd /usr/local
mkdir mongoDB
```
### 2、下载mongoDB
找到相应版本的mongoDB，下载地址：https://www.mongodb.org/dl/linux
```
cd /usr/src
wget wget http://downloads.mongodb.org/linux/mongodb-linux-x86_64-3.2.21-rc0.tgz
tar zxvf mongodb-linux-x86_64-3.2.21-rc0.tgz
mv mongodb-linux-x86_64-3.2.21-rc0 /usr/local/mongoDB/mongodbserver
```
### 3、配置mongoDB
```
cd /usr/local/mongoDB/mongodbserver
mkdir data
mkdir log
mkdir etc
vi /usr/local/mongoDB/mongodbserver/etc/mongodb.conf
# 填入下面内容---------------
dbpath=/usr/local/mongoDB/mongodbserver/data
logpath=/usr/local/mongoDB/mongodbserver/logs/mongodb.log
port=27017
fork=true
journal=false
storageEngine=wiredTiger
auth=false
# --------------------------
```
### 4、启动mongoDB
```
cd /usr/local/mongoDB/mongodbserver/bin/
./mongod --config /usr/local/mongoDB/mongodbserver/etc/mongodb.conf
# 可能有报错，查看日志
vi /usr/local/mongoDB/mongodbserver/logs/mongodb.log
```
### 5、添加管理用户：userAdminAnyDatabase
```
./mongo
# 进入mongo命令行-----------
> use admin
> db.createUser({user: "admin", pwd: "XXX", roles: [{role: "userAdminAnyDatabase", db: "admin"}]});
# 查看用户是否创建成功
> show users #db.system.users.find()
# 关闭mongo, 这里注意不要使用kill直接去杀掉mongodb进程，（如果这样做了，请去data/db目录下删除mongo.lock文件）
> db.shutdownServer()
```
### 6、使用权限方式重新启动mongoDB
在配置文件中，将"auth"改为"true"，然后重启后用admin用户验证
```
> use admin
> db.auth('admin', 'XXX') # 这验证成功后会显示：1
> db.updateUser("admin", {
    roles : [
      {"role": "userAdminAnyDatabase", "db": "admin"},
      {"role": "dbOwner", "db": "admin"},
      {"role": "clusterAdmin", "db": "admin"}
    ]
  })
```
### 7、配置环境变量
```
vi /etc/profile
# 加入以下内容-------------------
PATH=$PATH:/usr/local/mongoDB/mongodbserver/bin
# ------------------------------
source /etc/profile
ln -s /usr/local/mongoDB/mongodbserver/bin/mongo  /usr/bin/mongo
```
### 8、关闭服务，启动服务
```
# 关闭
> db.shutdownServer()
# 启动
mongod --config /usr/local/mongoDB/mongodbserver/etc/mongodb.conf
```
### 9、把mongoDB设置为系统服务并且设置开机启动
```
vi /etc/rc.d/init.d/mongod

# 加入以下内容-------------------
start() {  
/usr/local/mongoDB/mongodbserver/bin/mongod  --config /usr/local/mongoDB/mongodbserver/etc/mongodb.conf 
}  
  
stop() {  
/usr/local/mongoDB/mongodbserver/bin/mongod --config /usr/local/mongoDB/mongodbserver/etc/mongodb.conf --shutdown  
}  
case "$1" in  
  start)  
 start  
 ;;  
  
stop)  
 stop  
 ;;  
  
restart)  
 stop  
 start  
 ;;  
  *)  
 echo  
$"Usage: $0 {start|stop|restart}"  
 exit 1  
esac
# ------------------------------

chmod +x /etc/rc.d/init.d/mongod
service mongod start
```
