

### 使用 263 的yum 源
$ mv /etc/yum.repos.d/CentOS-Base.repo /etc/yum.repos.d/CentOS-Base.repo.backup
$ wget http://mirrors.163.com/.help/CentOS7-Base-163.repo -O /etc/yum.repos.d/CentOS7-Base-163.repo
$ rpm -Uvh https://mirror.webtatic.com/yum/el7/epel-release.rpm
$ rpm -Uvh https://mirror.webtatic.com/yum/el7/webtatic-release.rpm
$ yum clean all && yum makecache && yum -y update

### 设置时间同步，经常有服务器时间不对
$ yum install -y ntp telnet git
$ ntpdate time.nist.gov
$ crontab -e 
*/30 * * * * ntpdate time.nist.gov

### 安装 nginx
$ vi /etc/yum.repos.d/nginx.repo
[nginx]
name=nginx repo
baseurl=http://nginx.org/packages/centos/7/$basearch/
gpgcheck=0
enabled=1
$ yum install -y nginx
$ systemctl enable nginx.service && systemctl start nginx.service


$ yum install -y gcc gcc-c++ zlib-devel libuuid-devel pcre-devel libxml2 libxml2-devel curl curl-devel libjpeg libjpeg-devel libpng-devel freetype freetype-devel  mysql-devel pdo-mysql autoconf php71w php71w-fpm php71w-devel php71w-opcache php71w-pdo php71w-pdo_mysql php71w-pear php71w-mbstring php71w-pecl-mongodb php71w-gd

#### 修改 php.ini 时区信息 及 php-fpm的用户、用户组
$ sed -ie "s/;date.timezone =/date.timezone = Asia\/Chongqing/" /etc/php.ini
$ sed -ie "s/user = apache/user = nginx/" /etc/php-fpm.d/www.conf
$ sed -ie "s/group = apache/group = nginx/" /etc/php-fpm.d/www.conf



//如果nginx出现不能访问的权限问题，查看SELINUX是否关闭
//临时关闭
setenforce 0
//永久关闭，需要重启系统
$ vi /etc/selinux/config
SELINUX=disabled

#### 打开opcache
$ vi /etc/php.d/opcache.ini
//添加一行
zend_extension=opcache.so
//修改如下参数
opcache.enable=1
opcache.enable_cli=1
opcache.memory_consumption=64
opcache.file_cache=/tmp
opcache.huge_code_pages=1

$ cd /hwdata/www
$ wget https://raw.github.com/rlerdorf/opcache-status/master/opcache.php
$ wget https://raw.githubusercontent.com/amnuts/opcache-gui/master/index.php -O opcache-gui.php

$ systemctl enable php-fpm.service && systemctl start php-fpm.service

//创建项目目录
$ mkdir -p /hwdata/www
$ mkdir -p /hwdata/log/nginx
$ echo "<?php phpinfo();" > /hwdata/www/index.php

//拉取代码
$ git clone git@gitlab01.huan.tv:epgserver/laravel-epg.git

//修改nginx的配置
$ cp /etc/nginx/nginx.conf /etc/nginx/nginx.conf.backup
$ vi /etc/nginx/nginx.conf
user  nginx;
worker_processes auto;
worker_rlimit_nofile 102400;
error_log  /hwdata/log/nginx/error.log warn;
pid        /var/run/nginx.pid;

events {
    worker_connections  2048;
    multi_accept on;
    use epoll;
}
http { 
	server_tokens off;
    sendfile        on;
    tcp_nopush     on;
    tcp_nodelay on; 
    keepalive_timeout  20;
    client_header_timeout 20;
    client_body_timeout 20;
    reset_timedout_connection on;
    send_timeout 20;

    gzip  on;
    gzip_disable "msie6"; 
    gzip_proxied any; 
	gzip_min_length 1000; 
	gzip_comp_level 4; 
	gzip_types text/plain text/css application/json application/x-javascript text/xml application/xml application/xml+rss text/javascript; 
	open_file_cache max=100000 inactive=20s; 
    open_file_cache_valid 30s; 
    open_file_cache_min_uses 2; 
    open_file_cache_errors on; 
}

