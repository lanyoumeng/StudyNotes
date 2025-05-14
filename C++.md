# 问题

1. ```go
   func (p *ParasiteThick) WriteTxt(ctx *utility.Context) {
       p.LastUpdateIndexTime = time.Now()
       //z这里写入路径不一样，可以使用协程 优化
       for labelType, labelData := range p.LabelDic {

          cellLabelTxtPath := path.Join(ImageROOT, fmt.Sprintf(CELL_LABEL_TXT_PATH, p.GlobalTaskId, ParasiteThickType, labelType))
          if err := WriteLabelData(ctx, cellLabelTxtPath, labelData); err != nil {
             utility.Errorf(ctx, "WriteTxt WriteLabelData err:%+v", err)
             //continue
          }
          labelData.Sequence = []string{}
          labelData.Data = map[string]string{}
          p.LabelDic[labelType] = labelData
       }
   }
   ```

2. **捕获的生命周期问题**

   如果 `imageByte` 是一个局部变量，而 Lambda 被异步运行（例如在线程池中调度），那么 `imageByte` 的生命周期可能在 Lambda 执行之前结束。这会导致 Lambda 捕获的引用变成悬空引用，访问时表现为未定义行为。

   **解决方法**： 改用按值捕获来确保 `imageByte` 的生命周期在 Lambda 中有效：

   ```
   cpp复制代码image0.WriteRawImageDataWg.run([this, imageByte, fop, imageName] {
       spdlog::info("WriteImageData begin 00000000 ,imageByte.size() = {}", imageByte.size());
       image0.WriteImageData(fop, imageByte, imageName, image0.WhiteRawIndexesContent, image0.WhiteRawIndexesMux);
   });
   ```

   > **注意**：按值捕获会拷贝 `imageByte`，这对大数据结构可能有性能影响，但可以确保数据独立性。

3. 添加错误处理

4. tcmalloc、内存释放、析构函数

5. lamada 捕获 引用

6. map 不在存储 Mat，改为字节数据， 到合成玻片时，再转为 Mat 处理---这个太慢，因为原来是一次转换五次使用，变为了五次转换五次使用

7. 找一个库可以把 std::vector<std::vector<std::vector<uint8_t>>> image64 直接合并成一张大图片

8. pprof 和火焰图和内存分析

9. -lprofiler -ltcmalloc

10.

11. 部分 task 没加锁

12.

13. 玻片地图模块每次

14. 更新 http 的 url 接口所有字段都要

15. upload_status 在 finishscan 和 stoptask 中，未更新

    concentrationcalculate 注释掉了疟疾代码

16. 标注系统 的 gettaskuploadstatus 需要更新

# CygnusSC

Cygnus 系列项目玻片地图和写入模块

# 任务管理

写入模块：只有程序异常结束时才会 设为 Failture

# 使用库

```C++
json:nlohmann-json #include <nlohmann/json.hpp>  using json = nlohmann::json;
base64:cpp-base64
redis:redis-plus-plus #include <sw/redis++/redis++.h>
配置文件：yaml-cpp  #include <yaml-cpp/yaml.h> /  Boost.PropertyTree  <boost/property_tree/ini_parser.hpp>
数据库：libpqxx
opencv4
网络：cpr
日志：spdlog
并发：oneTbb #include <oneapi/tbb/task_group.h>
```

# 玻片地图

## 优化

### 已完成

1. 写入改为 pwrite + 定时刷新到磁盘，并发在指定偏移量写入数据不用加锁

2. 直接读取图片 byte，不再通过 Mat 中转

   - opencv 读取成 Mat 图片占用内存太大、 817.5KB 的图片变为 Mat 是 6.65MB

     导致处理时间倍增

3. base64 只有写入瓦片时从 Mat 转为 byte

4. 瓦片拼接使用 cv::hconcat`和`cv::vconcat

5.

### 待做

1. **libjpeg-turbo** 和 OpenCV 的 `cv::parallel_for_ ` / \#pragma omp parallel for

   1. 缩放、覆盖加速
   2. 字节流转化为 Mat 处理加速

2. 资源释放

3. 玻片地图拼接 多线程、直接覆盖

4. 错误处理

5. 注释

6. Mat 引用优化

7. 初始化时不再使用空白图形填充（申请内存），使用时再申请

   或者直接一个瓦片大图，缩放覆盖 （减少内存占用）

## 耗时

一共 10\*184 个图片，不限制线程数：

- 读取图片到内存： 126.375 s ---------》 全部读取完再处理，内存占用大约 4G

- 所有图片处理的总耗时：13 s 、15s 、17s

- 只写入原图数据：16s

- 只有 瓦片和时序图 处理：10s

- 只有瓦片：8s
- 只有时序图：8s
- 一次时序图覆盖时间：

- 一次原图写入时间：

| 例子          | 处理时间    | CPU 占用 | 内存占用      | 文件刷新间隔 |
| ------------- | ----------- | -------- | ------------- | ------------ |
| 10\*184(1.3G) | 13s         | 线程占满 | 4G            | 5s           |
| 10\*184(1.3G) | 9s 10s 6.5s | 线程占满 | 4G            | 3s           |
|               |             |          |               |              |
|               |             | 10 线程  |               |              |
| 10\*184(1.3G) |             | 15 线程  |               |              |
|               |             | 20 线程  |               |              |
| 20\*184(2.7G) | 14s 10.258s | 线程占满 | 19G-11.6=7.4G | 3S           |
| 30\*184(4G)   |             | 线程占满 | 19G-11.6=7.4G | 3S           |

## 接收数据

暂时从本地文件夹读取

```C++
  LoadAllImages("/home/xiaoying/project/CygnusSC/raw");

//直接读取byte数据
std::vector<uint8_t> imageByte = std::vector<uint8_t>(std::istreambuf_iterator<char>(std::ifstream(entry.path().string(), std::ios::binary).rdbuf()),
                                                                std::istreambuf_iterator<char>());

```

## 图片处理

1. 时序图缩放、覆盖

   原图缩放到时序大图的目标区域

2. 玻片地图缩放、拼接

imwrite 最后写入时序大图使用
imencode 耗时多 0.15s 写入瓦片图时，将 mat 转为 vector<uint8_t>

imread，imwrite，imdecode 和 imencode

## 玻片地图

1.预扫描结束阶段：生成各层空白瓦片地图 2.正式扫描阶段：
图片不断传输，偶数行替换翻转（循环扫描）

每次缩放到各层不同的大小填充不同瓦片的不同区域（每 splitnum 个图片为一个瓦片）
每个瓦片全部填充完毕，内部各个小图片合成一张图片（瓦片）

有边缘区域不足 splitnum\*splitnum 张 ，保留之前的空白图形
当 最后一行/列某一个瓦片的真实图片数 已经填充完毕，开始拼接

3.结束任务时，清理各层无需合成瓦片地图的数据

## 写入

并发偏移写入+定时刷新到磁盘中

```C++
fp = open(fileName.c_str(), O_CREAT | O_RDWR, 0644);
ssize_t bytes_written = pwrite(fp, data.data(), data.size(), offset);

image0.fileSyncTimer->SetTask(1, std::chrono::seconds(syncInterval),
                                [this] { image0.whiteRaw->FlushFile(); });
```

# 写入模块

从 python 传来的数据，以图片为单位，包含算法识别后 该图片的细胞信息（序列号、类别、名称、细胞图片 base64 编码、停止判断 ），

数据分为：

1. 接受的数据
2. 写入 txt 的数据
3. dat

标签计数
调用接口停止硬件

## 数据库

1. CellWhiteCount 需要查询 whiteResult

## 问题

2. 整理 redis 状态
3. redispool 获取的连接记得释放
4. python 对应接口开发
5. 写入字段再和钉钉文档对比一下

# 项目迁移

1. CMakeLists.txt 文件中

   ```
   # 指定 vcpkg 的安装路径
   set(CMAKE_PREFIX_PATH "/home/lanmengyou/code/c++/CygnusSC/vcpkg_installed/x64-linux/share")

   使用的是vcpkg的清单模式 --- 》vcpkg.json
   直接使用vcpkg install下载
   ```

2. signalImage.cpp 加载 图片路径

   ```C++
     LoadAllImages("/home/xiaoying/project/CygnusSC/raw");
   ```

3. Common.h 初始图片路径

   ```C++
     inline std::string DracoCoreROOT = "/home/xiaoying/project/CygnusSC/jpgs";
   ```

4. Server.cpp 任务 id

   ```C++
   std::string stringTaskID = "3";
   ```

# 待学

```
auto 关键字
for-each 循环
右值及移动构造函数 + std::forward + std::move + stl 容器新增的 emplace_back() 方法
std::thread 库、std::chrono 库
智能指针系列（std::shared_ptr/std::unique_ptr/std::weak_ptr），智能指针的实现原理一定要知道，最好是自己实现过
线程库 std::thread + 线程同步技术库 std::mutex/std::condition_variable/std::lock_guard 等
Lamda 表达式（Java 中现在也常常考察 Lamda 表达式的作用）
std::bind/std::function 库
其他的就是一些关键字的用法（override、final、delete），还有就是一些细节如可以像 Java 一样在类成员变量定义处给出初始化值。
make_unique
```

# 基础语法

## 错误处理

1. **简单场景**：优先选择返回值方式，避免过度复杂化。
2. **复杂模块**：全局变量适合需要分离计算结果与错误信息的情况，但需注意线程安全。
3. **复杂逻辑或分类处理**：异常机制适合需要灵活应对多种错误的场景，利于代码的可维护性。

| **方式**                     | **特点**                                                       | **优点**                                                   | **缺点**                                                           | **适用场景**                           |
| ---------------------------- | -------------------------------------------------------------- | ---------------------------------------------------------- | ------------------------------------------------------------------ | -------------------------------------- |
| **通过返回值传递错误信息**   | 返回值指示错误状态（如`0`成功，非`0`失败）；错误码需文档解析。 | 简单直观，调用方便；适合简单错误处理。                     | 返回值局限，函数无法直接返回计算结果；需额外维护错误码表。         | 自定义 SDK 或 API 设计                 |
| **使用全局变量保存错误信息** | 全局变量保存最后一次错误状态；返回值传递计算结果。             | 返回值可直接用于赋值或传递；调用逻辑简化。                 | 可能忘记检查全局变量，易遗留隐患；多线程环境下需额外处理线程安全。 | 系统级 API 或底层模块                  |
| **异常捕获与处理**           | 通过`try-catch-throw`捕获和分类处理异常，支持多种错误类型。    | 清晰划分正常逻辑与异常逻辑；支持复杂错误分类与层次化管理。 | 增加运行开销，可能影响性能；不适合高性能或资源受限场景。           | 面向对象的复杂业务逻辑，多错误类型场景 |

## any 和 variant

在 C++ 中，要定义一个类似于 Go 的 `map[string]interface{}` 的数据结构，通常需要结合 STL 容器和类型擦除机制。具体可以使用 `std::unordered_map<std::string, std::any>` 或 `std::unordered_map<std::string, std::variant<...>>`。

### 使用 `std::any`

`std::any` 是 C++17 引入的类型，可以存储任意类型的值，并在运行时提取。

```cpp
#include <unordered_map>
#include <string>
#include <any>

std::unordered_map<std::string, std::any> RedThresholdM;
```

#### 示例代码

```cpp
#include <iostream>
#include <unordered_map>
#include <string>
#include <any>

int main() {
    std::unordered_map<std::string, std::any> RedThresholdM;

    // 插入不同类型的值
    RedThresholdM["key1"] = 42;                // int
    RedThresholdM["key2"] = std::string("value"); // std::string
    RedThresholdM["key3"] = 3.14;             // double

    // 访问值（需要使用 std::any_cast 进行类型转换）
    try {
        int value = std::any_cast<int>(RedThresholdM["key1"]);
        std::cout << "key1: " << value << std::endl;

        std::string strValue = std::any_cast<std::string>(RedThresholdM["key2"]);
        std::cout << "key2: " << strValue << std::endl;

        double dblValue = std::any_cast<double>(RedThresholdM["key3"]);
        std::cout << "key3: " << dblValue << std::endl;
    } catch (const std::bad_any_cast& e) {
        std::cerr << "Bad cast: " << e.what() << std::endl;
    }

    return 0;
}
```

---

### 使用 `std::variant`

如果已知 `interface{}` 可能的具体类型集合，可以使用 `std::variant`，这样可以避免运行时类型转换带来的性能开销和潜在问题。

```cpp
#include <unordered_map>
#include <string>
#include <variant>

// 定义可能的类型
using VariantType = std::variant<int, std::string, double>;

std::unordered_map<std::string, VariantType> RedThresholdM;
```

#### 示例代码

```cpp
#include <iostream>
#include <unordered_map>
#include <string>
#include <variant>

int main() {
    // 定义 map
    std::unordered_map<std::string, std::variant<int, std::string, double>> RedThresholdM;

    // 插入值
    RedThresholdM["key1"] = 42;                // int
    RedThresholdM["key2"] = std::string("value"); // std::string
    RedThresholdM["key3"] = 3.14;             // double

    // 访问值（使用 std::get）
    try {
        std::cout << "key1: " << std::get<int>(RedThresholdM["key1"]) << std::endl;
        std::cout << "key2: " << std::get<std::string>(RedThresholdM["key2"]) << std::endl;
        std::cout << "key3: " << std::get<double>(RedThresholdM["key3"]) << std::endl;
    } catch (const std::bad_variant_access& e) {
        std::cerr << "Bad access: " << e.what() << std::endl;
    }

    return 0;
}
```

---

### 对比 `std::any` 和 `std::variant`

| **特性**       | **std::any**                 | **std::variant**       |
| -------------- | ---------------------------- | ---------------------- |
| **存储类型**   | 任意类型                     | 预定义的有限类型集合   |
| **访问安全性** | 需要运行时检查，可能抛出异常 | 编译期检查，安全性更高 |
| **性能**       | 较慢，需类型识别和转换       | 较快，直接匹配类型     |
| **用途**       | 需要存储任意类型的场景       | 类型集合已知的场景     |

- 如果类型不确定且动态性强，使用 `std::any`。
- 如果类型范围有限且稳定，使用 `std::variant`。

# 编译运行

## 编译器

C++ 编译器是将 **C++ 源代码**（`.cpp` 文件）翻译为机器代码的工具，最终生成可以在目标平台上运行的可执行文件。以下是关于 C++ 编译器的核心知识和常见编译器的介绍。

