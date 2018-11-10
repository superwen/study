## Ansible 使用笔记
复制文件  
`ansible hosts -m copy -a "src=/etc/hosts dest=/tmp/hosts"`  
修改文件属性  
`ansible hosts -m file -a "dest=/srv/foo/b.txt mode=600 owner=mdehaan group=mdehaan"`  
循环创建文件夹，-p  
`ansible hosts -m file -a "dest=/path/to/c mode=755 owner=mdehaan group=mdehaan state=directory"`  
删除文件  
 `ansible hosts -m file -a "dest=/path/to/c state=absent"`  

包管理  
`ansible hosts -m yum -a "name=supervisor state=present"`  

## playbook 常用
```
- hosts: tt
  vars:
    http_port: 80
    max_clients: 200
  become: yes
  tasks:
  - name: install_nginx
    yum: pkg=nginx state=latest
  - name: start_nginx
    service: name=nginx state=started
  - name: install_supervisor
    yum: pkg=supervisor state=latest
  - name: config_supervisor
    command: echo_supervisord_conf /etc/supervisord.conf  
  - name: start_supervisor
    service: name=supervisord state=started  
```

其他的一些常用moudle
+ 用wget下载  
`get_url: url=http://**** dest=/srv/**** sha256sum="****"`   
+ 添加group
`group: name=wordpress`  
+ 添加用户
`user: name=wordpress group=wordpress home=/srv/wordpress/`
+ 创建数据库
`mysql_db: name={{ wp_db_name }} state=present`  
+ 添加数据库用户
`mysql_user: name={{ dbuser }} password={{ dbpassword }} priv={{ dbname }}.*:ALL host='localhost' state=present`  
+ 文件魔板
`template: src=wp-config.php dest=/srv/wordpress/`  
