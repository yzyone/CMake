# CMake详解

- **什么是CMake**
  在android studio 2.2及以上，构建原生库的默认工具是 CMake。
- CMake是一个跨平台的构建工具，可以用简单的语句来描述所有平台的安装(编译过程)。能够输出各种各样的makefile或者project文件。Cmake 并不直接建构出最终的软件，而是产生其他工具的脚本（如Makefile ），然后再依这个工具的构建方式使用。

- CMake是一个比make更高级的编译配置工具，它可以根据不同平台、不同的编译器，生成相应的Makefile或者vcproj项目。从而达到跨平台的目的。Android Studio利用CMake生成的是ninja，ninja是一个小型的关注速度的构建系统。我们不需要关心ninja的脚本，知道怎么配置cmake就可以了。从而可以看出cmake其实是一个跨平台的支持产出各种不同的构建脚本的一个工具。


**CMake源文件**

- CMake的源码文件可以包含命令、注释、空格、和换行。
- 以CMake编写的源文件以CMakeLists.txt命名或以.cmake为扩展名。
- 可以通过add_subdirectory()命令把子目录的CMake源文件添加进来。
- CMake源文件中所有的语句都是命令，可以是内置命令或自定义的函数/宏命令

**CMake变量**

- CMake中所有变量都是string类型。可以使用set()和unset()命令来声明或者移除一个变量
- 变量的引用：${变量名}

```
#声明变量：SET(变量名  变量值)
SET(var 666)
#引用变量 message命令用来打印
MESSAGE("var=${var}")
```
**CMake变量的作用域**

- 全局层：cache变量，在整个项目范围内，一般在SET定义变量时，指定CACHE参数就能定义为cache变量。
- 目录层：在当前目录CMakeLists.txt中定义，以及在该文件包含的其它cmake源文件中定义的变量。
- 函数层：在命令函数中定义的变量，属于函数作用域内的变量。

**CMake列表（lists）**

- 列表也是字符串，可以把列表看做一个特殊的变量，这个变量有多个值。
- 语法格式：SET(列表名 值1 值2… 值N)或者SET(列表名 “值1;值2；…值N”)
- 列表的引用 ${列表名}


```
#SET(列表名 值1 值2... 值N)
#或者SET(列表名 “值1;值2；...值N”)
SET(list_var 1 3 5 7)
#或
SET(list_var "1;3;5;7")
MESSAGE("list_var=${list_var}")
```

**CMake流程控制-布尔常量**

类型	值
true	1，ON，YES，TRUE，Y，非0的值
false	0，OFF，NO，FALSE,N,IGNORE,NOTFOUND,
空字符串，以-NOTFOUND结尾的字符串

**CMake流程控制-操作符**

类型	值
一元	EXIST,COMMAND,DEFINED
二元	EQUAL,LESS,LESS_EQUAL,GREATER,GREATER_EQUAL,
STREQUAL,STRLESS,STRLESS_EQUAL,STRGREATER,
STRGREATER_EQUAL,VERSION_EQUAL,VERSION_LESS,
VERSION_LESS_EQUAL,VERSION_GREATER,VERSION_GREATER_EQUAL,MATCHES
逻辑	NOT,AND,OR

**CMake流程控制-循环遍历**
**循环遍历一**

- 语法格式：

  foreach(循环变量 参数1 参数2 … 参数n)

  COMMAND(ARGS …)

  endforeach(循环变量)

- 每次迭代设置循环变量为参数。

- foreach也支持breack()和continue()命令跳出循环。
```
FOREACH(item 2 4 6)
    MESSAGE("item=${item}")
ENDFOREACH(item)    
```
**循环遍历二**

- 语法格式：

  foreach(循环变量 RANGE total)

  COMMAND(ARGS …)

  endforeach(循环变量)

- 循环范围从0到total。
```
FOREACH(item RANGE 6)
    MESSAGE("item=${item}")
ENDFOREACH(item)    
```
**循环遍历三**

- 语法格式：

  foreach(循环变量 RANGE start stop step)

  COMMAND(ARGS …)

  endforeach(循环变量)

- 循环范围从start到stop,循环增量为step。
```
FOREACH(item RANGE 1 6 2)
    MESSAGE("item=${item}")
ENDFOREACH(item)    
```
**循环遍历四**

- foreach还支持对列表的循环。

