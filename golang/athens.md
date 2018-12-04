
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
在开发的机器上，设置go的环境  
```
export GO111MODULE=on
export GOPROXY=http://proxyserver:3000
```
再go get 包的时候，就会走代理服务器  
这个时候可以在proxyserver上查看日志  
```
docker logs athens-proxy
```
如下
```
time="2018-12-04T03:29:12Z" level=error msg="exit status 1: go: finding golang.org/x/net/html latest\ngo list -m golang.org/x/net/html: no matching versions for query \"latest\"\n" http-method=GET http-path=/golang.org/x/net/html/@v/list/ http-url=/golang.org/x/net/html/@v/list/ kind="Internal Server Error" module= operation=download.ListHandler ops="[download.ListHandler pool.List protocol.List vcsLister.List]" version=
handler: GET /golang.org/x/net/html/@v/list/ [500]
handler: GET /golang.org/x/net/@v/list/ [200]
handler: GET /golang.org/x/net/@latest/ [200]
handler: GET /golang.org/x/net/@v/v0.0.0-20181201002055-351d144fa1fc.zip [200]
handler: GET /golang.org/x/net/@v/v0.0.0-20181201002055-351d144fa1fc.mod [200]
```
