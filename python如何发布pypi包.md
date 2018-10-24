### 1、修改pip镜像地址
```
$ vi ~/.pip/pip.conf 
> [global]
> #index-url = https://pypi.tuna.tsinghua.edu.cn/simple  (清华)
> #index-url = http://pypi.douban.com/simple/  (豆瓣)
```
### 2、发布pypi包
1. 就是到pypi官网上注册自己的用户。地址：https://pypi.python.org/pypi
2. 准备setup.py或setup.conf文件，它是放在你包的根目录的
3. 开始安装：
```
$ python setup.py sdist build
$ twine upload dist/*
```
