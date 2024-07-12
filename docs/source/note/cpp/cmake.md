# Cmake

## Cmake基础知识

### 什么是cmake?

CMake（Cross-Platform Makefile Generator）是一个开源的跨平台构建系统生成工具。它设计用于简化和自动化软件的构建过程。CMake 不直接构建项目，而是生成用于特定构建工具的配置文件，例如Makefile或Visual Studio项目文件。

以下是CMake的一些关键特点：

1. **跨平台：** CMake是一个跨平台工具，可以在多种操作系统上运行，包括Linux、Windows和macOS。这使得开发者可以使用相同的构建配置文件在不同的平台上构建其项目。

2. **生成系统：** CMake生成针对特定构建系统的配置文件，如Makefile、Ninja、Visual Studio项目文件等。这使得开发者能够使用不同的构建工具，而不需要手动编写和维护这些配置文件。

3. **简化构建过程：** CMake使用简单的语法和高级功能，以简化构建过程的配置。它提供了一个抽象层，使得开发者可以更容易地配置源代码、库和可执行文件。

4. **模块化：** CMake支持模块化设计，使得可以分开管理项目的不同部分。开发者可以将构建配置信息分成多个CMakeLists.txt文件，然后在主项目中引用它们。

5. **可扩展性：** CMake具有很好的可扩展性，允许开发者编写自定义模块和函数，以满足特定项目的需求。

开发者通常通过创建一个名为CMakeLists.txt的文件来描述项目的构建过程。这个文件包含了项目的目录结构、依赖关系、编译选项等信息。然后，使用`cmake`命令生成配置文件，然后再使用生成的构建系统进行构建。

总体而言，CMake是一个强大的构建工具，广泛用于管理和构建各种规模的C/C++项目。

### 编译流程

编译过程是将源代码转换为可执行程序的过程。这个过程包含多个步骤，通常可以概括为以下阶段：

1. **预处理（Preprocessing）：**
   - **任务：** 移除注释、处理宏、包含头文件等。
   - **工具：** 预处理器（例如，`gcc -E`）。

2. **编译（Compilation）：**
   - **任务：** 将预处理后的源代码翻译成汇编代码。
   - **工具：** 编译器（例如，`gcc -S`）。

3. **汇编（Assembly）：**
   - **任务：** 将汇编代码转换为机器代码（目标文件）。
   - **工具：** 汇编器（例如，`as`）。

4. **链接（Linking）：**
   - **任务：** 将多个目标文件和库文件组合成一个可执行程序。
   - **工具：** 链接器（例如，`ld`）。

5. **加载（Loading）：**
   - **任务：** 将可执行程序加载到内存中，准备执行。
   - **工具：** 操作系统的加载器。

整个过程可以通过一条命令完成，这就是通常使用的编译命令，例如：

```bash
gcc source.c -o executable
```

这个命令包括了预处理、编译、汇编、链接这一系列步骤。一些工具链也可以将这些步骤拆分开来，以便在需要时执行特定的阶段。

在开发过程中，一般只需要关注编写源代码，然后使用编译器将其转换为可执行程序。编译器和链接器会处理大部分复杂的细节。

