#### defer 延迟调用

关键字 defer ⽤于延迟一个函数或者方法（或者当前所创建的匿名函数）的执行。注意，defer语句只能出现在函数或方法的内部

defer语句经常被用于处理成对的操作，如打开、关闭、连接、断开连接、加锁、释放锁。通过defer机制，不论函数逻辑多复杂，都能保证在任何执行路径下，资源被释放。释放资源的defer应该直接跟在请求资源的语句后

##### 多个defer执行顺序

以LIFO（后进先出）的顺序执行。哪怕函数或某个延迟调用发生错误，这些调用依旧会被执⾏。
```golang
package main //必须

import "fmt"

func test(x int) {
	result := 100 / x

	fmt.Println("result = ", result)
}

func main() {

	defer fmt.Println("liuhu")

	//若果参数为零，调用一个函数，导致内存出问题，则错误最后报出
	defer test(2)

	defer fmt.Println("newtask")
}

#执行结果顺序：
newtask
result =  50
liuhu
```

##### defer和匿名函数结合使用

注意参数的传递  可以与闭包结合理解
```golang
package main

import "fmt"

func main() {
	a := 100
	b := 200
	defer func(a, b int) {
		fmt.Printf("a = %d, b = %d\n", a, b)
	}(a, b) //()代表调用此匿名函数, 把参数传递过去，已经先传递参数，只是没有调用

	defer func(a, b int) {
		fmt.Printf("a = %d, b = %d\n", a, b)
	}(10, 20) //()代表调用此匿名函数, 直接指定参数传递过去，已经先传递参数，只是没有调用

	a += 50
	b += 400
	fmt.Printf("外部：a = %d, b = %d\n", a, b)
}

//外部：a = 150, b = 600
//a = 10, b = 20
//a = 100, b = 200
```