// 修改nginx的配置文件，下面是laravel的配置示例
server {
    listen       80 default_server;
    server_name  localhost;

    charset utf-8;
    root /hwdata/www/;
    access_log  /hwdata/log/nginx/localhost.access.log  main;
    error_log /hwdata/log/nginx/localhost.error.log;
    index index.php index.html index.htm;

    location / {
        try_files $uri $uri/ /index.php?$query_string;
    }

    error_page  404              /404.html;
    error_page   500 502 503 504  /50x.html;
    location = /50x.html {
        root   /usr/share/nginx/html;
    }

    location ~ \.php {
        fastcgi_pass 127.0.0.1:9000;
        fastcgi_index /index.php;
        include /etc/nginx/fastcgi_params;
        fastcgi_split_path_info       ^(.+\.php)(/.+)$;
        fastcgi_param PATH_INFO       $fastcgi_path_info;
        fastcgi_param PATH_TRANSLATED $document_root$fastcgi_path_info;
        fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;
    }
    
    location ~* \.(html|js|css|png|jpg|jpeg|gif|ico)$ {
        expires 24h;
        log_not_found off;
    }
    
    location = /favicon.ico {
        log_not_found off;
        access_log off;
    }
    
    location = /robots.txt {
        allow all;
        log_not_found off;
        access_log off;
    }
    
    location ~ /\.ht {
        deny all;
    }

}

server {
    listen       80 default_server;
    listen       [::]:80 default_server;
    server_name  _ weapp.huantest.com;

    charset utf-8;
    root /hwdata/www/laravel-epg/public;
    access_log  /var/log/nginx/localhost.access.log  main;
    error_log /var/log/nginx/localhost.error.log;
    index index.php index.html index.htm;

    location / {
        try_files $uri $uri/ /index.php?$query_string;
    }

    error_page  404              /404.html;
    error_page   500 502 503 504  /50x.html;
    location = /50x.html {
        root   /usr/share/nginx/html;
    }

    location ~ \.php {
        fastcgi_pass 127.0.0.1:9000;
        fastcgi_index /index.php;
        include /etc/nginx/fastcgi_params;
        fastcgi_split_path_info       ^(.+\.php)(/.+)$;
        fastcgi_param PATH_INFO       $fastcgi_path_info;
        fastcgi_param PATH_TRANSLATED $document_root$fastcgi_path_info;
        fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;
    }

    location ~* \.(html|js|css|png|jpg|jpeg|gif|ico)$ {
        $ expires 24h;
        log_not_found off;
    }

    location = /favicon.ico {
        log_not_found off;
        access_log off;
    }

    location = /robots.txt {
        allow all;
        log_not_found off;
        access_log off;
    }

    location ~ /\.ht {
        deny all;
    }
}





$ vi /etc/nginx/conf.d/default.conf
server {
    listen 80;
    server_name _;
    charset utf-8;
    error_log /var/log/nginx/laravel_error.log;
    access_log /var/log/nginx/laravel_access.log;

    root /hwdata/www/laravel/public;
    index index.php index.html index.htm;

    location / {
        try_files $uri $uri/ /index.php?$query_string;
    }

    location ~ \.php {
    	try_files $uri =404;
        fastcgi_pass 127.0.0.1:9000;
        fastcgi_index index.php;
        include /etc/nginx/fastcgi_params;
        fastcgi_param  SCRIPT_FILENAME    $document_root$fastcgi_script_name;
        fastcgi_param  SCRIPT_URI         http://$host$request_uri;
    }
    
    location ~* \.(html|js|css|png|jpg|jpeg|gif|ico)$ {
        expires 24h;
        log_not_found off;
    }
    
    location = /favicon.ico {
        log_not_found off;
        access_log off;
    }
    
    location = /robots.txt {
        allow all;
        log_not_found off;
        access_log off;
    }
    
    location ~ /\.ht {
        deny all;
    }
}

