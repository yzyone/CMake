# cmake跨平台编译之判断操作系统平台、32位64位系统

谁吃薄荷糖

于 2021-12-15 21:57:36 发布

1577
 收藏 2
分类专栏： 技术 文章标签： cmake
版权

技术
专栏收录该内容
107 篇文章1 订阅
订阅专栏
判断交叉编译：


```
if(CMAKE_CROSSCOMPILING)
    message(STATUS "Cross Comliling!!!,  ARM")
    set(CURRENT_SYSTEM "Arm")
else()
    message(STATUS "No Cross Comliling!!!")
endif()
```

判断32位64位系统：

```if(CMAKE_CL_64)
    set(CURRENT_PLATFORM "x64")
    message(STATUS "Current Platform is ${CURRENT_PLATFORM}")
else(CMAKE_CL_64)
    set(CURRENT_PLATFORM "x86")
    message(STATUS "Current Platform is ${CURRENT_PLATFORM}")
endif(CMAKE_CL_64)
```

判断操作系统：

```
if(CMAKE_SYSTEM_NAME MATCHES "Linux")
        set(CURRENT_SYSTEM "Linux")
    elseif(CMAKE_SYSTEM_NAME MATCHES "Windows")
        set(CURRENT_SYSTEM "Windows")
   elseif(CMAKE_SYSTEM_NAME MATCHES "FreeBSD")
        set(CURRENT_SYSTEM "FreeBSD")
    endif()
```

代码实例

CMakeLists.txt文件节选：

```
message(STATUS "===============================")
MESSAGE(STATUS "current operation system is ${CMAKE_SYSTEM}")
message(STATUS "current operation system name is ${CMAKE_SYSTEM_NAME}")

if(CMAKE_CL_64)
    set(CURRENT_PLATFORM "x64")
    message(STATUS "Current Platform is ${CURRENT_PLATFORM}")
else(CMAKE_CL_64)
    set(CURRENT_PLATFORM "x86")
    message(STATUS "Current Platform is ${CURRENT_PLATFORM}")
endif(CMAKE_CL_64)

if(CMAKE_CROSSCOMPILING)
    message(STATUS "Cross Comliling!!!,  ARM")
    set(CURRENT_SYSTEM "Arm")
else()
    message(STATUS "No Cross Comliling!!!")

    #根据不同平台给CURRENT_SYSTEM命名
    if(CMAKE_SYSTEM_NAME MATCHES "Linux")
        set(CURRENT_SYSTEM "Linux")
    elseif(CMAKE_SYSTEM_NAME MATCHES "Windows")
        set(CURRENT_SYSTEM "Windows")

   elseif(CMAKE_SYSTEM_NAME MATCHES "FreeBSD")
        set(CURRENT_SYSTEM "FreeBSD")
    endif()

endif()
```


cmake打印信息(Ubuntu20系统x86测试测试)：

```
root@root:/home/root123/testdemo/demo1/build# cmake ..
-- ===============================
-- current operation system is Linux-5.11.0-41-generic
-- current operation system name is Linux
-- Current Platform is x86
-- No Cross Comliling!!!
-- ===============================
-- /home/root123/testdemo/demo1/Lib/Debug/Linux/x86
-- ===============================
```


谁吃薄荷糖

微信公众号

————————————————

版权声明：本文为CSDN博主「谁吃薄荷糖」的原创文章，遵循CC 4.0 BY-SA版权协议，转载请附上原文出处链接及本声明。

原文链接：https://blog.csdn.net/zcc1229936385/article/details/121962646
