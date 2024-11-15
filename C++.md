# 问题



1. C++开发环境 11.11

2. 包管理器11.12

3. 错误处理

4. 日志--spdlog

5. 基本框架搭建11.13-11.14

   

   

   

   


# 数据库

```
//疟疾scanType分为三种：Thin,Thick,ThickThin
```

1. sql： task表 

2. redis：

   1. 时序小图base64---> json --->videoImg
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

| 名称   | 用途        | 功能 | 端口 |
| ------ | ----------- | ---- | ---- |
| gin    |             |      |      |
| redigo | redis客户端 |      |      |
|        |             |      |      |
|        |             |      |      |

### 数据交互

下位机、算法的交互是通过redis 



### main流程

有**瓦片图和时序图**



#### createtask

#### prescan 

判断扫描的整体区域  rows、cols

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

扫描时是往返扫描 ，下标从1开始

是三角坐标系，xyz

| 1,1  | 1,2  | 1,3  |
| ---- | ---- | ---- |
| 2,3  | 2,2  | 2,1  |
| 3,1  | 3,2  |      |
|      |      |      |



##### 三个

这三个是同时进行的

1. 瓦片地图（分层进行拼接） 初始化时全为空白图像，通过下位机的图片数据进行填充

   长宽减半可能导致小数7.5，改为8

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



| 层级                            | splitnum（瓦片<br />splitNum*splitNum的图片组成） | 总瓦片数                             |      |
| ------------------------------- | ------------------------------------------------- | ------------------------------------ | ---- |
| FirstImageMap: 第一层瓦片地图   | 8                                                 | ⌈ rows/splitNum ⌉*⌈ cols/splitNum  ⌉ |      |
| SecondImageMap: 第二层瓦片地图  | 64                                                |                                      |      |
| ThirdImageMap: 第三层瓦片地图   | 1                                                 |                                      |      |
| SeventhImageMap: 第七层瓦片地图 | 4                                                 |                                      |      |
| EighthImageMap: 第八层瓦片地图  | 2                                                 |                                      |      |
|                                 |                                                   |                                      |      |



```

```

#### 时序图

采取的是本次写入上一次传来的时序图，finish阶段写入最后的时序图



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

整体是一个调用函数，不停接收（while（true））新图像数据（socket接收数据），
原来在redis中获取的数据改为函数调用



1. 传参，判断不同阶段 
   1.  func HandleData(markNum common.MarkNum, splitData []string) error 
   2. 按照原来传给redis的数据，先不做修改
2. 心跳
3. 创建任务
4. 预扫描
5. 预扫描结束：
   1. 初始化各层空白图像，
6. 正式扫描
   1. 接收图片数据，拼接瓦片地图
7. 扫描结束
8. 停止任务
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

# 线程池





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

# 日志

`spdlog` 是一个快速、简单、高效的C++日志库。它支持异步、同步日志记录，并提供丰富的日志格式化选项。以下是 `spdlog` 的基本使用方法：

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

1. 使用 `nlohmann::json` 需要为自定义结构体提供适当的 `to_json` 和 `from_json` 方法来实现 JSON 和结构体之间的相互转换。

2. `get_to` 方法需要目标对象的成员类型与 JSON 数据兼容，因此在定义 `to_json` 和 `from_json` 时，需要确保字段匹配。

3. 

4.        //nlohmann::json 库将一个结构体对象（如 ScanMode）转换为 JSON 字符串
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





# pqsql

```c++
#include <pqxx/pqxx>
using namespace pqxx;
```
