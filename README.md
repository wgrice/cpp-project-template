# cpp工程目录结构

文档名称：C++工程目录规范

| 序号 | 更改原因 | 版本 | 作者 | 更改日期 | 备注 |
| ---- | -------- | ---- | ---- | -------- | ---- |
|   1   | 创建文档 | v0.0.1 | wg   | 2022.7.1 | 无 |
|      |          |      |      |          |      |
|      |          |      |      |          |      |

# 目录结构

目录命名方式统一为单数形式。

```shell
project_name
├── 3rdparty
├── build
├── cmake
├── deploy
├── doc
├── include
│   └── project_name
├── src
│   └── project_name
├── platform
├── script
├── sample
├── test
├── tool
├── .clang-format
├── .clang-tidy
├── .gitignore
├── build.sh
├── CMakeLists.txt
├── LICENSE
└── README.md
```

- **3rdparty :** 用于存放第三方库，每个第三库以单独目录的形式组织在 3rdparty 目录下。其中每个第三方目录下又有  include  和 lib 分别存放第三方库的头文件和库文件。
- **build :** 用于存放 build 时 cmake 产生的中间文件，其包含子目录 release 和 debug 。
- **cmake :** 用于存放
- **deploy :** 用于存放部署、交付的文件，其包含子目录 bin、lib、include 分别存放本项目最总生成的可执行文件、库文件以及对外所提供的头文件。
- **doc :** 用于存放项目的相关文档。
- **include/project_name :** 用于存放每个模块以及整个工程对外的头文件。具体格式如下文。
- **src/project_name :** 存放源码文件，以及内部头文件。具体格式如下文。
- **platform :** 用于一些交叉编译时所需要的工具链等文件，按照平台进行划分来组织子目录。
- **script :** 包含一些脚本文件，如使用 pipeline 进行自动化构建时所需要的脚本文件，以及一些用于预处理的脚本文件。
- **sample :** 存放示例代码。
- **test :** 分模块存放测试代码。
- **tool :** 包含一些支持项目构建的工具，如编译器等，一般情况下使用软链接。
- **.clang-format :** 使用 clang-format 格式化代码的配置文件。
- **.clang-tidy :** 使用 clang-tidy 静态检查工具的配置文件，基于AST。
- **.gitignore :** 指明git忽略规则。
- **build.sh :** build 脚本文件。
- **CMakeLists.txt :** cmake 文件。
- **LICENSE :** 版权信息说明。
- **README.md :** 存放工程说明文件。

# 源文件目录结构说明

结构示例：

```shell
# example modules tree
project_name
├── module_1
│   ├── dir_1
│   │   ├── something.cc
│   │   └── something.h
│   ├── dir_2
│   ├── module_1.cc
│   ├── CMakeLists.txt
│   └── README.md
├── module_2
│   ├── dir_1
│   ├── dir_2
│   ├── module_2.cc
│   ├── CMakeLists.txt
│   └── README.md
├── module_3
├── app
│   ├── main.cc
└── CMakeLists.txt
```

1. 总源码文件目录以 `project_name` 命名，即与项目同名，存放在项目 src 目录下。
2. 源码文件分模块进行组织，分别以各个模块进行命名存放在 `project_name` 目录下，如示例中的 `module_1` 、`module_2`。
3. 在每个子模块目录下，只包含源文件以及该模块内部所调用的头文件。
4. 每个子模块的根目录下存放该模块的主要功能逻辑代码，如 `module_1.cc`。另外，可按照功能再划分子目录进行源码组织，但不可以出现模块嵌套的情况。
5. 若要包含内部头文件时，包含路径要从 `project_name` 开始路径要完整，如`#include "project_name/module1/dir_1/somthing.h"`，以防止头文件名称冲突的情况，同时遵循了[Google C++编码规范](https://zh-google-styleguide.readthedocs.io/en/latest/google-cpp-styleguide/contents/)。

# 头文件目录结构说明

```shell
# example include tree
include
└── project_name
    ├── module_1
    │   ├── module_1_header_1.h
    │   └── module_1_header_2.h
    ├── module_2
    │   └── module_2.h
    └── project_name.h
```

1. （公共）头文件目录以 `include/project_name` 命名，即文件目录为两级，存放在项目根目录下。该目录只包含所有对外的头文件。
2. 头文件同样分模块进行组织，分别以各个模块进行命名存放在 `include/project_name` 目录下，如示例中的 `module_1` 、`module_1`。
3. `include/project_name` 目录下最多只包含一级子目录，即最多按照模块再划分一级，模块内的功能头文件不再以功能进行划分。
4. 若要包含外部头文件时，包含路径同样要从 `project_name` 开始路径要完整，如`#include "project_name/module_2/module_2.h"`。

# 其他

1. 针对头文件的包含，顶层 `CMakeLists.txt` 只指定 `${CMAKE_SOURCE_DIR}\include` 和 `${CMAKE_SOURCE_DIR}`，以保证所有的包含规则都是从工程根目录开始包含。
2. 添加 `include` 目录使得公共头文件和对内部文件可以分离开，使多个模块之间合作开发时项目内部结构更加清晰。
3. 在 `3rdparty` 下存放的工程中用到的第三方库和第三方源码。第三方库尽量不要直接把静态连接库直接放到git仓库中，应该另外提供链接以便下载，或者提供文档说明库的名称和版本自行安装下载，或者提供git仓库自行编译。第三方源码一般为开源的，只提供git链接。



