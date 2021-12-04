# golang基础语法

## 变量

### 声明方式

```go
package main

import "fmt"


var	e int = 100
var gB = 100

func main() {
	// 变量声明方式
	var a int
	a = 100
	fmt.Println(a)

	var b int  = 100
	fmt.Println(b)

	var c = 100
	fmt.Println(c)

	d := 100
	fmt.Printf("type of d is %T\n", d)

}
```

### 导包

一个包中可以有多个init函数，init函数的执行过程是按照每个_.go的字符串进行比较的



### defer

defer是什么？

  在Go语言中，可以使用关键字defer向函数注册退出调用，即主函数退出时，defer后的函数才被调用。defer语句的作用是不管程序是否出现异常，均在函数退出时自动执行相关代码。 

defer的用途 ？

在函数中,程序员经常需要创建资源(比如:数据库连接、文件句柄、锁等) ,为了在函数执行完 毕后,及时的释放资源,Go 的设计者提供 defer (延时机制)。

 注意事项： 

1) 当 go 执行到一个 defer 时,不会立即执行 defer 后的语句,而是将 defer 后的语句压入到一个栈 中[我为了讲课方便,暂时称该栈为 defer 栈], 然后继续执行函数下一个语句。 

2) 当函数执行完毕后,在从 defer 栈中,依次从栈顶取出语句执行(注:遵守栈 先入后出的机制), 

3) 在 defer 将语句放入到栈时,也会将相关的值拷贝同时入栈

> defer and return

```go
package main

import (
   "fmt"
)

func main() {
   fmt.Println("return:", *Demo2()) // 打印结果为 return: 2
}

func Demo2() *int {
   var i int
   defer func() {
      i++
      fmt.Println("defer2:", i) // 打印结果为 defer: 2
   }()
   defer func() {
      i++
      fmt.Println("defer1:", i) // 打印结果为 defer: 1
   }()
   return &i // 或者直接 return 效果相同
}
// 执行结果
defer2: 2
defer1: 1
return: 2


package main
 
import (
        "fmt"
)
 
func main() {
        fmt.Println("return:", Demo()) // 打印结果为 return: 0
}
 
func Demo() int {
        var i int
        defer func() {
                i++
                fmt.Println("defer2:", i) // 打印结果为 defer: 2
        }()
        defer func() {
                i++
                fmt.Println("defer1:", i) // 打印结果为 defer: 1
        }()
        return i
}

// 执行结果
defer2 1 
defer1 2 
return: 0
```

## 数组 动态数组



```go
package main

import "fmt"

func main() {
   // 数组
   a := [...]int{1,2,3,4,5,6,7} // 自动判断长度
   for index, value := range a{
      fmt.Println("index:", index, " value:", value)
   }
   // a的类型
   fmt.Printf("a的类型为：%T\n", a);

   // 切片 slice

   b := []int{1,2,3,4,5,6,7}
   for index, value := range b{
      fmt.Println("index:", index, " value:", value)
   }
   // a的类型
   fmt.Printf("a的类型为：%T\n", b);
}
```

## map

```go
package main

import "fmt"

func main() {
   cities := make(map[string]string, 3)

   cities["0"] = "beijing"
   cities["2"] = "beijing"
   cities["3"] = "beijing"
   cities["1"] = "beijing"
   cities["4"] = "beijing"

   fmt.Println(len(cities))
}
```



## struct

```go
package main

import "fmt"

type Book struct {
   age int
   title string
   author string
   publicer string
}

func main(){
   var book1 Book
   book1.age = 10
   book1.publicer = "新华出版社"
   book1.title = "朝花夕拾"
   book1.author = "鲁迅"
   fmt.Println(book1)

   book2 := Book{
      age:      0,
      title:    "",
      author:   "",
      publicer: "",
   }
 
    
  fmt.Println(book2)



}
```



## 面对对象

pass



## channel 管道