---

### 功能

C++ 编译器的主要功能包括：

1. **语法检查：** 检查源代码是否符合 C++ 语法规则。
2. **代码优化：** 根据指定的优化级别，生成高效的目标代码。
3. **预处理：** 处理头文件、宏等预处理指令（如 `#include`、`#define`）。
4. **编译：** 将源代码转换为汇编代码。
5. **汇编：** 将汇编代码转换为机器代码（生成目标文件 `*.o` 或 `*.obj`）。
6. **链接：** 将目标文件与库链接，生成可执行文件。

---

### **2. 常见的 C++ 编译器**

### **（1）GCC（GNU Compiler Collection）**

- **介绍：**
  - 开源跨平台编译器。
  - 提供对 C++ 的完整支持（使用 `g++` 命令）。
  - 兼容多个操作系统（Linux、Windows、macOS 等）。
- **优点：**
  - 免费开源。
  - 社区活跃，支持最新的 C++ 标准。
  - 跨平台，支持多种硬件架构。
- **安装：**
  - Linux 通常默认安装。
  - Windows 可以通过 **MinGW** 或 **Cygwin** 安装。

---

### **（2）Clang**

- **介绍：**
  - 基于 LLVM 的现代化编译器。
  - 支持最新的 C++ 标准，生成高效代码。
  - 强调更好的错误和警告信息。
- **优点：**
  - 输出更清晰的错误信息，适合调试。
  - 支持许多平台，集成度高（如 macOS 默认的 `clang++`）。
- **安装：**
  - macOS 系统自带。
  - 在 Linux 和 Windows 上也可以通过包管理工具安装。

---

### **（3）MSVC（Microsoft Visual C++）**

- **介绍：**
  - 微软为 Windows 开发的专属编译器。
  - 集成在 **Visual Studio** IDE 中。
  - 支持 Windows 专属特性和 Microsoft 平台的最佳优化。
- **优点：**
  - 深度集成 Windows 平台工具链。
  - 提供高级调试功能。
  - 支持 DirectX 和其他微软技术。
- **缺点：**
  - 仅支持 Windows。
  - 对 C++ 标准的支持可能滞后（不过近年来改进很大）。

---

### **（4）Intel C++ Compiler（ICC 或 ICX）**

- **介绍：**
  - Intel 提供的高性能 C++ 编译器。
  - 针对 Intel 硬件进行了深度优化。
- **优点：**
  - 出色的性能优化，尤其是针对 Intel 处理器。
  - 支持 OpenMP 等并行计算框架。
- **缺点：**
  - 商业编译器（但 Intel 提供免费的社区版）。
  - 针对非 Intel 硬件的优化效果较弱。

---

### **（5）其他编译器**

- **Borland C++ Builder：** 适合快速开发 Windows 图形应用程序。
- **Oracle Developer Studio：** 针对 Solaris 和 Linux 的优化编译器。
- **TinyCC：** 一个小型、轻量的 C/C++ 编译器，适合嵌入式环境。

---

### **3. 编译器的选择**

| **需求/环境**    | **推荐编译器**           |
| ---------------- | ------------------------ |
| 开源跨平台项目   | GCC 或 Clang             |
| Windows 原生开发 | MSVC                     |
| 高性能数值计算   | Intel C++ Compiler (ICC) |
| 嵌入式或轻量开发 | GCC 或 TinyCC            |
| macOS 原生开发   | Clang                    |

---

### **4. 使用示例**

### **编译一个简单程序**

示例代码（`hello.cpp`）：

```cpp
#include <iostream>
int main() {
    std::cout << "Hello, World!" << std::endl;
    return 0;
}
```

使用不同编译器进行编译：

1. **GCC (g++):**

   ```bash
   g++ hello.cpp -o hello
   ./hello
   ```

2. **Clang:**

   ```bash
   clang++ hello.cpp -o hello
   ./hello
   ```

3. **MSVC:**

   ```cmd
   cl hello.cpp
   hello.exe
   ```

---

### **5. 支持的 C++ 标准**

现代编译器通常支持多种 C++ 标准，可以通过命令行选项指定：

- **C++98/03:** `-std=c++98`
- **C++11:** `-std=c++11`
- **C++14:** `-std=c++14`
- **C++17:** `-std=c++17`
- **C++20:** `-std=c++20`
- **C++23 (部分支持):** `-std=c++23`

示例：

```bash
g++ -std=c++20 hello.cpp -o hello
```

---

### **6. 调试和优化**

### **调试：**

- 使用 `-g` 选项生成调试信息。

```bash
g++ -g program.cpp -o program
gdb ./program
```

### **优化：**

- 编译时可以使用优化选项：
  - `-O0`：无优化（默认）。
  - `-O1`：基本优化。
  - `-O2`：更高级的优化。
  - `-O3`：最大优化（可能增加编译时间）。

示例：

```bash
g++ -O3 program.cpp -o program
```

---

### **7. 未来趋势**

- 支持最新 C++ 标准的能力是选择编译器的重要因素。
- **Clang 和 GCC** 的竞争促进了 C++ 编译器的快速发展。
- **MSVC** 在 C++ 标准支持方面逐步追赶。

### **结论：**

- **跨平台开发：** 首选 GCC 或 Clang。
- **Windows 原生开发：** 使用 MSVC。
- **性能至上：** 使用 Intel C++ Compiler 或其他针对性优化的编译器。

## 静动态链接

在程序开发中，**静态链接**和**动态链接**是两种常见的链接方式，分别决定了程序在构建和运行时如何处理外部依赖（如库文件）。以下是它们的区别、优缺点和使用场景。

- **静态链接：** 稳定、独立，适合分发单个可执行文件。
- **动态链接：** 模块化、灵活，适合大型项目和需要频繁更新的库。

开发时可以根据项目需求选择合适的链接方式，或者混合使用两者（核心部分静态链接，非核心模块动态链接）。

---

### **1. 静态链接**

### **定义：**

静态链接是在编译期间将目标文件与所需的库直接合并到最终生成的可执行文件中。

### **特点：**

- 库文件的内容会被直接嵌入到可执行文件中。
- 生成的可执行文件是一个独立的、完整的文件。
- 运行时不需要依赖外部库。

### **优点：**

1. **独立性高**：生成的可执行文件不依赖外部库，在运行时不需要额外的动态链接库。
2. **执行速度快**：因为链接工作已经在编译时完成，运行时省去了动态链接的开销。
3. **分发方便**：用户运行程序时不需要安装额外的库。

### **缺点：**

1. **文件体积大**：因为将所有依赖库的代码嵌入可执行文件，导致生成的文件体积较大。
2. **更新不灵活**：如果依赖的库需要更新，需要重新编译整个程序。
3. **资源浪费**：不同的程序如果都静态链接相同的库，会导致重复存储。

### **适用场景：**

- 对运行环境依赖较少的应用程序（如嵌入式系统）。
- 需要单一可执行文件的场景（如便携工具）。
- 安全性要求较高的场景（避免动态库被替换或注入恶意代码）。

---

### **2. 动态链接**

### **定义：**

动态链接是在运行时将程序与动态链接库（如 `.dll`、`.so` 文件）绑定，程序本身只包含对库的引用，而不是库的代码。

### **特点：**

- 库文件独立于可执行文件。
- 可执行文件运行时需要动态链接库的支持。
- 动态库可以被多个程序共享。

### **优点：**

1. **文件体积小**：可执行文件本身不包含库的代码，库的内容单独存储。
2. **更新灵活**：库文件可以单独更新，不需要重新编译整个程序。
3. **共享内存**：多个程序可以共享同一个动态库，节省内存空间。
4. **模块化**：支持按需加载库，实现功能的动态扩展。

### **缺点：**

1. **运行时依赖**：运行时需要确保动态库的正确版本存在，否则程序可能无法运行。
2. **性能开销**：程序在运行时需要加载动态库并完成链接，启动时间会稍慢。
3. **兼容性问题**：动态库的版本变化可能导致兼容性问题（俗称“动态链接地狱”）。

### **适用场景：**

- 操作系统级别的共享库（如标准 C 库、图形库）。
- 大型软件系统，需要模块化和灵活更新。
- 对性能要求不高的应用程序。

---

### **3. 区别对比**

| 特性               | 静态链接                 | 动态链接               |
| ------------------ | ------------------------ | ---------------------- |
| **链接时间**       | 编译时完成               | 运行时完成             |
| **可执行文件大小** | 较大                     | 较小                   |
| **运行时依赖**     | 无需外部库支持           | 需要动态库             |
| **更新库的灵活性** | 不灵活，需重新编译       | 灵活，只需更新库文件   |
| **内存使用**       | 每个程序独立占用库的内存 | 多个程序可共享库的内存 |
| **启动性能**       | 较快                     | 稍慢（需要加载库）     |
| **分发便利性**     | 单文件，易于分发         | 需要附带或安装动态库   |

---

### **4. 示例**

### **静态链接：**

```bash
g++ -o app main.cpp -L./lib -lstaticlib -static
```

- `-static` 表示启用静态链接。
- `-L` 指定库文件的路径。
- `-lstaticlib` 链接静态库文件 `libstaticlib.a`。

### **动态链接：**

```bash
g++ -o app main.cpp -L./lib -ldynamiclib
```

- 不使用 `-static` 时默认是动态链接。
- `-ldynamiclib` 链接动态库文件 `libdynamiclib.so`。

运行动态链接的程序时，需要确保动态库文件路径在系统的共享库路径中（如 Linux 的 `LD_LIBRARY_PATH` 或 Windows 的 `PATH`）。

---

### **5. 动态链接的运行时动态加载**

在动态链接的基础上，可以通过运行时加载库（如 `dlopen` 或 Windows 的 `LoadLibrary`），实现更灵活的功能扩展。

### **Linux 示例（dlopen）：**

```cpp
#include <dlfcn.h>
#include <iostream>

int main() {
    void* handle = dlopen("libdynamiclib.so", RTLD_LAZY);
    if (!handle) {
        std::cerr << "Error loading library: " << dlerror() << std::endl;
        return 1;
    }

    typedef void (*func_t)();
    func_t my_func = (func_t)dlsym(handle, "my_function");
    if (!my_func) {
        std::cerr << "Error finding symbol: " << dlerror() << std::endl;
        dlclose(handle);
        return 1;
    }

    my_func();  // 调用动态库中的函数

    dlclose(handle);  // 释放动态库
    return 0;
}
```

---

# 数据库

```
//疟疾scanType分为三种：Thin,Thick,ThickThin
```

1. sql： task 表

2. redis：

   1. 时序小图 base64---> json --->videoImg
   2. 通知下位机传输预扫描数据成功 "lpush", "preScan", "0"

3. 数据：

   ```go
   //FinishPreScan的下位机数据
   //	stringTaskID := data[0] ？markNum
   	stepNumberStr := data[1]
   	stepLength := data[2]
   	scanAreaStr := data[3]
   	//slideThumbnail := data[4]
   	isFlat := data[5]
   	redStepNumberStr := data[6]

   //"scan_region": {"bianyuan": [0, 0, 0, 0], "platelet_region": [0, 0, 0, 0], "red_region": [0, 0, 0, 0], "tiwei": [0, 0, 0, 0], "white_region": [0, 0, 0, 0]}, "scan_type": "All"}
   	var scanRegion common.ScanRegionStructNew

   //scan下位机传输数据
   row := data[1]
   col := data[2]
   xString := strings.Trim(data[3], "-")
   yString := strings.Trim(data[4], "-")
   zString := strings.Trim(data[5], "-")
   imageContent := data[6]
   imageModelType := data[7]

   //红和血小板公用一套图片
   ```

# 技术栈

## go

| 名称   | 用途         | 功能 | 端口 |
| ------ | ------------ | ---- | ---- |
| gin    |              |      |      |
| redigo | redis 客户端 |      |      |
|        |              |      |      |
|        |              |      |      |

### 数据交互

下位机、算法的交互是通过 redis

### main 流程

有**瓦片图和时序图**

#### createtask

#### prescan

判断扫描的整体区域 rows、cols

#### FinishPreScan

```
FinishPreScan 阶段：
初始化空白瓦片地图
假如是 6rows   8cols
瓦片名称：%d-%d
imageMap[serialNumber]([][]image.Image  )   key:"%d-%d" value:二维数组图像--此时使用空白图片填充
```

，白、红/血小板（使用一套逻辑）

#### scan

##### 往返扫描

扫描时是往返扫描 ，下标从 1 开始

是三角坐标系，xyz

| 1,1 | 1,2 | 1,3 |
| --- | --- | --- |
| 2,3 | 2,2 | 2,1 |
| 3,1 | 3,2 |     |
|     |     |     |

##### 三个

这三个是同时进行的

1. 瓦片地图（分层进行拼接） 初始化时全为空白图像，通过下位机的图片数据进行填充

   长宽减半可能导致小数 7.5，改为 8

2. 向算法推送数据，（redis 存的是图片数据）

3. 处理算法的结果

##### imageModelType

```
p.Info.ModelType = imageModelType 、//
```

```go
//imageModelType = common.PlateletModelType
const RedModelType = "Red"        //红模块
const WhiteModelType = "White"    //白模块
const PlateletModelType = "PC"    //血小板模块
const ParasiteThinType = "Thin"   //疟疾薄模块
const ParasiteThickType = "Thick" //疟疾厚模块
```

##### 区域

```
ParasiteThinType 疟疾薄 RedCols RedRows
RedModelType和PlateletModelType 6行8列   RedCols RedRows

WhiteModelType 和 ParasiteThickType 使用common.TaskInfo的  Rows Cols
```

#### FinishScan

注意上位机停止和下位机停止会有时间差距，比如算法慢要进行阻塞

#### StopTask

结束任务，异常结束或者用户取消

### 1.图片拼接模块

分块大小 `splitNum`

行列数和图片模型类型的关系

```go
	if imageMapType == common.WhiteModelType || imageMapType == common.ParasiteThickType {
		finishCol = p.FinishWhiteCol
		finishRow = p.FinishWhiteRow
	}
	if imageMapType == common.ParasiteThinType || imageMapType == common.RedModelType {
		finishCol = p.FinishRedCol
		finishRow = p.FinishRedRow
	}
// 血小板模型
	if imageMapType == common.PlateletModelType {
		finishRow = 6
		finishCol = 8
	}
```

