# CentOS 7 中间件、数据库安装

- yum源配置  
<https://www.runoob.com/linux/linux-yum.html>

- redis安装配置    

https://www.cnblogs.com/autohome7390/p/6433956.html>

- mysql 安装配置  
```
检查该服务器上是否已安装，如果有需卸载
rpm -qa | grep -i mysql
rpm -qa | grep -i mysql |xargs rpm -e --nodeps
whereis mysql
rm -fr /usr/share/mysql
```
```
修改 数据存放路径
vi /etc/my.cnf
datadir=/media/mysql
```
```
启动mysql
sudo service mysqld start
查看数据库登录密码
sudo grep 'temporary password' /var/log/mysqld.log
在my.cnf中加入mysqld     --此处不加 远程登录会报密码错误
skip-grant-tables
然后修改密码：
update user set authentication_string=password('123456') where user='root';
FLUSH PRIVILEGES;
登录后，执行以下语句修改密码：
ALTER USER 'root'@'localhost' IDENTIFIED BY '123456';
创建一个和root一样权限的用户：
CREATE USER 'admin' IDENTIFIED BY '123456';
GRANT ALL ON *.* TO 'admin' WITH GRANT OPTION;
以下是修改mysql参数：
SET GLOBAL max_allowed_packet=1024*1024*16;
```   
- nginx 安装   
<https://blog.csdn.net/u014695188/article/details/51066824>   

- yum 安装java   
<https://www.cnblogs.com/kevingrace/p/5870814.html>
