安装mysql5.7（http://www.cnblogs.com/gongfuxiaomaintuan/p/5960373.html）
添加用户组合用户
groupadd -r mysql && useradd -r -g mysql -s /bin/false -M mysql

mkdir -p /usr/local/mysql/data
chown -R mysql:mysql /usr/local/mysql

编译工具cmake
yum -y install make gcc-c++ cmake bison-devel ncurses-devel libaio
安装相关依赖

 

yum -y install cmake ncurses ncurses-devel bison bison-devel boost boost-devel

wget http://dev.mysql.com/get/Downloads/MySQL-5.7/mysql-5.7.17.tar.gz
tar xzvf myswl...

cmake -DCMAKE_INSTALL_PREFIX="/usr/local/mysql" -DDEFAULT_CHARSET=utf8 -DMYSQL_DATADIR="/usr/local/mysql/data/" -DCMAKE_INSTALL_PREFIX="/usr/local/mysql" -DINSTALL_PLUGINDIR=plugin -DWITH_INNOBASE_STORAGE_ENGINE=1 -DDEFAULT_COLLATION=utf8_general_ci -DENABLE_DEBUG_SYNC=0 -DENABLED_LOCAL_INFILE=1 -DENABLED_PROFILING=1 -DWITH_ZLIB=system -DWITH_EXTRA_CHARSETS=none -DMYSQL_MAINTAINER_MODE=OFF -DEXTRA_CHARSETS=all -DWITH_PERFSCHEMA_STORAGE_ENGINE=1 -DWITH_MYISAM_STORAGE_ENGINE=1 -DDOWNLOAD_BOOST=1 -DWITH_BOOST=/usr/local/boost

make && make install

chown -R mysql:mysql /usr/local/mysql/
 mv ./my-default.cnf /etc/my.cnf
cp ./mysql.server /etc/init.d/mysqld
chmod 755 /etc/init.d/mysqld

 chkconfig --list mysqld
chkconfig --add mysqld
chkconfig --level 345 mysqld on

设置环境变量

echo "export PATH=/usr/local/mysql/bin/:$PATH" >> /etc/profile

source /etc/profile

mysql -uroot -p

bin/mysqld --initialize //初始化  

http://blog.csdn.net/qq_26941173/article/details/51548947（设置初始密码）{

1.修改配置文件my.cfg

[root@localhost ~]# vi /etc/my.cnf

找到mysqld在之后添加

skip-grant-tables

保存退出

2. 重启MySQL服务

service  mysqld  restart

3.直接登陆mysql而不需要密码

 mysql -u root   (直接点击回车)

4.在mysql中输入

update mysql.user  set password=password('root') where user='root'；

（此时提示ERROR 1054 (42S22): Unknown column 'password' in 'field list'）

5.（这是怎么回事？）原来是mysql数据库下已经没有password这个字段了，password字段改成了authentication_string

update mysql.user set authentication_string=password('root') where user='root' ;

6.执行flush
 privileges

7.退出mysql
 ，到my.cgf中把开始添加的skip-grant-tables去掉

8.重启mysql服务

大功告成！
 但是事实并非如此！

9.当你登陆mysql之后你会发现，当你执行命令时会出现

ERROR
 1820 (HY000): You must reset your password using ALTER USER statement；

当你执行了SET PASSWORD
 = PASSWORD('123456')；

出现：ERROR
 1819 (HY000): Your password does not satisfy the current policy requirements

10.你需要执行两个参数来把mysql默认的密码强度的取消了才行

 	set global validate_password_policy=0;

	set global validate_password_mixed_case_count=2;

11这是你在执行

        SET PASSWORD = PASSWORD('root')；


}

授权用户
grant all on *.* to lai@'%' identified by 'lai123456789';
