#### 参考文章
https://www.jianshu.com/p/1f546747cb09  
https://github.com/DavidCai1993/my-blog/issues/35  

```
go get -u github.com/valyala/fasthttp
```
Get请求
```
package main

import (
    "github.com/valyala/fasthttp"
)

func main() {
    url := `http://httpbin.org/get`

    status, resp, err := fasthttp.Get(nil, url)
    if err != nil {
        fmt.Println("请求失败:", err.Error())
        return
    }

    if status != fasthttp.StatusOK {
        fmt.Println("请求没有成功:", status)
        return
    }

    fmt.Println(string(resp))
}
```
简单Post请求
```
func main() {
    url := `http://httpbin.org/post?key=123`
    
    // 填充表单，类似于net/url
    args := &fasthttp.Args{}
    args.Add("name", "test")
    args.Add("age", "18")

    status, resp, err := fasthttp.Post(nil, url, args)
    if err != nil {
        fmt.Println("请求失败:", err.Error())
        return
    }

    if status != fasthttp.StatusOK {
        fmt.Println("请求没有成功:", status)
        return
    }

    fmt.Println(string(resp))
}
```
Post请求，设置header
```
func main() {
    url := `http://httpbin.org/post?key=123`
    
    req := &fasthttp.Request{}
    req.SetRequestURI(url)
    
    requestBody := []byte(`{"request":"test"}`)
    req.SetBody(requestBody)

    // 默认是application/x-www-form-urlencoded
    req.Header.SetContentType("application/json")
    req.Header.SetMethod("POST")

    resp := &fasthttp.Response{}

    client := &fasthttp.Client{}
    if err := client.Do(req, resp);err != nil {
        fmt.Println("请求失败:", err.Error())
        return
    }

    b := resp.Body()

    fmt.Println("result:\r\n", string(b))
}
```
通过服务协程和内存变量的复用,提供性能  
AcquireRequest()、AcquireResponse()  
```
func main() {
    url := `http://httpbin.org/post?key=123`

    req := fasthttp.AcquireRequest()
    defer fasthttp.ReleaseRequest(req) // 用完需要释放资源
    
    // 默认是application/x-www-form-urlencoded
    req.Header.SetContentType("application/json")
    req.Header.SetMethod("POST")
    
    req.SetRequestURI(url)
    
    requestBody := []byte(`{"request":"test"}`)
    req.SetBody(requestBody)

    resp := fasthttp.AcquireResponse()
    defer fasthttp.ReleaseResponse(resp) // 用完需要释放资源

    if err := fasthttp.Do(req, resp); err != nil {
        fmt.Println("请求失败:", err.Error())
        return
    }

    b := resp.Body()

    fmt.Println("result:\r\n", string(b))
}
```
