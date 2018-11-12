CPU:24核 
内存：32G 
硬盘：RAID5 SAS146G 15000转硬盘阵列
提示硬件配置没这么高不要设这么多！


# nginx conf conf/nginx.conf
# Created by http://www.wdlinux.cn
# Last Updated 2010.06.01
user  www www;
worker_processes  48;
error_log  logs/error.log  notice;
pid        logs/nginx.pid;
worker_rlimit_nofile 409600;
events {
    use epoll;
    worker_connections  409600;
}

http {
    include       mime.types;
    default_type  application/octet-stream;

    server_names_hash_bucket_size 128;
    client_header_buffer_size 32k;
    large_client_header_buffers 4 32k;
    client_max_body_size 8m;
    limit_conn_zone $binary_remote_addr zone=one:32k;

    fastcgi_cache_path /www/wdlinux/nginx/fastcgi_cache levels=1:2
                keys_zone=TEST:10m
                inactive=5m;
    fastcgi_connect_timeout 300;
    fastcgi_send_timeout 300;
    fastcgi_read_timeout 300;
    fastcgi_buffer_size 16k;
    fastcgi_buffers 16 16k;
    fastcgi_busy_buffers_size 16k;
    fastcgi_temp_file_write_size 16k;
    fastcgi_cache TEST;
    fastcgi_cache_valid 200 302 1h;
    fastcgi_cache_valid 301 1d;
    fastcgi_cache_valid any 1m;
    fastcgi_cache_min_uses 1;
    fastcgi_cache_use_stale error timeout invalid_header http_500;
    open_file_cache max=102400 inactive=20s;
    open_file_cache_valid 30s;
    open_file_cache_min_uses 1;

    sendfile        on;
    tcp_nopush     on;

    keepalive_timeout  60;
    tcp_nodelay on;

    gzip  on;
    gzip_min_length  1k;
    gzip_buffers     4 16k;
    gzip_http_version 1.0;
    gzip_comp_level 2;
    gzip_types       text/plain application/x-javascript text/css application/xml;
    gzip_vary on;

    log_format  wwwlogs  '$remote_addr - $remote_user [$time_local] $request $status $body_bytes_sent $http_referer $http_user_agent $http_x_forwarded_for';

    include vhost/*.conf;
}


php-fmp 设置

pm.max_children=512
pm.start_servers=128
pm.min_spare_servers=30
pm.max_spare_servers=128



worker_processes 8;
nginx进程数，建议按照cpu数目来指定，一般为它的倍数。比如4核，worker_processes就为8；

worker_rlimit_nofile 102400;
　　这个指令是指当一个nginx进程打开的最多文件描述符数目，理论值应该是最多打开文件数（ulimit -n）与nginx进程数相除，但是nginx分配请求并不是那么均匀，所以最好与ulimit -n的值保持一致。


pm.max_children：静态方式下开启的php-fpm进程数量。
pm.start_servers：动态方式下的起始php-fpm进程数量。
pm.min_spare_servers：动态方式下的最小php-fpm进程数量。
pm.max_spare_servers：动态方式下的最大php-fpm进程数量。

如果dm设置为static，那么只有pm.max_children这个参数生效。系统会开启设置的数个php-fpm进程。

如果dm设置为dynamic，那么pm.max_children参数失效，后面3个参数生效。系统会在php-fpm运行开始的时候启动 pm.start_servers个php-fpm进程，然后根据系统的需求动态在pm.min_spare_servers和 pm.max_spare_servers之间调整php-fpm进程数。

那么，对于我们的服务器，选择哪种执行方式比较好呢？事实上，跟Apache一样，我们运行的PHP程序在执行完成后，或多或少会有内存泄露的问 题。这也是为什么开始的时候一个php-fpm进程只占用3M左右内存，运行一段时间后就会上升到20-30M的原因了。所以，动态方式因为会结束掉多余 的进程，可以回收释放一些内存，所以推荐在内存较少的服务器或者VPS上使用。具体最大数量根据 内存/20M 得到。比如说512M的VPS，建议pm.max_spare_servers设置为20。至于pm.min_spare_servers，则建议根据服 务器的负载情况来设置，比较合适的值在5~10之间。

      然后对于比较大内存的服务器来说，设置为静态的话会提高效率。因为频繁开关php-fpm进程也会有时滞，所以内存够大的情况下开静态效果会更好。数量也可以根据 内存/30M 得到。比如说2GB内存的服务器，可以设置为50；4GB内存可以设置为100等。

      DEMO参数如下：

pm=dynamic
pm.max_children=20
pm.start_servers=5
pm.min_spare_servers=5
pm.max_spare_servers=20

      这样就可以最大的节省内存并提高执行效率。
