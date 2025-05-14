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

## 语言思想

### 组合

在 Go 中，没有传统意义上的类继承，而是通过**组合（composition）\**来实现代码复用和扩展功能。组合在 Go 中通常通过\**嵌入（embedding）\**实现，这允许一个结构体直接嵌入另一个结构体，从而获得其字段和方法。这样，嵌入的结构体被称为\**匿名字段**，它的所有方法可以直接在外层结构体上调用。

例如：

```go
package main

import "fmt"

// 定义一个基础结构体
type Person struct {
    Name string
}

func (p Person) Greet() {
    fmt.Println("Hello, my name is", p.Name)
}

// 定义一个Employee，通过组合（嵌入Person）实现复用
type Employee struct {
    Person  // 匿名字段，实现组合
    Company string
}

func main() {
    emp := Employee{
        Person:  Person{Name: "Alice"},
        Company: "Tech Co.",
    }
    // 直接调用组合自Person的Greet方法
    emp.Greet()  // 输出: Hello, my name is Alice
}
```

在这个例子中，`Employee` 通过嵌入 `Person`，不仅拥有了 `Person` 的字段，还可以直接调用 `Person` 定义的方法。这种方式使得 Go 的组合（composition）成为一种灵活、优雅的代码复用机制，也是一种推荐的设计模式，通常被认为比传统的继承更简单、透明且易于维护。

这种组合不仅可以用于简单的结构体嵌入，也可以通过接口实现更高层次的抽象和灵活性，让代码更符合面向接口编程的思想。



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

1.**不需要break**

  可以不指定case表达式的值，且可以进行**多条件判断**。

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



#### select

在 Go 语言中，`select` 语句是一个用于处理多个通道操作的控制结构。它允许在多个通道（`channel`）之间进行选择，并且可以根据通道的状态执行不同的逻辑。`select` 的作用类似于 `switch` 语句，但它主要用于处理并发场景中的多通道通信，而不是条件判断。

##### 1. **`select` 的基本概念**
`select` 语句在 Go 中的作用是监听多个通道操作（包括通道的发送和接收操作），当其中一个通道操作可以进行时，它就会执行相应的 `case` 分支。`select` 主要用于协调 goroutine 之间的通信，能够避免阻塞，提升程序的并发性能。

##### 2. **`select` 语法结构**
`select` 的语法和 `switch` 类似，但是它只能用于处理通道（`channel`）的操作。基本语法如下：

```go
select {
case <-channel1:
    // 当从 channel1 中接收到数据时执行的代码块
case channel2 <- value:
    // 当向 channel2 发送数据时执行的代码块
default:
    // 当所有的 case 都不满足条件时执行（可选）
}
```

- 每个 `case` 分支表示一个通道操作（接收或发送）。
- 只有一个通道操作可以被执行时，`select` 会选择该通道，并执行对应的代码块。
- 如果有多个 `case` 分支都可以执行，`select` 会**随机**选择一个进行处理。
- 如果没有任何通道操作可以执行，并且存在 `default` 分支，则 `default` 会立即执行。
- 如果没有 `default` 分支且所有通道都阻塞，则 `select` 本身也会阻塞，直到某个通道操作可以执行。

##### 3. **`select` 示例**

###### 3.1 基本示例：监听多个通道的接收
```go
package main

import (
    "fmt"
    "time"
)

func main() {
    ch1 := make(chan string)
    ch2 := make(chan string)

    go func() {
        time.Sleep(1 * time.Second)
        ch1 <- "Hello from ch1"
    }()

    go func() {
        time.Sleep(2 * time.Second)
        ch2 <- "Hello from ch2"
    }()

    select {
    case msg1 := <-ch1:
        fmt.Println(msg1)  // 如果 ch1 先有数据，打印 ch1 的消息
    case msg2 := <-ch2:
        fmt.Println(msg2)  // 如果 ch2 先有数据，打印 ch2 的消息
    case <-time.After(3 * time.Second):
        fmt.Println("Timeout!")
    }
}
```

**输出结果：**
```
Hello from ch1
```

在这个例子中：
- 两个 goroutine 分别向 `ch1` 和 `ch2` 发送数据，`ch1` 在 1 秒后发送，`ch2` 在 2 秒后发送。
- `select` 语句会等待任意一个通道有数据可接收。
- 因为 `ch1` 先接收到了数据，所以输出 `"Hello from ch1"`。
- 如果两个通道都没有数据，并且超过 3 秒，则 `select` 会执行 `time.After` 返回的超时分支并输出 `"Timeout!"`。

###### 3.2 示例：发送和接收操作
```go
package main

import (
    "fmt"
)

func main() {
    ch := make(chan int)
    done := make(chan bool)

    // 向通道发送数据的 goroutine
    go func() {
        for i := 0; i < 5; i++ {
            ch <- i
        }
        close(ch) // 关闭通道
    }()

    // 使用 select 语句接收通道的数据
    go func() {
        for {
            select {
            case val, ok := <-ch:
                if !ok {
                    done <- true // 当通道关闭时，跳出循环
                    return
                }
                fmt.Println("Received:", val)
            }
        }
    }()

    <-done // 等待 goroutine 执行完毕
}
```

**输出结果：**
```
Received: 0
Received: 1
Received: 2
Received: 3
Received: 4
```

在这个例子中：
- 第一个 goroutine 向 `ch` 通道发送数据，并在发送完后关闭通道。
- 第二个 goroutine 使用 `select` 语句监听 `ch` 通道，并在通道关闭时发送一个信号到 `done` 通道，结束程序。
- `select` 用来选择 `ch` 的接收操作，当 `ch` 中有数据时执行 `case` 语句，当通道关闭时（`ok == false`）结束接收操作。

##### 4. **`select` 的典型应用场景**

###### 4.1 **通道的超时控制**
使用 `select` 可以轻松地实现**通道的超时控制**。在网络请求或等待信号时，如果某个操作长时间没有响应，可以使用 `select` 和 `time.After` 来避免程序被永久阻塞。

```go
select {
case msg := <-ch:
    fmt.Println("Received:", msg)
case <-time.After(2 * time.Second):
    fmt.Println("Operation timed out!")
}
```

在这个例子中，如果 `ch` 通道在 2 秒内没有接收到数据，则 `select` 会自动执行 `time.After` 分支，输出 `"Operation timed out!"`。

###### 4.2 **多个通道之间的调度**
当一个程序中有多个通道需要同时监听时，`select` 能够根据不同通道的状态，动态地选择一个准备好的通道进行处理。它特别适合用来实现任务的调度和优先级控制。

```go
select {
case job := <-jobChannel:
    fmt.Println("Received job:", job)
case result := <-resultChannel:
    fmt.Println("Received result:", result)
default:
    fmt.Println("No activity")
}
```

如果 `jobChannel` 或 `resultChannel` 中的任一通道有数据，则优先处理该通道；如果都没有，则立即执行 `default` 分支输出 `"No activity"`。

##### 5. **注意事项**
- **`select` 不支持空的 `case` 语句**：每个 `case` 分支必须有具体的通道操作，不能仅仅是一个空的 `case` 语句。
- **`select` 中的 `default` 分支是非阻塞的**：如果 `select` 中存在 `default` 分支，则 `select` 语句永远不会阻塞。它会立即执行 `default` 分支，即使其他 `case` 也没有准备好。
- **`select` 的随机性**：如果多个通道同时准备好，`select` 会随机选择其中一个执行。因此，`select` 并不保证对多个通道的公平性。



------



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

#### init

可以在同一个包中定义多个 `init` 函数，甚至在同一个文件中定义多个 `init` 函数

`init` 函数按照它们在代码中出现的顺序依次执行。多个 `init` 函数的执行顺序如下：

1. 在同一个包内，按照文件的词法顺序（文件名顺序）依次初始化。
2. 同一个文件中，按照 `init` 函数的出现顺序依次执行。
3. 执行完所有 `init` 函数后，程序会进入 `main` 函数。

注意事项

- `init` 函数不能有任何参数，也没有返回值。
- `init` 函数的主要作用是完成程序启动时的必要初始化工作，不建议在 `init` 中做过于复杂的操作。



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





#### 空结构体 (`struct{}`)

空结构体是指没有任何字段的结构体，定义如下：

```go
type Empty struct{}
```

空结构体本质上是一个大小为 **零字节（0-byte）** 的数据类型，因为它不包含任何字段，因此在内存中不占用实际空间。

2. **空结构体的特点**

- **不占用内存**：因为空结构体没有字段，所以其实例不会消耗任何内存。
- **用来表示一种状态或信号**：通常用于表示某种状态、行为或事件的存在，而不需要存储具体的数据。

3. **空结构体的应用场景**

- **节省内存**：在需要大规模创建很多对象或元素时，使用空结构体可以节省大量内存。

  例如，一个表示集合的 `map`，只需要使用 `struct{}` 作为值类型来表示某个键的存在，而不需要使用 `bool` 等其它类型：

  ```go
  // 使用空结构体节省内存
  set := map[string]struct{}{"apple": {}, "banana": {}, "cherry": {}}
  ```

- **实现信号传递**：在使用 `channel` 传递数据时，可以用空结构体表示信号，而不是具体的数据值，从而节省内存。

  ```go
  done := make(chan struct{})
  go func() {
      // 执行一些操作
      done <- struct{}{}  // 使用空结构体表示操作完成的信号
  }()
  <-done
  ```

- **实现功能标记**：空结构体经常用于结构体或函数标记，以表示某个特性或行为。

- **同步机制**：在 `sync.Map` 或 `sync.Pool` 等同步数据结构中，使用 `struct{}` 作为标记来标识数据存储和操作行为。

4. **注意事项**

虽然空结构体不占用内存空间，但其指针却会消耗内存，因此需要注意在设计数据结构时尽量使用值而非指针。



#### 空接口与空结构体的区别

- **内存占用**：
  - 空接口（`interface{}`）本身占用内存空间，因为它需要维护类型和值信息（通常是 16 字节：8 字节的类型信息指针 + 8 字节的数据指针）。
  - 空结构体（`struct{}`）不占用任何内存（0 字节），仅表示某种状态或标记。

- **使用目的**：
  - 空接口用于 **表示任意类型的值**，方便泛型编程、动态数据处理和类型断言。
  - 空结构体用于 **表示一种状态或信号**，多用于表示存在性、集合数据结构和信号传递。

- **性能开销**：
  - 使用空接口会引入 **类型断言和反射** 时的性能开销。
  - 使用空结构体则没有这些开销，非常轻量。

四、总结

- **空接口 (`interface{}`)**：表示任意类型，广泛用于泛型函数和动态数据处理。
- **空结构体 (`struct{}`)**：表示一种状态或存在性，常用于内存优化和信号传递场景。

在实际项目中，合理使用空接口和空结构体可以提高代码的灵活性和性能。





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
type Person struct {
    Name string
}

func (p Person) Speak() string {
    return "Hi, I'm " + p.Name
}

func main() {
    var s Speaker
    s = Person{Name: "Alice"} // 自动实现了 Speaker 接口
    fmt.Println(s.Speak())    // 输出: Hi, I'm Alice
}


///////////////////////////////////
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



####  **反射与空接口**

空接口 (`interface{}`) 是 **没有任何方法约束** 的接口类型。因此，**所有类型都实现了空接口**，因为它不要求实现任何方法。

反射可以获取接口类型的元信息，并对其进行操作。可以通过 `reflect.TypeOf()` 和 `reflect.ValueOf()` 获取接口的具体类型和值。

例如：

```go
func main() {
    var x interface{} = 3.14
    fmt.Println("Type:", reflect.TypeOf(x))  // 输出：float64
    fmt.Println("Value:", reflect.ValueOf(x)) // 输出：3.14
}
```

**空接口的应用场景**

- **通用类型参数**：在设计函数时，如果参数类型不确定，可以使用空接口。

  例如，一个可以接收任意类型参数的打印函数：

  ```go
  func PrintAny(a interface{}) {
      fmt.Println(a)
  }
  ```

- **数据类型的动态处理**：在需要处理不同数据类型的场景中（如 JSON 解码、配置解析），空接口可以用来接收动态数据类型。

  例如，JSON 数据解码时可以先解码到 `map[string]interface{}` 中：

  ```go
  var data map[string]interface{}
  json.Unmarshal([]byte(jsonStr), &data)
  ```

- **类型断言和类型切换**：通过类型断言（`v, ok := i.(type)`）来获取接口内部存储的数据类型。

- 



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



#### any

在 Go 1.18 及以上版本中，`any` 是一种类型别名，代表了空接口 `interface{}`。`any` 类型可以用来表示任何类型的值，因为空接口可以接受任何类型的变量。这是 Go 引入泛型（Generics）之后的一部分，旨在提高代码的通用性和简洁性。

- `any` 是 `interface{}` 的别名，用于表示可以接受任意类型的变量。
- 主要应用在泛型函数、数据结构和灵活接口中，增加了代码的通用性。
- 使用 `any` 时，建议通过类型断言或类型转换确保类型安全。

##### `any` 的定义

在 Go 中，`any` 被定义为：
```go
type any = interface{}
```

这意味着 `any` 和 `interface{}` 是完全等价的，二者都可以存储任何类型的值。这也使得 `any` 类型可以用于泛型编程、通用数据结构（如切片、字典等），以及需要兼容多种类型的场景。

##### `any` 的使用场景

1. **泛型函数**：`any` 常用于泛型函数的参数或返回值中，允许函数处理多种数据类型。
   ```go
   func PrintAny(value any) {
       fmt.Println(value)
   }
   
   func main() {
       PrintAny(42)
       PrintAny("hello")
       PrintAny([]int{1, 2, 3})
   }
   ```

2. **数据结构**：`any` 可以用于数据结构（如切片、字典等）中，以便存储不同类型的值。
   ```go
   func main() {
       var data []any
       data = append(data, 1, "string", true, 3.14)
       fmt.Println(data) // 输出: [1 string true 3.14]
   }
   ```

3. **接口参数**：一些通用的库或函数接口（如日志库、配置库等）通常会使用 `any` 类型来接受任意类型的参数，以提供更大的灵活性。

##### `any` 和类型断言

由于 `any` 实际上是 `interface{}`，因此可以通过**类型断言**从 `any` 类型的变量中获取具体类型值：
```go
func main() {
    var value any = "hello"
    str, ok := value.(string) // 断言为 string 类型
    if ok {
        fmt.Println("字符串值:", str)
    } else {
        fmt.Println("类型不匹配")
    }
}
```

##### `any` 的限制

- **性能开销**：`any` 类型存储的值需要进行类型装箱和解包，存在一定的性能开销，尤其在频繁使用时。
- **类型安全性**：`any` 虽然增加了灵活性，但使用时需要注意类型断言，避免因为类型不匹配引发的运行时错误。





------



------

####  接口值和动态类型

接口值由两个部分构成：**动态类型**和**动态值**。例如：

- 当将 `Person{Name: "Alice"}` 赋值给 `Speaker` 类型的变量时，

  - 动态类型是 `Person`，
  - 动态值则是具体的 `Person` 实例。

- 接口变量可以通过类型断言（type assertion）或类型开关（type switch）来恢复具体类型：

  ```go
  s := Person{Name: "Alice"}
  var sp Speaker = s
  
  // 类型断言
  p, ok := sp.(Person)
  if ok {
      fmt.Println("Person's name is", p.Name)
  }
  
  // 类型开关
  switch v := sp.(type) {
  case Person:
      fmt.Println("Person:", v.Name)
  default:
      fmt.Println("Unknown type")
  }
  ```

------

#### 接口与方法集

- **方法集**：一个类型的方法集决定了它能实现哪些接口。对于非指针类型 `T`，它的方法集包含所有以 `T` 为接收者的方法；而对于指针类型 `*T`，它的方法集既包含指针接收者的方法，也包含值接收者的方法。
- **注意事项**：如果方法的接收者为指针，则只有指针类型才能实现该接口；而如果方法的接收者为值，则该类型的值和指针都可以实现该接口。

例如：

```go
type Animal interface {
    Speak() string
}

type Cat struct {
    Name string
}

// 值接收者
func (c Cat) Speak() string {
    return c.Name + " says meow"
}

func main() {
    var a Animal
    c := Cat{Name: "Kitty"}
    a = c      // Cat 实现了 Animal
    a = &c     // 也可以赋值为指针，因为方法集兼容
    fmt.Println(a.Speak())
}
```



------





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



#### recover

在 Go 语言中，`recover` 是一种用于从恐慌（panic）状态中恢复的机制。它允许程序在遇到运行时错误时，不直接崩溃，而是优雅地处理错误并继续执行或退出。

使用场景

`recover` 主要用于以下场景：

1. **错误处理**：在 goroutine 中处理可能的错误，以避免整个程序崩溃。
2. **保护关键代码**：确保关键部分的代码能够在出现问题时恢复，通常与 `defer` 一起使用。

语法

`recover` 只能在 `defer` 延迟调用的函数中使用。调用 `recover` 会返回导致恐慌的值。如果没有发生恐慌，`recover` 返回 `nil`。

示例代码

以下是一个使用 `recover` 的简单示例：

```go
package main

import (
	"fmt"
)

// 处理恐慌的函数
func handlePanic() {
	if r := recover(); r != nil {
		fmt.Println("发生恐慌:", r)
	}
}

// 一个可能导致恐慌的函数
func mayPanic() {
	defer handlePanic() // 使用 defer 确保在函数结束时调用 handlePanic
	panic("这是一个恐慌")  // 触发恐慌
}

func main() {
	fmt.Println("程序开始")
	mayPanic() // 调用可能导致恐慌的函数
	fmt.Println("程序继续运行") // 恢复后，程序继续执行
}
```

注意事项

1. **仅在适当的地方使用**：`recover` 应该用于处理运行时错误，而不是用于控制程序的流。在正常情况下，应该使用错误返回值来处理错误。
  
2. **必须配合 `defer` 使用**：`recover` 只能在 `defer` 语句中有效。

3. **程序的退出**：即使使用 `recover`，程序可能仍然需要在某些情况下退出。如果有严重的错误，应该决定是否继续执行。

通过使用 `recover`，您可以编写更稳健的代码，有效处理运行时错误，防止程序意外崩溃。



------

#### GMP 的关系

**GMP 的关键点**：

1. 一个 `M` 执行一个 `G`，通过 `P` 绑定 Goroutine 到具体的线程（`M`）。
2. Goroutine 的栈是动态增长的（初始仅 2KB，最大 1GB），栈管理直接影响 `panic` 和 `recover` 的实现。
3. 每个 Goroutine 是隔离的，因此一个 Goroutine 中的 `panic` 不会直接影响其他 Goroutine。

- **GMP 的关系：**
  - `panic` 和 `recover` 的行为依赖于 Goroutine 的隔离性和栈管理。
  - GMP 保证了多 Goroutine 的调度独立，即使某个 Goroutine 崩溃，其他 Goroutine 仍能正常运行。
- **实现机制：**
  - 每个 Goroutine 都有独立的 `_panic` 和 `_defer` 链表，栈展开局限于当前 Goroutine。
  - 调度器负责确保并发安全和崩溃 Goroutine 的资源回收。



##### **2.1 Goroutine 的隔离性**

- 每个 Goroutine 的栈是独立的，因此 `panic` 只会在触发它的 Goroutine 的栈中展开。
- 其他 Goroutine 不会直接感知到某个 Goroutine 的 `panic`，除非有显式的共享机制（如 `channel` 或全局变量）。

**示例：**

```go
package main

import (
	"fmt"
	"time"
)

func main() {
	go func() {
		defer func() {
			if r := recover(); r != nil {
				fmt.Println("Goroutine recovered from panic:", r)
			}
		}()
		panic("Panic in Goroutine!")
	}()

	time.Sleep(time.Second) // 保证 Goroutine 执行完
	fmt.Println("Main Goroutine continues...")
}
```

输出：

```
Goroutine recovered from panic: Panic in Goroutine!
Main Goroutine continues...
```

- `panic` 只在发生的 Goroutine 内部展开，主 Goroutine 不受影响。
- **原因**：每个 Goroutine 的栈是独立的，由 GMP 模型保证。

------

##### **2.2 Goroutine 的栈和 panic 栈展开**

当 `panic` 触发时：

1. Go 运行时会保存 `panic` 状态到当前 Goroutine 的元数据结构（即 `G` 结构体的 `_panic` 字段）。

2. 栈展开

   （Stack Unwinding）会沿着当前 Goroutine 的调用栈逐层执行 

   ```
   defer
   ```

   ，直到：

   - 找到一个 `recover`，中止展开；
   - 或者到达栈底，程序崩溃。

**G 结构体中的 panic 状态：**

```go
// runtime/runtime2.go
type g struct {
	_panic *_panic // 当前 Goroutine 正在处理的 panic 链表
	_defer *_defer // 当前 Goroutine 注册的 defer 链表
	...
}
```

- `panic` 和 `defer` 链表在栈展开过程中被逐层处理。
- Goroutine 的栈是动态的，`panic` 展开的范围只限于当前 Goroutine 的栈。

------

##### **2.3 多 Goroutine 下的 panic 和调度**

Goroutine 的 `panic` 行为与调度器密切相关：

- **单个 Goroutine 内部：**
  - `panic` 的栈展开过程完全由当前 Goroutine 的栈管理。
  - `defer` 链表按 LIFO（后进先出）顺序执行。
- **跨 Goroutine：**
  - `panic` 不会自动传播到其他 Goroutine。跨 Goroutine 的异常处理需要显式的通信（如 `channel`）。
  - Goroutine 的调度是抢占式的，因此即使一个 Goroutine 崩溃了，运行时仍会调度其他 Goroutine 执行。

**示例：多个 Goroutine 的 panic：**

```go
package main

import (
	"fmt"
	"time"
)

func worker(id int) {
	defer func() {
		if r := recover(); r != nil {
			fmt.Printf("Worker %d recovered from panic: %v\n", id, r)
		}
	}()
	if id == 2 {
		panic(fmt.Sprintf("Panic in worker %d", id))
	}
	fmt.Printf("Worker %d finished successfully\n", id)
}

func main() {
	for i := 1; i <= 3; i++ {
		go worker(i)
	}
	time.Sleep(time.Second) // 等待所有 Goroutine 完成
}
```

输出：

```
Worker 1 finished successfully
Worker 3 finished successfully
Worker 2 recovered from panic: Panic in worker 2
```

**分析：**

- 每个 Goroutine 的 `panic` 互不影响，由 GMP 模型隔离。
- Goroutine 崩溃时，调度器会继续调度其他 Goroutine。

------

##### **2.4 GMP 对 Goroutine 中 panic/recover 的优化**

1. **栈的动态增长：**
   - Goroutine 的栈初始仅 2KB，支持动态扩展（最多 1GB）。
   - 在 `panic` 展开时，如果栈需要扩展，运行时会处理栈拷贝，保证展开正常进行。
2. **延迟的垃圾回收：**
   - 当 Goroutine 由于 `panic` 崩溃，栈上的资源不会立即释放。
   - 运行时会延迟回收崩溃 Goroutine 的栈，确保调试工具可以打印完整的崩溃信息。
3. **并发安全：**
   - Go 的调度器和 GMP 模型保证了 `panic` 展开的并发安全性。
   - 即使有多个 Goroutine 同时发生 `panic`，运行时也能正确处理每个 Goroutine 的栈展开。

------

#### GMP 中的运行时实现

核心函数：

- `runtime.gopanic`：触发 panic，保存状态，执行栈展开。
- `runtime.gorecover`：从 `_panic` 链表中捕获 panic 状态。
- `runtime.gosched`：调度器继续执行其他 Goroutine。

核心流程：

1. **panic：**
   - 运行时调用 `gopanic`，将 panic 状态挂到当前 Goroutine 的 `_panic` 链表。
   - 触发栈展开，逐层执行 `defer`。
2. **recover：**
   - `recover` 通过 `runtime.gorecover` 检查当前 Goroutine 的 `_panic` 状态。
   - 如果有未处理的 panic，返回其值，并终止栈展开。
3. **调度：**
   - 崩溃 Goroutine 的栈在调度器中标记为不可用。
   - 调度器会重新分配线程资源给其他活跃 Goroutine。

------



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



##### 底层实现

`sync.WaitGroup` 是 Go 语言标准库中的一个用于管理并发任务的同步原语。它的主要功能是等待一组 goroutine 完成执行，从而可以控制并发任务的生命周期。理解其原理有助于在编写并发程序时更好地使用它，并避免常见的陷阱。

###### 1. **WaitGroup 的核心概念**

`WaitGroup` 主要通过以下三个方法来实现并发控制：

- **`Add(delta int)`**：添加（或减少）等待的 goroutine 数量。`delta` 可以是正数或负数，用来增加或减少计数值。
- **`Done()`**：减少等待的 goroutine 数量（相当于 `Add(-1)`）。一般在每个 goroutine 完成时调用。
- **`Wait()`**：阻塞当前 goroutine，直到 `WaitGroup` 的计数值变为 0。通常用于主 goroutine 等待其他 goroutine 完成。

###### 2. **WaitGroup 的数据结构**

`sync.WaitGroup` 的核心数据结构如下（省略了一些实现细节）：

```go
type WaitGroup struct {
    noCopy noCopy // 内部防止复制使用
    state1 [12]byte // 保存计数和状态信息
}
```

这里 `state1` 是一个长度为 12 字节的数组，它被分成 3 个部分：

1. **Counter**（8 字节）：用于保存当前的计数值，表示等待的 goroutine 数量。
2. **Waiter**（4 字节）：用于保存等待 `Wait` 的 goroutine 数量。
3. **Semaphore**：内部信号量，用于协调 goroutine 的阻塞和唤醒。

Go 的 `WaitGroup` 使用了 `atomic` 操作来确保在多核环境下的计数操作是原子的，从而避免竞争条件。

###### 3. **WaitGroup 的内部原理**

以下是 `WaitGroup` 各个方法的详细原理分析：

**3.1 Add 方法**

```go
func (wg *WaitGroup) Add(delta int) {
    statep, semap := wg.state()
    state := atomic.AddUint64(statep, uint64(delta)<<32) // 将 delta 添加到计数器中
    if int32(state>>32) < 0 {
        panic("sync: negative WaitGroup counter") // 防止负计数器的出现
    }
    if int32(state>>32) == 0 && int32(state) > 0 {
        runtime_Semrelease(semap, false, 0) // 当计数值为 0 时，释放信号量，唤醒等待的 goroutine
    }
}
```

- **`atomic.AddUint64(statep, uint64(delta)<<32)`**：通过原子操作更新 `Counter` 的值，确保并发环境下的安全性。
- **状态检查**：如果 `Counter` 变为 0 并且存在 `Wait()` 的 goroutine，则释放信号量（`runtime_Semrelease`），以唤醒阻塞的 `Wait()`。

**3.2 Done 方法**

`Done()` 是 `Add(-1)` 的简写，当 goroutine 完成时调用，以表示当前 goroutine 已执行完毕，减少 `WaitGroup` 的计数。

```go
func (wg *WaitGroup) Done() {
    wg.Add(-1)
}
```

因此，`Done` 的内部逻辑与 `Add(-1)` 完全相同。

**3.3 Wait 方法**

`Wait()` 会阻塞当前 goroutine，直到 `Counter` 变为 0。实现原理如下：

```go
func (wg *WaitGroup) Wait() {
    statep, semap := wg.state()
    for {
        state := atomic.LoadUint64(statep)
        if int32(state>>32) == 0 { // 如果计数值为 0，则表示所有 goroutine 都已完成，直接返回
            return
        }
        runtime_Semacquire(semap) // 阻塞等待其他 goroutine 完成
    }
}
```

- **`atomic.LoadUint64(statep)`**：读取当前 `WaitGroup` 的状态。
- 如果 `Counter` 为 0，则所有 goroutine 都已完成，`Wait()` 直接返回。
- 否则，通过信号量（`runtime_Semacquire`）阻塞当前 goroutine，等待其他 goroutine 完成并调用 `Done()`。

###### 4. **WaitGroup 的工作流程图**

1. **主 goroutine 调用 `wg.Add(n)`**：将等待的 goroutine 数量增加到 n。
2. **启动 n 个子 goroutine**：每个子 goroutine 执行完毕后调用 `wg.Done()`，递减计数。
3. **主 goroutine 调用 `wg.Wait()`**：等待所有子 goroutine 完成。
4. **当计数值变为 0 时**：`wg.Wait()` 中的阻塞解除，主 goroutine 继续执行。

###### 5. **WaitGroup 的使用场景**

`WaitGroup` 主要用于以下场景：

1. **等待多个 goroutine 完成**：如并发任务处理、批量爬虫、批量数据库查询等。
2. **管理 goroutine 生命周期**：避免程序提前退出，确保主 goroutine 在子 goroutine 完成前不会结束。

###### 6. **注意事项与常见错误**

