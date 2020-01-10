一、创建mysql数据库
1.创建数据库语法
--创建名称为“testdb”数据库，并设定编码集为utf8
CREATE DATABASE IF NOT EXISTS testdb DEFAULT CHARSET utf8 COLLATE utf8_general_ci;
二、创建用户
1.新建用户
 --创建了一个名为：test 密码为：1234 的用户
 create user 'test'@'localhost' identified by '1234';
注意：
此处的"localhost"，是指该用户只能在本地登录，不能在另外一台机器上远程登录。如果想远程登录的话，将"localhost"改为"%"，表示在任何一台电脑上都可以登录。也可以指定某台机器可以远程登录。

2.查询用户
--查询用户
select user,host from mysql.user;
3.删除用户
--删除用户“test”
drop user test@localhost ;
--若创建的用户允许任何电脑登陆，删除用户如下
drop user test@'%';
4.更改密码
--方法1，密码实时更新；修改用户“test”的密码为“1122”
set password for test =password('1122');
--方法2，需要刷新；修改用户“test”的密码为“1234”
update  mysql.user set  password=password('1234')  where user='test'
--刷新
flush privileges;
5.用户分配权限
--授予用户test通过外网IP对数据库“testdb”的全部权限
grant all privileges on 'testdb'.* to 'test'@'%' identified by '1234';  

--刷新权限
flush privileges; 

--授予用户“test”通过外网IP对于该数据库“testdb”中表的创建、修改、删除权限,以及表数据的增删查改权限
grant create,alter,drop,select,insert,update,delete on testdb.* to test@'%';     
6.查看用户权限
--查看用户“test”
show grants for test;
注意：修改完权限以后 一定要刷新服务，或者重启服务，刷新服务用：flush privileges;
