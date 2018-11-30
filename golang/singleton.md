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