1. **计数为负的错误**：`WaitGroup` 计数不允许为负值。当调用 `Add` 时，如果 `delta` 过大或 `Done` 调用次数多于 `Add`，会导致负计数，触发 `panic`。
  
   ```go
   var wg sync.WaitGroup
   wg.Add(1)
   wg.Done()
   wg.Done() // 会引发 panic: sync: negative WaitGroup counter
   ```

2. **重复调用 `Wait` 的问题**：在 `WaitGroup` 计数还未归零时，不要重复调用 `Wait()`，否则会引起程序行为不一致。

3. **确保 `Add` 调用在 goroutine 启动前**：如果在 goroutine 启动后才调用 `Add`，可能导致主 goroutine 提前进入 `Wait()` 状态，无法正确等待新启动的 goroutine。

   ```go
   var wg sync.WaitGroup
   wg.Add(1)
   go func() {
       defer wg.Done()
       // do some work
   }()
   wg.Wait() // 正常流程
   ```

4. **`WaitGroup` 不能复制使用**：如果 `WaitGroup` 被复制，则内部状态会丢失，导致同步逻辑错误。

   ```go
   var wg sync.WaitGroup
   wg.Add(1)
   wgCopy := wg  // 复制 WaitGroup
   go func() {
       defer wgCopy.Done() // 不会影响原始 wg 的状态
   }()
   wg.Wait() // 可能会导致永久阻塞
   ```

###### 7. **WaitGroup 与其他并发原语的对比**

- **`sync.Mutex` 和 `sync.RWMutex`**：主要用于数据共享的锁定，而 `WaitGroup` 更适合用于等待一组任务完成。
- **`channel`**：也可以用于等待多个任务完成，但 `WaitGroup` 使用更简洁，性能上开销较小。

通过理解 `WaitGroup` 的原理，可以更有效地编写并发程序，避免一些常见的坑。如果在使用中遇到特定问题或有更深入的需求，可以进一步讨论。



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

##### 区别与选择

- **错误处理**：`sync.WaitGroup` 不提供错误处理机制，需要手动实现。如果需要处理并发任务中的错误，`errgroup` 是更好的选择。
- **取消机制**：`errgroup` 结合 `context.Context`，提供了任务取消和超时控制机制，而 `sync.WaitGroup` 仅负责等待所有任务完成。
- **简单性**：如果不需要错误处理和取消机制，`sync.WaitGroup` 更加简单直接。



#### 多协程控制

**`sync.WaitGroup`**：适合简单等待多个协程完成。

**`context`**：适合协程超时、取消控制。

**`channel` 和 `select`**：适合数据传递、并发事件监听。

**`errgroup`**：适合需要并发处理并捕获错误的场景。

**`Fan-in/Fan-out`**：适合并发任务分发和结果汇总的场景。



#### 协程池

##### 1. **节省资源，控制并发数量**
   - 在高并发场景中，如果直接启动大量协程，可能会导致内存资源和系统调度资源的过度消耗。协程池可以限制并发数量，确保同时运行的协程数控制在合理范围内，从而避免了因创建过多协程导致的性能瓶颈和资源消耗。
   - 通过限制并发数量，可以有效地降低程序的内存占用和 CPU 负载。

##### 2. **提升任务处理效率**
   - 协程池允许协程复用，不必每次处理任务都创建和销毁协程。相较于频繁创建新协程，协程复用减少了协程启动的开销，显著提升了处理效率。
   - 对于大量小任务，协程池可以高效管理和调度任务，缩短任务执行时间。

##### 4. **便于监控和管理**
   - 使用协程池时，协程数是确定的，便于开发者在代码中对协程的状态进行监控，便于错误追踪、性能调优和资源管理。
   - 协程池可以集成日志系统，记录每个任务的状态、执行时间和结果，为后期调试和优化提供依据。

##### 5. **更易实现超时和重试机制**
   - 在协程池中，可以方便地实现任务的超时控制和失败重试机制。例如，如果某个任务在规定时间内未完成，协程池可以中止该任务，确保其他任务不受影响。
   - 当任务执行失败时，协程池可以重试该任务或将其记录下来以便后续处理，提高任务的执行成功率。

##### 6. **代码可读性和维护性**
   - 协程池将任务的并发管理封装在一个结构中，代码的结构更加清晰。相较于手动管理大量协程和任务的状态，协程池的方式让代码更易读，后续维护性也更高。
   - 可以通过设定协程池的大小和管理方式，灵活调整系统的并发策略，适应不同的业务需求。

##### 使用场景

协程池非常适合需要高并发但不希望无限制地创建协程的场景，典型的应用包括：

- **批量数据处理**：需要处理大量数据的批量操作，如日志解析、数据转换等。
- **网络请求并发处理**：如 API 调用、Web 爬虫、消息推送等高并发请求处理。
- **文件处理**：批量的文件上传、下载或处理场景，协程池可以控制文件操作的并发数，防止占用过多资源。

在 Go 中使用协程池可以在保持高性能的前提下有效控制资源消耗，是优化并发处理和提升系统性能的有效工具。



------



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

**指针**可以被 JSON 序列化，但实际上会序列化为其指向的值。如果指针为 nil，则会序列化为 JSON 的 null；如果指向的数据类型可序列化，则序列化结果就是该值的 JSON 表示。

- **序列化是将数据结构转换（结构体）为一种序列化格式（例如JSON、XML等），以便于传输或存储。**
- 反序列化是将数据从一种序列化格式（例如JSON、XML等）转换回原始数据的过程。


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



## 控制语句



### select 

select 的底层实现依赖于编译器将其转译为对 runtime.selectgo 的调用，然后由运行时利用内部的数据结构和调度机制实现对多个 channel 操作的竞争和阻塞/唤醒，确保多路复用操作的正确和公平执行。大致流程如下：

1. **编译转化**
    编译器会为一个 select 语句构造一个内部的数据结构（通常是一个包含所有 case 信息的数组，每个 case 会记录是发送还是接收、关联的 channel、目标变量的地址等）。这个数组在运行时会传给 runtime.selectgo 函数。
2. **运行时选择**
    runtime.selectgo 会遍历所有 case：
   - 如果有多个 case 同时满足条件（例如有可用数据的接收或可以发送的 case），它会随机选择一个（以防止饥饿）。
   - 如果没有 case 立即可用，运行时就会将当前 goroutine 挂起，并把它放到各个 channel 的等待队列中。一旦某个 channel 状态改变（例如新数据到达或发送成功），挂起的 goroutine 会被唤醒继续执行 select 后面的逻辑。
3. **阻塞与唤醒机制**
    select 的实现利用了 channel 内部的同步原语和调度器机制。当没有 case 满足时，当前 goroutine 被放入等待队列；一旦相关 channel 变为就绪状态，调度器会选择合适的等待 case 唤醒该 goroutine。
4. **底层数据结构**
    Go 的 runtime（在 src/runtime/select.go 中）定义了一个 select 描述符结构体（selectgo）和每个 case 的结构（scase）。这些结构保存了所有必要信息，用于实现随机化选择、同步操作和超时处理。



### defer

#### 特征

- **参数预计算**：defer 注册时，传入被延迟函数的所有参数会被立即求值并复制
- **LIFO 执行顺序**：编译器都会将其注册的信息（包括调用的函数指针、参数、调用点的栈指针和程序计数器等）记录到当前 goroutine 的 _defer 链表（内部实现为一个单链表）中。当函数返回时，运行时会从链表头依次取出并执行这些延迟函数，从而保证了后注册的 defer 会先执行。
- **多模式实现**：根据条件不同，defer 信息可能分配在堆上、栈上或通过开放编码直接展开，目的是尽可能降低运行时开销。
- **返回过程**：return 操作先设置返回值，再执行所有 defer 调用，最后执行 ret 指令返回。

这些设计体现了 Go 语言在编译期尽量将能确定的事情提前计算、降低运行时成本的思路，同时保证了 defer 语义（如参数预计算和 LIFO 执行）的正确性。



####  编译器和运行时的协作实现

defer 的实现有三种模式：

- **堆上分配**：最早的实现，编译器将每个 defer 转换为对运行时函数 `runtime.deferproc` 的调用，它会在堆上分配一个 `_defer` 结构体（记录调用信息）并链接到当前 goroutine 的 defer 链表中；在函数返回前，运行时通过 `runtime.deferreturn` 遍历链表，依次调用注册的函数。
- **栈上分配**：Go 1.13 引入优化，在某些条件下（比如 defer 不在循环中且不会逃逸）defer 的信息可以在调用函数的栈上分配，避免了堆分配带来的额外开销，从而提升性能。
- **开放编码（Open Coded Defer）**：Go 1.14 进一步优化，在满足一定条件（如 defer 数量较少、没有循环、返回值与 defer 数量乘积较小等）时，编译器会将 defer 的调用直接内联展开，生成专门的延迟比特（defer bit）用于在函数退出前判断并调用相应的延迟函数。这种方式可以大幅降低 defer 的运行时开销（甚至接近零成本）。

------

#### 函数返回过程与 defer 的执行顺序

需要注意的是，函数的 return 语句并非一个原子操作，其执行过程大致为：

1. 计算并保存返回值（如果有具名返回值，则为一个局部变量）；
2. 执行所有注册的 defer 语句（按后进先出顺序）；
3. 最后执行实际的 ret 指令返回。

因此，在具名返回值的情况下，defer 注册的匿名函数可以修改返回值，从而影响最终的返回结果。



------



## 数据类型

### 字节数

Go语言的基本数据类型有多种，不同的数据类型占用的字节数也各不相同。以下是常见数据类型及其所占字节数的详细信息：

#### 1. 布尔类型
- `bool`：占用1个字节，值为 `true` 或 `false`。

#### 2. 整型类型
- `int` 和 `uint`：
  - 占用字节数依赖于系统架构（32位系统占4字节，64位系统占8字节）。
  
- 定长整型：
  - `int8`：1字节，范围：`-128` 到 `127`
  - `int16`：2字节，范围：`-32768` 到 `32767`
  - `int32`：4字节，范围：`-2147483648` 到 `2147483647`
  - `int64`：8字节，范围：`-9223372036854775808` 到 `9223372036854775807`
  - `uint8`：1字节，范围：`0` 到 `255`（即 `byte` 类型）
  - `uint16`：2字节，范围：`0` 到 `65535`
  - `uint32`：4字节，范围：`0` 到 `4294967295`
  - `uint64`：8字节，范围：`0` 到 `18446744073709551615`

#### 3. 浮点数类型
- `float32`：4字节，范围：约为 `1.18E-38` 到 `3.4E38`，精度为7位十进制数。
- `float64`：8字节，范围：约为 `2.23E-308` 到 `1.8E308`，精度为15位十进制数。

#### 4. 复数类型
- `complex64`：8字节（实部和虚部分别为 `float32`）
- `complex128`：16字节（实部和虚部分别为 `float64`）

#### 5. 字符串类型
- `string`：字符串在Go中是不可变的，底层结构包含一个指向字节数组的指针和一个长度值。占用的内存是字符串的长度（每个字符1字节）加上一个指针和长度字段（在64位系统上共16字节）。

#### 6. 字符类型
- `byte`：1字节，相当于 `uint8`。
- `rune`：4字节，相当于 `int32`，用于表示单个Unicode字符。

#### 7. 指针类型
- `*T`：指针类型在64位系统中占8字节，32位系统中占4字节。

#### 8. 切片（Slice）、映射（Map）、接口（Interface）
这些类型的大小取决于其内部结构：
- `slice`：底层数据结构占用24字节（包含指针、长度和容量）。
- `map`：占用的字节数依赖于存储的数据，但基本结构的开销是约8字节。
- `interface`：占用16字节（包含类型信息和指向数据的指针）。

这些是Go语言中最常见的基础数据类型及其占用的字节数。不同系统架构下可能会有略微不同的表现。



### string

`string` 类型表示的是一段 **UTF-8 编码的字节序列**，不可变。具体来说，字符串一旦被创建，内容不能被修改。

#### 特性
1. **不可变性**：Go 的字符串是只读的，也就是说字符串内容不能被改变。如果需要对字符串进行修改，需要创建一个新的字符串。
   
2. **底层结构**：字符串本质上是一个字节数组（`[]byte`），其内部包含两个字段：
   - **指向底层字节数组的指针**。
   - **字符串的长度**。

#### 内存占用
字符串占用的内存包括：
- **每个字符1个字节**（对于ASCII字符），即字符串中的字符数。
- **字符串的指针和长度**字段，通常占用16个字节（在64位系统上）。

#### 示例
```go
package main

import "fmt"

func main() {
    var s string = "Hello, 世界"
    fmt.Println(s)          // 输出: Hello, 世界
    fmt.Println(len(s))     // 输出字符串的长度（以字节为单位），结果为13，因为'世'和'界'是多字节字符
}
```

#### 其他操作
- **字符串长度**：使用 `len(s)` 获取字符串的字节长度。
- **字符串索引**：可以通过索引访问字符串中的字节（返回的是 `byte` 类型），但无法直接修改它。
- **字符串遍历**：可以使用 `for range` 循环逐字符遍历字符串。

遍历字符串

```go
for i, ch := range s {
    fmt.Printf("字符 %c 在位置 %d\n", ch, i)
}
```
该代码会按字符遍历字符串，并处理多字节字符。

#### 字符串的转换
- 将字符串转换为 `[]byte`（字节数组）：`[]byte(s)`
- 将 `[]byte` 转换为字符串：`string(b)`

总的来说，Go 中的 `string` 是一种高效的、不可变的字节序列，用于表示文本数据。



### map

#### 初始化方法

golang 中，对 map 的初始化分为以下几种方式：

```
myMap1 := make(map[int]int,2)
```

通过 make 关键字进行初始化，同时指定 map 预分配的容量.

```
myMap2 := make(map[int]int)
```

通过 make 关键字进行初始化，不显式声明容量，因此默认容量 为 0.

```
myMap3 :=map[int]int{
  1:2,
  3:4,
}
```

初始化操作连带赋值

#### key 的类型要求

map 中，key 的数据类型必须为可比较的类型，chan、map、func不可比较

 

#### 1.3 读

读 map 分为下面两种方式：

```
v1 := myMap[10]
```

第一种方式是直接读，倘若 key 存在，则获取到对应的 val，倘若 key 不存在或者 map 未初始化，会返回 val 类型的零值作为兜底.

```
v2,ok := myMap[10]
```

第二种方式是读的同时添加一个 bool 类型的 flag 标识是否读取成功. 倘若 ok == false，说明读取失败， key 不存在，或者 map 未初始化.

此处同一种语法能够实现不同返回值类型的适配，是由于代码在汇编时，会根据返回参数类型的区别，映射到不同的实现方法.

 

#### 1.4 写

```
myMap[5] = 6
```

写操作的语法如上. 须注意的一点是，倘若 map 未初始化，直接执行写操作会导致 panic：

```
const plainError string
panic(plainError("assignment to entry in nil map"))
```

 

#### 1.5 删

```
delete(myMap,5)
```

执行 delete 方法时，倘若 key 存在，则会从 map 中将对应的 key-value 对删除；倘若 key 不存在或 map 未初始化，则方法直接结束，不会产生显式提示.

 

#### 1.6 遍历

遍历分为下面两种方式：

```
for k,v := range myMap{
  // ...
}
```

基于 k,v 依次承接 map 中的 key-value 对；

```
for k := range myMap{
  // ...
}
```

基于 k 依次承接 map 中的 key，不关注 val 的取值.

需要注意的是，在执行 map 遍历操作时，获取的 key-value 对并没有一个固定的顺序，因此前后两次遍历顺序可能存在差异.

 

#### 1.7 并发冲突

map 不是并发安全的数据结构，倘若存在并发读写行为，会抛出 fatal error.

具体规则是：

（1）并发读没有问题；

（2）并发读写中的“写”是广义上的，包含写入、更新、删除等操作；

（3）读的时候发现其他 goroutine 在并发写，抛出 fatal error；

（4）写的时候发现其他 goroutine 在并发写，抛出 fatal error.

 

```
fatal("concurrent map read and map write")
fatal("concurrent map writes")
```

需要关注，此处并发读写会引发 fatal error，是一种比 panic 更严重的错误，无法使用 recover 操作捕获.



### sync.Map

`sync.Map` 是 Go 语言中提供的一个并发安全的 map（字典），它位于 `sync` 包中。`sync.Map` 允许多个 goroutine 同时对 map 进行读写操作，而无需手动管理锁机制。

- • sync.Map 适用于**读多、更新多、删多、写少**的场景；
- • 倘若写操作过多，sync.Map 基本等价于互斥锁 + map；
- • sync.Map 可能存在性能抖动问题，主要发生于在读/删流程 miss 只读 map 次数过多时（触发 missLocked 流程），下一次插入操作的过程当中（dirtyLocked 流程）.



1. **并发安全性**：`sync.Map` 能够安全地在多个 goroutine 中并发读写，不需要额外的锁机制。它内部实现了一种无锁（lock-free）的并发算法，使得对其的读操作是无锁的，写操作也能在很多情况下避免锁竞争，提高了并发性能。

2. **无需初始化**：`sync.Map` 不需要初始化，直接声明即可使用。

3. **动态键值对**：与普通的 `map` 不同，`sync.Map` 的键和值可以是**任意类型**的数据，不需要预先声明或指定类型。

4. **Load、Store 和 Delete 操作**：通过 `Load` 方法可以安全地从 `sync.Map` 中加载键对应的值，`Store` 方法可以安全地存储键值对，`Delete` 方法可以安全地删除键。

5. **`LoadOrStore(key, value)`**：如果键存在，则返回键对应的值；如果键不存在，则存储该键值对，并返回传入的值和 `false`。

6. **Range 迭代**：在遍历的过程中仍然允许其他操作，不会出现并发冲突。

   **`Range(f func(key, value interface{}) bool)`**：遍历所有的键值对，直到遍历完成或函数 `f` 返回 `false`。



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





### slice

#### 用法

1.var初始化或者通过内置函数 **make()** 初始化切片**s**

```Go
var numbers []int
s :=make([]int,len,cap) 
 // 声明一个字符串指针的切片    listOfNumberStrings := []*string{}

```

2.直接初始化切片，**[]** 表示是切片类型，**{1,2,3}** 初始化值依次是 **1,2,3**，其 **cap=len=3**。

```Go
s :=[]int {1,2,3 } 

var balance = [...]float32{1000.0, 2.0, 3.4, 7.0, 50.0}
或
balance := [...]float32{1000.0, 2.0, 3.4, 7.0, 50.0}

//  将索引为 1 和 3 的元素初始化
balance := [5]float32{1:2.0,3:7.0}
```

3.初始化切片 **s**，是数组 arr 的引用

```Go
s := arr[:] 
```

将 arr 中从下标 startIndex 到 endIndex-1 下的元素创建为一个新的切片 s := arr[startIndex:endIndex]



在 Go 中，切片支持三索引语法：`s[low:high:max]`。这表示：

- **low**：起始索引（包含该索引）。
- **high**：结束索引（不包含该索引）。
- **max**：切片的最大容量上界，结果切片的容量为 `max - low`。

例如，对于表达式：

```
slice[1:2:6]
```

假设 `slice` 至少有 6 个元素，则返回的切片包含：

- **元素**：从下标 1 到 2（不包含 2），即只有 `slice[1]` 一个元素。
- **长度**：`2 - 1 = 1`
- **容量**：`6 - 1 = 5`

这种语法允许我们精确控制新切片的容量，有助于限制对底层数组的引用范围，从而避免不必要的内存占用或修改底层数据。



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

#### 坑

sliec函数传递时，进行了扩容操作，地址改变，那么函数内部的操作可能会失效

如果函数内部持有原 slice 底层数组地址的引用（例如缓存了某个元素的地址），那么这些引用在扩容后就“失效”了，因为此时它们仍然指向旧数组，而新的操作则在新数组上进行。

这种情况正是因为 slice 扩容的“非原地”特性所导致的。所以在函数内部操作 slice 时，应始终使用 append 的返回值，而不要假设原 slice 会自动更新，从而避免因底层数组地址变化而导致的逻辑错误。

------



### channel

**基于通信实现共享内存**

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

`ok` 为`false`：**读已关闭且没有数据**通道

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

------

### select





------



## 数据结构

源码的函数名和代码使用时的名称不一样，是通过编译来连接到一起的

### map

#### 介绍

map 又称字典，是一种常用的数据结构，核心特征包含下述三点：

（1）存储基于 key-value 对映射的模式；

（2）基于 key 维度实现存储数据的去重；

（3）读、写、删操作控制，时间复杂度 O(1).



 

#### 2 核心原理

map 又称为 hash map，在算法上基于 hash 实现 key 的映射和寻址；在数据结构上基于桶数组实现 key-value 对的存储.

以一组 key-value 对写入 map 的流程为例进行简述：

（1）通过哈希方法取得 key 的 hash 值；

（2）hash 值对桶数组长度取模，确定其所属的桶；

（3）在桶中插入 key-value 对.

 

hash 的性质，保证了相同的 key 必然产生相同的 hash 值，因此能映射到相同的桶中，通过桶内遍历的方式锁定对应的 key-value 对.

因此，只要在宏观流程上，**控制每个桶中 key-value 对的数量**，就能保证 map 的几项操作都限制为常数级别的时间复杂度.

<img src="https://raw.githubusercontent.com/lanyoumeng/Drawing-bed/main/docs/202409052327764.webp" alt="640 (9)" style="zoom:50%;" />



##### 2.2 桶数组

map 中，会通过长度为 2 的整数次幂的桶数组进行 key-value 对的存储：

（1）每个桶固定可以存放 8 个 key-value 对；

（2）倘若超过 8 个 key-value 对打到桶数组的同一个索引当中，此时会通过创建桶链表的方式来化解这一问题.

 <img src="https://raw.githubusercontent.com/lanyoumeng/Drawing-bed/main/docs/202409052328097.webp" alt="640 (10)" style="zoom:50%;" />



##### 2.3 拉链法解决 hash 冲突

在 map 解决 hash /分桶 冲突问题时，实际上结合了拉链法和开放寻址法两种思路. 以 map 的插入写流程为例，进行思路阐述：

（1）桶数组中的每个桶，严格意义上是一个单向桶链表，以桶为节点进行串联；

（2）每个桶固定可以存放 8 个 key-value 对；

（3）当 key 命中一个桶时，首先根据开放寻址法，在桶的 8 个位置中寻找空位进行插入；

（4）倘若桶的 8 个位置都已被占满，则基于桶的溢出桶指针，找到下一个桶，重复第（3）步；

（5）倘若遍历到链表尾部，仍未找到空位，则基于拉链法，在桶链表尾部续接新桶，并插入 key-value 对.

<img src="https://raw.githubusercontent.com/lanyoumeng/Drawing-bed/main/docs/202409052329609.webp" alt="640 (13)" style="zoom:50%;" />



对标拉链还有开放寻址法，两者的优劣对比：

| **方法**   | **优点**                                                     |
| ---------- | ------------------------------------------------------------ |
| 拉链法     | 简单常用；无需预先为元素分配内存.                            |
| 开放寻址法 | 无需额外的指针用于链接元素；内存地址完全连续，可以基于局部性原理，充分利用 CPU 高速缓存. |

 



##### 2.4 扩容优化性能

倘若 map 的桶数组长度固定不变，那么随着 key-value 对数量的增长，当一个桶下挂载的 key-value 达到一定的量级，此时操作的时间复杂度会趋于线性，无法满足诉求.

因此在实现上，map 桶数组的长度会随着 key-value 对数量的变化而实时调整，以保证每个桶内的 key-value 对数量始终控制在常量级别，满足各项操作为 O(1) 时间复杂度的要求.

map 扩容机制的核心点包括：

（1）扩容分为**增量扩容**和等量扩容；

（2）当桶内 key-value 总数/桶数组长度 > 6.5 时发生增量扩容，桶数组长度**增长为原值的两倍**；

（3）当桶内溢出桶数量大于等于 2^B 时( B 为桶数组长度的指数，B 最大取 15)，发生等量扩容，松散的键值对重新排列一次，桶的长度**保持为原值**；

（4）采用渐进扩容的方式，当桶被实际操作到时，由使用者负责完成数据迁移，避免因为一次性的全量数据迁移引发性能抖动.

<img src="https://raw.githubusercontent.com/lanyoumeng/Drawing-bed/main/docs/202409052330897.webp" alt="640 (14)" style="zoom:50%;" />

#### 3 数据结构

##### 3.1 hmap

<img src="https://raw.githubusercontent.com/lanyoumeng/Drawing-bed/main/docs/202409052330440.webp" alt="640 (15)" style="zoom:67%;" />

```
type hmap struct {
    count     int 
    flags     uint8
    B         uint8  
    noverflow uint16 
    hash0     uint32 
    buckets    unsafe.Pointer 
    oldbuckets unsafe.Pointer 
    nevacuate  uintptr       
    extra *mapextra 
}
```

（1）count：map 中的 key-value 总数；

（2）flags：map 状态标识，可以标识出 map 是否被 goroutine 并发读写；

（3）B：桶数组长度的指数，桶数组长度为 2^B；

（4）noverflow：map 中溢出桶的数量；

（5）hash0：hash 随机因子，生成 key 的 hash 值时会使用到；

（6）buckets：桶数组；

（7）oldbuckets：扩容过程中老的桶数组；

（8）nevacuate：扩容时的进度标识，index 小于 nevacuate 的桶都已经由老桶转移到新桶中；

（9）extra：预申请的溢出桶.

 

##### 3.2 mapextra

<img src="https://raw.githubusercontent.com/lanyoumeng/Drawing-bed/main/docs/202409052331347.webp" alt="640 (16)" style="zoom:50%;" />

```
type mapextra struct {
    overflow    *[]*bmap
    oldoverflow *[]*bmap


    nextOverflow *bmap
}
```

在 map 初始化时，倘若容量过大，会提前申请好一批溢出桶，以供后续使用，这部分溢出桶存放在 hmap.mapextra 当中：

（1）mapextra.overflow：供桶数组 buckets 使用的溢出桶；

（2）mapextra.oldoverFlow: 扩容流程中，供老桶数组 oldBuckets 使用的溢出桶；

（3）mapextra.nextOverflow：下一个可用的溢出桶.

 

##### 3.3 bmap

<img src="https://raw.githubusercontent.com/lanyoumeng/Drawing-bed/main/docs/202409052331101.webp" alt="640 (17)" style="zoom:50%;" />

```
const bucketCnt = 8
type bmap struct {
    tophash [bucketCnt]uint8
}
```

（1）bmap 就是 map 中的桶，可以存储 8 组 key-value 对的数据，以及一个指向下一个溢出桶的指针；

（2）每组 key-value 对数据包含 key 高 8 位 hash 值 tophash，key 和 val 三部分；

（3）在代码层面只展示了 tophash 部分，但由于 tophash、key 和 val 的数据长度固定，因此可以通过内存地址偏移的方式寻找到后续的 key 数组、val 数组以及溢出桶指针；

（4）为方便理解，把完整的 bmap 类声明代码补充如下：

```
type bmap struct {
    tophash [bucketCnt]uint8
    keys [bucketCnt]T
    values [bucketCnt]T
    overflow uint8
}
```



#### 4 构造方法

<img src="https://raw.githubusercontent.com/lanyoumeng/Drawing-bed/main/docs/202409052331873.webp" alt="640 (18)" style="zoom:67%;" />

创建 map 时，实际上会调用 runtime/map.go 文件中的 makemap 方法，下面对源码展开分析：

##### 4.1 makemap

方法主干源码一览：

```
func makemap(t *maptype, hint int, h *hmap) *hmap {
    mem, overflow := math.MulUintptr(uintptr(hint), t.bucket.size)
    if overflow || mem > maxAlloc {
        hint = 0
    }


    if h == nil {
        h = new(hmap)
    }
    h.hash0 = fastrand()


    B := uint8(0)
    for overLoadFactor(hint, B) {
        B++
    }
    h.B = B


    if h.B != 0 {
        var nextOverflow *bmap
        h.buckets, nextOverflow = makeBucketArray(t, h.B, nil)
        if nextOverflow != nil {
            h.extra = new(mapextra)
            h.extra.nextOverflow = nextOverflow
        }
    }


    return 
```

 

（1）hint 为 map 拟分配的容量；在分配前，会提前对拟分配的内存大小进行判断，倘若超限，会将 hint 置为零；

```
mem, overflow := math.MulUintptr(uintptr(hint), t.bucket.size)
if overflow || mem > maxAlloc {
   hint = 0
}
```

 

（2）通过 new 方法初始化 hmap；

```
if h == nil {
   h = new(hmap)
}
```

 

（3）调用 fastrand，构造 hash 因子：hmap.hash0；

```
h.hash0 = fastrand()
```

 

（4）大致上基于 log2(B) >= hint 的思路（具体见 4.2 小节 overLoadFactor 方法的介绍），计算桶数组的容量 B；

```
B := uint8(0)
for overLoadFactor(hint, B) {
    B++
}
h.B =
```

 

