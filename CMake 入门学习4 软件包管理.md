# CMake 入门学习4 软件包管理

# 一、Linux下的软件包管理

# 1. 检索已安装的软件包

pkg-config 是一个用于在 Linux 系统中查找和检索已安装软件包信息的工具，如：
在终端中执行以下命令可以查看是否安装了 OpenSSL：

```
pkg-config --list-all | grep openssl
```

如果终端输出了 openssl 相关信息，表示该软件包已经安装。
如果没有找到，则可能需要在系统中安装 OpenSSL。在 Ubuntu 上可以使用以下命令进行安装：

```
sudo apt-get install libssl-dev
```

安装完成后再次执行 pkg-config --list-all | grep openssl 命令进行检查。

# 2. 让自己编译软件支持pkg-config搜索

如果你想将自己编译的软件添加到 pkg-config 中，需要在安装时提供对应的 .pc 文件，以便 pkg-config 可以使用。

一般来说，需要在安装时将 .pc 文件安装到特定目录，例如 /usr/lib/pkgconfig 或 /usr/local/lib/pkgconfig，这些目录是默认的 pkg-config 搜索路径。

要创建 .pc 文件，可以使用文本编辑器手动创建，或者使用 pkg-config 的工具自动生成。
以下是一个 .pc 文件的示例：

```
prefix=/usr/local
exec_prefix=${prefix}
libdir=${exec_prefix}/lib

Name: example
Description: An example library
Version: 1.0
Libs: -L${libdir} -lexample
Cflags: -I${prefix}/include
```

参数说明：

- prefix 软件的安装路径，
- Name 软件包名称
- Description 描述信息
- Version 软件版本号
- Libs 链接库
- Cflags 编译选项。

# 3. 在CMakeLists查找已安装的软件包

CMakeLists.txt里使用find_package用来查找已经安装的软件包。

```
find_package(OpenSSL REQUIRED)
target_link_libraries(my_program OpenSSL::SSL OpenSSL::Crypto)
```

这将查找 OpenSSL 的 .pc 文件，并在链接时自动添加 OpenSSL 库和头文件。如果没有找到对应的软件包，会出现错误提示。后面还会有详细说明。

# 二、适合Windows下的包管理工具

# 1. vcpkg

在 Windows 平台上，可以使用 vcpkg 工具来管理和查找已安装的软件包信息。vcpkg 是一个开源的 C++ 库管理器，采用C++编写，支持 Windows、Linux 和 macOS 等多个平台，并且可以与 CMake 集成使用。

Vcpkg使用的是静态库，这意味着Vcpkg管理的软件包可以在不同平台之间共享，并且构建速度比动态库更快。Vcpkg支持多个编译器，并且可以自动生成CMake的配置文件，方便使用CMake来构建项目。

在使用 vcpkg 之前，需要先在系统中安装 vcpkg 工具，并使用 vcpkg install 命令来安装所需的库。安装好的库文件会保存在 vcpkg 安装目录下的 vcpkg/installed 目录中。

通过在 CMakeLists.txt 中调用 vcpkg 提供的命令和函数，可以方便地获取并链接所需的库，同样地，例如使用 find_package() 函数来查找 vcpkg 中安装的库：

```
find_package(OpenSSL REQUIRED)
target_link_libraries(my_app OpenSSL::SSL OpenSSL::Crypto)
```

常用命令：

1. vcpkg install <library> 安装库
2. vcpkg install <library>:<version> 安装指定版本库
3. vcpkg update 更新库版本
4. vcpkg remove <library> 卸载库

# 2. Conan

Conan是一个开源的C++分布式包管理器，使用Python编写，可用于C/C++和其他语言。Conan通过远程存储库（如conan-center）提供了大量的现成的软件包，并且可以使用自定义的存储库来管理本地的软件包。
Conan具有灵活的配置选项，支持多种不同的构建系统和编译器，并且可以在不同平台之间共享二进制文件，从而提高了构建速度。

# (1) 安装Conan

```
pip install conan
```

# (2) 配置Conan

```
conan config install https://github.com/bincrafters/conan-remote-templates.git
# 创建一个默认的 profile
conan profile new default --detect
# profile 中的 libstdc++11 选项设置为 libstdc++11
conan profile update settings.compiler.libcxx=libstdc++11 default
```

# (3) 创建工程

创建一个工程，然后在工程的根目录下创建一个名为 conanfile.txt 的文件，并添加需要依赖的库。例如，如果需要依赖 OpenSSL 库，可以在 conanfile.txt 中添加以下内容：

```
[requires]
OpenSSL/1.1.1k

[generators]
cmake
```

上述内容表示需要依赖 OpenSSL 库的 1.1.1k 版本，并且使用 cmake 生成器。

