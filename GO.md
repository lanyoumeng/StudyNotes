# GO

## 下载更新

```Shell
 wget   https://go.dev/dl/go1.22.0.linux-amd64.tar.gz
 tar -xvzf go1.22.0.linux-amd64.tar.gz -C $HOME/go
  mv $HOME/go/go $HOME/go/go1.22.0
   export PATH=$HOME/go/go1.22.0/bin:$PATH
    source ~/.bashrc  # 或 source ~/.bash_profile
    
 
 echo 'export PATH=$PATH:/home/lanmengyou/go/bin' >> ~/.bashrc
source ~/.bashrc

设置代理
# 执行以下命令
go env -w GO111MODULE=on
go env -w GOPROXY=https://goproxy.cn,direct
 
```

## 基础语法

### 运行参数

短参数（Short Option）：短参数通常以单个短横线 `-` 开头，后面紧跟单个字符，用于表示一个参数。例如，`-v` 表示一个名为 `v` 的参数。

长参数（Long Option）：长参数以两个短横线 `--` 开头，后面跟有一个描述性的名称，用于表示一个参数。例如，`--version` 表示一个名为 `version` 的参数。

1. `go run` 命令：
   1. 作用：`go run` 命令用于编译并直接运行 Go 程序，而不会在文件系统中生成可执行文件。
   2. 使用场景：一般用于在开发过程中快速编译和测试代码，方便进行调试和开发阶段的快速迭代。
   3. 示例：`go run main.go`
2. `go run` 命令不支持直接传递参数，需要使用 `go build` 构建可执行文件，然后再运行它并传递参数。

```Shell
go build -o main
./main -addr=:8080
```

1. `go build` 命令：
   1. 作用：`go build` 命令用于编译 Go 程序，并在文件系统中生成一个可执行文件。生成的可执行文件的名称默认与当前目录下的 Go 源文件的包名相同。
   2. 使用场景：适用于项目的构建和发布，或者构建长期运行的可执行文件。
   3. `-o` 标志用于指定编译输出的文件名或路径。例如，`go build -o myapp` 将生成一个名为 `myapp` 的可执行文件（如果操作系统支持的话），或者 `myapp.exe`（在 Windows 上）。如果你想将输出文件放在指定路径，可以使用类似这样的命令：`go build -o /path/to/output/myapp`.
   4. `-v` 标志用于在构建过程中输出详细的日志信息，包括编译的包名和版本信息，以及所链接的对象文件等。这在调试构建过程或查找构建问题时非常有用。例如，`go build -v` 将显示构建过程中编译的每个包的信息。
2. 下面是两个标志同时使用的例子：

```Go
go build -o myapp -v cmd/main.go
```

1. 这个命令将编译 `cmd/main.go` 文件，并将生成的可执行文件命名为 `myapp`，同时输出详细的构建日志信息。
2. `go get`：用于下载并安装指定的包及其依赖。例如，`go get github.com/example/package` 会下载并安装 `github.com/example/package` 包。
3. `go install`：编译并安装 Go 程序，类似于 `go build`，但会将生成的可执行文件放置在 `$GOPATH/bin` 或 `$GOBIN` 目录下。
4. `go test`：用于运行测试文件并执行测试。它会自动查找项目中的测试文件并运行其中的测试函数。
5. `go vet`：静态分析工具，用于检查 Go 代码中的常见错误和问题。
6. `go fmt`：用于格式化 Go 源代码，使其符合 Go 的格式规范。
7. `go mod`：用于管理 Go 项目的模块依赖关系。常用命令包括：
   1. `go mod init`：初始化一个新的 Go 模块。
   2. `go mod tidy`：移除无用的依赖项并添加缺少的依赖项。
   3. `go mod vendor`：将依赖项复制到 `vendor` 目录。
8. `go doc`：用于查看 Go 源代码的文档。例如，`go doc fmt.Printf` 可以查看 `fmt` 包中 `Printf` 函数的文档。
9. `go generate`：执行 Go 源代码中的生成命令，用于生成一些衍生文件，如模拟代码、资源文件等。

这些命令只是 Go 工具链中的一部分。你可以通过运行 `go help` 命令查看完整的命令列表和用法说明。例如，`go help` 或者 `go help build`

### io

#### 输入

1、fmt.Scan

2、fmt.Scanf

3、fmt.Scanln 换行停止

4、bufio.NewReader 可以输入包含空格的字符串

```Go
   reader := bufio.NewReader(os.Stdin) // 从标准输入生成读对象
   text, _ := reader.ReadString('\n') // 读到换行   err
```

#### 输出

fmt.Printf(" ", ) %v 就可以输出任意类型 // %+v 输出详细信息 %#v更详细 %.3f 保留小数点三位 %t bool类型 %T输出变量的类型 %p 指针

### 控制循环

#### if

```Go
if a,b:=1,2; a>b{  //ab只在if语句中使用
print("a>b")
}else{
print("a<b")}
```

#### switch - case

1.不需要break

  可以不指定case表达式的值，且可以进行多条件判断。

```Go
switch{
    case 1,2,3,4:
    default:
}
```

2.判断某个 interface 变量中实际存储的变量类型 3.fallthrough 会强制执行后面的 case 语句，fallthrough 不会判断下一条 case 的表达式结果是否为 true。

fallthrough不能用在switch的最后一个分支。

4.支持多条件匹配

```Go
switch {
    case false:
        fmt.Println("The integer was <= 4")
        fallthrough
    case true:
        fmt.Println("The integer was <= 5")
        fallthrough
    case false:
        fmt.Println("The integer was <= 6")
        fallthrough
    case true:
        fmt.Println("The integer was <= 7")
        fallthrough
    case false:
        fmt.Println("The integer was <= 8")
    default:
        fmt.Println("default case")
    }
    
    输出结果：
The integer was <= 5
The integer was <= 6
The integer was <= 7
The integer was <= 8
```

5.switch 的 default 不论放在哪都是最后执行

#### for

```Go
1.for key, value := range oldMap {
    fmt.Println(key ,value)
}
```

2.`map`的迭代顺序随机





### 数据

#### 数据定义

1. **intVal := 1**

```Go
2. var intVal int 
intVal =1 
```

#### 数据类型

##### int

c++ int 大部分是4字节

go中机器位数 64位是8个字节

##### 字符串

1.本质是【不可修改的byte数组】 可以 range遍历 //不可以 s[0] ="a" 这样修改

转换成 arr := []rune(str) byte数组 （要想输出字符，可以用%c)

2.utf-8编码中，一个汉字是3个byte 

3.可以用反引号 包含 （会输出代码中的样子）

4.

