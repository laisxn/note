
#centos-docker-php

##docker安装
1、查看内核 `uname -r`

2、yum 安装`yum -y install docker`

3、加入开机启动 `systemctl start docker systemctl enable docker`

4、解决用Dockerfile生成镜像慢的问题 

    编辑文件 `/etc/docker/daemon.json（Linux） { "registry-mirrors": ["https://navyf335.mirror.aliyuncs.com"] }`

##docker-compose安装
1、安装python3 `yum -y install python3`

2、安装python-pip 

    ① `yum -y install epel-release`

    ② `yum -y install python-pip`

3、安装docker-compose `pip install docker-compose`

##git安装
1、`yum -y install git`

##Dokcerfile安装php加composer例子
[https://github.com/laisxn/docker-php7.2/blob/master/Dockerfile]
git clone地址，进入对应的project文件夹，执行命令
`docker build -t php7.2:v4 .`(注意最后面有.表示当前目录，也可以-f指定文件路径)

######docker登录 `docker login`

######docker提交镜像 `dokcer push 镜像名:tag`

##docker-compose 例子
[https://github.com/laisxn/docker-project]

git clone地址，进入对应的project文件夹，执行命令
`docker-compose up`
