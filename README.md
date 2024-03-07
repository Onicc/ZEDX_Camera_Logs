# ZEDX_Camera_Logs

### System Information

- **Machine Model:** [Vecow EAC5000](https://www.vecow.com/dispPageBox/vecow/VecowCT.aspx?ddsPageID=PRODUCTDTL_EN&dbid=4852986947)
- **Firmware Version:** EAC-5000_64GB_EMMC-BOOT_JetPack5.1.1_230713_ZEDX
- **Operating System Version:** Ubuntu 20.04
- **ROS Version:** ROS2 Humble
- **ZED Model:** ZED X

### Launch Script

Please refer to the [zedx_multi_camera.launch.py](./zedx_multi_camera.launch.py) file. This script utilizes the [zed-ros2-wrapper](https://github.com/stereolabs/zed-ros2-wrapper)'s [zed_camera.launch.py](https://github.com/stereolabs/zed-ros2-wrapper/blob/master/zed_wrapper/launch/zed_camera.launch.py) script, specifying cameras by their serial numbers. Two ZED X cameras are connected to the first and third ports of GMSL2 on the EAC5000.

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

