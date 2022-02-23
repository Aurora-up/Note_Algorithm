[TOC]

##### 快速输入输出

```go
package main

import (
	"bufio"
	. "fmt"
	"os"
)

func main() {
	in := bufio.NewReader(os.Stdin)
	out := bufio.NewWriter(os.Stdout)
	defer out.Flush()

	var a , b, c int
	Fscan(in , &a , &b , &c)

	Fprint(out , a , b, c)
}
```





##### 控制流

###### if

go 语言的 if 的一个强大之处在于可以 条件判断语句中允许声明一个变量，这个变量的作用域只在该条件逻辑块内。 不支持三元操作符 "a > b ? a : b"。

```go
//if x > 10  // Error: missing condition in if statement
//{  
//}
x := 0

func main(){
    if n := "abc" x > 0 {
        Println(n[2])
    }else if x < 0 {
        Println(n[1])
    }else {
        Println(n[0])
    } 
}
```

###### for

```go
s := "abc" 
for i, n := 0, len(s); i < n; i++ { // 常⻅见的 for 循环，⽀支持初始化语句。 		
    println(s[i])
}


n := len(s) 
for n > 0 {							// 替代 while (n > 0) {}
	println(s[n]) 					// 替代 for (; n > 0;) {}
    n--
} 

for { 								// 替代 while (true) {}
    println(s)						// 替代 for (;;) {}
}
```

不要期望编译器能理解你的想法，在初始化语句中计算出全部结果是个好主意

```go
func length(s string) int { 
    println("call length.") 
    return len(s)
}

func main() { 
    s := "abcd"
	for i, n := 0, length(s); i < n; i++ { // 避免多次调⽤用 length 函数。 
        println(i, s[i])
	}
}
```

输出：

```
call length.
0 97 
1 98 
2 99
3 100
```

实例：

```go
package main

import (
	. "fmt"
)

func isPrime(n int) bool {
	if n < 2 { return false }
	for i := 2 ; i <=  n / i ; i++ {
		if n % i == 0 { return false }
	}
	return true
}

func main() {

	var f bool
	for i := 0 ; i <= 100 ; i++ {
		f = isPrime(i)
		if f == true { Printf("%d 是素数\n" ,i)}
	}
}
```

###### range

类似迭代器操作， 返回 (索引, 值) 或 (键, 值)。

```
					1st value 				2nd value
------------------+-------------------+------------------+------------------string				index 					s[index]		unicode , rune
array/slice map     index					s[index]
map					key						m[key]
channel				element
```

可忽略不想要的返回值，或用 "_" 这个特殊变量。

```go
s := "abc"

for i := range s {  	// 忽略 2nd value，⽀支持 string/array/slice/map。
	println(s[i])
}

for _,c := range s {	// 忽略 index。
    println(c)   
}

m := map[string]int{"a": 1, "b": 2}

for k,v := range m {
    println(k, v)
}
```

注意，range 会复制对象。









##### 字符串

不可变值类型，内部使用指针指向 UTF-8 **字节数组**。

- 默认是空字符串  “”。
- 用索引号访问某字节，如 `s[i]` 
- 不能⽤用序号获取字节元素指针，`&s[i]` 非法。
- **不可变类型，无法修改字节数组。**
- 字节数组尾部不包含 NULL。

```go
// runtime.h
struct String{
    byte* str;
    intgo len;
};
```

###### 字符串定义

使用   \`    \`  定义不做转义处理的原始字符串，支持跨行

```go
s := `a
b\r\n\x00
c`

Prinf(s)
```

  输出:

```
a 
b\r\n\x00
c
```

连接跨行字符串时，"+" 必须在上⼀一行末尾，否则导致编译错误。

```go
s := "Hello, " + 
	 "World!"
	 
s2 := "Hello, " 
      + "World!"   // Error: invalid operation: + untyped string
      
```

###### 字符定义

无 char 类型，为 **rune** 类型

单引号字符常量表⽰示 Unicode Code Point，⽀支持 \uFFFF、\U7FFFFFFF、\xFF 格式。 对应 rune 类型，UCS-4。

```go
func main() { 
	fmt.Printf("%T\n", 'a')
	var c1, c2 rune = '\u6211', '们' 
	println(c1 == '我', string(c2) == "\xe4\xbb\xac")
}
```

输出：

```
int32 			// rune 是 int32 的别名
true true
```

###### 字符串切片

**切片** ：支持用两个索引号返回⼦串。子串依然指向原字节数组，仅修改了指针和长度属性。

$[first\_index , second\_index)$  左闭右开

```go
s := "Hello world!"

s1 := s[:5]   // Hello   [0 , 5)
s2 := s[7:]   // World!  [7 , len(s) + 1)
s3 := s[1:5]  // ello    [1 , 5)
```

###### 字符串修改

修改字符串，可先将其转换成 `[]rune` 或 `[]byte`，完成后再转换为 `string`。无论哪种转换，都会重新分配内存，并复制字节数组。

​		因为字符串的底层是 字节数组 所以转为为字节数组来修改，又因为是由字符组成的，所以也可以拆解成字符数组来修改

```go
func main(){
	s := "abcd"
	bs := []byte(s)    // 修改成 字节数组
	
	bs[1] = 'B'
    Println(string(bs))
    
    u := "电脑"
    us := []rune(u)    // 修改成 字符数组
    us[1] = '话'
    Println(string(us))
}
```

输出:

```
aBcd
电话
```

###### 字符串遍历

用 for 循环遍历字符串时，也有 byte 和 rune 两种⽅方式。

```go
func main(){
    s := "abc汉字"
    
    for i := 0 ; i < len(s) ; i++ {   // byte
        fmt.Printf("%c , " , s[i])
    }
    
    fmt.Println()
    
    for _, i := range s {            // rune ， 支持中文
        fmt.Printf("%c , " , i)
    }
}
```

























