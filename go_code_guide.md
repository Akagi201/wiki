# Go编码规范指南

##序言

看过很多方面的编码规范, 可能每一家公司都有不同的规范, 这份编码规范是写给我自己的, 同时希望我们公司内部同事也能遵循这个规范来写Go代码.

如果你的代码没有办法找到下面的规范, 那么就遵循标准库的规范, 多阅读标准库的源码, 标准库的代码可以说是我们写代码参考的标杆.

## 格式化规范

`go`默认已经有了`gofmt`工具, 但是我们强烈建议使用`goimport`工具, 这个在`gofmt`的基础上增加了自动删除和引入包.

`go get golang.org/x/tools/cmd/goimports`

不同的编辑器有不同的配置, `sublime`的配置教程: <http://michaelwhatcott.com/gosublime-goimports/>

`LiteIDE`默认已经支持了`goimports`, 如果你的不支持请点击`属性配置->golangfmt->勾选goimports`

保存之前自动`fmt`你的代码.

## 行长约定

一行最长不超过80个字符, 超过的请使用换行展示, 尽量保持格式优雅.

## `go vet`

`vet`工具可以帮我们静态分析我们的源码存在的各种问题, 例如多余的代码, 提前return的逻辑, `struct`的`tag`是否符合标准等.

`go get golang.org/x/tools/cmd/vet`

使用如下:

`go vet .`

## `package`名字

保持`package`的名字和目录保持一致, 尽量采取有意义的包名, 简短, 有意义, 尽量和标准库不要冲突.

## `import`规范

`import`在多行的情况下, `goimports`会自动帮你格式化, 但是我们这里还是规范一下`import`的一些规范, 如果你在一个文件里面引入了一个`package`, 还是建议采用如下格式:

```
import (
  "fmt"
)
```

如果你的包引入了三种类型的包, 标准库包, 程序内部包, 第三方包, 建议采用如下方式进行组织你的包:

```
import (
    "encoding/json"
    "strings"

    "myproject/models"
    "myproject/controller"
    "myproject/utils"

    "github.com/astaxie/beego"
    "github.com/go-sql-driver/mysql"
)
```

有顺序的引入包, 不同的类型采用空格分离, 第一种实标准库, 第二是项目包, 第三是第三方包.

在项目中不要使用相对路径引入包:

```
// 这是不好的导入
import “../net”

// 这是正确的做法
import “github.com/repo/proj/src/net”
```

## 变量申明

变量名采用驼峰标准, 不要使用`_`来命名变量名, 多个变量申明放在一起

```
var (
  Found bool
  count int
)
```

在函数外部申明必须使用`var`, 不要采用`:=`, 容易踩到变量的作用域的问题.

## 自定义类型的`string`循环问题

如果自定义的类型定义了`String`方法, 那么在打印的时候会产生隐藏的一些bug

```
type MyInt int
func (m MyInt) String() string {
  return fmt.Sprint(m)   //BUG:死循环
}

func(m MyInt) String() string {
  return fmt.Sprint(int(m))   //这是安全的,因为我们内部进行了类型转换
}
```

## 避免返回命名的参数

如果你的函数很短小, 少于10行代码, 那么可以使用, 不然请直接使用类型, 因为如果使用命名变量很容易引起隐藏的bug

```
func Foo(a int, b int) (string, ok){

}
```

当然如果是有多个相同类型的参数返回, 那么命名参数可能更清晰:

```
func (f *Foo) Location() (float64, float64, error)
```

下面的代码就更清晰了:

```
// Location returns f's latitude and longitude.
// Negative values mean south and west, respectively.
func (f *Foo) Location() (lat, long float64, err error)
```

官方建议: 最好命名返回值, 因为不命名返回值, 虽然使得代码更加简洁了, 但是会造成生成的文档可读性差.

## 错误处理

错误处理的原则就是不能丢弃任何有返回`err`的调用, 不要采用`_`丢弃, 必须全部处理.

接收到错误, 要么返回`err`, 要么实在不行就`panic`, 或者使用`log`记录下来.

## `error`信息

`error`的信息不要采用大写字母, 尽量保持你的错误简短, 但是要足够表达你的错误的意思.

## 长句子打印或者调用, 使用参数进行格式化分行

我们在调用`fmt.Sprint`或者`log.Sprint`之类的函数时, 有时候会遇到很长的句子, 我们需要在参数调用处进行多行分割:

下面是错误的方式:

