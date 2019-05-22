
```
export GO111MODULE=on
export GOPROXY=http://888.888.888.888:3000

# mod支持源替换
go mod edit -replace=google.golang.org/grpc=github.com/grpc/grpc-go@latest
go mod tidy
go mod vendor
go build -mod=vendor
```
