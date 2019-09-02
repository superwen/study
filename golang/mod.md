需改GO111MODULE
```
export GO111MODULE=on
```

推荐使用阿里的代理
```
https://mirrors.aliyun.com/goproxy
```

```
设置环境变量 GOPROXY 的值为代理网址，目前可用的模块公共代理网址有:
https://goproxy.io
https://athens.azurefd.net
https://goproxy.cn
https://gocenter.io
(注：Go语言官方已推出官方模块代理 https://proxy.golang.org 但目前国内处于被墙状态。)
或者使用:
https://github.com/goproxyio/goproxy
https://github.com/gomods/athens
自建模块代理。

mod是模块英文modules的简写。

列举一些常用的命令行：

go help mod查看帮助。
go mod init <项目模块名称>初始化模块，会在项目根目录下生成 go.mod 文件。参数<项目模块名称>是非必写的，但如果你的项目还没有代码编写，这个参数能快速初始化模块。如果之前使用其它依赖管理工具(比如dep，glide等)，mod会自动接管原来依赖关系。
go mod tidy根据go.mod文件来处理依赖关系。
go mod vendor将依赖包复制到项目下的 vendor 目录。建议一些使用了被墙包的话可以这么处理，方便用户快速使用命令go build -mod=vendor编译。
go list -m all显示依赖关系。go list -m -json all显示详细依赖关系。
go mod download <path@version>下载依赖。参数<path@version>是非必写的，path是包的路径，version是包的版本。
其它命令可以通过go help mod来查看。

```
常用的replace
```
go mod edit -replace=golang.org/x/build@latest=github.com/golang/build@latest
go mod edit -replace=golang.org/x/crypto@latest=github.com/golang/crypto@latest
go mod edit -replace=golang.org/x/mobile@latest=github.com/golang/mobile@latest
go mod edit -replace=golang.org/x/net@latest=github.com/golang/net@latest
go mod edit -replace=golang.org/x/sync@latest=github.com/golang/sync@latest
go mod edit -replace=golang.org/x/sys@latest=github.com/golang/sys@latest
go mod edit -replace=golang.org/x/text@latest=github.com/golang/text@latest
go mod edit -replace=golang.org/x/time@latest=github.com/golang/time@latest
go mod edit -replace=golang.org/x/tools@latest=github.com/golang/tools@latest
```
