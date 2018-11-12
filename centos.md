服务端创建可sudo用户，并且免密码    
```
ssh user@ssh-server
useradd -d /home/superwen -m superwen
passwd superwen
echo "superwen ALL = (root) NOPASSWD:ALL" | sudo tee /etc/sudoers.d/superwen
chmod 0440 /etc/sudoers.d/superwen
```

客户端允许无密码 SSH 登录，创建key  
```
ssh-keygen -t rsa -C "superwen@gmail.com"
ssh-copy-id superwen@ssh-server
```

升级内核版本  
```
$ rpm -import https://www.elrepo.org/RPM-GPG-KEY-elrepo.org
$ rpm -Uvh http://www.elrepo.org/elrepo-release-7.0-3.el7.elrepo.noarch.rpm
$ yum -y --enablerepo=elrepo-kernel install kernel-ml
$ vim /etc/default/grub
GRUB_DEFAULT=0
$ grub2-mkconfig -o /boot/grub2/grub.cfg
//验证
$ grub2-editenv list
// 重启
$ shutdown -r now
// 重启后查看内核版本是不是4.11
$ uname -r
```

如果出现  Loaded plugins: fastestmirror Please use /usr/bin/yum --help 错误  
```
$ vi /etc/yum/pluginconf.d/fastestmirror.conf    
enabled=0  //把1改为0  
$ vi /etc/yum.conf
plugins=0                 #将plugins的值1修改为0
$ yum clean all
```

wget https://git.kernel.org/torvalds/t/linux-4.12-rc5.tar.gz --no-check-certificate
tar –xvf linux-4.12-rc5.tar.gz
cd linux-4.12
yum install hmaccalc zlib-develbinutils-devel elfutils-libelf-devel

// yum-config-manager --enable epel-testing 
// yum-config-manager --disable epel-testing 
// yum update --enablerepo=epel-testing
// yum install <foo> --enablerepo=epel-testing
// yum install epel-release

yum makecache fast
