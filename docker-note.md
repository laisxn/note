基于centos7

# 查看内核
uname -r
# 安装docker
yum install docker
# 加入开机启动
systemctl start docker
systemctl enable docker

#安装 docker-compose

pip install docker-compose

$ docker ps // 查看所有正在运行容器
$ docker stop containerId // containerId 是容器的ID

$ docker ps -a // 查看所有容器
$ docker ps -a -q // 查看所有容器ID

$ docker stop $(docker ps -a -q) //  stop停止所有容器
$ docker  rm $(docker ps -a -q) //   remove删除所有容器

docker exec -i -t 容器ID或名字 /bin/bash  // 进入运行中的容器

# 宿主机运行定时任务/脚本
 docker exec  project_php_1  php /www/ws.php
# docker情况下 宿主机查看端口连接情况
docker inspect -f '{{.State.Pid}}' <containerid> # note the PID
sudo nsenter -t <pid> -n netstat | grep ESTABLISHED

# 解决用dockerfile生成镜像慢的问题

添加 文件 /etc/docker/daemon.json（Linux）
{
  "registry-mirrors": ["https://navyf335.mirror.aliyuncs.com"]
}

启动        systemctl start docker
守护进程重启   sudo systemctl daemon-reload
重启docker服务   systemctl restart  docker
重启docker服务  sudo service docker restart
关闭docker service docker stop
关闭docker systemctl stop docker

