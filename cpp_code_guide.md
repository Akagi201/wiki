## 原则
* 使用 C 语言的首要规则是, 能不用就不用.
* 如果必须要用 C 语言, 应该遵照现代的规则.

## 编译器
* 使用 c11 标准, clang 和 gcc 都默认使用扩展的 c11 版本(GNU c11 模式), 如果要使用标准的 c11 标准, 需要指定 `-std=c11`.
* 优化级别使用 `-O2` 或者 `-O3`, 通常使用 `-O2`, 也可以对比测试, 保留最佳性能的二进制代码.

## 类型
* 不再使用 char, int, short, long 或者 unsigned 类型, 使用 `#include <stdint.h>` 里面的类型.

## 声明
* 声明不要都放到开头, 卸载初始化语句附近.

## 头文件
* 不再使用 `#ifndef XXX #define XXX #endif`, 而使用 `#pragma once`

## 初始化
* C语言允许静态初始化自动分配数组, `uint32_t numbers[64] = {0};`  `struct thing localThing = {0};`

## 注释

* 使用 Doxygen 的注释规范.
* 可以参考 [janus-gateway](https://github.com/meetecho/janus-gateway) 项目 源码.

### Doxygen 结构化注释块
* 包含三个部分: brief description, detailed description, in body description.
* brief description: a short one-liner.
* detailed description: longer, more detailed documentation.
* in body description: For methods and functions there is also a third type of description, the so called in body description, which consists of the concatenation of all comment blocks found within the body of the method or function.
* An "in body" description can also act as a detailed description or can describe a collection of implementation details.
* 避免使用 structural commands.
* 对外函数的注释, 写到函数实现的上面, 不写到函数头文件声明的地方, doxygen 支持这样写法, 也能关联到对应的注释.

### 简短注释与详细注释

使用 `\brief` 命令, 后面跟着 brief description, 与 detailed description 之间用一个空行分隔.

```
/*! \brief Brief description.
 *         Brief description continued.
 *
 *  Detailed description starts here.
 */
```

### 成员注释

成员包括: file, struct, union, class, enum, function param 的成员.

```
int var; ///< Detailed description after the member
         ///< ...
```

### 函数成员注释

使用 `\param` 命令来注释函数参数, 使用 `[in]`, `[out]`, 
`[in,out]` 来注释参数传递方向.

```
\param [(dir)] <parameter-name> { parameter description }
```

```
/*! \brief Brief description.
 *
 *  \param[in] a an integer argument.
 *  \param[in] s a constant character pointer.
 *  \return The test results.
 */
int testMe(int a, const char *s);
```

### 文件头注释

```
/*! \file janus.c
 *  \author Akagi201
 *  \brief Brief description.
 *
 *  Detailed descriptions.
 */
```

## 结构体定义

```
typedef xxx_xxxx {
  xxxx;
} xxx_xxx_t;
```

## format 工具
* [ClangFormat](http://clang.llvm.org/docs/ClangFormat.html)
* [clang-tidy](http://clang.llvm.org/extra/clang-tidy/)
* `clang-format -sort-includes -style=Google -i xxx.c xxx.h`

## 开发工具
* sublime text
* CLion
* XCode

## 构建工具
* CMake 推荐.
* gn 为了方便与 WebRTC 集成.
* Makefile 不推荐使用, 但使用也没问题.

## 框架
* [boost](http://www.boost.org/)

## 注释与文档生成工具
* [doxygen](http://www.stack.nl/~dimitri/doxygen/)

## 单元测试
* gtest: <https://github.com/google/googletest>
* gmock: <https://github.com/google/googlemock>

## 包管理工具
* [clib](https://github.com/clibs/clib) 推荐, 类似 npm 的包管理方式, 就是 npm 的社区开发的, 比较简单.
* [conan](https://conan.io/) 比较复杂, 不是很直观, 强制使用 cmake.

## 最佳实践
* <https://github.com/isocpp/CppCoreGuidelines>
* C in 2016

## Refs
* C in 2016: [英文](https://matt.sh/howto-c) [中文](http://www.infoq.com/cn/articles/c-language-2016) [HN](https://news.ycombinator.com/item?id=10864176) [反对](https://github.com/Keith-S-Thompson/how-to-c-response)
* <http://awesomecpp.com/>
* <https://github.com/isocpp/CppCoreGuidelines>
