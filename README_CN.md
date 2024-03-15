# ZEDX_Camera_Logs

[中文](./README_CN.md) | [English](./README.md)

### 系统信息

- 机器型号：[Vecow EAC5000](https://www.vecow.com/dispPageBox/vecow/VecowCT.aspx?ddsPageID=PRODUCTDTL_EN&dbid=4852986947)
- 固件版本：EAC-5000_64GB_EMMC-BOOT_JetPack5.1.1_230713_ZEDX
- 系统版本：Ubuntu 20.04
- ROS版本：ROS2 Humble
- ZED型号：[ZED X (2.2mm)](https://store.stereolabs.com/products/zed-x-stereo-camera)

### 复现步骤

通过[zed-ros2-wrapper](https://github.com/stereolabs/zed-ros2-wrapper)的[zed_camera.launch.py](https://github.com/stereolabs/zed-ros2-wrapper/blob/master/zed_wrapper/launch/zed_camera.launch.py)指定序列号启动相机。两个ZED X分别接在[EAC5000](https://www.vecow.com/dispPageBox/vecow/VecowCT.aspx?ddsPageID=PRODUCTDTL_EN&dbid=4852986947)的GMSL2的第一和第三口。

以下为具体复现流程：

1. [安装ROS2 Humble](https://nvidia-isaac-ros.github.io/getting_started/isaac_ros_buildfarm_cdn.html#install-ros-2-packages)

2. ROS2 环境设置。将以下写入`~/.bashrc`

    ```
    source /opt/ros/humble/setup.bash
    ```

    然后运行下面这个命令即可生效
    ```
    source ~/.bashrc
    ```

3. 安装必要的软件包
    ```
    sudo apt-get install ros-humble-nmea-msgs* ros-humble-bond* libxsimd-dev ros-humble-ompl* ros-humble-test-msgs* ros-humble-joint-state-publisher* ros-message-runtime libxtensor-dev
    ```

4. 建立工作空间并下载[zed-ros2-wrapper](https://github.com/stereolabs/zed-ros2-wrapper)，[zed-ros2-wrapper](https://github.com/stereolabs/zed-ros2-wrapper)由ZED官方提供，ZED官方支持与ROS2一起运行。

    ```
    mkdir -p ros2_ws/src
    cd ros2_ws/src
    git clone --recursive https://github.com/stereolabs/zed-ros2-wrapper.git
    ```

5. 编译项目

    ```
    cd ../
    colcon build
    ```

6. 配置环境
    ```
    source ./install/local_setup.sh
    ```

7. 查看ZED X序列号

    ```
    $ ZED_Explorer --all
    
    ## Cam  0  ##
     Model :  "ZED-X"
     S/N :  45656860
     State :  "Camera Available"
     Path :  /dev/i2c-30
     ID :  0
     Type :  "GMSL2"
    ********************
    ## Cam  1  ##
     Model :  "ZED-X"
     S/N :  46411394
     State :  "Camera Available"
     Path :  /dev/i2c-31
     ID :  1
     Type :  "GMSL2"
    ********************
    ```

8. 通过序列号启动ZED X（这里需要修改为自己相机的序列号，从步骤5获取）

    ```
    ros2 launch zed_wrapper zed_camera.launch.py camera_name:=zedx/left camera_model:=zedx serial_number:=45656860
    ```

    ```
    ros2 launch zed_wrapper zed_camera.launch.py camera_name:=zedx/right camera_model:=zedx serial_number:=40051193
    ```

9. 打开显示界面（如果你有桌面环境）
    ```
    ros2 run rqt_image_view rqt_image_view
    ```

    分别选择`/zedx/left/zed_node/stereo/image_rect_color`和`/zedx/right/zed_node/stereo/image_rect_color`话题即可看到对应摄像头的画面。

10. 问题复现

    - 问题1：启动程序后随机出现
    - 问题2：成功启动后静置一段时间出现

### 问题

问题1：同时启动两个ZED X，会出现启动失败的现象。以下为log信息。

- 运行启动命令信息: [launch.log](./logs/20240304/launch.log)
- 运行`sudo dmesg`信息: [dmesg.log](./logs/20240304/dmesg.log)
- 运行`sudo ZED_Diagnostic --dmesg`信息: [zed_dmesg.log](./logs/20240304/zed_dmesg.log)



问题2：同时启动两个ZED X，启动成功并过一段时间后，有一个ZED运行不正常。以下为log信息。

- 运行启动命令信息:[launch.log](./logs/20240305/launch.log)
- 运行`sudo dmesg`信息:[dmesg.log](./logs/20240305/dmesg.log)
- 运行`sudo ZED_Diagnostic --dmesg`信息: [zed_dmesg.log](./logs/20240305/zed_dmesg.log)
- 系统报错信息: [nvargus-daemon.crash](./logs/20240305/_usr_sbin_nvargus-daemon.0.crash)