// symfony 2.0的配置示例
$ vi /etc/nginx/conf.d/default.conf 
server {
    server_name domain.tld www.domain.tld;
    root /hwdata/www;
    location / {
        try_files $uri /hwdata.php$is_args$args;
    }
    $ PROD
    location ~ ^/hwdata\.php(/|$) {
    	fastcgi_pass unix:/var/run/php5-fpm.sock;
        fastcgi_split_path_info ^(.+\.php)(/.*)$;
        include fastcgi_params;
       	fastcgi_param SCRIPT_FILENAME $realpath_root$fastcgi_script_name;
       	fastcgi_param DOCUMENT_ROOT $realpath_root;
       	internal;
   }
   $ return 404 for all other php files not matching the front controller
   $ this prevents access to other php files you don't want to be accessible.
   location ~ \.php$ {
     return 404;
   }
   error_log /var/log/nginx/project_error.log;
   access_log /var/log/nginx/project_access.log;
}


$ yum install -y redis
$ systemctl enable redis && systemctl start redis

//安装node
curl --silent --location https://rpm.nodesource.com/setup_6.x | bash -
yum -y install nodejs

//安装python工具easy_install
//升级本地python版本为2.7.9以上，所以安装2.7.14
$ cd /data/src
$ wget https://www.python.org/ftp/python/2.7.14/Python-2.7.14.tgz
$ tar zxvf Python-2.7.14.tgz
$ cd Python-2.7.14
$ ./configure --prefix=/usr/local
$ make && make install
$ rm -rf /usr/bin/python
$ ln -s /usr/local/bin/python2.7 /usr/bin/python

// 升级完成后，修改yum，yum还不支持2.7.14，所以还有系统自动的2.7.5
$ vi /usr/bin/yum
  $! /usr/bin/python2
$ vi /usr/libexec/urlgrabber-ext-down
  $! /usr/bin/python2

// 安装setuptools  
$ wget --no-check-certificate https://pypi.python.org/packages/source/s/setuptools/setuptools-1.4.2.tar.gz
$ tar -xvf setuptools-1.4.2.tar.gz
$ cd setuptools-1.4.2
$ python setup.py install
$ easy_install --version
### 安装supervisor
$ easy_install supervisor

// 生成配置文件
$ echo_supervisord_conf > /etc/supervisord.conf


// 修改配置里面的 ，/tmp/supervisor.sock, 会有可能被系统删除
[unix_http_server]
file=/var/run/supervisor.sock
[supervisorctl]
serverurl=unix:///var/run/supervisor.sock

//使用redhat-init-mingalevme 将 supervisor 加为启动项
$ vi /lib/systemd/system/supervisord.service
  [Unit]
  Description=Supervisor daemon

  [Service]
  Type=forking
  ExecStart=/usr/local/bin/supervisord
  ExecStop=/usr/local/bin/supervisorctl $OPTIONS shutdown
  ExecReload=/usr/local/bin/supervisorctl $OPTIONS reload
  KillMode=process
  Restart=on-failure
  RestartSec=42s
  
  [Install]
  WantedBy=multi-user.target
$ systemctl daemon-reload
$ systemctl enable supervisord.service
$ systemctl start supervisord  

$ vi /data/ssh/test.php
<?php
while(1) {
  file_put_contents("/data/ssh/test.log.".date("Y-m-d"), date("H:i:s").PHP_EOL, FILE_APPEND);
  sleep(60);
} 

$ vi /etc/supervisord.conf 
$ shift + g
[program:test]
command=php /data/ssh/test.php              
process_name=%(program_name)s
numprocs=1
autostart=true

[program:laravel-epg-work]
command=/usr/bin/php /hwdata/www/laravel-epg/artisan queue:work --tries=3 --timeout=60 --sleep=2
process_name=%(program_name)s_%(process_num)02d
autostart=true
autorestart=true
numprocs=2
stdout_logfile=/hwdata/www/laravel-epg/storage/logs/worker.log

$ systemctl start supervisord.service

//安装mysql5.7
$ yum install -y http://dev.mysql.com/get/mysql57-community-release-el7-9.noarch.rpm
$ yum install -y mysql mysql-devel mysql-server mysql-utilities
$ systemctl enable mysqld  && systemctl start mysqld
$ cat /var/log/mysqld.log |grep password
2017-04-26T09:56:28.542837Z 1 [Note] A temporary password is generated for root@localhost: l2tV$z1dytI/
// root@localhost 后面的就是密码
// 修改mysql密码，mysql5.7之后密码必须满足大小写加数字才可以。
$ /usr/bin/mysql_secure_installation
// 输入刚刚cat的密码，修改新密码，同时清除test库等。

