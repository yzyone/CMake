# CMake 入门学习3 基本指令、宏、模块、函数

# 一、CMake 基本指令

# 1. ADD_DEFINITIONS

给编译增加参数定义，如向 C/C++编译器添加-D 参数，例:

```
ADD_DEFINITIONS(-DENABLE_DEBUG -DABC123)，多个参数使用空格分割。
```

宏定义 #ifdef ENABLE_DEBUG #endif 会让上面指令生效。

# 2. ADD_DEPENDENCIES

增加 make 里编译依赖关系。

```
ADD_DEPENDENCIES(target-name depend-target1 depend-target2 ...)
```

两个target有依赖关系时，使用此指令可以自动按顺序编译依赖，最后再连接目标。

# 3.ADD_EXECUTABLE

定义可执行文件的指令：

```
ADD_EXECUTABLE(hello main.cpp)
```

# 4.ADD_LIBRARY

调用其它库，语法：

```
add_library(<name> [STATIC | SHARED | MODULE]
            [EXCLUDE_FROM_ALL]
            source1 [source2 ...])
```

- 其中表示链接库文件的名字，全局唯一；库的源文件可指定，也可用target_sources()后续指定。
- 库的类型： STATIC(静态库)、SHARED(动态库)、MODULE(模块库)之一；如果不设置，也可以通过全局的 BUILD_SHARED_LIBS 来指定。
  windows下，如果dll没有export任何信息，则不能使用SHARED，要标识为MODULE。

添加的库会被输出到以下几个目录：

- ARCHIVE_OUTPUT_DIRECTORY
- LIBRARY_OUTPUT_DIRECTORY
- RUNTIME_OUTPUT_DIRECTORY
- 设置EXCLUDE_FROM_ALL，可使这个library排除在all之外，即必须明确点击生成才会生成。

# 5. ADD_SUBDIRECTORY

```
ADD_SUBDIRECTORY(source_dir [binary_dir] [EXCLUDE_FROM_ALL])
```

这个指令用于向当前工程添加存放源文件的子目录，并可以指定中间二进制和目标二进制存放的位置。

- EXCLUDE_FROM_ALL 参数的含义是将这个子目录的所有target排除在all target列表之外，这样当执行make时，这个子目录的所有target就不会被编译。

# 6. CMAKE_MINIMUM_REQUIRED

```
CMAKE_MINIMUM_REQUIRED(VERSION versionNumber [FATAL_ERROR])
```

检查cmake的版本，要求最低版本为versionNumber。例如 CMAKE_MINIMUM_REQUIRED(VERSION 2.5 FATAL_ERROR) 。

# 7. INCLUDE_DIRECTORIES

```
INCLUDE_DIRECTORIES([AFTER|BEFORE] [SYSTEM] dir1 [dir2 ...])
```

指定头文件的搜索路径。

例：

```
include_directories(/usr/local/include) 
```

来让库文件搜索以/usr/local/include为基础，然后在main函数前写上：

```
#include “opencv/cv.h" 
```

# 8. LINK_DIRECTORIES

```
LINK_DIRECTORIES(directory1 directory2 ...)
```

添加需要链接的库文件目录，相当于g++命令的-L选项的作用。 该指令有时候不一定需要，因为find_package和find_library指令可以得到库文件的绝对路径。

一般自己写的动态库文件放在自己新建的目录下时，可以用该指令定位，如：

```
LINK_DIRECTORIES("/home/my/mylib/bin/mylibs")
```

# 9. LINK_LIBRARIES

```
LINK_LIBRARIES(library1 library2 ...)
```

添加需要链接的库文件路径，注意这里是全路径，要用在add_executable之前。示例：

```
LINK_LIBRARIES("/home/my/mylib/bin/mylibs/aaa.so")
```

# 10. TARGET_LINK_LIBRARIES

```
TARGET_LINK_LIBRARIES(target library1 <debug | optimized> library2 ...)
```

