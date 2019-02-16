可以用锁的方式，但是不够优雅，推荐使用sync.Once  

```
var (
    r    *repository
    once sync.Once
)

type repository struct {
    items map[string]string
}

func Repository() *repository {
    once.Do(func() {
        r = &repository{
            items: make(map[string]string),
        }
    })
    return r
}
```
再比如数据库连接实例
```go

import (
	"gopkg.in/mgo.v2"
	"sync"
)

type Mongodb struct {
	Connect string //连接字符串
}

var (
	m          *Mongodb
	once       sync.Once
)

/**
 * 返回单例实例
 * @method New
 */
func New(connect string) *Mongodb {
	once.Do(func() { //只执行一次
		m = &Mongodb{Connect: connect}
	})
	return m
}
```
