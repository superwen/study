## 我整理的GO语言相关学习资料

### 安装
```
tar -C /usr/local -xzf go1.10.2.linux-amd64.tar.gz
mkdir -p /app/go1.10/src/github.com/huannet
export GOROOT=/usr/local/go
export GOPATH=/app/go1.10
PATH=$PATH:/usr/local/go/bin:/app/go1.10/bin
```

### 交叉编译
```
CGO_ENABLED=0 GOOS=linux GOARCH=amd64 go build {package_path}
```

### 包管理
go1.11以后使用go mod来管理包，但是对于被墙的问题，可以使用proxy工具athens  
https://github.com/gomods/athens  
具体使用方法见[golang/athens.md](golang/athens.md)  
vgo的使用方法 https://github.com/wuyumin/vgo/blob/master/docs/zh-CN.md  

### 推荐框架  
web框架 https://github.com/kataras/iris *****  
比较热的web框架revel https://revel.github.io/  *****

### 热门库整理
redis库 https://github.com/gomodule/redigo  
orm不用多说 github.com/jinzhu/gorm *****  
mongoDb客户端 https://github.com/globalsign/mgo  
Etcd客户端 https://github.com/etcd-io/etcd  
常用的str工具集 https://github.com/mgutz/str  
好用的日志操作 https://github.com/sirupsen/logrus  
kafka客户端 https://github.com/Shopify/sarama  
强烈推荐json解析库 https://github.com/json-iterator/go  *****  
crontab解析库 https://github.com/gorhill/cronexpr  
好的captcha库 https://github.com/dchest/captcha  
YAML解析库 https://github.com/go-yaml/yaml  
基于内存的cache库 https://github.com/patrickmn/go-cache  
命令行工具 https://github.com/urfave/cli *****   
配置加载库 https://github.com/jinzhu/configor *****  
类似jquery的dom解析库 https://github.com/PuerkitoBio/goquery  
阿里云oss https://github.com/aliyun/aliyun-oss-go-sdk  
模型验证 gopkg.in/go-playground/validator.v9（iris推荐使用这个）*****     
模型验证 github.com/asaskevich/govalidator  

### 热门项目
国人用go开发的git server *****  
https://github.com/gogs/gogs  
Macaron 是一个具有高生产力和模块化设计的 Go Web 框架  
https://go-macaron.com/docs  
国人无闻写的《Go入门指南》  
https://github.com/Unknwon/the-way-to-go_ZH_CN  
同样无闻出的《Go 编程基础》免费视频教程  
https://github.com/Unknwon/go-fundamental-programming  

### 基础入门参考项目
一个简单的明星库项目 https://github.com/yz124/superstar    
go实现的blog项目 https://github.com/b3log/pipe  

### 相关资料汇总
gopher笔记  
https://github.com/xmge/gonote  
golang语法例子  
https://gobyexample.com/  
包管理工具gb  
https://getgb.io/docs/project/    
包管理工具gopm  
https://github.com/gpmgo/gopm  
国人整理的golang阅读列表  
https://github.com/qichengzx/gopher-reading-list-zh_CN  
国外大牛整理的go资料汇总  
https://github.com/enocom/gopher-reading-list  
腾讯云上golang的学习资料  
https://cloud.tencent.com/developer/doc/1101  
国外大牛整理的go资料汇总  
https://github.com/avelino/awesome-go  
慕课网golang实战课程列表  
https://www.imooc.com/course/list?c=go  

### 其他
构建go，利用docker去交叉编译，解决环境问题是个不错的选择  
https://medium.com/@kelseyhightower/optimizing-docker-images-for-static-binaries-b5696e26eb07  

手工快速下载golang包的方法  
https://www.golangtc.com/download/package  

### 其他文章
http://blog.ralch.com/  
