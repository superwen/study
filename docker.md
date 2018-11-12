# CCentos7 下安装docker

yum update
yum install -y yum-utils device-mapper-persistent-data lvm2
yum-config-manager --add-repo https://download.docker.com/linux/centos/docker-ce.repo
yum list docker-ce --showduplicates | sort -r

yum install docker-ce
systemctl start docker
systemctl enable docker
docker version



curl -L https://github.com/docker/compose/releases/download/1.20.1/docker-compose-`uname -s`-`uname -m` -o /usr/local/bin/docker-compose

curl -L https://github.com/docker/compose/releases/download/1.22.0/docker-compose-Linux-x86_64 -o /usr/local/bin/docker-compose




如果之前安装过docker，删除docker的方法：
yum remove docker docker-common docker-selinux docker-engine -y
/etc/systemd -name '*docker*' -exec rm -f {} ;
find /etc/systemd -name '*docker*' -exec rm -f {} \;
find /lib/systemd -name '*docker*' -exec rm -f {} \;












#确定centos内核
$ uname -r
3.10.0-229.el7.x86_64

#yum update
$ yum -y update

#添加源
$ vi /etc/yum.repos.d/docker.repo
[dockerrepo]
name=Docker Repository
baseurl=https://yum.dockerproject.org/repo/main/centos/7/
enabled=1
gpgcheck=1
gpgkey=https://yum.dockerproject.org/gpg

$ sudo yum install -y docker-engine
$ sudo systemctl start docker





$ docker pull centos:latest
$ docker run -t -i centos /bin/bash

$ docker ps -a
$ docker images
$ docker stop ****
$ docker rm ****
$ docker stop `docker ps -a -q`
$ docker rm `docker ps -a -q`

$ docker run -d --name web-dev-container -h web-dev-container -p 80:80 -v /usr/share/nginx/html/:/usr/share/nginx/html goriparthi/centos7-nginx



$ docker pull mysql:5.7
$ docker run --name mysql01 -e MYSQL_ROOT_PASSWORD=portalhuan -d -p 3306:3306 mysql:5.7
# 通过docker运行mysql命令行
$ docker run -it --link some-mysql:mysql --rm mysql sh -c 'exec mysql -h"$MYSQL_PORT_3306_TCP_ADDR" -P"$MYSQL_PORT_3306_TCP_PORT" -uroot -p"$MYSQL_ENV_MYSQL_ROOT_PASSWORD"'


docker run -it --link mysql:mysql --rm mysql sh -c 'exec mysql -h"$MYSQL_PORT_3306_TCP_ADDR" -P"$MYSQL_PORT_3306_TCP_PORT" -uroot -pportalhuan'
# 配置mysql
-v /hwdata/config/mysql:/etc/mysql/conf.d
# 存储数据
$ docker run --name mysql01 -e MYSQL_ROOT_PASSWORD=portalhuan -d -p 3306:3306 -v /hwdata/data/mysql:/var/lib/mysql  mysql:5.7
# 备份数据
$ docker exec mysql01 sh -c 'exec mysqldump --all-databases -uroot -p"$MYSQL_ROOT_PASSWORD"' > /hwdata/bak/mysql/all-databases.sql
$ docker exec mysql01 sh -c 'exec mysqldump epg -uroot -pportalhuan' > /hwdata/bak/mysql/epg.sql

$ docker pull redis
$ docker run --name redis01 -d redis -p 6379:6379
#持久化
$ docker run --name redis01 -d redis redis-server -p 6379:6379 --appendonly yes

$ docker run -d --name redis -p 6379:6379 bitnami/redis:latest
$ docker pull bitnami/redis:latest