为库或二进制可执行文件添加库链接，要用在add_executable之后。 上述指令中的target是指通过add_executable()和add_library()指令生成已经创建的目标文件。示例：

```
TARGET_LINK_LIBRARIES(myProject hello)，连接libhello.so库
TARGET_LINK_LIBRARIES(myProject libhello.a)
```

# 11. PKG_CHECK_MODULES

```
pkg_check_modules(<PREFIX> [REQUIRED] [QUIET]
                  [NO_CMAKE_PATH] [NO_CMAKE_ENVIRONMENT_PATH]
                  <MODULE> [<MODULE>]*)
```

pkg_check_modules 是 CMake 自己的 pkg-config 模块 的一个用来简化的封装：你不用再检查 CMake 的版本，加载合适的模块，检查是否被加载，等等，参数和传给 find_package 的一样：先是待返回变量的前缀，然后是包名（pkg-config 的）。这样就定义了<prefix>_INCLUDE_DIRS和其他的这类变量，后续的用法就与 find_package 一致。

# 二、 CMake的宏、模块、函数

# 1. 宏 macro定义：

```
macro(<name> [arg1 [arg2 [arg3 ...]]])
...
endmacro([name])
```

# 2. function定义：

```
function(<name> [arg1 [arg2 [arg3 ...]]])
...
endfunction([name])
```

函数和宏都有默认内部变量可以使用：

| **变量** | **说明**                                                     |
| -------- | ------------------------------------------------------------ |
| ARGV#    | ARGV0为第一个参数，ARGV1为第二个参数，依次类推               |
| ARGV     | 定义宏（函数）时参数为2个，实际传了4个，则ARGV代表实际传入的两个 |
| ARGN     | 定义宏（函数）时参数为2个，实际传了4个，则ARGN代表剩下的两个 |
| ARGC     | 实际传入的参数的个数                                         |

调用示例：

```
# 定义函数
Function(myfunction ag1 ag2)
message(STATUS "function ag is " ${ag1})
message(STATUS "function ag is " ${ag2})
endfunction(myfunction)


# 定义宏
macro(mymacro ag1 ag2)
message(STATUS "macro ag is " ${ag1})
message(STATUS "macro ag is " ${ag2})
endmacro(mymacro)

# 调用函数
myfunction(1 2 3)
message(STATUS "\n")

# 调用宏
mymacro(1 2 3)
```

要注意的是 宏的ARGN、ARGV等内部变量不能直接在if、foreach(…IN LISTS…)等逻辑语句中使用。

# 二、模块

# 1. 模块说明

CMakeLists.txt和**.cmake结尾的文件可以用来作为CMake的模块文件，用来封装一些函数功能，再使用include(**.cmake)方式引用。

cmake系统本身内置了一些预定义的模块，如FindCURL模块。
预定义模块可以通过FIND_PACKAGE指令来引用。

# 2. 模块定义示例

# 根目录的主Cmake文件定义

```
# CMake 最低版本号要求
cmake_minimum_required (VERSION 2.8)

# 项目工程名
project (example)
message(STATUS "root This is BINARY dir " ${PROJECT_BINARY_DIR})
message(STATUS "root This is SOURCE dir " ${PROJECT_SOURCE_DIR})

# 添加子目录
ADD_SUBDIRECTORY(subDirectory)
```

# 子模块定义

创建
subDirectory/CMakeLists.txt，内容如下：

```
# 打印信息
message(STATUS "src This is BINARY dir " ${PROJECT_BINARY_DIR})
message(STATUS "src This is SOURCE dir " ${PROJECT_SOURCE_DIR})

set(EXECUTABLE_OUTPUT_PATH ${PROJECT_BINARY_DIR}/bin)

# CMAKE_SOURCE_DIR 是cmake内置变量，工程根目录的CMakeLists.txt文件路径
SET(ROOT_DIR ${CMAKE_SOURCE_DIR})

# 构建可执行程序
ADD_EXECUTABLE(example main.cpp)

# 查找指定的库,CURL是
FIND_PACKAGE(CURL)
```