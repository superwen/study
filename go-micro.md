
```
# 安装consul并运行
brew install consul
consul agent -dev

# 安装go-micro
go get github.com/micro/go-micro

# 安装proto生成工具
go get github.com/micro/protobuf/{proto,protoc-gen-go}

# 安装micro工具包
go get github.com/micro/micro

# 开发service
# 这里面有一个例子
# github.com/micro/examples/greeter/srv
# clone 下来 build一下，运行就可以
go get github.com/micro/examples/greeter/srv && srv

# 查看在运行的services
micro list services

# 按照package查看service信息
micro get service hello

# 按照packeage调用service服务
micro call hello Hello.Ping '{"Name": "portalhuan"}' 
```