- 语法格式：

  foreach(循环变量 IN LISTS 列表)

  COMMAND(ARGS …)

  endforeach(循环变量)
```
SET(list_var 1 2 3)
FOREACH(item IN LISTS list_var)
    MESSAGE("item=${item}")
ENDFOREACH(item)    
```

**CMake自定义函数命令**

- 自定义函数命令格式：

  function([arg1[arg2[arg3…]]])

  COMMAND()

  endfunction()

- 函数命令调用调用格式：

  name(实参列表)
```
   FUNCTION(func x y z)
    MESSAHE("call function func")
    MESSAGE("x=${x}")
    MESSAGE("y=${y}")
    MESSAGE("z=${z}")
    MESSAGE("ARGC = ${ARGC}")
    MESSAGE("arg1= ${ARGV0}" arg2= ${ARGV1} arg3= ${ARGV2})
    MESSAGE("all args=${ARGV}")
   ENDFUNCTION(func)

   func(2,5,7)
```
**CMake自定义宏命令**

- 自定义宏命令格式：

  macro([arg1[arg2[arg3…]]])

  COMMAND()

  endmacro()

- 宏命令调用格式：

  name(实参列表)
```
   MACRO(ma x y z)
    MESSAHE("call macro ma")
    MESSAGE("x=${x}")
    MESSAGE("y=${y}")
    MESSAGE("z=${z}")
   ENDMOCRO(ma)

   ma(2,5,7)
```

**常用命令**

常用命令-AUX_SOURCE_DIRECTORY(. DIR_SRCS)

```
 AUX_SOURCE_DIRECTORY(. DIR_SRCS)
```

- 查找当前目录所有源文件 并将源文件名称列表保存到 DIR_DRCS变量中

- 不能查找子目录

**常用命令add_library**
添加一个库

- 添加一个库文件，名为<name>

- 指定STATIC,SHARED,MODULE参数来指定库的类型。STATIC：静态库；SHARED:动态库；
- MODULE:在使用dyld的系统有效，若不支持dydld,等同于SHARED.
- EXCLUED_FROM_ALL:表示该库不会被默认构建。
- source1 source2 … sourceN:用来指定库的源文件
```
add_library(<name> [STATIC | SHARED | MODULE] [EXCLUED_FORM_ALL] source1 source2 ... sourceN)
#比如
add_library(hello SHARED hello.cpp)    
```
导入预编译库

- 添加一个已存在的与编译库，名为<name>
- 一般配合set_target_properties使用
```
add_library(<name> [STATIC | SHARED | MODULE|UNKNOWN] IMPORTED)
#比如
add_library(test SHARED IMPORTED)
set_target_properties(
    test  #指明目标库名
    PROPERTIES IMPORTED_LOCATION #指明要设置的参数
    库路径/${ANDROID_ABI}/libtest.so #导入库的路径
    )    
```

**常用命令-SET**

- 设置CMake变量
```
    #设置可执行文件的输出路径（EXCUTABLE_OUTPUT_PATH是全局变量）
​    SET(EXCUTABLE_OUTPUT_PATH [out_path])
​    #设置库文件的输出路径(LIBRARY_OUTPUT_PATH是全局变量)
​    SET(LIBRARY_OUTPUT_PATH [output_path])
​    #设置c++编译参数(CMAKE_CXX_FLAGS是全局变量)
​    SET(CMAKE_CXX_FLAGS "-Wall std=c++11")
​    #设置源文件集合（SOURCE_FILES是本地变量即自定义变量）
​    SET(SOURCE_FILES main.cpp test.cpp)
```

**常用命令-include_directories**

- 设置头文件目录
- 相当于g++选项中的-l参数
```
   #可以用相对或绝对路径，也可以用自定义的变量值
   include_directories(./include ${MY_INCLUDE})  
```
**常用命令-add_executable**
添加可执行文件
```
    add_executable(<name> ${SRC_LIST})
```
**常用命令-target_link_libraries**

