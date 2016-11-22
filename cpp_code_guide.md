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
### 基于[Doxygen](https://zh.wikipedia.org/wiki/Doxygen)的注释规范
具体可以参照janus源码的形式，这里做简单的规范，大家可以持续补充

####基本格式
* 简短描述：结构体，枚举，联合体等结构体及成员，均使用`/*! \brief xxxxx */`。
如：
```
/*! \brief Janus media statistics: received packet info
 * \note To improve with more stuff */
typedef struct janus_ice_stats_item {
	/*! \brief Bytes sent or received */
	guint64 bytes;
	/*! \brief Time at which this happened */
	gint64 when;
} janus_ice_stats_item;
```
* 详细（文档）描述：函数使用  `/*!  关键字 text … */`，关键字如： \note、\brief  ，等
如：
```
/*! \note The RTP/RTCP port range configuration may be just a placeholder: for
	 * instance, libnice supports this since 0.1.0, but the 0.1.3 on Fedora fails
	 * when linking with an undefined reference to \c nice_agent_set_port_range 
	 * so this is checked by the install.sh script in advance. */
	rtp_range_min = rtp_min_port;
	rtp_range_max = rtp_max_port;
	if(rtp_range_max < rtp_range_min) {
		JANUS_LOG(LOG_WARN, 
                    "Invalid ICE port range: %"SCNu16" > %"SCNu16"\n", rtp_range_min, rtp_range_max);
	} else if(rtp_range_min > 0 || rtp_range_max > 0) {
#ifndef HAVE_PORTRANGE
		JANUS_LOG(LOG_WARN, "nice_agent_set_port_range unavailable, port range disabled\n");
#else
		JANUS_LOG(LOG_INFO, "ICE port range: %"SCNu16"-%"SCNu16"\n", rtp_range_min, rtp_range_max);
#endif
```

#### 文件头
以上看文件的字段名：有文件名(\file)，作者(\auther)，版权声明(copyright)，
简述(\brief)，详细描述(details)，模块名(\ingroup) ，引用(\ref)
如：
```
/*! \file   janus.c    
 * \author Lorenzo Miniero <lorenzo@meetecho.com>
 * \copyright GNU General Public License v3
 * \brief  Janus core
 * \details Implementation of the gateway core. This code takes care of
 * the gateway initialization (command line/configuration) and setup,
 * and makes use of the available transport plugins (by default HTTP,
 * WebSockets, RabbitMQ, if compiled) and Janus protocol (a JSON-based
 * protocol) to interact with the applications, whether they're web based
 * or not. The core also takes care of bridging peers and plugins
 * accordingly, in terms of both messaging and real-time media transfer
 * via WebRTC.
 *
 * \ingroup core
 * \ref core
 *
```

####函数声明
函数注释字段名： 简述(\brief)、注意(\note)、形参(@param[in or out or inout])、返回值(@returns)
如：
```
/*! \brief Method to return a list of the plugins a specific token has access to
 * \note It's the caller responsibility to free the list (but NOT the values)
 * @param[in] token The token to get the list for
 * @returns A pointer to a GList instance containing the liist */
GList *janus_auth_list_plugins(const char *token);
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
