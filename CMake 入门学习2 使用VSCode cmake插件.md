# CMake 入门学习2 使用VSCode cmake插件

# 一、准备环境

- vscode
- gcc
- gdb
- cmake
- vscode 安装插件：CMake Tools

# 二、操作步骤

# 1. 新建一个目录

```
mkdir -p d:/documents/cmake
```

新建一个main.cpp文件：

```
#include <iostream>

int main(int argc, char *argv[])
{
   std::cout << "Hello CMake!" << std::endl;
   return 0;
}
```

# 2. 在命令面板输入 Cmake:Quick Start

按提示输入项目名称：

![img](./cmake/b06360d3485e48b69f5d7fd621f34bf1~noop.image)



选择创建可执行文件。

![img](./cmake/d3b6503731d64d8e8a3eb33ec570a728~noop.image)



# 3. 设置cmake

在命令面板选择 CMake Select a kit

按提示选择后，在界面最下方工具栏可以看到：

![img](./cmake/486aca312ca4405f90ef3409cd2dd7a3~noop.image)



# 三、配置项目

# 1. 选择编译环境

在命令面板选择CMake: Select Variant，选择Debug。

# 2. 执行configure

在命令面板选择CMake: configure

# 3. Build

在命令面板选择CMake: build

![img](./cmake/dd08faff40304b96be8fed2896095abf~noop.image)




这时在build目录下就可以运行生成的 mytest.exe程序了。

![img](./cmake/5677cdf889b94018804d6a1aab2adac0~noop.image)



# 四、debug

# 1. 在程序里设置一个断点

![img](./cmake/c265cfc1078d42bbbc03c7672c45ed29~noop.image)



# 2. 在命令面板选择CMake: Debug

![img](./cmake/68a359dec554404bac5a03b76a9daf98~noop.image)




程序会停在断点处，按F5继续执行程序。

更多文档参考：
https://github.com/microsoft/vscode-cmake-tools/blob/develop/docs/README.md