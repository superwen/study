```
package main

import (
	"fmt"
	"math/rand"
	"sync"
	"time"
)

// Task 任务类型
type Task struct {
	ID   string
	Name string
}

var wg sync.WaitGroup

// Run 任务执行方法
func (task Task) Run() error {
	fmt.Println(task.ID + " starting...")
	sleepTime := rand.Intn(10)
	time.Sleep(time.Duration(sleepTime) * time.Second)
	fmt.Printf("%s finised， acount for %d  second. \n", task.ID, sleepTime)
	return nil
}

func main() {
	rand.Seed(time.Now().Unix())

	tasks := []Task{
		{"task001", "task001"},
		{"task002", "task002"},
		{"task003", "task003"},
	}

	for _, task := range tasks {
		wg.Add(1)
		go func(task Task) {
			defer wg.Done()
			task.Run()
		}(task)
	}

	fmt.Println("wg wait")
	wg.Wait()
	fmt.Println("wg finished")
}
```
