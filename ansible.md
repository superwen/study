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
