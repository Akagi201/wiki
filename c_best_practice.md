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

## format 工具
* [ClangFormat](http://clang.llvm.org/docs/ClangFormat.html)
* [clang-tidy](http://clang.llvm.org/extra/clang-tidy/)

## Refs
* <http://www.infoq.com/cn/articles/c-language-2016>