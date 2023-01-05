# CMake从入门到精通

# 为什么需要CMake

你或许听过好几种 Make 工具，例如 ：

- GNU Make
- QT 的 qmake
- 微软的 MS nmake
- BSD Make（pmake）Makepp

这些 Make 工具遵循着不同的规范和标准，所执行的 Makefile 格式也千差万别。这样就带来了一个严峻的问题：**如果软件想跨平台，必须要保证能够在不同平台编译**。而如果使用上面的 Make 工具，就得为每一种标准写一次 Makefile ，这将是一件让人抓狂的工作。

CMake就是针对上面问题所设计的工具：它首先允许开发者编写一种平台无关的 CMakeList.txt 文件来定制整个编译流程，然后再根据目标用户的平台进一步生成所需的本地化 Makefile 和工程文件，如 Unix 的 Makefile 或 Windows 的 Visual Studio 工程。从而做到“Write once, run everywhere”。

# CMake是什么

CMake 是一个跨平台的安装（编译）工具，可以用简单、统一的语句来描述所有平台的安装或编译过程。能够输出不同编译器的 makefile 或 project 文件。

CMake 使用 CMakeLists.txt文件作为项目组织文件，CMake 并非跨平台编译工具，而是项目构建工具，可以在不同的平台上根据构建参数生成工程项目，例如 Windows 平台下可以构建 Visual Studio 工程 或 NMake 工程，可选指定 Visual Studio 的版本；在 Unix 构建 Makefile 工程 。

# CMake的优势

- 开放源代码，使用 BSD 许可证发布
- 跨平台，可以生成 native 编译配置文件。在 linux/unix 平台可以生成 makefile，在 mac 平台可以生成 xcode，在 windows 平台可以生成 msvc 工程的配置文件，能够管理大型项目
- 简化编译构建过程和编译过程，只需要 cmake + make 就可以
- 高效率
- 可扩展，可以为 cmake 编写特定功能的模块，扩充 cmake 功能

# cmake系列目录

- [cmake：centos、win中安装cmake（基础）](https://blog.csdn.net/zhizhengguan/article/details/111712800)
- [cmake：Hello cmake（上）](https://blog.csdn.net/zhizhengguan/article/details/111712909)
- [cmake：同一目录下多个源文件（中）](https://blog.csdn.net/zhizhengguan/article/details/111713182)
- [cmake：不同目录下多个源文件（下）](https://blog.csdn.net/zhizhengguan/article/details/111713320)
- [cmake：定义并打印某个消息](https://blog.csdn.net/zhizhengguan/article/details/84826253)
- [cmake：add_library生成静态库和动态库](https://blog.csdn.net/zhizhengguan/article/details/111713847)
- [cmake：make install安装项目](https://blog.csdn.net/zhizhengguan/article/details/118416116)
- [cmake：find_package 添加第三方依赖库](https://blog.csdn.net/zhizhengguan/article/details/111714160)
- [cmake：检查当前是什么操作系统（CMAKE_SYSTEM_NAME）](https://blog.csdn.net/zhizhengguan/article/details/112151244)
- [cmake：设置C++标准](https://blog.csdn.net/zhizhengguan/article/details/124690878)
- [cmake：设置编译选项](https://blog.csdn.net/zhizhengguan/article/details/111743586)
- [cmake：project指定C/C++混合编程](https://blog.csdn.net/zhizhengguan/article/details/80939063)
- [cmake：生成动态链接库并使用](https://blog.csdn.net/zhizhengguan/article/details/107041871)
- [cmake：pkg_check_modules](https://blog.csdn.net/zhizhengguan/article/details/111826697)
- [cmake：add_definitions定义的变量可以直接在程序中使用](https://blog.csdn.net/zhizhengguan/article/details/112151625)
- [cmake：在 CMake 生成的 VS2015 工程中保持源码文件的目录组织](https://blog.csdn.net/zhizhengguan/article/details/125080498)
- [cmake：切换生成器](https://blog.csdn.net/zhizhengguan/article/details/124965321)
- [cmake：解决c++11的phread库问题：undefined reference to `pthread_create’](https://blog.csdn.net/zhizhengguan/article/details/107163891)
- [cmake：cl is not a full path and was not found in the PATH.](https://blog.csdn.net/zhizhengguan/article/details/112443132)

# 参考

- [cmake官方文档](https://cmake.org/cmake/help/latest/index.html#)
- [cmake官方入门教程](https://cmake.org/cmake/help/latest/guide/tutorial/index.html#guide:CMake Tutorial)
- [cmake:设置编译选项的讲究(add_compile_options和CMAKE_CXX_FLAGS的区别)](https://blog.csdn.net/10km/article/details/51731959)
- [Cmake语句find_package()函数](https://blog.csdn.net/sen873591769/article/details/90183015)
- [CMake快速入门教程：实战](https://www.kancloud.cn/wepeng/php/1172776)
- [“轻松搞定CMake”系列博客概述](https://blog.csdn.net/zhanghm1995/article/details/105455764)
- [现代cmake语法学习](https://github.com/ttroy50/cmake-examples/tree/master/01-basic/A-hello-cmake)
- [Linux下CMake简明教程](https://blog.csdn.net/whahu1989/article/details/82078563)
- [CMake学习资源汇总](https://blog.csdn.net/zhanghm1995/article/details/105455490)
- [等待研究](https://www.kancloud.cn/wepeng/php/1172776)
- [其他教程](https://juejin.im/post/5a6f32e86fb9a01ca6031230)