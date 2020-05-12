## 我整理的GO语言相关学习资料

### 正在学习的
[煎鱼的迷之博客go-gin-example](https://book.eddycjy.com/golang/gin/install.html)  
[高性能 Go 代码工坊》中译](https://www.yuque.com/ksco/uiondt)  
[Golang 程序员开发效率神器汇总！](https://segmentfault.com/a/1190000021155038)  
[Go语法基础与工程实践](https://github.com/wx-chevalier/Go-Series)  
[go开发规范总结](https://github.com/cristaloleg/go-advices/blob/master/README_ZH.md)  
[鉴权的框架](https://github.com/casbin/casbin)  

### go1.13相关
设置代理
```
go env -w GOPROXY=https://goproxy.cn,direct
```
设置不走代理的源
```
go env -w GOPRIVATE=gogs.cedock.com
```
由于验证包的服务器是 sum.golang.org ，被墙，可以关闭验证包
```
go env -w GOSUMDB=off
```
也可以设置验证包服务器
```
go env -w GOSUMDB=sum.golang.google.cn
```

### 安装
```
wget https://dl.google.com/go/go1.12.5.linux-amd64.tar.gz
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
vgo的使用方法 [wuyumin/tutorial/Modules](https://github.com/wuyumin/tutorial/blob/master/zh-cn/Modules/README.md)   

### 推荐框架  
web框架 https://github.com/kataras/iris *****  
比较热的web框架revel https://revel.github.io/  *****  
一个比较好的GUI框架  https://github.com/andlabs/ui ****  

### 热门库整理
[go语言数组及集合处理函数]（https://github.com/emirpasic/gods） https://github.com/emirpasic/gods  
[一个快速的结构化的日志库](https://github.com/uber-go/zap) https://github.com/uber-go/zap  
[一个配置库](http://github.com/spf13/viper) http://github.com/spf13/viper  
一个更快的json解析库 https://github.com/segmentio/encoding  
目前golang性能最好的http库 https://github.com/valyala/fasthttp  
redis库 https://github.com/gomodule/redigo  
orm不用多说 github.com/jinzhu/gorm *****  
mongoDb客户端 https://github.com/globalsign/mgo  
Etcd客户端 https://github.com/etcd-io/etcd  
常用的str工具集 https://github.com/mgutz/str  
好用的日志操作 https://github.com/sirupsen/logrus  
kafka客户端 https://github.com/Shopify/sarama  
强烈推荐json解析库 https://github.com/json-iterator/go  *****  
不固定数据结构的json解析库 https://github.com/buger/jsonparser ****  
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
无闻大师的ini库 https://github.com/go-ini/ini  

### 热门项目
国人开发的一个基于beego的cms  
https://github.com/yunnet/gardens  
国人用go开发的git server *****  
https://github.com/gogs/gogs  
Macaron 是一个具有高生产力和模块化设计的 Go Web 框架  
https://go-macaron.com/docs  
国人无闻写的《Go入门指南》  
https://github.com/Unknwon/the-way-to-go_ZH_CN  
同样无闻出的《Go 编程基础》免费视频教程  
https://github.com/Unknwon/go-fundamental-programming  
国人开发的一个web开发框架还不错  
https://github.com/henrylee2cn/faygo

### 基础入门参考项目
一个简单的明星库项目  
https://github.com/yz124/superstar    
go实现的blog项目  
https://github.com/b3log/pipe  
iris和xorm实现的后台api  
https://github.com/snowlyg/IrisApiProject

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