| 层级                            | splitnum（瓦片<br />splitNum\*splitNum 的图片组成） | 总瓦片数                             |     |
| ------------------------------- | --------------------------------------------------- | ------------------------------------ | --- |
| FirstImageMap: 第一层瓦片地图   | 8                                                   | ⌈ rows/splitNum ⌉\*⌈ cols/splitNum ⌉ |     |
| SecondImageMap: 第二层瓦片地图  | 64                                                  |                                      |     |
| ThirdImageMap: 第三层瓦片地图   | 1                                                   |                                      |     |
| SeventhImageMap: 第七层瓦片地图 | 4                                                   |                                      |     |
| EighthImageMap: 第八层瓦片地图  | 2                                                   |                                      |     |
|                                 |                                                     |                                      |     |

```

```

#### 时序图

采取的是本次写入上一次传来的时序图，finish 阶段写入最后的时序图

### 2.处理算法输出结果

白模块、红模块、血小板模块、疟疾薄模块、疟疾厚模块、疟疾红计数、疟疾白计数

```go
TaskType  string //normal qc
ScanType  string //Count All Area ThickThin
SlideId   string //玻片号
ModelType string //red write pc
"PC"    //血小板模块
```

```
// QCTaskType 质量控制

txt文本文件
dat数据文件
```

#### 红模块

红细胞有不同的标签，最后是统计不同标签的数量：ThrombocyteAggregation,GiantThrombocyte,LargePlatelets,GranulesReducedMissingPlatelets,MalformedPlatelets,AbnormalPlatelet,OtherP

```go
type SingleLabelValue struct {
	Count  int           `json:"count"`
	Log    []interface{} `json:"log"`
	Ration any           `json:"ration"` //
}

// 红细胞结果
type RedTaskResult struct {
	AllCount   int                         `json:"all_count"`   //已经分析的细胞总数
	LabelCount map[string]SingleLabelValue `json:"label_count"` //标签计数 key:标签名 value:SingleLabelValue struc
}

{"all_count":586,"label_count":{"BasophilicStippling":{"count":0,"log":[],"ration":""},"BiphasicCell":{"count":0,"log":[],"ration":""},"BiteCell":{"count":0,"log":[],"ration":""},"BlisterCell":{"count":0,"log":[],"ration":""},"BurrCell":{"count":0,"log":[],"ration":""},"CabotRing":{"count":0,"log":[],"ration":""},"Elliptocyte":{"count":2,"log":[],"ration":""},"Eperythrozoon":{"count":0,"log":[],"ration":""},"Erythrocyte":{"count":372,"log":[],"ration":""},"HaemoglobinH":{"count":0,"log":[],"ration":""},"HenizBodies":{"count":0,"log":[],"ration":""},"HowellJollyBody":{"count":0,"log":[],"ration":""},"Hyperpigmented":{"count":0,"log":[],"ration":""},"Hypochromic":{"count":0,"log":[],"ration":""},"IntracellularHaemoglobinCrystals":{"count":0,"log":[],"ration":""},"Keratocyte":{"count":0,"log":[],"ration":""},"Macrocyte":{"count":0,"log":[],"ration":""},"Microcyte":{"count":165,"log":[],"ration":""},"Ovalocyte":{"count":51,"log":[],"ration":""},"Pappenheimer":{"count":0,"log":[],"ration":""},"Poikilocyte":{"count":93,"log":[],"ration":""},"PolychromatophilicRedCell":{"count":0,"log":[],"ration":""},"RedCellAgglutinates":{"count":0,"log":[],"ration":""},"RedNoCount":{"count":1656,"log":null,"ration":""},"Rouleaux":{"count":0,"log":[],"ration":""},"Schistocyte":{"count":4,"log":[],"ration":""},"SickieCell":{"count":0,"log":[],"ration":""},"Spherocyte":{"count":0,"log":[],"ration":""},"SpurCell":{"count":0,"log":[],"ration":""},"Stomatocyte":{"count":0,"log":[],"ration":""},"TargetCell":{"count":1,"log":[],"ration":""},"TeardropCell":{"count":0,"log":[],"ration":""},"UnevenSize":{"count":0,"log":[],"ration":""}}}

{"scan_type": "Count", "scan_region": {"white_region": [0, 0, 0, 0], "red_region": [0, 0, 0, 0], "platelet_region": [0, 0, 0, 0], "tiwei": [0, 0, 0, 0], "bianyuan": [0, 0, 0, 0]}, "scan_parameter": {"white_count": 50}, "module_info": {"PC": true, "Platelet": true, "Red": true, "White": true, "pc_factor_num": 0}}
```

## python

## c++

### 瓦片地图部分

设计架构

需要重构的有

整体是一个调用函数，不停接收（while（true））新图像数据（socket 接收数据），
原来在 redis 中获取的数据改为函数调用

1. 传参，判断不同阶段
2. 心跳 // 待定
3. 创建任务
4. 预扫描
   1. 状态检测、数据校验
   2. 更新任务状态 ，当前任务是否已删除
   3. 写入玻片缩略图
5. 预扫描结束：
   1. 状态检测、数据校验
   2. 反序列化预扫描数据,更新任务信息
   3. 初始化时序图和各层瓦片地图
   4. 更新任务信息
   5. 通知前端预扫描成功
   6. 启动定时任务，文件的数据被定时写入磁盘
6. 正式扫描：传输处理图形数据
   1. 状态检测、数据校验
   2. 获取图像信息
   3. 更新进度条
   4. 独立软件逻辑 图片计数，更新进度
   5. 上传标注系统
   6. 处理图片数据：填充-- 瓦片地图、时序图、原始图
      1. HandleTransportData 处理扫描数据
      2. 原图写入本地硬盘 根据索引写入文件
      3. ---索引实时发送 redis
      4. 生成瓦片地图和时序图并写入磁盘
      5. 向算法推送图片数据 通过 redis
7. 扫描结束
   1. 状态检测、数据校验
   2. 等待瓦片地图和时序图生成
   3. 写入瓦片地图和时序图 以及索引文件
   4. 停止定时任务、关闭文件操作
   5. 清理 redis 任务
   6. 没有算法模块，更新任务状态、清理时序小图和大图
   7. 扫描结束，发送停止相应算法模型信号 redis
8. 停止任务
   1. 状态检测、数据校验
   2. 更新数据库任务状态
   3. 通知前端任务失败 redis
   4. 等待瓦片地图和时序图生成
   5. 清理 redis 数据
   6. 写入瓦片地图和时序图 以及索引文件
   7. 如果有 上传标注系统
   8. C++算法结束 redis
9. 默认

## **瓦片大小**和**地图缩放**

在玻片地图（Tiled Map）中，**瓦片大小**和**地图缩放**之间的关系是至关重要的。瓦片大小决定了单个瓦片在地图中的物理尺寸，而地图的缩放则会影响整个地图的显示方式，包括瓦片如何在屏幕上渲染。理解二者之间的关系对于优化游戏地图的视觉效果和性能至关重要。

### 1. **瓦片大小（Tile Size）**

瓦片的大小是指单个瓦片的物理尺寸，通常以像素为单位。常见的瓦片尺寸有 32x32 像素、64x64 像素等。瓦片大小在设计时是固定的，这意味着每个瓦片占据的空间大小在设计时就已经决定了。

- **瓦片大小定义**：每个瓦片代表一个地图单元，可以是地面、墙壁、障碍物等。瓦片的大小由开发者在游戏地图编辑器中设置（如 Tiled 编辑器），并会影响地图的整体布局和游戏玩法。

例如，瓦片大小为 32x32 像素意味着每个瓦片在游戏中显示为 32 像素宽和 32 像素高的矩形。

### 2. **地图缩放（Map Scaling）**

地图缩放通常是指游戏中相机视图的缩放，它决定了玩家所看到的地图区域大小以及瓦片如何在屏幕上呈现。缩放可以影响地图和瓦片的显示比例，进而影响游戏的视觉效果和操作体验。

- **缩放级别（Zoom Level）**：通常，游戏中的相机会根据不同的缩放级别来调整显示区域。不同的缩放级别会让地图变得更大或更小，瓦片的显示尺寸也会发生变化。

  - **放大（Zoom In）**：放大地图时，瓦片会显示得更大，每个瓦片占用更多的屏幕空间。地图的可见区域变小，通常是玩家的视角会变得更近，细节更加突出。
  - **缩小（Zoom Out）**：缩小时，瓦片会变小，更多的地图区域会被显示在屏幕上，通常会导致瓦片看起来较为密集，游戏中的世界显得更广阔。

### 3. **瓦片大小与缩放的关系**

瓦片的显示大小会随着地图缩放发生变化。具体来说，当地图进行缩放时，瓦片的屏幕尺寸会按比例调整，通常遵循以下几个原则：

- **固定瓦片大小**：如果瓦片的大小在设计时已经固定，那么它们的物理尺寸不会改变，而是通过缩放相机来改变地图的显示方式。也就是说，地图缩放不会改变瓦片本身的大小，而是改变它们在屏幕上的显示比例。

  - 例如，在默认的缩放级别下，每个瓦片可能是 32x32 像素，但如果缩放级别为 2（放大两倍），那么每个瓦片在屏幕上显示为 64x64 像素。
  - 如果缩小地图到原来的 0.5 倍，瓦片则显示为 16x16 像素。

- **缩放因子**：缩放因子（Zoom Factor）是决定瓦片如何缩放的关键参数。缩放因子是一个比率，通常可以设定为 1x、2x、0.5x 等。这个因子会影响瓦片的显示尺寸以及地图上瓦片的密集程度。

- **放大地图**：每个瓦片变大，显示的瓦片数量减少。

  **缩小地图**：每个瓦片变小，显示的瓦片数量增加。

### 4. **瓦片与相机的配合**

瓦片的缩放通常与相机的缩放或视角（field of view）配合使用。在大多数 2D 游戏中，相机控制游戏中的视图，缩放相机意味着玩家可以看到更大或更小的区域。

- **相机缩放**：相机的缩放控制着显示的区域大小。例如，放大相机会让地图显示更小的部分，但每个瓦片的显示会变大。缩小相机则会让显示的区域增大，但每个瓦片的显示会变小。

  - 在没有缩放的情况下，相机显示的区域与瓦片大小直接相关。例如，如果瓦片大小为 32x32 像素，且相机显示 10x10 的瓦片区域，那么相机的视野大小为 320x320 像素（32 像素 × 10）。
  - 如果相机进行 2x 缩放，则相机的视野会减半，显示区域变成 160x160 像素，但每个瓦片会显示为 64x64 像素。

- **游戏中的视觉效果**：通过动态调整相机的缩放级别，游戏可以实现多种视觉效果，如放大细节、显示更多区域、提供视差效果等。

### 5. **性能影响**

瓦片大小与地图缩放也会对游戏性能产生影响，尤其是在渲染和物理计算时。

- **较大瓦片（放大缩放）**：如果瓦片被放大，它们会占用更多的屏幕空间，从而减少需要渲染的瓦片数量。这通常会降低渲染开销，因为在同一时间内，屏幕上显示的瓦片数量减少。

- **较小瓦片（缩小缩放）**：缩小时，瓦片的显示尺寸会减小，导致在屏幕上显示更多的瓦片。这意味着游戏需要渲染更多的瓦片，可能会增加渲染的负担，尤其是在大规模地图中。

因此，开发者需要在瓦片大小、地图缩放和游戏性能之间找到一个平衡，确保地图的显示效果和游戏性能都能得到优化。

### 6. **瓦片缩放的插值与平滑**

在缩放瓦片时，尤其是当瓦片显示尺寸发生大幅度变化时，可能会出现锯齿状边缘或者模糊的效果。为了避免这种情况，游戏引擎通常会使用纹理插值算法（如双线性插值或各向异性过滤）来平滑瓦片的显示效果，尤其是在放大缩小时。

- **双线性插值（Bilinear Filtering）**：这种方法通过计算四个临近像素的加权平均值来平滑瓦片的显示，使其在缩放时看起来更加平滑。
- **各向异性过滤（Anisotropic Filtering）**：这种方法优化了瓦片在不同角度下的显示效果，尤其是在大角度的缩放下，提供更高质量的视觉表现。

### 总结

瓦片大小与地图缩放之间的关系是非常重要的，理解二者之间的互动对于优化地图的显示效果和游戏性能至关重要。通过合理调整瓦片的设计大小、缩放因子、相机视角和渲染策略，开发者可以在游戏中创造出清晰、流畅且具有良好视觉体验的地图。

# 各类库

#include <fmt/ranges.h>

# 内存

## 比较

1. **普通应用程序**：默认使用 `ptmalloc` 即可，简单可靠。
2. **高并发场景**：推荐使用 `tcmalloc`，能显著提升分配效率。
3. **长时间运行，碎片敏感场景**：选择 `jemalloc`，能更好地控制内存碎片和提升性能。

| **特性**       | **ptmalloc**            | **tcmalloc**                       | **jemalloc**                             |
| -------------- | ----------------------- | ---------------------------------- | ---------------------------------------- |
| **线程支持**   | 使用锁，线程安全        | 线程缓存，性能优越                 | 多区域（arena），性能优越                |
| **内存碎片**   | 碎片控制一般            | 碎片优化较好                       | 碎片优化优秀                             |
| **分配性能**   | 普通性能                | 极高性能                           | 高性能                                   |
| **内存使用**   | 内存占用较少            | 内存占用较多（线程缓存带来的开销） | 内存占用中等                             |
| **调优能力**   | 调优能力有限            | 适合高并发、大量小对象场景         | 灵活调优，适合复杂场景                   |
| **适用场景**   | 普通应用程序            | 高并发应用（如 Web 服务器）        | 长时间运行、对碎片敏感的服务（如数据库） |
| **代表性应用** | 默认在 Linux 系统中使用 | Google 内部服务，gRPC，Bigtable 等 | Redis、Cassandra，FreeBSD 系统           |

## tcmalloc

`tcmalloc`（Thread-Caching Malloc）是 Google 开发的一款高性能内存分配器，主要用来优化内存分配的速度和减少多线程环境中的锁竞争。在实际使用中，可以通过以下步骤将 `tcmalloc` 集成到项目中并发挥其优势。

`tcmalloc` 是高性能应用中非常实用的内存分配优化工具，使用步骤包括：