# (4) 安装依赖库

```
conan install .
```

# (5) 使用依赖库

如果要在CmakeLists.txt中使用Conan 安装的依赖库，则在CmakeLists.txt中添加内容：

```
include(${CMAKE_BINARY_DIR}/conanbuildinfo.cmake)
conan_basic_setup()

target_link_libraries(<target_name> ${CONAN_LIBS})
```

示例的内容会包含Conan 生成的conanbuildinfo.cmake文件，设置编译选项和链接选项，以便使用依赖库。
可以通过${CONAN_LIBS}变量引用依赖库：

```
find_package(Boost REQUIRED COMPONENTS program_options)
target_link_libraries(my_target ${CONAN_LIBS} Boost::program_options)
```

在这个例子中，${CONAN_LIBS}变量将会自动包含Conan管理的所有依赖库，同时链接Boost::program_options库。

注意，${CONAN_LIBS}变量应该放在target_link_libraries()命令的最前面，因为依赖库的链接顺序很重要，Conan管理的依赖库可能会有依赖关系，必须按照正确的顺序链接。

# 三、CMakeLists的 find_package指令

如前面例子介绍，在 CMakeLists.txt 中，find_package 用于在系统中查找需要的软件包。其用法是：

```
find_package(PackageName [Version] [EXACT] [QUIET] [REQUIRED]
  [[COMPONENTS] [component1] [component2] ...])
```

参数说明：

- PackageName 需要查找的软件包名称；
- Version 需要查找的软件包版本；
- EXACT 表示需要严格按照指定版本进行查找；
- QUIET 表示在查找失败时不输出警告信息；
- REQUIRED 表示查找不到软件包时停止构建；
- COMPONENTS 参数用于指定需要查找的软件包中的组件名称。

如果软件包提供了多个组件，可以通过 COMPONENTS 参数指定需要查找的组件名称：

```
find_package(OpenSSL REQUIRED COMPONENTS crypto)
target_link_libraries(my_program OpenSSL::SSL OpenSSL::Crypto)
```

查找成功后，CMake 会设置一些变量，可以使用这些变量来链接软件包；但find_package 在不同的操作系统和环境下的表现可能不一致，需要参考不同的软件包的文档来使用。

其中，find_package() 函数会在 vcpkg 中查找 OpenSSL 库，并设置 OpenSSL_INCLUDE_DIR 和 OpenSSL_LIBRARIES 两个变量。
target_link_libraries() 函数用于将 OpenSSL 库链接到 my_app 应用程序中。

# 四、LINK_DIRECTORIES

LINK_DIRECTORIES 用于指定库文件的目录。这样编译器在链接时就能够从这些目录中找到需要的库文件。语法如下：

```
LINK_DIRECTORIES(directory1 [directory2 ...])
```

# 五、LINK_LIBRARIES

LINK_LIBRARIES 用于指定需要链接的库文件。这个命令可以接受库文件的完整路径，也可以使用 CMake 变量和 target，CMake 会在链接时自动找到这些库。语法如下：

```
LINK_LIBRARIES(library1 [library2 ...] [debug|optimized <library>] ...)
```

当链接库文件时，一般使用 LINK_LIBRARIES。LINK_LIBRARIES 会自动添加 -l 参数，也会在库文件名后面加上后缀（如在 Linux 下会自动添加 .so 后缀）。而 LINK_DIRECTORIES 则是指定编译器查找库文件的目录。

# 六、 target_link_libraries

target_link_libraries 用于将库链接到一个 CMake 目标中，比如可执行文件或库。语法如下：

```
target_link_libraries(target library1 library2 ...)
```

其中 target 表示要链接的目标，library1，library2 等表示要链接的库。

需要区分 LINK_LIBRARIES和target_link_libraries，其中LINK_LIBRARIES是变量，下面使用中很明显看出它们的关系：

```
add_executable(myapp main.cpp)
target_link_libraries(myapp ${LINK_LIBRARIES})
```

除了本文提到的工具，还有一些C/C++的包管理工具：

1. NuGet：一种针对 .NET 开发的包管理器，可用于安装、卸载、更新和管理 .NET 应用程序的包。
2. Hunter：一个跨平台的 C/C++ 包管理器，提供与 CMake 集成的插件，可以在多个操作系统和构建平台上使用。
3. Biicode：一个面向 C/C++ 开发的包管理器，旨在简化依赖关系的处理。
4. CPM：一个轻量级的 C++ 包管理器，使用 CMakeLists.txt 文件来定义项目依赖关系。

另外有些编译环境会有自带的包管理工具，如ESP32，使用了IDF即自己的包管理工具。