- 将若干个库连接到目标文件
- 链接的顺序应当符合gcc链接顺序规则，被连接的库放在依赖它的库的后面，即如果上面的命令中，lib1依赖于lib2，lib2又依赖于lib3，则在上面命令中必须严格按照lib1 lib2 lib3的顺序排列，否则会报错
```
格式 target_link_libraries(<name> lib1 lib2 lib3)
#如果出现互相依赖的静态库，CMake会允许依赖图中包含循环依赖，如：
​    add_library（A STATIC a.c）
​    add_library（B STATIC b.c）
​    target_link_libraries(A B)
​    target_link_libraries(B A)
​    add_executable(main main.c)
```
**常用命令-add_subdirectory**
如果当前目录下还有子目录是可以使用add_subdirectory，子目录中也需要包含有CMakeLists.txt
```
# sub_dir指定包含CMakeLists.txt和源文件的子目录位置
# binary_dir是输出路径，一般可以不指定
add_subdirectory(sub_dir [binary_dir])
```
**常用命令-file**

- 文件操作命令
```
#将message写入filename文件中，会覆盖文件原有内容
file(WRITE filename "message")

# 将message写入filename文件中，会追加在文件末尾
file(APPEND filename "message")
#从filename文件中读取内容并存储到var变量中，如果指定了numBytes和offset，
#则从offset处开始最多度numBytst个字节，另外如果指定了HEX参数，则内容会以十六进制形式存储在var变量中
file(READ filename var [LIMIT numBytes] [OFFSET offset] [HEX])
#重名名文件
file(RENAME <oldname> <newname>)
#删除文件，等于rm命令
file(REMOVE [file1 ...])
#递归的执行删除文件命令，等于rm -r
file(REMOVE_RECURSE [file1 ...])
#根据指定的url下载文件
#timeout超时时间；下载的状态会保存到status中；下载日志会被保存到log；sum指定所下载文件预期的MD5值，如果指定会自动进行比对
#如果不一致，则返回一个错误；SHOW_PROGRESS,进度信息会以状态信息的形式被打印出来
file(DOWNLOAD url file [TIMEOUT timeout] [STATUS status] [LOG log] [EXPECTED_MD5 sum] [SHOW_PROGRESS])
#创建目录
file(MAKE_DIRECTORY [dir1 dir2 ...])
#会把path转换为以unix的开头的cmake风格路径，保存在result中
file(TO_CMAKE_PATH path result)
#它会把cmake风格的路径转换为本地路径风格：windows下用“\”,而unix下用“/”
file(TO_NATIVE_PATH path result)
#将会为所有匹配查询表达式的文件生成一个list，并将该list存储进变量variable里，如果一个表达式指定了RELATIVE，返回的结果
#将会是相对于给定路径的相对路径，查询表达式例子：*.cxx,*.vt?
#NOTE:按照官方文档的说法，不建议使用file的GLOB指令来收集工程的源文件
file(GLOB variable [RELATIVE path] [globbing expressions]....)

```
**常用命令-set_directory_properties**
```
set_directory_properties(PROPERTIES prop1 value1 prop2 value2)
```
- 设置某个路径的一种属性

- prop1,prop2代表属性，取值为：
```
INCLUDE_DIRECTORIES

LINK_DIRECTORIES

INCLUDE_PEGULAR_EXPRESSION

ADDITIONAL_MAKE_CLEAN_FILES
```
**常用命令-set_property**
```
 set_property(<GLOBAL |
                DIRECTORY [dir] |
                TARGET[target ...]> |
                SOURCE [src1 ....] |
                TEST [test1 ...]) |
                CACHE [ENTRY1 ...]>
                [APPEND]
                PROPERTY <name> [value ...])
                
```
- 在给定的作用域内设置一个命名的属性

- PROPERTY参数是必须的
- 第一个参数决定了属性可以影响的作用域：
```
GLOBAL:全局作用域

DIRECTORY:默认当前路径，也可以用[dir] 指定路径

TARGET:目标作用域，可以是0个或多个已有目标

SOURCE:源文件作用域，可以是0个或多个源文件

TEST:测试作用域，可以是0个或多个源文件

CACHE:必须指定0个或多个cache中已有的条目
```
**多个源文件处理**

- 如果源文件很多，把所有文件一个个加入很麻烦，可以使用aux_source_directory命令或file命令，会查找指定目录下的所有源文件，然后将结果存进指定变量名
```
#cmake最低版本
cmake_minimum_required(VERSION 3.6.0)

# 查找当前目录所有源文件 并将名称保存到 DIR_SRCS变量
# 不能查找子目录
aux_source_directory(. DIR_SRCS)
# 也可以使用
# file(GLOB DIR_SRCS *.c *.cpp)

add_library(
     native-lib
     SHARED
     ${DIR_SRCS})
```

