```
1. 下载tgz包
https://www.python.org/downloads/
2.解压压缩包
tar zxvf {包名}
3.安装
./configure --prefix=/usr/local/python --with-ssl --enable-optimizations
make && make install
4.创建软链接
sudo unlink /usr/bin/python
sudo ln -s /usr/local/python/bin/python3 /usr/bin/python
sudo unlink /usr/bin/pip
sudo ln -s /usr/local/python/bin/pip3 /usr/bin/pip
/usr/local/python/bin/python3.7 -m pip install --upgrade pip -i http://mirrors.aliyun.com/pypi/simple/ --trusted-host mirrors.aliyun.com
5.验证
python -V
```
