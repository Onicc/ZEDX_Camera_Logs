# ZEDX_Camera_Logs

[中文](./README_CN.md) | [English](./README.md)

### 系统信息

机器型号：[Vecow EAC5000](https://www.vecow.com/dispPageBox/vecow/VecowCT.aspx?ddsPageID=PRODUCTDTL_EN&dbid=4852986947)

固件版本：EAC-5000_64GB_EMMC-BOOT_JetPack5.1.1_230713_ZEDX

系统版本：Ubuntu 20.04

ROS版本：ROS2 Humble

ZED型号：ZED X

### 启动脚本

见[zedx_multi_camera.launch.py](./zedx_multi_camera.launch.py)。该脚本调用[zed-ros2-wrapper](https://github.com/stereolabs/zed-ros2-wrapper)的[zed_camera.launch.py](https://github.com/stereolabs/zed-ros2-wrapper/blob/master/zed_wrapper/launch/zed_camera.launch.py)，通过序列号指定相机。两个ZED X分别接在EAC5000的GMSL2的第一和第三口。

### 问题

问题1：同时启动两个ZED X，会出现启动失败的现象。以下为log信息。

运行启动命令信息: [launch.log](./logs/20240304/launch.log)

运行`sudo dmesg`信息: [dmesg.log](./logs/20240304/dmesg.log)

运行`sudo ZED_Diagnostic --dmesg`信息: [zed_dmesg.log](./logs/20240304/zed_dmesg.log)



问题2：同时启动两个ZED X，启动成功并过一段时间后，有一个ZED运行不正常。以下为log信息。

运行启动命令信息:[launch.log](./logs/20240305/launch.log)

运行`sudo dmesg`信息:[dmesg.log](./logs/20240305/dmesg.log)

运行`sudo ZED_Diagnostic --dmesg`信息: [zed_dmesg.log](./logs/20240305/zed_dmesg.log)

系统报错信息: [nvargus-daemon.crash](./logs/20240305/_usr_sbin_nvargus-daemon.0.crash)