```
log.Printf(“A long format string: %s %d %d %s”, myStringParameter, len(a),
  expected.Size, defrobnicate(“Anotherlongstringparameter”,
      expected.Growth.Nanoseconds() /1e6))
```

应该是如下的方式:

```
log.Printf(
    “A long format string: %s %d %d %s”,
    myStringParameter,
    len(a),
    expected.Size,
    defrobnicate(
    “Anotherlongstringparameter”,
    expected.Growth.Nanoseconds()/1e6,
    ),
）
```

## 注意闭包的调用

在循环中调用函数或者`goroutine`方法, 一定要采用显示的变量调用, 不要再闭包函数里面调用循环的参数

```
fori:=0;i<limit;i++{
  go func(){ DoSomething(i) }() //错误的做法
  go func(i int){ DoSomething(i) }(i)//正确的做法
￼}
```

<http://golang.org/doc/articles/race_detector.html#Race_on_loop_counter>

## 在逻辑处理中禁用`panic`

在`main`包中只有当实在不可运行的情况采用`panic`, 例如文件无法打开, 数据库无法连接导致程序无法正常运行, 但是对于其他的`package`对外的接口不能有`panic`, 只能在包内采用。

强烈建议在`main`包中使用`log.Fatal`来记录错误, 这样就可以由`log`来结束程序.

## 注释规范

注释可以帮我们很好的完成文档的工作, 写得好的注释可以方便我们以后的维护. 详细的如何写注释可以参考: <http://golang.org/doc/effective_go.html#commentary>

## `bug`注释

针对代码中出现的`bug`, 可以采用如下教程使用特殊的注释, 在`godocs`可以做到注释高亮:

```
// BUG(astaxie):This divides by zero.
var i float = 1/0
```

<http://blog.golang.org/2011/03/godoc­documenting­go­code.html>

## `struct`规范

`struct`申明和初始化格式采用多行:

定义如下:

```
type User struct{
  Username  string
  Email     string
}
```

初始化如下:

```
u := User{
  Username: "astaxie",
  Email:    "astaxie@gmail.com",
}
```

## `recieved`是值类型还是指针类型

到底是采用值类型还是指针类型主要参考如下原则:

```
func(w Win) Tally(playerPlayer)int    //w不会有任何改变
func(w *Win) Tally(playerPlayer)int    //w会改变数据
```

更多的请参考: <https://code.google.com/p/go-wiki/wiki/CodeReviewComments#Receiver_Type>

## 带`mutex`的`struct`必须是指针`receivers`

如果你定义的`struct`中带有`mutex`, 那么你的`receivers`必须是指针.

## `init()` 函数

虽然一个 `package` 里面可以写任意多个 `init`函数, 但这无论是对于可读性还是以后的可维护性来说, 我们都强烈建议用户在一个 `package` 中每个文件只写一个 `init` 函数.

## `Example` 写法

* `example_test.go` 里面写 `ExampleFunc()` 函数来实现.

## `test` 写法

* 使用包 `package xxx_test` 而不污染单元测试的 package.

## 文件夹与文件命名

* 文件夹: `aaa-bbb/`
* 文件: `aaa_bbb.go`

## 代码审核权威指南

* <https://github.com/golang/go/wiki/CodeReview>

包名： 应该根据某个模块提供的服务而不是它的内容来定义包名。如果一个包含有HelloWorld消息，那么它不应该被称为common或consts，而是greetings。包名应该表明它所做什么，而不是它有什么。

点导入： Bourgon建议不要使用“点导入”，这个特性通过设置点号来代替包名，使得开发者不需要明确的包名就可以访问相应包中的变量。这个特性降低了项目的可读性，尤其对于新手，新来的开发人员容易弄错哪个变量属于哪个包。Go——显式声明优于隐式声明。

Flags： Bourgon并不认为在init()方法而不在main()方法中初始化flags是一个好主意，因为这使得这些flags无法在全局领域使用，而某些测试用例要用到这些flags。

有意义的默认值： 不要使用nil初始化某个变量，这使得每次在使用该变量的时候都需要进行空值检查，最好使用一个无操作值（no-operation value）进行变量初始化。例如，使用ioutil.Discard初始化一个output变量。


## 参考资料

* <http://golanghome.com/post/550>
* <https://code.google.com/p/go-wiki/wiki/CodeReviewComments>
* <http://golang.org/doc/effective_go.html>
* <http://talks.golang.org/2014/readability.slide#1>
* <https://github.com/sergiotapia/go-style-guide>
* [Go语言规范](https://golang.org/ref/spec)
* [Go命名指南](https://talks.golang.org/2014/names.slide#1)