![image-20230309130644912](https://subingwen.cn/cmake/CMake-primer/image-20230309130644912.png)

`蓝色虚线`表示使用makefile构建项目的过程
`红色实线`表示使用cmake构建项目的过程

### make和makefile

`make` 是一个用于自动构建和编译程序的工具，通常与 `makefile` 文件一起使用。它能够根据文件的依赖关系，判断哪些文件需要重新编译，以及如何进行编译。`make` 主要用于管理大型项目的编译过程，确保只有发生了变化的文件才会重新编译，从而提高编译效率。

下面是 `make` 和 `makefile` 的简要说明：

1. **make：** 
   - **任务：** `make` 命令用于执行 `makefile` 文件中定义的规则，进行自动化构建。
   - **使用：** 在命令行中，你可以使用 `make` 命令来触发构建过程。

2. **makefile：** 
   - **任务：** `makefile` 是一个包含构建规则的文本文件，定义了源文件、目标文件以及构建的规则和依赖关系。
   - **语法：** `makefile` 使用一定的语法规则，如规则、目标、依赖、命令等，来描述构建过程。
   - **示例：**
     
     ```makefile
     all: program
     
     program: main.o functions.o
         gcc -o program main.o functions.o
     
     main.o: main.c
         gcc -c main.c
     
     functions.o: functions.c
         gcc -c functions.c
     ```
   - **使用：** 在项目目录中，可以运行 `make` 命令，它会查找名为 `makefile` 或 `Makefile` 的文件并执行其中定义的规则。

通过编写 `makefile` 文件，开发者可以定义项目的编译规则，包括源文件、目标文件、依赖关系以及构建命令。这使得在大型项目中，通过简单的命令就能够执行复杂的构建过程，确保只有必要的文件被重新编译。

### 库文件

库文件（Library files）是包含预编译代码和数据的文件，它们通常被用于软件开发中，以便在多个程序或项目中共享可重用的功能和资源。库文件的目的是提供一种模块化的方式，使得开发者能够在不同的项目中重复使用已经编写和测试过的代码。

有两种主要类型的库文件：静态库和共享库。

1. **静态库（Static Library）：**
   - 静态库是在编译时被链接到程序中的，它会被整体复制到可执行文件中。
   - 静态库的文件扩展名通常是 `.a`（在Unix/Linux系统中）或 `.lib`（在Windows系统中）。
   - 优点：在编译时会被整合到可执行文件中，不需要在运行时加载，因此在运行时没有额外的加载开销。
   - 缺点：每个使用该库的程序都包含一份拷贝，可能导致可执行文件变得较大。

2. **共享库（Dynamic Library 或 Shared Library）：**
   - 共享库是在运行时被动态加载到内存中的，多个程序可以共享同一份库。
   - 共享库的文件扩展名通常是 `.so`（在Unix/Linux系统中）或 `.dll`（在Windows系统中）。
   - 优点：节省内存，因为多个程序可以共享一份库；允许库的更新而无需重新编译所有使用该库的程序。
   - 缺点：在运行时需要加载，可能会引入一些额外的开销。

库文件包含了被其他程序调用的函数、类、变量等代码和数据。通过使用库文件，开发者可以避免在每个项目中都重新实现相同的功能，从而提高代码的复用性和维护性。

## Cmake的使用

### 注释

CMake 使用`#` 进行行注释，可以放在任何位置。

```cmake
# 这是一个CmakeLists文件
cmake_minimum_required(VERSION 3.0.0)
```

- 注释块

```cmake
#[[ 这是一个 CMakeLists.txt 文件。
这是一个 CMakeLists.txt 文件
这是一个 CMakeLists.txt 文件]]

cmake_minimum_required(VERSION 3.0.0)

```

### 只有源文件

```
$ tree
.
├── add.c
├── div.c
├── head.h
├── main.c
├── mult.c
└── sub.c
```

在上述源文件所在目录下添加一个新文件 CMakeLists.txt，文件内容如下：

```cmake
cmake_minimum_required(VERSION 3.0)
project(CALC)
add_executable(app add.c div.c main.c mult.c sub.c)
```

- `cmake_minimum_required(VERSION 3.0)`指定使用的 cmake 的最低版本

- `project`定义工程名称，并可指定工程的版本、工程描述、web主页地址、支持的语言（默认情况支持所有语言），如果不需要这些都是可以忽略的，只需要指定出工程名字即可。

```cmake
# PROJECT 指令的语法是：
project(<PROJECT-NAME> [<language-name>...])
project(<PROJECT-NAME>
       [VERSION <major>[.<minor>[.<patch>[.<tweak>]]]]
       [DESCRIPTION <project-description-string>]
       [HOMEPAGE_URL <url-string>]
       [LANGUAGES <language-name>...])
```

- `add_executable`定义工程会生成一个可执行程序
  - 这里的可执行程序名和project中的项目名没有任何关系
  - 源文件名可以是一个也可以是多个，如有多个可用空格或`;`间隔

```cmake
# 样式1
add_executable(app add.c div.c main.c mult.c sub.c)
# 样式2
add_executable(app add.c;div.c;main.c;mult.c;sub.c)
```

**执行cmake命令**`cmake .`:**当前目录**,后执行`make`命令,可以执行可执行程序名

**创建build**执行`cmake .. `上一级目录,后执行`make`命令,可以执行可执行程序名

**使用build目录编译的好处:**使源文件结构清晰

### SET的使用

#### 1. 定义变量

   ```cmake
   # SET 指令的语法是：
   # [] 中的参数为可选项, 如不需要可以不写
   SET(VAR [VALUE] [CACHE TYPE DOCSTRING [FORCE]])
   ```

   ```cmake
   # 方式1: 各个源文件之间使用空格间隔
   # set(SRC_LIST add.c  div.c   main.c  mult.c  sub.c)
   
   # 方式2: 各个源文件之间使用分号 ; 间隔
   set(SRC_LIST add.c;div.c;main.c;mult.c;sub.c)
   add_executable(app  ${SRC_LIST})
   ```

   

#### 2. 指定使用的c++标准

   - cmakefiles文件设置

   ```cmake
     #增加-std=c++11
     set(CMAKE_CXX_STANDARD 11)
     #增加-std=c++14
     set(CMAKE_CXX_STANDARD 14)
     #增加-std=c++17
     set(CMAKE_CXX_STANDARD 17)
   ```

   - 输出命令行指定宏

   ```
     #增加-std=c++11
   cmake CMakeLists.txt文件路径 -DCMAKE_CXX_STANDARD=11
   #增加-std=c++14
   cmake CMakeLists.txt文件路径 -DCMAKE_CXX_STANDARD=14
   #增加-std=c++17
   ```

3. 指定输出路径(绝对路径)

```cmake
set(HOME /home/robin/Linux/Sort)
set(EXECUTABLE_OUTPUT_PATH ${HOME}/bin)
```