1. 安装和集成（动态或静态链接）。
2. 配置环境变量进行性能调优。
3. 利用调试工具监控和分析内存使用。

在高并发应用程序（如 Web 服务器、实时计算系统）中，它可以显著提升内存分配性能并减少锁竞争，适合性能要求较高的场景。

---

### **1. 安装 tcmalloc**

#### **（1）通过系统包管理器安装**

在 Linux 系统上，许多发行版提供了 `tcmalloc` 的二进制包，例如：

- **Debian/Ubuntu 系统**：

  ```bash
  sudo apt-get update
  sudo apt-get install libgoogle-perftools-dev
  ```

- **CentOS/RHEL 系统**：

  ```bash
  sudo yum install gperftools
  ```

#### **（2）从源码安装**

如果需要最新版本或特定编译选项，可以从源码编译安装：

1. **克隆代码库**：

   ```bash
   git clone https://github.com/gperftools/gperftools.git
   cd gperftools
   ```

2. **编译和安装**：

   ```bash
   ./autogen.sh
   ./configure
   make -j$(nproc)
   sudo make install
   ```

3. **更新动态库路径**： 添加 `libtcmalloc.so` 到系统库路径（如 `/usr/local/lib`）：

   ```bash
   echo "/usr/local/lib" | sudo tee -a /etc/ld.so.conf.d/gperftools.conf
   sudo ldconfig
   ```

---

### **2. 使用 tcmalloc**

#### **（1）动态链接 tcmalloc**

在编译和运行阶段，通过动态链接库将 `tcmalloc` 替代默认的内存分配器：

1. **链接 tcmalloc**： 在链接阶段添加 `-ltcmalloc`：

   ```bash
   g++ -o my_program my_program.cpp -ltcmalloc
   ```

2. **运行时加载 tcmalloc**： 如果无法修改程序的编译参数，可以通过环境变量加载 `tcmalloc` 动态库：

   ```bash
   LD_PRELOAD=/usr/lib/libtcmalloc.so ./my_program
   ```

#### **（2）静态链接 tcmalloc**

静态链接适用于需要完全控制二进制的场景：

```bash
g++ -o my_program my_program.cpp -Wl,--whole-archive -ltcmalloc -Wl,--no-whole-archive
```

---

### **3. 配置 tcmalloc**

`tcmalloc` 提供多种配置选项，可以通过环境变量调整其行为。以下是常用选项：

| **环境变量**                            | **描述**                                                                              |
| --------------------------------------- | ------------------------------------------------------------------------------------- |
| `TCMALLOC_RELEASE_RATE`                 | 控制释放未使用内存回操作的频率，默认为 1。值越大，释放速度越慢，但性能更高。          |
| `TCMALLOC_LARGE_ALLOC_REPORT_THRESHOLD` | 设置大块内存分配的阈值（单位：字节），超过此值的分配会在日志中报告。                  |
| `TCMALLOC_MAX_TOTAL_THREAD_CACHE_BYTES` | 设置线程缓存的总内存限制，默认 16MB。可根据内存大小和应用需求调整此值以减少内存占用。 |

#### **设置环境变量**

在运行程序时，通过 `export` 设置：

```bash
export TCMALLOC_RELEASE_RATE=10
export TCMALLOC_MAX_TOTAL_THREAD_CACHE_BYTES=67108864  # 设置为 64MB
```

或者通过程序内调用 `setenv` 设置环境变量：

```cpp
#include <cstdlib>

int main() {
    setenv("TCMALLOC_RELEASE_RATE", "10", 1);
    setenv("TCMALLOC_MAX_TOTAL_THREAD_CACHE_BYTES", "67108864", 1);
    // Your application logic
    return 0;
}
```

---

### **4. 调试与性能监控**

`tcmalloc` 提供内置的性能调试和监控功能：

#### **（1）导出内存统计**

运行时导出详细的内存统计信息到日志文件：

```bash
pprof --text ./my_program /proc/self/smaps > tcmalloc_stats.txt
```

#### **（2）启用内置调试**

通过环境变量启用调试功能：

```bash
export TCMALLOC_VERBOSE=1
```

#### **（3）分析内存泄漏**

`tcmalloc` 提供了和 Google 的 `HeapProfiler` 工具集成的功能，可用于检测内存泄漏：

```bash
HEAPPROFILE=/tmp/heap_profile ./my_program
```

---

### **5. 注意事项**

1. **多线程下的优势**：
   - `tcmalloc` 在多线程环境下能显著提升分配性能，但在单线程程序中优势不明显。
2. **内存占用的权衡**：
   - 通过线程缓存优化性能可能导致更高的内存占用，尤其是在高并发场景下需要根据实际需求调优。
3. **与系统内存分配器的冲突**：
   - 如果程序中部分组件或第三方库对内存分配器有特殊要求（如 `mmap`），需要确保 `tcmalloc` 的兼容性。

## jemalloc

`jemalloc` 是一个高效的内存分配器，设计用于减少内存碎片、提高分配效率，特别是在高并发应用中表现出色。它广泛用于 Redis、MongoDB、ClickHouse 等高性能系统中。以下是 `jemalloc` 的使用步骤和关键注意事项。

`jemalloc` 是一个强大的内存分配器，特别适合内存分配密集型的高性能场景。其使用步骤包括：

1. 安装并集成到项目中（动态或静态链接）。
2. 配置行为以满足应用需求。
3. 利用调试工具优化和监控内存分配性能。

通过合理调优，`jemalloc` 可以大幅提升内存管理的效率，减少碎片化，同时降低多线程环境下的锁竞争问题。

---

### **1. 安装 jemalloc**

#### **（1）通过包管理器安装**

在一些主流的 Linux 发行版上，可以直接通过包管理器安装 `jemalloc`：

- **Debian/Ubuntu 系统**：

  ```bash
  sudo apt-get update
  sudo apt-get install libjemalloc-dev
  ```

- **CentOS/RHEL 系统**：

  ```bash
  sudo yum install jemalloc
  ```

#### **（2）从源码安装**

如果需要最新版本或自定义编译选项，可以从源码安装：

1. **下载源码**：

   ```bash
   git clone https://github.com/jemalloc/jemalloc.git
   cd jemalloc
   ```

2. **编译和安装**：

   ```bash
   ./autogen.sh
   make -j$(nproc)
   sudo make install
   ```

3. **配置动态库路径**： 确保 `jemalloc` 的库路径已添加到系统动态库搜索路径中（如 `/usr/local/lib`）：

   ```bash
   echo "/usr/local/lib" | sudo tee -a /etc/ld.so.conf.d/jemalloc.conf
   sudo ldconfig
   ```

---

### **2. 使用 jemalloc**

#### **（1）动态链接 jemalloc**

在编译和运行时，可以通过动态链接库启用 `jemalloc`：

1. **链接 jemalloc**： 在链接时添加 `-ljemalloc`：

   ```bash
   g++ -o my_program my_program.cpp -ljemalloc
   ```

2. **运行时加载 jemalloc**： 如果无法修改程序的编译参数，可以通过 `LD_PRELOAD` 环境变量强制加载 `jemalloc`：

   ```bash
   LD_PRELOAD=/usr/lib/libjemalloc.so ./my_program
   ```

#### **（2）静态链接 jemalloc**

静态链接可以避免动态库加载的问题，但会增加可执行文件的体积：

```bash
g++ -o my_program my_program.cpp -Wl,--whole-archive -ljemalloc -Wl,--no-whole-archive
```

---

### **3. 配置 jemalloc**

`jemalloc` 提供丰富的配置选项，通过环境变量可以控制内存分配行为和调试功能。

#### **（1）常用配置选项**

| **环境变量**         | **描述**                                                           |
| -------------------- | ------------------------------------------------------------------ |
| `MALLOC_CONF`        | 全局配置选项，用于设置 `jemalloc` 行为，支持多个配置项用逗号分隔。 |
| `MALLOC_STATS_PRINT` | 设置为 `1` 时，程序结束时会自动输出内存分配统计信息。              |
| `opt.lg_chunk`       | 设置分配的最小大块内存大小，默认为 22（即 4MB）。                  |
| `opt.lg_dirty_mult`  | 控制脏页比例，影响未使用内存的回收速度。                           |
| `opt.prof`           | 启用分配性能分析，用于调试。                                       |

#### **（2）配置方法**

- **通过环境变量**：

  ```bash
  export MALLOC_CONF="lg_chunk:23,lg_dirty_mult:3"
  ./my_program
  ```

- **通过代码设置**： 在运行时通过 `mallctl` 接口设置选项：

  ```cpp
  #include <jemalloc/jemalloc.h>

  int main() {
      size_t sz = 23; // 设置 lg_chunk
      mallctl("opt.lg_chunk", NULL, NULL, &sz, sizeof(sz));
      return 0;
  }
  ```

---

### **4. 调试与性能分析**

`jemalloc` 提供了丰富的调试工具，可以帮助分析内存分配效率、碎片化和泄漏问题。

#### **（1）内存分配统计**

运行时打印内存分配统计信息：

```bash
export MALLOC_STATS_PRINT=1
./my_program
```

#### **（2）启用性能分析**

开启性能分析，通过 `prof` 选项生成内存分配热点信息：

```bash
export MALLOC_CONF="prof:true,prof_prefix:/tmp/jeprof"
./my_program
```

生成的文件可以使用 `jeprof` 工具进行分析：

```bash
jeprof --dot ./my_program /tmp/jeprof.1234.0.f.heap > profile.dot
dot -Tpng profile.dot -o profile.png
```

#### **（3）分析内存碎片**

可以通过 `mallctl` 动态获取内存分配器状态：

```cpp
#include <jemalloc/jemalloc.h>
#include <iostream>

int main() {
    char stats[4096];
    size_t size = sizeof(stats);
    mallctl("stats.print", stats, &size, NULL, 0);
    std::cout << stats;
    return 0;
}
```

---

### **5. 注意事项**

1. **兼容性问题**：
   - 某些程序或库可能依赖系统默认的 `malloc` 行为，使用 `jemalloc` 时需要测试兼容性。
2. **调优与监控**：
   - 根据实际需求调节配置项（如 `lg_chunk` 和 `lg_dirty_mult`），以平衡性能和内存占用。
3. **线程安全性**：
   - `jemalloc` 的设计高度优化了多线程环境下的内存分配，但线程模型复杂的应用仍需注意可能的锁竞争。
4. **与其他内存工具的冲突**：
   - 如果使用了 `valgrind` 或 `asan` 进行内存检查，需要确保工具与 `jemalloc` 兼容。

## 内存分配

在 C++ 项目中，内存分配通常涉及堆内存和栈内存的使用。以下是一些常见的操作和代码，它们会导致内存分配：

### 1. **局部变量和函数参数（栈分配）**

- **局部变量**：函数内部定义的普通变量（例如 `int a = 5;` 或 `CellData cellData;`）会在栈上分配内存。
- **函数参数**：函数参数（如传入的引用或指针）会影响栈内存的使用。

### 2. **动态内存分配（堆分配）**

动态内存分配通过使用 `new` 或 `malloc` 等操作在堆上分配内存，内存由程序员手动管理。

- `new` 操作符

  ：

  ```
  new
  ```

  会在堆上分配内存，并返回指向该内存的指针。常见用法如下：

  ```cpp
  int* p = new int;  // 在堆上分配一个整数
  int* arr = new int[10];  // 在堆上分配一个整数数组
  ```

- **`new[]` 操作符**：用于分配数组。

- **`delete` 和 `delete[]`**：用于释放通过 `new` 或 `new[]` 分配的内存。

### 3. **`std::string` 和 `std::vector` 等容器**

- `std::string`

  ：每个

  ```
  std::string
  ```

  对象通常在堆上分配内存来存储字符串内容。当你使用

  ```
  std::string
  ```

  对象时，内部会自动管理内存。

  ```cpp
  std::string str = "Hello, World!";  // 堆分配内存来存储字符串
  ```

- `std::vector`

  ：

  ```
  std::vector
  ```

  内部会为其元素在堆上分配内存。当你向

  ```
  std::vector
  ```

  添加元素时，底层的动态数组可能会重新分配内存。

  ```cpp
  std::vector<int> vec;  // 动态分配内存来存储 vector 的元素
  vec.push_back(10);     // 如果 vec 未能容纳新元素，则会分配更多内存
  ```

### 4. **`std::map`、`std::set` 等关联容器**

- 容器如 `std::map`、`std::set` 和 `std::unordered_map` 等，在插入元素时可能会在堆上分配内存，尤其是当容器需要扩展或重新平衡时。

### 5. **`std::unique_ptr` 和 `std::shared_ptr`（智能指针）**

- 智能指针本身会分配栈内存（例如

  ```
  std::unique_ptr<int> ptr;
  ```

  ），但它们所管理的对象通常是在堆上分配内存（例如

  ```
  std::make_unique<int>(10)
  ```

  ）。

  ```cpp
  auto ptr = std::make_unique<int>(10);  // 在堆上分配内存
  ```

### 6. **`std::thread`**

- 每当你创建一个新的线程，C++ 会为该线程分配堆内存来存储线程的上下文信息。

  ```cpp
  std::thread t([]() { std::cout << "Hello from thread!" << std::endl; });
  t.join();
  ```

### 7. **类和结构体的成员变量**

- 类和结构体的实例在栈上分配内存，但如果成员变量是指针或其他容器类型（如

  ```
  std::string
  ```

  、

  ```
  std::vector
  ```

  等），它们会在堆上分配内存。例如：

  ```cpp
  struct CellData {
      std::string label;
      std::vector<int> data;
  };
  CellData cellData;  // 在栈上分配 CellData，但 label 和 data 会在堆上分配内存
  ```

### 8. **`std::make_shared` 和 `std::make_unique`**

- 使用

  ```
  std::make_shared
  ```

  或

  ```
  std::make_unique
  ```

  时，会在堆上分配内存给对象，并返回一个智能指针来管理该内存。

  ```cpp
  auto ptr = std::make_shared<int>(10);  // 在堆上分配内存
  ```

### 9. **`malloc` 和 `calloc`**

- 这些 C 风格的内存分配函数会在堆上分配内存，并返回指向该内存的指针。

  ```cpp
  int* p = (int*)malloc(sizeof(int));  // 在堆上分配内存
  ```

### 10. **`std::shared_ptr`、`std::weak_ptr`（共享指针）**

