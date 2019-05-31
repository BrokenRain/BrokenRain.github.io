---
title: C/C++中如何获取数组和指针的长度
date: 2019-05-31 17:30:02
tags:
  - C/C++
---


# 获取数组长度

* **算术表达式**

```
#include <iostream>
using namespace std;

int main()
{
    int arr[15];
    cout << "sizeof(arr) / sizeof(*arr):" << sizeof(arr) / sizeof(*arr) << endl;	//输出15
    cout << "sizeof(arr) / sizeof(arr[0]):" << sizeof(arr) / sizeof(arr[0]) << endl;//输出15

    return 0;
}
```

<!--more-->

* **函数模板参数自动推断**

```
#include <iostream>
using namespace std;

template<class T, size_t N>
size_t getCount(T (&arr)[N])
{
    return N;
}

int main()
{
    int arr[15];
    cout << "getCount(arr):" << getCount(arr) << endl;//输出15

    return 0;
}
```

* **标准C++模板库**

```
#include <iostream>
#include <type_traits>  // 需包含此头文件
using namespace std;

int main()
{
    int arr[15];
    cout << "extent<decltype(arr), 0>::value:" << extent<decltype(arr), 0>::value << endl;//输出15

    return 0;
}
```

* **模板特化与自动类型推断**

```
include <iostream>
using namespace std;

template <typename T>
class ComputeSize;
 
template <typename T, size_t N>
class ComputeSize<T[N]>
{
public: 
    static const size_t value = N;
};
 
int main()
{ 
    int arr[15]; 
    cout << "ComputeSize<decltype( arr )>::value:" << ComputeSize<decltype( arr )>::value << endl; // 输出15

    return 0;
}
```

* **Visual C++编译器预定义宏**

```
include <iostream>
#include <stdlib.h>
using namespace std;
 
int main()
{ 
    int arr[15]; 
    cout << "_countof(arr):" << _countof(arr) << endl; // 输出15

    return 0;
}

```

* **boost库**

```
#include <iostream>
#include "boost/range.hpp"
using namespace std;
 
int main()
{   
    int arr[15];   
    cout << "boost::size(arr):" << boost::size(arr) << endl; // 输出15

    return 0;
}
```

# 获取指针长度

* **windows平台**

```
include <iostream>
using namespace std;
 
int main()
{ 
    int *arr = new int[15]; 
    cout << "_msize(arr):" << _msize(arr) / sizeof(*arr) << endl; // 输出15
    delete arr;

    return 0;
}
```

* **linux平台**

```
include <iostream>
using namespace std;
 
int main()
{ 
    int *arr = new int[15]; 
    cout << "malloc_usable_size(arr):" << malloc_usable_size(arr) / sizeof(*arr) << endl; // 输出15
    delete arr;

    return 0;
}
```

本文参考链接: <https://blog.csdn.net/brucethl/article/details/79257360>