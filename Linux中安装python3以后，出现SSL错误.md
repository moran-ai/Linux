# Linux中安装python3以后，出现SSL错误

Linux中安装了Python3以后，无法使用pip3进行模块的下载以及pip的升级,在python3中无法使用ssl这个模块 出错信息

```python
[GCC 4.1.2 20080704 (Red Hat 4.1.2-54)] on linux2
Type "help", "copyright", "credits" or "license" for more information.
>>> import ssl
Traceback (most recent call last):
File "<stdin>", line 1, in <module>
File "/usr/local/python27/lib/python2.7/ssl.py", line 60, in <module>
import _ssl # if we can't import it, let the error propagate
ImportError: No module named _ssl
```

解决办法如下：

1.查看openssl安装包，发现缺少openssl-devel包

```bash
[root@localhost ~]# rpm -aq|grep openssl
openssl-0.9.8e-20.el5
openssl-0.9.8e-20.el5
[root@localhost ~]#
```

2. yum安装openssl-devel

   ```bash
   [root@localhost ~]# yum install openssl-devel -y
   #查看安装结果
   [root@localhost ~]# rpm -aq|grep openssl
   openssl-0.9.8e-26.el5_9.1
   openssl-0.9.8e-26.el5_9.1
   openssl-devel-0.9.8e-26.el5_9.1
   openssl-devel-0.9.8e-26.el5_9.1
   ```

3.重新编译python 进入python的安装目录

```python
#修改Setup文件
vi /usr/software/Python-2.7.5/Modules/Setup
#修改结果如下：

# 通过输入：？ssl 快速查找到定位到以下内容（备注）


# Socket module helper for socket(2)
_socket socketmodule.c timemodule.c    #去除该行注释（备注）

# Socket module helper for SSL support; you must comment out the other
# socket line above, and possibly edit the SSL variable:
#SSL=/usr/local/ssl
_ssl _ssl.c \      #去除该行注释（备注）
-DUSE_SSL -I$(SSL)/include -I$(SSL)/include/openssl \  #去除该行注释（备注）
-L$(SSL)/lib -lssl -lcrypto         #去除该行注释（备注）
```

4.进行编译  进入python的安装目录

```bash
make
make install
```

5.测试，可用

```python
>>> import ssl
>>>
```

https://www.cnblogs.com/yuechaotian/archive/2013/06/03/3115472.html