- `std::shared_ptr` 和 `std::weak_ptr` 在管理对象时，会对堆上的内存进行引用计数。`std::shared_ptr` 会在对象引用计数为零时自动释放堆内存。

### 11. **动态数组**

- 使用 `new` 或 `malloc` 等动态分配函数创建的数组（如 `new int[10]` 或 `malloc(sizeof(int) * 10)`）会分配堆内存。

### 12. **对象的复制或赋值（深拷贝）**

- 如果你对类或结构体对象进行深拷贝（通过自定义的拷贝构造函数或赋值运算符），并且类内包含动态分配的内存（例如 `std::string`、`std::vector`），那么这时会发生堆内存的分配。

### 13. **Lambda 表达式（捕获）**

- 如果 Lambda 表达式捕获外部变量并且传递了引用或指针，Lambda 对象的内部可能会导致堆内存分配。尤其是当 Lambda 被存储在 `std::function` 中时。

### 14. **返回局部对象（返回值优化、RVO）**

- 如果你返回一个局部对象，并且编译器没有应用返回值优化（RVO），它可能会在堆上分配内存来存储返回的对象，尽管现代编译器通常会尽量避免这种情况。

### 15. **动态加载的库（DLL 或共享库）**

- 如果在程序中动态加载库（如通过 `dlopen` 或 `LoadLibrary`），则可能涉及到堆内存的分配，尤其是在加载过程中。

---

### 小结

内存分配通常发生在以下几种情况：

- **栈分配**：局部变量、函数参数、对象实例。
- **堆分配**：通过 `new`、`malloc`、`std::vector`、`std::string` 等容器进行的内存分配。
- **动态分配**：智能指针、线程、Lambda 表达式等在堆上分配内存。

## 内存管理

在 C++ 中，内存管理分为两部分：内存分配和内存回收。内存回收指的是释放不再使用的内存，以避免内存泄漏（memory leak）。下面介绍了如何进行内存回收，以及与内存管理相关的策略。

### 1. **栈内存的自动回收**

栈内存是由操作系统自动管理的，当一个局部变量超出其作用域时（例如，函数返回），其栈内存会自动释放。因此，不需要手动回收栈内存。

```cpp
void func() {
    int local_var = 5;  // 栈内存
}  // local_var 超出作用域时，栈内存会自动回收
```

### 2. **手动释放堆内存**

对于通过 `new` 或 `malloc` 等分配的堆内存，必须手动进行释放，以防止内存泄漏。

#### 2.1 **`delete` 和 `delete[]`**

- 使用 `new` 分配的内存应使用 `delete` 来释放。
- 使用 `new[]` 分配的数组应使用 `delete[]` 来释放。

```cpp
int* p = new int;        // 分配内存
delete p;                // 释放内存

int* arr = new int[10];  // 分配数组
delete[] arr;            // 释放数组
```

#### 2.2 **`malloc` 和 `free`**

- 使用 `malloc` 分配的内存需要使用 `free` 来释放。

```cpp
int* p = (int*)malloc(sizeof(int)); // 分配内存
free(p);                             // 释放内存
```

### 3. **智能指针（`std::unique_ptr` 和 `std::shared_ptr`）**

智能指针自动管理内存，不需要手动回收内存，它们在指针不再使用时自动释放所持有的内存。

#### 3.1 **`std::unique_ptr`**

`std::unique_ptr` 是一种独占所有权的智能指针，当 `std::unique_ptr` 被销毁时，它所管理的内存会被自动释放。

```cpp
std::unique_ptr<int> p = std::make_unique<int>(10);  // 自动管理内存
// 当 p 离开作用域时，内存会自动释放
```

#### 3.2 **`std::shared_ptr`**

`std::shared_ptr` 是一种共享所有权的智能指针，它使用引用计数来管理内存。当所有的 `shared_ptr` 对象都不再指向该内存时，内存会自动释放。

```cpp
std::shared_ptr<int> p1 = std::make_shared<int>(10);
std::shared_ptr<int> p2 = p1;  // 引用计数增加
// 当 p1 和 p2 都离开作用域时，内存会被释放
```

### 4. **`std::vector`、`std::string` 等 STL 容器**

STL 容器（如 `std::vector`、`std::string` 等）会自动管理其内部存储的内存。例如，`std::vector` 在添加或删除元素时，会自动进行内存的分配和回收。

```cpp
std::vector<int> vec;  // 分配内存
vec.push_back(10);     // 自动分配内存
// 当 vec 离开作用域时，内存会自动释放
```

### 5. **内存回收策略**

C++ 的内存管理模型并不具备自动垃圾回收机制，程序员需要显式地释放不再使用的内存。以下是一些常见的内存回收策略：

#### 5.1 **避免内存泄漏**

确保每个 `new` 或 `malloc` 的内存分配都有相应的 `delete` 或 `free` 操作。可以使用工具（如 `valgrind`）检查内存泄漏。

#### 5.2 **RAII（Resource Acquisition Is Initialization）**

RAII 是一种常见的 C++ 编程惯例，通过将资源（如内存、文件句柄）与对象的生命周期绑定来管理资源。在对象销毁时，资源会自动释放。`std::unique_ptr` 和 `std::shared_ptr` 就是实现 RAII 的例子。

#### 5.3 **避免悬挂指针**

确保不会访问已经释放的内存。悬挂指针是指指向已释放内存的指针，访问这些指针会导致程序崩溃。

```cpp
int* p = new int(10);
delete p;
p = nullptr;  // 设置为 nullptr，避免悬挂指针
```

#### 5.4 **智能指针管理**

尽量使用智能指针（如 `std::unique_ptr` 和 `std::shared_ptr`）来管理动态分配的内存。智能指针会自动管理内存释放，避免忘记调用 `delete`。

#### 5.5 **避免多次释放同一内存（双重释放）**

确保每块内存只被释放一次。例如，使用 `std::unique_ptr` 来避免双重释放。

```cpp
std::unique_ptr<int> p1 = std::make_unique<int>(10);
// p1 自动释放内存，无法多次释放
```

### 6. **内存池和自定义内存管理**

在某些情况下，特别是高性能场景下，使用内存池（memory pool）可以减少频繁的堆分配和回收。内存池通过预先分配大块内存，动态分配较小的内存块，从而提高性能。

### 7. **`std::weak_ptr` 和引用计数**

`std::weak_ptr` 是一种不参与引用计数的智能指针，常用于解决循环引用问题。当你不需要增加引用计数时，可以使用 `std::weak_ptr`，它不会阻止对象的销毁。

```cpp
std::shared_ptr<int> shared = std::make_shared<int>(10);
std::weak_ptr<int> weak = shared;  // 不增加引用计数
```

### 总结

在 C++ 中，内存回收的主要方式有：

1. **栈内存**：由编译器自动管理，超出作用域时自动释放。
2. **堆内存**：通过 `delete`、`delete[]`、`free` 等手动释放；或使用智能指针（如 `std::unique_ptr`、`std::shared_ptr`）来自动管理内存。
3. **STL 容器**：自动管理内存，例如 `std::vector`、`std::string` 等。
4. **避免内存泄漏**：确保动态分配的内存在不再使用时及时释放。

通过合理使用智能指针和 RAII 习惯，C++ 程序员可以高效、可靠地进行内存管理，避免内存泄漏和其他常见的内存错误。

## RAII

RAII（Resource Acquisition Is Initialization，资源获取即初始化）是 C++ 中一种广泛使用的编程习惯，它通过将资源（如内存、文件句柄、网络连接等）的管理与对象的生命周期绑定来确保资源的自动释放。RAII 习惯能够有效地防止资源泄漏，并简化资源管理。

### RAII 的核心理念

RAII 的核心思想是：**资源的分配与对象的构造绑定在一起，资源的释放与对象的析构绑定在一起**。换句话说，资源的生命周期由对象的生命周期管理，当对象生命周期结束时，资源会自动释放。

### RAII 的关键特点

1. **资源分配与初始化**：在对象的构造函数中分配资源（如内存分配、文件打开、数据库连接等）。
2. **资源释放与析构**：在对象的析构函数中释放资源。当对象超出作用域或被销毁时，析构函数会自动调用，释放相应的资源。

### 典型的 RAII 实现

1. **内存管理（`std::unique_ptr` 和 `std::shared_ptr`）**

   - `std::unique_ptr` 和 `std::shared_ptr` 是智能指针，用于管理动态分配的内存，它们会自动释放内存，从而避免内存泄漏。

   ```cpp
   // 使用 std::unique_ptr 管理动态内存
   std::unique_ptr<int> ptr = std::make_unique<int>(42);  // 内存分配
   // 不需要显式调用 delete，析构时内存自动释放
   ```

2. **文件操作**

   - 当一个类打开一个文件时，可以在该类的构造函数中打开文件，在析构函数中关闭文件。

   ```cpp
   class FileGuard {
   public:
       FileGuard(const std::string& filename) {
           file = fopen(filename.c_str(), "r");
           if (!file) {
               throw std::runtime_error("Failed to open file");
           }
       }

       ~FileGuard() {
           if (file) {
               fclose(file);
           }
       }

   private:
       FILE* file = nullptr;
   };

   void readFile() {
       FileGuard guard("file.txt");
       // 在 guard 生命周期内，文件已打开
       // 当 guard 离开作用域时，文件会自动关闭
   }
   ```

3. **锁管理（`std::lock_guard` 和 `std::unique_lock`）**

   - `std::lock_guard` 是 RAII 风格的锁管理类，它在构造时自动加锁，在析构时自动解锁。它用于防止死锁和保证线程安全。

   ```cpp
   std::mutex mtx;

   void threadSafeFunction() {
       std::lock_guard<std::mutex> lock(mtx);  // 自动加锁
       // 临界区代码
       // 当 lock 离开作用域时，自动解锁
   }
   ```

4. **数据库连接**

   - 当一个类用于管理数据库连接时，可以在构造函数中打开数据库连接，在析构函数中关闭连接。

   ```cpp
   class DBConnection {
   public:
       DBConnection(const std::string& dbName) {
           conn = connectToDatabase(dbName);
       }

       ~DBConnection() {
           if (conn) {
               closeDatabaseConnection(conn);
           }
       }

   private:
       DatabaseConnection* conn;
   };

   void useDatabase() {
       DBConnection db("my_database");
       // 在 db 对象的生命周期内，数据库连接有效
       // 当 db 离开作用域时，数据库连接自动关闭
   }
   ```

### RAII 的好处

1. **自动释放资源**：RAII 确保了资源（如内存、文件句柄、网络连接等）在对象超出作用域时自动释放，避免了手动释放资源时可能产生的错误。
2. **简化代码**：不再需要显式的 `delete`、`free` 或资源释放代码，减少了代码冗余和错误的机会。
3. **异常安全**：RAII 可以确保即使在函数中发生异常时，资源也会被正确地释放。这是因为智能指针、锁等 RAII 类型会在离开作用域时自动调用析构函数，从而释放资源。
4. **更易于维护和理解**：RAII 让资源管理与对象生命周期紧密绑定，使得代码结构更加清晰，容易理解和维护。

### RAII 与异常处理

RAII 最重要的特性之一是它与异常处理的结合。因为 C++ 中异常机制的存在可能会导致控制流的跳转，这时 RAII 机制会确保资源即使在异常发生时也能够被正确释放。

例如，在下例中，`std::lock_guard` 会确保即使发生异常，也能自动解锁。

```cpp
std::mutex mtx;

void safeFunction() {
    std::lock_guard<std::mutex> lock(mtx);  // 加锁
    // 可能会抛出异常
    throw std::runtime_error("Something went wrong");
    // 锁将在函数结束时自动释放
}
```

### RAII 应用场景总结

RAII 适用于任何需要手动管理资源的场景，特别是在以下情况下：

- **内存管理**：`std::unique_ptr`、`std::shared_ptr` 等智能指针。
- **文件处理**：自动打开和关闭文件。
- **锁管理**：通过 `std::lock_guard`、`std::unique_lock` 等自动管理互斥锁。
- **数据库连接**：管理数据库连接的生命周期。
- **网络连接**：管理网络连接的生命周期。

### 总结

RAII 是 C++ 中最重要的编程习惯之一，它通过将资源管理与对象生命周期绑定来确保资源的自动释放，减少了内存泄漏、双重释放和其他资源管理错误的风险。RAII 能够使 C++ 编程更加安全、可靠和易于维护，是 C++ 编程中不可或缺的技巧。

---

# 并发

## **oneTBB**

** (oneAPI Threading Building Blocks)** 提供了一系列高效的并行化工具和模式，适合构建多线程的高性能应用程序。以下是 oneTBB 的常用用法与示例：

---

### **1. 并行循环：`parallel_for`**

**用途：** 用于并行化普通的循环操作。

#### **代码示例**

```cpp
#include <tbb/parallel_for.h>
#include <vector>
#include <iostream>

int main() {
    std::vector<int> data(100);

    // 使用 parallel_for 并行填充数组
    tbb::parallel_for(size_t(0), data.size(), [&data](size_t i) {
        data[i] = i * i;
    });

    // 输出结果
    for (const auto& val : data) {
        std::cout << val << " ";
    }

    return 0;
}
```

#### **特点**

- 自动分割任务以充分利用 CPU 核心。
- 支持循环范围自定义，如 `tbb::blocked_range`。

---

### **2. 并行归约：`parallel_reduce`**

**用途：** 适合执行大规模的累加、归约或聚合操作。

#### **代码示例**

```cpp
#include <tbb/parallel_reduce.h>
#include <vector>
#include <iostream>

int main() {
    std::vector<int> data(100, 1);  // 数据初始化为 1

    // 并行归约求和
    int sum = tbb::parallel_reduce(
        tbb::blocked_range<size_t>(0, data.size()),
        0,
        [&data](const tbb::blocked_range<size_t>& range, int local_sum) {
            for (size_t i = range.begin(); i < range.end(); ++i) {
                local_sum += data[i];
            }
            return local_sum;
        },
        std::plus<int>() // 合并局部和
    );

    std::cout << "Sum: " << sum << std::endl;
    return 0;
}
```

#### **特点**

- 可并行计算局部值并合并结果。
- 合并操作由用户指定（如 `std::plus`）。

---

### **3. 并行扫描：`parallel_scan`**

**用途：** 用于并行前缀和（扫描）计算，例如计算累计和。

#### **代码示例**

