# Ubuntu20.04安装MySQL8和virtualenv

```bash
sudo apt-get update  #更新源
```

安装mysql服务

```bash
sudo apt-get install mysql-server #安装
```

```bash
sudo service mysql status # 查看服务状态
sudo service mysql start # 启动服务
sudo service mysql stop # 停止服务
sudo service mysql restart # 重启服务
```

查看安装完成后的MySQL默认密码

```bash
sudo cat /etc/mysql/debian.cnf
```

使用默认的账户进行登录

```bash
mysql -u debian-sys-maint -p
```

修改默认密码

```bash
alter user 'root'@'localhost' identified with mysql_native_password by '密码';
alter user 'root'@'%' identified with mysql_native_password by '密码';
```

@后面的内容需要去mysql 中的user表中进行查看host的内容。有的是localhost，有的是%

卸载mysql

```bash
sudo apt purge mysql-*
sudo rm -rf /etc/mysql/ /var/lib/mysql
sudo apt autoremove
sudo apt autoclean
```

### mysql建立远程连接

#### ① 首先需要创建一个新用户

在这里我创建了一个新用户，用户名为admin， 密码设置为123

create user '用户名'@'%' identified by '密码';

```bash
create user 'admin'@'%' identified by '123';
```

#### ② 赋予权限

这里赋予的是所有的权限

grant all privileges on *.* to '用户名'@'%'  ;

这个%代表的是主机名，如果是localhost则需要改为localhost

```bash
grant all privileges on *.* to 'admin'@'%';
```

刷新

```python
flush privileges;
```

为新用户设置密码

```bash
alter user 'admin'@'%' identified with mysql_native_password by '123';
```

刷新

```bash
flush privileges;
```

#### ③ 修改配置文件

先需要切换到root 使用su

如果提示验证失败，则需要设置root密码 20以后的root密码为8为字符

```bash
sudo passwd root
```

设置密码即可

进入/etc/mysql/mysql.conf.d目录下

编辑mysqld.cnf文件,修改bind-address

![image-20210723121401964](https://github.com/moran-ai/Linux/blob/master/image-20210723121401964.png)

重新启动mysql服务

```bash
service mysql restart
```

### 配置vitualenv

#### 安装

```python
pip3 install virtualenv 
pip3 install virtualenvwrapper
```

#### 配置安装目录

在家目录下~新建一个文件夹，用来存放虚拟环境

```bash
mkdir .virtualenvs
```

编辑家目录下的.bashrc文件，在最后加入

ubuntu18以后，virtualenv的安装位置为.local/bin/virtualenv

```bash
export WORKON_HOME=$HOME/.virtualenvs  # 虚拟环境存放位置
export VIRTUALENVWRAPPER_PYTHON=/usr/bin/python3  # python的位置
export VIRTUALENVWRAPPER_VIRTUALENV=~/.local/bin/virtualenv  # virtualenv的安装位置
source ~/.local/bin/virtualenvwrapper.sh  # virtualenvwrapper的安装位置
```