Y/ftHp4<x*9t

其实想要重置 5.7 的密码很简单，就一层窗户纸：
1、修改 /etc/my.cnf，在 [mysqld] 小节下添加一行：skip-grant-tables=1
这一行配置让 mysqld 启动时不对密码进行验证
2、重启 mysqld 服务：systemctl restart mysqld
3、使用 root 用户登录到 mysql：mysql -u root 
4、切换到mysql数据库，更新 user 表：
use mysql;
update user set authentication_string = password('Portalhuan$123'), password_expired = 'N', password_last_changed = now() where user = 'root';
5、退出 mysql，编辑 /etc/my.cnf 文件，删除 skip-grant-tables=1 的内容
6、重启 mysqld 服务，再用新密码登录即可

创建数据库，添加用户，注意5.7以后密码加了强验证，密码必须保证有大小写字母和数字。
mysql> create database `laravel-epg` default character set utf8;
mysql> CREATE USER 'weather'@'10.35.36.16' IDENTIFIED BY 'Portalhuan#123';
mysql> GRANT ALL PRIVILEGES ON `weather`.* TO 'weather'@'10.35.36.16' WITH GRANT OPTION;
mysql> FLUSH PRIVILEGES ;

CREATE USER 'tvq'@'%' IDENTIFIED BY 'TvqOwner$123';
GRANT ALL PRIVILEGES ON `tvq`.* TO 'tvq'@'%' WITH GRANT OPTION;


/etc/yum.repos.d/mongodb-org-3.4.repo
[mongodb-org-3.4]
name=MongoDB Repository
baseurl=https://repo.mongodb.org/yum/redhat/$releasever/mongodb-org/3.4/x86_64/
gpgcheck=1
enabled=1
gpgkey=https://www.mongodb.org/static/pgp/server-3.4.asc
$ yum install -y mongodb-org
$ echo never > /sys/kernel/mm/transparent_hugepage/enabled
$ echo never > /sys/kernel/mm/transparent_hugepage/defrag
$ systemctl enable mongod.service 
$ systemctl start mongod

$ vi /etc/security/limits.d/99-mongodb-nproc.conf 
mongod soft nofile 65535  
mongod hard nofile 65535  
mongod soft nproc 32768 
mongod hard nproc 32768 

//设置安全
$ vi /etc/mongod.conf
security:
  authorization: enabled
$ mongo  
use admin
db.createUser({user:"admin",pwd:"Portalhuan$123",roles:[{role:"userAdminAnyDatabase",db:"admin"}]})
db.createUser({user:"root",pwd:"Portalhuan$123",roles:[{role:"root",db:"admin"}]})
use laravel-epg
db.createUser({user:"epg",pwd: "epgowner$123",roles:[{role:"dbOwner",db:"laravel-epg" }]})
db.createUser({user:"firstcup001",pwd: "Firstcup$123",roles:[{role:"dbOwner",db:"firstcup001" }]})
$ service mongod restart

firewall-cmd --add-port=27017/tcp --permanent


ab -n 10000 -c 100 http://139.196.84.20/info.php

Concurrency Level:      100
Time taken for tests:   38.839 seconds
Complete requests:      10000
Failed requests:        9972
   (Connect: 0, Receive: 0, Length: 9972, Exceptions: 0)
Write errors:           0
Total transferred:      866760611 bytes
HTML transferred:       865390337 bytes
Requests per second:    257.47 [$/sec] (mean)
Time per request:       388.388 [ms] (mean)
Time per request:       3.884 [ms] (mean, across all concurrent requests)
Transfer rate:          21793.80 [Kbytes/sec] received

Connection Times (ms)
              min  mean[+/-sd] median   max
Connect:        1    4  48.9      1    1003
Processing:     5  329 806.7     29   26082
Waiting:        2   24 138.7      3    9024
Total:          6  333 808.8     31   26085

Percentage of the requests served within a certain time (ms)
  50%     31
  66%    225
  75%    257
  80%    444
  90%    849
  95%   1449
  98%   2490
  99%   3297
 100%  26085 (longest request)


关闭防火墙
systemctl stop firewalld
systemctl disable firewalld




certbot certonly --webroot -w /hwdata/www/laravel-epg/public -d weapp.huantest.com