```cpp
#include <tbb/parallel_scan.h>
#include <vector>
#include <iostream>

int main() {
    std::vector<int> data(10, 1);  // 数据初始化
    std::vector<int> result(data.size());

    tbb::parallel_scan(
        tbb::blocked_range<size_t>(0, data.size()),
        0,
        [&data, &result](const tbb::blocked_range<size_t>& range, int sum, bool is_final_scan) {
            for (size_t i = range.begin(); i < range.end(); ++i) {
                sum += data[i];
                if (is_final_scan) {
                    result[i] = sum;
                }
            }
            return sum;
        },
        std::plus<int>()
    );

    // 输出结果
    for (const auto& val : result) {
        std::cout << val << " ";
    }

    return 0;
}
```

#### **特点**

- 支持双遍扫描（第一遍计算总和，第二遍更新结果）。
- 适用于累计和等需要前后依赖的场景。

---

### **4. 并行任务组：`task_group`**

**用途：** 允许动态生成并管理多个子任务。

#### **代码示例**

```cpp
#include <tbb/task_group.h>
#include <iostream>

int main() {
    tbb::task_group group;

    group.run([] {  // 创建第一个任务
        std::cout << "Task 1 executed\n";
    });

    group.run([] {  // 创建第二个任务
        std::cout << "Task 2 executed\n";
    });

    group.wait();  // 等待所有任务完成
    std::cout << "All tasks completed\n";

    return 0;
}
```

#### **特点**

- 动态创建任务。
- `wait` 方法阻塞当前线程，直到所有任务完成。

---

### **5. 并行流水线：`parallel_pipeline`**

**用途：** 用于实现数据流中的多阶段处理。

#### **代码示例**

```cpp
#include <tbb/pipeline.h>
#include <iostream>
#include <vector>

int main() {
    const size_t n = 100;
    std::vector<int> data(n);

    // 定义流水线
    tbb::parallel_pipeline(
        4, // 并行度
        tbb::make_filter<void, int>(
            tbb::filter_mode::serial_in_order,
            [&data, i = 0]() mutable -> int {
                if (i < data.size()) return data[i++] = i;
                return tbb::filter::end_of_stream;
            }
        ) &
        tbb::make_filter<int, void>(
            tbb::filter_mode::parallel,
            [](int value) {
                std::cout << "Processed: " << value << std::endl;
            }
        )
    );

    return 0;
}
```

#### **特点**

- 可定义多阶段流水线，每个阶段可以是串行或并行。
- 适合流式数据处理。

---

### **6. 并行排序：`parallel_sort`**

**用途：** 高效并行化排序。

#### **代码示例**

```cpp
#include <tbb/parallel_sort.h>
#include <vector>
#include <iostream>

int main() {
    std::vector<int> data = {5, 3, 8, 6, 2, 7, 4, 1};

    // 并行排序
    tbb::parallel_sort(data.begin(), data.end());

    // 输出排序结果
    for (const auto& val : data) {
        std::cout << val << " ";
    }

    return 0;
}
```

#### **特点**

- 使用并行分治法对数据排序。
- 性能优于单线程排序。

---

### **7. 使用自定义任务：`tbb::task`**

**用途：** 提供更精细的任务控制。

#### **代码示例**

```cpp
#include <tbb/task.h>
#include <iostream>

class CustomTask : public tbb::task {
public:
    tbb::task* execute() override {
        std::cout << "Executing custom task\n";
        return nullptr;
    }
};

int main() {
    tbb::task_scheduler_init init;

    tbb::task& task = *new(tbb::task::allocate_root()) CustomTask();
    tbb::task::spawn_root_and_wait(task);

    return 0;
}
```

---

### **8. 高级同步工具**

- **`concurrent_vector`**：线程安全的动态数组。
- **`concurrent_queue`**：线程安全的队列。
- **`concurrent_hash_map`**：线程安全的哈希表。

#### **代码示例：`concurrent_queue`**

```cpp
#include <tbb/concurrent_queue.h>
#include <thread>
#include <iostream>

int main() {
    tbb::concurrent_queue<int> queue;

    // 生产者线程
    std::thread producer([&] {
        for (int i = 0; i < 10; ++i) {
            queue.push(i);
        }
    });

    // 消费者线程
    std::thread consumer([&] {
        int value;
        while (queue.try_pop(value)) {
            std::cout << "Consumed: " << value << std::endl;
        }
    });

    producer.join();
    consumer.join();

    return 0;
}
```

---

### **总结**

- **`parallel_for`**：并行循环。
- **`parallel_reduce`**：并行归约。
- **`task_group`**：任务分组。
- **`parallel_pipeline`**：数据流水线。
- **`concurrent_\*` 容器**：线程安全数据结构。

如果有具体场景需求，可以针对性优化实现！

# HTTP

以下是如何使用 `cpr` 库的详细指南，包括安装、基本用法和进阶功能示例。

---

## **安装**

### **1. 使用 vcpkg**

```bash
vcpkg install cpr
```

使用 `vcpkg` 安装后，确保项目正确集成了 `vcpkg` 的工具链。

### **2. 使用 CMake**

在项目的 `CMakeLists.txt` 中添加以下内容：

```cmake
include(FetchContent)
FetchContent_Declare(
    cpr
    GIT_REPOSITORY https://github.com/libcpr/cpr.git
    GIT_TAG        1.10.1  # 替换为需要的版本号
)
FetchContent_MakeAvailable(cpr)

target_link_libraries(your_target PRIVATE cpr::cpr)
```

---

## **基本用法**

### **1. 发送 GET 请求**

```cpp
#include <cpr/cpr.h>
#include <iostream>

int main() {
    auto response = cpr::Get(cpr::Url{"https://httpbin.org/get"});
    std::cout << "Status code: " << response.status_code << "\n";
    std::cout << "Response text: " << response.text << "\n";
    return 0;
}
```

---

### **2. 发送 POST 请求**

```cpp
#include <cpr/cpr.h>
#include <iostream>

int main() {
    auto response = cpr::Post(
        cpr::Url{"https://httpbin.org/post"},
        cpr::Payload{{"key1", "value1"}, {"key2", "value2"}}
    );
    std::cout << "Status code: " << response.status_code << "\n";
    std::cout << "Response text: " << response.text << "\n";
    return 0;
}
```

---

### **3. 添加请求头**

```cpp
#include <cpr/cpr.h>
#include <iostream>

int main() {
    auto response = cpr::Get(
        cpr::Url{"https://httpbin.org/headers"},
        cpr::Header{{"User-Agent", "cpr-example"}, {"Accept", "application/json"}}
    );
    std::cout << "Response text: " << response.text << "\n";
    return 0;
}
```

---

### **4. 发送 URL 参数**

```cpp
#include <cpr/cpr.h>
#include <iostream>

int main() {
    auto response = cpr::Get(
        cpr::Url{"https://httpbin.org/get"},
        cpr::Parameters{{"param1", "value1"}, {"param2", "value2"}}
    );
    std::cout << "Response text: " << response.text << "\n";
    return 0;
}
```

---

### **5. 设置超时**

```cpp
#include <cpr/cpr.h>
#include <iostream>

int main() {
    auto response = cpr::Get(
        cpr::Url{"https://httpbin.org/delay/5"},
        cpr::Timeout{2000}  // 超时设置为 2 秒
    );
    std::cout << "Status code: " << response.status_code << "\n";
    std::cout << "Response text: " << response.text << "\n";
    return 0;
}
```

---

### **6. 上传文件**

```cpp
#include <cpr/cpr.h>
#include <iostream>

int main() {
    auto response = cpr::Post(
        cpr::Url{"https://httpbin.org/post"},
        cpr::Multipart{
            {"file", cpr::File{"example.txt"}}
        }
    );
    std::cout << "Status code: " << response.status_code << "\n";
    std::cout << "Response text: " << response.text << "\n";
    return 0;
}
```

---

### **7. 下载文件**

虽然 `cpr` 不直接支持文件下载，但可以用以下方法实现：

```cpp
#include <cpr/cpr.h>
#include <fstream>
#include <iostream>

int main() {
    auto response = cpr::Get(cpr::Url{"https://example.com/file.txt"});
    if (response.status_code == 200) {
        std::ofstream outFile("downloaded_file.txt");
        outFile << response.text;
        outFile.close();
        std::cout << "File downloaded successfully.\n";
    } else {
        std::cout << "Failed to download file. Status code: " << response.status_code << "\n";
    }
    return 0;
}
```

---

### **8. 设置代理**

```cpp
#include <cpr/cpr.h>
#include <iostream>

int main() {
    auto response = cpr::Get(
        cpr::Url{"https://httpbin.org/get"},
        cpr::Proxies{{"http", "http://proxyserver:port"}}
    );
    std::cout << "Response text: " << response.text << "\n";
    return 0;
}
```

---

### **9. 处理 JSON 数据**

配合 `nlohmann/json` 库，可以解析返回的 JSON 数据：

```cpp
#include <cpr/cpr.h>
#include <nlohmann/json.hpp>
#include <iostream>

int main() {
    auto response = cpr::Get(cpr::Url{"https://httpbin.org/json"});
    if (response.status_code == 200) {
        auto json_data = nlohmann::json::parse(response.text);
        std::cout << "JSON Response: " << json_data.dump(4) << "\n";
    } else {
        std::cout << "Request failed with status: " << response.status_code << "\n";
    }
    return 0;
}
```

---

## **常见选项**

| **选项**              | **描述**                     |
| --------------------- | ---------------------------- |
| `cpr::Url`            | 设置请求的 URL               |
| `cpr::Header`         | 添加请求头                   |
| `cpr::Parameters`     | 添加 URL 参数                |
| `cpr::Payload`        | 添加 POST 数据（键值对格式） |
| `cpr::Timeout`        | 设置超时时间（单位：毫秒）   |
| `cpr::Proxies`        | 设置代理                     |
| `cpr::Multipart`      | 发送多部分数据（如文件上传） |
| `cpr::Authentication` | 添加基本认证（用户名和密码） |

---

# redis

`redis-plus-plus` 是一个 C++ Redis 客户端库，它提供了简洁的接口来与 Redis 数据库进行交互。这个库基于 `hiredis` 库（一个 C 语言实现的 Redis 客户端）构建，并提供了对 Redis 命令的 C++ 封装，支持同步、异步操作以及一些 Redis 数据结构的操作。

### 1. 安装和依赖

首先，确保你已经安装了 `hiredis` 库，`redis-plus-plus` 是基于 `hiredis` 的。

#### 安装 `redis-plus-plus`：

如果你使用 CMake，可以按照以下步骤：

```bash
# 安装 hiredis 库
git clone https://github.com/redis/hiredis.git
cd hiredis
make
sudo make install

# 安装 redis-plus-plus
git clone https://github.com/sewenew/redis-plus-plus.git
cd redis-plus-plus
mkdir build
cd build
cmake ..
make
sudo make install
```

确保你的 CMake 配置文件能够正确地找到 `redis-plus-plus` 和 `hiredis` 库。

### 2. 基本用法

`redis-plus-plus` 提供了非常直观的 API 以便你能够通过 C++ 与 Redis 进行交互。

#### 2.1 引入头文件

你需要在代码中引入 `redis-plus-plus` 的头文件：

```cpp
#include <sw/redis++/redis++.h>
```

#### 2.2 创建连接

你可以通过 `sw::redis::Redis` 类来建立与 Redis 服务的连接。`redis++` 支持多种连接方式，包括单机、集群等。

**连接 Redis 示例：**

```cpp
#include <sw/redis++/redis++.h>
#include <iostream>

int main() {
    try {
        // 创建一个同步 Redis 对象，连接到默认的 localhost:6379
        sw::redis::Redis redis("tcp://127.0.0.1:6379");

        // 设置 Redis 键值
        redis.set("key", "value");

        // 获取 Redis 键值
        auto val = redis.get("key");
        if (val) {
            std::cout << "The value of 'key' is: " << *val << std::endl;
        }

    } catch (const sw::redis::RedisException& e) {
        std::cerr << "Redis exception: " << e.what() << std::endl;
    }

    return 0;
}
```

在这个简单的示例中，我们：

1. 连接到本地 Redis 服务。
2. 设置并获取一个键值对。

### 3. 支持的操作

`redis-plus-plus` 提供了大量的 Redis 命令支持，基本上覆盖了 Redis 的所有命令。以下是一些常用的操作：

#### 3.1 设置和获取键值

```cpp
// 设置键值
redis.set("name", "redis-plus-plus");

// 获取键值
auto name = redis.get("name");
if (name) {
    std::cout << "Name: " << *name << std::endl;
} else {
    std::cout << "Name not found" << std::endl;
}
```

#### 3.2 设置过期时间

你可以设置键的过期时间（单位：秒）。

```cpp
redis.setex("session", 30, "user_123");  // 键 "session" 的值将在 30 秒后过期
```

#### 3.3 删除键

```cpp
redis.del("session");  // 删除键 "session"
```

#### 3.4 列表操作

`redis-plus-plus` 支持 Redis 的列表操作：

```cpp
// 向列表的右端添加元素
redis.rpush("mylist", "one");
redis.rpush("mylist", "two");

// 从列表左端取出元素
auto value = redis.lpop("mylist");
if (value) {
    std::cout << "Popped from list: " << *value << std::endl;
}
```

#### 3.5 哈希操作

```cpp
// 设置哈希
redis.hset("user:1000", "name", "Alice");
redis.hset("user:1000", "age", "30");

// 获取哈希值
auto name = redis.hget("user:1000", "name");
if (name) {
    std::cout << "User name: " << *name << std::endl;
}
```

### 4. 异步操作

`redis-plus-plus` 还支持异步操作，可以通过 `sw::redis::Redis` 提供的异步 API 来提高并发性能。

**异步示例：**

```cpp
#include <sw/redis++/redis++.h>
#include <iostream>
#include <future>

int main() {
    try {
        sw::redis::Redis redis("tcp://127.0.0.1:6379");

        // 异步设置键值
        std::future<> f1 = redis.set("key1", "value1");

        // 异步获取键值
        std::future<std::optional<std::string>> f2 = redis.get("key1");

        // 等待异步任务完成
        f1.get();

        auto value = f2.get();
        if (value) {
            std::cout << "Async value: " << *value << std::endl;
        }

    } catch (const sw::redis::RedisException& e) {
        std::cerr << "Redis exception: " << e.what() << std::endl;
    }

    return 0;
}
```