（5）调用 makeBucketArray 方法，初始化桶数组 hmap.buckets；

```
var nextOverflow *bmap
h.buckets, nextOverflow = makeBucketArray(t, h.B, n
```

 

（6）倘若 map 容量较大，会提前申请一批溢出桶 hmap.extra.

```
if nextOverflow != nil {
   h.extra = new(mapextra)
   h.extra.nextOverflow = nextOverflow
}
```

 

##### 4.2 overLoadFactor

通过 overLoadFactor 方法，对 map 预分配容量和桶数组长度指数进行判断，决定是否仍需要增长 B 的数值：

```
const loadFactorNum = 13
const loadFactorDen = 2
const goarch.PtrSize = 8
const bucketCnt = 8


func overLoadFactor(count int, B uint8) bool {
    return count > bucketCnt && uintptr(count) > loadFactorNum*(bucketShift(B)/loadFactorDen)
}


func bucketShift(b uint8) uintptr {
    return uintptr(1) << (b & (goarch.PtrSize*8 - 1))
```

（1）倘若 map 预分配容量小于等于 8，B 取 0，桶的个数为 1；

（2）保证 map 预分配容量小于等于桶数组长度 * 6.5.

 

map 预分配容量、桶数组长度指数、桶数组长度之间的关系如下表：

| **kv 对数量**             | **桶数组长度指数 B** | **桶数组长度 2^B** |
| ------------------------- | -------------------- | ------------------ |
| 0 ~ 8                     | 0                    | 1                  |
| 9 ~ 13                    | 1                    | 2                  |
| 14 ~ 26                   | 2                    | 4                  |
| 27 ~ 52                   | 3                    | 8                  |
| 2^(B-1) * 6.5+1 ~ 2^B*6.5 | B                    | 2^B                |

 

##### 4.3 makeBucketArray

makeBucketArray 方法会进行桶数组的初始化，并根据桶的数量决定是否需要提前作溢出桶的初始化. 方法主干代码如下：

```
func makeBucketArray(t *maptype, b uint8, dirtyalloc unsafe.Pointer) (buckets unsafe.Pointer, nextOverflow *bmap) {
    base := bucketShift(b)
    nbuckets := base
    if b >= 4 {
        nbuckets += bucketShift(b - 4)
    }
    
    buckets = newarray(t.bucket, int(nbuckets))
   
    if base != nbuckets {
        nextOverflow = (*bmap)(add(buckets, base*uintptr(t.bucketsize)))
        last := (*bmap)(add(buckets, (nbuckets-1)*uintptr(t.bucketsize)))
        last.setoverflow(t, (*bmap)(buckets))
    }
    return buckets, nextOverflow
}
```

 

makeBucketArray 会为 map 的桶数组申请内存，在桶数组的指数 b >= 4时（桶数组的容量 >= 52 ），会需要提前创建溢出桶.

通过 base 记录桶数组的长度，不包含溢出桶；通过 nbuckets 记录累加上溢出桶后，桶数组的总长度.

 

```
base := bucketShift(b)
nbuckets := base
if b >= 4 {
   nbuckets += bucketShift(b - 4)
}
```

 

调用 newarray 方法为桶数组申请内存空间，连带着需要初始化的溢出桶：

```
buckets = newarray(t.bucket, int(nbuckets))
```

 

倘若 base != nbuckets，说明需要创建溢出桶，会基于地址偏移的方式，通过 nextOverflow 指向首个溢出桶的地址.

```
if base != nbuckets {
   nextOverflow = (*bmap)(add(buckets, base*uintptr(t.bucketsize)))
   last := (*bmap)(add(buckets, (nbuckets-1)*uintptr(t.bucketsize)))
   last.setoverflow(t, (*bmap)(buckets))
}
return buckets, nextOverflow
```

 

倘若需要创建溢出桶，会在将最后一个溢出桶的 overflow 指针指向 buckets 数组，以此来标识申请的溢出桶已经用完.

```
func (b *bmap) setoverflow(t *maptype, ovf *bmap) {
    *(**bmap)(add(unsafe.Pointer(b), uintptr(t.bucketsize)-goarch.PtrSize)) = ovf
}
```

#### 5 读流程

<img src="https://raw.githubusercontent.com/lanyoumeng/Drawing-bed/main/docs/202409052332260.webp" alt="640 (19)" style="zoom:67%;" />

##### 5.1 读流程梳理

map 读流程主要分为以下几步：

（1）根据 key 取 hash 值；

（2）根据 hash 值对桶数组取模，确定所在的桶；

（3）沿着桶链表依次遍历各个桶内的 key-value 对；

（4）命中相同的 key，则返回 value；倘若 key 不存在，则返回零值.

map 读操作最终会走进 runtime/map.go 的 mapaccess 方法中，下面开始阅读源码：

 

##### 5.2 mapaccess 方法源码走读

```
func mapaccess1(t *maptype, h *hmap, key unsafe.Pointer) unsafe.Pointer {
    if h == nil || h.count == 0 {
        return unsafe.Pointer(&zeroVal[0])
    }
    if h.flags&hashWriting != 0 {
        fatal("concurrent map read and map write")
    }
    hash := t.hasher(key, uintptr(h.hash0))
    m := bucketMask(h.B)
    b := (*bmap)(add(h.buckets, (hash&m)*uintptr(t.bucketsize)))
    if c := h.oldbuckets; c != nil {
        if !h.sameSizeGrow() {
            m >>= 1
        }
        oldb := (*bmap)(add(c, (hash&m)*uintptr(t.bucketsize)))
        if !evacuated(oldb) {
            b = oldb
        }
    }
    top := tophash(hash)
bucketloop:
    for ; b != nil; b = b.overflow(t) {
        for i := uintptr(0); i < bucketCnt; i++ {
            if b.tophash[i] != top {
                if b.tophash[i] == emptyRest {
                    break bucketloop
                }
                continue
            }
            k := add(unsafe.Pointer(b), dataOffset+i*uintptr(t.keysize))
            if t.indirectkey() {
                k = *((*unsafe.Pointer)(k))
            }
            if t.key.equal(key, k) {
                e := add(unsafe.Pointer(b), dataOffset+bucketCnt*uintptr(t.keysize)+i*uintptr(t.elemsize))
                if t.indirectelem() {
                    e = *((*unsafe.Pointer)(e))
                }
                return e
            }
        }
    }
    return unsafe.Pointer(&zeroVal[0])
}


func (h *hmap) sameSizeGrow() bool {
    return h.flags&sameSizeGrow != 0
}


func evacuated(b *bmap) bool {
    h := b.tophash[0]
    return h > emptyOne && h < minTopHash
}
```

 

（1）倘若 map 未初始化，或此时存在 key-value 对数量为 0，直接返回零值；

```
if h == nil || h.count == 0 {
    return unsafe.Pointer(&zeroVal[0])
}
```

 

（2）倘若发现存在其他 goroutine 在写 map，直接抛出并发读写的 fatal error；其中，并发写标记，位于 hmap.flags 的第 3 个 bit 位；

```
 const hashWriting  = 4
 
 if h.flags&hashWriting != 0 {
        fatal("concurrent map read and map write")
 }
```

 

（3）通过 maptype.hasher() 方法计算得到 key 的 hash 值，并对桶数组长度取模，取得对应的桶. 关于 hash 方法的内部实现，golang 并未暴露.

```
 hash := t.hasher(key, uintptr(h.hash0))
 m := bucketMask(h.B)
 b := (*bmap)(add(h.buckets, (hash&m)*uintptr(t.bucketsize))
```

 

其中，bucketMast 方法会根据 B 求得桶数组长度 - 1 的值，用于后续的 & 运算，实现取模的效果：

```
func bucketMask(b uint8) uintptr {
    return bucketShift(b) - 1
}
```

 

（4）在取桶时，会关注当前 map 是否处于扩容的流程，倘若是的话，需要在老的桶数组 oldBuckets 中取桶，通过 evacuated 方法判断桶数据是已迁到新桶还是仍存留在老桶，倘若仍在老桶，需要取老桶进行遍历.

```
 if c := h.oldbuckets; c != nil {
    if !h.sameSizeGrow() {
        m >>= 1
    }
    oldb := (*bmap)(add(c, (hash&m)*uintptr(t.bucketsize)))
    if !evacuated(oldb) {
        b = oldb
    }
 }
```

 

在取老桶前，会先判断 map 的扩容流程是否是增量扩容，倘若是的话，说明老桶数组的长度是新桶数组的一半，需要将桶长度值 m 除以 2.

```
const (
    sameSizeGrow = 8
)


func (h *hmap) sameSizeGrow() bool {
    return h.flags&sameSizeGrow != 0
}
```

 

取老桶时，会调用 evacuated 方法判断数据是否已经迁移到新桶. 判断的方式是，取桶中首个 tophash 值，倘若该值为 2,3,4 中的一个，都代表数据已经完成迁移.

```
const emptyOne = 1
const evacuatedX = 2
const evacuatedY = 3
const evacuatedEmpty = 4 
const minTopHash = 5


func evacuated(b *bmap) bool {
    h := b.tophash[0]
    return h > emptyOne && h < minTopHash
}
```

 

（5）取 key hash 值的高 8 位值 top. 倘若该值 < 5，会累加 5，以避开 0 ~ 4 的取值. 因为这几个值会用于枚举，具有一些特殊的含义.

```
const minTopHash = 5


top := tophash(hash)


func tophash(hash uintptr) uint8 {
    top := uint8(hash >> (goarch.PtrSize*8 - 8))
    if top < minTopHash {
        top += minTopHash
    }
    return top
```

 

（6）开启两层 for 循环进行遍历流程，外层基于桶链表，依次遍历首个桶和后续的每个溢出桶，内层依次遍历一个桶内的 key-value 对.

```
bucketloop:
for ; b != nil; b = b.overflow(t) {
    for i := uintptr(0); i < bucketCnt; i++ {
        // ...
    }
}
return unsafe.Pointer(&zeroVal[0])
```

 

内存遍历时，首先查询高 8 位的 tophash 值，看是否和 key 的 top 值匹配.

倘若不匹配且当前位置 tophash 值为 0，说明桶的后续位置都未放入过元素，当前 key 在 map 中不存在，可以直接打破循环，返回零值.

```
const emptyRest = 0
if b.tophash[i] != top {
    if b.tophash[i] == emptyRest {
          break bucketloop
    }
    continue
}
```

 

倘若找到了相等的 key，则通过地址偏移的方式取到 value 并返回.

其中 dataOffset 为一个桶中 tophash 数组所占用的空间大小.

```
if t.key.equal(key, k) {
     e := add(unsafe.Pointer(b), dataOffset+bucketCnt*uintptr(t.keysize)+i*uintptr(t.elemsize))
     return e
}
```

倘若遍历完成，仍未找到匹配的目标，返回零值兜底.

 

#### 6 写流程

