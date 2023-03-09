# CMake 入门学习1 Hello World

# 一、简介

CMake是一款跨平台的 通过CMakeList.txt构建Makefile的工具。
入门代码：
https://github.com/ttroy50/cmake-examples

- CMake安装过程本文不作讲解。
- CMake版本：3.5

# 二、第一个简单入门程序

# 1. 建立一个main.cpp文件

```
#include <iostream>

int main(int argc, char *argv[])
{
   std::cout << "Hello CMake!" << std::endl;
   return 0;
}
```

# 2. 创建 CMakeLists.txt 文件

```
# Set the minimum version of CMake that can be used
# To find the cmake version run
# $ cmake --version
cmake_minimum_required(VERSION 3.5)

# Set the project name
project (hello_cmake)

# Add an executable
add_executable(hello_cmake main.cpp)
```

# 3. 构建过程

```
cmake .
make 
./hello_make
```

构建后目录示例：tree

```
# tree
.
├── build
│   ├── CMakeCache.txt
│   ├── CMakeFiles
│   │   ├── 3.16.3
│   │   │   ├── CMakeCCompiler.cmake
│   │   │   ├── CMakeCXXCompiler.cmake
│   │   │   ├── CMakeDetermineCompilerABI_C.bin
│   │   │   ├── CMakeDetermineCompilerABI_CXX.bin
│   │   │   ├── CMakeSystem.cmake
│   │   │   ├── CompilerIdC
│   │   │   │   ├── a.out
│   │   │   │   ├── CMakeCCompilerId.c
│   │   │   │   └── tmp
│   │   │   └── CompilerIdCXX
│   │   │       ├── a.out
│   │   │       ├── CMakeCXXCompilerId.cpp
│   │   │       └── tmp
│   │   ├── cmake.check_cache
│   │   ├── CMakeDirectoryInformation.cmake
│   │   ├── CMakeOutput.log
│   │   ├── CMakeTmp
│   │   ├── feature_tests.bin
│   │   ├── feature_tests.cxx
│   │   ├── hello_cmake.dir
│   │   │   ├── build.make
│   │   │   ├── cmake_clean.cmake
│   │   │   ├── CXX.includecache
│   │   │   ├── DependInfo.cmake
│   │   │   ├── depend.internal
│   │   │   ├── depend.make
│   │   │   ├── flags.make
│   │   │   ├── link.txt
│   │   │   ├── main.cpp.o
│   │   │   └── progress.make
│   │   ├── Makefile2
│   │   ├── Makefile.cmake
│   │   ├── progress.marks
│   │   └── TargetDirectories.txt
│   ├── cmake_install.cmake
│   ├── hello_cmake
│   └── Makefile
├── CMakeLists.txt
└── main.cpp
```

# 三、CMakeList.txt文件说明

# 1. cmake_minimum_required(VERSION 3.5)

定义最小需要的cmake版本

# 2. project (hello_cmake)

定义项目名称

# 3. 定义项目需要的源文件

```
add_executable(hello_cmake main.cpp)
```

# 4. 引用变量

```
cmake_minimum_required(VERSION 2.6)
project (hello_cmake)
add_executable(${PROJECT_NAME} main.cpp)
```

这里${PROJECT_NAME}用来引用项目名称。

# 5. 可执行文件目录

可以在当前文件夹执行cmake .，这样生成的可执行文件在当前目录下；
也可以不在源码目录来构建，如：

```
mkdir build
cd build
cmake ..
make
./hello_make
```

这样生成的可执行文件和CMake的文件都在build下。