![202407090147642](https://raw.githubusercontent.com/lanyoumeng/Drawing-bed/main/docs/202407111917183.png)

5.原始字符串（raw string）

是指由反引号``括起来的字符串文字。使用原始字符串可以让你在字符串中包含特殊字符（如换行符 `\n`、制表符 `\t`、反斜杠 `\` 等）而无需进行转义。

原始字符串的语法格式如下：

```Plaintext
`raw string`
func main() {
        rawStr := `This is a raw string.
It can contain special characters like \n, \t, and others without needing to escape them.
For example, you can include backslashes: \\.`
        fmt.Println(rawStr)
}
```

在上面的示例中，`rawStr` 是一个原始字符串，其中包含多行文本和特殊字符，而不需要对它们进行转义。在打印 `rawStr` 时，它会保持原始形式输出，包含换行符、反斜杠等特殊字符。

##### 常量

```Go
const (
    Unknown = 0
    Female = 1
    Male = 2
)
```



#### 类型转换

1.go 不支持隐式转换类型

```Go
    var a int64 = 3
    var b int32
    b = a   //会报错
```

##### Atoi 和 Itoa

`Atoi`将string转成int，`Itoa`将int转成string。例如：

```Go
fmt.Println(strconv.Atoi("123"))  // 123, nil
fmt.Println(strconv.Atoi("123a")) // 0, strconv.ParseInt: parsing "123a": invalid syntax
fmt.Println(strconv.Itoa(123))    // 123
```

实际上，`Atoi`和`Itoa`分别调用了`ParseInt`和`FormatInt`方法。

##### format 和 parse

format和parse是两组相反的方法，用于string和其它类型之间的相互转化。相关方法有：

- func FormatBool(b bool) string
- func FormatFloat(f float64, fmt byte, prec, bitSize int) string
- func FormatInt(i int64, base int) string  
- func FormatUint(i uint64, base int) string

- func ParseBool(str string) (bool, error)
- func ParseFloat(s string, bitSize int) (float64, error)
- func ParseInt(s string, base int, bitSize int) (i int64, err error)
- func ParseUint(s string, base int, bitSize int) (uint64, error)

format相关方法将其它类型的数据转换为string，例如：

```Go
fmt.Println(strconv.FormatBool(true))                 // true
fmt.Println(strconv.FormatFloat(3.1415, 'E', -1, 64)) // 3.1415E+00
fmt.Println(strconv.FormatInt(-42, 16))               // -2a
fmt.Println(strconv.FormatUint(42, 16))               // 2a
```

parse相关方法将string转为其它类型的数据，例如：

```Go
fmt.Println(strconv.ParseBool("tRue"))        // true <nil>

fmt.Println(strconv.ParseFloat("3.1415", 32)) // 3.1414999961853027 <nil>
fmt.Println(strconv.ParseFloat("3.1415", 64)) // 3.1415 <nil>

fmt.Println(strconv.ParseInt("-42", 10, 32))  // -42 <nil>
fmt.Println(strconv.ParseInt("11011", 2, 32)) // 27 <nil>
fmt.Println(strconv.ParseInt("0xff", 0, 32))  // 255 <nil>
fmt.Println(strconv.ParseUint("42", 16, 32))  // 66 <nil>
```

对于`ParseBool`，遵循如下规则：

- 返回`true`：`"1"`，`"t"`，`"T"`，`"TRUE"`，`"true"`，`"True"`
- 返回`false`：`"0"`，`"f"`，`"F"`，`"FALSE"`，`"false"`，`"False"`
- 其它情况报错

对于`ParseFloat`，`bitSize`取值为`32`或`64`。

对于`ParseInt`，`base`取值在2~36之间，如果`base`为0，那么会根据第一个参数字符串的前缀来决定进制：`0`为8进制，`0x`为16进制，其它情况为10进制。第三个参数`bitSize`可以为`0`(int)，`8`(int8)，`16`(int16)，`32`(int32)，`64`(int64)。

`ParseUint`与`ParseInt`类似。

```Go
可以都转换为int 再转换
同类型之间转换，比如int64到int，直接int(int64)即可；
```

#### 类型定义

是一种创建新的自定义类型的机制，可以基于现有的基本类型或其他自定义类型创建新的类型名称。类型定义使代码更具可读性、可维护性，并有助于提高代码的安全性。你可以使用关键字 `type` 来定义新的类型。

以下是一些常见的类型定义示例：

1. **基于基本类型的类型定义**：

```Go
type ID int
type Name string
type Age uint8
```

1. 在这些示例中，我们创建了三个新类型：`ID`，`Name` 和 `Age`，分别基于 `int`、`string` 和 `uint8` 类型。这允许我们在代码中使用这些新类型名称，并将它们用于区分不同的数据元素。
2. **基于结构体的类型定义**：

```Go
type Person struct {
    FirstName string
    LastName  string
    Age       int
}
```

1. 在这个示例中，我们创建了一个名为 `Person` 的新类型，它基于一个结构体。这使我们可以将 `Person` 作为一个独立的复合类型来使用，包含了多个字段。
2. **基于接口的类型定义**：

```Go
type Logger interface {
    Log(message string)
}
```

这里我们创建了一个名为 `Logger` 的新类型，它基于一个接口定义。这可以用于定义函数或方法需要满足的接口规范。

在使用自定义类型时，需要注意类型转换的问题。尽管两个类型可能具有相同的底层表示，但它们在Go中仍然是不同的类型，需要显式类型转换来进行相互转换。例如：

```Go
type Celsius float64
type Fahrenheit float64

func main() {
    c := Celsius(22.0)
    f := Fahrenheit(c*9/5 + 32)
    fmt.Println(f)
}
```

在上面的示例中，`Celsius` 和 `Fahrenheit` 都是 `float64` 的别名类型，但它们不能直接相互赋值，需要进行类型转换。

### 函数

本身是一个变量 f2:=f1 1.可以返回多个值 2.错误就地处理

#### 匿名函数

自己调用自己

```Go
r1:=func(a,b int ) int {
    return a+b
}(10,20) //(括号是在调用这个函数)   这种写法等同于：r1(10,20)
```

#### 高阶函数

可以接受一个函数作为参数

```Plaintext
func oper (a,b int  , f func(int ,int) int) int{
r:=fun(a,b)
return r
}
```

#### ...

在 Go 语言中，`...` 是一种语法表示法，用于表示可变数量的参数或可变长度的切片。

1. **可变参数（Variadic Parameters）：** 当用于函数参数列表时，`...` 表示该函数接受可变数量的参数。可变参数被视为一个切片，在函数内部可以像操作切片一样进行处理。例如：

```Go
func Sum(nums ...int) int {
    total := 0
    for _, num := range nums {
        total += num
    }
    return total
}

sum := Sum(1, 2, 3, 4, 5) // 传递可变数量的参数
```

在上述示例中，`Sum` 函数接受可变数量的 `int` 类型参数。通过使用 `...int` 表示法，可以在函数调用时传递任意数量的参数。在函数内部，参数 `nums` 被视为一个类型为 `[]int` 的切片，可以通过遍历切片来计算总和。

1. **可变长度切片（Variadic Slice）：** 当用于切片后的参数名时，`...` 表示将一个切片的元素展开为独立的参数。这在函数调用时可以方便地将切片中的元素作为参数传递给接受可变参数的函数。例如：

```Go
func Sum(nums ...int) int {
    total := 0
    for _, num := range nums {
        total += num
    }
    return total
}

numbers := []int{1, 2, 3, 4, 5}
sum := Sum(numbers...) // 将切片元素展开为独立参数
```

在上述示例中，`numbers` 是一个包含一组整数的切片。通过使用 `numbers...`，可以将切片中的元素展开为独立的参数传递给 `Sum` 函数。

总之，`...` 是一种在 Go 语言中表示可变数量参数和可变长度切片的语法表示法。它使得函数可以接受不定数量的参数或将切片中的元素展开为独立的参数。

#### 隐式返参

这是 Go 语言的一种特性，允许在函数内部使用 `return` 语句来返回已命名的返回参数的值，而无需显式地指定返回值。

在您的代码中，`tokenString` 和 `err` 被隐式地作为返回参数命名，所以 `return` 语句会将它们的值返回。以下是修正后的解释：

```Go
// Sign 使用 jwtSecret 签发 token，token 的 claims 中会存放传入的 subject.
func Sign(identityKey string) (tokenString string, err error) {
    // Token 的内容
    token := jwt.NewWithClaims(jwt.SigningMethodHS256, jwt.MapClaims{
        config.identityKey: identityKey,
        "nbf":              time.Now().Unix(),
        "iat":              time.Now().Unix(),
        "exp":              time.Now().Add(100000 * time.Hour).Unix(),
    })
    // 签发 token
    tokenString, err = token.SignedString([]byte(config.key))

    return // 隐式返回 tokenString 和 err
}
```

### defer

1.栈后进先出 2.defer加匿名函数 ————>会在执行时进行求值 defer后直接加golang语句 ————>`defer`语句被声明时求值

```Go
func main() {
    a := 1
    defer fmt.Println(a)
    
    a++
    fmt.Println(a)
}
2
1

func main() {
    a := 1

    defer func() {

        fmt.Println(a)
    }()

    a++
    fmt.Println(a)
}
2
2
```

### 结构体

```Go
type stu struct {
        Age  int
        name string
}
func main() {
        var s stu
        s = stu{Age: 12, name: "小明"}
}
```

### 指针

nil 指针也称为空指针。

```Go
//错误写法   因为此时user是nil，对nil赋值会报错
        var user *model.User 
        user.Id, _ = strconv.ParseInt(redisData["id"], 10, 64)
        user.FollowCount, _ = strconv.ParseInt(redisData["follow_count"], 10, 64)
//正确写法
 user := &biz.User{} // 初始化 user
        
```

在Go语言中，变量初始化通常是指为变量分配内存并为其设置初始值。

对于指针变量，初始化是将其指向某个已分配内存的位置（赋值为一个有效的内存地址）。如果指针变量没有被赋值，它将是`nil`，表示它没有指向任何有效的内存位置。

对于指针变量，以下几种情况被认为是初始化指针：

1. **使用****`new()`****函数分配内存：** 使用`new()`函数可以分配一块内存，并返回一个指向该内存的指针。这个指针已经被初始化为零值（即`nil`）。

```Go
var ptr *int
ptr = new(int) // 初始化指针，分配int类型的内存空间，并将ptr指向该内存
```

1. **使用取地址操作符****`&`****：** 当你使用取地址操作符`&`将一个变量的地址赋值给指针变量时，也被视为初始化。

```Go
var x int
ptr := &x // 初始化指针，将x的地址赋值给ptr
```

1. **直接赋值：** 你可以直接将一个已存在的指针赋值给另一个指针。

```Go
var ptr1 *int
var ptr2 *int
ptr1 = ptr2 // 初始化指针，将ptr2的值赋给ptr1，可能是nil或者其他指针
```

### 数组

```Go
var balance = [...]float32{1000.0, 2.0, 3.4, 7.0, 50.0}
或
balance := [...]float32{1000.0, 2.0, 3.4, 7.0, 50.0}

//  将索引为 1 和 3 的元素初始化
balance := [5]float32{1:2.0,3:7.0}
```

### 切片

• slice 是一个长度可变的连续数据序列，在实现上基于一个 slice header 组成，其中包含的字段包括：指向内存空间地址起点的指针 array、一个表示了存储数据长度的 len 和分配空间长度的 cap

• 由于 slice 在传递过程中，本质上传递的是 slice header 实例中的内存地址 array，因此属于引用传递

• slice 在扩容时，遵循如下机制:

• 如果扩容时预期的新容量超过原容量的两倍，直接取预期的新容量

• 如果原容量小于 256，直接取原容量的两倍作为新容量

• 如果原容量大于等于 256，在原容量 n 的基础上循环执行 n += (n+3*256)/4 的操作，直到 n 大于等于预期新容量，并取 n 作为新容量

• 最后还需要友情提示一下，slice 不是并发安全的数据结构，大家在使用时请务必注意并发安全问题.

1.var初始化或者通过内置函数 **make()** 初始化切片**s**

```Go
var numbers []int
s :=make([]int,len,cap) 
 // 声明一个字符串指针的切片    listOfNumberStrings := []*string{}
```

2.直接初始化切片，**[]** 表示是切片类型，**{1,2,3}** 初始化值依次是 **1,2,3**，其 **cap=len=3**。

```Go
s :=[]int {1,2,3 } 
```

3.初始化切片 **s**，是数组 arr 的引用

```Go
s := arr[:] 
```

将 arr 中从下标 startIndex 到 endIndex-1 下的元素创建为一个新的切片 s := arr[startIndex:endIndex]

4.len() 和 cap() 函数

```Go
len(x),cap(x)  //长度和容量
```

5.append() 和 copy() 函数

```Go
 numbers = append(numbers, 2,3,4)
 numbers := append(numbers, 2,3,4) 
 //append会构建一个新的切片，不是在原来的切片中添加 ，但指向没变(浅拷贝)
//如果发生扩容，会申请一块全新的空间，指向改变
 
 copy(numbers1,numbers)/* 拷贝 numbers 的内容到 numbers1 */
```

7.输出切片详情

```Go
func printSlice(x []int){
   fmt.Printf("len=%d cap=%d slice=%v\n",len(x),cap(x),x)
}
```

8.遍历切片

```Go
for i,ele := range arr{  //i 下标 ele 对于的值  (是一块单独的空间)
  fmt.println(i,ele)  
}
```

9.定义切片和定义容器

```Go
//定义切片  不会分配内存，默认为nil
var s []string
//定义容器    明确会使用  分配内存
v := make(map[int]string, 4)
v := make([]string, 0, 4)
```

#### 底层实现

源码为 go v1.19

##### **数据结构**

##### **数据结构**

![202407092257772](https://raw.githubusercontent.com/lanyoumeng/Drawing-bed/main/docs/202407111918838.png)

```Plaintext
type slice struct {
    // 指向起点的地址
    array unsafe.Pointer
    // 切片长度
    len   int
    // 切片容量
    cap   int
}
```

切片的类型定义如上，我们称之为 slice header，对应于每个 slice 实例，其中核心字段包括：

-  array：指向了内存空间地址的起点. 由于 slice 数据存放在连续的内存空间中，后续可以根据索引 index，在起点的基础上快速进行地址偏移，从而定位到目标元素
- len：切片的长度，指的是逻辑意义上 slice 中实际存放了多少个元素
- cap：切片的容量，指的是物理意义上为 slice 分配了足够用于存放多少个元素的空间. 使用 slice 时，要求 cap 永远大于等于 len

slice 本身属于引用传递操作

 

##### **初始化**

下面先来介绍下切片的初始化操作：

• 声明但不初始化

下面给出的第一个例子，只是声明了 slice 的类型，但是并没有执行初始化操作，即 s 这个字面量此时是一个空指针 nil，并没有完成实际的内存分配操作.

```Plaintext
  var s []int 
```

 

• 基于 make 进行初始化

make 初始化 slice 也分为两种方式, 第一种方式如下：

```Plaintext
  s := make([]int,8)
```

此时会将切片的长度 len 和 容量 cap 同时设置为 8. 需要注意，切片的长度一旦被指定了，就代表对应位置已经被分配了元素，尽管设置的会是对应元素类型下的零值.

 

第二种方式，是分别指定切片的长度 len 和容量 cap，代码如下：

```Plaintext
  s := make([]int,8,16)
```

如上所示，代表已经在切片中设置了 8 个元素，会设置为对应类型的零值；

cap = 16 代表为 slice 分配了用于存放 16 个元素的空间. 需要保证 cap >= len. 在 index 为 [len, cap) 的范围内，虽然内存空间已经分配了，但是逻辑意义上不存在元素，直接访问会 panic 报数组访问越界；

但是访问 [0,len) 范围内的元素是能够正常访问到的，只不过会是对应元素类型下的零值.

```Go
    s := make([]int, 10, 12)
    s1 := s[8:]
    s1 = append(s1, []int{10, 11}...)
    println(s[10])
    //也会报panic
```

- • 初始化连带赋值

初始化 slice 时还能一气呵成完成赋值操作. 如下所示：

```Plaintext
  s := []int{2,3,4}
```

这样操作的话，会将 slice 长度 len 和容量 cap 均设置为 3，同时完成对这 3 个元素赋值.

 

下面我们来一起过目一下切片初始化的源码，方法入口位于 golang 标准库文件 runtime/slice.go 文件的 makeslice 方法中：

```Plaintext
func makeslice(et *_type, len, cap int) unsafe.Pointer {
    // 根据 cap 结合每个元素的大小，计算出消耗的总容量
    mem, overflow := math.MulUintptr(et.size, uintptr(cap))
    if overflow || mem  maxAlloc || len < 0 || len  cap {
        // 倘若容量超限，len 取负值或者 len 超过 cap，直接 panic
        mem, overflow := math.MulUintptr(et.size, uintptr(len))
        if overflow || mem  maxAlloc || len < 0 {
            panicmakeslicelen()
        }
        panicmakeslicecap()
    }
    // 走 mallocgc 进行内存分配以及切片初始化
    return mallocgc(mem, et, true)
}
```

上述方法核心步骤是

- • 调用 math.MulUintptr 的方法，结合每个元素的大小以及切片的容量，计算出初始化切片所需要的内存空间大小
- • 倘若内存空间超限，则直接抛出 panic
- • 调用位于 runtime/malloc.go 文件中的 mallocgc 方法，为切片进行内存空间的分配

 

##### 引用传递

每次我们在方法间传递切片时，会对 slice header 实例本身进行一次值拷贝，然后将 slice header 的副本传递到局部方法中.

然而，这个 slice header 副本中的 array 和原 slice 指向同一片内存空间，因此在局部方法中执行修改操作时，还会根据这个地址信息影响到原 slice 所属的内存空间，从而对内容发生影响.

 

注意：

#####  **内容截取**

 s[a:b] 的格式，

截取动作会创建出一个新的 ***slice header*** 实例

```Plaintext
  s := []int{0,1,2,3,4,5,6,7}
  s1 := s[2:5]
  // ...
```

s1 = s[1:] 的操作，会创建出一个 s1 的 slice header，其中的字段 array 会在 s.array 的基础上向右偏移一个切片元素大小的数值；

s1.len 和 s1.cap 也会以 s1 的起点为起点，以 s 原定的 len 和 cap 终点为终点

s1.len = 3 s1.cap = s.cap - 2=6

注意示例：

```Plaintext
func changeSlice(s1 []int) {
     s1 = append(s1, 10)
    //s1[0] = 10 但要是修改就可以
}

func main() {
    s := make([]int, 10, 12)
    s1 := s[8:]
    changeSlice(s1)
    fmt.Printf("s: %v, len of s: %d, cap of s: %d\n", s, len(s), cap(s))
    fmt.Printf("s1: %v, len of s1: %d, cap of s1: %d", s1, len(s1), cap(s1))
}
```

答案：

```Plaintext
s: [0 0 0 0 0 0 0 0 0 0], len of s: 10, cap of s: 12
s1: [0 0], len of s1: 2, cap of s1: 4
```

虽然切片是引用传递，但是在方法调用时，传递的会是一个新的 slice header.

因此在局部方法 changeSlice 中，虽然对 s1 进行了 append 操作，只在局部方法中这个独立的 slice header 中生效，不会影响到原方法 Test_slice 当中的 s 和 s1 的长度和容量.

**但要是修改就可以**

##### **元素追加**

 

错误的使用方式示例：

```Plaintext
func Test_slice(t *testing.T){
    s := make([]int,5)
    for i := 0; i < 5; i++{
       s = append(s, i)
    }
    // 结果为：
    // s: [0,0,0,0,0,0,1,2,3,4]
}
```

我们预期的操作时声明出一个长度为 5 的 slice，同时依次向其中填入 0,1,2,3,4 的五个元素，然而按照上述代码执行下来，得到的结果是事与愿违的，其原因在于

- • 我们通过 make 操作，声明了一个长度和容量均为 5 的切片 s，此时前 5 个元素已经被填充为零值
- • 接下来执行 append 操作时，只会在长度末尾进行追加. 最终会引发扩容，并最终得到结果为 [0,0,0,0,0,0,1,2,3,4,5]

 

两个正确的使用示例：

示例一：

倘若大家希望使用 append 操作完成 slice 赋值，则应该在初始化 slice 时，给其设置不同的长度 len 和容量 cap 值，cap 和 len 之间的差值就是预留出来用于 append 操作的空间. 具体代码如下：

```Plaintext
func Test_slice(t *testing.T){
    s := make([]int,0,5)
    for i := 0; i < 5; i++{
       s = append(s, i)
    }
    // 结果为：
    // s: [0,1,2,3,4]
}
```

 

示例二：

我们将 slice 的长度和容量都设置为 5,然后通过遍历 slice 的方式进行执行位置元素的赋值（不使用 append 操作）：

```Plaintext
func Test_slice(t *testing.T){
    s := make([]int,5)
    for i := 0; i < 5; i++{
       s[i] = i
    }
    // 结果为：
    // s: [0,1,2,3,4]
}
```

 

这里称之为规范的核心原因在于，我们在创建 slice 时，如果能够预估到其未来所需的容量空间，则应该提前分配好对应容量，避免在运行过程中频繁触发扩容操作，这样会对性能产生不利的影响.

 

##### **切片扩容**

下面我们捋一下切片扩容的流程. 当 slice 当前的长度 len 与容量 cap 相等时，下一次 append 操作就会引发一次切片扩容

扩容后会迁移到新地址

```Plaintext
    // len:4, cap: 4
    s := []int{2,3,4,5}
    // len:5, cap: 8    
    s = append(s,6)
```

![202407092258524](https://raw.githubusercontent.com/lanyoumeng/Drawing-bed/main/docs/202407111918304.png)

切片的扩容流程源码位于 runtime/slice.go 文件的 growslice 方法当中，其中核心步骤如下：

• 倘若扩容后预期的新容量小于原切片的容量，则 panic

• 倘若切片元素大小为 0（元素类型为 struct{}），则直接复用一个全局的 zerobase 实例，直接返回

• 倘若预期的新容量超过老容量的两倍，则直接采用预期的新容量

• 倘若老容量小于 256，则直接采用老容量的2倍作为新容量

• 倘若老容量已经大于等于 256，则在老容量的基础上扩容 1/4 的比例并且累加上 192 的数值，持续这样处理，直到得到的新容量已经大于等于预期的新容量为止

• 结合 mallocgc 流程中，对内存分配单元 mspan 的等级制度，推算得到实际需要申请的内存空间大小

• 调用 mallocgc，对新切片进行内存初始化

• 调用 memmove 方法，将老切片中的内容拷贝到新切片中

• 返回扩容后的新切片

 

```Go
func growslice(et *_type, old slice, cap int) slice {
    //... 
    if cap < old.cap {
        panic(errorString("growslice: cap out of range"))
    }


    if et.size == 0 {
        // 倘若元素大小为 0，则无需分配空间直接返回
        return slice{unsafe.Pointer(&zerobase), old.len, cap}
    }


    // 计算扩容后数组的容量
    newcap := old.cap
    // 取原容量两倍的容量数值
    doublecap := newcap + newcap
    // 倘若新的容量大于原容量的两倍，直接取新容量作为数组扩容后的容量
    if cap  doublecap {
        newcap = cap
    } else {
        const threshold = 256
        // 倘若原容量小于 256，则扩容后新容量为原容量的两倍
        if old.cap < threshold {
            newcap = doublecap
        } else {
            // 在原容量的基础上，对原容量 * 5/4 并且加上 192
            // 循环执行上述操作，直到扩容后的容量已经大于等于预期的新容量为止
            for 0 < newcap && newcap < cap {             
                newcap += (newcap + 3*threshold) / 4
            }
            // 倘若数值越界了，则取预期的新容量 cap 封顶
            if newcap <= 0 {
                newcap = cap
            }
        }
    }


    var overflow bool
    var lenmem, newlenmem, capmem uintptr
    // 基于容量，确定新数组容器所需要的内存空间大小 capmem
    switch {
    // 倘若数组元素的大小为 1，则新容量大小为 1 * newcap.
    // 同时会针对 span class 进行取整
    case et.size == 1:
        lenmem = uintptr(old.len)
        newlenmem = uintptr(cap)
        capmem = roundupsize(uintptr(newcap))
        overflow = uintptr(newcap)  maxAlloc
        newcap = int(capmem)
    // 倘若数组元素为指针类型，则根据指针占用空间结合元素个数计算空间大小
    // 并会针对 span class 进行取整
    case et.size == goarch.PtrSize:
        lenmem = uintptr(old.len) * goarch.PtrSize
        newlenmem = uintptr(cap) * goarch.PtrSize
        capmem = roundupsize(uintptr(newcap) * goarch.PtrSize)
        overflow = uintptr(newcap)  maxAlloc/goarch.PtrSize
        newcap = int(capmem / goarch.PtrSize)
    // 倘若元素大小为 2 的指数，则直接通过位运算进行空间大小的计算   
    case isPowerOfTwo(et.size):
        var shift uintptr
        if goarch.PtrSize == 8 {
            // Mask shift for better code generation.
            shift = uintptr(sys.Ctz64(uint64(et.size))) & 63
        } else {
            shift = uintptr(sys.Ctz32(uint32(et.size))) & 31
        }
        lenmem = uintptr(old.len) << shift
        newlenmem = uintptr(cap) << shift
        capmem = roundupsize(uintptr(newcap) << shift)
        overflow = uintptr(newcap)  (maxAlloc  shift)
        newcap = int(capmem  shift)
    // 兜底分支：根据元素大小乘以元素个数
    // 再针对 span class 进行取整     
    default:
        lenmem = uintptr(old.len) * et.size
        newlenmem = uintptr(cap) * et.size
        capmem, overflow = math.MulUintptr(et.size, uintptr(newcap))
        capmem = roundupsize(capmem)
        newcap = int(capmem / et.size)
    }




    // 进行实际的切片初始化操作
    var p unsafe.Pointer
    // 非指针类型
    if et.ptrdata == 0 {
        p = mallocgc(capmem, nil, false)
        // ...
    } else {
        // 指针类型
        p = mallocgc(capmem, et, true)
        // ...
    }
    // 将切片的内容拷贝到扩容后的位置 p 
    memmove(p, old.array, lenmem)
    return slice{p, old.len, newcap}
}
```

##### **元素删除**

比如，我们期望删除 slice 中的首个元素，在操作上等同于从切片 index = 1 开始向后进行内容截取：

```Plaintext
func Test_slice(t *testing.T){
    s := []int{0,1,2,3,4}
    // [1,2,3,4]
    s = s[1:]
}
```

 

如果我们希望删除 slice 的尾部元素，则操作等价于截取切片内容，并将终点设置在 len(s) - 1 的位置：

```Plaintext
func Test_slice(t *testing.T){
    s := []int{0,1,2,3,4}
    // [0,1,2,3]
    s = s[0:len(s)-1]
}
```

 

如果需要删除 slice 中间的某个元素，操作思路则是采用内容截取加上元素追加的复合操作，可以先截取待删除元素的左侧部分内容，然后在此基础上追加上待删除元素后侧部分的内容：

```Plaintext
func Test_slice(t *testing.T){
    s := []int{0,1,2,3,4}
    // 删除 index = 2 的元素
    s = append(s[:2],s[3:]...)
    // s: [0,1,3,4], len: 4, cap: 5
    t.Logf("s: %v, len: %d, cap: %d", s, len(s), cap(s))
}
```

 

最后，当我们需要删除 slice 中的所有元素时，也可以采用切片内容截取的操作方式：s[:0]. 这样操作后，slice header 中的指针 array 仍指向远处，但是逻辑意义上其长度 len 已经等于 0，而容量 cap 则仍保留为原值.

```Plaintext
func Test_slice(t *testing.T){
    s := []int{0,1,2,3,4}
    s = s[:0]
    // s: [], len: 0, cap: 5
    t.Logf("s: %v, len: %d, cap: %d", s, len(s), cap(s))
}
```

 

##### **切片拷贝**

slice 的拷贝可以分为简单拷贝和完整拷贝两种类型.

要实现简单拷贝，我们只需要对切片的字面量进行赋值传递即可，这样相当于创建出了一个新的 slice header 实例，但是其中的指针 array、容量 cap 和长度 len 仍和老的 slice header 实例相同.

操作实例如下，最终输出的结果中，s 和 s1 的地址是一致的.

```Plaintext
func Test_slice(t *testing.T) {
    s := []int{0, 1, 2, 3, 4}
    s1 := s
    t.Logf("address of s: %p, address of s1: %p", s, s1)
}
```

 

这里再声明一下，切片的**截取操作也属于是简单拷贝**，以下面操作代码为例，s 和 s1 会使用同一片内存空间，只不过地址起点位置偏移了一个元素的长度. s1 和 s 的地址，刚好相差 8 个 byte.

```Plaintext
func Test_slice(t *testing.T) {
    s := []int{0, 1, 2, 3, 4}
    s1 := s[1:]
    t.Logf("address of s: %p, address of s1: %p", s, s1)
}
```

slice 的完整复制，指的是会创建出一个和 slice 容量大小相等的独立的内存区域，并将原 slice 中的元素一一拷贝到新空间中.

在实现上，slice 的完整复制可以调用系统方法 copy，代码示例如下，通过日志打印的方式可以看到，s 和 s1 的地址是相互独立的：

```Plaintext
func Test_slice(t *testing.T) {
    s := []int{0, 1, 2, 3, 4}
    s1 := make([]int, len(s))
    copy(s1, s)
    t.Logf("s: %v, s1: %v", s, s1)
    t.Logf("address of s: %p, address of s1: %p", s, s1)
}
```

#####  **问题**

```Plaintext
func Test_slice(t *testing.T){
    s := make([]int,512)  
    s = append(s,1)
    t.Logf("len of s: %d, cap of s: %d",len(s),cap(s))
}
```

答案：

```Plaintext
len: 513, cap: 848
```

首先，由于切片 s 原有容量为 512，已经超过了阈值 256，因此对其进行扩容操作会采用的计算共识为 512 * (512 + 3*256)/4 = 832

其次，在真正申请内存空间时，我们会根据切片元素大小乘以容量计算出所需的总空间大小，得出所需的空间为 8byte * 832 = 6656 byte

再进一步，结合分配内存的 mallocgc 流程，为了更好地进行内存空间对其，golang 允许产生一些有限的内部碎片，对拟申请空间的 object 进行大小补齐，最终 6656 byte 会被补齐到 6784 byte 的这一档次. （内存分配时，对象分档以及与 mspan 映射细节可以参考 golang 标准库 runtime/sizeclasses.go 文件，也可以阅读我的文章了解更多细节——golang 内存模型与分配机制）

```Plaintext
// class  bytes/obj  bytes/span  objects  tail waste  max waste  min align
//     1          8        8192     1024           0     87.50%          8
//     2         16        8192      512           0     43.75%         16
//     3         24        8192      341           8     29.24%          
// ...
//    48       6528       32768        5         128      6.23%        128
//    49       6784       40960        6         256      4.36%        128 
```

再终，在 mallocgc 流程中，我们为扩容后的新切片分配到了 6784 byte 的空间，于是扩容后实际的新容量为 cap = 6784/8 = 848.

### map

#### 1.初始化

```Go
/* 声明变量，默认 map 是 nil */ 需要用make分配空间
var map_variable map[key_data_type]value_data_type

/* 使用 make 函数 */
map_variable := make(map[key_data_type]value_data_type , 100 )

/* 赋值*/
 countryCapitalMap [ "France" ] = "巴黎"
```

#### 2.delete函数

delete() 函数用于删除集合的元素, 参数为 map 和其对应的 key

```Go
 delete(countryCapitalMap, "France")
```

#### 3.判断键值对是否存在

```Go
        if value, exist := m["a"]; exist {
                fmt.Print(value)
        } else {
                fmt.Print("map[\"a\"]不存在")
        }
```

#### 4.range遍历

```Go
for( k,v : range m){
fmt.println( k,v)
}
```

#### 5.sync.Map

是 Go 语言标准库提供的一种线程安全的 map 实现，它适用于多个 goroutine 并发访问的场景。与

1. **并发安全性**：`sync.Map` 能够安全地在多个 goroutine 中并发读写，不需要额外的锁机制。它内部实现了一种无锁（lock-free）的并发算法，使得对其的读操作是无锁的，写操作也能在很多情况下避免锁竞争，提高了并发性能。
2. **无需初始化**：`sync.Map` 不需要初始化，直接声明即可使用。
3. **动态键值对**：与普通的 `map` 不同，`sync.Map` 的键和值可以是任意类型的数据，不需要预先声明或指定类型。
4. **Load、Store 和 Delete 操作**：通过 `Load` 方法可以安全地从 `sync.Map` 中加载键对应的值，`Store` 方法可以安全地存储键值对，`Delete` 方法可以安全地删除键。
5. **Range 迭代**：支持 `Range` 方法进行安全的迭代操作，这个方法能够遍历整个 map，而且在遍历的过程中仍然允许其他操作，不会出现并发冲突。

`sync.Map` 在某些特定场景下能提供更好的性能，但在其他情况下可能并不是最佳选择。例如，如果对 map 的并发访问并不频繁，或者只是单纯的用于一组有限的数据，使用普通的 map 并结合互斥锁可能更简单直接。

```Go
func main()  {
        var m sync.Map
        // 1. 写入
        m.Store("qcrao", 18)
        m.Store("stefno", 20)

        // 2. 读取
        age, _ := m.Load("qcrao")
        fmt.Println(age.(int))

        // 3. 遍历
        m.Range(func(key, value interface{}) bool {
                name := key.(string)
                age := value.(int)
                fmt.Println(name, age)
                return true
        })

        // 4. 删除
        m.Delete("qcrao")
        age, ok := m.Load("qcrao")
        fmt.Println(age, ok)

        // 5. 读取或写入
        m.LoadOrStore("stefno", 100)
        age, _ = m.Load("stefno")
        fmt.Println(age)
}



          var m sync.Map
        eg, ctx := errgroup.WithContext(ctx)

        // 使用 goroutine 提高接口性能
        for _, video := range videoList {

                //获取作者信息
                eg.Go(func() error {
                        select {
                        case <-ctx.Done():
                                return nil
                        default:
                                author, err := v.repo.GetAuthorInfoById(ctx, video.AuthorId)
                                if err != nil {
                                        log.Errorw("Failed to list posts", "err", err)
                                        return err
                                }
                                
                                // 读取数据
                                value, exists := m.Load(video.Id)
                                if exists {
                                        // 数据存在，进行修改并再次写入
                                        if existVideo, ok := value.(*vpb.Video); ok {
                                                // 在现有数据的基础上修改
                                                existVideo.Id = video.Id
                                                existVideo.PlayUrl = video.PlayUrl
                                                existVideo.CoverUrl = video.CoverUrl
                                                existVideo.Title = video.Title
                                                existVideo.Author = author

                                                // 将修改后的数据重新存储到 sync.Map 中
                                                m.Store(video.Id, existVideo)
                                        } else {
                                                // 类型断言失败，记录错误并处理
                                                log.Errorw("Failed to assert type for video ID", "id", video.Id, "value", value)
                                                return errors.New("类型断言失败")
                                        }
                                } else {
                                        // 数据不存在，进行初始化并存储
                                        m.Store(video.Id, &vpb.Video{
                                                Id:       video.Id,
                                                PlayUrl:  video.PlayUrl,
                                                CoverUrl: video.CoverUrl,
                                                Title:    video.Title,
                                                Author:   author,
                                        })
                                }

                                return nil
                        }
                })
    }
```

### 接口

Go语言中的接口实现是隐式的，即类型只需要实现了接口中的方法，就被认为是实现了该接口。无需显式地声明类型实现了某个接口。

```Go
/* 定义接口 */
type interface_name interface {
   method_name1 [return_type]
   method_name2 [return_type]
   method_name3 [return_type]
   ...
   method_namen [return_type]
}

/* 定义结构体 */
type struct_name struct {
   /* variables */
}

/* 实现接口方法 */
func (struct_name_variable struct_name) method_name1() [return_type] {
   /* 方法实现 */
}

运用：  结构体名.method_name1()   //相当于加到结构体上
通过这种方式，我们可以使用接口来定义通用的方法集合，然后由不同的结构体实现这些方法，
实现了接口的多态性。这样可以提高代码的灵活性和可复用性。
```

#### 1.隐式转换

在Go语言中，结构体实例可以隐式地转换为接口类型，

```Go
package main

import "fmt"

type Person interface {
        Greet()
}

func NewPerson(name string, age int) Person {
        return person{
                name: name,
                age:  age,
        }
}

// /实现接口的结构体
type person struct {
        name string
        age  int
}

func (p person) Greet() {
        fmt.Printf("Hi! My name is %s\n", p.name)
}

func main() {
        p := NewPerson("李四", 12)
        p.Greet()
}
```

在函数的最后，直接返回`person`结构体实例。由于`person`结构体实现了`Person`接口中的所有方法，它可以隐式地转换为`Person`接口类型。

#### 2.`interface{}`

是 Go 语言中的接口类型。它是一个空接口，也称为最大接口，因为它不包含任何方法签名。 在 Go 中，`interface{}` 表示可以持有任意类型的值。它可以用作函数参数、函数返回值、结构体字段或接口类型的基础。 由于 `interface{}` 可以表示任意类型的值，因此在使用时需要进行类型断言或类型判断，以便在运行时正确地操作其中的具体值。 以下是一些使用 `interface{}` 的示例：

```Go
func PrintValue(value interface{}) {
    fmt.Println(value)
}

func main() {
    PrintValue(42)              // 传递一个 int 类型的值
    PrintValue("hello, world")  // 传递一个 string 类型的值
    PrintValue(3.14)            // 传递一个 float64 类型的值
}
```

在上述示例中，`PrintValue` 函数接受一个参数类型为 `interface{}` 的值。在函数内部，我们可以将其直接打印出来。通过使用 `interface{}` 类型，可以接受不同类型的值作为参数，而不需要为每种类型编写不同的函数。

需要注意的是，在使用 `interface{}` 时需要小心处理类型转换和类型断言，以确保操作的安全性和正确性。如果对具体类型的值进行错误的操作，可能会导致运行时错误。

#### 3.实现保证

在这个示例中，假设有一个接口类型 `IStore` 和一个实现该接口的结构体类型 `datastore`。为了确保 `datastore` 类型实现了 `IStore` 接口的所有方法，我们可以使用以下语法：

```Go
var _    IStore = (*datastore)(nil)
```

这行代码的作用是将 `(*datastore)(nil)` 转换为 `IStore` 类型并将其赋值给一个匿名的空白标识符变量 `_`。通过这种方式，我们可以确保 `datastore` 类型实现了 `IStore` 接口中定义的所有方法。这在编译时提供了一种静态的保证，以避免在运行时发生错误。

在这个表达式中，`(*datastore)` 表示将 `nil` 转换为 `*datastore` 类型的指针。这可以用于进行类型检查或验证某个类型是否实现了某个接口。

#### 4.类型断言

是一种用于判断一个接口值是否包含特定类型的操作，同时可以将其转换为该特定类型的操作。类型断言的语法如下：

```Go
value, ok := interfaceValue.(Type)
```

其中：

- `value` 是一个变量，用于存储接口值转换后的具体值。
- `ok` 是一个布尔值，用于表示类型断言是否成功。如果成功，`ok` 为 `true`，否则为 `false`。
- `interfaceValue` 是要进行类型断言的接口值。
- `Type` 是要断言的具体类型。

类型断言的基本工作原理如下：

1. 如果 `interfaceValue` 包含了 `Type` 类型的值，那么 `value` 将接收到该值，同时 `ok` 将设置为 `true`。
2. 如果 `interfaceValue` 不包含 `Type` 类型的值，那么 `value` 将接收到零值（默认值），同时 `ok` 将设置为 `false`。

以下是一个类型断言的示例：

```Go
package main

import (
        "fmt"
)

func main() {
        var i interface{}
        i = 42

        // 尝试将接口值 i 转换为 int 类型
        value, ok := i.(int)

        if ok {
                fmt.Printf("Value: %d\n", value)
        } else {
                fmt.Println("Conversion failed")
        }
}
```

在这个示例中，我们将接口值 `i` 转换为 `int` 类型，并使用 `ok` 变量来检查转换是否成功。由于 `i` 包含 `int` 类型的值，所以转换成功，并且 `ok` 的值为 `true`。

需要注意的是，如果类型断言失败，即 `ok` 的值为 `false`，那么在访问 `value` 变量时可能会导致运行时恐慌。因此，在进行类型断言之前，最好始终检查 `ok` 的值以确保安全的类型转换。否则，你可以使用类型断言来获取接口值中的具体类型的值，以便进行后续操作。

### 错误处理

实现 error 接口类型来生成错误信息

1.使用errors.New 可返回一个错误信息：

```Go
return errors.New("math: square root of negative number")
```

2.

```Go
func divide(a, b float64) (float64, error) {
    if b == 0 {
        return 0, errors.New("division by zero") // 返回自定义的错误信息
    }
    return a / b, nil // 返回计算结果和nil表示没有错误
}

func main() {
   
    if  result, err := divide(10, 2);err != nil {
        fmt.Println("Error:", err) // 处理错误信息
        return
    }else{
        fmt.Println("Result:", result)
}    
}
```

3.

```Go
// 定义一个 DivideError 结构
type DivideError struct {
    dividee int
    divider int
}

// 实现 `error` 接口
func (de *DivideError) Error() string {
    strFormat := `
    Cannot proceed, the divider is zero.
    dividee: %d
    divider: 0
`
    return fmt.Sprintf(strFormat, de.dividee)
}

   dData := DivideError{}
   errorMsg = dData.Error()
 if  errorMsg != "" {
            fmt.Println("errorMsg is: ", errorMsg)
    }
```

### 并发

#### **MPG并发模型**

![202407092259765](https://raw.githubusercontent.com/lanyoumeng/Drawing-bed/main/docs/202407111919497.png)

- M（Machine）：代表着操作系统线程，也称为机器线程。Go 运行时系统使用 M 来实际执行 Goroutine，并提供了与操作系统交互的能力。
- P（Processor）：是 Go 运行时系统的调度单元，负责管理 Goroutine 的执行。每个 P 关联一个 M，并且可以在多个 M 和 P 之间进行绑定和解绑。
- G（Goroutine）：是 Go 语言中的轻量级线程，由 Go 运行时系统调度和管理。Goroutine 可以看作是函数的执行实例，可以在并发环境中高效地运行。

MPG 模型的基本思想是将 Goroutine 调度和执行的工作分离到不同的 P 上，以实现并发和并行执行。M 负责与操作系统线程进行交互，并执行 P 所调度的 Goroutine。

Go 运行时系统会根据当前的负载情况动态调整 M、P 和 Goroutine 的数量，以提供高效的并发执行。它可以自动地创建和销毁 M 和 P，并将 Goroutine 均匀地分配给可用的 P。

通过这种 MPG 模型，Go 语言能够有效地处理大量的并发任务，充分利用多核处理器的能力，并提供简洁的并发编程模型。

2.goroutine 协程 是轻量级线程

```Go
go 函数名( 参数列表 )
```

（1）父子协程是一样的 是平行关系

 子协程不会影响main()协程 ，但 main函数一结束，runtime中的所有协程都会结束

![202407092300579](https://raw.githubusercontent.com/lanyoumeng/Drawing-bed/main/docs/202407111919422.png)

#### 并发安全

<2> recover

```Go
func f1(){
    defer func(){
        err := recover()
        if err !=nil{
            fmt.Println("发生了panic%s \n",err)
        }
    }()
    a,b := 3,0
    _=a/b
}
```

<3> 临界区和锁

```Go
var lock = sync.Mutex{}
for i:=0 ;i<11000;i++{   
    lock.Lock()
        i++
        lock.Unlock()  //等价于atomic.AddInt32(&n,1) 原子操作
}
```

#### sync.WaitGroup

![202407092301387](https://raw.githubusercontent.com/lanyoumeng/Drawing-bed/main/docs/202407111919223.png)

和slice mutex使用

```Go
// CommentList 方法
func (uc *CommentUsecase) CommentList(ctx context.Context, videoId int64) ([]*pbComment, error) {
    // 获取mysql 评论列表
    comments, err := uc.repo.CommentList(ctx, videoId)
    if err != nil {
       return nil, err
    }

    // 使用 sync.WaitGroup
    var wg sync.WaitGroup
    commentList := make([]*pb.Comment, len(comments))
    mu := sync.Mutex{}

    for i, comment := range comments {
       wg.Add(1)
       i, comment := i, comment // 捕获变量
       go func() {
          defer wg.Done()
          userinfo, err := uc.GetUserinfoByUIdVIdAId(ctx, comment.UserId, videoId)
          if err != nil {
             uc.log.Debug("Error getting user info: %v", err)
             return
          }
          commentresp := &pb.Comment{
             Id:         comment.Id,
             Content:    comment.Content,
             User:       userinfo,
             CreateDate: comment.CreateDate,
          }
          mu.Lock()
          commentList[i] = commentresp
          mu.Unlock()
       }()
    }

    wg.Wait()

    return commentList, nil
}
```

#### errgroup

1.**和sync.map结合使用**

2.errgroup 最常见的用法是结合 `context` 包使用，通过 `context` 来控制 goroutine 的生命周期，并且可以通过 `errgroup` 来等待并发操作的完成或处理错误。

它的主要方法是 `Go` 和 `Wait`：

- `Go`: 用于启动一个新的 goroutine，并监视这个 goroutine 的执行状态。
- `Wait`: 用于等待所有被 `Go` 启动的 goroutine 完成。

```Go
import (
        "context"
        "fmt"
        "golang.org/x/sync/errgroup"
)

func main() {
        g, ctx := errgroup.WithContext(context.Background())

        // 启动多个goroutine
        for i := 0; i < 5; i++ {
                // 使用errgroup.Go启动goroutine
                i := i // 使用局部变量i，避免闭包引用问题
                g.Go(func() error {
                        // 模拟一个可能出错的操作
                        if i == 2 {
                                return fmt.Errorf("error in goroutine %d", i)
                        }
                        fmt.Printf("goroutine %d completed\n", i)
                        return nil
                })
        }

        // 等待所有goroutine完成
        if err := g.Wait(); err != nil {
                fmt.Println("received error:", err)
        } else {
                fmt.Println("all goroutines completed successfully")
        }
}
```

#### 区别与选择

- **错误处理**：`sync.WaitGroup` 不提供错误处理机制，需要手动实现。如果需要处理并发任务中的错误，`errgroup` 是更好的选择。
- **取消机制**：`errgroup` 结合 `context.Context`，提供了任务取消和超时控制机制，而 `sync.WaitGroup` 仅负责等待所有任务完成。
- **简单性**：如果不需要错误处理和取消机制，`sync.WaitGroup` 更加简单直接。

### 通道

基于通信实现共享内存

1.通道（channel）是用来传递数据的一个数据结构。（其实就是一个环形队列） //多协程共享、协调同步 通道可用于两个 goroutine 之间通过传递一个指定类型的值来同步运行和通讯。操作符 `<-` 用于指定通道的方向，发送或接收。如果未指定方向，则为双向通道。

如果无法发送或接收，管道就会堵塞

```Go
ch <- v    // 把 v 发送到通道 ch
v := <-ch  // 从 ch 接收数据
           // 并把值赋给 v

select {
    case num1 := <-ch1:
        fmt.Println("Received from ch1:", num1)
    case num2 := <-ch2:
        fmt.Println("Received from ch2:", num2)
    }
```

2.声明一个通道

```Go
ch := make(chan int)
```

默认情况下，通道是不带缓冲区的 非缓冲通道意味着它**一次只能容纳一个值**，并且当发送者向通道写入数据时，它会**阻塞**直到接收者从通道中读取数据，反之亦然。

3.通道缓冲区 通道可以设置缓冲 区，通过 make 的第二个参数指定缓冲区大小：

```Go
ch := make(chan int, 100)
```

4.遍历通道与关闭通道 range 关键字

在 Go 语言中，使用 `range` 关键字可以对通道（channel）进行迭代，从通道接收数据直到通道关闭为止。使用 `range` 遍历通道可以更加简洁和直观地处理通道中的数据。

```Go
func main() {
        // 创建一个字符串类型的通道
        channel := make(chan string)

        // 启动一个 goroutine 发送数据到通道
        go func() {
                channel <- "Hello"
                channel <- "Go"
                close(channel) // 关闭通道
        }()

        // 使用 range 迭代通道中的数据，直到通道关闭为止
        for message := range channel {
                fmt.Println(message)
        }
}
```

需要注意的是，当通道被关闭后，`range` 循环会自动退出。因此，在使用 `range` 进行通道迭代时，通常不需要显示地检查通道是否关闭，因为 `range` 会自动感知通道的关闭状态并退出循环。

```Go
v, ok := <-ch
```

`ok` 为`false`：通道关闭且为空

```Go
延迟返回
go func(){
defer close(src)
for i:=0;i<5;i++{
   src <- i
} }
```

5.

一个消费者生产者例子

```Go
ch := make(chan int, 100)
wg := sync.WaitGroup{}

//两个生产者
wg.Add(2)
go func() {
    defer wg.Done()
    for i := 0; i < 10; i++ {
       ch <- i
    }
}()
go func() {
    defer wg.Done()
    for i := 0; i < 10; i++ {
       ch <- i
    }
}()
//wg.Wait()
//一个消费者
//可以利用管道来实现main堵塞，达到和 sync.WaitGroup 一样的功能
mc := make(chan struct{}) //使用struct{} 1.可读性强，明显可以看出不是充当容器的  2.空结构体不占内存空间
go func() {
    sum := 0
    for {
       a, ok := <-ch
       if !ok {
          break
       } else {
          sum += a
       }
    }
    fmt.Printf("sum=%d\n", sum)
    mc <- struct{}{}
}()

wg.Wait()
close(ch)
<-mc
```

6.死锁问题

当没有希望时，会直接结束go进程

但如果有个未知代码块时，会等待该代码,而不是立马结束go进程

如sleep(1*time.hour)

### 进程信息

![202407092303833](https://raw.githubusercontent.com/lanyoumeng/Drawing-bed/main/docs/202407111919747.png)

### 时间处理

time.Now()

time.Now().Unix() 时间戳 time.sleep() time.sleep( 50 * time.Millisecond )休眠50毫秒

```Go
// go中必须这样写
const (
        DATE = "2006-01-02"
        TIME = "2006-01-02 15:04:05"
)

func main() {
        t1 := time.Now()
        fmt.Println(t1.Unix())
        time.Sleep(50 * time.Millisecond)

        t2 := time.Now()

        //Time - Time = Duration
        //Duration是time包的一种数据类型
        t3 := t2.Sub(t1)
        fmt.Println(t3.Milliseconds()) //fmt.Println(time.Since(t1).Milliseconds())  等价于前两行

        //Time + Duration = Time
        d := time.Duration(50 * time.Millisecond)
        t4 := t1.Add(d)

        fmt.Println(t4.Unix())

        //转换成字符串，规范输出
        fmt.Println(t1.Format(DATE))
        fmt.Println(t1.Format(TIME))

        //字符串转换成时间数据类型
        s := "2023-07-13 22:58:44"
        // // t5, error := time.Parse(TIME, s)
        // t5, _ := time.Parse(TIME, s)
        // fmt.Println(t5)

        //确保时区为东八区
        loc, _ := time.LoadLocation("Asia/Shanghai")
        t5, _ := time.ParseInLocation(TIME, s, loc)
        fmt.Println(t5)
}
```

![202407092303417](https://raw.githubusercontent.com/lanyoumeng/Drawing-bed/main/docs/202407111920944.png)

### JSON

1.序列化的过程将数据转换为一串字节流或字符串，使得这些数据可以被写入文件、传输到网络上，或者存储在数据库中。反之，将这些序列化后的数据重新还原为内存中的数据结构的过程称为反序列化（Deserialization）。

**序列化是将数据结构转换（结构体）为一种序列化格式（例如JSON、XML等），以便于传输或存储。**

反序列化是将数据从一种序列化格式（例如JSON、XML等）转换回原始数据的过程。

```Go
        jsonData, err := json.Marshal(p)//序列化
        if err != nil {
                fmt.Println("JSON serialization error:", err)
                return
        }
        jsonStr := string(jsonData) //转换成字符串输出
        fmt.Println(jsonStr)
////////
        var p Person
        err := json.Unmarshal([]byte(jsonStr), &p)//反序列化
        if err != nil {
                fmt.Println("JSON deserialization error:", err)
                return
        }
        fmt.Printf("%+v",p)
```

2.sonic (字节) //更快

```Go
sonic.Marshal(p)
sonic.Unmarshal([]byte(jsonStr), &p)
```

### context

#### 介绍

在 Go 语言中，`context` 是用于在并发操作中传递请求范围的数据、取消信号和截止时间的工具。它是在标准库中提供的一个强大的工具，用于管理 Goroutine 的生命周期、控制超时和取消操作。

**1**.`context` 主要用于以下几个方面：

- **传递请求范围的数据：** 在并发的场景中，一个请求可能涉及多个 Goroutine 的处理。`context.Context` 类型允许您在 Goroutine 之间传递请求范围的数据，而无需显式地传递参数。
- **控制超时和取消操作：** 在一些场景中，可能需要控制某个 Goroutine 的执行时间，避免因为长时间的执行而导致性能问题。`context` 提供了超时和取消机制，可以设置 Goroutine 的截止时间或取消正在执行的操作。
- **跟踪 Goroutine：** 使用 `context` 可以创建一个包含请求范围信息的树状结构，从而可以跟踪所有相关联的 Goroutine。
- **上下文控制：**可以使用 Context 来控制 goroutine 的生命周期

**2**.底层结构

```Go
type Context interface {
    Deadline() (deadline time.Time, ok bool)
    Done() <-chan struct{}
    Err() error
    Value(key interface{}) interface{}
}
```

| 字段     | 含义                                                         |
| -------- | ------------------------------------------------------------ |
| Deadline | 返回一个time.Time，表示当前Context应该结束的时间，ok则表示有结束时间 |
| Done     | 当Context被取消或者超时时候返回的一个close的只读channel，告诉给context相关的函数要停止当前工作然后返回了。(这个有点像全局广播) |
| Err      | context被取消的原因                                          |
| Value    | context实现共享数据存储的地方，是协程**安全**的（还记得之前有说过map是不安全的？所以遇到map的结构,如果不是sync.Map,需要加锁来进行操作） |

**3**.**cancel**

在 Go 语言的 `context` 包中，`cancel()` 方法是 `context.Context` 类型的一个方法，用于取消派生的子 `context` 和它们所关联的 Goroutine。

取消操作可以用于优雅地终止 Goroutine 的执行，释放资源，避免资源泄漏或长时间阻塞等问题。在多个 Goroutine 之间协调取消操作可以保证应用程序的稳定性和可靠性。

defer cancel()

**4.使用**

原则：

1. 日常编写代码时，Context 对象会被被约定作为函数的**第一个参数**传递
2. 不要把context放到一个结构体中，应该作为第一个参数显式地传入函数
3. 即使方法允许，也不要传入一个nil的context，如果不确定需要什么context的时候，传入一个context.TODO
4. 使用context的Value相关方法应该传递和请求相关的元数据，不要用它来传递一些可选参数
5. 同样的context可以传递到多个goroutine中，Context在多个goroutine中是安全的
6. 在子context传入goroutine中后，应该在子goroutine中对该子context的Done channel进行监控，一旦该channel被关闭，应立即终止对当前请求的处理，并释放资源。

`context` 包含以下几个主要的方法：

- `context.Background()`：创建一个空的 `context.Context`，用作**根节点**。在一个请求范围内，通常从这个根节点派生其他子节点的 `context`。
- `context.TODO()`：类似于 `context.Background()`，但在还不确定应该使用哪种 `context` 的情况下使用。
- `context.WithCancel(parentContext)`：创建一个带有取消信号的子 `context`。当父 `context` 被取消时（ cancel()方法 ），所有派生的子 `context` 也会被取消。
- `context.WithDeadline(parentContext, deadline)`：创建一个带有截止时间的子 `context`。在截止时间之后，子 `context` 会被自动取消。 父节点过期时，所有的子孙节点必须同时关闭。
- `context.WithTimeout(parentContext, timeout)`：类似于 `context.WithDeadline()`，但截止时间是相对于当前时间的一个**相对时间**段。
- `context.WithValue(parentContext, key, value)`：创建一个带有请求范围数据的子 `context`。可以在 Goroutine 之间传递数据，但不适合传递取消信号。键和值都是空接口类型`interface{}`。(gin框架中有set和get方法)

使用 `context` 示例：

```Go
//context.Background()和context.WithCancel()用法
func worker(ctx context.Context) {
    for {
        select {
        case <-ctx.Done():
            fmt.Println("Worker cancelled")
            return
        default:
            fmt.Println("Working...")
            time.Sleep(1 * time.Second)
        }
    }
}

func main() {
    parentCtx := context.Background()
    ctx, cancel := context.WithCancel(parentCtx)

    go worker(ctx)

    time.Sleep(3 * time.Second)
    cancel() // 取消 Goroutine

    time.Sleep(2 * time.Second)
}
在上述示例中，我们创建了一个简单的 `worker` Goroutine，该 Goroutine 不断地打印 "Working..."，并模拟了一个耗时的工作。在 `main` 函数中，我们创建了一个 `context`，并在 3 秒后调用 `cancel()` 方法来取消 Goroutine。这会导致 `worker` Goroutine 打印 "Worker cancelled" 并结束。

//WithValue() 方法
type key int

const (
    userKey key = iota
)

func users(ctx context.Context, req *Request) {
    // 从请求中获取用户信息
    user := req.GetUser
    // 将用户信息保存到 Context 中
    ctx = context.WithValue(ctx, userKey, user)
    // 启动一个 goroutine 来处理请求
    go func(ctx context.Context) {
        // 从 Context 中获取用户信息
        user := ctx.Value(userKey).(*User)
        // 处理请求...
    }(ctx)
}
  
//WithDeadline() 设置截止时间
func users(ctx context.Context, req *Request) {
    // 设置请求的截止时间为当前时间加上 1 秒钟
    //ctx, cancel := context.WithDeadline(ctx, time.Now().Add(time.Second))
    
    // 设置请求的超时时间为 1 秒钟
    ctx, cancel := context.WithTimeout(ctx, time.Second)

    // 启动一个 goroutine 来处理请求
    go func(ctx context.Context) {
        // 等待请求完成或者超时
        select {
            case <-time.After(time.Millisecond * 500): //这里定时器比较特殊用于在0.5s内完成
            // 请求完成
            fmt.Println("Request finish")
            case <-ctx.Done():
            // 请求超时或者被取消
            fmt.Println("Request canceled or timed out")
        }
    }(ctx)

    // 等待一段时间后取消请求
    time.Sleep(time.Millisecond * 1500)
    cancel()
}
在 select 语句中，case <-time.After(duration) 是一个通道操作，其中 time.After(duration) 返回一个通道（<-chan time.Time），在指定的时间 duration 后，该通道会发送一个当前时间的值。 是 Go 语言中 select 语句中的一种特殊用法，用于设置一个定时器来控制超时。
```

`context` 在 Go 语言中的使用非常重要，尤其在处理并发操作时，它可以帮助您管理 Goroutine 的生命周期，控制超时和取消操作，以及传递请求范围的数据。正确使用 `context` 可以提高程序的稳定性和可维护性。

#### gin.Context和context.Context

##### 一

Gin框架中使用`gin.Context`来获取请求的参数、设置响应、记录日志等 使用标准库的`context.Context`来处理更一般的跨goroutine的上下文传递。

在Go语言中，`context.Context`和`gin.Context`是两种不同的上下文类型，用于在程序中传递请求范围的信息，但它们有不同的用途和功能。

1. `context.Context`：
   1. `context.Context`是Go标准库`context`中定义的核心类型，用于在多个goroutine之间传递请求范围的数据、取消信号和截止时间等。
   2. `context.Context`通常用于在Go程序中传递请求范围的上下文信息，例如请求ID、用户信息、截止时间等。
   3. 可以通过`context.Background()`或`context.TODO()`创建一个新的`context.Context`，并使用`context.WithValue`等函数将额外的信息附加到上下文中，以便在程序中传递和使用这些信息。
2. `gin.Context`：
   1. `gin.Context`是Gin框架中定义的上下文类型，用于在**HTTP请求**处理函数之间传递请求范围的信息。
   2. `gin.Context`是基于`context.Context`的，它实际上是在`context.Context`上增加了一些框架特定的功能，例如方便的获取请求参数、设置响应状态码、记录日志等。
   3. 在Gin框架中，每个HTTP请求处理函数都会接收一个`gin.Context`参数，用于访问请求信息、写入响应和操作上下文中的数据。

##### 二

`*gin.Context` 和 `context.Context` 不是同一种类型，虽然它们都涉及到上下文（Context）的概念，但是用途和实现是不同的。

1. `*gin.Context`：
   1. `*gin.Context` 是 Gin 框架中的类型，用于表示 HTTP 请求的上下文。
   2. 它包含了与当前 HTTP 请求相关的信息，如请求头、查询参数、路由参数、响应数据等。
   3. `*gin.Context` 是 Gin 框架为处理 HTTP 请求而设计的一种数据结构，通常用于编写 Web 应用程序的处理程序。
2. `context.Context`：
   1. `context.Context` 是 Go 标准库中的类型，用于处理上下文信息的传递和取消。
   2. 它通常用于在函数调用链中传递上下文信息，以便实现请求范围的取消、超时、截止日期等功能。
   3. `context.Context` 用于处理更广泛的并发控制和上下文传递，而不仅仅是处理 HTTP 请求。

尽管它们都涉及到上下文的概念，但它们有不同的用途和实现方式。`*gin.Context` 主要用于在 Web 应用程序中处理 HTTP 请求，而 `context.Context` 用于更通用的并发和上下文传递场景。

在某些情况下，你可以将 `*gin.Context` 转换为 `context.Context` 或将 `context.Context` 注入到 `*gin.Context` 中，以便在 Web 应用程序中实现更高级的并发控制或上下文传递，但它们仍然是不同的类型，用于不同的目的。

##### 获取值

`ctx.Value` 和 Gin 框架的 `ctx.Get` 都用于从上下文（Context）中获取值

1. **`ctx.Value(key interface{}) interface{}`**：
   1. `ctx.Value` 是 Go 标准库中的 `context.Context` 接口的方法。
   2. 它允许您从上下文中获取与指定键相关联的值。键和值都可以是任意类型。
   3. 您需要指定键（通常是字符串或自定义类型），然后 `ctx.Value(key)` 返回与该键关联的值，或者返回 `nil`（如果键不存在）。
   4. 适用于 Go 标准库的 `context.Context`，通常用于跨函数传递上下文信息。

```Go
value := ctx.Value("myKey")
```

1. **`ctx.Get(key string) (value interface{}, isExist bool)`**：
   1. `ctx.Get` 是 Gin 框架的方法，用于从 Gin 上下文（Gin Context）中获取值。
   2. 它允许您通过字符串键获取与该键相关联的值，并返回一个布尔值来指示是否存在这个键。
   3. Gin 的上下文是基于 Go 的 `context.Context` 的扩展，用于处理 HTTP 请求，并且包括更多的功能和方法，其中包括 `ctx.Get`。
   4. 通常用于 Gin 框架的请求处理中。

```Go
value, exists := ctx.Get("myKey")
```

总的来说，`ctx.Value` 是 Go 标准库 `context.Context` 接口提供的方法，用于通用的上下文值传递，而 `ctx.Get` 是 Gin 框架的扩展方法，用于从 Gin 上下文中获取值，通常在 Gin 的请求处理中使用。如果您使用 Gin 框架处理 HTTP 请求，您可以使用 `ctx.Get` 来访问请求特定的值。如果您需要在不涉及 HTTP 请求处理的上下文传递中使用上下文值，您可以使用 `ctx.Value`。

虽然`gin.Context`是基于`context.Context`的，但是它在HTTP请求处理中提供了许多方便的功能和方法，使得在Gin框架中处理HTTP请求更加简洁和高效。你可以在

### 文件

```Go
//获得当前目录
        currentDir, err := os.Getwd()
        if err != nil {
                fmt.Println("获取当前目录失败:", err)
                return
        }

        fmt.Println("当前目录:", currentDir)
```

1.os包

读写 *os.File 的read和 write / WriteString

写 os.WriteFile

2.bufio包

读 bufio.NewReader(f) ---> Read和ReadString

写 bufio.NewWriter(f) ---> Write和WriteString

3.io包 （ioutil在Go 1.16版本以后被废弃，用io和os替换）

读 io.ReadAll(file) 和 io.ReadFile(filepath)

写 io.WriteString(file, s)

#### 读取文件

读取文件有三种方式：

- 按字节数读取
- 按行读取
- 将文件整个读入内存

1.字节

```Go
////////使用os.Open()和io.Read函数来进行自定义的读取操作：
func main() {

   file := "D:/gopath/src/golang_development_notes/example/log.txt"
   f, err := os.Open(file)
   if err != nil {
       fmt.Println("无法打开文件:", err)
       return
   }
   defer f.Close()

   chunks := make([]byte, 0)
   buf := make([]byte, 1024)
   for {
      n, err := f.Read(buf)
            if err != nil {
                        if err != io.EOF {
                                fmt.Println("读取文件出错:", err)
                        }
                        break
                }
      chunks = append(chunks, buf[:n]...)
   }
   fmt.Println(string(chunks))
}
////使用bufio.NewReader包装的缓冲流
func main() {
   filepath := "D:/gopath/src/golang_development_notes/example/log.txt"
   fi, err := os.Open(filepath)
   if err != nil {
            fmt.Println("无法打开文件:", err)
       return
   }
   defer fi.Close()
   r := bufio.NewReader(fi)

   chunks := make([]byte, 0)
   buf := make([]byte, 1024) //一次读取多少个字节
   for {
      n, err := r.Read(buf)
      if err != nil && err != io.EOF {
         panic(err)
      }
      if 0 == n {
         break
      }
      chunks = append(chunks, buf[:n]...)
   }
   fmt.Println(string(chunks))
}
```

2.行

```Go
func main() {
        filepath := "a.txt"
        file, err := os.OpenFile(filepath, os.O_RDWR, 0666)
        if err != nil {
                fmt.Println("Open file error!", err)
                return
        }
        defer file.Close()

        stat, err := file.Stat()
        if err != nil {
                panic(err)
        }
        var size = stat.Size()
        fmt.Println("file size=", size)

        buf := bufio.NewReader(file)
        for {
                line, err := buf.ReadString('\n') //到结束位置时也会将最后一行读入
                if err != nil {
                        if err == io.EOF { //要输出最后一行
                                fmt.Print(line)
                                fmt.Println("File read ok!")
                                break
                        } else {
                                fmt.Println("Read file error!", err)
                                return
                        }
                } else {
                        fmt.Print(line)
                }
        }
}
```

3.整个文件

```Go
func main() {
   file, err := os.Open("D:/gopath/src/golang_development_notes/example/log.txt")
   if err != nil {
      panic(err)
   }
   defer file.Close()
   content, err := io.ReadAll(file)
   fmt.Println(string(content))
}

////////
func main() {
   filepath := "D:/gopath/src/golang_development_notes/example/log.txt"
   content ,err :=io.ReadFile(filepath)
   if err !=nil {
      panic(err)
   }
   fmt.Println(string(content))
}
```

#### 写入文件

写入文件有三种方式：

- os.WriteFile
- io.WriteString(file, s)
- f.Write 和 f.WriteString
- 

1.`os.WriteFile`函数会直接写入文件并覆盖已存在的内容

```Go
content := []byte("测试1\n测试2\n")
        err := os.WriteFile("a.txt", content, 0644)
        if err != nil {
                panic(err)
        }
```

2.io.WriteString(f, s)可以在文件内容末尾添加新内容。

```Go
func checkFileIsExist(filename string) bool {
        if _, err := os.Stat(filename); os.IsNotExist(err) {
                return false
        }
        return true
}
func main() {
        s := "测试1\n测试2\n"
        var filename = "./test.txt"
        var f *os.File
        var err1 error

        if checkFileIsExist(filename) { //如果文件存在
                f, err1 = os.OpenFile(filename, os.O_APPEND, 0666) //打开文件
                if err1 != nil {
                        panic(err1)
                }
                fmt.Println("文件存在")
        } else {
                f, err1 = os.Create(filename) //创建文件
                if err1 != nil {
                        panic(err1)
                }
                fmt.Println("文件不存在")
        }
        defer f.Close()
        
        n, err1 := io.WriteString(f, s) //写入文件(字符串)
        if err1 != nil {
                panic(err1)
        }
        fmt.Printf("写入 %d 个字节n", n)
}
```

3.*os.File 的 f.Write 和 f.WriteString

```Go
        n, err1 := f.Write([]byte(str)) //写入文件(字节数组)
        fmt.Printf("写入 %d 个字节n", n)

        n, err1 = f.WriteString(str) //写入文件(字符串)
        if err1 != nil {
                panic(err1)
        }
        fmt.Printf("写入 %d 个字节n", n)
        f.Sync()
```

### 正则表达式

`regexp` 是 Go 语言标准库中的正则表达式包，用于在字符串中进行模式匹配和文本搜索。它提供了一种强大的方式来查找、提取或替换文本中的特定模式。正则表达式是一种用于描述文本模式的表达式，允许您执行复杂的字符串匹配操作。

下面是一些常用的 `regexp` 包的功能和方法：

1. **编译正则表达式**：使用 `regexp.Compile()` 或 `regexp.MustCompile()` 函数可以将正则表达式编译为可用于匹配的 `*Regexp` 对象。`MustCompile` 在编译失败时会引发恐慌，而 `Compile` 返回错误。
2. **执行匹配**：使用 `Regexp` 对象的 `Match()`、`MatchString()`、`MatchReader()` 等方法来执行正则表达式匹配。它们可用于检查字符串是否匹配正则表达式。
3. **查找匹配**：使用 `Find()`、`FindString()`、`FindAll()` 等方法来查找字符串中的正则表达式匹配项，返回匹配的部分。
4. **提取匹配**：使用 `Submatch()`、`SubmatchIndex()` 等方法来提取匹配的子字符串。
5. **替换文本**：使用 `ReplaceAll()` 和 `ReplaceAllString()` 方法可以用指定的字符串替换匹配的部分。
6. **正则表达式语法**：`regexp` 使用的正则表达式语法基本上遵循 POSIX 扩展正则表达式的语法，支持字符类、分组、量词、锚点等。

以下是一个简单的示例，演示了如何使用 `regexp` 包来执行基本的正则表达式匹配：

```Go
package main

import (
        "fmt"
        "regexp"
)

func main() {
        text := "Hello, my email is user@example.com, and another email is admin@example.com."

        // 定义正则表达式模式
        pattern := `[a-zA-Z0-9._%+-]+@[a-zA-Z0-9.-]+\.[a-zA-Z]{2,4}`

        // 编译正则表达式
        re := regexp.MustCompile(pattern)

        // 查找第一个匹配项
        match := re.FindString(text)
        fmt.Println("First match:", match)

        // 查找所有匹配项
        matches := re.FindAllString(text, -1)
        fmt.Println("All matches:", matches)
}
```

在上面的示例中，我们使用 `regexp` 包来查找文本中的电子邮件地址。首先，我们定义了一个正则表达式模式，然后编译它，并使用 `FindString` 和 `FindAllString` 方法来查找匹配项。这个示例只是一个入门示例，`regexp` 包提供了更多高级功能和选项，以满足各种复杂的匹配需求。

### 嵌入系统

能够将静态资源打包到二进制包中，部署过程更简单。传统部署要么需要将静态资源与已编译程序打包在一起上传，或者使用 docker 和 dockerfile 自动化前者，这是很麻烦的。 确保程序的完整性。在运行过程中损坏或丢失静态资源通常会影响程序的正常运行。 静态资源访问没有 io 操作，速度会非常快。 embed 基础用法 通过 官方文档 我们知道 embed 嵌入的三种方式：string、bytes 和 FS（File Systems）。其中 string 和 []byte 类型都只能匹配一个文件，如果要匹配多个文件或者一个目录，就要使用 embed.FS 类型。

特别注意：embed 这个包一定要导入，如果导入不使用的话，使用 _ 导入即可

一、嵌入为字符串 比如当前文件下有个 hello.txt 的文件，文件内容为 hello,world!。通过 go:embed 指令，在编译后下面程序中的 s 变量的值就变为了 

```go
hello,world!。

package main
import ( _ "embed" "fmt" )
//go:embed hello.txt 
var s string 
func main() { 
    fmt.Println(s)
} 
```

二、嵌入为 byte slice 你还可以把单个文件的内容嵌入为 slice of byte，也就是一个字节数组。

```go
package main
import ( _ "embed" "fmt" )
//go:embed hello.txt 
var b []byte 
func main() { 
    fmt.Println(b) 
} 
```

三、嵌入为 fs.FS 甚至你可以嵌入为一个文件系统，这在嵌入多个文件的时候非常有用。

比如嵌入一个文件：



```
package main
import (
"embed"
"fmt" 
) 
//go:embed hello.txt
var f embed.FS 
func main() { 
data, _ := f.ReadFile("hello.txt")
fmt.Println(string(data))
} 
```



嵌入本地的另外一个文件 hello2.txt, 支持同一个变量上多个 go:embed 指令 (嵌入为 string 或者 byte slice 是不能有多个 go:embed 指令的):

```
package main
import (
"embed"
"fmt" 
) 
//go:embed hello.txt 
//go:embed hello2.txt 
var f embed.FS 
func main() { 
data, _ := f.ReadFile("hello.txt") 
fmt.Println(string(data)) 
data, _ = f.ReadFile("hello2.txt") 
fmt.Println(string(data)) 
} 
```



当前重复的 go:embed 指令嵌入为 embed.FS 是支持的，相当于一个:

```go
package main 
import ( 
    "embed" 
    "fmt" ) 
//go:embed hello.txt 
//go:embed hello.txt 
var f embed.FS 
func main() {
    data, _ := f.ReadFile("hello.txt")
    fmt.Println(string(data)) 
} 
```



还可以嵌入子文件夹下的文件：

```go
package main
import ( 
    "embed" 
    "fmt" 
) 
//go:embed p/hello.txt 
//go:embed p/hello2.txt 
var f embed.FS 
func main() { 
    data, _ := f.ReadFile("p/hello.txt") 
    fmt.Println(string(data))
    data, _ = f.ReadFile("p/hello2.txt") 
    fmt.Println(string(data))
} 
```



embed 进阶用法 Go1.16 为了对 embed 的支持也添加了一个新包 io/fs。两者结合起来可以像之前操作普通文件一样。

一、只读 嵌入的内容是只读的。也就是在编译期嵌入文件的内容是什么，那么在运行时的内容也就是什么。

FS 文件系统值提供了打开和读取的方法，并没有 write 的方法，也就是说 FS 实例是线程安全的，多个 goroutine 可以并发使用。

embed.FS 结构主要有 3 个对外方法，如下：

```
// Open 打开要读取的文件，并返回文件的fs.File结构. func (f FS) Open(name string) (fs.File, error)

// ReadDir 读取并返回整个命名目录 func (f FS) ReadDir(name string) ([]fs.DirEntry, error)

// ReadFile 读取并返回name文件的内容. func (f FS) ReadFile(name string) ([]byte, error) 
```

二、嵌入多个文件 package main

```go
import ( "embed" "fmt" )

//go:embed hello.txt hello2.txt var f embed.FS

func main() { 
    data, _ := f.ReadFile("hello.txt")
    fmt.Println(string(data))
    data, _ = f.ReadFile("hello2.txt")
    fmt.Println(string(data))
} 
```



当然你也可以像前面的例子一样写成多行 go:embed:

```go
package main 
import (
    "embed"
    "fmt" 
) 
//go:embed hello.txt 
//go:embed hello2.txt 
var f embed.FS 
func main() { 
    data, _ := f.ReadFile("hello.txt")
    fmt.Println(string(data)) 
    data, _ = f.ReadFile("hello2.txt") 
    fmt.Println(string(data)) 
} 
```



三、支持文件夹 文件夹分隔符采用正斜杠 /, 即使是 windows 系统也采用这个模式。

```go
package main 
import ( 
    "embed" 
    "fmt" 
) 
//go:embed p
var f embed.FS 
func main() { 
    data, _ := f.ReadFile("p/hello.txt")
    fmt.Println(string(data)) 
    data, _ = f.ReadFile("p/hello2.txt") 
    fmt.Println(string(data))
} 
```



应用 在我们的项目中，是将应用的常用的一些配置写在了.env 的一个文件上，所以我们在这里就可以使用 go:embed 指令。



```go
main.go 文件：

//go:embed ".env" "v1d0/.env"
var FS embed.FS

func main(){
    setting.InitSetting(FS)
    manager.InitManager() 
    cron.InitCron() 
    routersInit := routers.InitRouter() 
    readTimeout := setting.ServerSetting.ReadTimeout 
    writeTimeout := setting.ServerSetting.WriteTimeout 
    endPoint := fmt.Sprintf(":%d", setting.ServerSetting.HttpPort) 
    maxHeaderBytes := 1 << 20 
    server := &http.Server{
        Addr: endPoint,
        Handler: routersInit, 
        ReadTimeout: readTimeout, 
        WriteTimeout: writeTimeout, 
        MaxHeaderBytes: maxHeaderBytes,
    } 
    server.ListenAndServe() 
}

setting.go 文件：

func InitSetting(FS embed.FS) { 
    // 总配置处理 
    var err error
    data, err := FS.ReadFile(".env") 
    if err != nil {
        log.Fatalf("Fail to parse '.env': %v", err) 
    } 
    cfg, err = ini.Load(data) 
    if err != nil { 
        log.Fatal(err)
    } 
    mapTo("server", ServerSetting) 
    ServerSetting.ReadTimeout = ServerSetting.ReadTimeout * time.Second 
    ServerSetting.WriteTimeout = ServerSetting.WriteTimeout * time.Second 
    // 分版本配置引入 
    v1d0Setting.InitSetting(FS) 
}
```



## 数据结构



## GMP

go 1.19

### 介绍

gmp = goroutine + machine + processor （+ 一套有机组合的机制）

**P的个数默认等于CPU核数，M的个数会略大于P的个数**

g是在用户态

g：

- （1）g 即goroutine，是 golang 中对协程的抽象；
- （2）g 有自己的运行栈、状态、以及执行的任务函数（用户通过 go func 指定）；
- （3）g 需要绑定到 p 才能执行，在 g 的视角中，p 就是它的 cpu.

m：

- （1）m 即 machine，是 golang 中对线程的抽象；

- （2）m 不直接执行 g，而是先和 p 绑定，由其实现代理；

- （3）借由 p 的存在，m 无需和 g 绑死，也无需记录 g 的状态信息，因此 g 在全生命周期中可以实现跨 m 执行.

- 一般情况下M的个数会略大于P的个数，这多出来的M将会在G产生系统调用时发挥作用。类似线程池，

- 如图所示，当G0即将进入系统调用时，M0将释放P，进而某个空闲的M1获取P，继续执行P队列中剩下的G。而M0由于陷入系统调用而进被阻塞，M1接替M0的工作，只要P不空闲，就可以保证充分利用CPU。

  M1的来源有可能是M的缓存池，也可能是新建的。当G0系统调用结束后，跟据M0是否能获取到P，将会将G0做不同的处理：

  1. 如果有空闲的P，则获取一个P，继续执行G0。
  2. 如果没有空闲的P，则将G0放入全局队列，等待被其他的P调度。然后M0将进入缓存池睡眠。

- <img src="https://raw.githubusercontent.com/lanyoumeng/Drawing-bed/main/docs/202409031740095.png" alt="image-20240903174027792" style="zoom:50%;" />

 p:

- （1）p 即 processor，是 golang 中的调度器；
- （2）p 是 gmp 的中枢，借由 p 承上启下，实现 g 和 m 之间的动态有机结合；
- （3）对 g 而言，p 是其 cpu，g 只有被 p 调度，才得以执行；
- （4）对 m 而言，p 是其执行代理，为其提供必要信息的同时（可执行的 g、内存分配情况等），并隐藏了繁杂的调度细节；
- （5）**p 的数量决定了 g 最大并行数量**，可由用户通过 GOMAXPROCS 进行设定（超过 CPU 核数时无意义）.
- P的个数在程序启动时决定，默认情况下**等同于CPU的核数**，由于M必须持有一个P才可以运行Go代码，所以同时运行的M个数，也即线程数一般等同于CPU的个数，以达到尽可能的使用CPU而又不至于产生过多的线程切换开销。

 





### 对比

三个模型的各项能力对比如下:

| **模型**  | **弱依赖内核** | **可并行** | **可应对阻塞** | **栈可动态扩缩** |
| --------- | -------------- | ---------- | -------------- | ---------------- |
| 线程      | ❎              | ✅          | ✅              | ❎                |
| 协程      | ✅              | ❎          | ❎              | ❎                |
| goroutine | ✅              | ✅          | ✅              | ✅                |



通常语义中的线程，指的是内核级线程，核心点如下：

（1）是操作系统最小调度单元；

（2）创建、销毁、调度交由内核完成，cpu 需完成用户态与内核态间的切换；

（3）可充分利用多核，实现并行.

线程数过多，意味着操作系统会不断的切换线程，频繁的上下文切换就成了性能瓶颈。



协程，又称为用户级线程，核心点如下：

（1）与线程存在映射关系，为 M：1；

（2）创建、销毁、调度在用户态完成，对内核透明，所以更轻；

（3）从属同一个内核级线程，无法并行；一个协程阻塞会导致从属同一线程的所有协程无法执行.

<img src="https://raw.githubusercontent.com/lanyoumeng/Drawing-bed/main/docs/202409031220675.webp" alt="640" style="zoom:50%;" />

**Goroutine**，经 Golang 优化后的特殊“协程”，核心点如下：

（1）与线程存在映射关系，为 M：N；

（2）创建、销毁、调度在用户态完成，对内核透明，足够轻便；

（3）可利用多个线程，实现并行；

（4）通过调度器的斡旋，实现和线程间的动态绑定和灵活调度；

（5）栈空间大小可动态扩缩，因地制宜.

<img src="https://raw.githubusercontent.com/lanyoumeng/Drawing-bed/main/docs/202409031222009.webp" alt="640 (1)" style="zoom:50%;" />



### 宏观模型

GMP 宏观模型如上图所示，下面对其要点和细节进行逐一介绍：

（1）M 是线程的抽象；G 是 goroutine；P 是承上启下的调度器；

（2）M调度G前，需要和P绑定；

（3）全局有多个M和多个P，但同时并行的G的最大数量等于P的数量；

（4）G的存放队列有三类：P的本地队列；全局队列；和wait队列（图中未展示，为io阻塞就绪态goroutine队列）；

（5）M调度G时，优先取P本地队列，其次取全局队列，最后取wait队列；这样的好处是，取本地队列时，可以接近于无锁化，减少全局锁竞争；为了防止全局队列饥饿：每61次调度则去全局可执行队列中获取一个g执行

（6）为防止不同P的闲忙差异过大，设立work-stealing机制，本地队列为空的P可以尝试从其他P本地队列**偷取一半**的G补充到自身队列.所以本地队列还是有并发访问需要加锁，但是发生很少接近于无锁化。偷取时遍历的起点是随机的



<img src="https://raw.githubusercontent.com/lanyoumeng/Drawing-bed/main/docs/202409031228194.webp" alt="640 (2)" style="zoom:50%;" />

### GOMAXPROCS设置对性能的影响

一般来讲，程序运行时就将GOMAXPROCS大小设置为CPU核数，可让Go程序充分利用CPU。
在某些IO密集型的应用里，这个值可能并不意味着性能最好。理论上当某个Goroutine进入系统调用时，会有一个新的M被启用或创建，继续占满CPU。
但由于Go调度器检测到M被阻塞是有一定**延迟**的，也即旧的M被阻塞和新的M得到运行之间是有一定间隔的，所以在IO密集型应用中不妨把GOMAXPROCS设置的大一些，或许会有好的效果。



### 调度

#### g0和g

<img src="https://raw.githubusercontent.com/lanyoumeng/Drawing-bed/main/docs/202409031748129.webp" alt="640 (4)" style="zoom:50%;" />

goroutine 的类型可分为两类：

I 负责调度普通 g 的 g0，执行固定的调度流程，与 m 的关系为一对一；

II 负责执行用户函数的普通 g.

m 通过 p 调度执行的 goroutine 永远在普通 g 和 g0 之间进行切换，当 g0 找到可执行的 g 时，会调用 gogo 方法，调度 g 执行用户定义的任务；当 g 需要主动让渡或被动调度时，会触发 mcall 方法，将执行权重新交还给 g0.

gogo 和 mcall 可以理解为对偶关系，其定义位于 runtime/stubs.go 文件中.

```
func gogo(buf *gobuf)
// ...
func mcall(fn func(*g))
```



特殊的g0本身是和m一一对应的. 而m又只有建立了和p的绑定之后才能运作. 所以g0这部分工作在逻辑上可以视为是由p执行的

Go 调度器是一个更宽泛的概念，P 主要还是起到存储 G 的作用。



#### 四种调度

<img src="https://raw.githubusercontent.com/lanyoumeng/Drawing-bed/main/docs/202409031747955.webp" alt="640 (3)" style="zoom:50%;" />

通常，调度指的是由 g0 按照特定策略找到下一个可执行 g 的过程. 而本小节谈及的调度类型是广义上的“调度”，指的是调度器 p 实现从执行一个 g 切换到另一个 g 的过程

（1）主动调度

一种用户主动执行让渡的方式，主要方式是，用户在执行代码中调用了 runtime.Gosched 方法，此时当前 g 会当让出执行权，主动进行队列等待下次被调度执行.

（2）被动调度

因当前不满足某种执行条件，g 可能会陷入阻塞态无法被调度，直到关注的条件达成后，g才从阻塞中被唤醒，重新进入可执行队列等待被调度.

常见的被动调度触发方式为因 channel 操作或互斥锁操作陷入阻塞等操作，底层会走进 gopark 方法.

（3）正常调度：

g 中的执行任务已完成，g0 会将当前 g 置为死亡状态，发起新一轮调度.

（4）抢占调度：

倘若 g 执行系统调用超过指定的时长，且全局的 p 资源比较紧缺，此时将 p 和 g 解绑，抢占出来用于其他 g 的调度. 等 g 完成系统调用后，会重新进入可执行队列中等待被调度.

值得一提的是，前 3 种调度方式都由 m 下的 g0 完成，唯独抢占调度不同.

因为发起系统调用时需要打破用户态的边界进入内核态，此时 m 也会因系统调用而陷入僵直，无法主动完成抢占调度的行为.

因此，在 Golang 进程会有一个全局监控协程 monitor g 的存在，这个 g 会越过 p 直接与一个 m 进行绑定，不断轮询对所有 p 的执行状况进行监控. 倘若发现满足抢占调度的条件，则会从第三方的角度出手干预，主动发起该动作

## 内存管理

### 内存分配

#### Golang 内存模型

![640 (7)](https://raw.githubusercontent.com/lanyoumeng/Drawing-bed/main/docs/202409032106193.webp)

1. **以空间换时间，一次缓存，多次复用**

   由于每次向操作系统申请内存的操作很重，那么不妨一次多申请一些，以备后用.

   Golang 中的堆 mheap 正是基于该思想，产生的数据结构. 我们可以从两个视角来解决 Golang 运行时的堆：

   I 对操作系统而言，这是用户进程中缓存的内存

   II 对于 Go 进程内部，堆是所有对象的内存起源 

2. **多级缓存，实现无/细锁化**

   <img src="https://raw.githubusercontent.com/lanyoumeng/Drawing-bed/main/docs/202409032059101.webp" alt="640 (5)" style="zoom:50%;" />

   堆是 Go 运行时中最大的临界共享资源，这意味着每次存取都要加锁，在性能层面是一件很可怕的事情.

   在解决这个问题，Golang 在**堆 mheap** 之上，依次细化粒度，建立了 mcentral、mcache 的模型，下面对三者作个梳理：

   - • mheap：全局的内存起源，访问要加全局锁
   - • mcentral：每种对象大小规格（全局共划分为 68 种）对应的缓存，锁的粒度也仅限于同一种规格以内
   - • mcache：每个 P（正是 GMP 中的 P）持有一份的内存缓存，访问时无锁

   对于区分堆栈内存来说， mheap central mcache 都算是堆内存

3.  **多级规格，提高利用率**

   <img src="https://raw.githubusercontent.com/lanyoumeng/Drawing-bed/main/docs/202409032105506.webp" alt="640 (6)" style="zoom:50%;" />

   首先理下 page 和 mspan 两个概念：

   （1）page：最小的存储单元.

   Golang 借鉴操作系统分页管理的思想，每个最小的存储单元也称之为页 page，但大小为 8 KB

   （2）mspan：最小的管理单元.

   mspan 大小为 page 的整数倍，且从 8B 到 80 KB 被划分为 67 种不同的规格，分配对象时，会根据大小映射到不同规格的 mspan，从中获取空间.

   于是，我们回头小节多规格 mspan 下产生的特点：

   I 根据规格大小，产生了等级的制度

   II 消除了外部碎片，但不可避免会有内部碎片

   III 宏观上能提高整体空间利用率

   IV 正是因为有了规格等级的概念，**才支持 mcentral 实现细锁化**



#### 内存单元 mspan

<img src="https://raw.githubusercontent.com/lanyoumeng/Drawing-bed/main/docs/202409032108544.webp" alt="640 (8)" style="zoom:50%;" />

分点阐述 mspan 的特质：

- • mspan 是 Golang 内存管理的最小单元
- • mspan 大小是 page 的整数倍（Go 中的 page 大小为 8KB），且内部的页是连续的（至少在虚拟内存的视角中是这样）
- • 每个 mspan 根据空间大小以及面向分配对象的大小，会被划分为不同的等级（2.2小节展开）
- • 同等级的 mspan 会从属同一个 mcentral，最终会被组织成链表，因此带有前后指针（prev、next）
- • 由于同等级的 mspan 内聚于同一个 mcentral，所以会基于同一把互斥锁管理
- • mspan 会基于 bitMap 辅助快速找到空闲内存块（块大小为对应等级下的 object 大小），此时需要使用到 Ctz64 算法.

<img src="https://raw.githubusercontent.com/lanyoumeng/Drawing-bed/main/docs/202409032109072.webp" alt="640 (9)" style="zoom:50%;" />

### 垃圾回收





### 逃逸分析







## 常用函数

### 1.随机数

rand.Seed(time.Now().UnixNano()) //设置随机数种子 selectNumber := rand.Intn(maxNum)

### 2.修剪字符串后缀

TrimSuffix(s, suffix string) 返回没有提供的尾随后缀字符串的 s。如果 s 不以 suffix 结尾，则 s 原样返回

*strings.TrimSuffix(s ,"\r\n") //windows的换行符修剪*

### 3.fmt.Sprintf

将格式化的字符串生成并返回一个新的字符串。该函数类似于 `Printf`，但不会将结果打印到标准输出，而是将结果作为字符串返回，供程序进一步使用。

在输出文本、日志记录以及构造复杂字符串时非常有用的函数。

```Go
func Sprintf(format string, a ...interface{}) string
```

- `format`：格式化字符串，可以包含占位符（例如 `%d`, `%s`, `%f` 等），用来指定将要格式化的值的类型和输出的格式。
- `a ...interface{}`：可变参数列表，可以传入任意数量的参数，用于替换格式化字符串中的占位符。

返回值：

- `string`：返回生成的格式化后的字符串。

使用 `fmt.Sprintf`，你可以根据指定的格式将不同类型的值转换为字符串。以下是一个简单的示例：

```Go
func main() {
        name := "Alice"
        age := 30

        // 使用 Sprintf 将字符串和整数格式化为新的字符串
        message := fmt.Sprintf("My name is %s and I am %d years old.", name, age)

        fmt.Println(message) // 输出："My name is Alice and I am 30 years old."
}
```

### 4.睡眠

time.Sleep(time.Second) //每个一秒打印一次

### 5.defer 后进先出

传参是调用时就传入了，只不过最后执行函数



### 6.copier.Copy()

用于将一个数据结构的字段值复制到另一个数据结构。它

以下是函数调用 `copier.Copy(&userM, r)` 的含义： 《

- `copier.Copy()`: 这是 `copier` 包中的 `Copy()` 函数，用于复制数据。
- `&userM`: 这是目标数据结构 `userM` 的指针。`Copy()` 函数将会把源数据复制到这个目标结构体中。
- `r`: 这是源数据结构，可能是另一个结构体或者一个具有**相同字段名**的 map、slice 等。`Copy()` 函数将会从源数据中获取字段值并复制到目标结构体中。

使用 `copier.Copy()` 函数可以方便地将源数据结构的字段值复制到目标结构体中，而无需手动逐个字段进行复制。请确保源数据结构和目标数据结构的字段名和类型匹配，以保证成功复制数据。



### 7.钩子函数

（Hook Function）是在软件开发中的一种编程概念，它允许开发人员在特定事件发生时插入自定义的代码逻辑，以对事件进行处理或拦截。

在许多编程框架和库中，都提供了钩子函数的概念，用于允许开发人员在关键的时刻介入代码的执行流程。通常，钩子函数在某个特定事件发生之前或之后被调用，以便开发人员可以在这些时间点执行额外的逻辑。

钩子函数可以用于各种目的，例如：

1. 数据验证：在保存数据到数据库之前，进行数据验证和清理。
2. 加密/解密：在保存敏感数据到数据库之前，对数据进行加密或解密。
3. 日志记录：在特定事件发生时，记录日志或监控应用程序行为。
4. 缓存管理：在获取数据前，检查缓存中是否存在，并在没有缓存时进行数据库查询并添加到缓存中。

在软件开发中，钩子函数是一种常见的设计模式，它允许代码解耦和模块化，并使得逻辑更易于维护和扩展。通过使用钩子函数，开发人员可以在不修改原有代码的情况下，灵活地扩展和定制功能。

在不同的编程语言和框架中，钩子函数的实现方式可能有所不同。例如，在 Go 语言中，GORM 框架提供了 BeforeCreate、AfterCreate 等钩子函数用于在数据库记录创建前后执行自定义逻辑。而在 JavaScript 中，React 框架提供了生命周期钩子函数（Lifecycle Hooks）用于在组件的不同生命周期中执行自定义逻辑。

总的来说，钩子函数是一种强大的编程概念，它允许开发人员在关键事件发生时干预程序的执行流程，并灵活地定制代码行为。

**举例**

在 GORM 中，`BeforeCreate` 是一个预定义的钩子函数，用于在创建数据库记录之前执行自定义逻辑。它会在执行插入（Create）操作前被自动调用。

要使 `BeforeCreate` 钩子函数生效，需要满足以下两个条件：

1. 模型（Model）需要实现 GORM 的 `BeforeCreate` 方法。在 Go 语言中，方法的接收者类型为模型结构体指针，并且方法名必须为 `BeforeCreate`。
2. 当执行创建数据库记录的操作时，GORM 会自动触发 `BeforeCreate` 钩子函数。

示例代码如下：

```Go
type User struct {
        ID        uint
        Username  string
        Email     string
        Password  string
        CreatedAt time.Time
        UpdatedAt time.Time
}

func (u *User) BeforeCreate(tx *gorm.DB) error {
        // 在创建数据库记录之前，自动执行此函数
        fmt.Println("BeforeCreate: Encrypting password...")
        u.Password = "encrypted_password" // 假设在此处进行密码加密
        return nil
}

func main() {
        dsn := "test.db"
        db, err := gorm.Open(sqlite.Open(dsn), &gorm.Config{})
        if err != nil {
                panic("failed to connect database")
        }

        // 自动创建表
        db.AutoMigrate(&User{})

        // 创建用户
        user := &User{
                Username: "john_doe",
                Email:    "john@example.com",
                Password: "password123", // 这里的密码会在 BeforeCreate 中被加密
        }
        db.Create(user)
}
```

在程序的 `main` 函数中，我们创建了一个新用户，并通过 `db.Create(user)` 插入到数据库中。在执行插入操作之前，GORM 会自动调用 `BeforeCreate` 钩子函数，对密码进行加密的操作，从而实现在创建数据库记录之前执行自定义逻辑的效果。

### 8.正则表达式检查错误消息

使用正则表达式来检查错误消息中是否包含特定模式的方法可以用于处理各种类型的错误。以下是一些常见用法：

1. **处理数据库错误**：通常用于检测某些数据库操作，例如插入重复的用户名，以便返回自定义错误，而不是原始数据库错误。
2. **处理日期格式错误**：您可以使用正则表达式来检查日期格式是否符合要求。例如，如果需要特定的日期格式，您可以编写一个正则表达式来检查日期字符串是否匹配该格式。
3. **检测文件或路径错误**：在文件操作中，您可以使用正则表达式来检查文件名或路径是否符合特定规则。例如，检查文件名是否包含特定的扩展名。
4. **处理网络请求错误**：您可以使用正则表达式来检查网络请求返回的错误消息中是否包含特定的错误代码或关键词。这可用于处理 HTTP 请求或其他网络协议的错误。
5. **处理配置文件解析错误**：如果您的应用程序依赖于配置文件，您可以使用正则表达式来检查配置文件解析错误消息中是否包含特定错误标志或关键字，以便更具体地处理配置错误。
6. **处理自定义错误消息**：如果您在应用程序中使用自定义错误消息，并希望根据错误消息中的特定模式来处理不同类型的错误，可以使用正则表达式进行匹配。

请注意，使用正则表达式来处理错误需要小心，因为它可能会引入一些复杂性，并且需要确保正则表达式模式与错误消息的实际格式相匹配。此外，正则表达式匹配可能会对性能产生一定的影响，因此在性能敏感的情况下，可能需要考虑替代方案。最好的做法是使用正则表达式来处理错误时，尽可能详细地测试和验证匹配模式，以确保它按预期工作。

```Go
if err := b.ds.Users().Create(ctx, &userM); err != nil {
                //一个正则表达式,检查错误是否出现了重复的用户名
                if match, _ := regexp.MatchString("Duplicate entry '.*' for key 'username'", err.Error()); match {
                        return errno.ErrUserAlreadyExist
                }

                return err
        }
```

### 9.参数校验

使用 `govalidator.ValidateStruct` 来进行参数校验。`govalidator` 包能够根据结构体中的 `valid` tag 进行校验，并支持多种校验规则，具体可参考：[asaskevich/govalidator](https://link.juejin.cn/?target=https%3A%2F%2Fgithub.com%2Fasaskevich%2Fgovalidator)。

```Go
// CreateUserRequest 指定了 `POST /v1/users` 接口的请求参数.
type CreateUserRequest struct {
        Username string `json:"username" valid:"alphanum,required,stringlength(1|255)"`
        Password string `json:"password" valid:"required,stringlength(6|18)"`
        Nickname string `json:"nickname" valid:"required,stringlength(1|255)"`
        Email    string `json:"email" valid:"required,email"`
        Phone    string `json:"phone" valid:"required,stringlength(11|11)"`
}

var r v1.CreateUserRequest
if _, err := govalidator.ValidateStruct(r); err != nil {
    core.WriteResponse(c, errno.ErrInvalidParameter.SetMessage(err.Error()), nil)
    return
}
```

### 10.加密纯文本

```Go
// Encrypt 使用 bcrypt 加密纯文本.
func Encrypt(source string) (string, error) {
        hashedBytes, err := bcrypt.GenerateFromPassword([]byte(source), bcrypt.DefaultCost)

        return string(hashedBytes), err
}

// Compare 比较密文和明文是否相同.
func Compare(hashedPassword, password string) error {
        return bcrypt.CompareHashAndPassword([]byte(hashedPassword), []byte(password))
}
```

`bcrypt` 是一种密码哈希函数，通常用于存储用户密码的安全散列值。它的主要目的是保护用户密码，以防止明文密码泄漏时的安全问题。`bcrypt` 是一种慢哈希函数，它使用加盐和迭代来增加密码散列的安全性。

以下是关于 `bcrypt` 的一些关键信息和使用方法：

1. **加盐哈希**：`bcrypt` 会随机生成一个加盐值（salt），并将其与密码组合在一起进行哈希。这意味着相同的密码在每次哈希时都会生成不同的散列值，从而增加了安全性。
2. **迭代次数**：`bcrypt` 允许您指定哈希时的迭代次数，通常以 "工作因子"（work factor）表示。较高的迭代次数会增加哈希操作的计算复杂度，提高了密码的安全性，但也会增加计算时间。通常，迭代次数应该根据硬件性能和应用程序的需求进行权衡选择。
3. **验证密码**：要验证用户输入的密码是否与存储在数据库中的 `bcrypt` 哈希匹配，您需要将用户输入的密码与存储的哈希值进行比较。`bcrypt` 提供了验证函数来执行此操作，它会自动从哈希中提取加盐值。

在 Go 语言中，您可以使用 `golang.org/x/crypto/bcrypt` 包来进行 `bcrypt` 哈希的创建和验证。以下是一个简单的示例：

```Go
package main

import (
    "fmt"
    "golang.org/x/crypto/bcrypt"
)

func main() {
    password := "mysecurepassword"

    // 生成 bcrypt 哈希
    hashedPassword, err := bcrypt.GenerateFromPassword([]byte(password), bcrypt.DefaultCost)
    if err != nil {
        fmt.Println("Error hashing password:", err)
        return
    }

    fmt.Println("Hashed password:", string(hashedPassword))

    // 验证密码
    inputPassword := "mysecurepassword"
    err = bcrypt.CompareHashAndPassword(hashedPassword, []byte(inputPassword))
    if err == nil {
        fmt.Println("Password is correct.")
    } else {
        fmt.Println("Password is incorrect.")
    }
}
```

在上面的示例中，我们首先生成一个 `bcrypt` 哈希，然后验证输入密码是否正确。

bcrypt.DefaultCost（10）可以用于指定哈希的默认迭代次数，

但您可以根据需要进行调整以提高或降低安全性和性能。安全性和性能之间的权衡是一个重要考虑因素，取决于您的应用程序需求。

### 11.生成短 id

```Go
package id

import (
        "strings"

        shortid "github.com/jasonsoft/go-short-id"
)

// GenShortID 生成 6 位字符长度的唯一 ID.
func GenShortID() string {
        opt := shortid.Options{
                Number:        4,
                StartWithYear: true,
                EndWithHost:   false,
        }

        return strings.ToLower(shortid.Generate(opt))
}
//生成的短 ID 会包含 4 位数字、年份信息（根据 StartWithYear 选项是否为 true）、主机信息（根据 EndWithHost 选项是否为 true）等内容，以确保生成的 ID 在给定的选项下是唯一的。这可以用于生成唯一的标识符
```

### 12.strings.Builder

是 Go 语言标准库中的一个类型，用于高效地构建字符串。它提供了一种方式来动态构建字符串而无需频繁地分配内存，尤其是在需要大量字符串拼接的情况下，可以提升性能。

通过 `strings.Builder`，你可以执行以下操作：

- **字符串拼接**：使用 `WriteString` 方法向 `strings.Builder` 中追加字符串。
- **字符拼接**：使用 `WriteRune` 方法向 `strings.Builder` 中追加单个字符。
- **获取最终字符串**：使用 `String` 方法获取最终构建的字符串。

例如：

```Go
var builder strings.Builder
builder.WriteString("Hello, ")
builder.WriteString("world!")
finalString := builder.String()
fmt.Println(finalString) // Output: Hello, world!
```

使用 `strings.Builder` 相比直接使用字符串拼接操作（例如使用 `+` 或 `fmt.Sprintf`）具有更好的性能，因为它避免了在每次追加字符串时重新分配内存的开销。

此外，`strings.Builder` 还提供了其他方法来支持更多字符串操作，如在字符串中查找、截取等。

## 常用库

### strings

包是 Go 语言中用于操作字符串的标准库之一，提供了许多有用的函数来执行各种字符串操作。以下是其中一些常用函数：

- **字符串判断**：
  - `Contains(s, substr string) bool`：检查字符串 `s` 是否包含子字符串 `substr`。
  - `HasPrefix(s, prefix string) bool`：检查字符串 `s` 是否以指定的前缀开头。
  - `HasSuffix(s, suffix string) bool`：检查字符串 `s` 是否以指定的后缀结尾。
  - `ContainsAny(s, chars string) bool`：检查字符串 `s` 中是否包含 `chars` 中的任何字符。
- **字符串查找**：
  - `Index(s, substr string) int`：返回 `substr` 在字符串 `s` 中第一次出现的索引，若不存在则返回 -1。
  - `LastIndex(s, substr string) int`：返回 `substr` 在字符串 `s` 中最后一次出现的索引，若不存在则返回 -1。
- **字符串替换和分割**：
  - `Replace(s, old, new string, n int) string`：将 `s` 中的 `old` 字符串替换为 `new` 字符串，替换次数由 `n` 指定（如果 `n` 小于 0，则替换所有匹配项）。
  - `Split(s, sep string) []string`：根据分隔符 `sep` 对字符串 `s` 进行分割，返回一个字符串切片。
- **字符串处理**：
  - `ToUpper(s string) string`：将字符串 `s` 中的所有字符转换为大写。
  - `ToLower(s string) string`：将字符串 `s` 中的所有字符转换为小写。
  - `Trim(s string, cutset string) string`：去掉字符串 `s` 开头和结尾处包含在 `cutset` 中的字符。
  - `TrimSpace(s string) string`：去掉字符串 `s` 开头和结尾处的空白字符。

这些函数提供了对字符串进行操作和处理的多种方式，可用于实现各种字符串处理需求。

```Go
  s := "apple,orange,banana"
    parts := strings.Split(s, ",")

 s := "***Hello, World!***"
    trimmed := strings.Trim(s, "*") // 去掉开头和结尾的星号
```

### assert 测试->断言

`github.com/stretchr/testify/assert

是一个在 Go 语言中编写测试时常用的第三方测试工具，它提供了丰富的断言函数，用于编写测试用例并验证测试结果是否符合预期。这个库使得编写和维护测试代码更加方便和可读。

一些常见的断言函数包括：

1. `assert.Equal(t, expected, actual)`：验证 `actual` 是否等于 `expected`。
2. `assert.NotEqual(t, notExpected, actual)`：验证 `actual` 是否不等于 `notExpected`。
3. `assert.True(t, condition)`：验证条件 `condition` 是否为真。
4. `assert.False(t, condition)`：验证条件 `condition` 是否为假。
5. `assert.Nil(t, actual)`：验证 `actual` 是否为 `nil`。
6. `assert.NotNil(t, actual)`：验证 `actual` 是否不为 `nil`。
7. `assert.Error(t, err)`：验证是否发生了错误（`err` 不为 `nil`）。
8. `assert.NoError(t, err)`：验证是否没有发生错误（`err` 为 `nil`）。
9. `assert.Contains(t, container, containee)`：验证 `container` 是否包含 `containee`。

这些断言函数可以用于编写测试用例，确保代码的不同部分在各种情况下都表现如预期。如果断言失败，测试框架将记录失败的条件并提供详细的错误信息，以帮助你识别和修复问题。

`github.com/stretchr/testify/assert` 是一个流行的测试工具，广泛用于 Go 语言的单元测试和集成测试。它可以帮助开发人员编写更健壮的测试用例，从而提高代码的可靠性。

### validator

`validator` 是一个用于验证 Go 结构体字段的库，它提供了多种验证标签，可以用于对结构体字段进行各种类型的验证。以下是 `validator` 常用的一些验证标签示例：

- **`required`****：** 标识字段为必填项。
- **`min`****：** 验证数字类型字段的最小值。
- **`max`****：** 验证数字类型字段的最大值。
- **`len`****：** 验证字符串、数组、切片或 map 类型字段的长度。
- **`email`****：** 验证字段是否为有效的电子邮件格式。
- **`url`****：** 验证字段是否为有效的 URL。
- **`alpha`****：** 验证字段是否只包含字母字符。
- **`alphanum`****：** 验证字段是否只包含字母和数字字符。
- **`numeric`****：** 验证字段是否为数字类型。
- **`hexadecimal`****：** 验证字段是否为十六进制格式。
- **`hexcolor`****：** 验证字段是否为有效的十六进制颜色格式。
- **`ipv4`****：** 验证字段是否为有效的 IPv4 地址。
- **`ipv6`****：** 验证字段是否为有效的 IPv6 地址。
- **`mac`****：** 验证字段是否为有效的 MAC 地址。
- **`uuid`****：** 验证字段是否为有效的 UUID。

这些标签可以添加到结构体的字段上，用于声明该字段的验证规则。例如，可以在字段的标签中使用 `validator` 提供的验证标签：

```Go
type User struct {
    Username string `validate:"required,min=3,max=20"`
    Email    string `validate:"required,email"`
    Age      int    `validate:"required,min=18"`
}
```

在这个例子中，`validate` 是 `validator` 包提供的标签，后面跟着不同的验证规则。这些规则会根据字段类型进行验证，确保满足设定的条件。可以根据需要组合使用不同的验证规则，以确保字段数据的有效性。

### base64Captcha

`github.com/mojocn/base64Captcha` 是一个用于生成和验证验证码的 Go 语言库。它可以生成包含数字、字母等的验证码图片，并能将验证码以 base64 编码的形式返回给用户。

这个库提供了创建各种类型验证码的功能，比如数字验证码、字符验证码、滑块验证码等。它的主要特点包括：

1. **多种验证码类型支持**：可以生成数字、字符、滑块等多种类型的验证码。
2. **可定制性强**：可以设置验证码图片的宽高、干扰线数量、噪点密度等参数。
3. **验证码验证**：提供验证用户输入的验证码是否与生成的一致的功能。

基本使用方法通常包括以下步骤：

1. **创建驱动器（Driver）**：选择要生成的验证码类型，并配置相关参数。
2. **创建验证码实例**：使用选定的驱动器创建验证码实例。
3. **生成验证码**：调用生成验证码的方法，并获取生成的验证码信息，如 ID 和图片数据。
4. **将验证码信息返回给用户**：通常以 base64 编码的字符串形式返回给用户，用户可以将其嵌入到页面中以展示验证码。
5. **验证用户输入**：接收用户输入的验证码，与生成的验证码进行比较验证。

```Go
var Store = base64Captcha.DefaultMemStore

type CaptchaInfo struct {
        CaptchaId string
        PicPath   string
}

// GetCaptcha 生成验证码
/*func NewDriverDigit(height int, width int, length int, maxSkew float64, dotCount int) *DriverDigit
height: 验证码图片的高度（以像素为单位）。
width: 验证码图片的宽度（以像素为单位）。
length: 验证码中包含的数字个数。
maxSkew: 数字倾斜程度，控制数字的倾斜程度。一般情况下介于 0 和 1 之间。
dotCount: 图片中干扰点的数量。*/

func GetCaptcha(ctx context.Context) (*CaptchaInfo, error) {
        driver := base64Captcha.NewDriverDigit(80, 250, 5, 0.7, 80)
        cp := base64Captcha.NewCaptcha(driver, Store)
        id, b64s, err := cp.Generate()
        if err != nil {
                return nil, err
        }

        return &CaptchaInfo{
                CaptchaId: id,
                PicPath:   b64s,
        }, nil
}

//登录时验证 验证码
// VerifyCaptcha 验证用户输入的验证码是否正确    匹配则返回 true，否则返回 false。
 store.Verify(id, answer, true)
```

## 依赖管理

1.go mod init ( modulename ) 2.同一个目录下的所有go代码能相互调用，可以合并到一个文件下 3.函数和变量的首字母大小决定作用域

4.代码中引用 包名.函数名/包名.变量名 import引用 module/../ 目录名 //可以取别名 math "g6/util/.math" go get ... / go mod tidy

5.init 特殊函数 一个文件中可以写多个 执行顺序是按照import中导入依赖的对应包顺序写的 在main函数执行前 func init(){ } 可以在依赖前加 下划线+空格 _ 表示自动执行对应依赖下的init函数

## 测试

### 简述

Go 语言有自带的测试框架 `testing`，可以用来实现单元测试和性能测试，通过 `go test` 命令来执行单元测试和性能测试。

`go test` 执行测试用例时，是以 Go 包为单位进行测试的。执行时需要指定包名，比如：`go test 包名`，如果没有指定包名，默认会选择执行命令时所在的包。`go test` 在执行时会遍历以 `_test.go` 结尾的源码文件，执行其中以 `Test`、`Benchmark`、`Example`、`Fuzz` 开头的测试函数。其中源码文件需要满足以下规范：

- 文件名必须是 `_test.go` 结尾，跟源文件在同一个包；
- 测试用例函数必须以 `Test`、`Benchmark`、`Example`、`Fuzz` 开头；
- 执行测试用例时的顺序，会按照源码中的顺序依次执行；
- 单元测试函数 `TestXxx()` 的参数是 `testing.T`，可以使用该类型来记录错误或测试状态；
- 性能测试函数 `BenchmarkXxx()` 的参数是 `testing.B`，函数内以 `b.N` 作为循环次数，其中 `N` 会动态变化；
- 示例函数 `ExampleXxx()` 没有参数，执行完会将输出与注释 `// Output:` 进行对比；
- 测试函数原型：`func TestXxx(t *testing.T)`，`Xxx` 部分为任意字母数字组合，首字母大写，例如： `TestgenShortId` 是错误的函数名，`TestGenShortId` 是正确的函数名；
- 通过调用 `testing.T` 的 `Error`、`Errorf`、`FailNow`、`Fatal`、`FatalIf` 方法来说明测试不通过，通过调用 `Log`、`Logf` 方法来记录测试信息：

```Bash
    t.Log t.Logf     # 正常信息 
    t.Error t.Errorf # 测试失败信息 
    t.Fatal t.Fatalf # 致命错误，测试程序退出的信息
    t.Fail     # 当前测试标记为失败
    t.Failed   # 查看失败标记
    t.FailNow  # 标记失败，并终止当前测试函数的执行，需要注意的是，我们只能在运行测试函数的 Goroutine 中调用 t.FailNow 方法，而不能在我们在测试代码创建出的 Goroutine 中调用它
    t.Skip     # 调用 t.Skip 方法相当于先后对 t.Log 和 t.SkipNow 方法进行调用，而调用 t.Skipf 方法则相当于先后对 t.Logf 和 t.SkipNow 方法进行调用。方法 t.Skipped 的结果值会告知我们当前的测试是否已被忽略
    t.Parallel # 标记为可并行运算
```

### 单元测试

单元测试的目的是验证代码的**正确性和稳定性**。单元测试通常是针对单个函数或方法进行测试的，以确保它们按照预期运行。单元测试通常是在开发过程中执行的，以帮助开发人员发现和修复代码中的错误。在 Go 语言中，可以使用内置的 `testing` 包来编写单元测试，并使用 `go test` 命令来运行测试。

《1》Go: Generate Unit Tests For Function 工具可以自动生成单元测试

```Go
func Test_checkFileIsExist(t *testing.T) {
```

<2> assert 库

```Go
import (
        "testing"
        "gopkg.in/go-playground/assert.v1"
)
func TestGenShortID(t *testing.T) {
        shortID := GenShortID()//生成6位短id
        assert.NotEqual(t, "", shortID)
        assert.Equal(t, 6, len(shortID))
}
```

<3> 开始调试

```Go
go test
go test -v -count 3
go test -run TestGenShortID -v   //`-run` 参数支持正则表达式。
```

#### t.Run

是 Go 标准库中的一个测试函数，它用于在一个测试函数内部执行多个子测试（子测试案例），以便更清晰地组织和报告测试结果。通常，你会使用 `t.Run` 来遍历一个测试案例切片，每个测试案例代表一个具体的测试情况。

`t.Run` 的一般语法如下：

```Go
func (t *T) Run(name string, f func(t *T)) bool
```

- `t` 是 `*testing.T` 类型的测试对象。
- `name` 是子测试的名称，用于标识和描述子测试的目的。
- `f` 是一个函数，通常包含了子测试的代码逻辑。这个函数接受一个 `*testing.T` 对象，用于报告测试结果。

通过使用 `t.Run`，你可以将多个相关的测试用例组织在一起，并将它们显示为单独的测试结果。这对于大型测试套件和复杂的测试情况特别有用，因为它可以提供更好的可读性和报告。

以下是一个示例，演示如何使用 `t.Run` 来组织子测试：

```Go
func TestMyFunction(t *testing.T) {
    testCases := []struct {
        name     string
        input    int
        expected int
    }{
        {"Test Case 1", 2, 4},
        {"Test Case 2", 3, 9},
    }

    for _, tc := range testCases {
        t.Run(tc.name, func(t *testing.T) {
            result := MyFunction(tc.input)
            if result != tc.expected {
                t.Errorf("Expected %d, but got %d", tc.expected, result)
            }
        })
    }
}
```

在上述示例中，`t.Run` 用于执行多个子测试，每个子测试都有一个名称，以及输入和期望输出。这有助于在测试报告中清晰地识别每个测试情况的结果。

### 基准测试

基准测试的目的是测量代码的**性能**。基准测试通常是针对一段代码的性能进行测试的，以找出它的瓶颈和优化点。

1. 使用 `b.ReportAllocs()` 来报告分配的内存情况。
2. 使用 `b.ResetTimer()` 重置计时器，以便不计入基准测试中的初始化时间。
3. 使用 `b.StartTimer()` 开始计时器，以便在测试代码块中测量执行时间。
4. 在测试代码块中执行你要测试的代码。
5. 使用 `b.StopTimer()` 停止计时器，以便不计入测试代码块之外的时间。
6. 使用 `testing.B` 对象的 `b.N` 字段，该字段表示测试运行的迭代次数。
7. 使用 `go test -bench` 命令运行基准测试，并在 `-bench` 标志后指定基准测试函数。
8. 分析基准测试的结果，可以结合 `go tool pprof` 来分析性能数据。

```Go
func Benchmark_checkFileIsExist(b *testing.B) {
    for i=0;i<b.N;i++{ 
    }        }
```

以下是一个示例基准测试文件 `mycode_test.go`，并演示如何使用 pprof 进行性能分析：

```Go
import (
    "testing"
    _ "net/http/pprof"
    "runtime/pprof"
    "os"
)
// 假设这是你要测试的函数
func myFunction() {
    // 在这里执行你的代码
}
func BenchmarkMyFunction(b *testing.B) {
    // 创建 pprof 文件，用于保存性能分析数据
    f, _ := os.Create("cpu.pprof")
    pprof.StartCPUProfile(f)
    defer pprof.StopCPUProfile()

    b.ReportAllocs()
    b.ResetTimer()
    for i := 0; i < b.N; i++ {
        myFunction()
    }
}

func TestMain(m *testing.M) {
    go func() {
        // 启动 pprof HTTP 服务器，以便通过浏览器访问性能数据
        _ = http.ListenAndServe("localhost:6060", nil)
    }()
    os.Exit(m.Run())
}
```

在上述示例中，我们在 `BenchmarkMyFunction` 基准测试函数中启动了 CPU profiling，并将结果保存在名为 `cpu.pprof` 的文件中。还启动了 pprof HTTP 服务器，以便通过浏览器访问性能数据。

你可以在终端中使用 `go test -bench` 命令运行基准测试：

```Bash
go test -bench=. mycode_test.go //-benchtime=30s
go tool pprof cpu.pprof
#或者
#go test -test.bench=".*" -cpuprofile=cpu.profile  在当前目录下生成 cpu.profile 和 id.test 文件
#go tool pprof id.test cpu.profile
```

然后在 pprof 交互式界面中，你可以执行各种命令来查看和分析性能数据，帮助你识别性能瓶颈和优化代码。

### 注意

- 单元测试和基准测试应该分别编写，不要混淆。
- 单元测试应该覆盖代码的各种情况和分支，以确保代码的正确性和稳定性。
- 基准测试应该测量代码的各种情况和分支的性能，以找出瓶颈和优化点。
- 单元测试和基准测试都应该是可重复的，可以在不同的环境和平台上执行，并得到相同的结果。
- 单元测试和基准测试应该是快速的，可以在开发过程中频繁运行，并不会影响开发进度。

### 模糊测试

## RPC

RPC，即 Remote Procedure Call（远程过程调用），是一个计算机通信协议。 该协议允许运行于一台计算机的程序调用另一台计算机的子程序，而程序员无需额外地为这个交互作用编程。

## 性能调优

### pprof

#### 分析

##### <1>浏览器图形界面

一共有三个界面

1. 使用net/http/pprof 动态收集        的 http://localhost:6060/debug/pprof 界面 大方向 cpu/内存/协程/..
2. 应用运行起来就可以访问

![202407092319992](https://raw.githubusercontent.com/lanyoumeng/Drawing-bed/main/docs/202407111940516.png)

1. go tool pprof -http=:8080 "http://localhost:6060/debug/pprof/goroutine"

2. go tool pprof -http=:6062 id.test cpu.profile 的分析界面 top/火焰图/

   ![202407092320933](https://raw.githubusercontent.com/lanyoumeng/Drawing-bed/main/docs/202407111941232.png)

1. go tool trace -http=0.0.0.0:6061 xxx.trace 的trace界面

![202407092321269](https://raw.githubusercontent.com/lanyoumeng/Drawing-bed/main/docs/202407111941220.png)

![202407092321945](https://raw.githubusercontent.com/lanyoumeng/Drawing-bed/main/docs/202407111941460.png)

##### <2>shell 界面分析 .pprof 文件

通过性能分析点生成

```Go
f, err := os.Create("cpu.pprof")
pprof.StartCPUProfile(f)
```

通过基准测试生成

```Bash
go test -test.bench= ".*"*
go test -test.bench=".*" -cpuprofile=cpu.profile
```

- CPU profiling：使用 `go tool pprof` 命令来分析 CPU profiling 数据文件。例如：

```Plaintext
go tool pprof cpu.pprof
```

- 内存 profiling：使用 `go tool pprof` 命令来分析内存 profiling 数据文件。例如：

```Plaintext
go tool pprof mem.pprof
```

#### 1.net/http/pprof 包

`pprof` 是 Go 语言标准库的一部分，用于性能分析和性能优化。它允许你在代码中嵌入性能分析点，然后生成分析数据以识别性能问题。

1. 导入 `net/http/pprof` 包：首先，你需要导入 `net/http/pprof` 包，以便使用标准的 `pprof` HTTP 端点来获取性能数据。

```Go
import _ "net/http/pprof"
```

1. 启动 `pprof` HTTP 端点：
2. 需要显式注册 `pprof` 端点

```Go
go func() {
    log.Println(http.ListenAndServe("localhost:6060", nil))
}()
```

1. 或者        pprof.Register(g)
2. 是一个用于注册 `pprof` 端点的函数，其中 `g` 是一个 `*http.ServeMux` 或 `*http.ServeMux` 兼容的对象。

```Go
   // 创建一个 HTTP 服务器
    mux := http.NewServeMux()  //g := gin.New()
    // 注册 pprof 端点
    pprof.Register(mux)   //pprof.Register(g)
```

1. 添加性能分析点：在你的代码中，你可以使用 `pprof` 包的不同函数来添加性能分析点。以下是一些常见的用法：
   1. `pprof.StartCPUProfile(w io.Writer) error`：开始 CPU 分析并将数据写入 `io.Writer`。
   2. `pprof.StopCPUProfile()`：停止 CPU 分析。
   3. `pprof.WriteHeapProfile(w io.Writer) error`：写入堆内存分析数据。
2. 你可以在你的代码中根据需要插入这些函数，以便在特定的代码路径中进行性能分析。

```Go
import (
    "os"
    "runtime/pprof"
)

// ...

// 启动 CPU profiling
f, err := os.Create("cpu.pprof")
if err != nil {
    log.Fatal("无法创建 CPU profile 文件: ", err)
}
pprof.StartCPUProfile(f)
defer pprof.StopCPUProfile()

// 在这里执行你的代码，收集 CPU profile 数据
// 在执行完你的代码后，CPU profiling 数据将保存在 "cpu.pprof" 文件中
```

**显式添加性能分析点**：

- 你可以精确控制在代码中哪些地方启用性能分析。
- 可以只收集特定部分代码的性能数据。
- 适用于精细粒度的性能分析和优化。

**使用** **`net/http/pprof`** **包和 HTTP 端点**：

- 全局性能分析，整个应用程序中的所有性能数据都可以访问。
- 通过 Web 浏览器或命令行工具访问性能数据。
- 适用于全局性能监控、故障排除和大规模应用程序的性能分析。

4.访问 `pprof` 数据：一旦你的程序运行，并且 `pprof` HTTP 端点正在监听

使用浏览器访问以下 URL 来查看性能数据： (linux也一样)

http://localhost:6060/debug/pprof/：显示可用的性能分析选项列表。

#### 2. go tool pprof

**`net/http/pprof`** **包 和 .pprof 文件都可以** // 就是`net/http/pprof` 包 分析时是在shell窗口，而不是浏览器 （可以通过web命令跳转火焰图）

分析 CPU Profile 数据：

首先，确保你的程序已经添加了 `net/http/pprof` 包和相应的 HTTP 端点

- 生成 CPU Profile 数据：
- 运行你的 Go 程序并让它收集 CPU profile 数据。你可以通过访问 `http://localhost:6060/debug/pprof/profile` 来触发采集。
- 使用 `go tool pprof` 进行分析：
- 这将打开交互式分析界面，让你可以查看各种性能数据、生成图形和分析结果。

```Bash
go tool pprof http://localhost:6060/debug/pprof/profile    #cpu排查
#go tool pprof "http://localhost:6060/debug/pprof/profile?seconds=10"   cpu排查 时间是10s
# go tool pprof http://localhost:6060/debug/pprof/heap #内存


#不仅获取性能数据，还启动一个 HTTP 服务器，
 go tool pprof -http=:6061 "http://localhost:6060/debug/pprof/heap"   内存排查  
 go tool pprof -http=:8080 "http://localhost:6060/debug/pprof/goroutine"  协程
 go tool pprof -http=:8080 "http://localhost:6060/debug/pprof/mutex"   锁
 go tool pprof -http=:8080 "http://localhost:6060/debug/pprof/block"  阻塞

浏览器： http://127.0.0.1:6060/debug/pprof/
```

- 这将进入交互式分析界面，你可以在其中执行各种命令来查看分析结果
- `top` 命令来查看 CPU 使用率最高的函数：

```Plaintext
(pprof)(top) [ -cum ] [ -n N ]
```

-  top：用于执行 top 命令。 -cum：可选参数，用于显示累积（cumulative）CPU 时间占用百分比。累积 CPU 时间考虑了函数及其所有子函数的 CPU 时 间。 -n N：可选参数，用于指定显示多少个函数。通常是前 10 个函数
- `web` 命令生成火焰图（Flame Graph）：

```Bash
(pprof) web
          火焰图是一种直观的可视化工具，用于识别 CPU 使用率最高的代码路径。
```

- `list` 命令用于查看指定函数的源代码，并显示与性能数据关联的行号和代码：
-  这可以帮助你分析哪些具体代码行导致了性能问题。

```Plaintext
(pprof) (list|l) [ -s ] [ -t ] [ -n N ] [ function ]
```

- `list` 或 `l`：用于执行 `list` 命令。
- `-s`：可选参数，用于显示函数的源代码。
- `-t`：可选参数，用于显示函数的汇编代码。
- `-n N`：可选参数，用于指定显示多少行源代码。例如，`-n 10` 表示显示前 10 行源代码。
- `function`：要查看源代码的函数名称。如果不提供函数名称，则将显示当前选定的函数

#### 服务器查看

如果你希望从本地 Windows 的 Web 页面上查看远程 Linux 服务器上的 `pprof` 数据，可以使用 SSH 隧道将远程 `pprof` 端点映射到本地，并在本地 Web 浏览器上访问。以下是具体步骤：

1. **确保 Linux 服务器上已启用** **`pprof`** **端点**：
   1. 在你的 Go 代码中，确保你已导入了 `net/http/pprof` 包并在代码中注册了 `pprof` 端点。
   2. 在 Linux 服务器上运行你的 Go 程序，确保 HTTP 服务器在 `pprof` 端点上监听。例如，你可以使用以下代码：
   3. ```Go
      go func() {
          log.Println(http.ListenAndServe("0.0.0.0:6060", nil))
      }()
      ```
2. 这将允许从外部访问 Linux 服务器上的 `pprof` 数据。
3. **使用 SSH 隧道连接到 Linux 服务器**：
   1. 在 Windows 上，使用 SSH 客户端（例如 PuTTY、OpenSSH、或 Windows Subsystem for Linux - WSL）建立 SSH 连接到 Linux 服务器。
   2. 在 SSH 连接命令中，使用 `-L` 选项创建本地端口映射，将远程服务器上的 `pprof` 端点映射到本地端口。例如：
   3. ```Bash
      ssh -L 6060:localhost:6060 username@linux-server
      ```

   4. 在上述命令中，将 `username` 替换为你的 Linux 服务器用户名，`linux-server` 替换为服务器的 IP 地址或域名。这将在本地端口 `6060` 上创建一个隧道，将请求转发到远程服务器的端口 `6060`。
4. **在本地 Windows 上使用 Web 浏览器访问** **`pprof`** **端点**：
   1. 打开本地的 Web 浏览器（例如 Chrome、Firefox、Edge 等）。
   2. 在浏览器地址栏中输入以下 URL 来访问远程 Linux 服务器上的 `pprof` 端点：
   3. ```Plaintext
      http://localhost:6060/debug/pprof/
      ```

   4. 这将在本地浏览器上打开 `pprof` 页面，你可以点击不同的链接来查看性能数据，如 CPU profiling、内存 profiling、goroutine 数据等。

通过这些步骤，你可以使用本地 Web 浏览器访问远程 Linux 服务器上的 `pprof` 数据，并查看性能数据。确保设置了正确的 SSH 隧道映射，以便将远程 `pprof` 端点映射到本地端口。

## 分布式定时任务

分布式定时任务是指在多个节点上协同运行定时任务，以实现高可用性和负载均衡。使用 Go 语言可以实现分布式定时任务，通常会借助一些库和框架。以下是一个基本的示例，展示如何使用 `cron` 包和 `etcd` 进行分布式定时任务调度：

1. **安装依赖：**

   需要安装 `cron` 和 `etcd` 的 Go 包。

   ```sh
   go get github.com/robfig/cron/v3
   go get go.etcd.io/etcd/clientv3
   ```

2. **编写分布式定时任务代码：**

   ```go
   package main

   import (
       "context"
       "fmt"
       "log"
       "time"

       "go.etcd.io/etcd/clientv3"
       "go.etcd.io/etcd/clientv3/concurrency"

       "github.com/robfig/cron/v3"
   )

   func main() {
       // 创建etcd客户端
       cli, err := clientv3.New(clientv3.Config{
           Endpoints:   []string{"localhost:2379"},
           DialTimeout: 5 * time.Second,
       })
       if err != nil {
           log.Fatal(err)
       }
       defer cli.Close()

       // 创建cron实例
       c := cron.New()

       // 添加一个定时任务
       _, err = c.AddFunc("@every 1m", func() {
           // 创建分布式锁
           session, err := concurrency.NewSession(cli)
           if err != nil {
               log.Fatal(err)
           }
           defer session.Close()

           mutex := concurrency.NewMutex(session, "/my-lock/")
           // 尝试获取锁
           if err := mutex.Lock(context.Background()); err != nil {
               log.Fatal(err)
           }

           // 执行任务
           fmt.Println("Executing distributed task at", time.Now())

           // 释放锁
           if err := mutex.Unlock(context.Background()); err != nil {
               log.Fatal(err)
           }
       })
       if err != nil {
           log.Fatal(err)
       }

       // 启动cron调度
       c.Start()

       // 保持程序运行
       select {}
   }
   ```

3. **运行代码：**

   在多个节点上运行此代码，确保每个节点都连接到同一个 `etcd` 集群。通过分布式锁机制，只有一个节点能够在每个时间点执行任务。

**注意事项：**

- **etcd 集群配置：** 确保 `etcd` 集群已正确配置并运行。
- **任务重试机制：** 根据需要添加任务失败时的重试机制。
- **负载均衡：** 根据任务的复杂性和节点的能力进行负载均衡。
- **故障恢复：** 考虑节点故障时的任务恢复和重启机制。

通过这种方式，可以实现一个简单的分布式定时任务系统。如果任务复杂度较高或有更多需求，可以考虑使用专业的分布式任务调度系统，如 Apache Airflow、Kubernetes CronJobs 等。



## 框架

### web框架

Hertz / Gin

### ORM框架

gorm / xorm

### RPC框架

kitex / gRPC /

## 包

### os

Go语言标准库中的`os`包提供了许多常用的函数和类型，用于与操作系统交互，处理文件和目录，执行命令等。以下是`os`包中一些常用函数的简要介绍：

1. 文件和目录操作：
   1. `os.Create(filename string) (*File, error)`: 创建一个新文件，返回文件对象和可能出现的错误。
   2. `os.Open(filename string) (*File, error)`: 打开一个文件用于读取，返回文件对象和可能出现的错误。
   3. `os.OpenFile(name string, flag int, perm FileMode) (*File, error)`: 根据指定的标志和权限打开文件，返回文件对象和可能出现的错误。
   4. `os.Mkdir(name string, perm FileMode) error`: 创建一个新目录，使用指定的权限。
   5. `os.MkdirAll(path string, perm FileMode) error`: 创建所有不存在的目录，使用指定的权限。
   6. `os.Remove(name string) error`: 删除文件或目录。
   7. `os.RemoveAll(path string) error`: 删除目录及其所有子目录和文件。
2. 文件信息：
   1. `os.Stat(name string) (FileInfo, error)`: 返回一个文件的FileInfo对象，包含文件的基本信息。
   2. `os.IsExist(err error) bool`: 判断错误是否表示文件或目录已存在。
   3. `os.IsNotExist(err error) bool`: 判断错误是否表示文件或目录不存在。
3. 执行命令：
   1. `os/exec`包中的函数和类型用于执行外部命令和进程的相关操作，如`Command`、`Run`、`Start`等。
4. 环境变量和命令行参数：
   1. `os.Args`: 一个字符串切片，包含命令行参数。
   2. `os.Getenv(key string) string`: 获取指定环境变量的值。
   3. `os.Setenv(key, value string) error`: 设置指定环境变量的值。
5. 标准输入输出：
   1. `os.Stdin`: 表示标准输入的文件对象。
   2. `os.Stdout`: 表示标准输出的文件对象。
   3. `os.Stderr`: 表示标准错误输出的文件对象。
6. 获取当前工作目录：
   1. `os.Getwd() (dir string, err error)`: 返回当前工作目录的绝对路径。
7. 进程操作：
   1. `os.Getpid() int`: 获取当前进程的PID。
   2. `os.Getppid() int`: 获取当前进程的父进程的PID。

### runtime

是一个非常重要的标准库包，它提供了与 Go 程序的运行时环境相关的功能和接口。`runtime` 包在 Go 代码中通常以 `import "runtime"` 的方式引入。

`runtime` 包的功能包括：

1. **协程和调度器管理**：`runtime` 包提供了创建和管理 Go 协程（goroutine）的函数，如 `go` 关键字。它还包含调度器（scheduler）用于在多个操作系统线程上运行 goroutine，自动进行任务调度和资源管理。
2. **垃圾回收**：Go 使用垃圾回收（garbage collection）来自动管理内存。`runtime` 包提供了与垃圾回收相关的接口，例如手动触发垃圾回收的函数。
3. **内存分配**：`runtime` 包提供了一些用于内存分配和释放的函数，例如 `malloc` 和 `free`。
4. **并发同步**：`runtime` 包中提供了原子操作、互斥锁、条件变量等并发同步的原语，用于实现线程安全的并发操作。
5. **栈和堆管理**：Go 程序在运行时会使用栈和堆来存储变量和数据。`runtime` 包提供了一些函数来控制栈和堆的大小和管理。
6. **系统信息查询**：`runtime` 包提供了获取系统信息的函数，如 CPU 核数、操作系统、Go 版本等。
7. **Panic 和 Recover**：`runtime` 包提供了 panic 和 recover 机制，用于处理异常和恢复。

总之，`runtime` 包为 Go 程序提供了很多运行时环境的功能和支持，使得 Go 语言能够高效地管理协程、内存和并发操作。请注意，大部分 `runtime` 包的函数和接口都是底层的，通常只有高级使用场景才需要直接使用它们，一般情况下，我们可以直接使用标准库中提供的更高级的包和函数。

### internal目录

在 Go 1.4 版本引入的 `internal` 目录是一种特殊的目录结构，用于创建 Go 包内的私有包，这些包只能被同一模块内的其他包引用，而无法被外部模块访问。这有助于实现模块内的信息封装和隐藏，提高包的封装性，同时确保包的私有性。

下面是有关 `internal` 目录的一些关键信息：

1. **目的：** `internal` 目录的主要目的是将包内的一些功能模块或包隐藏起来，以确保它们只能被同一模块内的其他包访问。这有助于减少外部模块对内部包的直接依赖，同时保护包的私有性。
2. **作用范围：** `internal` 目录的作用范围限于同一 Go 模块。只有同一模块中的包才能引用 `internal` 包。外部模块无法引用 `internal` 包。
3. **目录结构：** `internal` 目录通常位于 Go 包的根目录下。在 `internal` 目录中，您可以创建子目录和包来组织模块内的私有代码。例如：

```Plaintext
mymodule/
├── mypublicpackage/
│   └── public.go
├── internal/
│   ├── myinternalpackage/
│   │   └── internal.go
│   └── anotherinternalpackage/
│       └── internal.go
├── go.mod
└── main.go
```

1. **包命名：** `internal` 包的命名规则与普通包一样，但它们通常以 `internal` 包名前缀开头，以明确表示它们的作用范围。例如，`internal` 包可以命名为 `internal/myinternalpackage`。
2. **导入路径：** `internal` 包的导入路径是相对于模块根目录的路径，因此可以通过相对路径来引用它们。例如，`mypublicpackage` 中可以引用 `internal/myinternalpackage` 如下：

```Go
import "mymodule/internal/myinternalpackage"
```

1. **注意事项：** `internal` 包应该谨慎使用，仅用于那些确实需要在同一模块内共享但不希望对外公开的代码。不建议滥用 `internal` 包，因为它会导致较弱的模块隔离，增加了代码的耦合性。

总之，`internal` 目录是 Go 模块内用于管理私有包的一种机制，它有助于提高代码的封装性和隔离性，同时确保只有同一模块内的包能够访问这些私有包。

### 同一模块

在 Go 中，一个模块（Module）是一个包含 Go 代码的单元，通常对应于一个版本控制的代码仓库（如 Git 存储库）或一个代码目录。模块是 Go 1.11 版本引入的概念，用于管理和版本化 Go 项目中的依赖关系。

下面是关于 Go 模块的一些关键概念：

1. **模块路径（Module Path）：** 模块路径是一个唯一的标识符，通常与代码仓库的 URL 或文件系统路径关联。例如，`github.com/user/repo` 可以作为模块路径。模块路径用于引用和下载模块的代码。
2. **模块版本（Module Version）：** 模块版本是模块的特定快照或标签，通常对应于代码仓库的提交或发布。模块版本是通过语义版本控制规则（Semver）定义的，如 `v1.2.3`。模块版本用于确保代码的稳定性和一致性。
3. **模块文件（****`go.mod`****）：** `go.mod` 文件是定义 Go 模块的清单文件。它包含了模块的路径、依赖关系、版本信息等。模块文件是 Go 项目的核心，用于管理模块的依赖关系。
4. **依赖关系：** 模块可以依赖于其他模块。`go.mod` 文件中列出了模块的依赖关系，包括依赖模块的路径和版本。Go 工具会自动下载和管理这些依赖关系。
5. **同一模块（Same Module）：** 同一模块指的是在同一个 `go.mod` 文件中声明的所有包。这些包共享相同的模块路径，版本和依赖关系。同一模块内的包可以相互引用，并享有访问权限。
6. **不同模块（Different Modules）：** 不同模块指的是在不同的 `go.mod` 文件中声明的包。每个 `go.mod` 文件代表一个不同的模块，拥有独立的依赖关系和版本信息。不同模块的包之间默认不能直接引用，需要通过导入路径来引用其他模块的包。

Go 模块的引入有助于管理依赖关系，并使代码更容易维护和版本化。不同模块之间的依赖关系通过模块路径和版本来定义，确保了不同模块的包之间不会发生命名冲突。同一模块内的包可以直接引用彼此，而不同模块的包需要使用导入路径来引用。这种模块化的方式有助于 Go 项目的组织和协作。

## 杂

### 1.交换两个变量的值

**a, b = b, a**

### 2.数字转换

字符串-》数字 strconv.ParseInt("123",10,64) //字符串，进制(传0自动推测)，位数 strconv.ParseFloat("1.25",64) strconv.Atoi("123")

```Go
num, _ := strconv.ParseInt( , , )
```

数字-》字符串 strconv.Itoa(120) //注意string()会直接把字节或者数字转换为字符的UTF-8表现形式。string(120) 会输出 x

### 3.空白标识符

_ 也被用于抛弃值，如值 5 在：_, b = 5, 7 中被抛弃。 //并不需要使用从一个函数得到的所有返回值

### 4.在nil和空（empty）

表示不同的概念，具体取决于数据类型的不同。

- **nil**：在 Go 中，`nil` 通常用于表示**引用类型（比如指针、切片、映射、通道和函数）的零值或未初始化值**。对于引用类型，`nil` 表示指针或引用不指向任何有效的内存地址或对象。
- 例如：

```Go
var ptr *int     // 声明一个整型指针，默认为 nil
var slice []int  // 声明一个整型切片，默认为 nil
var m map[string]int  // 声明一个字符串到整型的映射，默认为 nil
```

- **空（empty）**：对于一些值类型，例如字符串、切片、映射和通道，有时候我们可以说它们是空的。这表示它们已经被初始化，但是没有任何元素。它们的长度可能是0，但是它们并非指向 `nil`，而是已经分配了对应的空间。
- 例如：

```Go
var emptySlice = []int{}       // 声明一个空的整型切片，长度为0但不是nil
var emptyMap = map[string]int{}  // 声明一个空的字符串到整型的映射，长度为0但不是nil
var emptyString = ""           // 声明一个空字符串，长度为0但不是nil
```

总的来说，`nil` 是针对引用类型的零值，表示未初始化或者没有指向有效对象的指针。而空则是对于某些值类型的一种特殊状态，表示已初始化但没有内容。

### 5.判断字符串为空

```Go
if len(str) ==0 {  //当字符串是 nil 时，len() 将返回 0，而 str == "" 将返回 false。
}else{
}
```

### 6.目录

`./`： 表示当前目录

`../`： 表示父级目录

`~/`： 表示用户的主目录

7.文档

go语言有一个工具叫godoc，是go语言的[![img](https://i0.hdslb.com/bfs/reply/9f3ad0659e84c96a711b88dd33f4bc2e945045e0.png)文档生成器](https://search.bilibili.com/all?from_source=webcommentline_search&keyword=文档生成器&seid=16037719187694541639)，注释前写上函数名可以让godoc认为这是这个函数的文档，调用文档生成器时可以自动生成这个函数的文档，而不需要你手动写文档。



# Vscode

## 1.命令行

ctrl+shift+p

Go:show All command //

<1> Go: Install/Update Tools 		下载更新所有插件
<2>Go: Generate Unit Tests For Function 鼠标右键 自动生成单元测试		 //--->打开日志级别 go插件设置 Go: Build Tags 填上 -v <3>Go: Fill struct 自动生成结构实例化 单元测试用的 
<4> Go: Generate Interface Stubs 自动实现接口		 //先创建go.mod 需要填写三个参数 
<5> Go:Add Tags To Struct Fields struct自动增加Tag 		// 通过设置Setting.json ---> "go.addTags" 
"transform": "camelcase", 	//下划线式的命名方式转换为驼峰式
"transform": "snakecase"	//将驼峰式的命名方式转换为下划线式
Go: Remove Tags From Struct Fields 移除Tag 
<6> 重命名符号 鼠标右键 引用也会从命名 
<7>Go: Extract to variable 抽象变量 		//鼠标选中独立的一部分 抽象函数也有，不过不太好用 
<8>Go: Add Package to Workspace 将第三方包添加到工作空间中(左边目录)
<9>保存时 插件go设置 save		 // 执行单元测试，是否符合标准等

<10> 鼠标右键菜单 ： "go.editorContextMenuCommands"

## 2.快捷键

Ctrl+C 组合键来中断正在运行的程序

## 3.更新

1. 在用户级别的目录中保存 `code.repo` 文件并更新软件包：
   1. 在用户的主目录（例如 `/home/username/`）中创建一个目录，例如 `vscode-repo`：
   2. ```Bash
      mkdir ~/vscode-repo
      ```

   3. 在 `vscode-repo` 目录中创建一个 `code.repo` 文件并进行相应的配置：
   4. ```Bash
      vi ~/vscode-repo/code.repo
      ```

   5. 在文件中添加以下内容（假设使用 Visual Studio Code 官方仓库）：
   6. ```Plaintext
      [code]
      name=Visual Studio Code
      baseurl=https://packages.microsoft.com/yumrepos/vscode
      enabled=1
      gpgcheck=1
      gpgkey=https://packages.microsoft.com/keys/microsoft.asc
      ```

   7. 使用管理员权限将此文件复制到 `/etc/yum.repos.d/` 目录：
   8. ```Bash
      sudo cp ~/vscode-repo/code.repo /etc/yum.repos.d/
      ```

   9. 使用管理员权限更新软件包：
   10. ```Bash
       sudo dnf update code
       ```

# Gin

## 1..从HTTP请求中获取参数数据的方法

1 ctx.Query方法用于从URL中获取查询参数（Query Parameter）。查询参数通常是通过URL的`?`后面附加的键值对，例如：`http://example.com?name=John&age=30`。

2 ctx.PostForm`方法用于从POST请求中获取表单数据。通常，它用于从表单提交的数据中获取值。

post和form表单一起用

3 ctx.BindJSON`方法用于将JSON格式的请求主体解析为Go结构体。它通常用于处理客户端以JSON格式发送的数据。

4 ctx.Param方法用于获取路由参数（Route Parameter）。路由参数是定义在URL路径中的占位符，例如：`/users/:id`，其中`:id`就是一个路由参数。

5 ctx.DefaultQuery`方法用于获取查询参数的值，如果查询参数不存在，则可以指定一个默认值。 c.DefaultQuery("name", "Guest")

6 ctx.ShouldBind`和`ctx.ShouldBindJSON是用于将HTTP请求数据绑定到Go结构体的方法。它们根据请求的Content-Type自动选择适当的绑定方法，例如，如果请求是JSON格式，它将使用`ctx.ShouldBindJSON`进行绑定。

## **2.向客户端发送HTTP响应的方法。**

**c.JSON和c.String**

（1）c.JSON: c.JSON方法用于返回JSON格式的响应。它将Go数据结构转换为JSON字符串，并设置响应的Content-Type为"application/json"。这是在构建API时常用的方法，允许您将结构化数据以JSON格式返回给客户端。

使用c.JSON的基本用法如下：

```Go
func main() {
        r := gin.Default()
        r.GET("/data", func(c *gin.Context) {
                data := map[string]interface{}{
                        "key1": "value1",
                        "key2": 123,
                }
                c.JSON(http.StatusOK, data)
        })

        r.Run(":8080")
}
```

当访问`/data`路径时，`c.JSON`方法会将`data`映射转换为JSON格式，并作为响应发送给客户端。

（2）c.String: `c.String`方法用于返回纯文本的响应。它将一个字符串作为响应内容，并设置响应的Content-Type为"text/plain"。这是在简单的文本API或需要返回纯文本响应的情况下使用的方法。

使用`c.String`的基本用法如下：

```Go
func main() {
        r := gin.Default()
        r.GET("/hello", func(c *gin.Context) {
                c.String(http.StatusOK, "Hello, World!")
        })

        r.Run(":8080")
}
```

当访问`/hello`路径时，`c.String`方法会将"Hello, World!"作为纯文本响应发送给客户端。

总结：

- `c.JSON`用于返回JSON格式的响应，适用于API响应。
- `c.String`用于返回纯文本格式的响应，适用于简单的文本API响应。

## 3.gin.H{}

在`gin`框架中，`gin.H{}`是一个用于构建`map[string]interface{}`的简便方法。

```Go
func main() {
        r := gin.Default()
        r.GET("/data", func(c *gin.Context) {
                data := gin.H{
                        "key1": "value1",
                        "key2": 123,
                }
                c.JSON(http.StatusOK, data)
        })
        r.Run(":8080")
}
```

## 4.context

- 在Gin框架中，每个HTTP请求都有自己独立的`gin.Context`对象。
- `gin.Context`对象在每次请求到达时创建，并在请求处理结束后销毁。
- 每个请求的处理过程中使用的`gin.Context`对象是相互隔离的，不会共享数据和状态。
- 传参时 *gin.Context=Context.Context

## 5.set和get

于在 gin.Context 对象中设置和获取键值对数据的方法

`set`方法：`set`方法属于Gin框架自定义的扩展方法，只在当前请求的处理过程中有效，如果需要跨请求传递数据或全局共享数据，可能需要考虑其他方法，如全局变量或数据库等。

```Go
func main() {
        r := gin.Default()
        r.GET("/user/:id", func(c *gin.Context) {
                // 从URL中获取id参数并设置到Context中
                id := c.Param("id")
                c.Set("userID", id)

                if userID, exists := c.Get("userID"); exists {
                        c.String(http.StatusOK, "User Profile for ID: %s", userID)
                } else {
                        c.String(http.StatusNotFound, "User ID not found in Context.")
                }
        })

        r.Run(":8081")
}
// /opt/miniblog/bin/miniblog --config=/etc/miniblog/miniblog.yaml
```

## 6.http

`http`是Go语言标准库中的一个包，提供了HTTP客户端和服务器的功能，用于实现HTTP通信。

其中，`http`包中主要包含以下几个重要的结构体和函数：

1. `http.Client`： `http.Client`是用于发送HTTP请求的结构体。通过创建`http.Client`对象，可以配置一些HTTP请求的参数，例如超时时间、代理等。
2. `http.Request`： `http.Request`是一个表示HTTP请求的结构体。您可以使用`http.NewRequest`函数创建一个新的`http.Request`对象，然后设置请求的方法、URL、请求头、请求体等信息。
3. `http.Response`： `http.Response`是一个表示HTTP响应的结构体。在客户端发送HTTP请求后，服务器会返回一个`http.Response`对象，其中包含响应的状态码、响应头、响应体等信息。
4. `http.Get`、`http.Post`等函数： `http`包还提供了一些便捷的函数，如`http.Get`和`http.Post`，用于发送GET和POST请求，使得发送HTTP请求变得简单快捷。
5. `http.Serve`、`http.Handle`、`http.HandlerFunc`等函数： 这些函数用于创建HTTP服务器，监听指定的地址并处理客户端的请求。`http.Serve`用于启动HTTP服务器，`http.Handle`和`http.HandlerFunc`用于注册处理程序函数，用于处理不同的HTTP请求路径和方法。

这些是`http`包的一些主要功能和组件，它们提供了构建HTTP客户端和服务器所需的基本功能。通过`http`包，您可以轻松地在Go语言中实现HTTP通信，创建自己的HTTP客户端和服务器。

## 7.设置响应头

在 Gin 框架中，您可以使用 `c.Header(key, value)` 方法来设置 HTTP 响应头部。这个方法允许您自定义响应的头部信息。以下是对它的解释：

- `Header(key, value)` 方法用于设置响应头部的特定键的值。它接受两个参数：
  - `key` 是要设置的响应头部的名称，通常是一个字符串，如 "Content-Type"、"Cache-Control" 等。
  - `value` 是要设置的响应头部的值，通常也是一个字符串，表示该头部的内容。

示例：

```Go
func MyHandler(c *gin.Context) {
    // 设置响应头部的 Content-Type 为 JSON
    c.Header("Content-Type", "application/json")
    // 设置自定义响应头部
    c.Header("X-Custom-Header", "CustomValue")

    // 发送响应
    c.String(http.StatusOK, "Response Body")
}
```

在这个示例中，`Header` 方法被用于设置响应的 `Content-Type` 为 JSON，并添加一个自定义的 `X-Custom-Header` 到响应头中。这些设置可以影响客户端接收到的响应的头部信息。您可以根据需要设置各种响应头部，以满足您的应用程序的需求。

相比 `c.Writer.Header().Set(key, value)` 方法不，Gin 框架推荐使用 `c.Header(key, value)` 方法来设置响应头部，因为它更符合 Gin 框架的惯例，并提供了更方便的方式来操作响应头部。

## 8.HTML 渲染

### 1.LoadHTMLGlob和 LoadHTMLFiles

（1）LoadHTMLGlob(pattern string)`:`LoadHTMLGlob`方法用于加载指定目录下的所有匹配模式的 HTML 模板文件。`pattern`参数是一个匹配模式，可以是一个具体的文件名或包含通配符（`*`）的模式。例如，`**pattern**`可以是`**"templates/*.html"`，表示加载位于 "templates" 目录下所有以`.html` 为后缀的模板文件。

```Go
r.LoadHTMLGlob("templates/*.html")
//实际路径
router.LoadHTMLGlob("../html321/*")
```

（2）`LoadHTMLFiles(files ...string)`: `LoadHTMLFiles` 方法用于加载指定的多个 HTML 模板文件。您可以通过参数将多个模板文件的路径传递给该方法。这对于加载指定的模板文件或只加载少量模板文件很有用。

```Go
r.LoadHTMLFiles("templates/index.html", "templates/about.html")
```

在路由处理函数中，可以通过 `c.HTML` 方法来渲染加载的模板，并将生成的 HTML 作为响应发送给客户端。

### 2.router.Static

`router.Static`是Gin框架中的一个函数，用于在路由中注册静态文件服务。它允许你指定一个URL路径前缀，并将该前缀映射到服务器上的一个实际文件系统目录，从而可以通过该URL路径访问静态文件（如图片、CSS、JavaScript等）。

例如，假设你有一个存放静态文件的目录`static`，里面包含了一些图片和样式文件。你可以使用`router.Static`函数将这个目录映射到一个URL路径上，使得客户端可以通过URL来访问这些静态文件。

以下是一个简单的例子：

```Go
func main() {
    r := gin.Default()

    // 将 "/static" 路径映射到 "./static" 目录
    r.Static("/static", "./static")

    r.Run(":8080")
}
```

在这个例子中，所有以 `/static` 开头的请求都会映射到 `./static` 目录下的对应文件。例如，如果你的服务器上有一个文件 `./static/image.jpg`，那么客户端可以通过访问 `http://yourdomain.com/static/image.jpg` 来获取这张图片。

需要注意的是，为了让`router.Static`函数能够正确地提供静态文件，你的应用程序必须有权限读取指定的静态文件目录。此外，在生产环境中，通常不会使用Gin来提供静态文件服务，而是会使用专门的Web服务器（如Nginx或Apache）来处理静态文件，以提高性能和安全性。Gin框架的主要用途是用于构建动态的API和Web应用程序。

### 3.前端文件处理

支持的 "{{}}" 语法用法实际上取决于您所使用的具体模板引擎。

1. **Go 模板引擎 (text/template 和 html/template)：**

Go 语言内置了两个模板引擎：text/template 和 html/template。

- 数据绑定：`{{.}}` 表示当前上下文的数据对象。
- 循环：`{{range .Items}} ... {{end}}` 遍历数组或切片。
- 条件：`{{if .Condition}} ... {{else}} ... {{end}}` 根据条件进行条件判断。
- 访问对象属性：`{{.FieldName}}` 访问对象的字段。
- 调用函数：`{{funcName .Arg1 .Arg2}}` 调用自定义函数并传递参数。
- 定义模版：{{define}} 在其他地方，我们可以通过指定 "header" 这个名称来引用这个模板片段，以在其他模板中重用它：

```HTML
{{define "header"}}
  <header>
    <h1>{{.Title}}</h1>
    <nav>
      <ul>
        {{range .MenuItems}}
          <li>{{.}}</li>
        {{end}}
      </ul>
    </nav>
  </header>
{{end}}
{{template "header" .HeaderData}}
//循环演示        
router.GET("/index", func(c *gin.Context) {
                User := make(map[string]interface{})
                User["张三"] = 123
                User["李四"] = 666
                c.HTML(http.StatusOK, "index.html", User)
        })

<html>
//第一种遍历
{{range .}}<br>
{{.}}
{{end}}
//第二种
<br>
{{range $k,$v :=.}}
{{$k}}
{{$v}}<br>
{{end}}
</html>
```

1. **HTML 模板引擎 (html/template) 的 with 和 range 扩展：**

html/template 还支持一些扩展，如 `{{with}}` 和 `{{range}}` 可以在引用对象或数组之前为其设置临时变量：

```HTML
{{with .User}}
  <p>Name: {{.Name}}</p>
  <p>Email: {{.Email}}</p>
{{end}}

{{range .Items}}
  <p>{{.}}</p>
{{end}}
```

模板引擎是一种用于动态生成文本输出的工具或框架。它的主要目的是将数据和模板结合起来，产生最终的输出。在前端开发和后端开发中都可以使用模板引擎。

模板引擎的基本工作原理是通过将静态模板和动态数据结合，生成最终的文本输出。静态模板是包含了标记和占位符的文本文件，其中的占位符可以被动态数据替换。动态数据可以是来自后端服务器的数据，也可以是前端应用中的变量。

## 演示一

![202407092327490](https://raw.githubusercontent.com/lanyoumeng/Drawing-bed/main/docs/202407111949567.png)

```Go
// 自定义中间件
func myHandler() gin.HandlerFunc {
        return func(ctx *gin.Context) {
                ctx.Set("usersss", "拦截")
                ctx.Next()
        }
}
func main() {
        router := gin.Default()
        //注册中间件
        router.Use(myHandler())

        router.LoadHTMLGlob("../html321/*")
        router.Static("/source", "../source/*")

        router.GET("/page", func(c *gin.Context) {
                c.HTML(http.StatusOK, "index.html", gin.H{
                        "msg": "123",
                }) 
        })

        router.GET("/user/first", myHandler(), func(c *gin.Context) {
                //取出中间件的值
                usersss := c.MustGet("usersss").(string)
                log.Println(">>>>>>>", usersss)

                userid := c.Query("userid")
                username := c.Query("username")
                c.JSON(http.StatusOK, gin.H{
                        "userid":   userid,
                        "username": username,
                })

        })
        router.GET("/user/code/:id/:name", func(c *gin.Context) {
                userid := c.Param("id")
                username := c.Param("name")
                c.JSON(200, gin.H{
                        "userid":   userid,
                        "username": username,
                })

        })
        router.POST("/json", func(ctx *gin.Context) {
                data, _ := ctx.GetRawData()
                var m map[string]interface{}
                _ = json.Unmarshal(data, &m)
                ctx.JSON(200, m)

        })
        router.POST("/user/add", func(ctx *gin.Context) {
                username := ctx.PostForm("usname")
                password := ctx.PostForm("password")
                ctx.JSON(http.StatusOK, gin.H{
                        "username": username,
                        "password": password,
                })
        })

        //重定向
        router.GET("/test", func(ctx *gin.Context) {
                ctx.Redirect(301, "https://baidu.com")
        })
        //404
        router.NoRoute(func(ctx *gin.Context) {
                ctx.HTML(http.StatusNotFound, "404.html", nil)
        })
        usergroup := router.Group("/user")
        {
                usergroup.GET("/add")
                usergroup.GET("/code")
        }

        //中间件

        router.Run(":8081")
}

//http://8.130.16.80:8080/ping
```

## 演示二

```Go
// 服务端
type Student struct {
        Name   string
        Age    int
        Height float32
}

type Request struct {
        StudentId string `json:"student_id"`
}

// 从redis上根据studentId获取Student实体
func GetStudentInfo(studentId string) Student {
        client := redis.NewClient(&redis.Options{
                Addr:     "127.0.0.1:6379",
                Password: "",
                DB:       0,
        })
        ctx := context.TODO()
        stu := Student{}
        for field, value := range client.HGetAll(ctx, "学生1").Val() {
                if field == "Name" {
                        stu.Name = value
                } else if field == "Age" {
                        age, err := strconv.Atoi(value)
                        if err == nil {
                                stu.Age = age
                        }
                } else if field == "Height" {
                        height, err := strconv.ParseFloat(value, 10)
                        if err == nil {
                                stu.Height = float32(height)
                        }
                }
        }
        return stu
}

func GetName(ctx *gin.Context) {
        param := ctx.Query("student_id") //从Get请求中获取参数
        if len(param) == 0 {             //没有指定student_id参数
                ctx.String(http.StatusBadRequest, "please indidate student_id") //StatusBadRequest即400
                return
        }
        stu := GetStudentInfo(param)
        ctx.String(http.StatusOK, stu.Name) //StatusOK即200
        // ctx.JSON(http.StatusOK,stu)
        return
}

func GetAge(ctx *gin.Context) {
        param := ctx.PostForm("student_id") //从post form中获取参数
        if len(param) == 0 {                //没有指定student_id参数
                ctx.String(http.StatusBadRequest, "please indidate student_id") //StatusBadRequest即400
                return
        }
        stu := GetStudentInfo(param)
        ctx.String(http.StatusOK, strconv.Itoa(stu.Age)) //StatusOK即200
        // ctx.JSON(http.StatusOK,stu)
        return
}

func GetHeight(ctx *gin.Context) {
        var request Request
        err := ctx.BindJSON(&request)
        if err != nil {
                ctx.String(http.StatusBadRequest, "please indidate student_id in json")
                return
        }
        stu := GetStudentInfo(request.StudentId)
        ctx.String(http.StatusOK, strconv.FormatFloat(float64(stu.Height), 'f', 1, 64)) //保留1位小数
        // ctx.JSON(http.StatusOK, stu)
        return
}

func main() {
        client := redis.NewClient(&redis.Options{
                Addr:     "127.0.0.1:6379",
                Password: "",
                DB:       0,
        })
        ctx := context.TODO()
        err := client.HMSet(ctx, "学生1", "Name", "张三", "Age", "18", "Height", "173.5").Err()
        checkError(err)
        err = client.HSet(ctx, "学生2", "Name", "李四", "Age", 20, "Height", 180.0).Err()
        checkError(err)

        engine := gin.Default()
        engine.GET("/get_name", GetName)
        engine.POST("/get_age", GetAge)
        engine.POST("/get_height", GetHeight)

        err = engine.Run("0.0.0.0:2345")
        if err != nil {
                panic(err)
        }
}

func checkError(err error) {
        if err != nil {
                if err == redis.Nil {
                        fmt.Println("key不存在")
                } else {
                        fmt.Println(err)
                        os.Exit(1)
                }
        }
}
```

客户端

```Go
func TestGetStudentInfo(t *testing.T) {
        id := "学生1"
        stu := GetStudentInfo(id)
        if len(stu.Name) == 0 {
                t.Fail()
        } else {
                fmt.Printf("%+v\n", stu)
        }
}

func TestGetName(t *testing.T) {
        id := "学生1"
        resp, err := http.Get("http://127.0.0.1:2345/get_name?student_id=" + id)
        if err != nil {
                fmt.Println(err)
                t.Fail()
        } else {
                defer resp.Body.Close()
                bytes, err := ioutil.ReadAll(resp.Body)
                if err != nil {
                        fmt.Println(err)
                        t.Fail()
                } else {
                        fmt.Println(string(bytes))
                        // var stu Student
                        // err:=json.Unmarshal(bytes,&stu)
                        // if err!=nil{
                        //         fmt.Println(err)
                        //         t.Fail()
                        // }else{
                        //         fmt.Printf("%+v\n",stu)
                        // }
                }
        }
}

func TestGetAge(t *testing.T) {
        id := "学生1"
        //type Values map[string][]string
        resp, err := http.PostForm("http://127.0.0.1:2345/get_age", url.Values{"student_id": []string{id}})
        if err != nil {
                fmt.Println(err)
                t.Fail()
        } else {
                defer resp.Body.Close()
                bytes, err := ioutil.ReadAll(resp.Body)
                if err != nil {
                        fmt.Println(err)
                        t.Fail()
                } else {
                        fmt.Println(string(bytes))
                        // var stu Student
                        // err:=json.Unmarshal(bytes,&stu)
                        // if err!=nil{
                        //         fmt.Println(err)
                        //         t.Fail()
                        // }else{
                        //         fmt.Printf("%+v\n",stu)
                        // }
                }
        }
}

func TestGetHeight(t *testing.T) {
        reader := strings.NewReader(`{"student_id":"学生1"}`)
        request, err := http.NewRequest("POST", "http://127.0.0.1:2345/get_height", reader)
        if err != nil {
                fmt.Println(err)
                return
        }
        //当GET或POST请求需要携带Header时，只能通过http.Client发起请求，这是一种万能的请求方式
        request.Header.Add("Content-Type", "application/json")
        client := http.Client{}
        resp, err := client.Do(request)
        if err != nil {
                fmt.Println(err)
                t.Fail()
        } else {
                defer resp.Body.Close()
                bytes, err := ioutil.ReadAll(resp.Body)
                if err != nil {
                        fmt.Println(err)
                        t.Fail()
                } else {
                        fmt.Println(string(bytes))
                        // var stu Student
                        // err:=json.Unmarshal(bytes,&stu)
                        // if err!=nil{
                        //         fmt.Println(err)
                        //         t.Fail()
                        // }else{
                        //         fmt.Printf("%+v\n",stu)
                        // }
                }
        }
} 
```

## 中间件

#### 介绍

Gin 中间件，其实是一个 **type HandlerFunc func(Context)* 类型的函数

在 Web 开发中，我们要实现很多功能，例如：认证、授权、限流、熔断、设置请求/返回 Header（例如：请求 ID）、跨域等，这些都需要通过 Web 中间件的方式来实现，可以说中间件是 Web 框架或者 Web 服务非常核心的一个功能，一个中大型的 Web 应用基本都需要用到。

那么什么是 Web 中间件呢？简单来说，Web 中间件是 HTTP / RPC 请求必经的一个中间层，该中间层可以统一处理所有的请求，你可以根据需要开发不同功能的中间层。例如：你可以在中间层给所有的请求/返回头中添加 `X-Request-ID` 头，用以标识唯一一次请求，方便追踪、排障。

Gin 也具有强大的中间件能力，Gin 的中间件是基于洋葱模型的，如下图所示：

![202407092329919](https://raw.githubusercontent.com/lanyoumeng/Drawing-bed/main/docs/202407111930381.webp)

上图中，有 2 个中间件：Middleware A、Middleware B。HTTP 请求，从开始到结束经历的路径为：Middleware A -> Middleware B -> 主体函数 -> Middleware B -> Middleware A。执行顺序类似于栈。

从上图可以知道，Gin 中间件，其实可以起到请求前置拦截和后置拦截的功能：

- **请求前置拦截：** Web 请求到达我们定义的 HTTP 请求处理方法之前，拦截请求并进行相应处理；
- **请求后置拦截：** 在处理完成请求并响应客户端时，拦截响应并进行相应的处理。

#### 添加

##### **全局中间件：**

全局中间件设置之后对全局的路由都起作用。

我们可以通过默认路由来设置全局中间件，例如：`r.Use()`。可以根据需要设置 1 个或者多个：

```CSS
css复制代码router := gin.New()
//一次设置多个中间件 
router.Use(Logger(), Recovery())
//一次设置一个中间件
router.Use(gin.Logger())
router.Use(gin.Recovery())
```

##### **路由组中间件**

路由组中间件仅对该路由组下面的路由起作用。

可以通过以下 2 种方式来设置路由组中间件：

- 在声明路由组的同时设置路由组中间件，例如：

```CSS
css
复制代码 authorized := r.Group("/users", AuthRequired())
```

- 先声明路由组然后再通过 `Use` 方法进行设置，例如：

```CSS
css复制代码authorized := r.Group("/users")
authorized.Use(AuthRequired())
```

##### **单个路由中间件：**

单个路由中间件仅对一个路由起作用。

可以通过以下 2 种方式来设置单个路由中间件：

- 单个路由设置单个中间件，例如：

```Plaintext
arduino
复制代码authorized.POST("/login", loginEndpoint)
```

- 单个路由设置多个中间件，例如：

```Plaintext
vbnet
复制代码r.GET("/benchmark", MyBenchLogger(), benchEndpoint)
```

上述代码段中，各个中间件作用的路由如下：

- `r.Use(gin.Logger())`、`r.Use(gin.Recovery())` 将中间件添加在全局路由上，也就是说，所有请求路径以 `/` 开头的请求都会被 `gin.Logger()`、`gin.Recovery()` 中间件按顺序请求；
- `AuthRequired()` 中间件被添加在了 `authorized` 路由分组中，也就是所有请求路径以 `/users` 开头的请求，都会被 `AuthRequired()` 中间件处理；
- `analyticsEndpoint` 中间件被添加在了 `testing` 路由分组中，也就是所有请求路径以 `/users/testing` 开头的请求，都会被 `analyticsEndpoint` 中间件处理；
- `loginEndpoint` 中间件只作用在 `POST /users/login` 方法。

因为中间件会附加在每个请求的链路上，所以如果中间件性能不好，或者不稳定，影响的是所有 API 接口。

##### 实例

Gin 中添加中间件的方法如下：

```Go
func main() {
    // 创建一个不带任何中间件的路由
    r := gin.New()

    // 全局中间件
    // Logger 中间件将日志写到 gin.DefaultWriter，即使设置了 GIN_MODE=release
    // 默认设置 gin.DefaultWriter = os.Stdout
    r.Use(gin.Logger())

    // Recovery 中间件，从任何 panic 恢复，并返回一个 500 错误
   r.Use(gin.Recovery())

    // 对于每一个路由，如果有需要，可以添加多个中间件
    r.GET("/benchmark", MyBenchLogger(), benchEndpoint)

    // 授权组
    // authorized := r.Group("/users", AuthRequired())
    // 也可以这样
    authorized := r.Group("/users")
    // 在这个示例中，我们使用了一个自定义的中间件 AuthRequired()，该中间件只作用于 authorized 组
   authorized.Use(AuthRequired())
    {
        authorized.POST("/login", loginEndpoint)
        authorized.POST("/submit", submitEndpoint)
        authorized.POST("/read", readEndpoint)

        // 嵌套组
        testing := authorized.Group("testing")
       testing.GET( "/analytics" , analyticsEndpoint)
    }

    // 监听并服务于0.0.0.0:8080
    r.Run(":8080")
}
```

#### 自定义

```Go
// RequestID 是一个 Gin 中间件，用来在每一个 HTTP 请求的 context, response 中注入 `X-Request-ID` 键值对.
func RequestID() gin.HandlerFunc {
    return func(c *gin.Context) {
        // 检查请求头中是否有 `X-Request-ID`，如果有则复用，没有则新建
        requestID := c.Request.Header.Get(known.XRequestIDKey)

        if requestID == "" {
            requestID = uuid.New().String()
        }

        // 将 RequestID 保存在 gin.Context 中，方便后边程序使用
        c.Set(known.XRequestIDKey, requestID)

        // 将 RequestID 保存在 HTTP 返回头中，Header 的键为 `X-Request-ID`
        c.Writer.Header().Set(known.XRequestIDKey, requestID)
        c.Next()
    }
}
```

Gin 为中间件功能提供了 2 个核心方法，在开发中间件时会经常被用到：

- （1）`c.Next()`：在中间件中调用 `Next()` 方法，`Next()` 方法之前的代码会在到达请求方法前执行，`Next()` 方法之后的代码则在请求方法处理后执行，例如：

```SCSS
scss复制代码func MyMiddleware(c *gin.Context){
    //请求前
    c.Next()
    //请求后
}
```

- （2）`c.Abort()`：在中间件中调用 `Abort()` 方法，会直接终止请求。

# gorm

## 1.约定

GORM 倾向于约定，而不是配置。默认情况下，GORM 使用 ID 作为主键，使用结构体名的 蛇形复数 作为表名，字段名的 蛇形 作为列名，并使用 CreatedAt、UpdatedAt 字段追踪创建、更新时间

模型命名符合**驼峰命名法**，gorm会自动转换。若末尾不是数字结尾，模型会自动添加s。

在此约定下，结构名若为TestTb这种**大**[驼峰命名法](https://so.csdn.net/so/search?q=驼峰命名法&spm=1001.2101.3001.7020)时，创建的表名则为test_tbs ,同样的字段名为UserName时创建的列名则为user_name。

**gorm结构体名和表名**

1.使用 `TableName` 方法：

```Go
func (User) TableName() string {
    return "users" // 自定义表名为 "users"
}
GORM 将使用我们指定的 "users" 作为表名，而不是默认的 "users"（根据结构体名自动生成）。
```

2.使用 `gorm:"tableName:users"` 标签

```Go
type User struct {
    gorm.Model `gorm:"tableName:users"`
    FirstName  string `gorm:"column:first_name"`
    LastName   string `gorm:"column:last_name"`
    Email      string `gorm:"column:email"`
}
```

## 2.gorm.Model

GORM 定义一个 gorm.Model 结构体，其包括字段 ID、CreatedAt、UpdatedAt、DeletedAt

```Go
// gorm.Model 的定义
type Model struct {
  ID        uint           `gorm:"primaryKey"`
  CreatedAt time.Time
  UpdatedAt time.Time
  DeletedAt gorm.DeletedAt `gorm:"index"`
}
```

含有Model结构体的模型在进行删除操作时，是进行软删除，删除之后数据依旧存在于数据库，但不会被查找出来。含有Model结构体的模型经行查找时会判断DeletedAt是否为null。

## 3.gorm

### 字段标签

声明 model 时，tag 是可选的，GORM 支持以下 tag

column、type、size 、primaryKey

```Go
type User struct {
    gorm.Model
    FirstName string `gorm:"column:firstName"`
    LastName  string `gorm:"column:lastName"`
}
```

### 字段级权限控制

可导出的字段在使用 GORM 进行 CRUD 时拥有全部的权限，此外，GORM 允许您用标签控制字段级别的权限。这样您就可以让一个字段的权限是只读、只写、只创建、只更新或者被忽略 注意： 使用 GORM Migrator 创建表时，不会创建被忽略的字段

```Go
type User struct {
  Name string `gorm:"<-:create"` // 允许读和创建
  Name string `gorm:"<-:update"` // 允许读和更新
  Name string `gorm:"<-"`        // 允许读和写（创建和更新）
  Name string `gorm:"<-:false"`  // 允许读，禁止写
  Name string `gorm:"->"`        // 只读（除非有自定义配置，否则禁止写）
  Name string `gorm:"->;<-:create"` // 允许读和写
  Name string `gorm:"->:false;<-:create"` // 仅创建，禁止从db读
  Name string `gorm:"-"`            // 通过struct读写会忽略该字段
  Name string `gorm:"-:all"`        // 写入、读取和迁移结构时，忽略这个字段。
  Name string `gorm:"-:migration"`  // 迁移时忽略这个字段
}
```

## 4.连接

```Go
dsn := "user:password@tcp(localhost:3306)/dbname?charset=utf8mb4&parseTime=True&loc=Local"
db, err := gorm.Open(mysql.Open(dsn), &gorm.Config{})
if err != nil {
    panic("failed to connect database")
} 
defer db.Close() //记得关闭数据库连接
// 自动迁移模型，将模型的结构映射到数据库表中
    db.AutoMigrate(&User{})
```

**db.AutoMigrate(&User{})** 方法在以下情况下非常有用：

1. **应用程序启动时数据表创建和更新：** 当您的应用程序启动时，可以使用 `db.AutoMigrate(&User{})` 方法自动创建或更新数据库表。这样，您无需手动编写 SQL DDL 命令来创建表，而是通过 GORM 自动根据模型结构创建数据库表。如果表已经存在，`AutoMigrate` 方法还会检查表结构是否与模型结构一致，如果不一致，则会自动修改表结构以匹配模型结构。
2. **确保数据库表结构与模型一致：** 在开发过程中，模型结构可能会发生变化，例如添加新的字段或修改字段类型。使用 `AutoMigrate` 方法可以确保数据库表的结构与模型结构保持一致，避免由于模型变更而导致数据库结构不匹配的问题。
3. **无需手动处理数据库表变更：** 在开发过程中，如果您手动管理数据库表结构的变更，可能会变得非常繁琐和复杂。使用 `AutoMigrate` 方法可以简化这一过程，让 GORM 帮助您自动处理数据库表的变更，让您可以专注于应用程序的业务逻辑。
4. **初始化数据表：** 如果您需要在应用程序初始化阶段创建数据库表，并初始化一些初始数据，`AutoMigrate` 方法非常方便，它可以在应用程序启动时一次性完成这些任务。

总而言之，`db.AutoMigrate(&User{})` 方法是一个非常有用的工具，可以帮助您管理数据库表的创建和更新，并确保数据库表与模型结构保持一致。它特别适用于应用程序启动时的数据表管理和初始化阶段，能够简化开发流程，提高开发效率。

## 5.增删改查

```Go
type User struct{
    Id int //id 主键
    Keyword string `gorm :" column:keywords"`
    City string //city
}

// 创建记录
1.一条
user := User{Name: "John", Email: "john@example.com"}
db.Create(&user)
2.多条
users := []User{
    {FirstName: "John", LastName: "Doe", Email: "john@example.com"},
    {FirstName: "Jane", LastName: "Smith", Email: "jane@example.com"},
    {FirstName: "Bob", LastName: "Johnson", Email: "bob@example.com"},
}
db.Create(&users)

// 查询记录
1. 通过主键 ID 查询第一条记录
var user User
db.First(&user, 1) 

2.查询多个结果
func read(db *gorm.DB , city string) *user{
    var users []User
 多个   // db.where("city=?", city).Find(&users)
 多个  err:= db.select("id,city").where("city=?", city).limit(3).Find(&users).Error //用问号可以防止sql注入
   一个 // db.select("id,city").where("city=?", city).limit(3).First(&users)  First第一个
   一个 // db.select("id,city").where("city=?", city).limit(3).Take(&users)  Take随机一个
    if err != nil{  //错误处理
         panic("failed to read database")
    }
    if len(users)>0{
        return &users[0]
    }else{
        return nil
    }
}


// List 根据 offset 和 limit 返回 user 列表.
func (u *users) List(ctx context.Context, offset, limit int) (count int64, ret []*model.UserM, err error) {
        err = u.db.Offset(offset).Limit(defaultLimit(limit)).Order("id desc").Find(&ret).
                Offset(-1).
                Limit(-1).
                Count(&count).
                Error
        return
}
//  .Offset(-1) 和 .Limit(-1) //意思是不受偏移量和limit限制获取总的行数，它们不会影响查询结果，只是为了获取总行数。通常用于分页查询

3.主键特殊用法
func read(db *gorm.DB , city string) *user{
    var user User

   user.Id = 323  //主键特殊用法
   err:= db.select("id,city").where("city=?", city).First(&user).Error  //等价于where id =XXX and city =XXX
 
    if err != nil{  //错误处理
         panic("failed to read database")
    }
   
    return &user
}


// 更新记录        如果是新增或更新操作，Save 方法会更加方便，如果需要精细控制更新的字段，Update 方法更为适用。
1.Updates
一个   db.Model(&User).where("id=?" , 323).Update("city", "郑州")  //Model 指明是哪个表

db.Model(&User{}).Where("name = ?", "Alice").Updates(User{Name: "Alice Smith", Age: 31}) // 更新多个字段
db.Model(&User).where("id=?" , 323).Update( map[string]interface{}{"city":"郑州" , "keyword": "pkq"})//使用map更新  

2.Save
//1.GORM 会自动将生成的主键（如自增ID）回填到传入的结构体中
//2.Save 方法用于创建新记录或者更新已存在的记录，它会根据主键来判断是新增还是更新操作。如果结构体中定义的主键为空，则会执行插入操作；如果主键已经有值，则会执行更新操作。
//根据结构体的信息自动保存到相应的表中
user := User{Name: "Alice", Age: 25}
db.Save(&user) // 插入新记录或者更新已存在的记录

user.Age = 26
db.Save(&user) // 更新记录

如果是新增或更新操作，Save 方法会更加方便，如果需要精细控制更新的字段，Update 方法更为适用。

// 删除记录
db.where("id=?" , 323).Delete(&user)




// GetUserById  查询不到数据的情况
func (r *userRepo) GetUserById(ctx context.Context, Id int64) (*biz.User, error) {
    var user User
    result := r.data.db.Where(&User{ID: Id}).First(&user)
    if result.Error != nil {
        return nil, result.Error
    }

    if result.RowsAffected == 0 {
        return nil, status.Errorf(codes.NotFound, "用户不存在")
    }
    re := modelToResponse(user)
    return &re, nil
}
```

## 6.日志

在 GORM 中，日志级别用于控制是否输出 SQL 查询语句和执行过程的日志信息。不同的日志级别代表了不同的输出控制：

- `logger.Silent`：0表示完全关闭日志输出，不会输出任何日志信息。
- `logger.Error`：1表示只输出错误级别的日志信息，用于记录出现的错误。
- `logger.Warn`：2表示输出警告级别的日志信息，用于记录警告和错误。
- `logger.Info`：3表示输出信息级别的日志信息，用于记录常规的信息和错误。
- `logger.LogLevel(4)`：自定义日志级别，数字值对应不同的级别。在这里，数字 4 对应 `logger.Info`。

## 7.db.DB (*sql.DB)

```Go
        db, err := gorm.Open(mysql.Open(opts.DSN()), &gorm.Config{
                Logger: logger.Default.LogMode(logLevel),
        })
        if err != nil {
                return nil, err
        }        

        sqlDB, err := db.DB()
        if err != nil {
                return nil, err
        }

        // SetMaxOpenConns 设置到数据库的最大打开连接数
        sqlDB.SetMaxOpenConns(opts.MaxOpenConnections)

        // SetConnMaxLifetime 设置连接可重用的最长时间
        sqlDB.SetConnMaxLifetime(opts.MaxConnectionLifeTime)

        // SetMaxIdleConns 设置空闲连接池的最大连接数
        sqlDB.SetMaxIdleConns(opts.MaxIdleConnections)
```

在这段代码中，`db.DB()` 方法被用来获取 GORM 库底层的 `*sql.DB` 实例。GORM 使用 `*sql.DB` 来管理与数据库的连接池和连接相关的配置。

`*sql.DB` 是 Go 语言标准库 `database/sql` 中的一个类型。它代表了与特定数据库的连接，是一个数据库连接对象，用于进行底层的数据库操作和连接管理。`*sql.DB` 类型实现了 `database/sql` 包中的 `DB` 接口。

在 `database/sql` 包中，`*sql.DB` 类型的主要作用有：

1. 执行数据库查询和事务处理。
2. 管理数据库连接池，自动维护和复用连接。
3. 设置数据库连接的一些配置，如最大连接数、最大空闲连接数、连接超时等。
4. 实现了 `Query`, `QueryRow`, `Exec` 等用于执行 SQL 查询的方法。
5. 支持通过上下文 `context.Context` 来控制查询的上下文超时和取消。

需要注意的是，`*sql.DB` 是并发安全的，可以在多个 goroutine 中并发使用，因为它已经实现了内部的连接池和连接管理机制。因此，当需要进行数据库操作时，可以共享一个 `*sql.DB` 实例，而不必每次都重新连接和关闭数据库。

## 8.钩子函数

1. BeforeCreate：在创建数据库记录之前执行自定义逻辑。
2. AfterCreate：在创建数据库记录之后执行自定义逻辑。
3. BeforeUpdate：在更新数据库记录之前执行自定义逻辑。
4. AfterUpdate：在更新数据库记录之后执行自定义逻辑。
5. BeforeDelete：在删除数据库记录之前执行自定义逻辑。
6. AfterDelete：在删除数据库记录之后执行自定义逻辑。
7. BeforeSave：在保存（包括创建和更新）数据库记录之前执行自定义逻辑。
8. AfterSave：在保存（包括创建和更新）数据库记录之后执行自定义逻辑。

## 9.事务

```Go
func main() {
        db, err := gorm.Open("sqlite3", "test.db")
        if err != nil {
                panic("Failed to connect to database")
        }
        defer db.Close()

        // 开始事务
        tx := db.Begin()

        // 在事务中执行数据库操作
        user1 := User{Name: "Alice"}
        user2 := User{Name: "Bob"}

        if err := tx.Create(&user1).Error; err != nil {
                tx.Rollback() // 操作出错，回滚事务
                fmt.Println("Error creating user1:", err)
                return
        }

        if err := tx.Create(&user2).Error; err != nil {
                tx.Rollback() // 操作出错，回滚事务
                fmt.Println("Error creating user2:", err)
                return
        }

        // 提交事务
        tx.Commit()

        fmt.Println("Users created successfully")
}
```

db.Transaction

```Go
func main() {
        db, err := gorm.Open("sqlite3", "test.db")
        if err != nil {
                panic("Failed to connect to database")
        }
        defer db.Close()

        // 开始事务
    //  事务回调函数的签名是 func(*gorm.DB) error
        err = db.Transaction(func(tx *gorm.DB) error {
                user1 := User{Name: "Alice"}
                user2 := User{Name: "Bob"}

                if err := tx.Create(&user1).Error; err != nil {
                        return err // 返回错误，事务将被回滚
                }

                if err := tx.Create(&user2).Error; err != nil {
                        return err // 返回错误，事务将被回滚
                }

                // 返回 nil 表示事务将被提交
                return nil
        })

        if err != nil {
                fmt.Println("Error during transaction:", err)
                return
        }

        fmt.Println("Users created successfully")
}
```

## 10.外键和关联

GORM 会为关联创建外键约束，您可以在初始化过程中禁用此功能：

```Plaintext
db, err := gorm.Open(sqlite.Open("gorm.db"), &gorm.Config{
  DisableForeignKeyConstraintWhenMigrating: true,
})
```

**为什么不用外键**

1、减少了额外的查询，不用检查数据是否违反完整性，提升了性能。 2、数据源进行切换的时候，不互相产生影响。 3、分库分表十分方便，不存在强制耦合。 4、批量导入数据方便 如果有外键，要确定数据导入的顺序。

### 区别

外键（Foreign Key）：

- **数据库级别的约束：** 外键是一种数据库级别的约束，用于保持数据完整性。
- **指定数据表关联：** 外键在数据库模式中指定了两个表之间的关联，确保一个表的特定列值只能引用另一个表中的列值，或者为 NULL（如果允许的话）。
- **约束数据一致性：** 它确保了引用表的数据一致性，防止在关联表中插入不存在的引用值。

关联关系（Relationship）：

- **逻辑层面的关联：** 关联关系描述了数据之间的关系，不一定需要在数据库级别有外键约束。
- **ORM 或应用层的概念：** 它是在应用程序或 ORM（对象关系映射）中定义的概念，用于表示不同实体之间的关联，但不一定直接映射到数据库中的外键约束。
- **方便数据查询：** 关联关系可以用来方便地查询和操作相关联的数据，但并不强制要求数据库层面的外键约束。

虽然外键约束是保持数据库完整性的一种重要机制，并且通常与关联关系相关，但关联关系并不一定要求在数据库中实现外键约束。某些情况下，应用程序或者 ORM 可能会依赖于开发者手动维护关联关系，而不是依赖于数据库层面的外键约束。

### 关联

#### Belongs To

#### Has One

与另一个模型建立一对一的关联

##### 声明

```Go
// User 有一张 CreditCard，UserID 是外键
type User struct {
  gorm.Model
  CreditCard CreditCard
}

type CreditCard struct {
  gorm.Model
  Number string
  UserID uint
}
```

##### 检索

```Go
// 检索用户列表并预加载信用卡
func GetAll(db *gorm.DB) ([]User, error) {
    var users []User
    err := db.Model(&User{}).Preload("CreditCard").Find(&users).Error
    return users, err
}
```

##### 重写外键

对于 `has one` 关系，同样必须存在外键字段。拥有者将把属于它的模型的主键保存到这个字段。

这个字段的名称通常由 `has one` 模型的类型加上其 `主键` 生成，对于上面的例子，它是 `UserID`。

为 user 添加 credit card 时，它会将 user 的 `ID` 保存到自己的 `UserID` 字段。

如果你想要使用另一个字段来保存该关系，你同样可以使用标签 `foreignKey` 来更改它，例如：

```Go
type User struct {
  gorm.Model
  CreditCard CreditCard `gorm:"foreignKey:UserName"` // 使用 UserName 作为外键
}

type CreditCard struct {
  gorm.Model
  Number   string
  UserName string
}
```

##### 重写引用

默认情况下，拥有者实体会将 `has one` 对应模型的主键保存为外键，您也可以修改它，用另一个字段来保存，例如下面这个使用 `Name` 来保存的例子。

您可以使用标签 `references` 来更改它，例如：

```Go
type User struct {
  gorm.Model
  Name       string     `gorm:"index"`
  CreditCard CreditCard `gorm:"foreignKey:UserName;references:name"`
}

type CreditCard struct {
  gorm.Model
  Number   string
  UserName string
}
```

#### Has Many

#### Many To Many

#### 关联模式

## 11.预加载

在 GORM 中，预加载（Preloading）是一种机制，允许你在进行查询的同时，一次性获取相关联的数据，避免了多次查询数据库。这可以通过 `Preload` 方法来实现。

### 基本用法

使用 `Preload` 方法可以在进行查询时预先加载相关联的数据。例如，如果你有一个 `User` 结构体和一个 `UserCount` 结构体，它们之间有关联，你可以这样预加载 `User` 数据：

```Go
var userCount UserCount
db.Preload("User").First(&userCount, 1)
```

这段代码会在查询 `UserCount` 数据时，同时预加载关联的 `User` 数据。这样，在获取 `UserCount` 实例的同时，相关联的 `User` 数据也会被获取到。

### 嵌套预加载

如果你有多个关联，并且想要预加载多个层级的关联数据，可以使用嵌套的 `Preload` 方法：

```Go
var user User
db.Preload("UserCount").Preload("UserCount.AnotherRelatedModel").First(&user, 1)
```

这种方法允许你在一个查询中预加载多个层级的关联数据，便于在获取数据时一次性获取所有相关联的数据。

### 条件预加载

有时你可能需要对预加载的数据进行筛选，可以在 `Preload` 中使用条件：

```Go
var user User
db.Preload("UserCount", "follow_count > ?", 10).First(&user, 1)
```

这个例子中，`follow_count > ?` 是一个条件，指定了只预加载 `UserCount` 中 `follow_count` 大于 10 的数据。

通过这些方式，你可以使用 GORM 的 `Preload` 方法在查询时预加载相关联的数据，提高查询效率并减少数据库访问次数。

## 错误处理

### 处理查询结果为空的情况

1. **多条记录查询 (****`Find`****)**：回一个空的切片。
2. **单条记录查询 (****`First`****,** **`Take`****)**：返回 `gorm.ErrRecordNotFound` 错误。

```Go
var users []User
result := db.Where("name = ?", "NonExistentUser").Find(&users)

if result.Error != nil {
fmt.Println("Error occurred:", result.Error)
} else if len(users) == 0 {
fmt.Println("No users found")
} else {
fmt.Println("Users found:", users)
}




result := db.Where("name = ?", "NonExistentUser").First(&user)
if result.Error != nil {
if result.Error == gorm.ErrRecordNotFound {
fmt.Println("No user found")
} else {
fmt.Println("Error occurred:", result.Error)
}
} else {
fmt.Println("User found:", user)
}
```

## 软删除

软删除是一种在数据库中标记数据为已删除而不是实际从数据库中移除数据的技术。通常情况下，软删除通过在表中添加一个额外的列来实现，该列用于记录删除的时间或者标记是否已删除。这个额外的列通常被称为“删除标记”。

在软删除中，当执行删除操作时，不是真正地从数据库中删除记录，而是更新了删除标记，通常是将某个时间戳值设置为表明删除的时间点，或者将一个特殊的标记值设置为表示已删除状态。

软删除的优点在于：

1. **数据可恢复性：** 软删除保留了被删除数据的信息，因此可以随时根据需要恢复这些数据。
2. **数据完整性：** 在一些业务场景中，需要保留历史数据以用于分析、审计或其他目的，软删除可以帮助维护数据的完整性。
3. **避免信息泄露：** 软删除可以避免直接删除敏感数据，从而减少信息泄露的风险。

然而，软删除也有一些需要注意的地方：

1. **查询复杂性：** 查询已删除和未删除的数据需要增加一些条件，可能导致查询变得更复杂。
2. **存储开销：** 软删除会占用额外的存储空间，因为已删除的数据仍然存在于数据库中。

在软删除中，需要注意处理查询时过滤已删除的数据，以及在需要时提供恢复已删除数据的机制。常见的软删除实现方式包括使用标记字段（如上文提到的 `DeletedAt` 字段）或者使用标记状态（如一个 `is_deleted` 字段）。这取决于业务需求和使用的数据库管理系统。

```Go
type Video struct {
        Id       int64 `json:"id" gorm:"column:id;primaryKey;autoIncrement;comment:视频id"`
    
        CreatedAt time.Time      `gorm:"column:create_at;autoCreateTime"`
        UpdatedAt time.Time      `gorm:"column:update_at;autoUpdateTime"`
        DeletedAt gorm.DeletedAt // 软删除 删除时间
}
```

# Kratos

```Bash
kratos new user
//biz目录下user.go 接口上的注释
//go:generate mockgen -destination=../mocks/mrepo/user.go -package=mrepo . UserRepo

cd user
make init
go get github.com/golang/mock/mockgen
go get google.golang.org/grpc@latest


main.go 改Name 
注意id不能重复

# 生成所有proto源码、wire等等
go generate ./...

kratos proto add api/user/v1/user.proto
make api #这时可以看到 user/api/user/v1/ 目录下多出了 proto 创建的文件
//或者 kratos proto client api/user/v1/user.proto 


kratos proto server api/user/v1/user.proto -t internal/service
kratos proto server api/favorite/v1/favorite.proto -t internal/service
kratos proto server api/comment/v1/comment.proto -t internal/service
kratos proto server api/relation/v1/relation.proto -t internal/service
kratos proto server api/message/v1/message.proto -t internal/service

#user/configs/config.yaml和user/internal/conf/conf.proto要保持一样
make config  #新生成 user/internal/conf/conf.pb.go 文件，执行 


 cd cmd/user/ && wire

 kratos run
 

 
$ cd message
$ go generate ./...
$ go build -o ./bin/ ./... 
$ ./bin/message -conf ./configs

#日志用法
v.log.Info("Error creating user1:", err)
```
