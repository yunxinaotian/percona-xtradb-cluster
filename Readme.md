#安装docker
```
yum -y update
yum install -y docker
#启动docker
service docker start
```
#获取percona-xtradb-cluster镜像
```
docker pull percona/percona-xtradb-cluster:5.7.21
#名称太长，指定个名称
docker tag percona/percona-xtradb-cluster:5.7.21 pxc
#删除原有镜像
docker rmi percona/percon-xtradb-cluster:5.7.21
```
#创建虚拟网络，linux支持虚拟网络
```
docker network create --subnet=172.18.0.0/24 net1
#查看网路命令
docker inspect net1
#删除网络命令
docker network rm net1
```
#创建卷,把数据映射到主机上
```
docker volume create --name v1
#删除卷命令
docker volume rm v1

docker volume create --name v2
docker volume create --name v3
docker volume create --name v4
docker volume create --name v5

```
#创建pxc容器
```
docker run -d -p 3306:3306 -v v1:/var/lib/mysql -e MYSQL_ROOT_PASSWORD=abc123456 -e CLUSTER_NAME=PXC -e XTRABACKUP_PASSWORD=abc123456 --privileged --name=node1 --net=net1 --ip 172.18.0.2 pxc
#等待node1创建完成后用客户端连接到数据库再创建下面的节点
docker ps 
docker run -d -p 3307:3306 -v v2:/var/lib/mysql -e MYSQL_ROOT_PASSWORD=abc123456 -e CLUSTER_NAME=PXC -e XTRABACKUP_PASSWORD=abc123456 -e CLUSTER_JOIN=node1 --privileged --name=node2 --net=net1 --ip 172.18.0.3 pxc
docker run -d -p 3308:3306 -v v3:/var/lib/mysql -e MYSQL_ROOT_PASSWORD=abc123456 -e CLUSTER_NAME=PXC -e XTRABACKUP_PASSWORD=abc123456 -e CLUSTER_JOIN=node1 --privileged --name=node3 --net=net1 --ip 172.18.0.4 pxc
docker run -d -p 3309:3306 -v v4:/var/lib/mysql -e MYSQL_ROOT_PASSWORD=abc123456 -e CLUSTER_NAME=PXC -e XTRABACKUP_PASSWORD=abc123456 -e CLUSTER_JOIN=node1 --privileged --name=node4 --net=net1 --ip 172.18.0.5 pxc
docker run -d -p 3310:3306 -v v5:/var/lib/mysql -e MYSQL_ROOT_PASSWORD=abc123456 -e CLUSTER_NAME=PXC -e XTRABACKUP_PASSWORD=abc123456 -e CLUSTER_JOIN=node1 --privileged --name=node5 --net=net1 --ip 172.18.0.6 pxc
```
