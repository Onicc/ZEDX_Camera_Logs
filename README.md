# ZEDX_Camera_Logs

[中文](./README_CN.md) | [English](./README.md)

### System Information

- **Machine Model:** [Vecow EAC5000](https://www.vecow.com/dispPageBox/vecow/VecowCT.aspx?ddsPageID=PRODUCTDTL_EN&dbid=4852986947)
- **Firmware Version:** EAC-5000_64GB_EMMC-BOOT_JetPack5.1.1_230713_ZEDX
- **Operating System Version:** Ubuntu 20.04
- **ROS Version:** ROS2 Humble
- **ZED Model:** [ZED X (2.2mm)](https://store.stereolabs.com/products/zed-x-stereo-camera)

### Reproduction Steps

Start the cameras by specifying serial numbers using [zed_camera.launch.py](https://github.com/stereolabs/zed-ros2-wrapper/blob/master/zed_wrapper/launch/zed_camera.launch.py) from [zed-ros2-wrapper](https://github.com/stereolabs/zed-ros2-wrapper). Two ZED X cameras are connected to the first and third ports of GMSL2 on the [EAC5000](https://www.vecow.com/dispPageBox/vecow/VecowCT.aspx?ddsPageID=PRODUCTDTL_EN&dbid=4852986947).

Below are the detailed reproduction steps:

1. [Install ROS2 Humble](https://nvidia-isaac-ros.github.io/getting_started/isaac_ros_buildfarm_cdn.html#install-ros-2-packages)

2. Set up the workspace and clone [zed-ros2-wrapper](https://github.com/stereolabs/zed-ros2-wrapper). [zed-ros2-wrapper](https://github.com/stereolabs/zed-ros2-wrapper) is provided by ZED official and supports running with ROS2.

    ```
    mkdir -p ros2_ws/src
    cd ros2_ws/src
    git clone --recursive https://github.com/stereolabs/zed-ros2-wrapper.git
    ```

3. Build the project

    ```
    cd ../
    colcon build
    ```

4. Set up the environment

    ```
    source ./install/local_setup.sh
    ```

5. View the serial numbers of the ZED X cameras.

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

6. Start ZED X by specifying their serial numbers (modify to match your camera serial numbers obtained from step 5).

    ```
    ros2 launch zed_wrapper zed_camera.launch.py camera_name:=zedx/left camera_model:=zedx zedx_left_serial_number:=45656860
    ```

    ```
    ros2 launch zed_wrapper zed_camera.launch.py camera_name:=zedx/right camera_model:=zedx zedx_left_serial_number:=40051193
    ```

7. Open the display interface (if you have a desktop environment).

    ```
    ros2 run rqt_image_view rqt_image_view
    ```

    Select `/zedx/left/zed_node/stereo/image_rect_color` and `/zedx/right/zed_node/stereo/image_rect_color` topics to view the corresponding camera images.

8. Reproduce the issues:
    
    - Issue 1: Occurs randomly after starting the program.
    - Issue 2: Occurs after the program runs for a period of time.

### Issues

**Issue 1:**

When attempting to start two ZED X cameras simultaneously, the startup process fails. Please find the log details below:
- Log Information for Launch Command: [launch.log](./logs/20240304/launch.log)
- Output of `sudo dmesg`: [dmesg.log](./logs/20240304/dmesg.log)
- Output of `sudo ZED_Diagnostic --dmesg`: [zed_dmesg.log](./logs/20240304/zed_dmesg.log)

**Issue 2:**

While successfully starting two ZED X cameras simultaneously, one of the cameras encounters an abnormal state after some time. Please find the log details below:
- Log Information for Launch Command: [launch.log](./logs/20240305/launch.log)
- Output of `sudo dmesg`: [dmesg.log](./logs/20240305/dmesg.log)
- Output of `sudo ZED_Diagnostic --dmesg`: [zed_dmesg.log](./logs/20240305/zed_dmesg.log)

Additionally, system error information can be found here: [nvargus-daemon.crash](./logs/20240305/_usr_sbin_nvargus-daemon.0.crash)

