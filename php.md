安装依赖
yum install -y autoconf automake
yum install libxml2 libxml2-devel openssl openssl-devel bzip2 bzip2-devel libcurl libcurl-devel libjpeg libjpeg-devel libpng libpng-devel freetype freetype-devel gmp gmp-devel libmcrypt libmcrypt-devel readline readline-devel libxslt libxslt-devel
下载php
wget -O php-7.2.2.tar.gz  http://cn2.php.net/get/php-7.2.2.tar.gz/from/this/mirror
tar zxvf php-7.2.2.tar.gz
cd php-7.2.2

./configure \
--prefix=/usr/local/php\
--with-config-file-path=/etc\
--enable-fpm \
--with-fpm-user=nginx \
--with-fpm-group=nginx \
--enable-inline-optimization \
--disable-debug \
--disable-rpath \
--enable-shared \
--enable-soap \
--with-libxml-dir \
--with-xmlrpc \
--with-openssl \
--with-mhash \
--with-pcre-regex \
--with-sqlite3 \
--with-zlib \
--enable-bcmath \
--with-iconv \
--with-bz2 \
--enable-calendar \
--with-curl \
--with-cdb \
--enable-dom \
--enable-exif \
--enable-fileinfo \
--enable-filter \
--with-pcre-dir \
--enable-ftp \
--with-gd \
--with-openssl-dir \
--with-jpeg-dir \
--with-png-dir \
--with-zlib-dir \
--with-freetype-dir \
--enable-gd-jis-conv \
--with-gettext \
--with-gmp \
--with-mhash \
--enable-json \
--enable-mbstring \
--enable-mbregex \
--enable-mbregex-backtrack \
--with-libmbfl \
--with-onig \
--enable-pdo \
--with-mysqli=mysqlnd \
--with-pdo-mysql=mysqlnd \
--with-zlib-dir \
--with-pdo-sqlite \
--with-readline \
--enable-session \
--enable-shmop \
--enable-simplexml \
--enable-sockets \
--enable-sysvmsg \
--enable-sysvsem \
--enable-sysvshm \
--enable-wddx \
--with-libxml-dir \
--with-xsl \
--enable-zip \
--enable-mysqlnd-compression-support \
--with-pear \
--enable-opcache

make
make test
make install

cp php.ini-production /usr/local/php/etc/php.ini  #复制php配置文件到安装目录

rm -rf /etc/php.ini  #删除系统自带配置文件

ln -s /usr/local/php/etc/php.ini /etc/php.ini   #添加软链接到 /etc目录

cp /usr/local/php/etc/php-fpm.conf.default /usr/local/php/etc/php-fpm.conf  #拷贝模板文件为php-fpm配置文件

ln -s /usr/local/php/etc/php-fpm.conf /etc/php-fpm.conf  #添加软连接到 /etc目录

 #编辑
vim php-fpm.conf
pid = run/php-fpm.pid #取消前面的分号

cd php-fpm.d
cp www.conf.default  www.conf
user = www #设置php-fpm运行账号为www

group = www #设置php-fpm运行组为www

:wq! #保存退出

设置 php-fpm开机启动

cp /usr/local/src/php-7.2.2/sapi/fpm/init.d.php-fpm /etc/rc.d/init.d/php-fpm #拷贝php-fpm到启动目录

chmod +x /etc/rc.d/init.d/php-fpm #添加执行权限

chkconfig php-fpm on #设置开机启动

设置php环境变量
echo "export PATH=/usr/local/php/bin/:$PATH" >> /etc/profile
source /etc/profile

php -v
