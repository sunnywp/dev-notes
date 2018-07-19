###1、安装Python2.7.3
```
sudo wget http://www.python.org/ftp/python/2.7.3/Python-2.7.3.tgz
sudo tar zxvf Python-2.7.3.tgz 
cd Pythod-2.7.3
sudo ./configure --prefix=/usr/local/python2.7
make
sudo make install
sudo ln -s /usr/local/python2.7/bin/python /bin/python2.7
```

./configure 
编辑Modules/Setup文件 
找到下面这句，去掉注释 
#zlib zlibmodule.c -I$(prefix)/include -L$(exec_prefix)/lib -lz 
重新编译安装：make & make install