### 5. 发布与订阅

`redis-plus-plus` 还支持 Redis 的发布与订阅（Pub/Sub）功能：

```cpp
#include <sw/redis++/redis++.h>
#include <iostream>
#include <thread>

void subscriber() {
    try {
        sw::redis::Redis redis("tcp://127.0.0.1:6379");

        redis.subscribe({"my_channel"}, [](const std::string& channel, const std::string& message) {
            std::cout << "Received message: " << message << " on channel: " << channel << std::endl;
        });
    } catch (const sw::redis::RedisException& e) {
        std::cerr << "Redis exception: " << e.what() << std::endl;
    }
}

void publisher() {
    try {
        sw::redis::Redis redis("tcp://127.0.0.1:6379");

        std::this_thread::sleep_for(std::chrono::seconds(1));  // 等待订阅者准备好

        redis.publish("my_channel", "Hello, Redis!");
    } catch (const sw::redis::RedisException& e) {
        std::cerr << "Redis exception: " << e.what() << std::endl;
    }
}

int main() {
    std::thread sub_thread(subscriber);
    std::thread pub_thread(publisher);

    sub_thread.join();
    pub_thread.join();

    return 0;
}
```

### 6. 线程安全

`redis-plus-plus` 在内部使用线程池，并确保客户端和 Redis 之间的连接是线程安全的。你可以在多线程环境中使用它而无需担心竞争问题。

### 7. 总结

`redis-plus-plus` 是一个功能强大的 C++ Redis 客户端，提供了与 Redis 数据库交互的同步和异步接口。它支持 Redis 的所有常用命令，具有非常友好的 API，能够方便地执行 Redis 操作，并能够在 C++ 中轻松实现高效的 Redis 操作。

- 支持多种 Redis 数据结构（字符串、列表、哈希、集合等）。
- 支持同步和异步操作。
- 支持发布/订阅。
- 支持连接池、线程安全等高级特性。

对于需要与 Redis 进行高效、可靠交互的 C++ 项目，`redis-plus-plus` 是一个非常适合的选择。

# opencv

OpenCV（Open Source Computer Vision Library）是一个开源计算机视觉库，广泛用于图像处理和计算机视觉领域。C++ 是 OpenCV 的主要开发语言，因此大多数库的核心功能和性能优化都针对 C++ 进行了优化。以下是如何在 C++ 中使用 OpenCV 库的一些基本步骤和示例。

### 1. 安装 OpenCV

在 Ubuntu 系统上可以通过以下命令安装 OpenCV：

```bash
sudo apt update
sudo apt install libopencv-dev
```

