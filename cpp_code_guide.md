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
* 内部函数, 注释写到函数定义的上面. 外部函数, 注释写到头文件的函数的上面.

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
