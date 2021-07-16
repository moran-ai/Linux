# 配置Linux中的pip源和设置python，pip的软链接

centos7中的默认python版本为python2,使用wget进行安装

① 首先需要下载wget工具

```bash
yum install wget
```

② 下载依赖包

```bash
yum install zlib-devel bzip2-devel openssl-devel ncurses-devel sqlite-devel readline-devel tk-devel gcc make libffi-devel
```

③ 下载python3源码

```bash
wget https://www.python.org/ftp/python/3.8.1/Python-3.8.1.tgz
```

④ 解压

```bash
tar -zxvf Python-3.8.1.tgz  
```

⑤ 进入解压的文件夹

```bash
cd Python-3.8.1
```

⑥ 配置安装位置

```bash
./configure --prefix=/usr/local/python3.8
```

⑦ 进行编译和安装

```bash
make && make install 
```

编译没出错就代表成功

进入/usr/local/python3.8，使用ls可以看见内容就可以

⑧ 添加软链接

语法：

ln -s  python安装路径 软链接的路径

```bash
ln -s /usr/local/python3.8/bin/python3.8 /usr/bin/python3
ln -s /usr/local/python3.8/bin/pip3.8 /usr/bin/pip3 
```

⑨ 测试  

终端输入python3 和pip3

修改软链接

```
ln -sb /usr/local/python3.8/bin/python3.8  /usr/bin/python3

语法:
	ln -sb python/pip安装路径  新的软链接路径
```

删除软链接

```
rm -rf 软链接路径
```



### 修改pip源

切换为root用户，进入用户根目录~

```
cd ~
mkdir .pip
cd .pip
vim pip.conf

里面添加源
[global]
index-url=https://pypi.tuna.tsinghua.edu.cn/simple
trusted-host = pypi.tuna.tsinghua.edu.cn

保存退出
```