**多目录多源文件处理**

- 主目录中的CMakeLists.txt添加add_subdirectory(child)命令，指明本地项目包含一个子项目child。并在target_link_libraries指明本项目需要链接一个名为child的库。
- 子目录child中创建CMakeLists.txt，这里child编译为共享库。
```
#cmake最低版本
cmake_minimum_required(VERSION 3.6.0)

# 查找当前目录所有源文件 并将名称保存到 DIR_SRCS变量
# 不能查找子目录
aux_source_directory(. DIR_SRCS)
# 添加child子目录下的cmakelist
add_subdirectory(child)

add_library(
     native-lib
     SHARED
     ${DIR_SRCS})
 target_link_libraries(native child)
#--------------------------------
#child目录下的CMakeLists.txt
cmake_minimum_required(VERSION 3.6.0)
# 查找当前目录所有源文件 并将名称保存到 DIR_SRCS变量
# 不能查找子目录
aux_source_directory(. DIR_LIB_SRCS)
add_library(
     native-lib
     SHARED
     ${DIR_LIB_SRCS})

```
**添加与编译库（Android6.0版本以前）**

- 假如我们本地项目引用了libhello-jni.so。
- 添加add_library命令，第一个参数是模块名，第二个参数SHARED表示动态库，STARIC表示静态库，第三个参数IMPORTED表示以打入的形式添加
- 添加set_target_properties命令设置打入路径属性。
- 将import-lib添加到target_link_libraries命令参数中，native-lib需要链接到libhello-jni.so模块

```
#cmake最低版本
cmake_minimum_required(VERSION 3.6.0)
#使用IMPORTED标志告知 CMake只希望将库导入到项目中
#如果是静态库则将shared改为static
add_library(
     libhello-jni.so
     SHARED
     IMPORTED)
#参数分别为：库、属性、导入地址、库所在的地址
set_target_properties(
                libhello-jni.so
                PROPERTIES
                IMPORTED_LOCATION
                <路径>/libhello-jni.so)
 aux_source_directory(. DIR_SRCS) 
 add_library(
     native-lib
     SHARED
     ${DIR_SRCS})
 target_link_libraries(native-lib libhello-jni)

```
**添加编译库（Android6.0版本以后）**

- 在Android6.0及以上版本，如果使用上节的方法添加预编译动态库的话，会有问题。我们可以使用另外一种方式来配置。
```
#set命令定义一个变量
#CMAKE_C_FLAGS:c的参数，会传递给编译器
#-L：库的查找路径
set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS}" -L[so所在目录])
```
**添加头文件目录**

- 为了确保CMake可以在编译时定位头文件，使用include_directories,相当于g++选项中的-l参数。这样就可以使用#include "xx.h"否则需要使用#include “path/xx.h”
```
#cmake最低版本
cmake_minimum_required(VERSION 3.6.0)
#设置头文件目录
include_directories(<文件目录>)
set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -L${CMAKE_SOURCE_DIR}/../../../libs/${ANDROID_ABI}")
aux_source_directory(. DIR_SRCS) 
add_library(
     native-lib
     SHARED
     ${DIR_SRCS})
 target_link_libraries(native libhello-jni)
```
**Build.gradle配置**
```
android {

    defaultConfig {
        
        externalNativeBuild {
            cmake {
                //使用的编译器clang/gcc
                //cmake默认就是gnustl_static
                arguments "DANDROID_TOOLCAAIN=clang","-DANDROID_STL=gnustl_static"
                //指定cflags和cppflags，效果和cmakelist使用一样
                cppFlags ""
                cFlags “”
                //指定需要编译的cpu架构
                abiFilters 'arm64-v8a', "x86", "armeabi-v7a", "x86_64"
            }
        }
    }
    //指定库的位置
    sourceSets {
        main {
            jniLibs.srcDirs(['libs'])
        }
    }
       
    externalNativeBuild {
        cmake {
            path file('src/main/cpp/CMakeLists.txt')
        }
    }

}
```
**CMake message说明**
目前 Android Studio 默认支持的 CMake 最新版本是 3.10.2，但是这个版本 message 输出根本看不到。记得之前 3.6.x 版本中 build 之后还是可以看到的，这篇文章主要说下 message() 的使用和日志输出位置。