![640 (20)](https://raw.githubusercontent.com/lanyoumeng/Drawing-bed/main/docs/202409052333901.webp)

##### 6.1 写流程梳理

map 写流程主要分为以下几步：

（1）根据 key 取 hash 值；

（2）根据 hash 值对桶数组取模，确定所在的桶；

（3）倘若 map 处于扩容，则迁移命中的桶，帮助推进渐进式扩容；

（4）沿着桶链表依次遍历各个桶内的 key-value 对；

（5）倘若命中相同的 key，则对 value 中进行更新；

（6）倘若 key 不存在，则插入 key-value 对；

（7）倘若发现 map 达成扩容条件，则会开启扩容模式，并重新返回第（2）步.

 

map 写操作最终会走进 runtime/map.go 的 mapassign 方法中，下面开始阅读源码：

 

##### 6.2 mapassign

```
func mapassign(t *maptype, h *hmap, key unsafe.Pointer) unsafe.Pointer {
    if h == nil {
        panic(plainError("assignment to entry in nil map"))
    }
    if h.flags&hashWriting != 0 {
        fatal("concurrent map writes")
    }
    hash := t.hasher(key, uintptr(h.hash0))


    h.flags ^= hashWriting


    if h.buckets == nil {
        h.buckets = newobject(t.bucket) 
    }


again:
    bucket := hash & bucketMask(h.B)
    if h.growing() {
        growWork(t, h, bucket)
    }
    b := (*bmap)(add(h.buckets, bucket*uintptr(t.bucketsize)))
    top := tophash(hash)


    var inserti *uint8
    var insertk unsafe.Pointer
    var elem unsafe.Pointer
bucketloop:
    for {
        for i := uintptr(0); i < bucketCnt; i++ {
            if b.tophash[i] != top {
                if isEmpty(b.tophash[i]) && inserti == nil {
                    inserti = &b.tophash[i]
                    insertk = add(unsafe.Pointer(b), dataOffset+i*uintptr(t.keysize))
                    elem = add(unsafe.Pointer(b), dataOffset+bucketCnt*uintptr(t.keysize)+i*uintptr(t.elemsize))
                }
                if b.tophash[i] == emptyRest {
                    break bucketloop
                }
                continue
            }
            k := add(unsafe.Pointer(b), dataOffset+i*uintptr(t.keysize))
            if t.indirectkey() {
                k = *((*unsafe.Pointer)(k))
            }
            if !t.key.equal(key, k) {
                continue
            }
            if t.needkeyupdate() {
                typedmemmove(t.key, k, key)
            }
            elem = add(unsafe.Pointer(b), dataOffset+bucketCnt*uintptr(t.keysize)+i*uintptr(t.elemsize))
            goto done
        }
        ovf := b.overflow(t)
        if ovf == nil {
            break
        }
        b = ovf
    }


    if !h.growing() && (overLoadFactor(h.count+1, h.B) || tooManyOverflowBuckets(h.noverflow, h.B)) {
        hashGrow(t, h)
        goto again 
    }


    if inserti == nil {
        newb := h.newoverflow(t, b)
        inserti = &newb.tophash[0]
        insertk = add(unsafe.Pointer(newb), dataOffset)
        elem = add(insertk, bucketCnt*uintptr(t.keysize))
    }


    if t.indirectkey() {
        kmem := newobject(t.key)
        *(*unsafe.Pointer)(insertk) = kmem
        insertk = kmem
    }
    if t.indirectelem() {
        vmem := newobject(t.elem)
        *(*unsafe.Pointer)(elem) = vmem
    }
    typedmemmove(t.key, insertk, key)
    *inserti = top
    h.count++




done:
    if h.flags&hashWriting == 0 {
        fatal("concurrent map writes")
    }
    h.flags &^= hashWriting
    if t.indirectelem() {
        elem = *((*unsafe.Pointer)(elem))
    }
    retur
```

 

（1）写操作时，倘若 map 未初始化，直接 panic；

```
if h == nil {
        panic(plainError("assignment to entry in nil map"))
}
```

 

（2）倘若其他 goroutine 在进行写或删操作，抛出并发写 fatal error；

```
if h.flags&hashWriting != 0 {
    fatal("concurrent map writes")
}
```

 

（3）通过 maptype.hasher() 方法求得 key 对应的 hash 值；

```
 hash := t.hasher(key, uintptr(h.hash0))
```

 

（4）通过异或位运算，将 map.flags 的第 3 个 bit 位置为 1，添加写标记；

```
h.flags ^= hashWriting
```

 

（5）倘若 map 的桶数组 buckets 未空，则对其进行初始化；

```
if h.buckets == nil {
     h.buckets = newobject(t.bucket) 
}
```

 

（6）找到当前 key 对应的桶索引 bucket；

```
bucket := hash & bucketMask(h.B)
```

 

（7）倘若发现当前 map 正处于扩容过程，则帮助其渐进扩容，具体内容在第 9 节中再作展开；

```
   if h.growing() {
        growWork(t, h, bucket)
  }
```

 

（8）从 map 的桶数组 buckets 出发，结合桶索引和桶容量大小，进行地址偏移，获得对应桶 b；

```
b := (*bmap)(add(h.buckets, bucket*uintptr(t.bucketsize)))
```

 

（9）取得 key 的高 8 位 tophash：

```
top := tophash(hash)
```

 

（10）提前声明好的三个指针，用于指向存放 key-value 的空槽:

inserti：tophash 拟插入位置；

insertk：key 拟插入位置 ；

elem：val 拟插入位置；

```
var inserti *uint8
var insertk unsafe.Pointer
var elem unsafe.Pointer
```

 

（11）开启两层 for 循环，外层沿着桶链表依次遍历，内层依次遍历桶内的 key-value 对：

```
bucketloop:
    for {
        for i := uintptr(0); i < bucketCnt; i++ {
            // ...
        }
        ovf := b.overflow(t)
        if ovf == nil {
            break
        }
        b = ovf
     }
```

 

(12）倘若 key 的 tophash 和当前位置 tophash 不同，则会尝试将 inserti、insertk elem 调整指向首个空位，用于后续的插入操作.

倘若发现当前位置 tophash 标识为 emtpyRest（0），则说明当前桶链表后续位置都未空，无需继续遍历，直接 break 遍历流程即可.

```
if b.tophash[i] != top {
      if isEmpty(b.tophash[i]) && inserti == nil {
                    inserti = &b.tophash[i]
                    insertk = add(unsafe.Pointer(b), dataOffset+i*uintptr(t.keysize))
                    elem = add(unsafe.Pointer(b), dataOffset+bucketCnt*uintptr(t.keysize)+i*uintptr(t.elemsize))
                }
                if b.tophash[i] == emptyRest {
                    break bucketloop
                }
                continue
         }
}
```

 

倘若桶中某个位置的 tophash 标识为 emptyOne（1），说明当前位置未放入元素，倘若为 emptyRest（0），说明包括当前位置在内，此后的位置都为空.

```
const emptyRest = 0 
const emptyOne = 1 


func isEmpty(x uint8) bool {
    return x <= emptyOne
}
```

 

（13）倘若找到了相等的 key，则执行更新操作，并且直接跳转到方法的 done 标志位处，进行收尾处理；

```
    k := add(unsafe.Pointer(b), dataOffset+i*uintptr(t.keysize))
    if t.indirectkey() {
         k = *((*unsafe.Pointer)(k))
    }
    if !t.key.equal(key, k) {
        continue
    }
    elem = add(unsafe.Pointer(b), dataOffset+bucketCnt*uintptr(t.keysize)+i*uintptr(t.elemsize))
    goto done
```

 

（14）倘若没找到相等的 key，会在执行插入操作前，判断 map 是否需要开启扩容模式. 这部分内容在第 9 节中作展开.

倘若需要扩容，会在开启扩容模式后，跳转回 again 标志位，重新开始桶的定位以及遍历流程.

```
    if !h.growing() && (overLoadFactor(h.count+1, h.B) || tooManyOverflowBuckets(h.noverflow, h.B)) {
        hashGrow(t, h)
        goto again 
    }
```

 

（15）倘若遍历完桶链表，都没有为当前待插入的 key-value 对找到空位，则会创建一个新的溢出桶，挂载在桶链表的尾部，并将 inserti、insertk、elem 指向溢出桶的首个空位：

```
    if inserti == nil {
        newb := h.newoverflow(t, b)
        inserti = &newb.tophash[0]
        insertk = add(unsafe.Pointer(newb), dataOffset)
        elem = add(insertk, bucketCnt*uintptr(t.keysize))
    }
```

 

创建溢出桶时：

<img src="https://raw.githubusercontent.com/lanyoumeng/Drawing-bed/main/docs/202409052333276.webp" alt="640 (21)" style="zoom:50%;" />

I 倘若 hmap.extra 中还有剩余可用的溢出桶，则直接获取 hmap.extra.nextOverflow，并将 nextOverflow 调整指向下一个空闲可用的溢出桶；

II 倘若 hmap 已经没有空闲溢出桶了，则创建一个新的溢出桶.

III hmap 的溢出桶数量 hmap.noverflow 累加 1；

IV 将新获得的溢出桶添加到原桶链表的尾部；

V 返回溢出桶.

 

```
func (h *hmap) newoverflow(t *maptype, b *bmap) *bmap {
    var ovf *bmap
    if h.extra != nil && h.extra.nextOverflow != nil {
        ovf = h.extra.nextOverflow
        if ovf.overflow(t) == nil {
            h.extra.nextOverflow = (*bmap)(add(unsafe.Pointer(ovf), uintptr(t.bucketsize)))
        } else {
            ovf.setoverflow(t, nil)
            h.extra.nextOverflow = nil
        }
    } else {
        ovf = (*bmap)(newobject(t.bucket))
    }
    h.incrnoverflow()
    if t.bucket.ptrdata == 0 {
        h.createOverflow()
        *h.extra.overflow = append(*h.extra.overflow, ovf)
    }
    b.setoverflow(t, ovf)
    return ovf
}
```

 

（16）将 tophash、key、value 插入到取得空位中，并且将 map 的 key-value 对计数器 count 值加 1；

```
    if t.indirectkey() {
        kmem := newobject(t.key)
        *(*unsafe.Pointer)(insertk) = kmem
        insertk = kmem
    }
    if t.indirectelem() {
        vmem := newobject(t.elem)
        *(*unsafe.Pointer)(elem) = vmem
    }
    typedmemmove(t.key, insertk, key)
    *inserti = top
    h.count++
```

 

（17）收尾环节，再次校验是否有其他协程并发写，倘若有，则抛 fatal error. 将 hmap.flags 中的写标记抹去，然后退出方法.

```
done:
    if h.flags&hashWriting == 0 {
        fatal("concurrent map writes")
    }
    h.flags &^= hashWriting
    if t.indirectelem() {
        elem = *((*unsafe.Pointer)(elem))
    }
    return elem
```

 

 

#### 7 删流程

##### 7.1 删除 kv 对流程梳理

map 删楚 kv 对流程主要分为以下几步：

（1）根据 key 取 hash 值；

（2）根据 hash 值对桶数组取模，确定所在的桶；

（3）倘若 map 处于扩容，则迁移命中的桶，帮助推进渐进式扩容；

（4）沿着桶链表依次遍历各个桶内的 key-value 对；

（5）倘若命中相同的 key，删除对应的 key-value 对；并将当前位置的 tophash 置为 emptyOne，表示为空；

（6）倘若当前位置为末位，或者下一个位置的 tophash 为 emptyRest，则沿当前位置向前遍历，将毗邻的 emptyOne 统一更新为 emptyRest.

 

map 删操作最终会走进 runtime/map.go 的 mapdelete 方法中，下面开始阅读源码：

 

##### 7.2 mapdelete

```
func mapdelete(t *maptype, h *hmap, key unsafe.Pointer) {
    if h == nil || h.count == 0 {
        return
    }
    if h.flags&hashWriting != 0 {
        fatal("concurrent map writes")
    }


    hash := t.hasher(key, uintptr(h.hash0))


    h.flags ^= hashWriting


    bucket := hash & bucketMask(h.B)
    if h.growing() {
        growWork(t, h, bucket)
    }
    b := (*bmap)(add(h.buckets, bucket*uintptr(t.bucketsize)))
    bOrig := b
    top := tophash(hash)
search:
    for ; b != nil; b = b.overflow(t) {
        for i := uintptr(0); i < bucketCnt; i++ {
            if b.tophash[i] != top {
                if b.tophash[i] == emptyRest {
                    break search
                }
                continue
            }
            k := add(unsafe.Pointer(b), dataOffset+i*uintptr(t.keysize))
            k2 := k
            if t.indirectkey() {
                k2 = *((*unsafe.Pointer)(k2))
            }
            if !t.key.equal(key, k2) {
                continue
            }
            // Only clear key if there are pointers in it.
            if t.indirectkey() {
                *(*unsafe.Pointer)(k) = nil
            } else if t.key.ptrdata != 0 {
                memclrHasPointers(k, t.key.size)
            }
            e := add(unsafe.Pointer(b), dataOffset+bucketCnt*uintptr(t.keysize)+i*uintptr(t.elemsize))
            if t.indirectelem() {
                *(*unsafe.Pointer)(e) = nil
            } else if t.elem.ptrdata != 0 {
                memclrHasPointers(e, t.elem.size)
            } else {
                memclrNoHeapPointers(e, t.elem.size)
            }
            b.tophash[i] = emptyOne
            if i == bucketCnt-1 {
                if b.overflow(t) != nil && b.overflow(t).tophash[0] != emptyRest {
                    goto notLast
                }
            } else {
                if b.tophash[i+1] != emptyRest {
                    goto notLast
                }
            }
            for {
                b.tophash[i] = emptyRest
                if i == 0 {
                    if b == bOrig {
                        break
                    }
                    c := b
                    for b = bOrig; b.overflow(t) != c; b = b.overflow(t) {
                    }
                    i = bucketCnt - 1
                } else {
                    i--
                }
                if b.tophash[i] != emptyOne {
                    break
                }
            }
        notLast:
            h.count--
            if h.count == 0 {
                h.hash0 = fastrand()
            }
            break search
        }
    }


    if h.flags&hashWriting == 0 {
        fatal("concurrent map writes")
    }
    h.flags &^= hashWritin
```

 

（1）倘若 map 未初始化或者内部 key-value 对数量为 0，删除时不会报错，直接返回；

```
if h == nil || h.count == 0 {
        return
}
```

 

（2）倘若存在其他 goroutine 在进行写或删操作，抛出并发写的 fatal error；

```
if h.flags&hashWriting != 0 {
    fatal("concurrent map writes")
}
```

 

（3）通过 maptype.hasher() 方法求得 key 对应的 hash 值；

```
 hash := t.hasher(key, uintptr(h.hash0))
```

 

（4）通过异或位运算，将 map.flags 的第 3 个 bit 位置为 1，添加写标记；

```
h.flags ^= hashWriting
```

 

（5）找到当前 key 对应的桶索引 bucket；

```
bucket := hash & bucketMask(h.B)
```

 

（6）倘若发现当前 map 正处于扩容过程，则帮助其渐进扩容，具体内容在第 9 节中再作展开；

```
   if h.growing() {
        growWork(t, h, bucket)
  }
```

 

（7）从 map 的桶数组 buckets 出发，结合桶索引和桶容量大小，进行地址偏移，获得对应桶 b，并赋值给 bOrg；

```
b := (*bmap)(add(h.buckets, bucket*uintptr(t.bucketsize)))
bOrig := b
```

 

（8）取得 key 的高 8 位 tophash：

```
top := tophash(hash)
```

 

（9）开启两层 for 循环，外层沿着桶链表依次遍历，内层依次遍历桶内的 key-value 对.

```
search:
    for ; b != nil; b = b.overflow(t) {
        for i := uintptr(0); i < bucketCnt; i++ {
            // ...
        }
    }
  
```

 

（10）遍历时，倘若发现当前位置 tophash 值为 emptyRest，则直接结束遍历流程：

```
   if b.tophash[i] != top {
        if b.tophash[i] == emptyRest {
             break search
         }
         continue
   }
          
```

 

（11）倘若 key 不相等，则继续遍历：

```
   k := add(unsafe.Pointer(b), dataOffset+i*uintptr(t.keysize))
   k2 := k
   if t.indirectkey() {
        k2 = *((*unsafe.Pointer)(k2))
    }
    if !t.key.equal(key, k2) {
        continue
    }
```

 

（12）倘若 key 相等，则删除对应的 key-value 对，并且将当前位置的 tophash 置为 emptyOne：

```
   if t.indirectkey() {
        *(*unsafe.Pointer)(k) = nil
    } else if t.key.ptrdata != 0 {
        memclrHasPointers(k, t.key.size)
    }
    e := add(unsafe.Pointer(b), dataOffset+bucketCnt*uintptr(t.keysize)+i*uintptr(t.elemsize))
    if t.indirectelem() {
        *(*unsafe.Pointer)(e) = nil
    } else if t.elem.ptrdata != 0 {
        memclrHasPointers(e, t.elem.size)
    } else {
        memclrNoHeapPointers(e, t.elem.size)
    }
    b.tophash[i] = emptyOne      
```

 

（13）倘若当前位置不位于最后一个桶的最后一个位置，或者当前位置的后置位 tophash 不为 emptyRest，则无需向前遍历更新 tophash 标识，直接跳转到 notLast 位置即可；

```
   if i == bucketCnt-1 {
        if b.overflow(t) != nil && b.overflow(t).tophash[0] != emptyRest {
            goto notLast
        }
    } else {
       if b.tophash[i+1] != emptyRest {
            goto notLast
        }
    }
```

 

（14）向前遍历，将沿途的空位（ tophash 为 emptyOne ）的 tophash 都更新为 emptySet.

```
   for {
                b.tophash[i] = emptyRest
                if i == 0 {
                    if b == bOrig {
                        break
                    }
                    c := b
                    for b = bOrig; b.overflow(t) != c; b = b.overflow(t) {
                    }
                    i = bucketCnt - 1
                } else {
                    i--
                }
                if b.tophash[i] != emptyOne {
                    break
                }
        }
          
```

 

（15）倘若成功从 map 中删除了一组 key-value 对，则将 hmap 的计数器 count 值减 1. 倘若 map 中的元素全都被删除完了，会为 map 更换一个新的随机因子 hash0.

```
   notLast:
        h.count--
        if h.count == 0 {
            h.hash0 = fastrand()
        }
        break search
      
```

 

（16）收尾环节，再次校验是否有其他协程并发写，倘若有，则抛 fatal error. 将 hmap.flags 中的写标记抹去，然后退出方法.

```
    if h.flags&hashWriting == 0 {
        fatal("concurrent map writes")
    }
    h.flags &^= hashWri
```

 

#### 8 遍历流程

如果有渐进性扩容，遍历以新桶的顺序为准，如果老桶还有数据，编程模拟在哪个新桶

<img src="https://raw.githubusercontent.com/lanyoumeng/Drawing-bed/main/docs/202409052334596.webp" alt="640 (22)" style="zoom:67%;" />

map 的遍历流程首先会走进 runtime/map.go 的 mapiterinit() 方法当中，初始化用于遍历的迭代器 hiter；接着会调用 runtime/map.go 的 mapiternext() 方法开启遍历流程.

 

##### 8.1 迭代器数据结构

```
type hiter struct {
    key         unsafe.Pointer 
    elem        unsafe.Pointer 
    t           *maptype
    h           *hmap
    buckets     unsafe.Pointer 
    bptr        *bmap         
    overflow    *[]*bmap      
    oldoverflow *[]*bmap      
    startBucket uintptr       
    offset      uint8         
    wrapped     bool         
    B           uint8
    i           uint8
    bucket      uintptr
    checkBucket uintptr
}
```

hiter 是遍历 map 时用于存放临时数据的迭代器：

（1）key：指向遍历得到 key 的指针；

（2）value：指向遍历得到 value 的指针；

（3）t：map 类型，包含了 key、value 类型大小等信息；

（4）h：map 的指针；

（5）buckets：map 的桶数组；

（6）bptr：当前遍历到的桶；

（7）overflow：新老桶数组对应的溢出桶；

（8）startBucket：遍历起始位置的桶索引；

（9）offset：遍历起始位置的 key-value 对索引；

（10）wrapped：遍历是否穿越桶数组尾端回到头部了；

（11）B：桶数组的长度指数；

（12）i：当前遍历到的 key-value 对在桶中的索引；

（13）bucket：当前遍历到的桶；

（14）checkBucket：因为扩容流程的存在，需要额外检查的桶.

 

##### 8.2 mapiterinit

map 遍历流程开始时，首先会走进 runtime/map.go 的 mapiterinit() 方法当中，此时会对创建 map 迭代器 hiter，并且通过取随机数的方式，决定遍历的起始桶号，以及起始 key-value 对索引号.

<img src="https://raw.githubusercontent.com/lanyoumeng/Drawing-bed/main/docs/202409052334570.webp" alt="640 (23)" style="zoom:50%;" />

```
func mapiterinit(t *maptype, h *hmap, it *hiter) {
    it.t = t
    if h == nil || h.count == 0 {
        return
    }


    it.h = h


    it.B = h.B
    it.buckets = h.buckets
    if t.bucket.ptrdata == 0 {
        h.createOverflow()
        it.overflow = h.extra.overflow
        it.oldoverflow = h.extra.oldoverflow
    }


    // decide where to start
    var r uintptr
    r = uintptr(fastrand())
    it.startBucket = r & bucketMask(h.B)
    it.offset = uint8(r >> h.B & (bucketCnt - 1))


    // iterator state
    it.bucket = it.startBucket


    // Remember we have an iterator.
    // Can run concurrently with another mapiterinit().
    if old := h.flags; old&(iterator|oldIterator) != iterator|oldIterator {
        atomic.Or8(&h.flags, iterator|oldIterator)
    }


    mapiternext(
```

 

（1）通过取随机数的方式，决定遍历时的起始桶，以及桶中起始 key-value 对的位置：

```
   var r uintptr
    r = uintptr(fastrand())
    it.startBucket = r & bucketMask(h.B)
    it.offset = uint8(r >> h.B & (bucketCnt - 1))




    // iterator state
    it.bucket = it.startB
```

 

（2）完成迭代器 hiter 中各项参数的初始化后，不如 mapiternext 方法开启遍历.

 

##### 8.2 mapiternext

```
func mapiternext(it *hiter) {
    h := it.h
    if h.flags&hashWriting != 0 {
        fatal("concurrent map iteration and map write")
    }
    t := it.t
    bucket := it.bucket
    b := it.bptr
    i := it.i
    checkBucket := it.checkBucket


next:
    if b == nil {
        if bucket == it.startBucket && it.wrapped {
            it.key = nil
            it.elem = nil
            return
        }
        if h.growing() && it.B == h.B {
            oldbucket := bucket & it.h.oldbucketmask()
            b = (*bmap)(add(h.oldbuckets, oldbucket*uintptr(t.bucketsize)))
            if !evacuated(b) {
                checkBucket = bucket
            } else {
                b = (*bmap)(add(it.buckets, bucket*uintptr(t.bucketsize)))
                checkBucket = noCheck
            }
        } else {
            b = (*bmap)(add(it.buckets, bucket*uintptr(t.bucketsize)))
            checkBucket = noCheck
        }
        bucket++
        if bucket == bucketShift(it.B) {
            bucket = 0
            it.wrapped = true
        }
        i = 0
    }
    for ; i < bucketCnt; i++ {
        offi := (i + it.offset) & (bucketCnt - 1)
        if isEmpty(b.tophash[offi]) || b.tophash[offi] == evacuatedEmpty {
            continue
        }
        k := add(unsafe.Pointer(b), dataOffset+uintptr(offi)*uintptr(t.keysize))
        if t.indirectkey() {
            k = *((*unsafe.Pointer)(k))
        }
        e := add(unsafe.Pointer(b), dataOffset+bucketCnt*uintptr(t.keysize)+uintptr(offi)*uintptr(t.elemsize))
        if checkBucket != noCheck && !h.sameSizeGrow() {
                if checkBucket>>(it.B-1) != uintptr(b.tophash[offi]&1) {
                    continue
                }
            
        }
        if (b.tophash[offi] != evacuatedX && b.tophash[offi] != evacuatedY) ||
            !(t.reflexivekey() || t.key.equal(k, k)) {
            
            it.key = k
            if t.indirectelem() {
                e = *((*unsafe.Pointer)(e))
            }
            it.elem = e
        } else {
            rk, re := mapaccessK(t, h, k)
            if rk == nil {
                continue // key has been deleted
            }
            it.key = rk
            it.elem = re
        }
        it.bucket = bucket
        if it.bptr != b { // avoid unnecessary write barrier; see issue 14921
            it.bptr = b
        }
        it.i = i + 1
        it.checkBucket = checkBucket
        return
    }
    b = b.overflow(t)
    i = 0
    goto next
}
```

 

（1）遍历时发现其他 goroutine 在并发写，直接抛出 fatal error：

```
if h.flags&hashWriting != 0 {
    fatal("concurrent map iteration and map write")
}
```

 

（2）开启最外圈的循环，依次遍历桶数组中的每个桶链表，通过 next 和 goto next 关键字实现循环代码块；

```
next:
    if b == nil {
        // ...
        b = (*bmap)(add(it.buckets, bucket*uintptr(t.bucketsize)))
        // 
        bucket++
        if bucket == bucketShift(it.B) {
            bucket = 0
            it.wrapped = true
        }
        i = 0
    }
    // ...
    b = b.overflow(t)
    // ...
    goto next
}
```

 

 

（3）倘若已经遍历完所有的桶，重新回到起始桶为止，则直接结束方法；

```
 if bucket == it.startBucket && it.wrapped {
     it.key = nil
     it.elem = nil
     return
  }
```

 

（4）倘若 map 处于扩容流程，取桶时兼容新老桶数组的逻辑. 倘若桶处于旧桶数组且未完成迁移，需要将 checkBucket 置为当前的桶号；

```
 if h.growing() && it.B == h.B {
     oldbucket := bucket & it.h.oldbucketmask()
     b = (*bmap)(add(h.oldbuckets, oldbucket*uintptr(t.bucketsize)))
     if !evacuated(b) {
          checkBucket = bucket
     } else {
          b = (*bmap)(add(it.buckets, bucket*uintptr(t.bucketsize)))
          checkBucket = noCheck
     }
 } else {
     b = (*bmap)(add(it.buckets, bucket*uintptr(t.bucketsize)))
     checkBucket = noCheck
 }
```

 

（5）遍历的桶号加 1，倘若来到桶数组末尾，则将桶号置为 0. 将 key-value 对的遍历索引 i 置为 0.

```
bucket++
if bucket == bucketShift(it.B) {
     bucket = 0
     it.wrapped = true
}
i = 0
```

 

（6）依次遍历各个桶中每个 key-value 对：

```
    for ; i < bucketCnt; i++ {
        // ...
        return
    }
```

 

（7）倘若遍历到的桶属于旧桶数组未迁移完成的桶，需要按照其在新桶中的顺序完成遍历. 比如，增量扩容流程中，旧桶中的 key-value 对最终应该被分散迁移到新桶数组的 x、y 两个区域，则此时遍历时，哪怕 key-value 对仍存留在旧桶中未完成迁移，遍历时也应该严格按照其在新桶数组中的顺序来执行.

```
        if checkBucket != noCheck && !h.sameSizeGrow() {
            
                if checkBucket>>(it.B-1) != uintptr(b.tophash[offi]&1) {
                    continue
            }
        }
```

 

（8）执行 mapaccessK 方法，基于读流程方法获取 key-value 对，通过迭代 hiter 的 key、value 指针进行接收，用于对用户的遍历操作进行响应：

```
rk, re := mapaccessK(t, h, k)
if rk == nil {
      continue // key has been deleted
}
it.key = rk
it.elem = re
```

 

 

#### 9 扩容流程

##### 9.1 扩容类型

![640 (24)](https://raw.githubusercontent.com/lanyoumeng/Drawing-bed/main/docs/202409052335915.webp)

map 的扩容类型分为两类，一类叫做增量扩容，一类叫做等量扩容.

（1）增量扩容

表现：扩容后，桶数组的长度增长为原长度的 2 倍；

目的：降低每个桶中 key-value 对的数量，优化 map 操作的时间复杂度.

 

（2）等量扩容

松散的键值对重新排列一次，以使bucket的使用率更高，进而保证更快的存取。在

表现：扩容后，桶数组的长度和之前保持一致；但是溢出桶的数量会下降.

目的：提高桶主体结构的数据填充率，减少溢出桶数量，避免发生内存泄漏.

 

##### 9.2 何时扩容

（1）只有 map 的写流程可能开启扩容模式；

（2）写 map 新插入 key-value 对之前，会发起是否需要扩容的逻辑判断：

```
func mapassign(t *maptype, h *hmap, key unsafe.Pointer) unsafe.Pointer {
    // ...
    
    if !h.growing() && (overLoadFactor(h.count+1, h.B) || tooManyOverflowBuckets(h.noverflow, h.B)) {
        hashGrow(t, h)
        goto again
    }


    // ...
}
```

 

（3）根据 hmap 的 oldbuckets 是否空，可以判断 map 此前是否已开启扩容模式：

```
func (h *hmap) growing() bool {
    return h.oldbuckets != nil
}
```

 

（4）倘若此前未进入扩容模式，且 map 中 key-value 对的数量超过 8 个，且大于桶数组长度的 6.5 倍，则进入增量扩容：

```
const(
   loadFactorNum = 13
   loadFactorDen = 2
   bucketCnt = 8
)


func overLoadFactor(count int, B uint8) bool {
    return count > bucketCnt && uintptr(count) > loadFactorNum*(bucketShift(B)/loadFactorDen)
}
```

 

（5）倘若溢出桶的数量大于 2^B 个（即桶数组的长度；B 大于 15 时取15），则进入等量扩容：

```
func tooManyOverflowBuckets(noverflow uint16, B uint8) bool {
    if B > 15 {
        B = 15
    }
    return noverflow >= uint16(1)<<(B&15)
}
```

 

##### 9.3 如何开启扩容模式

开启扩容模式的方法位于 runtime/map.go 的 hashGrow 方法中：

```
func hashGrow(t *maptype, h *hmap) {
    bigger := uint8(1)
    if !overLoadFactor(h.count+1, h.B) {
        bigger = 0
        h.flags |= sameSizeGrow
    }
    oldbuckets := h.buckets
    newbuckets, nextOverflow := makeBucketArray(t, h.B+bigger, nil)




    flags := h.flags &^ (iterator | oldIterator)
    if h.flags&iterator != 0 {
        flags |= oldIterator
    }
    // commit the grow (atomic wrt gc)
    h.B += bigger
    h.flags = flags
    h.oldbuckets = oldbuckets
    h.buckets = newbuckets
    h.nevacuate = 0
    h.noverflow = 0


    if h.extra != nil && h.extra.overflow != nil {
        // Promote current overflow buckets to the old generation.
        if h.extra.oldoverflow != nil {
            throw("oldoverflow is not nil")
        }
        h.extra.oldoverflow = h.extra.overflow
        h.extra.overflow = nil
    }
    if nextOverflow != nil {
        if h.extra == nil {
            h.extra = new(mapextra)
        }
        h.extra.nextOverflow = nextOverflow
    }
```

 

（1）倘若是增量扩容，bigger 值取 1；倘若是等量扩容，bigger 值取 0，并将 hmap.flags 的第 4 个 bit 位置为 1，标识当前处于等量扩容流程.

```
const sameSizeGrow = 8


bigger := uint8(1)
if !overLoadFactor(h.count+1, h.B) {
    bigger = 0
    h.flags |= sameSizeGrow
}
```

 

（2）将原桶数组赋值给 oldBuckets，并创建新的桶数组和一批新的溢出桶.

此处会通过变量 bigger，实现不同扩容模式下，新桶数组长度的区别处理.

```
    oldbuckets := h.buckets
    newbuckets, nextOverflow := makeBucketArray(t, h.B+bigger, nil)
```

 

（3）更新 hmap 的桶数组长度指数 B，flag 标识，并将新、老桶数组赋值给 hmap.oldBuckets 和 hmap.buckets；扩容迁移进度 hmap.nevacuate 标记为 0；新桶数组的溢出桶数量 hmap.noverflow 置为 0.

```
    flags := h.flags &^ (iterator | oldIterator)
    if h.flags&iterator != 0 {
        flags |= oldIterator
    }
    // commit the grow (atomic wrt gc)
    h.B += bigger
    h.flags = flags
    h.oldbuckets = oldbuckets
    h.buckets = newbuckets
    h.nevacuate = 0
    h.noverflow = 0
```

 

（4）将原本存量可用的溢出桶赋给 hmap.extra.oldoverflow；倘若存在下一个可用的溢出桶，赋给 hmap.extra.nextOverflow.

```
   if h.extra != nil && h.extra.overflow != nil {
        h.extra.oldoverflow = h.extra.overflow
        h.extra.overflow = nil
    }
    if nextOverflow != nil {
        if h.extra == nil {
            h.extra = new(mapextra)
        }
        h.extra.nextOverflow = nextOverflow
  }
```

 

##### 9.4 扩容迁移规则

<img src="https://raw.githubusercontent.com/lanyoumeng/Drawing-bed/main/docs/202409052335278.webp" alt="640 (25)" style="zoom:50%;" />

（1）在等量扩容中，新桶数组长度与原桶数组相同；

（2）key-value 对在新桶数组和老桶数组的中的索引号保持一致；

（3）在增量扩容中，新桶数组长度为原桶数组的两倍；

（4）把新桶数组中桶号对应于老桶数组的区域称为 x 区域，新扩展的区域称为 y 区域.

（5）实际上，一个 key 属于哪个桶，取决于其 hash 值对桶数组长度取模得到的结果，因此依赖于其低位的 hash 值结果.；

（6）在增量扩容流程中，新桶数组的长度会扩展一位，假定 key 原本从属的桶号为 i，则在新桶数组中从属的桶号只可能是 i （x 区域）或者 i + 老桶数组长度（y 区域）；

（7）当 key 低位 hash 值向左扩展一位的 bit 位为 0，则应该迁往 x 区域的 i 位置；倘若该 bit 位为 1，应该迁往 y 区域对应的 i + 老桶数组长度的位置.

 

##### 9.5 渐进式扩容

map 采用的是渐进扩容的方式，避免因为一次性的全量数据迁移引发性能抖动.

当每次触发写、删操作时，会为处于扩容流程中的 map 完成**两组桶**的数据迁移：

（1）一组桶是**当前**写、删操作所命中的桶；

（2）另一组桶是，当前未迁移的桶中，**索引最小**的那个桶.

```
func growWork(t *maptype, h *hmap, bucket uintptr) {
    // make sure we evacuate the oldbucket corresponding
    // to the bucket we're about to use
    evacuate(t, h, bucket&h.oldbucketmask())


    // evacuate one more oldbucket to make progress on growing
    if h.growing() {
        evacuate(t, h, h.nevacuate)
    }
}
```

<img src="https://raw.githubusercontent.com/lanyoumeng/Drawing-bed/main/docs/202409052336454.webp" alt="640 (26)" style="zoom:67%;" />

数据迁移的逻辑位于 runtime/map.go 的 evacuate 方法当中：

```
func evacuate(t *maptype, h *hmap, oldbucket uintptr) {
    // 入参中，oldbucket 为当前要迁移的桶在旧桶数组中的索引
    // 获取到待迁移桶的内存地址 b
    b := (*bmap)(add(h.oldbuckets, oldbucket*uintptr(t.bucketsize)))
    // 获取到旧桶数组的容量 newbit
    newbit := h.noldbuckets()
    // evacuated 方法判断出桶 b 是否已经迁移过了，未迁移过，才进入此 if 分支进行迁移处理
    if !evacuated(b) {
        // 通过一个二元数组 xy 指向当前桶可能迁移到的目的桶
        // x = xy[0]，代表新桶数组中索引和旧桶数组一致的桶
        // y = xy[1]，代表新桶数组中，索引为原索引加上旧桶容量的桶，只在增量扩容中会使用到
        var xy [2]evacDst
        x := &xy[0]
        x.b = (*bmap)(add(h.buckets, oldbucket*uintptr(t.bucketsize)))
        x.k = add(unsafe.Pointer(x.b), dataOffset)
        x.e = add(x.k, bucketCnt*uintptr(t.keysize))


        // 只有进入增量扩容的分支，才需要对 y 进行初始化
        if !h.sameSizeGrow() {
            // Only calculate y pointers if we're growing bigger.
            // Otherwise GC can see bad pointers.
            y := &xy[1]
            y.b = (*bmap)(add(h.buckets, (oldbucket+newbit)*uintptr(t.bucketsize)))
            y.k = add(unsafe.Pointer(y.b), dataOffset)
            y.e = add(y.k, bucketCnt*uintptr(t.keysize))
        }


        // 外层 for 循环，遍历桶 b 和对应的溢出桶
        for ; b != nil; b = b.overflow(t) {
            // k,e 分别记录遍历桶时，当前的 key 和 value 的指针
            k := add(unsafe.Pointer(b), dataOffset)
            e := add(k, bucketCnt*uintptr(t.keysize))
            // 遍历桶内的 key-value 对
            for i := 0; i < bucketCnt; i, k, e = i+1, add(k, uintptr(t.keysize)), add(e, uintptr(t.elemsize)) {
                top := b.tophash[i]
                if isEmpty(top) {
                    b.tophash[i] = evacuatedEmpty
                    continue
                }
                if top < minTopHash {
                    throw("bad map state")
                }
                k2 := k
                if t.indirectkey() {
                    k2 = *((*unsafe.Pointer)(k2))
                }
                var useY uint8
                if !h.sameSizeGrow() {
                    // Compute hash to make our evacuation decision (whether we need
                    // to send this key/elem to bucket x or bucket y).
                    hash := t.hasher(k2, uintptr(h.hash0))
                    if hash&newbit != 0 {
                       useY = 1
                    }
                }
                b.tophash[i] = evacuatedX + useY // evacuatedX + 1 == evacuatedY
                dst := &xy[useY]                 // evacuation destination
                if dst.i == bucketCnt {
                    dst.b = h.newoverflow(t, dst.b)
                    dst.i = 0
                    dst.k = add(unsafe.Pointer(dst.b), dataOffset)
                    dst.e = add(dst.k, bucketCnt*uintptr(t.keysize))
                }
                dst.b.tophash[dst.i&(bucketCnt-1)] = top // mask dst.i as an optimization, to avoid a bounds check
                if t.indirectkey() {
                    *(*unsafe.Pointer)(dst.k) = k2 // copy pointer
                } else {
                    typedmemmove(t.key, dst.k, k) // copy elem
                }
                if t.indirectelem() {
                    *(*unsafe.Pointer)(dst.e) = *(*unsafe.Pointer)(e)
                } else {
                    typedmemmove(t.elem, dst.e, e)
                }
                dst.i++
                dst.k = add(dst.k, uintptr(t.keysize))
                dst.e = add(dst.e, uintptr(t.elemsize))
            }
        }
        // Unlink the overflow buckets & clear key/elem to help GC.
        if h.flags&oldIterator == 0 && t.bucket.ptrdata != 0 {
            b := add(h.oldbuckets, oldbucket*uintptr(t.bucketsize))
            // Preserve b.tophash because the evacuation
            // state is maintained there.
            ptr := add(b, dataOffset)
            n := uintptr(t.bucketsize) - dataOffset
            memclrHasPointers(ptr, n)
        }
    }


    if oldbucket == h.nevacuate {
        advanceEvacuationMark(h, t, newbit)
    }
}


func (h *hmap) noldbuckets() uintptr {
    oldB := h.B
    if !h.sameSizeGrow() {
        oldB--
    }
    return bucketShift(oldB)
```

（1）从老桶数组中获取到待迁移的桶 b；

```
b := (*bmap)(add(h.oldbuckets, oldbucket*uintptr(t.bucketsize)))
```

（2）获取到老桶数组的长度 newbit；

```
newbit := h.noldbuckets()
```

（3）倘若当前桶已经完成了迁移，则无需处理；

（4）创建一个二元数组 xy，分别承载 x 区域和 y 区域（含义定义见 9.4 小节）中的新桶位置，用于接受来自老桶数组的迁移数组；只有在增量扩容的流程中，才存在 y 区域，因此才需要对 xy 中的 y 进行定义；

```
var xy [2]evacDst
x := &xy[0]
x.b = (*bmap)(add(h.buckets, oldbucket*uintptr(t.bucketsize)))
x.k = add(unsafe.Pointer(x.b), dataOffset)
x.e = add(x.k, bucketCnt*uintptr(t.keysize))


if !h.sameSizeGrow() {
    y := &xy[1]
    y.b = (*bmap)(add(h.buckets, (oldbucket+newbit)*uintptr(t.bucketsize)))
    y.k = add(unsafe.Pointer(y.b), dataOffset)
    y.e = add(y.k, bucketCnt*uintptr(t.keysize))
}
```

 

（5）开启两层 for 循环，外层遍历桶链表，内层遍历每个桶中的 key-value 对：

```
    for ; b != nil; b = b.overflow(t) {
        k := add(unsafe.Pointer(b), dataOffset)
        e := add(k, bucketCnt*uintptr(t.keysize))
        for i := 0; i < bucketCnt; i, k, e = i+1, add(k, uintptr(t.keysize)), add(e, uintptr(t.elemsize)) {
           // ...
        }
    }
       
```

 

（6）取每个位置的 tophash 值进行判断，倘若当前是个空位，则将当前位置 tophash 值置为 evacuatedEmpty，开始遍历下一个位置：

```
 top := b.tophash[i]
 if isEmpty(top) {
      b.tophash[i] = evacuatedEmpty
      continue
 }
```

 

（7）基于 9.4 的规则，寻找到迁移的目的桶；

```
  const evacuatedX = 2
  const evacuatedY = 3  


  k2 := k
  var useY uint8
  if !h.sameSizeGrow() {       
       hash := t.hasher(k2, uintptr(h.hash0))
       if hash&newbit != 0 {
            useY = 1
       }
  }
  b.tophash[i] = evacuatedX + useY // evacuatedX + 1 == evacuatedY
  dst := &xy[useY]
```

 

其中目的桶的类型定义如下：

```
type evacDst struct {
    b *bmap          // current destination bucket
    i int            // key/elem index into b
    k unsafe.Pointer // pointer to current key storage
    e unsafe.Pointer // pointer to current elem storage
}
```

I evacDst.b：目的地的所在桶；

II evacDst.i：即将入桶的 key-value 对在桶中的索引；

III evacDst.k：入桶 key 的存储指针；

IV evacDst.e：入桶 value 的存储指针.

 

（8）将 key-value 对迁移到目的桶中，并且更新目的桶结构内几个指针的指向：

```
  if dst.i == bucketCnt {
       dst.b = h.newoverflow(t, dst.b)
       dst.i = 0
       dst.k = add(unsafe.Pointer(dst.b), dataOffset)
       dst.e = add(dst.k, bucketCnt*uintptr(t.keysize))
  }
  dst.b.tophash[dst.i&(bucketCnt-1)] = top // mask dst.i as an optimization, to avoid a bounds check
  if t.indirectkey() {
       *(*unsafe.Pointer)(dst.k) = k2 // copy pointer
  } else {
       typedmemmove(t.key, dst.k, k) // copy elem
  }
  if t.indirectelem() {
       *(*unsafe.Pointer)(dst.e) = *(*unsafe.Pointer)(e)
  } else {
       typedmemmove(t.elem, dst.e, e)
  }
  dst.i++
  dst.k = add(dst.k, uintptr(t.keysize))
  dst.e = add(dst.e, uintptr(t.elemsize
```

 

（9）倘若当前迁移的桶是旧桶数组未迁移的桶中索引最小的一个，则 hmap.nevacuate 累加 1.

倘若已经迁移完所有的旧桶，则会确保 hmap.flags 中，等量扩容的标识位被置为 0.

```
  if oldbucket == h.nevacuate {
      advanceEvacuationMark(h, t, newbit)
  }
```

 

```
func advanceEvacuationMark(h *hmap, t *maptype, newbit uintptr) {
    h.nevacuate++
    // ...
    if h.nevacuate == newbit { // newbit == # of oldbuckets
        h.oldbuckets = nil
        if h.extra != nil {
            h.extra.oldoverflow = nil
        }
        h.flags &^= sameSizeGrow
    }
}
```













### sync.map

read map 和 dirty map 的数据流转

1.range是read map的数据不足

2.miss值 过高超过了dirty map数据量，全量覆盖read map  O(1)，dirty map 置nil，

然后又插入新的key-value（read map失守），再拷贝回dirty map ：：此时因为要避免软删除和硬删除的数据，会进行for遍历，性能下降严重O(N)，线性抖动

​	进入dirty map删除和读，都会让miss+1

通过read map减少加锁，cas删除和 插入/更新

CAS无锁化编程



#### 介绍

`sync.Map` 是 Go 语言中提供的一个并发安全的 map（字典），它位于 `sync` 包中。`sync.Map` 允许多个 goroutine 同时对 map 进行读写操作，而无需手动管理锁机制。

- • sync.Map 适用于读多、更新多、删多、写少的场景；
- • 倘若写操作过多，sync.Map 基本等价于互斥锁 + map；
- • sync.Map 可能存在性能抖动问题，主要发生于在读/删流程 miss 只读 map 次数过多时（触发 missLocked 流程），下一次插入操作的过程当中（dirtyLocked 流程）.





#### 核心数据结构

##### Map

<img src="https://raw.githubusercontent.com/lanyoumeng/Drawing-bed/main/docs/202409052306503.png" alt="640" style="zoom: 67%;" />

```
type Map struct {
    mu Mutex
    read atomic.Value 
    dirty map[any]*entry
    misses int
}
```

sync.Map 主类中包含以下核心字段：

- • read：无锁化的只读 map，实际类型为 readOnly，2.3 小节会进一步介绍；
- • dirty：加锁处理的读写 map；
- • misses：记录访问 read 的失效次数，累计达到阈值时，会进行 read map/dirty map 的更新轮换；
- • mu：一把互斥锁，实现 dirty 和 misses 的并发管理.

可见，sync.Map 的特点是冗余了两份 map：read map 和 dirty map，后续的所介绍的交互流程也和这两个 map 息息相关，基本可以归结为两条主线：

主线一：首先基于无锁操作访问 read map；倘若 read map 不存在该 key，则加锁并使用 dirty map 兜底；

主线二：read map 和 dirty map 之间会交替轮换更新.



#####  entry 及对应的几种状态

```
type entry struct {
    p unsafe.Pointer 
}
```

kv 对中的 value，统一采用 unsafe.Pointer 的形式进行存储，通过 entry.p 的指针进行链接.

entry.p 的指向分为三种情况：

I 存活态：正常指向元素；

II 软删除态：指向 nil；

III 硬删除态：指向固定的全局变量 expunged.

```
var expunged = unsafe.Pointer(new(any))
```

- • 存活态很好理解，即 key-entry 对仍未删除；
- • nil 态表示软删除，read map 和 dirty map 底层的 map 结构仍存在 key-entry 对，但在逻辑上该 key-entry 对已经被删除，因此无法被用户查询到；
- • expunged 态表示硬删除，dirty map 中已不存在该 key-entry 对.

 

##### readOnly

```
type readOnly struct {
    m       map[any]*entry
    amended bool // true if the dirty map contains some key not in m.
}
```

sync.Map 中的只读 map：read 内部包含两个成员属性：

- • m：真正意义上的 read map，实现从 key 到 entry 的映射；
- • amended：标识 read map 中的 key-entry 对是否存在缺失，需要通过 dirty map 兜底.



1.无锁更新时（key在read存在并且是软删除），因为**value存的是指针**，在**readmap中进行更新值相当于把dirtymap的值更新**

2.所以没懂之前为什么要把dirty设置为nil，那个时候明明readonly和dirty一致的，如果之前没设置nil，这里就不需要又拷贝回来了啊

1. 把readonly map 中软删除更改为硬删除。注意此次拷贝不管是软删除还是硬删除都不会拷贝回dirty，相当于屏蔽掉（间接删除），因为read map不能直接删除。  所以dirty map一定没有硬删除状态数据。来回拷贝进行数据的轮换
2. 拷贝后将 dirty设置成nil，相当于**浅拷贝**（value存的是指针），是有种懒加载的思想在里面的，如果后续没有对该map有put操作就永远不需要进行深拷贝。



#### 读流程

![640](https://raw.githubusercontent.com/lanyoumeng/Drawing-bed/main/docs/202409052308266.webp)

#####  sync.Map.Load()

```
func (m *Map) Load(key any) (value any, ok bool) {
    read, _ := m.read.Load().(readOnly)
    e, ok := read.m[key]
    if !ok && read.amended {
        m.mu.Lock()
        read, _ = m.read.Load().(readOnly)
        e, ok = read.m[key]
        if !ok && read.amended {
            e, ok = m.dirty[key]
            m.missLocked()
        }
        m.mu.Unlock()
    }
    if !ok {
        return nil, false
    }
    return e.load()
}
```

- • 查看 read map 中是否存在 key-entry 对，若存在，则直接读取 entry 返回；
- • 倘若第一轮 read map 查询 miss，且 read map 不全，则需要加锁 double check；
- • 第二轮 read map 查询仍 miss（加锁后），且 read map 不全，则查询 dirty map 兜底；
- • 查询操作涉及到与 dirty map 的交互，misses 加一；
- • 解锁，返回查得的结果.

##### entry.load()

```
func (e *entry) load() (value any, ok bool) {
    p := atomic.LoadPointer(&e.p)
    if p == nil || p == expunged {
        return nil, false
    }
    return *(*any)(p), true
}
```

- • sync.Map 中，kv 对的 value 是基于 entry 指针封装的形式；
- • 从 map 取得 entry 后，最终需要调用 entry.load 方法读取指针指向的内容；
- • 倘若 entry 的指针状态为 nil 或者 expunged，说明 key-entry 对已被删除，则返回 nil；
- • 倘若 entry 未被删除，则读取指针内容，并且转为 any 的形式进行返回.

#####  sync.Map.missLocked()

```
func (m *Map) missLocked() {
    m.misses++
    if m.misses < len(m.dirty) {
        return
    }
    m.read.Store(readOnly{m: m.dirty})
    m.dirty = nil
    m.misses = 0
}
```

- • 在读流程中，倘若未命中 read map，且由于 read map 内容存在缺失需要和 dirty map 交互时，会走进 missLocked 流程；
- • 在 missLocked 流程中，首先 misses 计数器累加 1；
- • 倘若 miss 次数小于 dirty map 中存在的 key-entry 对数量，直接返回即可；
- • 倘若 miss 次数大于等于 dirty map 中存在的 key-entry 对数量，则使用 dirty map 覆盖 read map，并将 read map 的 amended flag 置为 false；
- • 新的 dirty map 置为 nil，misses 计数器清零.



#### 写流程

![640 (1)](https://raw.githubusercontent.com/lanyoumeng/Drawing-bed/main/docs/202409052310502.webp)

##### sync.Map.Store()

```
func (m *Map) Store(key, value any) {
    read, _ := m.read.Load().(readOnly)
    if e, ok := read.m[key]; ok && e.tryStore(&value) {
        return
    }


    m.mu.Lock()
    read, _ = m.read.Load().(readOnly)
    if e, ok := read.m[key]; ok {
        if e.unexpungeLocked() {
            m.dirty[key] = e
        }
        e.storeLocked(&value)
    } else if e, ok := m.dirty[key]; ok {
        e.storeLocked(&value)
    } else {
        if !read.amended {
            m.dirtyLocked()
            m.read.Store(readOnly{m: read.m, amended: true})
        }
        m.dirty[key] = newEntry(value)
    }
    m.mu.Unlock()
}


func (e *entry) storeLocked(i *any) {
    atomic.StorePointer(&e.p, unsafe.Pointe
}
```

（1）倘若 read map 存在拟写入的 key，且 entry 不为 expunged 状态，说明这次操作属于更新而非插入，直接基于 CAS 操作进行 entry 值的更新，并直接返回（存活态或者软删除，直接覆盖更新）；

（2）倘若未命中（1）的分支，则需要加锁 double check；

（3）倘若第二轮检查中发现 read map 或者 dirty map 中存在 key-entry 对，则直接将 entry 更新为新值即可（存活态或者软删除，直接覆盖更新）；

（4）在第（3）步中，如果发现 read map 中该 key-entry 为 expunged 态，需要在 dirty map 先补齐 key-entry 对，再更新 entry 值（从硬删除中恢复，然后覆盖更新）；

（5）倘若 read map 和 dirty map 均不存在，则在 dirty map 中插入新 key-entry 对，并且保证 read map 的 amended flag 为 true.（插入）

（6）第（5）步的分支中，倘若发现 dirty map 未初始化，需要前置执行 dirtyLocked 流程；

（7）解锁返回.  

下面补充介绍 Store() 方法中涉及到的几个子方法.

##### entry.tryStore()

```
func (m *Map) Store(key, value any) {
    read, _ := m.read.Load().(readOnly)
    if e, ok := read.m[key]; ok && e.tryStore(&value) {
        return
    }


    m.mu.Lock()
   // ...
}


func (e *entry) tryStore(i *any) bool {
    for {
        p := atomic.LoadPointer(&e.p)
        if p == expunged {
            return false
        }
        if atomic.CompareAndSwapPointer(&e.p, p, unsafe.Pointer(i)) {
            return true
        }
    }
}
```

- • 在写流程中，倘若发现 read map 中已存在对应的 key-entry 对，则会对调用 tryStore 方法尝试进行更新；
- • 倘若 entry 为 expunged 态，说明已被硬删除，dirty 中缺失该项数据，因此 tryStore 执行失败，回归主干流程；
- • 倘若 entry 非 expunged 态，则直接执行 CAS 操作完成值的更新即可.

 

##### entry.unexpungeLocked()

```
func (m *Map) Store(key, value any) {
    // ...
    m.mu.Lock()
    read, _ = m.read.Load().(readOnly)
    if e, ok := read.m[key]; ok {
        if e.unexpungeLocked() {
            m.dirty[key] = e
        }
        e.storeLocked(&value)
    } 
    // ...
}


func (e *entry) unexpungeLocked() (wasExpunged bool) {
    return atomic.CompareAndSwapPointer(&e.p, expunged, nil)
}
```

- • 在写流程加锁 double check 的过程中，倘若发现 read map 中存在对应的 key-entry 对，会执行该方法；
- • 倘若 key-entry 为硬删除 expunged 态，该方法会基于 CAS 操作将其更新为软删除 nil 态，然后进一步在 dirty map 中补齐该 key-entry 对，实现从硬删除到软删除的恢复.

 

##### entry.storeLocked()

```
func (m *Map) Store(key, value any) {
    // ...
    m.mu.Lock()
    read, _ = m.read.Load().(readOnly)
    if e, ok := read.m[key]; ok {
       // ...
        e.storeLocked(&value)
    } else if e, ok := m.dirty[key]; ok {
        e.storeLocked(&value)
    } 
    // ...
}


func (e *entry) storeLocked(i *any) {
    atomic.StorePointer(&e.p, unsafe.Pointer)
}
```

写流程中，倘若 read map 或者 dirty map 存在对应 key-entry，最终会通过原子操作，将新值的指针存储到 entry.p 当中.



##### sync.Map.dirtyLocked()

![640 (2)](https://raw.githubusercontent.com/lanyoumeng/Drawing-bed/main/docs/202409052311507.webp)

```
func (m *Map) dirtyLocked() {
    if m.dirty != nil {
        return
    }


    read, _ := m.read.Load().(readOnly)
    m.dirty = make(map[any]*entry, len(read.m))
    for k, e := range read.m {
        if !e.tryExpungeLocked() {
            m.dirty[k] = e
        }
    }
}


func (e *entry) tryExpungeLocked() (isExpunged bool) {
    p := atomic.LoadPointer(&e.p)
    for p == nil {
        if atomic.CompareAndSwapPointer(&e.p, nil, expunged) {
            return true
        }
        p = atomic.LoadPointer(&e.p)
    }
    return p == expunged
}
```

- • 在写流程中，倘若需要将 key-entry 插入到兜底的 dirty map 中，并且此时 dirty map 为空（从未写入过数据或者刚发生过 missLocked），会进入 dirtyLocked 流程；
- • 此时会遍历一轮 read map ，将未删除的 key-entry 对拷贝到 dirty map 当中；
- • 在遍历时，还会将 read map 中软删除 nil 态的 entry 更新为硬删除 expunged 态，因为在此流程中，不会将其拷贝到 dirty map.

 

#### 删流程



![640 (3)](https://raw.githubusercontent.com/lanyoumeng/Drawing-bed/main/docs/202409052312376.webp)



##### sync.Map.Delete()

```
func (m *Map) Delete(key any) {
    m.LoadAndDelete(key)
}


func (m *Map) LoadAndDelete(key any) (value any, loaded bool) {
    read, _ := m.read.Load().(readOnly)
    e, ok := read.m[key]
    if !ok && read.amended {
        m.mu.Lock()
        read, _ = m.read.Load().(readOnly)
        e, ok = read.m[key]
        if !ok && read.amended {
            e, ok = m.dirty[key]
            delete(m.dirty, key)
            m.missLocked()
        }
        m.mu.Unlock()
    }
    if ok {
        return e.delete()
    }
    return nil, false
}
```

（1）倘若 read map 中存在 key，则直接基于 cas 操作将其删除；

（2）倘若read map 不存在 key，且 read map 有缺失（amended flag 为 true），则加锁 dou check；

（3）倘若加锁 double check 时，read map 仍不存在 key 且 read map 有缺失，则从 dirty map 中取元素，并且将 key-entry 对从 dirty map 中物理删除；

（4）走入步骤（3），删操作需要和 dirty map 交互，需要走进 3.3 小节介绍的 missLocked 流程；

（5）解锁；

（6）倘若从 read map 或 dirty map 中获取到了 key 对应的 entry，则走入 entry.delete() 方法逻辑删除 entry；

（7）倘若 read map 和 dirty map 中均不存在 key，返回 false 标识删除失败.  

##### entry.delete()

```
func (e *entry) delete() (value any, ok bool) {
    for {
        p := atomic.LoadPointer(&e.p)
        if p == nil || p == expunged {
            return nil, false
        }
        if atomic.CompareAndSwapPointer(&e.p, p, nil) {
            return *(*any)(p), true
        }
    }
}
```

- • 该方法是 entry 的逻辑删除方法；
- • 倘若 entry 此前已被删除，则直接返回 false 标识删除失败；
- • 倘若 entry 当前仍存在，则通过 CAS 将 entry.p 指向 nil，标识其已进入软删除状态.

 

#### 遍历流程

![640 (4)](https://raw.githubusercontent.com/lanyoumeng/Drawing-bed/main/docs/202409052312967.webp)

```
func (m *Map) Range(f func(key, value any) bool) {
    read, _ := m.read.Load().(readOnly)
    if read.amended {
        m.mu.Lock()
        read, _ = m.read.Load().(readOnly)
        if read.amended {
            read = readOnly{m: m.dirty}
            m.read.Store(read)
            m.dirty = nil
            m.misses = 0
        }
        m.mu.Unlock()
    }


    for k, e := range read.m {
        v, ok := e.load()
        if !ok {
            continue
        }
        if !f(k, v) {
            break
        }
    }
}
```

- （1）在遍历过程中，倘若发现 read map 数据不全（amended flag 为 true），会额外加一次锁，并使用 dirty map 覆盖 read map；
- （2）遍历 read map（通过步骤（1）保证 read map 有全量数据），执行用户传入的回调函数，倘若某次回调时返回值为 false，则会终止全流程.

 

#### 总结

##### entry 的 expunged 态

**思考问题：**

为什么需要使用 expunged 态来区分软硬删除呢？仅用 nil 一种状态来标识删除不可以吗？

**回答：**

首先需要明确，无论是软删除(nil)还是硬删除(expunged),都表示在逻辑意义上 key-entry 对已经从 sync.Map 中删除，nil 和 expunged 的区别在于：

• 软删除态（nil）：read map 和 dirty map 在物理上仍保有该 key-entry 对，因此倘若此时需要对该 entry 执行写操作，可以直接 CAS 操作；

• 硬删除态（expunged）：dirty map 中已经没有该 key-entry 对，倘若执行写操作，必须加锁（dirty map 必须含有全量 key-entry 对数据）.

![640 (5)](https://raw.githubusercontent.com/lanyoumeng/Drawing-bed/main/docs/202409052313249.webp)

设计 expunged 和 nil 两种状态的原因，就是为了**优化在 dirtyLocked 前，针对同一个 key 先删后写的场景**. 通过 expunged 态额外标识出 dirty map 中是否仍具有指向该 entry 的能力，这样能够实现对一部分 nil 态 key-entry 对的解放，能够基于 CAS 完成这部分内容写入操作而无需加锁.

 

##### read map 和 dirty map 的数据流转

sync.Map 由两个 map 构成：

- • read map：访问时全程无锁；
- • dirty map：是兜底的读写 map，访问时需要加锁.

之所以这样处理，是希望能根据对读、删、更新、写操作频次的探测，来实时动态地调整操作方式，希望在读、更新、删频次较高时，更多地采用 **CAS 的方式无锁化**地完成操作；在写操作频次较高时，则直接了当地采用加锁操作完成.

因此， sync.Map 本质上采取了一种**以空间换时间 + 动态调整策略**的设计思路，下面对两个 map 间的数据流转过程进行详细介绍：

######  两个 map

![640 (6)](https://raw.githubusercontent.com/lanyoumeng/Drawing-bed/main/docs/202409052313236.webp)

- • 总体思想，希望能多用 read map，少用 dirty map，因为操作前者无锁，后者需要加锁；
- • 除了 expunged 态的 entry 之外，**read map 的内容为 dirty map 的子集；**

###### dirty map -> read map

![640 (7)](https://raw.githubusercontent.com/lanyoumeng/Drawing-bed/main/docs/202409052313813.webp)

• 记录读/删流程中，通过 misses 记录访问 read map miss 由 dirty 兜底处理的次数，当 miss 次数达到阈值，则进入 missLocked 流程，进行新老 read/dirty 替换流程；此时将老 dirty 作为新 read，新 dirty map 则暂时为空，直到 dirtyLocked 流程完成对 dirty 的初始化；

###### read map -> dirty map

![640 (8)](https://raw.githubusercontent.com/lanyoumeng/Drawing-bed/main/docs/202409052314458.webp)

- • 发生 dirtyLocked 的前置条件：I dirty 暂时为空（此前没有写操作或者近期进行过 missLocked 流程）；II 接下来一次写操作访问 read 时 miss，需要由 dirty 兜底；
- • 在 dirtyLocked 流程中，需要对 read 内的元素进行状态更新，因此需要遍历，是一个线性时间复杂度的过程，可能存在性能抖动；
- • dirtyLocked 遍历中，会将 read 中未被删除的元素（非 nil 非 expunged）拷贝到 dirty 中；会将 read 中所有此前被删的元素统一置为 expunged 态.



### slice

源码为 go v1.19

• slice 是一个长度可变的连续数据序列，在实现上基于一个 slice header 组成，其中包含的字段包括：指向内存空间地址起点的指针 array、一个表示了存储数据长度的 len 和分配空间长度的 cap

• 由于 slice 在传递过程中，本质上传递的是 slice header 实例中的内存地址 array，因此属于引用传递

• slice 在扩容时，遵循如下机制:

- • 如果扩容时预期的新容量超过原容量的两倍，直接取预期的新容量

- • 如果原容量小于 256，直接取原容量的两倍作为新容量

- • 如果原容量大于等于 256，在原容量 n 的基础上循环执行 n += (n+3*256)/4 --------5/4n+192的操作，直到 n 大于等于预期新容量，并取 n 作为新容量


• 最后还需要友情提示一下，slice 不是并发安全的数据结构，大家在使用时请务必注意并发安全问题.



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
-  len：切片的长度，指的是逻辑意义上 slice 中实际存放了多少个元素
-  cap：切片的容量，指的是物理意义上为 slice 分配了足够用于存放多少个元素的空间. 使用 slice 时，要求 cap 永远大于等于 len

slice 本身属于引用传递操作

 

##### **初始化**

下面先来介绍下切片的初始化操作：

• 声明但不初始化

下面给出的第一个例子，只是声明了 slice 的类型，但是并没有执行初始化操作，即 s 这个字面量此时是一个**空指针 nil**，并没有完成实际的内存分配操作.

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

下面我们捋一下切片扩容的流程. 当 **slice 当前的长度 len 与容量 cap 相等时，下一次 append 操作就会引发一次切片扩容**

**扩容后会迁移到新地址**

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

• 倘若预期的**新容量超过老容量的两倍**，则直接采用预期的新容量

• 倘若**老容量小于 256**，则直接采用老容量的**2倍**作为新容量

• 倘若**老容量已经大于等于 256**，则在老容量的基础上**扩容 1/4 的比例并且累加上 192** 的数值，持续这样处理，直到得到的新容量已经大于等于预期的新容量为止

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

要实现简单拷贝，我们只需要对切片的字面量进行赋值传递即可，这样相当于创建出了一个新的 slice header 实例，但是**其中的指针 array、容量 cap 和长度 len 仍和老的 slice header 实例相同.**

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



slice 的**完整复制**，指的是会创建出一个和 slice 容量大小相等的独立的内存区域，并将原 slice 中的元素一一拷贝到新空间中.

在实现上，slice 的完整复制可以**调用系统方法copy**，代码示例如下，通过日志打印的方式可以看到，s 和 s1 的地址是相互独立的：

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

###### 1.

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

再进一步，结合分配内存的 mallocgc 流程，为了更好地进行内存空间对其，golang 允许产生一些有限的内部碎片，对拟申请空间的 object 进行大小补齐，**最终 6656 byte 会被补齐到 6784 byte 的这一档次.** （内存分配时，对象分档以及与 **mspan 映射**细节可以参考 golang 标准库 runtime/sizeclasses.go 文件，也可以阅读我的文章了解更多细节——golang 内存模型与分配机制）

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

###### 2

```
func Test_slice(t *testing.T){
    s := make([]int,10,12)  
    s1 := s[8:]
    changeSlice(s1)
    t.Logf("s: %v, len of s: %d, cap of s: %d",s, len(s), cap(s))
    t.Logf("s1: %v, len of s1: %d, cap of s1: %d",s1, len(s1), cap(s1))
}


func changeSlice(s1 []int){
  s1 = append(s1, 10)
}
```

答案：

```
s: [0 0 0 0 0 0 0 0 0 0], len of s: 10, cap of s: 12
s1: [0 0], len of s1: 2, cap of s1: 4
```

虽然切**片是引用传递，但是在方法调用时，传递的会是一个新的 slice header.**

因此在局部方法 changeSlice 中，虽然对 s1 进行了 append 操作，但这会在局部方法中这个**独立的 slice header 中生效**，不会影响到原方法 Test_slice 当中的 s 和 s1 的长度和容量.





### channel

通过**通信**来实现**共享内存**（channel），而不是通过共享内存来实现通信（例如多线程）



#### 1 核心数据结构

<img src="https://raw.githubusercontent.com/lanyoumeng/Drawing-bed/main/docs/202409191052990.png" alt="640" style="zoom: 67%;" />

##### 1.1 hchan

```
type hchan struct {
    qcount   uint           // total data in the queue
    dataqsiz uint           // size of the circular queue
    buf      unsafe.Pointer // points to an array of dataqsiz elements
    elemsize uint16
    closed   uint32
    elemtype *_type // element type
    sendx    uint   // send index
    recvx    uint   // receive index
    recvq    waitq  // list of recv waiters
    sendq    waitq  // list of send waiters
    
    lock mutex
}
```

hchan：channel 数据结构

- • qcount：当前 channel 中存在多少个元素；
- • dataqsize: 当前 channel 能存放的元素容量；
- • buf：channel 中用于存放元素的环形缓冲区；
- • elemsize：channel 元素类型的大小；
- • closed：标识 channel 是否关闭；
- • elemtype：channel 元素类型；
- • sendx：发送元素进入环形缓冲区的 index；
- • recvx：接收元素所处的环形缓冲区的 index；
- • recvq：因接收而陷入阻塞的协程队列；
- • sendq：因发送而陷入阻塞的协程队列；

##### 1.2 waitq

```
type waitq struct {
    first *sudog
    last  *sudog
}
```

waitq：阻塞的协程队列

- • first：队列头部
- • last：队列尾部

##### 1.3 sudog

```
type sudog struct {
    g *g

    next *sudog
    prev *sudog
    elem unsafe.Pointer // data element (may point to stack)

    isSelect bool

    c        *hchan 
}
```

sudog：用于包装协程的节点

- • g：goroutine，协程；
- • next：队列中的下一个节点；
- • prev：队列中的前一个节点；
- • elem: 读取/写入 channel 的数据的容器;
- • isSelect：标识当前协程是否处在 select 多路复用的流程中；
- • c：标识与当前 sudog 交互的 chan.

####  构造器函数

![640 (1)](https://raw.githubusercontent.com/lanyoumeng/Drawing-bed/main/docs/202409191053106.webp)

- • 判断申请内存空间大小是否越界，mem 大小为 element 类型大小与 element 个数相乘后得到，仅当无缓冲型 channel 时，因个数为 0 导致大小为 0；
- • 根据类型，初始 channel，分为 无缓冲型、有缓冲元素为 struct 型、有缓冲元素为 pointer 型 channel;
- • 倘若为无缓冲型，则仅申请一个大小为默认值 96 的空间；
- • 如若有缓冲的 struct 型，则一次性分配好 96 + mem 大小的空间，并且调整 chan 的 buf 指向 mem 的起始位置；
- • 倘若为有缓冲的 pointer 型，则分别申请 chan 和 buf 的空间，两者无需连续；
- • 对 channel 的其余字段进行初始化，包括元素类型大小、元素类型、容量以及锁的初始化.





#### 写流程

#####  写流程整体串联

<img src="https://raw.githubusercontent.com/lanyoumeng/Drawing-bed/main/docs/202409191057865.webp" alt="640 (5)" style="zoom:50%;" />

##### 3.1 两类异常情况处理

- • 对于未初始化的 chan，写入操作会引发死锁；
- • 对于已关闭的 chan，写入操作会引发 panic.



#####  case1：写时存在阻塞读协程

<img src="https://raw.githubusercontent.com/lanyoumeng/Drawing-bed/main/docs/202409191055904.webp" alt="640 (2)" style="zoom:50%;" />

- • 加锁；
- • 从阻塞度协程队列中取出一个 goroutine 的封装对象 sudog；
- • 在 send 方法中，会基于 memmove 方法，直接将元素拷贝交给 sudog 对应的 goroutine；
- • 在 send 方法中会完成解锁动作.

##### case2：写时无阻塞读协程但环形缓冲区仍有空间

<img src="https://raw.githubusercontent.com/lanyoumeng/Drawing-bed/main/docs/202409191056912.webp" alt="640 (3)" style="zoom:50%;" />

- • 加锁；
- • 将当前元素添加到环形缓冲区 sendx 对应的位置；
- • sendx++;
- • qcount++;
- • 解锁，返回.

##### case3：写时无阻塞读协程且环形缓冲区无空间

<img src="https://raw.githubusercontent.com/lanyoumeng/Drawing-bed/main/docs/202409191056039.webp" alt="640 (4)" style="zoom:50%;" />

- • 加锁；
- • 构造封装当前 goroutine 的 sudog 对象；
- • 完成指针指向，建立 sudog、goroutine、channel 之间的指向关系；
- • 把 sudog 添加到当前 channel 的阻塞写协程队列中；
- • park 当前协程；
- • 倘若协程从 park 中被唤醒，则回收 sudog（sudog能被唤醒，其对应的元素必然已经被读协程取走）；
- • 解锁，返回





#### 读流程

#####  读流程整体串联

<img src="https://raw.githubusercontent.com/lanyoumeng/Drawing-bed/main/docs/202409240936993.webp" alt="640 (3)" style="zoom:50%;" />

##### 4.1 异常 case1：读nil channel

```
func chanrecv(c *hchan, ep unsafe.Pointer, block bool) (selected, received bool) {
    if c == nil {
        gopark(nil, nil, waitReasonChanReceiveNilChan, traceEvGoStop, 2)
        throw("unreachable")
    }
    // ...
}
```

- • park 挂起，引起死锁；

##### 4.2 异常 case2：channel 已关闭且内部无元素

```
func chanrecv(c *hchan, ep unsafe.Pointer, block bool) (selected, received bool) {
  
    lock(&c.lock)

    if c.closed != 0 {
        if c.qcount == 0 {
            unlock(&c.lock)
            if ep != nil {
                typedmemclr(c.elemtype, ep)
            }
            return true, false
        }
        // The channel has been closed, but the channel's buffer have data.
    } 

    // ...
```

- • 直接解锁返回即可

已关闭且内部有元素，正常读取

##### 4.3 case3：读时有阻塞的写协程

<img src="https://raw.githubusercontent.com/lanyoumeng/Drawing-bed/main/docs/202409240934358.webp" alt="640" style="zoom:50%;" />

- • 加锁；
- • 从阻塞写协程队列中获取到一个写协程；
- • 倘若 channel 无缓冲区，则直接读取写协程元素，并唤醒写协程；
- • 倘若 channel 有缓冲区，则读取缓冲区头部元素，并将写协程元素写入缓冲区尾部后唤醒写写成；
- • 解锁，返回.



##### 4.4 case4：读时无阻塞写协程且缓冲区有元素

- • 加锁；
- • 获取到 recvx 对应位置的元素；
- • recvx++
- • qcount--
- • 解锁，返回

<img src="https://raw.githubusercontent.com/lanyoumeng/Drawing-bed/main/docs/202409240934654.webp" alt="640 (1)" style="zoom:50%;" />

##### 4.5 case5：读时无阻塞写协程且缓冲区无元素

- • 加锁；
- • 构造封装当前 goroutine 的 sudog 对象；
- • 完成指针指向，建立 sudog、goroutine、channel 之间的指向关系；
- • 把 sudog 添加到当前 channel 的阻塞读协程队列中；
- • park 当前协程；
- • 倘若协程从 park 中被唤醒，则回收 sudog（sudog能被唤醒，其对应的元素必然已经被写入）；
- • 解锁，返回

<img src="https://raw.githubusercontent.com/lanyoumeng/Drawing-bed/main/docs/202409240935645.webp" alt="640 (2)" style="zoom:50%;" />



####  阻塞与非阻塞模式

在上述源码分析流程中，均是以阻塞模式为主线进行讲述，忽略非阻塞模式的有关处理逻辑. 此处阐明两个问题：

- • 非阻塞模式下，流程逻辑有何区别？
- • 何时会进入非阻塞模式？

##### 5.1 非阻塞模式逻辑区别

非阻塞模式下，读/写 channel 方法通过一个 bool 型的响应参数，用以标识是否读取/写入成功.

- • 所有需要使得当前 goroutine 被挂起的操作，在非阻塞模式下都会返回 false；
- • 所有是的当前 goroutine 会进入死锁的操作，在非阻塞模式下都会返回 false；
- • 所有能立即完成读取/写入操作的条件下，非阻塞模式下会返回 true.

##### 5.2 何时进入非阻塞模式

默认情况下，读/写 channel 都是阻塞模式，只有在 select 语句组成的多路复用分支中，与 channel 的交互会变成非阻塞模式：

```
ch := make(chan int)
select{
  case <- ch:
  default:
}
```

##### 5.3 代码一览

```
func selectnbsend(c *hchan, elem unsafe.Pointer) (selected bool) {
    return chansend(c, elem, false, getcallerpc())
}

func selectnbrecv(elem unsafe.Pointer, c *hchan) (selected, received bool) {
    return chanrecv(c, elem, false)
}
```

在 select 语句包裹的多路复用分支中，读和写 channel 操作会被汇编为 selectnbrecv 和 selectnbsend 方法，底层同样复用 chanrecv 和 chansend 方法，但此时由于第三个入参 block 被设置为 false，导致后续会走进非阻塞的处理分支.

#### 两种读 channel 的协议

读取 channel 时，可以根据第二个 bool 型的返回值用以判断当前 channel 是否已处于关闭状态：

true为读取到，false为关闭

```
ch := make(chan int, 2)
got1 := <- ch
got2,ok := <- ch
```

实现上述功能的原因是，两种格式下，读 channel 操作会被**汇编成不同的方法：**

```
func chanrecv1(c *hchan, elem unsafe.Pointer) {
    chanrecv(c, elem, true)
}

//go:nosplit
func chanrecv2(c *hchan, elem unsafe.Pointer) (received bool) {
    _, received = chanrecv(c, elem, true)
    return
}
```



#### 关闭

1. • 关闭未初始化过的 channel 会 panic；
2. • 加锁；
3. • 重复关闭 channel 会 panic；
4. • 将阻塞读协程队列中的协程节点统一添加到 glist；
5. • 将阻塞写协程队列中的协程节点统一添加到 glist；
6. • 唤醒 glist 当中的所有协程.
7. 所有阻塞写协程会报panic错误（在写协程被唤醒时判断是因为close被唤醒的），阻塞读协程不会（给一个对应元素的零值），4/5不会同时发生

<img src="https://raw.githubusercontent.com/lanyoumeng/Drawing-bed/main/docs/202409191023957.webp" alt="640" style="zoom:50%;" />





### context

Go 的 context 包主要用于在一组 goroutine 之间传递取消信号、截止时间（deadline）以及一些请求范围内的共享数据（键值对）。它

它利用嵌套的 context（如 cancelCtx、timerCtx 和 valueCtx）构成一颗树，通过关闭只读的 done channel 来广播取消信号，并利用互斥锁保护内部状态，确保并发安全。虽然其值传递（通过 valueCtx）的链式查找效率较低，但在绝大多数请求范围内共享少量数据的场景中完全足够用。这种设计在现代服务端编程中非常实用，帮助开发者有效管理多 goroutine 的生命周期，防止资源泄漏，并实现灵活的超时和取消控制

------

#### 1. Context 接口

context 定义了一个接口，该接口包含四个方法：

- **Deadline() (time.Time, bool)**
   返回截止时间和一个布尔值，表示是否设置了截止时间。
- **Done() <-chan struct{}**
   返回一个只读的 channel，当 context 被取消或超时时，该 channel 会被关闭。通过监听这个 channel，关联的 goroutine 能够及时响应取消信号。
- **Err() error**
   返回 context 被取消的原因，如 `Canceled` 或 `DeadlineExceeded`。
- **Value(key interface{}) interface{}**
   用于获取 context 中存储的键值对，通常用于传递一些请求范围的数据（但不建议用来传递函数参数）。

这些方法都是幂等的，即多次调用返回的结果是一致的。

------

#### 2. 四种主要实现

Go 的 context 包底层主要有四种实现，它们构成了一个类似树状结构的 context 继承关系：

1. **emptyCtx**
    用于实现最基本的空 context，如通过 `context.Background()` 或 `context.TODO()` 获取的上下文。emptyCtx 不支持取消、没有 deadline 也不携带任何键值对，其所有方法都直接返回“空”值（例如 Done 返回 nil
2. **cancelCtx**
    cancelCtx 用于实现通过 `WithCancel` 创建的可取消 context。它嵌入了一个父 context，并维护了一个 **done** channel（懒加载创建），一个 **err** 字段以及一个 **children map**，用于保存所有子 context。当调用取消函数时，cancelCtx 会设置错误信息、关闭 done channel（通过关闭 channel 来广播取消信号），并递归取消所有注册的子 context，确保取消信号沿着树形结构向下传播
3. **timerCtx**
    timerCtx 是 cancelCtx 的一个扩展，它在 cancelCtx 的基础上增加了一个 **deadline** 字段和一个 **time.Timer**。通过 `WithDeadline` 或 `WithTimeout` 函数创建 timerCtx，当当前时间达到 deadline 后，定时器会自动触发取消操作，调用 timerCtx 的 cancel 方法来广播超时取消信号，同时停止定时器以避免资源浪费
4. **valueCtx**
    valueCtx 主要用于通过 `WithValue` 函数在 context 中附加键值对。它将一个键值对与父 context 关联，Value 方法首先会检查当前节点的 key，如果不匹配，则递归向父 context 查找。由于每个 valueCtx 只存储一对键值，因此如果需要传递多个数据，就会形成一条链表，查找时复杂度为 O(n)，这也是不建议大量传递数据到 context 的原因

------

#### 3. 取消和信号传递机制

- **Done 机制**：
   每个实现了取消功能的 context（如 cancelCtx、timerCtx）都有一个 **done** channel，这个 channel 是在首次调用 Done() 时被创建（懒汉式创建）。一旦 context 被取消，done channel 会被关闭。因为在 Go 中，读取一个已关闭的 channel 会立即返回零值（这里是 struct{} 的零值），因此所有在 select 中监听 `<-ctx.Done()` 的 goroutine 都能迅速检测到取消信号，从而执行清理操作并退出。
- **取消传播**：
   当调用由 WithCancel、WithDeadline 或 WithTimeout 返回的取消函数时，内部会调用 cancelCtx 的 cancel 方法，这个方法不仅关闭自身的 done channel，还会遍历 children map，递归地调用每个子 context 的 cancel 方法，实现“广播式”取消。这种设计确保了如果父 context 被取消，其所有子 context 都会同步取消，避免资源泄露
- **与父 context 的关联**：
   在创建可取消的 context 时（例如 WithCancel），会调用 propagateCancel 方法，该方法检查父 context 是否可取消，并将子 context 注册到父 context 的 children 集合中。如果父 context 已经取消，则子 context 会立即被取消；否则，子 context 会等待父 context 的取消事件后，再由父 context 通过 children 列表通知取消

------



## GMP

go 1.19

### 介绍

gmp = goroutine + machine + processor （+ 一套有机组合的机制）

**P的个数默认等于CPU核数，M的个数会略大于P的个数**

IO密集型应用PROCS设置的大一些

g是在用户态

g：

- （1）g 即goroutine，是 golang 中对协程的抽象；
- （2）g 有自己的运行栈、状态、以及执行的任务函数（用户通过 go func 指定）；
- （3）g 需要绑定到 p 才能执行，在 g 的视角中，p 就是它的 cpu.

m：

- （1）m 即 machine，是 golang 中对线程的抽象；

- （2）m 不直接执行 g，而是先和 p 绑定，由其实现代理；

- （3）借由 p 的存在，m 无需和 g 绑死，也无需记录 g 的状态信息，因此 g 在全生命周期中可以实现跨 m 执行.

- 一般情况下M的个数会略大于P的个数，这**多出来的M将会在G产生系统调用时发挥作用。类似线程池，**

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
- P的个数在程序启动时决定，默认情况下**等同于CPU的核数**，由于**M必须持有一个P才可以运行Go代码**，所以同时运行的M个数，也即线程数一般等同于CPU的个数，以达到尽可能的使用CPU而又不至于产生过多的线程切换开销。

 





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

（6）为防止不同P的闲忙差异过大，设立work-stealing机制，本地队列为空的P可以尝试从其他P本地队列**偷取一半**的G补充到自身队列.所以本地队列还是有并发访问需要加锁，但是发生很少接近于无锁化。**偷取时遍历的起点是随机的**



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

------



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

   对于区分堆栈内存来说， **mheap central mcache 都算是堆内存**

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



#### 核心概念

##### 内存单元 mspan

<img src="https://raw.githubusercontent.com/lanyoumeng/Drawing-bed/main/docs/202409032108544.webp" alt="640 (8)" style="zoom:50%;" />

分点阐述 mspan 的特质：

- • mspan 是 Golang 内存管理的最小单元
- • mspan 大小是 page 的整数倍（Go 中的 page 大小为 8KB），且内部的页是连续的（至少在虚拟内存的视角中是这样）
- • 每个 mspan 根据空间大小以及面向分配对象的大小，会被划分为不同的等级（2.2小节展开）
- • 同等级的 mspan 会从属同一个 mcentral，最终会被组织成链表，因此带有前后指针（prev、next）
- • 由于同等级的 mspan 内聚于同一个 mcentral，所以会基于同一把互斥锁管理
- • mspan 会基于 bitMap 辅助快速找到空闲内存块（块大小为对应等级下的 object 大小），此时需要使用到 Ctz64 算法.

<img src="https://raw.githubusercontent.com/lanyoumeng/Drawing-bed/main/docs/202409032109072.webp" alt="640 (9)" style="zoom:50%;" />

mspan 类的源码位于 runtime/mheap.go 文件中：

##### 内存单元等级 spanClass

mspan 根据空间大小和面向分配对象的大小，被划分为 67 种等级（1-67，实际上还有一种隐藏的 0 级，用于处理更大的对象，上不封顶）

 

下表展示了部分的 mspan 等级列表，数据取自 runtime/sizeclasses.go 文件中：

| **class** | **bytes/obj** | **bytes/span** | **objects** | **tail waste** | **max waste** |
| --------- | ------------- | -------------- | ----------- | -------------- | ------------- |
| 1         | 8             | 8192           | 1024        | 0              | 87.50%        |
| 2         | 16            | 8192           | 512         | 0              | 43.75%        |
| 3         | 24            | 8192           | 341         | 8              | 29.24%        |
| 4         | 32            | 8192           | 256         | 0              | 21.88%        |
| ...       |               |                |             |                |               |
| 66        | 28672         | 57344          | 2           | 0              | 4.91%         |
| 67        | 32768         | 32768          | 1           | 0              | 12.50%        |

对上表各列进行解释：

（1）class：mspan 等级标识，1-67

（2）bytes/obj：该大小规格的对象会从这一 mspan 中获取空间. 创建对象过程中，大小会向上取整为 8B 的整数倍，因此该表可以直接实现 object 到 mspan 等级 的映射

（3）bytes/span：该等级的 mspan 的总空间大小

（4）object：该等级的 mspan 最多可以 new 多少个对象，结果等于 （3）/（2）

（5）tail waste：（3）/（2）可能除不尽，于是该项值为（3）%（2）

（6）max waste：通过下面示例解释：

<img src="https://raw.githubusercontent.com/lanyoumeng/Drawing-bed/main/docs/202409032120002.webp" alt="640 (10)" style="zoom:50%;" />

以 class 3 的 mspan 为例，class 分配的 object 大小统一为 24B，由于 object 大小 <= 16B 的会被分配到 class 2 及之前的 class 中，因此只有 17B-24B 大小的 object 会被分配到 class 3.

最不利的情况是，当 object 大小为 17B，会产生浪费空间比例如下：

```
    ((24-17)*341 + 8)/8192 = 0.292358 ≈ 29.24%
```

 

除了上面谈及的根据大小确定的 mspan 等级外，每个 object 还有一个重要的属性叫做 nocan，标识了 object 是否包含指针，在 gc 时是否需要展开标记.

在 Golang 中，会将 span class + nocan 两部分信息组装成一个 uint8，形成完整的 spanClass 标识. 8 个 bit 中，高 7 位表示了上表的 span 等级（总共 67 + 1 个等级，8 个 bit 足够用了），最低位表示 nocan 信息.

代码位于 runtime/mheap.go

##### 线程缓存 mcache

<img src="https://raw.githubusercontent.com/lanyoumeng/Drawing-bed/main/docs/202409032121214.webp" alt="640 (11)" style="zoom:50%;" />

要点：

（1）mcache 是每个 P 独有的缓存，因此交互无锁

（2）mcache 将每种 spanClass 等级的 mspan 各缓存了一个，总数为 2（nocan 维度） * 68（大小维度）= 136

（3）mcache 中还有一个为对象分配器 tiny allocator，用于处理小于 16B 对象的内存分配，在 3.3 小节中详细展开.

代码位于 runtime/mcache.go：

##### 中心缓存 mcentral

<img src="https://raw.githubusercontent.com/lanyoumeng/Drawing-bed/main/docs/202409032121350.webp" alt="640 (12)" style="zoom:50%;" />

要点：

（1）每个 mcentral 对应一种 spanClass

（2）每个 mcentral 下聚合了该 spanClass 下的 mspan

（3）mcentral 下的 mspan 分为两个链表，分别为有空间 mspan 链表 partial 和满空间 mspan 链表 full

（4）每个 mcentral 一把锁

代码位于 runtime/mcentral.go



##### 全局堆缓存 mheap

要点：

- • 对于 Golang 上层应用而言，堆是操作系统虚拟内存的抽象
- • 以页（8KB）为单位，作为最小内存存储单元
- • 负责将连续页组装成 mspan
- • 全局内存基于 bitMap 标识其使用情况，每个 bit 对应一页，为 0 则自由，为 1 则已被 mspan 组装
- • 通过 heapArena 聚合页，记录了页到 mspan 的映射信息（2.7小节展开）
- • 建立空闲页基数树索引 radix tree index，辅助快速寻找空闲页（2.6小节展开）
- • 是 mcentral 的持有者，持有所有 spanClass 下的 mcentral，作为自身的缓存
- • 内存不够时，向操作系统申请，申请单位为 heapArena（64M）

 

代码位于 runtime/mheap.go



#####  空闲页索引 pageAlloc

与 mheap 中，与空闲页寻址分配的基数树索引有关的内容较为晦涩难懂. 网上能把这个问题真正讲清楚的文章几乎没有.

所幸我最后找到这个数据结构的作者发布的笔记，终于对方案的原貌有了大概的了解，这里粘贴链接，供大家自取：https://go.googlesource.com/proposal/+/master/design/35112-scaling-the-page-allocator.md

 

要理清这棵技术树，首先需要明白以下几点：

（1）数据结构背后的含义：

I 2.5 小节有提及，mheap 会基于 bitMap 标识内存中各页的使用情况，bit 位为 0 代表该页是空闲的，为 1 代表该页已被 mspan 占用.

II 每棵基数树聚合了 16 GB 内存空间中各页使用情况的索引信息，用于帮助 mheap 快速找到指定长度的连续空闲页的所在位置

III mheap 持有 2^14 棵基数树，因此索引全面覆盖到 2^14 * 16 GB = 256 T 的内存空间.

 

（2）基数树节点设定

<img src="https://raw.githubusercontent.com/lanyoumeng/Drawing-bed/main/docs/202409032123617.webp" alt="640 (13)" style="zoom:50%;" />

基数树中，每个节点称之为 PallocSum，是一个 uint64 类型，体现了索引的聚合信息，包含以下四部分：

- • start：最右侧 21 个 bit，标识了当前节点映射的 bitMap 范围中首端有多少个连续的 0 bit（空闲页），称之为 start；
- • max：中间 21 个 bit，标识了当前节点映射的 bitMap 范围中最多有多少个连续的 0 bit（空闲页），称之为 max；
- • end：左侧 21 个 bit，标识了当前节点映射的 bitMap 范围中最末端有多少个连续的 0 bit（空闲页），称之为 end.
- • 最左侧一个 bit，弃置不用

 

（3）父子关系

<img src="https://raw.githubusercontent.com/lanyoumeng/Drawing-bed/main/docs/202409032124931.webp" alt="640 (14)" style="zoom:50%;" />

- • 每个父 pallocSum 有 8 个子 pallocSum
- • 根 pallocSum 总览全局，映射的 bitMap 范围为全局的 16 GB 空间（其 max 最大值为 2^21，因此总空间大小为 2^21*8KB=16GB）；
- • 从首层向下是一个依次八等分的过程，每一个 pallocSum 映射其父节点 bitMap 范围的八分之一，因此第二层 pallocSum 的 bitMap 范围为 16GB/8 = 2GB，以此类推，第五层节点的范围为 16GB / (8^4) = 4 MB，已经很小
- • 聚合信息时，自底向上. 每个父 pallocSum 聚合 8 个子 pallocSum 的 start、max、end 信息，形成自己的信息，直到根 pallocSum，坐拥全局 16 GB 的 start、max、end 信息
- • mheap 寻页时，自顶向下. 对于遍历到的每个 pallocSum，先看起 start 是否符合，是则寻页成功；再看 max 是否符合，是则进入其下层孩子 pallocSum 中进一步寻访；最后看 end 和下一个同辈 pallocSum 的 start 聚合后是否满足，是则寻页成功.

<img src="https://raw.githubusercontent.com/lanyoumeng/Drawing-bed/main/docs/202409032124519.webp" alt="640 (15)" style="zoom:50%;" />

<img src="https://raw.githubusercontent.com/lanyoumeng/Drawing-bed/main/docs/202409032124307.webp" alt="640 (16)" style="zoom:50%;" />

代码位于 runtime/mpagealloc.go

##### heapArena

- • 每个 heapArena 包含 8192 个页，大小为 8192 * 8KB = 64 MB
- • heapArena 记录了页到 mspan 的映射. 因为 GC 时，通过地址偏移找到页很方便，但找到其所属的 mspan 不容易. 因此需要通过这个映射信息进行辅助.
- • heapArena 是 mheap 向操作系统申请内存的单位（64MB）

 

代码位于 runtime/mheap.go

#### 对象分配流程

下面来串联 Golang 中分配对象的流程，不论是以下哪种方式，最终都会殊途同归步入 mallocgc 方法中，并且根据 3.1 小节中的策略执行分配流程：

- • new(T)
- • &T{}
- • make(xxxx)

##### 分配流程总览

Golang 中，依据 object 的大小，会将其分为下述三类：

<img src="https://raw.githubusercontent.com/lanyoumeng/Drawing-bed/main/docs/202409032155092.webp" alt="640 (17)" style="zoom:33%;" />

不同类型的对象，会有着不同的分配策略，这些内容在 mallocgc 方法中都有体现.

核心流程类似于读多级缓存的过程，由上而下，每一步只要成功则直接返回. 若失败，则由下层方法兜底.

对于微对象的分配流程：

（1）从 P 专属 mcache 的 tiny 分配器取内存（无锁）

（2）根据所属的 spanClass，从 P 专属 mcache 缓存的 mspan 中取内存（无锁）

（3）根据所属的 spanClass 从对应的 mcentral 中取 mspan 填充到 mcache，然后从 mspan 中取内存（spanClass 粒度锁）

（4）根据所属的 spanClass，从 mheap 的页分配器 pageAlloc 取得足够数量空闲页组装成 mspan 填充到 mcache，然后从 mspan 中取内存（全局锁）

（5）mheap 向操作系统申请内存，更新页分配器的索引信息，然后重复（4）.

 

对于小对象的分配流程是跳过（1）步，执行上述流程的（2）-（5）步；

对于大对象的分配流程是跳过（1）-（3）步，执行上述流程的（4）-（5）步.

![640 (18)](https://raw.githubusercontent.com/lanyoumeng/Drawing-bed/main/docs/202409032155194.webp)

#####  主干方法 mallocgc

先上道硬菜，malloc 方法主干全流程展示.

如果觉得理解曲线太陡峭，可以先跳到后续小节，把拆解的各部分模块都熟悉后，再回过头来总览一遍.

代码位于 runtime/malloc.go 文件中：

```
func mallocgc(size uintptr, typ *_type, needzero bool) unsafe.Pointer {
    // ...    
    // 获取 m
    mp := acquirem()
    // 获取当前 p 对应的 mcache
    c := getMCache(mp)
    var span *mspan
    var x unsafe.Pointer
    // 根据当前对象是否包含指针，标识 gc 时是否需要展开扫描
    noscan := typ == nil || typ.ptrdata == 0
    // 是否是小于 32KB 的微、小对象
    if size <= maxSmallSize {
    // 小于 16 B 且无指针，则视为微对象
        if noscan && size < maxTinySize {
        // tiny 内存块中，从 offset 往后有空闲位置
          off := c.tinyoffset
          // 如果大小为 5 ~ 8 B，size 会被调整为 8 B，此时 8 & 7 == 0，会走进此分支
          if size&7 == 0 {
                // 将 offset 补齐到 8 B 倍数的位置
                off = alignUp(off, 8)
                // 如果大小为 3 ~ 4 B，size 会被调整为 4 B，此时 4 & 3 == 0，会走进此分支  
           } else if size&3 == 0 {
           // 将 offset 补齐到 4 B 倍数的位置
                off = alignUp(off, 4)
                // 如果大小为 1 ~ 2 B，size 会被调整为 2 B，此时 2 & 1 == 0，会走进此分支  
           } else if size&1 == 0 {
            // 将 offset 补齐到 2 B 倍数的位置
                off = alignUp(off, 2)
           }
// 如果当前 tiny 内存块空间还够用，则直接分配并返回
            if off+size <= maxTinySize && c.tiny != 0 {
            // 分配空间
                x = unsafe.Pointer(c.tiny + off)
                c.tinyoffset = off + size
                c.tinyAllocs++
                mp.mallocing = 0
                releasem(mp)  
                return x
            } 
            // 分配一个新的 tiny 内存块
            span = c.alloc[tinySpanClass]    
            // 从 mCache 中获取
            v := nextFreeFast(span)        
            if v == 0 {
            // 从 mCache 中获取失败，则从 mCentral 或者 mHeap 中获取进行兜底
                v, span, shouldhelpgc = c.nextFree(tinySpanClass)
            }   
// 分配空间      
            x = unsafe.Pointer(v)
           (*[2]uint64)(x)[0] = 0
           (*[2]uint64)(x)[1] = 0
           size = maxTinySize
        } else {
          // 根据对象大小，映射到其所属的 span 的等级(0~66）
          var sizeclass uint8
          if size <= smallSizeMax-8 {
              sizeclass = size_to_class8[divRoundUp(size, smallSizeDiv)]
          } else {
              sizeclass = size_to_class128[divRoundUp(size-smallSizeMax, largeSizeDiv)]
          }        
          // 对应 span 等级下，分配给每个对象的空间大小(0~32KB)
          size = uintptr(class_to_size[sizeclass])
          // 创建 spanClass 标识，其中前 7 位对应为 span 的等级(0~66)，最后标识表示了这个对象 gc 时是否需要扫描
          spc := makeSpanClass(sizeclass, noscan) 
          // 获取 mcache 中的 span
          span = c.alloc[spc]  
          // 从 mcache 的 span 中尝试获取空间        
          v := nextFreeFast(span)
          if v == 0 {
          // mcache 分配空间失败，则通过 mcentral、mheap 兜底            
             v, span, shouldhelpgc = c.nextFree(spc)
          }     
          // 分配空间  
          x = unsafe.Pointer(v)
          // ...
       }      
       // 大于 32KB 的大对象      
   } else {
       // 从 mheap 中获取 0 号 span
       span = c.allocLarge(size, noscan)
       span.freeindex = 1
       span.allocCount = 1
       size = span.elemsize         
       // 分配空间   
        x = unsafe.Pointer(span.base())
   }  
   // ...
   return x
}                               
```

#####  步骤（1）：tiny 分配

每个 P 独有的 mache 会有个微对象分配器，基于 offset 线性移动的方式对微对象进行分配，每 16B 成块，对象依据其大小，会向上取整为 2 的整数次幂进行空间补齐，然后进入分配流程.

<img src="https://raw.githubusercontent.com/lanyoumeng/Drawing-bed/main/docs/202409032158761.webp" alt="640 (19)" style="zoom:50%;" />

##### 步骤（2）：mcache 分配

```
          // 根据对象大小，映射到其所属的 span 的等级(0~66）
          var sizeclass uint8
          // get size class ....     
          // 对应 span 等级下，分配给每个对象的空间大小(0~32KB)
          // get span class
          spc := makeSpanClass(sizeclass, noscan) 
          // 获取 mcache 中的 span
          span = c.alloc[spc]  
          // 从 mcache 的 span 中尝试获取空间        
          v := nextFreeFast(span)
          if v == 0 {
          // mcache 分配空间失败，则通过 mcentral、mheap 兜底            
             v, span, shouldhelpgc = c.nextFree(spc)
          }     
          // 分配空间  
          x = unsafe.Pointer(v)
```

 

在 mspan 中，基于 Ctz64 算法，根据 mspan.allocCache 的 bitMap 信息快速检索到空闲的 object 块，进行返回.

代码位于 runtime/malloc.go 文件中：

##### 步骤（3）：mcentral 分配

当 mspan 无可用的 object 内存块时，会步入 mcache.nextFree 方法进行兜底.

代码位于 runtime/mcache.go 文件中：

```
func (c *mcache) nextFree(spc spanClass) (v gclinkptr, s *mspan, shouldhelpgc bool) {
    s = c.alloc[spc]
    // ...
    // 从 mcache 的 span 中获取 object 空位的偏移量
    freeIndex := s.nextFreeIndex()
    if freeIndex == s.nelems {
        // ...
        // 倘若 mcache 中 span 已经没有空位，则调用 refill 方法从 mcentral 或者 mheap 中获取新的 span    
        c.refill(spc)
        // ...
        // 再次从替换后的 span 中获取 object 空位的偏移量
        s = c.alloc[spc]
        freeIndex = s.nextFreeIndex()
    }
    // ...
    v = gclinkptr(freeIndex*s.elemsize + s.base())
    s.allocCount++
    // ...
    return
}    
```

 

倘若 mcache 中，对应的 mspan 空间不足，则会在 mcache.refill 方法中，向更上层的 mcentral 乃至 mheap 获取 mspan，填充到 mache 中:

代码位于 runtime/mcache.go 文件中：

```
func (c *mcache) refill(spc spanClass) {  
    s := c.alloc[spc]
    // ...
    // 从 mcentral 当中获取对应等级的 span
    s = mheap_.central[spc].mcentral.cacheSpan()
    // ...
    // 将新的 span 添加到 mcahe 当中
    c.alloc[spc] = s
}
```

 

mcentral.cacheSpan 方法中，会加锁（spanClass 级别的 sweepLocker），分别从 partial 和 full 中尝试获取有空间的 mspan:

代码位于 runtime/mcentral.go 文件中：

```
func (c *mcentral) cacheSpan() *mspan {
    // ...
    var sl sweepLocker    
    // ...
    sl = sweep.active.begin()
    if sl.valid {
        for ; spanBudget >= 0; spanBudget-- {
            s = c.partialUnswept(sg).pop()
            // ...
            if s, ok := sl.tryAcquire(s); ok {
                // ...
                sweep.active.end(sl)
                goto havespan
            }
            
        // 通过 sweepLock，加锁尝试从 mcentral 的非空链表 full 中获取 mspan
        for ; spanBudget >= 0; spanBudget-- {
            s = c.fullUnswept(sg).pop()
           // ...
            if s, ok := sl.tryAcquire(s); ok {
                // ...
                sweep.active.end(sl)
                goto havespan
                }
                // ...
            }
        }
        // ...
    }
    // ...


    // 执行到此处时，s 已经指向一个存在 object 空位的 mspan 了
havespan:
    // ...
    return
}
```

 

##### 步骤（4）：mheap 分配

在 mcentral.cacheSpan 方法中，倘若从 partial 和 full 中都找不到合适的 mspan 了，则会调用 mcentral 的 grow 方法，将事态继续升级：

```
func (c *mcentral) cacheSpan() *mspan {
    // ...
    // mcentral 中也没有可用的 mspan 了，则需要从 mheap 中获取，最终会调用 mheap_.alloc 方法
    s = c.grow()
   // ...


    // 执行到此处时，s 已经指向一个存在 object 空位的 mspan 了
havespan:
    // ...
    return
}
```

 

经由 mcentral.grow 方法和 mheap.alloc 方法的周转，最终会步入 mheap.allocSpan 方法中：

```
func (c *mcentral) grow() *mspan {
    npages := uintptr(class_to_allocnpages[c.spanclass.sizeclass()])
    size := uintptr(class_to_size[c.spanclass.sizeclass()])


    s := mheap_.alloc(npages, c.spanclass)
    // ...


    // ...
    return s
}
```

 

代码位于 runtime/mheap.go

```
func (h *mheap) alloc(npages uintptr, spanclass spanClass) *mspan {
    var s *mspan
    systemstack(func() {
        // ...
        s = h.allocSpan(npages, spanAllocHeap, spanclass)
    })
    return s
}
```

 

代码位于 runtime/mheap.go

```
func (h *mheap) allocSpan(npages uintptr, typ spanAllocType, spanclass spanClass) (s *mspan) {
    gp := getg()
    base, scav := uintptr(0), uintptr(0)
    
    // ...此处实际上还有一阶缓存，是从每个 P 的页缓存 pageCache 中获取空闲页组装 mspan，此处先略去了...
    
    // 加上堆全局锁
    lock(&h.lock)
    if base == 0 {
        // 通过基数树索引快速寻找满足条件的连续空闲页
        base, scav = h.pages.alloc(npages)
        // ...
    }
    
    // ...
    unlock(&h.lock)


HaveSpan:
    // 把空闲页组装成 mspan
    s.init(base, npages)
    
    // 将这批页添加到 heapArena 中，建立由页指向 mspan 的映射
    h.setSpans(s.base(), npages, s)
    // ...
    return s
}
```

倘若对 mheap 空闲页分配器基数树 pageAlloc 分配空闲页的源码感兴趣，莫慌，3.8 小节见.

 

#####  步骤（5）：向操作系统申请

倘若 mheap 中没有足够多的空闲页了，会发起 mmap 系统调用，向操作系统申请额外的内存空间.

代码位于 runtime/mheap.go 文件的 mheap.grow 方法中：

```
func (h *mheap) grow(npage uintptr) (uintptr, bool) {
    av, asize := h.sysAlloc(ask)
}
```

 

```
func (h *mheap) sysAlloc(n uintptr) (v unsafe.Pointer, size uintptr) {
       v = sysReserve(unsafe.Pointer(p), n)
}
```

 

```
func sysReserve(v unsafe.Pointer, n uintptr) unsafe.Pointer {
    return sysReserveOS(v, n)
}
```

 

```
func sysReserveOS(v unsafe.Pointer, n uintptr) unsafe.Pointer {
    p, err := mmap(v, n, _PROT_NONE, _MAP_ANON|_MAP_PRIVATE, -1, 0)
    if err != 0 {
        return nil
    }
    return p
}
```

 

##### 3.8 步骤（4）拓展：基数树寻页

核心源码位于 runtime/pagealloc.go 的 pageAlloc 方法中，要点都以在代码中给出注释：

```
func (p *pageAlloc) find(npages uintptr) (uintptr, offAddr) {
    // 必须持有堆锁
    assertLockHeld(p.mheapLock)


    // current level.
    i := 0


    // ...
    lastSum := packPallocSum(0, 0, 0)
    lastSumIdx := -1


nextLevel:
    // 1 ~ 5 层依次遍历
    for l := 0; l < len(p.summary); l++ {
        // ...
        // 根据上一层的 index，映射到下一层的 index.
        // 映射关系示例：上层 0 -> 下层 [0~7]
        //             上层 1 -> 下层 [8~15]
        //             以此类推
        i <<= levelBits[l]
        entries := p.summary[l][i : i+entriesPerBlock]
        // ...
        // var levelBits = [summaryLevels]uint{
        //   14,3,3,3,3
        // }
        // 除第一层有 2^14 个节点外，接下来每层都只要关心 8 个 节点.
        // 由于第一层有 2^14 个节点，所以 heap 内存上限为 2^14 * 16G = 256T
        var base, size uint
        for j := j0; j < len(entries); j++ {
            sum := entries[j]
            // ...
            // 倘若当前节点对应内存空间首部即满足，直接返回结果
            s := sum.start()
            if size+s >= uint(npages) {               
                if size == 0 {
                    base = uint(j) << logMaxPages
                }             
                size += s
                break
            }
            // 倘若当前节点对应内存空间首部不满足，但是内部最长连续页满足，则到下一层节点展开搜索
            if sum.max() >= uint(npages) {               
                i += j
                lastSumIdx = i
                lastSum = sum
                continue nextLevel
            }
            // 即便内部最长连续页不满足，还可以尝试将尾部与下个节点的首部叠加，看是否满足
            if size == 0 || s < 1<<logMaxPages {
                                size = sum.end()
                base = uint(j+1)<<logMaxPages - size
                continue
            }
            // The entry is completely free, so continue the run.
            size += 1 << logMaxPages
        }
    
    // 根据 i 和 j 可以推导得到对应的内存地址，进行返回
    ci := chunkIdx(i)
    addr := chunkBase(ci) + uintptr(j)*pageSize
    // ...
    return addr, p.findMappedAddr(firstFree.base)
}
```

### 垃圾回收

#### 背景介绍

垃圾回收（Garbage Collection，简称 GC）是一种内存管理策略，由垃圾收集器以类似守护协程的方式在后台运作，按照既定的策略为用户回收那些不再被使用的对象，释放对应的内存空间.

（1）GC 带来的优势包括：

- • 屏蔽内存回收的细节

拥有 GC 能力的语言能够为用户屏蔽复杂的内存管理工作，使用户更好地聚焦于核心的业务逻辑.

- • 以全局视野执行任务

现代软件工程项目体量与日剧增，一个项目通常由团体协作完成，研发人员负责各自模块的同时，不可避免会涉及到临界资源的使用. 此时由于缺乏全局的视野，手动对内存进行管理无疑会增加开发者的心智负担. 因此，将这部分工作委托给拥有全局视野的垃圾回收模块来完成，方为上上之策.

（2）GC 带来的劣势：

- • 提高了下限但降低了上限

将释放内存的工作委托给垃圾回收模块，研发人员得到了减负，但同时也失去了控制主权. 除了运用有限的GC调优参数外，更多的自由度都被阉割，需要向系统看齐，服从设定.

- • 增加了额外的成本

全局的垃圾回收模块化零为整，会需要额外的状态信息用以存储全局的内存使用情况. 且部分时间需要中断整个程序用以支持垃圾回收工作的执行，这些都是GC额外产生的成本.

（3）GC 的总体评价

除开少量追求极致速度的特殊小规模项目之外，在绝大多数高并发项目中，GC模块都为我们带来了极大的裨益，已经成为一项不可或缺的能力.



#### 经典的垃圾回收算法

##### 标记清扫

<img src="https://raw.githubusercontent.com/lanyoumeng/Drawing-bed/main/docs/202409032229548.webp" alt="640 (20)" style="zoom:50%;" />

标记清扫（Mark-Sweep）算法，分为两步走：

- • 标记：标记出当前还存活的对象
- • 清扫：清扫掉未被标记到的垃圾对象

这是一种类似于排除法的间接处理思路，不直接查找垃圾对象，而是标记存活对象，从而取补集推断出垃圾对象.

至于标记清扫算法的不足之处，通过上图也得以窥见一二，那就是会产生内存碎片. 经过几轮标记清扫之后，空闲的内存块可能零星碎片化分布，此时倘若有大对象需要分配内存，可能会因为内存空间无法化零为整从而导致分配失败.

 

#####  标记压缩

<img src="https://raw.githubusercontent.com/lanyoumeng/Drawing-bed/main/docs/202409032230291.webp" alt="640 (21)" style="zoom:50%;" />

标记压缩（Mark-Compact）算法，是在标记清扫算法的基础上做了升级，在第二步”清扫“的同时还会对存活对象进行压缩整合，使得整体空间更为紧凑，从而解决内存碎片问题.

标记压缩算法在功能性上呈现得很出色，而其存在的缺陷也很简单，就是实现时会有很高的复杂度.

 

##### 半空间复制

<img src="https://raw.githubusercontent.com/lanyoumeng/Drawing-bed/main/docs/202409032230073.webp" alt="640 (22)" style="zoom:50%;" />

相信用过 Java 的同学对半空间复制（Semispace Copy）算法并不会感到陌生，它的核心点如下：

- • 分配两片相等大小的空间，称为 fromspace 和 tospace
- • 每轮只使用 fromspace 空间，以GC作为分水岭划分轮次
- • GC时，将fromspace存活对象转移到tospace中，并以此为契机对空间进行压缩整合
- • GC后，交换fromspace和tospace，开启新的轮次

显然，半空间复制算法应用了以空间换取时间的优化策略，解决了内存碎片的问题，也在一定程度上降低了压缩空间的复杂度. 但其缺点也同样很明显——比较浪费空间.

 

##### 引用计数

<img src="https://raw.githubusercontent.com/lanyoumeng/Drawing-bed/main/docs/202409032231346.webp" alt="640 (23)" style="zoom:50%;" />

引用计数（Reference Counting）算法是很简单高效的：

- • 对象每被引用一次，计数器加1
- • 对象每被删除引用一次，计数器减1
- • GC时，把计数器等于 0 的对象删除

然而，这个朴素的算法存在一个致命的缺陷：无法解决循环引用或者自引用问题.



#### Golang 中的垃圾回收

Golang 在 1.8版本之后，GC策略框架已经奠定，就是**并发三色标记法+混合写屏障机制**



#####  三色标记法

<img src="https://raw.githubusercontent.com/lanyoumeng/Drawing-bed/main/docs/202409032231332.webp" alt="640 (24)" style="zoom:50%;" />

Golang GC 中用到的三色标记法属于标记清扫-算法下的一种实现，由荷兰的计算机科学家 Dijkstra 提出，下面阐述要点：

- • 对象分为三种颜色标记：黑、灰、白
- • 黑对象代表，对象自身存活，且其指向对象都已标记完成
- • 灰对象代表，对象自身存活，但其指向对象还未标记完成
- • 白对象代表，对象尙未被标记到，可能是垃圾对象
- • 标记开始前，将根对象（全局对象、栈上局部变量等）置黑，将其所指向的对象置灰
- • 标记规则是，从灰对象出发，将其所指向的对象都置灰. 所有指向对象都置灰后，当前灰对象置黑
- • 标记结束后，白色对象就是不可达的垃圾对象，需要进行清扫.

 

##### 并发垃圾回收

<img src="https://raw.githubusercontent.com/lanyoumeng/Drawing-bed/main/docs/202409032232863.webp" alt="640 (25)" style="zoom:50%;" />

Golang 1.5 版本是个分水岭，在此之前，GC时需要停止全局的用户协程，专注完成GC工作后，再恢复用户协程，这样做在实现上简单直观，但是会对用户造成不好的体验.

<img src="https://raw.githubusercontent.com/lanyoumeng/Drawing-bed/main/docs/202409032232984.webp" alt="640 (26)" style="zoom:50%;" />

自1.5版本以来，Golang引入了并发垃圾回收机制，允许用户协程和后台的GC协程并发运行，大大地提高了用户体验. 但“并发”是一个值得大家警惕的字眼. 用户协程运行时可能对对象间的引用关系进行调整，这会严重打乱GC三色标记时的标记秩序. 这些问题，我们将在2.3小节展开介绍.

##### 几个问题

###### 漏标问题

<img src="https://raw.githubusercontent.com/lanyoumeng/Drawing-bed/main/docs/202409032233742.webp" alt="640 (27)" style="zoom:50%;" />

漏标问题指的是在用户协程与GC协程并发执行的场景下，部分存活对象未被标记从而被误删的情况. 这一问题产生的过程如下：

- • 条件：初始时刻，对象B持有对象C的引用
- • moment1：GC协程下，对象A被扫描完成，置黑；此时对象B是灰色，还未完成扫描
- • momen2：用户协程下，对象A建立指向对象C的引用
- • moment3：用户协程下，对象B删除指向对象C的引用
- • moment4：GC协程下，开始执行对对象B的扫描

在上述场景中，由于GC协程在B删除C的引用后才开始扫描B，因此无法到达C. 又因为A已经被置黑，不会再重复扫描，因此从扫描结果上看，C是不可达的.

然而事实上，C应该是存活的（被A引用），而GC结束后会因为C仍为白色，因此被GC误删.

漏标问题是无法接受，其引起的误删现象可能会导致程序出现致命的错误**. 针对漏标问题，Golang 给出的解决方案是屏障机制的使用**

 

###### 多标问题

<img src="https://raw.githubusercontent.com/lanyoumeng/Drawing-bed/main/docs/202409032233621.webp" alt="640 (28)" style="zoom:50%;" />

注：这里下面的white可能不存在

多标问题指的是在用户协程与GC协程并发执行的场景下，部分垃圾对象被误标记从而导致GC未按时将其回收的问题. 这一问题产生的过程如下：

- • 条件：初始时刻，对象A持有对象B的引用
- • moment1：GC协程下，对象A被扫描完成，置黑；对象B被对象A引用，因此被置灰
- • momen2：用户协程下，对象A删除指向对象B的引用

上述场景引发的问题是，在事实上，B在被A删除引用后，已成为垃圾对象，但由于其事先已被置灰，因此最终会更新为黑色，不会被GC删除.

错标问题对比于漏标问题而言，是相对可以接受的. 其导致本该被删但仍侥幸存活的对象被称为“浮动垃圾”，至多到下一轮GC，这部分对象就会被GC回收，因此错误可以得到弥补.

 

###### 内存碎片

<img src="https://raw.githubusercontent.com/lanyoumeng/Drawing-bed/main/docs/202409032234541.webp" alt="640 (29)" style="zoom:50%;" />

1.2 小节中有提及，标记清扫算法会存在产生“内存碎片”的缺陷. 那么采用标记清扫算法的Golang GC模块要如何化解这一问题呢？

在笔者一周前发布的文章—— “Golang 内存模型与分配机制”的介绍中已经能够给出这一问题的答复. Golang采用 TCMalloc 机制，依据对象的大小将其归属为到事先划分好的**spanClass**当中，这样能够消解外部碎片的问题，将问题限制在相对可控的内部碎片当中.

基于此，Golang选择采用实现上更为简单的标记清扫算法，避免使用复杂度更高的标记压缩算法，因为在 TCMalloc 框架下，后者带来的优势已经不再明显.

 

###### 为什么不选择分代垃圾回收

<img src="https://raw.githubusercontent.com/lanyoumeng/Drawing-bed/main/docs/202409032234358.webp" alt="640 (30)" style="zoom:50%;" />

分代算法指的是，将对象分为年轻代和老年代两部分（或者更多），采用不同的GC策略进行分类管理. 分代GC算法有效的前提是，绝大多数年轻代对象都是朝生夕死，拥有更高的GC回收率，因此适合采用特别的策略进行处理.

然而Golang中存在内存逃逸机制，会在编译过程中将生命周期更长的对象转移到堆中，将生命周期短的对象分配在栈上，并以栈为单位对这部分对象进行回收.

综上，**内存逃逸机制**减弱了分代算法对Golang GC所带来的优势，考虑分代算法需要产生额外的成本（如不同年代的规则映射、状态管理以及额外的写屏障），Golang 选择不采用分代GC算法.

 

#### 屏障机制

本章介绍屏障有关的内容，主要是为了解决2.3小节中提及的并发GC下的漏标问题.

##### 3.1 强弱三色不变式

漏标问题的本质就是，一个已经扫描完成的黑对象指向了一个被灰\白对象删除引用的白色对象. 构成这一场景的要素拆分如下：

（1）黑色对象指向了白色对象

（2）灰、白对象删除了白色对象

（3）（1）、（2）步中谈及的白色对象是同一个对象

（4）（1）发生在（2）之前

 

一套用于解决漏标问题的方法论称之为强弱三色不变式：

- • 强三色不变式：白色对象不能被黑色对象直接引用（直接破坏（1））
- • 弱三色不变式：白色对象可以被黑色对象引用，但要从某个灰对象出发仍然可达该白对象（间接破坏了（1）、（2）的联动）

 

##### 3.2 插入写屏障

<img src="https://raw.githubusercontent.com/lanyoumeng/Drawing-bed/main/docs/202409032312592.webp" alt="640 (31)" style="zoom:50%;" />

屏障机制类似于一个回调保护机制，指的是在完成某个特定动作前，会先完成屏障成设置的内容.

插入写屏障（Dijkstra）的目标是实现强三色不变式，保证当一个黑色对象指向一个白色对象前，会先触发屏障将白色对象置为灰色，再建立引用.

如果所有流程都能保证做到这一点，那么 3.1 小节中的（1）就会被破坏，漏标问题得以解决.



##### 3.3 删除写屏障

<img src="https://raw.githubusercontent.com/lanyoumeng/Drawing-bed/main/docs/202409032312269.webp" alt="640 (32)" style="zoom:50%;" />

删除写屏障（Yuasa barrier）的目标是实现弱三色不变式，保证当一个白色对象即将被上游删除引用前，会触发屏障将其置灰，之后再删除上游指向其的引用.

这一流程中，3.1小节的步骤（2）会被破坏，漏标问题得以解决.

##### 3.4 混合写屏障

结合3.2 3.3小节来看，插入写屏障、删除写屏障二者择其一，即可解决并发GC的漏标问题，至于错标问题，则采用容忍态度，放到下一轮GC中进行延后处理即可.

然而真实场景中，需要补充一个新的设定——屏障机制无法作用于栈对象.

这是因为栈对象可能涉及频繁的轻量操作，倘若这些高频度操作都需要一一触发屏障机制，那么所带来的成本将是无法接受的.

在这一背景下，单独看插入写屏障或删除写屏障，都无法真正解决漏标问题，除非我们引入额外的Stop the world（STW）阶段，对栈对象的处理进行兜底。

为了消除这个额外的 STW 成本，Golang 1.8 引入了混合写屏障机制，可以视为糅合了插入写屏障+删除写屏障的加强版本，要点如下：

- **• GC 开始前，以栈为单位分批扫描，将栈中所有对象置黑**
- **• GC 期间，栈上新创建对象直接置黑**
- **• 堆对象正常启用插入写屏障**
- **• 堆对象正常启用删除写屏障**

下面我们通过 3.5 小节的几个 show case，来论证混合写屏障机制是否真的能解决并发GC下的各种极端场景问题.

**核心还是在于栈是协程私有独占的，无法被其他协程改变引用的地址，所以一旦被标记为黑色，就不可能还引用着白色对象**



##### 3.5 示例

（1）case 1：堆对象删除引用，栈对象建立引用

<img src="https://raw.githubusercontent.com/lanyoumeng/Drawing-bed/main/docs/202409032313265.webp" alt="640 (33)" style="zoom:33%;" />

- • 背景：存在栈上对象A，黑色（扫描完）；

存在堆上对象B，白色（未被扫描）；

存在堆上对象C，被堆上对象B引用，白色（未被扫描）

- • moment1：A建立对C的引用，由于栈无屏障机制，因此正常建立引用，无额外操作
- • moment2：B尝试删除对C的引用，删除写屏障被触发，C被置灰，因此不会漏标

 

（2）case 2：一个堆对象删除引用，成为另一个堆对象下游

<img src="https://raw.githubusercontent.com/lanyoumeng/Drawing-bed/main/docs/202409032314655.webp" alt="640 (34)" style="zoom:33%;" />

- • 背景：存在堆上对象A，白色（未被扫描）；

存在堆上对象B，黑色（已完成扫描）；

存在堆上对象C，被堆上对象B引用，白色（未被扫描）

- • moment1：B尝试建立对C的引用，插入写屏障被触发，C被置灰
- • moment2：A删除对C的引用，此时C已置灰，因此不会漏标

 

（3）case 3：栈对象删除引用，成为堆对象下游

<img src="https://raw.githubusercontent.com/lanyoumeng/Drawing-bed/main/docs/202409032314152.webp" alt="640 (35)" style="zoom:33%;" />

- • 背景：存在栈上对象A，白色（未完成扫描，说明对应的栈未扫描）；

存在堆上对象B，黑色（已完成扫描）；

存在堆上对象C，被栈上对象A引用，白色（未被扫描）

- • moment1：B尝试建立对C的引用，插入写屏障被触发，C被置灰
- • moment2：A删除对C的引用，此时C已置灰，因此不会漏标

 

（4）case 4：一个栈中对象删除引用，另一个栈中对象建立引用

<img src="https://raw.githubusercontent.com/lanyoumeng/Drawing-bed/main/docs/202409032314201.webp" alt="640 (36)" style="zoom:33%;" />

- • 背景：存在栈上对象A，白色（未扫描，这是因为对应的栈还未开始扫描）；

存在栈上对象B，黑色（已完成扫描，说明对应的栈均已完成扫描）；

存在堆上对象C，被栈上对象A引用，白色（未被扫描）

- • moment1：B建立对C的引用；
- • moment2：A删除对C的引用.
- • 结论：这种场景下，C要么已然被置灰，要么从某个灰对象触发仍然可达C.
- • 原因在于，对象的引用不是从天而降，一定要有个来处. 当前 case 中，对象B能建立指向C的引用，至少需要满足如下三个条件之一：

I 栈对象B原先就持有C的引用，如若如此，C就必然已处于置灰状态（因为B已是黑色）

II 栈对象B持有A的引用，通过A间接找到C. 然而这也是不可能的，因为倘若A能同时被另一个栈上的B引用到，那样A必然会升级到堆中，不再满足作为一个栈对象的前提；

III B同栈内存在其他对象X可达C，此时从X出发，必然存在一个灰色对象，从其出发存在可达C的路线.

综上，我们得以证明混合写屏障是能够胜任并发GC场景的解决方案，并且满足栈无须添加屏障的前提.

### 逃逸分析

在 Go 语言中，**逃逸分析**（Escape Analysis）是编译器用来判断一个变量是否需要分配到堆上而非栈上的一种优化技术。

Go 的逃逸分析是编译器在编译阶段做出的一项重要优化决定，它**确保只有那些确实需要在堆上存活的变量才会逃逸到堆上**，而大部分局部变量仍然分配在栈上，从而使得内存分配和释放更加高效。

#### 主要原理

- **目的**：
   Go 的编译器在编译阶段（主要在 SSA 转换阶段）会分析每个局部变量的使用情况，判断它们是否会“逃逸”出函数的局部作用域。如果一个变量的生命周期超过了函数调用（例如返回一个指针、闭包捕获了局部变量、或者变量被存储在全局数据结构中），那么该变量就会被认为是“逃逸”的，从而分配到堆上；否则，就可以分配在栈上。
- **优势与代价**：
   栈上的变量在函数结束时会自动释放，并且分配和释放的开销非常小；而堆上变量则需要经过垃圾回收（GC）处理，分配和回收的成本更高。所以，逃逸分析直接影响着内存分配策略与程序性能。

#### 分析过程

1. **编译器阶段**：
    在 SSA（静态单赋值）中间代码生成过程中，编译器会分析变量的引用和传递情况。如果发现变量的地址被存储或传递到了一个不会在函数结束后立即失效的地方，就认为该变量“逃逸”，需要在堆上分配。

2. **命令行工具**：
    我们可以通过在编译时使用 `-gcflags="-m"` 选项查看逃逸分析的结果。例如：

   ```
   go build -gcflags="-m" main.go
   ```

   这会输出编译器的逃逸分析报告，显示哪些变量逃逸到了堆上。

3. **常见场景**：

   - 返回局部变量的指针或引用。

   - 在闭包中使用局部变量。

     ```go
     func makeCounter() func() int {
         i := 0 // 局部变量 i 被闭包捕获
         return func() int {
             i++ 
             return i
         }
     }
     
     func main() {
         counter := makeCounter()
         fmt.Println(counter()) // 输出 1
         fmt.Println(counter()) // 输出 2
     }
     ```

   - 将局部变量存入全局数据结构中。

#### 实际影响

- **性能**：
   如果很多局部变量被错误地逃逸到堆上，会增加垃圾回收负担，进而影响性能。因此，编写代码时应注意避免不必要的堆分配。
- **优化建议**：
   在实际开发中，可以利用逃逸分析报告来调整代码结构，尽可能让变量保持在栈上分配，从而提高程序效率。

------





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

------





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



## 包



### 图像处理

`draw.Draw` 是 Go 标准库 `image/draw` 包中的一个函数，用于将一个图像绘制到另一个图像上，可以实现图像的复制、合并、或者在指定区域内覆盖绘制等操作。

函数签名

```
go


复制代码
func Draw(dst Image, r image.Rectangle, src image.Image, sp image.Point, op Op)
```

参数说明

- **`dst`**：目标图像（`draw.Image`），绘制的结果会放到这个图像上。

- **`r`**：目标区域（`image.Rectangle`），指定将 `src` 图像绘制到 `dst` 的哪一个区域。

- **`src`**：源图像（`image.Image`），从该图像中提取像素并绘制到 `dst`。

- **`sp`**：源图像的起点（`image.Point`），即从 `src` 图像中开始读取像素的起始位置。

- `op`

  ：操作模式（

  ```
  draw.Op
  ```

  ），表示绘制时的合成模式。常见的有：

  - `draw.Src`：覆盖模式，直接覆盖目标图像中的像素。
  - `draw.Over`：叠加模式，将源图像和目标图像进行合成。







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



## 底层原理

- • 支持中间件操作（ handlersChain 机制 ）
- • 更方便的使用（ gin.Context ）
- • 更强大的路由解析能力（ radix tree 路由树 ）

### 整体

- • gin 将 Engine 作为 http.Handler 的实现类进行注入，从而融入 Golang net/http 标准库的框架之内
- • gin 中基于 handler 链的方式实现中间件和处理函数的协调使用
- • gin 中基于**压缩前缀树**的方式作为路由树的数据结构，对应于 9 种 http 方法共有 9 棵树
- • gin 中基于 gin.Context 作为一次 http 请求贯穿整条 handler chain 的核心数据结构
- • gin.Context 是一种会被频繁创建销毁的资源对象，因此使用对象池 sync.Pool 进行缓存复用



###  Gin 与 net/http 的关系

Gin 是在 Golang HTTP 标准库 net/http 基础之上的再封装，两者的交互边界如下图：

在 net/http 的既定框架下，gin 所做的是提供了一个 gin.Engine 对象作为 Handler 注入其中，从而实现路由注册/匹配、请求处理链路的优化.

<img src="https://raw.githubusercontent.com/lanyoumeng/Drawing-bed/main/docs/202409241739011.webp" alt="640 (4)" style="zoom:50%;" />

### Gin 框架使用示例

下面提供一段接入 Gin 的示例代码，让大家预先感受一下 Gin 框架的使用风格：

- • 构造 gin.Engine 实例：gin.Default()
- • 路由组注册中间件：Engine.Use()
- • 路由组注册 POST 方法下的 handler：Engine.POST()
- • 启动 http server：Engine.Run()

```go
import "github.com/gin-gonic/gin"


func main() {
    // 创建一个 gin Engine，本质上是一个 http Handler
    mux := gin.Default()
    // 注册中间件
    mux.Use(myMiddleWare)
    // 注册一个 path 为 /ping 的处理函数
    mux.POST("/ping", func(c *gin.Context) {
        c.JSON(http.StatusOK, "pone")
    })
    // 运行 http 服务
    if err := mux.Run(":8080"); err != nil {
        panic(err)
    }
}
```

### 核心数据结构

<img src="https://raw.githubusercontent.com/lanyoumeng/Drawing-bed/main/docs/202409241740456.webp" alt="640 (5)" style="zoom:50%;" />

首先看下核心数据结构：

#### （1）gin.Engine

```
type Engine struct {
   // 路由组
    RouterGroup
    // ...
    // context 对象池
    pool             sync.Pool
    // 方法路由树
    trees            methodTrees
    // ...
}
```

 

```
// net/http 包下的 Handler interface
type Handler interface {
    ServeHTTP(ResponseWriter, *Request)
}


func (engine *Engine) ServeHTTP(w http.ResponseWriter, req *http.Request) {
    // ...
}
```

Engine 为 Gin 中构建的 HTTP Handler，其实现了 net/http 包下 Handler interface 的抽象方法： Handler.ServeHTTP，因此可以作为 Handler 注入到 net/http 的 Server 当中.

Engine包含的核心内容包括：

- • 路由组 RouterGroup：第（2）部分展开
- • Context 对象池 pool：基于 sync.Pool 实现，作为复用 gin.Context 实例的缓冲池. gin.Context 的内容于本文第 5 章详解
- • 路由树数组 trees：共有 9 棵路由树，对应于 9 种 http 方法. 路由树基于压缩前缀树实现，于本文第 4 章详解.

 

9 种 http 方法展示如下：

```
const (
    MethodGet     = "GET"
    MethodHead    = "HEAD"
    MethodPost    = "POST"
    MethodPut     = "PUT"
    MethodPatch   = "PATCH" // RFC 5789
    MethodDelete  = "DELETE"
    MethodConnect = "CONNECT"
    MethodOptions = "OPTIONS"
    MethodTrace   = "TRACE"
)
```

 

#### （2）RouterGroup

```
type RouterGroup struct {
    Handlers HandlersChain
    basePath string
    engine *Engine
    root bool
}
```

RouterGroup 是路由组的概念，其中的配置将被从属于该路由组的所有路由复用：

- • Handlers：路由组共同的 handler 处理函数链. 组下的节点将拼接 RouterGroup 的公用 handlers 和自己的 handlers，组成最终使用的 handlers 链
- • basePath：路由组的基础路径. 组下的节点将拼接 RouterGroup 的 basePath 和自己的 path，组成最终使用的 absolutePath
- • engine：指向路由组从属的 Engine
- • root：标识路由组是否位于 Engine 的根节点. 当用户基于 RouterGroup.Group 方法创建子路由组后，该标识为 false

 

#### （3）HandlersChain

```
type HandlersChain []HandlerFunc


type HandlerFunc func(*Context)
```

HandlersChain 是由多个路由处理函数 HandlerFunc 构成的处理函数链. 在使用的时候，会按照索引的先后顺序依次调用 HandlerFunc.

 

### 2.2 流程入口

下面以创建 gin.Engine 、注册 middleware 和注册 handler 作为主线，进行源码走读和原理解析：

```
func main() {
    // 创建一个 gin Engine，本质上是一个 http Handler
    mux := gin.Default()
    // 注册中间件
    mux.Use(myMiddleWare)
    // 注册一个 path 为 /ping 的处理函数
    mux.POST("/ping", func(c *gin.Context) {
        c.JSON(http.StatusOK, "pone")
    })
    // ...
}
```

 

### 2.3 初始化 Engine

方法调用：gin.Default -> gin.New

- • 创建一个 gin.Engine 实例
- • 创建 Enging 的首个 RouterGroup，对应的处理函数链 Handlers 为 nil，基础路径 basePath 为 "/"，root 标识为 true
- • 构造了 9 棵方法路由树，对应于 9 种 http 方法
- • 创建了 gin.Context 的对象池

路由树相关的内容见本文第 4 章；gin.Context 有关内容见本文第 5 章.

```
func Default() *Engine {
    engine := New()
    // ...
    return engine
}
```

 

```
func New() *Engine {
    // ...
    // 创建 gin Engine 实例
    engine := &Engine{
        // 路由组实例
        RouterGroup: RouterGroup{
            Handlers: nil,
            basePath: "/",
            root:     true,
        },
        // ...
        // 9 棵路由压缩前缀树，对应 9 种 http 方法
        trees:                  make(methodTrees, 0, 9),
        // ...
    }
    engine.RouterGroup.engine = engine     
    // gin.Context 对象池   
    engine.pool.New = func() any {
        return engine.allocateContext(engine.maxParams)
    }
    return engine
}
```

 

### 2.3 注册 middleware

通过 Engine.Use 方法可以实现中间件的注册，会将注册的 middlewares 添加到 RouterGroup.Handlers 中. 后续 RouterGroup 下新注册的 handler 都会在前缀中拼上这部分 group 公共的 handlers.

```
func (engine *Engine) Use(middleware ...HandlerFunc) IRoutes {
    engine.RouterGroup.Use(middleware...)
    // ...
    return engine
}
```

 

```
func (group *RouterGroup) Use(middleware ...HandlerFunc) IRoutes {
    group.Handlers = append(group.Handlers, middleware...)
    return group.returnObj()
}
```

 

### 2.4 注册 handler

<img src="https://raw.githubusercontent.com/lanyoumeng/Drawing-bed/main/docs/202409241745719.webp" alt="640 (6)" style="zoom:50%;" />

以 http post 为例，注册 handler 方法调用顺序为 RouterGroup.POST-> RouterGroup.handle，接下来会完成三个步骤：

- • 拼接出待注册方法的完整路径 absolutePath
- • 拼接出代注册方法的完整处理函数链 handlers
- • 以 absolutePath 和 handlers 组成 kv 对添加到路由树中

 

```
func (group *RouterGroup) POST(relativePath string, handlers ...HandlerFunc) IRoutes {
    return group.handle(http.MethodPost, relativePath, handlers)
}
```

 

```
func (group *RouterGroup) handle(httpMethod, relativePath string, handlers HandlersChain) IRoutes {
    absolutePath := group.calculateAbsolutePath(relativePath)
    handlers = group.combineHandlers(handlers)
    group.engine.addRoute(httpMethod, absolutePath, handlers)
    return group.returnObj()
}
```

 

#### （1）完整路径拼接

结合 RouterGroup 中的 basePath 和注册时传入的 relativePath，组成 absolutePath

```
func (group *RouterGroup) calculateAbsolutePath(relativePath string) string {
    return joinPaths(group.basePath, relativePath)
}
```

 

```
func joinPaths(absolutePath, relativePath string) string {
    if relativePath == "" {
        return absolutePath
    }


    finalPath := path.Join(absolutePath, relativePath)
    if lastChar(relativePath) == '/' && lastChar(finalPath) != '/' {
        return finalPath + "/"
    }
    return finalPath
}
```

 

#### （2）完整 handlers 生成

深拷贝 RouterGroup 中 handlers 和注册传入的 handlers，生成新的 handlers 数组并返回

 

```
func (group *RouterGroup) combineHandlers(handlers HandlersChain) HandlersChain {
    finalSize := len(group.Handlers) + len(handlers)
    assert1(finalSize < int(abortIndex), "too many handlers")
    mergedHandlers := make(HandlersChain, finalSize)
    copy(mergedHandlers, group.Handlers)
    copy(mergedHandlers[len(group.Handlers):], handlers)
    return mergedHandlers
}
```

 

#### （3）注册 handler 到路由树

- • 获取 http method 对应的 methodTree
- • 将 absolutePath 和对应的 handlers 注册到 methodTree 中

路由注册方法 root.addRoute 的信息量比较大，放在本文第 4 章中详细拆解.

```
func (engine *Engine) addRoute(method, path string, handlers HandlersChain) {
    // ...
    root := engine.trees.get(method)
    if root == nil {
        root = new(node)
        root.fullPath = "/"
        engine.trees = append(engine.trees, methodTree{method: method, root: root})
    }
    root.addRoute(path, handlers)
    // ...
}
```

 

### 3 启动服务流程

#### 3.1 流程入口

下面通过 Gin 框架运行 http 服务为主线，进行源码走读：

```
func main() {
    // 创建一个 gin Engine，本质上是一个 http Handler
    mux := gin.Default()
    
    // 一键启动 http 服务
    if err := mux.Run(); err != nil{
        panic(err)
    }
}
```

 

#### 3.2 启动服务

一键启动 Engine.Run 方法后，底层会将 gin.Engine 本身作为 net/http 包下 Handler interface 的实现类，并调用 http.ListenAndServe 方法启动服务.

```
func (engine *Engine) Run(addr ...string) (err error) {
    // ...
    err = http.ListenAndServe(address, engine.Handler())
    return
}
```

 

顺便多提一嘴，ListenerAndServe 方法本身会基于主动轮询 + IO 多路复用的方式运行，因此程序在正常运行时，会始终阻塞于 Engine.Run 方法，不会返回.



<img src="https://raw.githubusercontent.com/lanyoumeng/Drawing-bed/main/docs/202409241745134.webp" alt="640 (7)" style="zoom:50%;" />

```
func (srv *Server) Serve(l net.Listener) error {
   // ...
   ctx := context.WithValue(baseCtx, ServerContextKey, srv)
    for {
        rw, err := l.Accept()
        // ...
        connCtx := ctx
        // ...
        c := srv.newConn(rw)
        // ...
        go c.serve(connCtx)
    }
}
```

 

#### 3.3 处理请求

在服务端接收到 http 请求时，会通过 Handler.ServeHTTP 方法进行处理. 而此处的 Handler 正是 gin.Engine，其处理请求的核心步骤如下：

- • 对于每笔 http 请求，会为其分配一个 gin.Context，在 handlers 链路中持续向下传递
- • 调用 Engine.handleHTTPRequest 方法，从路由树中获取 handlers 链，然后遍历调用
- • 处理完 http 请求后，会将 gin.Context 进行回收. 整个回收复用的流程基于对象池管理

```
func (engine *Engine) ServeHTTP(w http.ResponseWriter, req *http.Request) {
    // 从对象池中获取一个 context
    c := engine.pool.Get().(*Context)
    
    // 重置/初始化 context
    c.writermem.reset(w)
    c.Request = req
    c.reset()
    
    // 处理 http 请求
    engine.handleHTTPRequest(c)


    // 把 context 放回对象池
    engine.pool.Put(c)
}
```



<img src="https://raw.githubusercontent.com/lanyoumeng/Drawing-bed/main/docs/202409241746012.webp" alt="640 (8)" style="zoom:50%;" />

Engine.handleHTTPRequest 方法核心步骤分为三步：

- • 根据 http method 取得对应的 methodTree
- • 根据 path 从 methodTree 中找到对应的 handlers 链
- • 将 handlers 链注入到 gin.Context 中，通过 Context.Next 方法按照顺序遍历调用 handler

此处根据 path 从路由树寻找 handlers 的逻辑位于 root.getValue 方法中，和路由树数据结构有关，放在本文第 4 章详解；

根据 gin.Context.Next 方法遍历 handler 链的内容放在本文第 5 章详解.

```
func (engine *Engine) handleHTTPRequest(c *Context) {
    httpMethod := c.Request.Method
    rPath := c.Request.URL.Path
    
    // ...
    t := engine.trees
    for i, tl := 0, len(t); i < tl; i++ {
        // 获取对应的方法树
        if t[i].method != httpMethod {
            continue
        }
        root := t[i].root
        // 从路由树中寻找路由
        value := root.getValue(rPath, c.params, c.skippedNodes, unescape)
        if value.params != nil {
            c.Params = *value.params
        }
        if value.handlers != nil {
            c.handlers = value.handlers
            c.fullPath = value.fullPath
            c.Next()
            c.writermem.WriteHeaderNow()
            return
        }
        // ...
        break
    }
    // ...
}
```

 

### 4 Gin的路由树

#### 4.1 策略与原理

<img src="https://raw.githubusercontent.com/lanyoumeng/Drawing-bed/main/docs/202409241746188.webp" alt="640 (9)" style="zoom:50%;" />

在聊 Gin 路由树实现原理之前，需要先补充一个压缩前缀树 radix tree 的基础设定.

##### （1）前缀树

前缀树又称 trie 树，是一种基于字符串公共前缀构建索引的树状结构，核心点包括：

- • 除根节点之外，每个节点对应一个字符
- • 从根节点到某一节点，路径上经过的字符串联起来，即为该节点对应的字符串
- • 尽可能复用公共前缀，如无必要不分配新的节点

tries 树在 leetcode 上的题号为 208，大家感兴趣不妨去刷刷算法题，手动实现一下.

 

##### （2）压缩前缀树

压缩前缀树又称基数树或 radix 树，是对前缀树的改良版本，优化点主要在于空间的节省，核心策略体现在：

**倘若某个子节点是其父节点的唯一孩子，则与父节点进行合并**

 

在 gin 框架中，使用的正是压缩前缀树的数据结构.

 

##### （3）为什么使用压缩前缀树

与压缩前缀树相对的就是使用 hashmap，以 path 为 key，handlers 为 value 进行映射关联，这里选择了前者的原因在于：

- • path 匹配时不是完全精确匹配，比如末尾 ‘/’ 符号的增减、全匹配符号 '*' 的处理等，map 无法胜任（模糊匹配部分的代码于本文中并未体现，大家可以深入源码中加以佐证）
- • 路由的数量相对有限，对应数量级下 map 的性能优势体现不明显，在小数据量的前提下，map 性能甚至要弱于前缀树
- • path 串通常存在基于分组分类的公共前缀，适合使用前缀树进行管理，可以节省存储空间

 

##### （4）补偿策略

在 Gin 路由树中还使用一种补偿策略，在组装路由树时，会将注册路由句柄数量更多的 child node 摆放在 children 数组更靠前的位置.

这是因为某个链路注册的 handlers 句柄数量越多，一次匹配操作所需要花费的时间就越长，且被匹配命中的概率就越大，因此应该被优先处理.

<img src="https://raw.githubusercontent.com/lanyoumeng/Drawing-bed/main/docs/202409241747749.webp" alt="640 (10)" style="zoom:50%;" />

#### 核心数据结构

<img src="https://raw.githubusercontent.com/lanyoumeng/Drawing-bed/main/docs/202409241747111.webp" alt="640 (11)" style="zoom:50%;" />

下面聊一下路由树的数据结构，对应于 9 种 http method，共有 9 棵 methodTree. 每棵 methodTree 会通过 root 指向 radix tree 的根节点.

 

```
type methodTree struct {
    method string
    root   *node
}
```

 

node 是 radix tree 中的节点，对应节点含义如下：

- • path：节点的相对路径，拼接上 RouterGroup 中的 basePath 作为前缀后才能拿到完整的路由 path
- • indices：由各个子节点 path 首字母组成的字符串，子节点顺序会按照途径的路由数量 priority进行排序
- • priority：途径本节点的路由数量，反映出本节点在父节点中被检索的优先级
- • children：子节点列表
- • handlers：当前节点对应的处理函数链

 

```
type node struct {
    // 节点的相对路径
    path string
    // 每个 indice 字符对应一个孩子节点的 path 首字母
    indices string
    // ...
    // 后继节点数量
    priority uint32
    // 孩子节点列表
    children []*node 
    // 处理函数链
    handlers HandlersChain
    // path 拼接上前缀后的完整路径
    fullPath string
}
```

 

#### 4.3 注册到路由树

承接本文 2.4 小节第（3）部分，下述代码展示了将一组 path + handlers 添加到 radix tree 的详细过程，核心位置均已给出注释，此处就不再赘述了，请大家尽情享用源码盛宴吧！

```
// 插入新路由
func (n *node) addRoute(path string, handlers HandlersChain) {
    fullPath := path
    // 每有一个新路由经过此节点，priority 都要加 1
    n.priority++


    // 加入当前节点为 root 且未注册过子节点，则直接插入由并返回
    if len(n.path) == 0 && len(n.children) == 0 {
        n.insertChild(path, fullPath, handlers)
        n.nType = root
        return
    }


// 外层 for 循环断点
walk:
    for {
        // 获取 node.path 和待插入路由 path 的最长公共前缀长度
        i := longestCommonPrefix(path, n.path)
    
        // 倘若最长公共前缀长度小于 node.path 的长度，代表 node 需要分裂
        // 举例而言：node.path = search，此时要插入的 path 为 see
        // 最长公共前缀长度就是 2，len(n.path) = 6
        // 需要分裂为  se -> arch
                        -> e    
        if i < len(n.path) {
        // 原节点分裂后的后半部分，对应于上述例子的 arch 部分
            child := node{
                path:      n.path[i:],
                // 原本 search 对应的参数都要托付给 arch
                indices:   n.indices,
                children: n.children,              
                handlers:  n.handlers,
                // 新路由 see 进入时，先将 search 的 priority 加 1 了，此时需要扣除 1 并赋给 arch
                priority:  n.priority - 1,
                fullPath:  n.fullPath,
            }


            // 先建立 search -> arch 的数据结构，后续调整 search 为 se
            n.children = []*node{&child}
            // 设置 se 的 indice 首字母为 a
            n.indices = bytesconv.BytesToString([]byte{n.path[i]})
            // 调整 search 为 se
            n.path = path[:i]
            // search 的 handlers 都托付给 arch 了，se 本身没有 handlers
            n.handlers = nil           
            // ...
        }


        // 最长公共前缀长度小于 path，正如 se 之于 see
        if i < len(path) {
            // path see 扣除公共前缀 se，剩余 e
            path = path[i:]
            c := path[0]            


            // 根据 node.indices，辅助判断，其子节点中是否与当前 path 还存在公共前缀       
            for i, max := 0, len(n.indices); i < max; i++ {
               // 倘若 node 子节点还与 path 有公共前缀，则令 node = child，并调到外层 for 循环 walk 位置开始新一轮处理
                if c == n.indices[i] {                   
                    i = n.incrementChildPrio(i)
                    n = n.children[i]
                    continue walk
                }
            }
            
            // node 已经不存在和 path 再有公共前缀的子节点了，则需要将 path 包装成一个新 child node 进行插入      
            // node 的 indices 新增 path 的首字母    
            n.indices += bytesconv.BytesToString([]byte{c})
            // 把新路由包装成一个 child node，对应的 path 和 handlers 会在 node.insertChild 中赋值
            child := &node{
                fullPath: fullPath,
            }
            // 新 child node append 到 node.children 数组中
            n.addChild(child)
            n.incrementChildPrio(len(n.indices) - 1)
            // 令 node 指向新插入的 child，并在 node.insertChild 方法中进行 path 和 handlers 的赋值操作
            n = child          
            n.insertChild(path, fullPath, handlers)
            return
        }


        // 此处的分支是，path 恰好是其与 node.path 的公共前缀，则直接复制 handlers 即可
        // 例如 se 之于 search
        if n.handlers != nil {
            panic("handlers are already registered for path '" + fullPath + "'")
        }
        n.handlers = handlers
        // ...
        return
}  
```

 

```
func (n *node) insertChild(path string, fullPath string, handlers HandlersChain) {
    // ...
    n.path = path
    n.handlers = handlers
    // ...
}
```

 

呼应于 4.1 小节第（4）部分谈到的补偿策略，下面这段代码体现了，在每个 node 的 children 数组中，child node 在会依据 priority 有序排列，保证 priority 更高的 child node 会排在数组前列，被优先匹配.

```
func (n *node) incrementChildPrio(pos int) int {
    cs := n.children
    cs[pos].priority++
    prio := cs[pos].priority




    // Adjust position (move to front)
    newPos := pos
    for ; newPos > 0 && cs[newPos-1].priority < prio; newPos-- {
        // Swap node positions
        cs[newPos-1], cs[newPos] = cs[newPos], cs[newPos-1]
    }




    // Build new index char string
    if newPos != pos {
        n.indices = n.indices[:newPos] + // Unchanged prefix, might be empty
            n.indices[pos:pos+1] + // The index char we move
            n.indices[newPos:pos] + n.indices[pos+1:] // Rest without char at 'pos'
    }




    return newPos
}
```

 

#### 4.4 检索路由树

承接本文 3.3 小节，下述代码展示了从路由树中匹配 path 对应 handler 的详细过程，请大家结合注释消化源码吧.

 

```
type nodeValue struct {
    // 处理函数链
    handlers HandlersChain
    // ...
}
```

 

```
// 从路由树中获取 path 对应的 handlers 
func (n *node) getValue(path string, params *Params, skippedNodes *[]skippedNode, unescape bool) (value nodeValue) {
    var globalParamsCount int16


// 外层 for 循环断点
walk: 
    for {
        prefix := n.path
        // 待匹配 path 长度大于 node.path
        if len(path) > len(prefix) {
            // node.path 长度 < path，且前缀匹配上
            if path[:len(prefix)] == prefix {
                // path 取为后半部分
                path = path[len(prefix):]
                // 遍历当前 node.indices，找到可能和 path 后半部分可能匹配到的 child node
                idxc := path[0]
                for i, c := range []byte(n.indices) {
                    // 找到了首字母匹配的 child node
                    if c == idxc {
                        // 将 n 指向 child node，调到 walk 断点开始下一轮处理
                        n = n.children[i]
                        continue walk
                    }
                }


                // ...
            }
        }


        // 倘若 path 正好等于 node.path，说明已经找到目标
        if path == prefix {
            // ...
            // 取出对应的 handlers 进行返回 
            if value.handlers = n.handlers; value.handlers != nil {
                value.fullPath = n.fullPath
                return
            }


            // ...           
        }


        // 倘若 path 与 node.path 已经没有公共前缀，说明匹配失败，会尝试重定向，此处不展开
        // ...
 }  
```

 

### 5 Gin.Context

#### 5.1 核心数据结构

<img src="https://raw.githubusercontent.com/lanyoumeng/Drawing-bed/main/docs/202409241748910.webp" alt="640 (12)" style="zoom:50%;" />

gin.Context 的定位是对应于一次 http 请求，贯穿于整条 handlersChain 调用链路的上下文，其中包含了如下核心字段：

- • Request/Writer：http 请求和响应的 reader、writer 入口
- • handlers：本次 http 请求对应的处理函数链
- • index：当前的处理进度，即处理链路处于函数链的索引位置
- • engine：Engine 的指针
- • mu：用于保护 map 的读写互斥锁
- • Keys：缓存 handlers 链上共享数据的 map

```
type Context struct {
    // ...
    // http 请求参数
    Request   *http.Request
    // http 响应 writer
    Writer    ResponseWriter
    // ...
    // 处理函数链
    handlers HandlersChain
    // 当前处于处理函数链的索引
    index    int8
    engine       *Engine
    // ...
    // 读写锁，保证并发安全
    mu sync.RWMutex
    // key value 对存储 map
    Keys map[string]any
    // ..
}
```

 

#### 5.2 复用策略

<img src="https://raw.githubusercontent.com/lanyoumeng/Drawing-bed/main/docs/202409241748361.webp" alt="640 (13)" style="zoom:50%;" />

gin.Context 作为处理 http 请求的通用数据结构，不可避免地会被频繁创建和销毁. 为了缓解 GC 压力，gin 中采用对象池 sync.Pool 进行 Context 的缓存复用，处理流程如下：

- • http 请求到达时，从 pool 中获取 Context，倘若池子已空，通过 pool.New 方法构造新的 Context 补上空缺
- • http 请求处理完成后，将 Context 放回 pool 中，用以后续复用

sync.Pool 并不是真正意义上的缓存，将其称为回收站或许更加合适，放入其中的数据在逻辑意义上都是已经被删除的，但在物理意义上数据是仍然存在的，这些数据可以存活两轮 GC 的时间，在此期间倘若有被获取的需求，则可以被重新复用.

和对象池 sync.Pool 有关的内容可以阅读我的文章 《Golang 协程池 Ants 实现原理》，其中有将对象池作为协程池应用组件的前置知识点，进行详细讲解.

 

```
type Engine struct {
    // context 对象池
    pool             sync.Pool
}
```

 

```
func New() *Engine {
    // ...
    engine.pool.New = func() any {
        return engine.allocateContext(engine.maxParams)
    }
    return engine
}
```

 

```
func (engine *Engine) allocateContext(maxParams uint16) *Context {
    v := make(Params, 0, maxParams)
   // ...
    return &Context{engine: engine, params: &v, skippedNodes: &skippedNodes}
}
```

 

#### 5.3 分配与回收时机

<img src="https://raw.githubusercontent.com/lanyoumeng/Drawing-bed/main/docs/202409241749816.webp" alt="640 (14)" style="zoom:50%;" />

gin.Context 分配与回收的时机是在 gin.Engine 处理 http 请求的前后，位于 Engine.ServeHTTP 方法当中：

- • 从池中获取 Context
- • 重置 Context 的内容，使其成为一个空白的上下文
- • 调用 Engine.handleHTTPRequest 方法处理 http 请求
- • 请求处理完成后，将 Context 放回池中

```
func (engine *Engine) ServeHTTP(w http.ResponseWriter, req *http.Request) {
    // 从对象池中获取一个 context
    c := engine.pool.Get().(*Context)
    // 重置/初始化 context
    c.writermem.reset(w)
    c.Request = req
    c.reset()
    // 处理 http 请求
    engine.handleHTTPRequest(c)
    
    // 把 context 放回对象池
    engine.pool.Put(c)
}
```

 

#### 5.4 使用时机

##### （1）handlesChain 入口

在 Engine.handleHTTPRequest 方法处理请求时，会通过 path 从 methodTree 中获取到对应的 handlers 链，然后将 handlers 注入到 Context.handlers 中，然后启动 Context.Next 方法开启 handlers 链的遍历调用流程.

```
func (engine *Engine) handleHTTPRequest(c *Context) {
    // ...
    t := engine.trees
    for i, tl := 0, len(t); i < tl; i++ {
        if t[i].method != httpMethod {
            continue
        }
        root := t[i].root        
        value := root.getValue(rPath, c.params, c.skippedNodes, unescape)
        // ...
        if value.handlers != nil {
            c.handlers = value.handlers
            c.fullPath = value.fullPath
            c.Next()
            c.writermem.WriteHeaderNow()
            return
        }
        // ...
    }
    // ...
}
```

 

##### （2）handlesChain 遍历调用

<img src="https://raw.githubusercontent.com/lanyoumeng/Drawing-bed/main/docs/202409241749967.webp" alt="640 (15)" style="zoom:50%;" />

推进 handlers 链调用进度的方法正是 Context.Next. 可以看到其中以 Context.index 为索引，通过 for 循环依次调用 handlers 链中的 handler.

```
func (c *Context) Next() {
    c.index++
    for c.index < int8(len(c.handlers)) {
        c.handlers[c.index](c)
        c.index++
    }
}
```

 

由于 Context 本身会暴露于调用链路中，因此用户可以在某个 handler 中通过手动调用 Context.Next 的方式来打断当前 handler 的执行流程，提前进入下一个 handler 的处理中.

由于此时本质上是一个方法压栈调用的行为，因此在后置位 handlers 链全部处理完成后，最终会回到压栈前的位置，执行当前 handler 剩余部分的代码逻辑.

 <img src="https://raw.githubusercontent.com/lanyoumeng/Drawing-bed/main/docs/202409241749872.webp" alt="640 (16)" style="zoom:50%;" />

结合下面的代码示例来说，用户可以在某个 handler 中，于调用 Context.Next 方法的前后分别声明前处理逻辑和后处理逻辑，这里的“前”和“后”相对的是后置位的所有 handler 而言.

```
func myHandleFunc(c *gin.Context){
    // 前处理
    preHandle()  
    c.Next()
    // 后处理
    postHandle()
}
```

 

此外，用户可以在某个 handler 中通过调用 Context.Abort 方法实现 handlers 链路的提前熔断.

其实现原理是将 Context.index 设置为一个过载值 63，导致 Next 流程直接终止. 这是因为 handlers 链的长度必须小于 63，否则在注册时就会直接 panic. 因此在 Context.Next 方法中，一旦 index 被设为 63，则必然大于整条 handlers 链的长度，for 循环便会提前终止.

```
const abortIndex int8 = 63


func (c *Context) Abort() {
    c.index = abortIndex
}
```

 

此外，用户还可以通过 Context.IsAbort 方法检测当前 handlerChain 是出于正常调用，还是已经被熔断.

```
func (c *Context) IsAborted() bool {
    return c.index >= abortIndex
}
```

 

注册 handlers，倘若 handlers 链长度达到 63，则会 panic

```
func (group *RouterGroup) combineHandlers(handlers HandlersChain) HandlersChain {
    finalSize := len(group.Handlers) + len(handlers)
    // 断言 handlers 链长度必须小于 63
    assert1(finalSize < int(abortIndex), "too many handlers")
    // ...
}
```

 

##### （3）共享数据存取

<img src="https://raw.githubusercontent.com/lanyoumeng/Drawing-bed/main/docs/202409241750823.webp" alt="640 (17)" style="zoom:50%;" />

gin.Context 作为 handlers 链的上下文，还提供对外暴露的 Get 和 Set 接口向用户提供了共享数据的存取服务，相关操作都在读写锁的保护之下，能够保证并发安全.

 

```
type Context struct {
    // ...
    // 读写锁，保证并发安全
    mu sync.RWMutex


    // key value 对存储 map
    Keys map[string]any
}
```

 

```
func (c *Context) Get(key string) (value any, exists bool) {
    c.mu.RLock()
    defer c.mu.RUnlock()
    value, exists = c.Keys[key]
    return
}
```

 

```
func (c *Context) Set(key string, value any) {
    c.mu.Lock()
    defer c.mu.Unlock()
    if c.Keys == nil {
        c.Keys = make(map[string]any)
    }


    c.Keys[key] = value
}
```

 



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

3. 如果你使用 db.First() 查询单条记录，当没有记录时会返回 gorm.ErrRecordNotFound 错误；

   而使用 db.Find() 查询切片时，即使没有记录也不会报错，只会返回空的切片。

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

- **简单**：不过度设计，代码平实简单；
- **通用**：通用业务开发所需要的基础库的功能；
- **高效**：提高业务迭代的效率；
- **稳定**：基础库可测试性高，覆盖率高，有线上实践安全可靠；
- **健壮**：通过良好的基础库设计，减少错用；
- **高性能**：性能高，但不特定为了性能做 hack 优化，引入 unsafe ；
- **扩展性**：良好的接口设计，来扩展实现，或者通过新增基础库目录来扩展功能；
- **容错性**：为失败设计，大量引入对 SRE 的理解，鲁棒性高；
- **工具链**：包含大量工具链，比如 cache 代码生成，lint 工具等等；

特性

- **APIs**：协议通信以 HTTP/gRPC 为基础，通过 Protobuf 进行定义；
- **Errors**：通过 Protobuf 的 Enum 作为错误码定义，以及工具生成判定接口；
- **Metadata**：在协议通信 HTTP/gRPC 中，通过 Middleware 规范化服务元信息传递；
- **Config**：支持多数据源方式，进行配置合并铺平，通过 Atomic 方式支持动态配置；
- **Logger**：标准日志接口，可方便集成三方 log 库，并可通过 fluentd 收集日志；
- **Metrics**：统一指标接口，可以实现各种指标系统，默认集成 Prometheus；
- **Tracing**：遵循 OpenTelemetry 规范定义，以实现微服务链路追踪；
- **Encoding**：支持 Accept 和 Content-Type 进行自动选择内容编码；
- **Transport**：通用的 HTTP/gRPC 传输层，实现统一的 Middleware 插件支持；
- **Registry**：实现统一注册中心接口，可插件化对接各种注册中心；

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
kratos proto server api/video/v1/video.proto -t internal/service
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







