参考
https://pawelurbanek.com/elk-nginx-logs-setup

```
mkdir /hwdata/data/elk-certs
cd /hwdata/data/elk-certs/
openssl req -subj '/CN=myelk.huantest.com/' -x509 -days 3650 -batch -nodes -newkey rsa:2048 -keyout elk-ssl.key -out elk-ssl.crt
chown logstash elk-ssl.crt
chown logstash elk-ssl.key


// 安装 jdk
$ yum -y install java-1.8.0-openjdk  java-1.8.0-openjdk-devel
//  yum install -y java-1.8.0-openjdk-headless ???

$ curl -L -O https://artifacts.elastic.co/downloads/elasticsearch/elasticsearch-6.3.2.rpm
$ curl -L -O https://artifacts.elastic.co/downloads/logstash/logstash-6.3.2.rpm
$ curl -L -O https://artifacts.elastic.co/downloads/kibana/kibana-6.3.2-x86_64.rpm
$ curl -L -O https://artifacts.elastic.co/downloads/beats/filebeat/filebeat-6.3.2-x86_64.rpm
$ rpm --import https://artifacts.elastic.co/GPG-KEY-elasticsearch
$ yum localinstall elasticsearch-6.3.2.rpm -y
$ yum localinstall logstash-6.3.2.rpm -y
$ yum localinstall kibana-6.3.2-x86_64.rpm -y
$ yum localinstall filebeat-6.3.2-x86_64.rpm -y

mkdir -p /hwdata/data/elasticsearch
chown -R elasticsearch:elasticsearch  /hwdata/data/elasticsearch
mkdir -p /hwdata/log/elasticsearch
chown -R elasticsearch:elasticsearch /hwdata/log/elasticsearch

// 修改配置文件
vi /etc/elasticsearch/elasticsearch.yml 

$ systemctl daemon-reload
$ systemctl enable elasticsearch.service
$ systemctl start elasticsearch.service
$ systemctl enable logstash.service
$ systemctl start logstash.service
$ systemctl enable kibana.service
$ systemctl start kibana.service

netstat -nltp | grep java
firewall-cmd --permanent --add-port={9200/tcp,9300/tcp}
firewall-cmd --reload
firewall-cmd  --list-all


//
$ vi /etc/filebeat/filebeat.yml

// 测试elasticsearch是否安装成功
$ curl 'http://127.0.0.1:9200/?pretty'


yum -y install httpd
htpasswd -bc /etc/nginx/conf.d/htpasswd.users admin Portalhuan#123
htpasswd -c /etc/nginx/htpasswd.users kibanaadmin

server {
    listen 8080;
    server_name mykibana.test.com;
    auth_basic "Restricted Access";
    auth_basic_user_file /etc/nginx/conf.d/htpasswd.users;
    location / {
        proxy_pass http://127.0.0.1:5601;
        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection 'upgrade';
        proxy_set_header Host $host;
        proxy_cache_bypass $http_upgrade;
    }
}

/usr/share/elasticsearch/bin/elasticsearch-keystore create


/usr/share/elasticsearch/bin/elasticsearch-plugin install ingest-geoip
/usr/share/elasticsearch/bin/elasticsearch-plugin install ingest-user-agent

// 安装x-pack
$ /usr/share/elasticsearch/bin/elasticsearch-plugin install x-pack
$ /usr/share/kibana/bin/kibana-plugin install x-pack
$ /usr/share/logstash/bin/logstash-plugin install x-pack  ？？？？？
// 或者离线安装
$ wget  https://artifacts.elastic.co/downloads/packs/x-pack/x-pack-5.2.0.zip -O /hwdata/src/x-pack-5.2.0.zip
$ /usr/share/elasticsearch/bin/elasticsearch-plugin install file:///hwdata/src/x-pack-5.2.0.zip
$ /usr/share/kibana/bin/kibana-plugin install file:///hwdata/src/x-pack-5.2.0.zip
$ /usr/share/logstash/bin/logstash-plugin install file:///hwdata/src/x-pack-5.2.0.zip

// x-pack安装之后有一个超级用户elastic ，其默认的密码是changeme

curl -XPUT -u elastic '103.235.237.107:9200/_xpack/security/user/elastic/_password' -d '{
  "password" : "portalhuan#123"
}'

curl -XPUT -u elastic '103.235.237.107:9200/_xpack/security/user/kibana/_password' -d '{
  "password" : "portalhuan#123"
}'

/usr/share/elasticsearch/bin/x-pack/syskeygen
/etc/elasticsearch/x-pack/system_key

input {
  file {
    path => '/hwdata/logs/gouzheng/*.txt'
    start_position => "beginning"
  }
}  
filter {
  csv {
    columns => ["device_id", "mac4"]
    skip_empty_columnsedit => true
  }
}
output {
    elasticsearch {
        hosts => [ "localhost:9200" ]
        workers => 5
    }
}


、、、、、、、、、、
input {
  file {
    path => "/tmp/*_log"
  }
}

filter {
  if [path] =~ "access" {
    mutate { replace => { type => "apache_access" } }
    grok {
      match => { "message" => "%{COMBINEDAPACHELOG}" }
    }
    date {
      match => [ "timestamp" , "dd/MMM/yyyy:HH:mm:ss Z" ]
    }
  } else if [path] =~ "error" {
    mutate { replace => { type => "apache_error" } }
  } else {
    mutate { replace => { type => "random_logs" } }
  }
}

output {
  elasticsearch { hosts => ["localhost:9200"] }
  stdout { codec => rubydebug }
}

htpasswd -bc /etc/nginx/conf.d/htpasswd.users admin portalhuan#123

server {
    listen 80;
    server_name mykibana.test.com;    #当前主机名
    auth_basic "Restricted Access";
    auth_basic_user_file /etc/nginx/conf.d/htpasswd.users;
    location / {
    	proxy_pass http://127.0.0.1:5601;
   		proxy_http_version 1.1;
    	proxy_set_header Upgrade $http_upgrade;
    	proxy_set_header Connection 'upgrade';
    	proxy_set_header Host $host;
    	proxy_cache_bypass $http_upgrade;
    }
}


 https://grokdebug.herokuapp.com/
 
vim /etc/sysctl.conf 添加如下配置，sysctl -p /etc/sysctl.conf 配置生效
vm.max_map_count=655360

 vi /etc/security/limits.conf 修改 文件描述符限制：
elasticsearch    soft    nofile    65536
elasticsearch    hard    nofile    65536

vim /etc/security/limits.d/90-nproc.conf 如果文件存在，这里也需要修改
* soft nofile 65536
* hard nofile 65536

异常现象：启动ElasticSearch，报下面异常
[1]: max virtual memory areas vm.max_map_count [65530] is too low, increase to at least [262144]

解决：
1、修改max_map_count值
$ sudo sysctl -w vm.max_map_count=262144
2、查看是否修改为262144
$ more /proc/sys/vm/max_map_count
262144
3、重新启动ElasticSearch

安装 Cerebro
Cerebro是一个第三方的 Elasticsearch 集群管理软件，可以方便地查看集群状态：
下载地址： https://github.com/lmenezes/cerebro
启动进程后，可以在浏览器查看：
bin/cerebro -Dhttp.port=1234 -Dhttp.address=127.0.0.1

# elasticsearch 配置
cluster.name: logs_production    # 集群名称，同名称为集群节点
node.name: node102_01            # 节点名称，默认会随机取名称
node.master: true                # 是否为master节点
node.data: true                 # 是否为数据节点
path.data: /data/01,/data/02     # 如果在path.data中写多个目录用“，”分隔，功能类似于raid 0，而不是做备份
network.host: 10.100.0.102      # 监听主机
http.port: 9201                  # 监听端口
transport.tcp.port: 9301         # 传输端口
transport.tcp.compress: true
# 设置集群中master节点的初始列表，可以通过这些节点来自动发现新加入集群的节点
discovery.zen.ping.unicast.hosts: ["10.100.0.101:9301", "10.100.0.102:9301","10.100.0.103:9301"]
cluster.routing.allocation.same_shard.host: true



curl -L -O https://artifacts.elastic.co/downloads/beats/metricbeat/metricbeat-6.4.0-x86_64.rpm
sudo rpm -vi metricbeat-6.4.0-x86_64.rpm
/etc/metricbeat/metricbeat.yml

metricbeat modules enable|disable apache mysql
metricbeat modules list

metricbeat setup --dashboards

systemctl enable metricbeat
systemctl start metricbeat

http://localhost:9200/metricbeat-*/_search?pretty


Filebeat日志：
tail -f /var/log/filebeat/filebeat
Logstash日志：
tail -f /var/log/logstash/logstash-plain.log

```
