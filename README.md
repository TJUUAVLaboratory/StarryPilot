StarryPilot
============================

[中文页](中文说明.md) |

## 写在前面
近年来无人机应用市场日趋火热，无人机开始被应用在多个领域之中，比如航拍，植保，运输，安防等。随着应用场景的增加，对于无人机的大脑一飞控，的性能和功能要求也变得越来越高。国内具有一大批优质的无人机企业，如DJI，零度，亿航，极飞等。可是这些企业的飞控系统并不开源，而开源飞控市场却基本被国外所垄断，比如APM, PX4, Autoquad等，国内目前还没有一款开源飞控可以与之抗衡，在国际上也没有令人熟知的“国产”开源飞控。

正是基于开发出一款世界知名的中国的开源飞控，我从2016年开始了StarryPilot这个项目。飞控的设计理念是一款轻量，功能强大的飞控，主要面向科研和无人机行业应用，使得无人机开发技术更加普及，也更容易将无人机技术应用到各个行业。

## 开发环境

主控：STM32F427 + STM32F100(从控制器)
编译环境: Keil MDK5.23
RT-Thread版本: RTT-2.1.0

## 硬件设计

硬件采用国外开源的自驾仪Pixhawk(https://pixhawk.org/modules/pixhawk), 如下图所示。Pixhawk也是目前世界上应用最广，支持的飞控系统最多的开源飞控硬件。

其主要性能参数如下：

- 168MHz / 256 MIPS Cortex M4F
- 14路 PWM/伺服 输出
- 外扩总线接口 (I2C,CAN,UART)
- 冗余电源输入
- 外部安全开关
- 多色LED指示灯
- 外置microSD卡槽

传感器：

ST Micro L3GD20 16位陀螺仪
ST Micro LSM303D 16位加速度计/磁力计
Invensense MPU 6000 三轴加速度计/陀螺仪
MEAS MS5611 气压计

接口：

5x UART, 2x CAN, I2C, SPI
DSM/DSM2/DSM-X 卫星兼容输入
PPWM, S-BUS
3.3 和 6.6V ADC输入
microUSB

## 系统框图

整个系统除了Pixhawk之外，还有一些外接的电子设备，如无刷电机，GPS，电调，数传，RC接收机，Lidar-Lite激光雷达等。
整体的系统框架图如下图所示：

## 软件设计
软件采用分层结构设计，如下图所示，从底层到上层分别是Driver层，RTOS(RTT + Fatfs)，HAL硬件虚拟层，Framework层和应用层。


# About
A lightweight and powerful autopilot software, which focus on research and development of state-of-the-art software for UAVs. One of the project’s primary goals is to provide an open and flexible platform making it easy to be applied to a broad range of domains.

# Feature
- RT-Thread RTOS, Fatfs file system, System components, such as IPC, Msh shell system, file manager, parameter system, log system, etc.
- Completely support with Pixhawk hardware
- ADRC control & PID control
- Support with Mavlink(QGround Control)
- Support with Gazebo hardware-in-the-loop (HITL) simulation

# Control
[![ADRC vs PID](docs/images/adrc_video_demo.png)](https://www.youtube.com/watch?v=77-_nF-qqpA&t=63s)

- [**优酷视频地址**](https://v.youku.com/v_show/id_XMzY2Njg4ODk4NA==.html?spm=a2hzp.8244740.0.0)

# Simulator
- [*Matlab simulator*](https://github.com/JcZou/matlab_quadsim) (Software-in-the-loop)
- [*Gazebo simulator*](https://github.com/JcZou/gazebo_quadsim) (Hardware-in-the-loop)

# Tools
- Calibration software (magnetometer and accelerometer calibration)
- Logger checker

# Usage
The project is developed on Pixhawk (autopilot hardware). To download firmware into Pixhawk, please follow these steps:
- First compile the starry_fmu and generate bin file.
- Use QGroundControl (QGC) to download the bin file into fmu. To download custom firmware, choose the following choice.

![](docs/images/fmu_download.png)

- Now the firmware of starry_fmu should be correctly downloaded (If failed, please try again, or use the newer version of QGC). Connect Radio-telemetry to the **TELEM 2** port of Pixhawk, then you should see the Msh shell system output via serial terminal (default baudrate is **57600**).

![](docs/images/msh.png)

- If you didn't format the SD card before, please type `mkfs` command to format the SD card.
- Then you should download the starry_io firmware. To do so, first compile starry_io project to get the bin file and name it as **starryio.bin**. Copy the starryio.bin to the **root directory** of SD card, then open Msh shell system and type `uploader`, which is shown below.

![](docs/images/io_download.png)

- Choose **file system** option to start download. Notice that if it's the first time that you download starry_io, after you type uploader, you should push reset button of io (in the side of Pixhawk) to let io enter the bootloader.
- Congratulation, now the download is finished!

# Compile Environment
The compile environment is based on Keil MDK5.

![](docs/images/mdk5.png)

# Author
*Jiachi Zou*
---------------------------
*email*: zoujiachi666@163.com 

*LinkedIn*: www.linkedin.com/in/jiachizou-0710

*QQ群*: 459133925
