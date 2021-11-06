---
title: c++ chrono库高精度获取程序函数运行时间
date: 2021-01-27 12:37:10
tags: [特别的C++类]
---

今天谷歌上发现个c++ `chrono(计时)` 库，可以用来高精度跟踪时间，高精度统计程序运行时间,获取系统、GPS时间，c++ std 20还加入了各种日历类，支持对公历日期进行计算，可以用来解决`clock()`函数统计程序运行时间为0等问题，前面的内容较乱，看不懂没关系，直接看最后代码吧。

###  包含的类及其功能

> 以下类均包含在`std::chrono`命名空间内，`chrono` 库定义了三种主要类型，时钟类型(clocks)，时长类型（durations），时间点类型（time points）

<!-- more -->

#### 时钟类型

| [system_clock](https://zh.cppreference.com/w/cpp/chrono/system_clock)(C++11) | 来自系统范畴实时时钟的挂钟时间 (类)                          |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| [steady_clock](https://zh.cppreference.com/w/cpp/chrono/steady_clock)(C++11) | 决不会调整的单调时钟 (类)                                    |
| [high_resolution_clock](https://zh.cppreference.com/w/cpp/chrono/high_resolution_clock)(C++11) | 拥有可用的最短嘀嗒周期的时钟 (类)                            |
| [is_clockis_clock_v](https://zh.cppreference.com/w/cpp/chrono/is_clock)(C++20) | 确定类型是否为[*时钟* *(Clock)*](https://zh.cppreference.com/w/cpp/named_req/Clock) (类模板) (变量模板) |
| [utc_clock](https://zh.cppreference.com/w/cpp/chrono/utc_clock)(C++20) | 协调世界时 (UTC) 的[*时钟* *(Clock)*](https://zh.cppreference.com/w/cpp/named_req/Clock) (类) |
| [tai_clock](https://zh.cppreference.com/w/cpp/chrono/tai_clock)(C++20) | 国际原子时 (TAI) 的[*时钟* *(Clock)*](https://zh.cppreference.com/w/cpp/named_req/Clock) (类) |
| [gps_clock](https://zh.cppreference.com/w/cpp/chrono/gps_clock)(C++20) | GPS 时间的[*时钟* *(Clock)*](https://zh.cppreference.com/w/cpp/named_req/Clock) (类) |
| [file_clock](https://zh.cppreference.com/w/cpp/chrono/file_clock)(C++20) | 用于[文件时间](https://zh.cppreference.com/w/cpp/filesystem/file_time_type)的[*时钟* *(Clock)*](https://zh.cppreference.com/w/cpp/named_req/Clock) (typedef) |
| [local_t](https://zh.cppreference.com/w/cpp/chrono/local_t)(C++20) | 表示本地时间的伪时钟 (类)                                    |

##### steady_clock

- 类 `std::chrono::steady_clock` 表示单调时钟。此时钟的时间是一个常量，因此无法改变，成员函数，成员变量请参考：[steady_clock](https://zh.cppreference.com/w/cpp/chrono/steady_clock)
- now[静态]，返回表示当前时钟值的 time_point(public静态成员函数)

#### 时长类型

定义于命名空间 `std::chrono`

| [duration](https://zh.cppreference.com/w/cpp/chrono/duration)(C++11) | 时间区间(类模板) |
| ------------------------------------------------------------ | ---------------- |
|                                                              |                  |

#### 时间点类型

| [time_point](https://zh.cppreference.com/w/cpp/chrono/time_point)(C++11) | 时间中的点 (类模板)                                   |
| ------------------------------------------------------------ | ----------------------------------------------------- |
| [clock_time_conversion](https://zh.cppreference.com/w/cpp/chrono/clock_time_conversion)(C++20) | 定义如何转换一个时钟的时间点为另一个的特性类 (类模板) |
| [clock_cast](https://zh.cppreference.com/w/cpp/chrono/clock_cast)(C++20) | 转换一个时钟的时间点为另一个 (函数模板)               |

### 统计时间代码

需要编译器在c++ 17版本及以上

```csharp
#include <iostream>
#include <chrono>
using namespace std;
int main(){
        chrono::steady_clock::time_point start=chrono::steady_clock::now(); //记录开始时间
        
        //这里放置你需要测试时间的程序
        
        chrono::steady_clock::time_point pend = chrono::steady_clock::now(); //记录结束时间
        chrono::duration<double> span = chrono::duration_cast<chrono::duration<double>>(pend - start); //chrono时期对象
        cout<<"Spend Time:"<<span.count()<<endl;
		return 0;
}
```

> 参考文档，[Date and time utilities](https://en.cppreference.com/w/cpp/chrono)，美国cppreference