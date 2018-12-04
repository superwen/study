
### 利用docker安装
拉取image  
```
docker pull gomods/athens:canary
```
创建数据目录
```
mkdir -p /hwdata/data/athens-storage
```
添加环境变量  
```
vi /etc/profile
  export ATHENS_STORAGE=/hwdata/data/athens-storage
source /etc/profile  
```
运行  
```
docker run -d -v $ATHENS_STORAGE:/var/lib/athens \
   -e ATHENS_DISK_STORAGE_ROOT=/var/lib/athens \
   -e ATHENS_STORAGE_TYPE=disk \
   --name athens-proxy \
   --restart always \
   -p 3000:3000 \
   gomods/athens:canary
```
