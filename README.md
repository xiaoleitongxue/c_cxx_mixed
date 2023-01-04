# cmake：C/C++混合编程（C调用C++函数）

# 问题描述
在开发过程中，经常需要将C/C++混合编程，但用gcc编译器直接编译调用了C++函数的.c文件，若该C++函数使用了C++特有的库，比如iostream,gcc编译器将找不到iostream。
cmake是构建c/c++工程最重要的工具。以下例子展示了如何用cmake构建c语言调用c++。
# 项目文件
src目录下有3个文件，分别是main.c,cpp.cpp,cpp.hpp。从文件的拓展名可以看出，第一个文件是c语言写的，后两个是c++的文件。
main.c调用定义在cpp.cpp定义的函数。
完整项目链接：[https://github.com/xiaoleitongxue/c_cxx_mixed.git](https://github.com/xiaoleitongxue/c_cxx_mixed.git)
三个文件分别如下
main.c

```c
//main.c
#include "cpp.hpp"
int main() {
    cpp();
}
```

cpp.hpp

```c++
//cpp.hpp
#ifdef __cplusplus
extern "C"
{
#endif
void cpp();
#ifdef __cplusplus
}
#endif
```

cpp.cpp

```c++
//cpp.cpp
#include "cpp.hpp"
#include <iostream>
using namespace std;
void cpp()
{
    std::cout << "hello" << std::endl;
}
```

## 几个需要注意的点
1. 被c调用的c++函数需要用extern "C"
extern "C"是C++中的语法，extern "C" 使 C++ 中的函数名称具有 C 链接（编译器不会破坏名称），以便C代码可以使用仅包含函数声明的C兼容头文件链接到C++函数。cpp.cpp文件使用g++编译器编译。简而言之，我们需要将c++函数声明具有c链接，而cpp文件依旧使用c++的方式编译。
2. 若c++函数中使用了c++的库，比如iostream，#include<iostream>只能写在cpp源文件中，不能写在.h文件中.h文件由gcc编译器处理，它无法处理c++的头文件进行预处理，将抛出未定义错误。

# 用CMake构建
最后，为了用cmake构建这个项目，还需要编写CMakeLists.txt
CMakeLists.txt
```
cmake_minimum_required(VERSION 3.0.0)
# 指定项目用到的语言C和CXX
project(c_cxx_mixed VERSION 0.1.0 LANGUAGES C CXX)
# 编译源文件
add_executable(c_cxx_mixed src/main.c src/cpp.cpp src/cpp.hpp)
//指定头文件目录
target_include_directories(c_cxx_mixed PUBLIC src/)
```
在cmake中指定了编程语言，cmake会自动处理，调用相应的编译器gcc或g++。
