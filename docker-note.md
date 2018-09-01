基于centos7

#查看内核
uname -r
#安装docker
yum install docker
#加入开机启动
systemctl start docker
systemctl enable docker

安装docker-compose

pip install docker-compose

$ docker ps // 查看所有正在运行容器
$ docker stop containerId // containerId 是容器的ID

$ docker ps -a // 查看所有容器
$ docker ps -a -q // 查看所有容器ID

$ docker stop $(docker ps -a -q) //  stop停止所有容器
$ docker  rm $(docker ps -a -q) //   remove删除所有容器

