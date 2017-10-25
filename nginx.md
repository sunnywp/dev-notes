# 1.CentOS 7上Nginx的安装和启动
```
# rpm -Uvh http://nginx.org/packages/centos/7/noarch/RPMS/nginx-release-centos-7-0.el7.ngx.noarch.rpm

# yum install nginx

# firewall-cmd --permanent --zone=public --add-service=http
# firewall-cmd --permanent --zone=public --add-service=https
# firewall-cmd --reload

# systemctl start nginx
# systemctl enable nginx
```
安装完成后在浏览器中打开服务器对应的IP地址即可测试是否安装成功
# 2.配置nginx
  Nginx的主配置文件是/etc/nginx/nginx.conf
# 3.启动、停止和重启nginx
```
1)启动
 #方法1
 # /usr/sbin/nginx -c /usr/local/nginx/conf/nginx.conf
 #方法2
 # cd /usr/local/nginx/sbin
 # ./nginx

2) 停止
 #查询nginx主进程号 
 ps -ef | grep nginx
 #停止进程 
 kill -QUIT 主进程号 
 #快速停止 
 kill -TERM 主进程号 
 #强制停止 
 pkill -9 nginx
 
3) 重启
 (首次启动需：/usr/local/nginx/sbin/nginx -c /usr/local/nginx/conf/nginx.conf)
 /usr/local/nginx/sbin/nginx -s reload

```







