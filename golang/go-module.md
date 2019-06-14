
```
export GO111MODULE=on
export GOPROXY=http://888.888.888.888:3000

# mod支持源替换
go mod edit -replace=google.golang.org/grpc=github.com/grpc/grpc-go@latest
go mod tidy
go mod vendor
go build -mod=vendor
```

常用的源替换有:  
```
go mod edit -replace=golang.org/x/build=github.com/golang/build@latest
go mod edit -replace=golang.org/x/crypto=github.com/golang/crypto@latest
go mod edit -replace=golang.org/x/mobile=github.com/golang/mobile@latest
go mod edit -replace=golang.org/x/net=github.com/golang/net@latest
go mod edit -replace=golang.org/x/sync=github.com/golang/sync@latest
go mod edit -replace=golang.org/x/sys=github.com/golang/sys@latest
go mod edit -replace=golang.org/x/text=github.com/golang/text@latest
go mod edit -replace=golang.org/x/time=github.com/golang/time@latest
go mod edit -replace=golang.org/x/tools=github.com/golang/tools@latest
```