**一、CMake 命令 message**
CMake 如果要像用户展示消息需要可以使用 message() API，类似与 Android 中的 Log 输出。

message([] “message to display” …)
关键字，可以指定消息的类型：

(none) = 重要消息
STATUS = 附带消息
WARNING = CMake警告，继续处理
AUTHOR_WARNING = CMake警告（dev），继续处理
SEND_ERROR = CMake错误，继续处理，但跳过生成
FATAL_ERROR = CMake错误，停止处理和生成
DEPRECATION = 如果分别启用了变量CMAKE_ERROR_DEPRECATED或CMAKE_WARN_DEPRECATED，则CMake弃用错误或警告，否则无消息

**二、使用示例**
简单演示上面前三种，打印一些变量值：
```
message("CMAKE_SOURCE_DIR = ${CMAKE_SOURCE_DIR}")
message(STATUS "PROJECT_SOURCE_DIR = ${PROJECT_SOURCE_DIR}")
message(WARNING "CMAKE_BINARY_DIR = ${CMAKE_BINARY_DIR}")
message(WARNING "CMAKE_CXX_FLAGS = ${CMAKE_CXX_FLAGS}")
```
打印结果：
```
CMAKE_SOURCE_DIR = /Users/sundy/Documents/project/github/CmakeStudy/app/src/main/cpp/.
PROJECT_SOURCE_DIR = /Users/sundy/Documents/project/github/CmakeStudy/app/src/main/cpp
CMake Warning at /Users/sundy/Documents/project/github/CmakeStudy/app/src/main/cpp/CMakeLists.txt:15 (message):
CMAKE_BINARY_DIR =/Users/sundy/Documents/project/github/CmakeStudy/app/.cxx/cmake/debug/x86

CMake Warning at /Users/sundy/Documents/project/github/CmakeStudy/app/src/main/cpp/CMakeLists.txt:16 (message):
CMAKE_CXX_FLAGS = -g -DANDROID -fdata-sections -ffunction-sections
-funwind-tables -fstack-protector-strong -no-canonical-prefixes
-mstackrealign -fno-addrsig -Wa,–noexecstack -Wformat
-Werror=format-security
可以看到 (none) 和 STATUS 输出区别不大；
WARNING 会打印出 CMakeLists.txt 目录以及 行号，后面才是我们打印的内容。
```
**三、message 输出**
这里就要注意了，3.6 版本和 3.8 版本输出的位置不一样。

**CMake 3.6 版本**
方式一：
使用 Gradle build
build
Run 中查看编译日志

日志输出
方式二：

直接 build

Build中查看编译日志

注意：build 第一次会执行编译，之后不再修改 CMakeLists.txt 中的内容，再次 build，CMake 是不会重新编译的，需要修改一下 CMakeLists.txt ，比如加个空格之类的，或者将 app/.externalNativeBuild 文件夹删掉，也可以重新编译。

**CMake 3.8 版本**
还是使用上面方式 build 后查看 Run 中的编译日志：

日志输出

- 我们会发现 (none) 和 STATUS 的 message 不会没有打印，项目开发中，我在这里被坑了，开始以为是 API 改变了，查了很多文档也没有说明，直到尝试使用 WARNING 级别，才发现问题的所在。


在 3.8 版本 (none) 和 STATUS 两种级别的 message 不会直接打印出来，有两种解决办法：

- 方式一：如果想直接看到打印信息，使用 WARNING 以上的级别进行打印。

- 方式二：就是下面的兼容方案，推荐使用，可以兼容 CMake 所有版本。

**四、兼容方案**
其实，输出内容 CMake 会保存到文本中，包括前面的 3.6 版本，目录是：
```
y o u r − p r o j / your-proj/your−proj/your-module/.cxx/cmake/debug/$ARCH/build_output.txt
$your-module 默认是"app"，根据项目不同，可能不一样。
$ARCH 是 CPU 类型（armeabi-v7a，arm64-v8a，x86等） 。
```
————————————————

版权声明：本文为CSDN博主「竹子的人生」的原创文章，遵循CC 4.0 BY-SA版权协议，转载请附上原文出处链接及本声明。

原文链接：https://blog.csdn.net/u011150681/article/details/103630938