$ docker pull tutum/mongodb:3.2
$ docker run -d -p 27017:27017 -p 28017:28017 tutum/mongodb
$ docker run -d -p 27017:27017 -p 28017:28017 -e MONGODB_PASS="protalhuan" tutum/mongodb
$ docker run -d -p 27017:27017 -p 28017:28017 -e MONGODB_USER="user" -e MONGODB_DATABASE="mydatabase" -e MONGODB_PASS="mypass" tutum/mongodb
$ docker run -d -p 27017:27017 -p 28017:28017 -e AUTH=no tutum/mongodb



https://github.com/docker/compose/releases/download/1.8.1/docker-compose-Linux-x86_64

$ curl -L https://github.com/docker/compose/releases/download/1.1.0/docker-compose-`uname -s`-`uname -m` > /usr/local/bin/docker-compose
$ chmod +x /usr/local/bin/docker-compose
$ docker-compose --version

docker-compose.yml

	version: '2'
    services:
      web:
        build: .
        ports:
          - "5000:5000"
        volumes:
          - .:/code
        depends_on:
          - redis
		environment:
  		  RACK_ENV: development
  		  SHOW: 'true'
  	      SESSION_SECRET:
      redis:
        image: redis
      db:
        image: postgres  
        

$ docker-compose up


Dockerfile

FROM centos:

# Install Nginx
ADD nginx.repo /etc/yum.repos.d/
RUN cd /tmp && \
  curl -O http://nginx.org/keys/nginx_signing.key && \
  rpm --import nginx_signing.key && \
  yum update -y && \
  yum install -y nginx

# Clean Up
RUN yum clean all && rm -rf /tmp/nginx*

# Replace default nginx with 101-Nginx
RUN rm -rf /etc/nginx
ADD 101-nginx /etc/nginx

EXPOSE 80 443

CMD ["nginx", "-g", "daemon off;"]



========== start.sh ======
#!/usr/bin/env bash

# Tweak nginx to match the workers to cpu's
procs=$(cat /proc/cpuinfo |grep processor | wc -l)
sed -i -e "s/worker_processes auto/worker_processes $procs/" /etc/nginx/nginx.conf

# execute local startup script if one is available
if [ -f "/docker/start.sh" ]; then
    chmod +x /docker/start.sh
    /docker/start.sh
fi

# Start supervisord and services
/usr/bin/supervisord -n -c /etc/supervisord.conf












升级centos内核
rpm --import https://www.elrepo.org/RPM-GPG-KEY-elrepo.org
rpm -Uvh http://www.elrepo.org/elrepo-release-6-6.el6.elrepo.noarch.rpm
yum --enablerepo=elrepo-kernel install kernel-lt -y
vim /etc/grub.conf
确认刚刚安装的内核的位置，然后将default修改一下。改为0

CentOS6 下安装
$ sudo yum install http://mirrors.yun-idc.com/epel/6/i386/epel-release-6-8.noarch.rpm
$ sudo yum install docker-io
CentOS7 下安装
$ sudo yum install docker

注册和启动服务
$ sudo service docker start
$ sudo chkconfig docker on

获取镜像
$ sudo docker pull ubuntu:12.04
相当于
$ sudo docker pull registry.hub.docker.com/ubuntu:12.04
使用别的注册服务器
$ sudo docker pull dl.dockerpool.com:5000/ubuntu:12.04

创建容器
$ sudo docker run -t -i ubuntu:12.04 /bin/bash
$ sudo docker run -t -i ubuntu:14.04 /bin/bash

列出本地镜像
$ sudo docker images

上传镜像
$ sudo docker push ouruser/sinatra

存数镜像
$ sudo docker save -o ubuntu_14.04.tar ubuntu:14.04

载入镜像
$ sudo docker load --input ubuntu_14.04.tar
或者
$ sudo docker load < ubuntu_14.04.tar

移除本地镜像
$ sudo docker rmi training/sinatra



移除所有的容器和镜像
docker kill $(docker ps -q) ; docker rm $(docker ps -a -q) ; docker rmi $(docker images -q -a)

删除所有的容器
docker kill $(docker ps -q) ; docker rm $(docker ps -a -q) 
