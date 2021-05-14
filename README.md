# Depth-Cameras


## Performance Comparison (Quick view)

* Compatibility: Kinectv2 > D435i > Zed > Kinectv4
* Depth Accuracy: Kinectv4 > Kinectv2 > D435i > Zed
* Depth Range:
* RGB resolution:
* FPS:
* Price: 




## Current Available Depth Cameras in the Lab:
* [AzureKinect (Kinect V4)](https://docs.microsoft.com/en-us/azure/kinect-dk/hardware-specification)  x 2
* Kinectv2  x 2     - canceled support by Microsoft
* [Realsense D435i](https://www.intelrealsense.com/depth-camera-d435i/) x 0
* [Realsense D455](https://www.intelrealsense.com/depth-camera-d455/) x 1
* [Stereolabs Zed Camera](https://www.stereolabs.com/zed/) x 1
* Realsense LiDAR Camera L515 x 1


## Common Mistakes

* To use the script, you need to set it executable.

```shell
sudo chmod +x yourfile.py
```

* Use USB 3.0 Port instead of 2.0 .


# Installations

## Install ROS Melodic If you havent

### on Computers
See [http://wiki.ros.org/melodic/Installation](http://wiki.ros.org/melodic/Installation).

### on Nvidia Jetson
See [https://github.com/jetsonhacks/installROS](https://github.com/jetsonhacks/installROS).

```
./installROS.sh -p ros-melodic-desktop-full -p ros-melodic-rgbd-launch
```

## Install OpenCV If your havent

### on Computers
I forgot how. Google it yourself.

### on Nvidia Jetson NX Xavier

See [https://github.com/PolyU-Robocon/buildOpenCVXavier](https://github.com/PolyU-Robocon/buildOpenCVXavier).
```shell
git clone https://github.com/PolyU-Robocon/buildOpenCVXavier
cd buildOpenCVXavier

./buildOpenCV.sh
```

## Install K4A SDK and K4A ROS wrapper

### For Azure Kinect 
See [Here the setup guide of K4A (PC only)](https://github.com/PolyU-Robocon/Depth-Cameras/blob/main/Azure-Kinect(Kinectv4)/SETUP.md)
For Jetson and other ARM architecture, go google yourself

## Install KinectV2 SDK and K2 ROS wrapper

### For Kinect V2
See [Here the setup guide of K2](https://github.com/PolyU-Robocon/Depth-Cameras/blob/main/Kinectv2/SETUP.md)


## Install Intel RealSense SDK and Realsense ROS wrapper

If you are using D435/D455/L515, you need Intel RealSense SDK.

### Install Intel RealSense SDK

#### on Linux Computers
See [https://github.com/IntelRealSense/librealsense/blob/master/doc/distribution_linux.md](https://github.com/IntelRealSense/librealsense/blob/master/doc/distribution_linux.md)



#### on Nvidia Jetson
Here is a script to build realsenseSDK on the Nvidia JetsonNX.

```shell
sudo ./InstallRealSenseSDK.sh
```

Verify Installation
```shell
realsense-viewer
```
It should be LibRealSense v2.41.0.

### Install Intel RealSense ROS Wrapper

You need:
* ROS
* RealSense SDK

MAKE SURE THE ROS Wrapper Match the version of RealSense SDK.
otherwise you might encounter this error:
[RealSense SDK 2.0 is not detected in realsense-ros/realsense2_camera even it is installed](https://github.com/IntelRealSense/realsense-ros/issues/1322)

* You can check the version here. [https://github.com/IntelRealSense/realsense-ros/tags](https://github.com/IntelRealSense/realsense-ros/tags)

Create a catkin workspace if you haven't
```shell
mkdir -p ~/catkin_ws/src
cd ~/catkin_ws/src/
```


Specifically, make sure that the ros package `ddynamic_reconfigure` is installed. If you haven't install it through APT:

```shell
git clone https://github.com/pal-robotics/ddynamic_reconfigure.git
```

Get ros package Realsense ROS
```shell
git clone https://github.com/IntelRealSense/realsense-ros.git
cd realsense-ros/

#We use 2.21 for LibRealSense SDK v2.41.0
git checkout 2.2.21

#Instead of
#git checkout `git tag | sort -V | grep -P "^2.\d+\.\d+" | tail -1`


cd ..
```

```shell
catkin_init_workspace
cd ..
catkin_make clean
catkin_make -DCATKIN_ENABLE_TESTING=False -DCMAKE_BUILD_TYPE=Release
catkin_make install
```

If you haven't put it into source
```shell
echo "source ~/catkin_ws/devel/setup.bash" >> ~/.bashrc
source ~/.bashrc
```


[More info of RealSense ROS Wrapper](https://github.com/IntelRealSense/realsense-ros)


### Python Wrapper of Realsense

The pyrealsense2 package is a official wrapper which does support RealSense SDK 2.0.

#### Linux PC environment
For PC environment
```shell
pip3 install pyrealsense2
```

#### Jetson and other ARM achitecture
For Jetson and other ARM achitecture, you need to get the whl file.
Get it from here: [https://pypi.org/project/pyrealsense2-aarch64/#files](https://pypi.org/project/pyrealsense2-aarch64/#files)
Then:
```shell
pip3 install ~/Downloads/pyrealsense2_aarch64-2.23.0-cp36-none-any.whl 
```
