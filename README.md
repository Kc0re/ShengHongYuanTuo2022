# 第十七届全国大学生智能汽车竞赛-完全模型组

## 概览
这是一个基于 CMake 的嵌入式 / 视觉 + 控制 项目，包含摄像头图像处理、神经网络推理、串口通信与闭环速度控制等模块。项目入口为 [main.cpp](main.cpp)。

## 快速开始

1. 依赖
   - CMake >= 3.10
   - C++11/14 编译器
   - OpenCV
   - 平台串口库

2. 构建
```sh
mkdir -p build
cd build
cmake ..
make -j
```

生成的可执行文件可以在 `build/` 目录中找到，例如仓库中存在的 [build/ShenHongYT_17th](build/ShenHongYT_17th)。

3. 运行
```sh
./build/ShenHongYT_17th
```
（运行参数与配置请参考代码中的配置头与注释）

## 目录结构
- 项目根：  
  - [CMakeLists.txt](CMakeLists.txt) — 构建配置  
  - [main.cpp](main.cpp) — 程序入口  
  - [Six0Ne.hpp](Six0Ne.hpp) — 项目可能使用的通用头

- 头文件（接口与配置）
  - [include/common/model_config.hpp](include/common/model_config.hpp)  
  - [include/core/capture.hpp](include/core/capture.hpp)  
  - [include/core/detection.hpp](include/core/detection.hpp)

- 源代码（主要模块）
  - 控制（PID / 速度决策）
    - [`Pid`](src/Control/Pid.hpp) / [src/Control/Pid.cpp](src/Control/Pid.cpp)  
    - [src/Control/SpeedDecisions.hpp](src/Control/SpeedDecisions.hpp) / [src/Control/SpeedDecisions.cpp](src/Control/SpeedDecisions.cpp)
  - 串口与工具
    - [`Serial`](src/Serial/Serial.hpp) / [src/Serial/Serial.cpp](src/Serial/Serial.cpp)  
    - [src/Serial/stop_watch.hpp](src/Serial/stop_watch.hpp)  
    - [src/Serial/uart.hpp](src/Serial/uart.hpp)
  - 视觉（图像处理 / 深度学习）
    - 灰度图像处理：[`GrayImgproc`](src/Vision/GrayImagproc/GrayImgproc.hpp) / [src/Vision/GrayImagproc/GrayImgproc.cpp](src/Vision/GrayImagproc/GrayImgproc.cpp)  
      相关：[`Camera_Param`](src/Vision/GrayImagproc/Camera_Param.hpp)、[`Camera_Set`](src/Vision/GrayImagproc/Camera_Set.hpp)、[`Utils`](src/Vision/GrayImagproc/Utils.hpp)
    - 彩色图像处理：[`RgbImgProc`](src/Vision/RgbImagesProc/RgbImgProc.hpp) / [src/Vision/RgbImagesProc/RgbImgProc.cpp](src/Vision/RgbImagesProc/RgbImgProc.cpp)  
      相关模块：[`GasStation`](src/Vision/RgbImagesProc/GasStation.hpp)、[`Ramp`](src/Vision/RgbImagesProc/Ramp.hpp)
    - 深度学习推理：[`DeepLearning`](src/Vision/Neural_Networks/DeepLearning.hpp) / [src/Vision/Neural_Networks/DeepLearning.cpp](src/Vision/Neural_Networks/DeepLearning.cpp)  
      模型文件夹：`src/Vision/Neural_Networks/ClassModel/` 和 `src/Vision/Neural_Networks/Smartcar_Model/`
    - 其他视觉组件：[`Circle`](src/Vision/GrayImagproc/Circle.hpp)、[`Cross`](src/Vision/GrayImagproc/Cross.hpp)、[`Garage`](src/Vision/GrayImagproc/Garage.hpp)、[`Yroad`](src/Vision/GrayImagproc/Yroad.hpp)

## 模块说明
- 控制模块（src/Control）
  - `Pid`：实现 PID 控制器，用于闭环控制（速度、方向等）。参见 [`Pid`](src/Control/Pid.hpp)。
  - `SpeedDecisions`：速度决策逻辑，结合传感与视觉结果生成目标速度。

- 串口模块（src/Serial）
  - `Serial`：串口通信封装，用于与底盘或其他设备通信。参见 [`Serial`](src/Serial/Serial.hpp)。

- 视觉模块（src/Vision）
  - 灰度和彩色图像处理：包含预处理、目标检测与几何分析的实现（圆、交叉、车库检测等）。
  - 深度学习：封装模型加载与推理接口（请把模型文件放在相应目录以供加载）。

## 配置与模型
- 模型与参数放置：
  - [src/Vision/Neural_Networks/ClassModel/](src/Vision/Neural_Networks/ClassModel/)  
  - [src/Vision/Neural_Networks/Smartcar_Model/](src/Vision/Neural_Networks/Smartcar_Model/)

- 摄像机参数：
  - [src/Vision/GrayImagproc/Camera_Param.hpp](src/Vision/GrayImagproc/Camera_Param.hpp)

## 调试与开发建议
- 在开发视觉功能时，建议先在带有显示输出的环境运行 [src/Vision/GrayImagproc/Display.cpp](src/Vision/GrayImagproc/Display.cpp) / [src/Vision/RgbImagesProc/RgbImgProc.cpp](src/Vision/RgbImagesProc/RgbImgProc.cpp) 中的可视化路径以验证算法结果。
- 若串口通信异常，检查 [src/Serial/uart.hpp](src/Serial/uart.hpp) 与串口设备权限。

## 参考文件
- [CMakeLists.txt](CMakeLists.txt)  
- [main.cpp](main.cpp)  
- [Six0Ne.hpp](Six0Ne.hpp)  
- [include/common/model_config.hpp](include/common/model_config.hpp)  
- [include/core/capture.hpp](include/core/capture.hpp)  
- [include/core/detection.hpp](include/core/detection.hpp)  
- [src/Control/Pid.hpp](src/Control/Pid.hpp)  
- [src/Control/SpeedDecisions.hpp](src/Control/SpeedDecisions.hpp)  
- [src/Serial/Serial.hpp](src/Serial/Serial.hpp)  
- [src/Vision/GrayImagproc/GrayImgproc.hpp](src/Vision/GrayImagproc/GrayImgproc.hpp)  
- [src/Vision/GrayImagproc/Camera_Param.hpp](src/Vision/GrayImagproc/Camera_Param.hpp)  
- [src/Vision/Neural_Networks/DeepLearning.hpp](src/Vision/Neural_Networks/DeepLearning.hpp)  
- [src/Vision/RgbImagesProc/RgbImgProc.hpp](src/Vision/RgbImagesProc/RgbImgProc.hpp)


