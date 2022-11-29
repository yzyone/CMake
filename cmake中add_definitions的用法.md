# cmake中add_definitions的用法

add_definitions的功能和C/C++中的#define是一样的

比如我有如下两个文件，一个源文件main.cpp，一个CMakeLists.txt
源文件main.cpp

```
#include <iostream>
int main()
{
#ifdef TEST_IT_CMAKE
	std::cout<<"in ifdef"<<std::endl;
#endif
	std::cout<<"not in ifdef"<<std::endl;
}
```

cmake文件CMakeLists.txt

```
cmake_minimum_required(VERSION 3.10)
project(optiontest)

add_executable(optiontest main.cpp)
option(TEST_IT_CMAKE "test" ON)
message(${TEST_IT_CMAKE})
if(TEST_IT_CMAKE)
	message("itis" ${TEST_IT_CMAKE})
	add_definitions(-DTEST_IT_CMAKE)
endif()
```

其中的下边两部分一般是连在一起用的（message语句仅为了更好的输出变量，无特殊意义）

```
option(TEST_IT_CMAKE "test" ON)
message(${TEST_IT_CMAKE})
if(TEST_IT_CMAKE)
	message("itis" ${TEST_IT_CMAKE})
	add_definitions(-DTEST_IT_CMAKE)
endif()
```

通过option设置一个变量，并通过add_definitions将其转换为#define TEST_IT_CMAKE

当变量为ON时
`option(TEST_IT_CMAKE "test" ON)`

该程序的输出是

```
in ifdef
not in ifdef
```

当变量为OFF时
`option(TEST_IT_CMAKE "test" OFF)`

该程序的输出为

`not in ifdef`

注意，有时修改了CMakeLists.txt，但是却并没有重新编译Makefile，所以放了两个message方便检查

————————————————

版权声明：本文为CSDN博主「大王免礼」的原创文章，遵循CC 4.0 BY-SA版权协议，转载请附上原文出处链接及本声明。

原文链接：https://blog.csdn.net/qq_35699473/article/details/115837708