在 Windows 上，可以从 [OpenCV 官网](https://opencv.org/releases/) 下载预编译的库，然后进行配置。

### 2. 配置 C++ 工程以使用 OpenCV

在 Linux 系统上使用 g++ 编译时，需要链接 OpenCV 的库，例如：

```bash
g++ your_program.cpp -o your_program `pkg-config --cflags --libs opencv4`
```

如果使用 CMake，则可以在 `CMakeLists.txt` 文件中添加以下内容：

```cmake
cmake_minimum_required(VERSION 3.10)
project(MyOpenCVProject)

find_package(OpenCV REQUIRED)

add_executable(my_program your_program.cpp)
target_link_libraries(my_program ${OpenCV_LIBS})
```

### 3. OpenCV 基本示例

#### 读取和显示图像

```cpp
#include <opencv2/opencv.hpp>
#include <iostream>

int main() {
    // 读取图像
    cv::Mat image = cv::imread("image.jpg");
    if (image.empty()) {
        std::cerr << "无法加载图像" << std::endl;
        return -1;
    }

    // 显示图像
    cv::imshow("Display Image", image);
    cv::waitKey(0);  // 等待按键按下
    return 0;
}
```

#### 图像灰度化和保存

```cpp
#include <opencv2/opencv.hpp>

int main() {
    cv::Mat colorImage = cv::imread("image.jpg");
    if (colorImage.empty()) {
        std::cerr << "无法加载图像" << std::endl;
        return -1;
    }

    // 转换为灰度图像
    cv::Mat grayImage;
    cv::cvtColor(colorImage, grayImage, cv::COLOR_BGR2GRAY);

    // 显示灰度图像
    cv::imshow("Gray Image", grayImage);

    // 保存灰度图像
    cv::imwrite("gray_image.jpg", grayImage);

    cv::waitKey(0);
    return 0;
}
```

#### 图像缩放与旋转

```cpp
#include <opencv2/opencv.hpp>

int main() {
    cv::Mat image = cv::imread("image.jpg");
    if (image.empty()) {
        std::cerr << "无法加载图像" << std::endl;
        return -1;
    }

    // 缩放图像
    cv::Mat resizedImage;
    cv::resize(image, resizedImage, cv::Size(), 0.5, 0.5);  // 缩小 50%

    // 旋转图像
    cv::Mat rotatedImage;
    cv::Point2f center(resizedImage.cols / 2.0, resizedImage.rows / 2.0);
    cv::Mat rotMatrix = cv::getRotationMatrix2D(center, 45, 1.0);  // 旋转 45 度
    cv::warpAffine(resizedImage, rotatedImage, rotMatrix, resizedImage.size());

    // 显示结果
    cv::imshow("Resized Image", resizedImage);
    cv::imshow("Rotated Image", rotatedImage);

    cv::waitKey(0);
    return 0;
}
```

#### 边缘检测

```cpp
#include <opencv2/opencv.hpp>

int main() {
    cv::Mat image = cv::imread("image.jpg", cv::IMREAD_GRAYSCALE);
    if (image.empty()) {
        std::cerr << "无法加载图像" << std::endl;
        return -1;
    }

    // 使用 Canny 算法进行边缘检测
    cv::Mat edges;
    cv::Canny(image, edges, 100, 200);

    // 显示结果
    cv::imshow("Edges", edges);

    cv::waitKey(0);
    return 0;
}
```

### 4. 常见功能

- **形态学操作**（如膨胀、腐蚀）：`cv::dilate`, `cv::erode`
- **图像滤波**（如模糊）：`cv::blur`, `cv::GaussianBlur`
- **轮廓检测**：`cv::findContours`, `cv::drawContours`
- **人脸检测**：基于 Haar 或 DNN 的人脸检测
- **视频处理**：通过 `cv::VideoCapture` 和 `cv::VideoWriter` 读取和保存视频

OpenCV 提供了非常丰富的图像和视频处理功能，是图像处理、计算机视觉项目中非常强大的工具。

# 包管理器

## vcpkg

source ~/.bashrc

vcpkg install --triplet x64-linux

使用替代的下载源：

- 手动下载 `curl-8_11_0.tar.gz` 并放置在 `vcpkg/downloads/` 目录中。

# 日志

`spdlog` 是一个快速、简单、高效的 C++日志库。它支持异步、同步日志记录，并提供丰富的日志格式化选项。以下是 `spdlog` 的基本使用方法：

### 1. 安装 `spdlog`

如果你已经通过 `vcpkg` 安装了 `spdlog`，可以直接包含它的头文件使用：

```cpp
#include <spdlog/spdlog.h>
```

### 2. 基本用法

`spdlog` 的基本用法非常简单，可以直接使用默认的控制台日志记录器：

```cpp
#include <spdlog/spdlog.h>

int main() {
    spdlog::info("Hello, {}!", "world");
    spdlog::warn("This is a warning!");
    spdlog::error("This is an error message!");

    // 设置日志级别（只记录警告及以上级别的日志）
    spdlog::set_level(spdlog::level::warn);

    return 0;
}
```

### 3. 创建日志记录器

你可以创建不同类型的日志记录器，例如控制台日志记录器和文件日志记录器。

#### 控制台日志记录器

```cpp
auto console = spdlog::stdout_color_mt("console");
console->info("This is a colored console log.");
```

#### 文件日志记录器

```cpp
auto file_logger = spdlog::basic_logger_mt("file_logger", "logs/basic-log.txt");
file_logger->info("This log goes to a file.");
```

### 4. 设置日志格式

`spdlog` 支持自定义日志格式，例如时间戳、日志级别、消息内容等。可以通过以下代码设置格式：

```cpp
spdlog::set_pattern("[%Y-%m-%d %H:%M:%S] [%^%l%$] %v");
```

在格式中，`%^` 和 `%$` 分别表示开始和结束颜色。常用的格式代码包括：

- `%Y-%m-%d %H:%M:%S`：日期和时间
- `%l`：日志级别
- `%v`：日志消息内容

### 5. 异步日志

`spdlog` 支持异步日志，可以通过以下代码启用：

```cpp
#include <spdlog/async.h>
#include <spdlog/sinks/basic_file_sink.h>

auto async_file_logger = spdlog::basic_logger_mt<spdlog::async_factory>("async_file_logger", "logs/async-log.txt");
async_file_logger->info("This is an async log message.");
```

### 6. 设置日志级别

可以设置全局或特定记录器的日志级别。例如，只输出警告及以上级别的日志：

```cpp
spdlog::set_level(spdlog::level::warn); // 全局设置
file_logger->set_level(spdlog::level::info); // 特定记录器设置
```

### 7. 日志级别

`spdlog` 支持以下日志级别：

- `trace`
- `debug`
- `info`
- `warn`
- `error`
- `critical`
- `off`

### 示例：完整代码

下面是一个完整的示例：

```cpp
#include <spdlog/spdlog.h>
#include <spdlog/sinks/basic_file_sink.h>

int main() {
    // 设置日志格式
    spdlog::set_pattern("[%Y-%m-%d %H:%M:%S] [%^%l%$] %v");

    // 控制台日志
    spdlog::info("Hello, {}!", "world");

    // 文件日志
    auto file_logger = spdlog::basic_logger_mt("file_logger", "logs/example-log.txt");
    file_logger->info("This is a file log example.");

    // 异步文件日志
    auto async_file_logger = spdlog::basic_logger_mt<spdlog::async_factory>("async_file_logger", "logs/async-log.txt");
    async_file_logger->info("This is an async log message.");

    return 0;
}
```

通过这些基本操作，`spdlog` 可以帮助你实现高效的日志记录。

# 错误处理

在 **`try`** 块中，如果引发某个异常，类型与该异常的类型匹配的第一个关联 **`catch`** 块将捕获该异常。 换言之，执行将从 **`throw`** 语句跳转到 **`catch`** 语句。 如果未找到可用的 catch 块，则调用 `std::terminate` 并且程序会退出。 在 C++ 中可以引发任何类型；但是，我们建议引发直接或间接派生自 `std::exception` 的类型。 在前面的示例中，异常类型 [`invalid_argument`](https://learn.microsoft.com/zh-cn/cpp/standard-library/invalid-argument-class?view=msvc-170) 是在标准库的 [``](https://learn.microsoft.com/zh-cn/cpp/standard-library/stdexcept?view=msvc-170) 头文件中定义的。 C++ 既不提供也不需要 **`finally`** 块来确保在引发异常时释放所有资源。 资源采集是使用智能指针的初始化 (RAII) 习语，它提供所需的功能来清理资源。

```C++
#include <iostream>
using namespace std;

double division(int a, int b)
{
    if (b == 0)
    {
        // 抛出异常
        throw "Division by zero condition!";
    }
    return (a / b);
}

int main()
{
    int x = 50;
    int y = 0;
    double z = 0;

    try
    {
        // 保护代码
        z = division(x, y);
        cout << z << endl;
    }
    catch (exception &e) // 捕获异常
    {
        // 处理异常
        cerr << "Standard exception: " << e.what() << endl;
    }
    catch (const char *msg) // 捕获异常 字符串
    {
        // 处理异常
        cerr << msg << endl;
    }
    catch (...) // 捕获所有异常
    {
        // 处理异常
        cerr << "所有异常" << endl;
    }

    return 0;
}
```

# json

```
#include <nlohmann/json.hpp>
using json = nlohmann::json;
```

对于 `ScanRegionStructNew` 结构体，它包含多个 `std::vector<int>` 类型的成员，使用 `nlohmann::json` 进行 JSON 序列化和反序列化时，我们需要为这个结构体提供适当的 `to_json` 和 `from_json` 函数。这将允许 JSON 库将 JSON 数据正确地映射到 `ScanRegionStructNew` 的成员。

1.  使用 `nlohmann::json` 需要为自定义结构体提供适当的 `to_json` 和 `from_json` 方法来实现 JSON 和结构体之间的相互转换。

2.  `get_to` 方法需要目标对象的成员类型与 JSON 数据兼容，因此在定义 `to_json` 和 `from_json` 时，需要确保字段匹配。

3.

4.          //nlohmann::json 库将一个结构体对象（如 ScanMode）转换为 JSON 字符串
           std::string bs = json(scanModel).dump();

### 1. `ScanRegionStructNew` 的 `to_json` 和 `from_json` 函数

```cpp
#include <nlohmann/json.hpp>
#include <vector>

struct ScanRegionStructNew {
    std::vector<int> white_region;
    std::vector<int> red_region;
    std::vector<int> platelet_region;
    std::vector<int> tiwei;
    std::vector<int> bianyuan;
    std::vector<int> thick;
    std::vector<int> thin;
};

// 序列化为 JSON
void to_json(nlohmann::json& j, const ScanRegionStructNew& r) {
    j = nlohmann::json{
        {"white_region", r.white_region},
        {"red_region", r.red_region},
        {"platelet_region", r.platelet_region},
        {"tiwei", r.tiwei},
        {"bianyuan", r.bianyuan},
        {"thick", r.thick},
        {"thin", r.thin}
    };
}

// 反序列化 JSON 为结构体
void from_json(const nlohmann::json& j, ScanRegionStructNew& r) {
    j.at("white_region").get_to(r.white_region);
    j.at("red_region").get_to(r.red_region);
    j.at("platelet_region").get_to(r.platelet_region);
    j.at("tiwei").get_to(r.tiwei);
    j.at("bianyuan").get_to(r.bianyuan);
    j.at("thick").get_to(r.thick);
    j.at("thin").get_to(r.thin);
}
```

### 2. 解析 JSON 字符串到 `ScanRegionStructNew`

在代码中，当你需要将一个 JSON 字符串解析为 `ScanRegionStructNew` 对象时，可以使用 `json::parse` 和 `get_to` 方法：

```cpp
#include <nlohmann/json.hpp>
#include <iostream>
#include <string>

int main() {
    std::string scanAreaStr = R"({
        "white_region": [1, 2, 3],
        "red_region": [4, 5, 6],
        "platelet_region": [7, 8, 9],
        "tiwei": [10, 11],
        "bianyuan": [12, 13],
        "thick": [14, 15],
        "thin": [16, 17]
    })";

    ScanRegionStructNew scanRegion;
    try {
    // 解析 JSON 字符串到 json 对象
    json j = json::parse(jsonString);
    // 自动解析到结构体
    Person person = j.get<Person>();  // 这里自动调用了 from_json


        nlohmann::json::parse(scanAreaStr).get_to(scanRegion);
        // 打印结果
        std::cout << "white_region size: " << scanRegion.white_region.size() << std::endl;
        std::cout << "red_region size: " << scanRegion.red_region.size() << std::endl;
    } catch (const std::exception& e) {
        std::cerr << "Error parsing JSON: " << e.what() << std::endl;
    }

    return 0;
}
```

### 3. 解释

- `to_json` 函数：将 `ScanRegionStructNew` 类型的结构体序列化为 JSON 对象。在这个函数中，我们创建了一个包含各个字段的 JSON 对象，并将其赋值给 `j`（即目标 JSON）。
- `from_json` 函数：将 JSON 对象反序列化为 `ScanRegionStructNew` 结构体。在这里，我们从 JSON 对象中提取每个字段并将其赋值给 `ScanRegionStructNew` 结构体的相应成员。

- `get_to` 方法：该方法从 JSON 对象中获取数据并填充到目标对象中。它将 JSON 中的值转换为目标类型，前提是 `from_json` 和 `to_json` 方法已正确定义。

## JSON 对象和文件

在 C++ 中，使用 `nlohmann::json` 库可以轻松地操作 JSON 数据，并且通过文件 I/O 操作将 JSON 对象保存到文件或从文件加载。下面是如何在 C++ 中使用 JSON 对象和文件进行操作的基本示例。

### **1. 将 JSON 对象保存到文件**

将 JSON 对象保存到文件时，你可以使用 `dump()` 方法将 JSON 对象序列化为字符串，然后使用文件输出流（`std::ofstream`）将其写入到文件中。

#### **示例：**

```cpp
#include <iostream>
#include <fstream>
#include <nlohmann/json.hpp>

using json = nlohmann::json;

int main() {
    // 创建一个 JSON 对象
    json labelDic = {
        {"label1", "value1"},
        {"label2", "value2"},
        {"label3", "value3"}
    };

    // 打开文件进行输出
    std::ofstream file("output.json");

    if (file.is_open()) {
        // 将 JSON 对象转换为字符串并写入到文件
        file << labelDic.dump(4);  // 使用 4 个空格进行缩进格式化输出
        file.close();  // 关闭文件
        std::cout << "JSON written to output.json\n";
    } else {
        std::cerr << "Unable to open file\n";
    }

    return 0;
}
```

### **解释：**

1. **创建 JSON 对象：** `json labelDic = { ... };` 语句创建了一个 JSON 对象 `labelDic`，其中包含三个键值对。
2. **打开文件：** `std::ofstream file("output.json");` 打开文件 `output.json` 进行写入。如果文件不存在，它会被创建。
3. **写入 JSON 对象：** `file << labelDic.dump(4);` 将 JSON 对象 `labelDic` 序列化为字符串，并将格式化的 JSON 内容写入文件。`dump(4)` 表示使用 4 个空格进行缩进。
4. **关闭文件：** `file.close();` 用来关闭文件流。

### **2. 从文件读取 JSON 数据**

要从文件中读取 JSON 数据并将其转换回 JSON 对象，你可以使用 `nlohmann::json::parse()` 或者直接从文件流中读取并解析。

#### **示例：**

```cpp
#include <iostream>
#include <fstream>
#include <nlohmann/json.hpp>

using json = nlohmann::json;

int main() {
    // 打开文件进行读取
    std::ifstream file("output.json");

    if (file.is_open()) {
        // 从文件中读取 JSON 数据
        json labelDic;
        file >> labelDic;

        // 输出读取的 JSON 数据
        std::cout << "Read JSON from file:\n" << labelDic.dump(4) << std::endl;
        file.close();  // 关闭文件
    } else {
        std::cerr << "Unable to open file\n";
    }

    return 0;
}
```

### **解释：**

1. **打开文件：** `std::ifstream file("output.json");` 打开 `output.json` 文件进行读取。
2. **读取 JSON 数据：** `file >> labelDic;` 将文件中的 JSON 数据解析到 `labelDic` 对象中。
3. **输出 JSON 内容：** `std::cout << labelDic.dump(4);` 使用 4 个空格缩进的格式输出读取到的 JSON 内容。
4. **关闭文件：** `file.close();` 关闭文件流。

### **3. 错误处理：**

- **文件打开失败：** 在文件操作中，如果文件没有成功打开，可以使用 `file.is_open()` 来检查。
- **JSON 格式错误：** 在解析过程中，`nlohmann::json` 会抛出异常，如果 JSON 格式不正确，可以使用 `try-catch` 块来捕获并处理异常。

#### **处理 JSON 格式错误：**

```cpp
#include <iostream>
#include <fstream>
#include <nlohmann/json.hpp>

using json = nlohmann::json;

int main() {
    // 打开文件进行读取
    std::ifstream file("output.json");

    if (file.is_open()) {
        try {
            // 尝试从文件中读取并解析 JSON 数据
            json labelDic;
            file >> labelDic;

            // 输出读取的 JSON 数据
            std::cout << "Read JSON from file:\n" << labelDic.dump(4) << std::endl;
        } catch (const nlohmann::json::parse_error& e) {
            std::cerr << "JSON parse error: " << e.what() << std::endl;
        }
        file.close();  // 关闭文件
    } else {
        std::cerr << "Unable to open file\n";
    }

    return 0;
}
```

### **4. 总结：**

- 使用 `dump()` 方法将 JSON 对象序列化为字符串，并通过文件输出流 (`std::ofstream`) 将其写入文件。
- 使用文件输入流 (`std::ifstream`) 读取 JSON 数据，并通过 `nlohmann::json::parse()` 或 `file >> jsonObj` 将其解析为 JSON 对象。
- 处理文件操作时，确保检查文件是否成功打开并且处理可能的解析错误。

这些是 C++ 中与 JSON 对象和文件操作的基本用法。通过这些操作，你可以轻松地将 JSON 数据保存到文件或者从文件读取数据。

# pqsql

```c++
#include <pqxx/pqxx>
using namespace pqxx;
```

# 时间

在 C++ 中处理时间有多种方式，主要包括使用标准库提供的时间相关功能和一些第三方库。这里将对常用的时间处理方式进行总结：

### 1. **C 标准库 `<ctime>`**

这是 C++ 中最传统的时间处理方法，提供了对系统时间和日期的访问。

- **`std::time_t`**：表示自纪元（通常是 1970 年 1 月 1 日 00:00:00 UTC）以来的秒数。
- **`std::tm`**：表示日期和时间的结构体，可以通过 `localtime()` 和 `gmtime()` 转换为本地时间或 UTC 时间。
- **`std::time(nullptr)`**：获取当前的时间戳（自纪元以来的秒数）。
- **`std::difftime`**：计算两个 `std::time_t` 时间戳之间的差值，返回 `double` 类型（秒数差）。
- **`std::strftime`**：格式化时间为字符串。

#### 示例：

```cpp
#include <iostream>
#include <ctime>

int main() {
    std::time_t now = std::time(nullptr);  // 获取当前时间戳
    std::tm* localTime = std::localtime(&now);  // 获取本地时间

    std::cout << "当前时间: " << 1900 + localTime->tm_year << "-"
              << 1 + localTime->tm_mon << "-"
              << localTime->tm_mday << " "
              << localTime->tm_hour << ":"
              << localTime->tm_min << ":"
              << localTime->tm_sec << std::endl;

    return 0;
}
```

### 2. **C++11 `<chrono>` 库**

C++11 引入了 `<chrono>` 库，提供了更精确和灵活的时间测量方式，支持以不同精度（如毫秒、微秒、纳秒等）表示时间。

- **`std::chrono::duration`**：表示时间段，可以使用不同的单位（如秒、毫秒、分钟等）。
- **`std::chrono::time_point`**：表示某一时刻的时间点。
- **`std::chrono::system_clock`**：表示系统时间，可以获取当前时间。
- **`std::chrono::steady_clock`**：提供一个不受系统时间修改影响的时钟，通常用于测量时间间隔。
- **`std::chrono::high_resolution_clock`**：提供最高精度的时钟。

#### 示例：

```cpp
#include <iostream>
#include <chrono>
#include <thread>

int main() {
    auto start = std::chrono::high_resolution_clock::now();  // 获取当前时间点

    std::this_thread::sleep_for(std::chrono::seconds(2));  // 模拟延时

    auto end = std::chrono::high_resolution_clock::now();  // 获取结束时间点
    std::chrono::duration<double> elapsed = end - start;  // 计算时间差

    std::cout << "程序运行时间: " << elapsed.count() << " 秒" << std::endl;
    return 0;
}
```

### 3. **时间比较和差值**

无论是使用 `std::time_t` 还是 `std::chrono`，都可以比较不同时间点的差异。两者之间的比较方式有所不同：

- **使用 `std::time_t`**：
  - `std::difftime` 用于计算时间差，返回类型为 `double`（秒差）。
- **使用 `std::chrono`**：
  - 可以使用 `std::chrono::duration` 和 `std::chrono::time_point` 来直接进行时间差计算，并且支持不同的精度。

#### `std::chrono` 时间差计算示例：

```cpp
auto start = std::chrono::steady_clock::now();
std::this_thread::sleep_for(std::chrono::seconds(5));
auto end = std::chrono::steady_clock::now();
auto duration = std::chrono::duration_cast<std::chrono::seconds>(end - start);
std::cout << "延迟时间: " << duration.count() << " 秒" << std::endl;
```

### 4. **格式化时间**

- **使用 `std::strftime`**：
  可以使用 `std::strftime` 将 `std::tm` 转换为特定格式的字符串。

  #### 示例：

  ```cpp
  std::time_t now = std::time(nullptr);
  char buffer[80];
  std::strftime(buffer, sizeof(buffer), "%Y-%m-%d %H:%M:%S", std::localtime(&now));
  std::cout << "格式化时间: " << buffer << std::endl;
  ```

### 5. **高精度计时**

- **`std::chrono::high_resolution_clock`**：提供了最高精度的计时器，适用于对时间精度有较高要求的场合。
- **`steady_clock`**：适用于测量时间间隔，不会受系统时间更改的影响。

### 6. **常见时间操作**

- 获取当前时间：
  - **`std::time_t`**: `std::time(nullptr)`
  - **`std::chrono`**: `std::chrono::system_clock::now()`
- 时间延迟：
  - **`std::this_thread::sleep_for`** 用于延迟，支持秒、毫秒等时间单位。
- 计算时间差：
  - **`std::time_t`**: 使用 `std::difftime` 计算时间差。
  - **`std::chrono`**: 使用 `std::chrono::duration_cast` 计算时间差，返回指定精度。

### 总结：

- 使用 `std::time_t` 和 `std::tm` 适合处理较为传统的时间操作，能够进行时间戳转换、格式化、简单的时间差计算。
- `std::chrono` 提供了更精确和灵活的时间操作，支持多种时钟和时间单位，适合现代 C++ 程序。
- 在需要高精度计时时，`std::chrono` 提供了比 `std::time_t` 更强大的功能。

# cpp-base64

`cpp-base64` 是一个简单易用的 C++ 库，用于进行 Base64 编码和解码操作。它提供了轻量级的接口，可以方便地将字符串或二进制数据转换为 Base64 格式，或从 Base64 格式还原原始数据。

以下是 `cpp-base64` 的用法介绍：

---

### **1. 安装**

#### 方法一：从源码直接使用

1. 克隆项目：
   ```bash
   git clone https://github.com/ReneNyffenegger/cpp-base64.git
   ```
2. 直接将 `base64.cpp` 和 `base64.h` 文件复制到你的项目中。

#### 方法二：使用包管理器

如果你的项目使用 vcpkg，可以安装：

```bash
vcpkg install base64
```

---

### **2. 基本用法**

#### 引入头文件

```cpp
#include "base64.h"
```

#### 示例代码

```cpp
#include <iostream>
#include <string>
#include "base64.h"

int main() {
    std::string original = "Hello, Base64!";

    // Base64 编码
    std::string encoded = base64_encode(reinterpret_cast<const unsigned char*>(original.c_str()), original.length());
    std::cout << "Encoded: " << encoded << std::endl;

    // Base64 解码
    std::string decoded = base64_decode(encoded);
    std::cout << "Decoded: " << decoded << std::endl;

    return 0;
}
```

---

### **3. 核心函数**

`cpp-base64` 提供以下核心函数：

#### **1. `base64_encode`**

**作用**：将数据进行 Base64 编码。

- **函数原型**：
  ```cpp
  std::string base64_encode(const unsigned char* data, size_t len);
  ```
- **参数**：
  - `data`：待编码的数据（字节数组）。
  - `len`：数据长度。
- **返回值**：编码后的 Base64 字符串。

#### **2. `base64_decode`**

**作用**：将 Base64 字符串解码为原始数据。

- **函数原型**：
  ```cpp
  std::string base64_decode(const std::string& encoded_string);
  ```
- **参数**：
  - `encoded_string`：Base64 格式的字符串。
- **返回值**：解码后的字符串。

---

### **4. 示例输入输出**

#### 输入字符串：

```plaintext
Hello, Base64!
```

#### 编码后的输出：

```plaintext
SGVsbG8sIEJhc2U2NCE=
```

#### 解码后的输出：

```plaintext
Hello, Base64!
```

---

### **5. 注意事项**

1. **数据类型转换**：
   - 对于字符串输入，需要将其转换为字节数组（通常通过 `reinterpret_cast` 实现）。
   - 编码后的 Base64 是字符串格式，无需额外处理。
2. **二进制数据**：
   - Base64 通常用于编码二进制数据（如图像文件、密钥等）。处理二进制文件时，需小心输入和输出的格式。
3. **性能**：
   - `cpp-base64` 是轻量级实现，适合中小规模的编码需求。如果处理大规模数据，需评估其性能。
