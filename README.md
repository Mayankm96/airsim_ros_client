# airsim_ros_wrapper

## Overview

This repository is meant to integrate ROS and AirSim plugin using the python APIs available for the simulator.

The `airsim_ros_wrapper` package has been tested under ROS Kinetic and Ubuntu 16.04LTS. The source code is released under [MIT Licence](LICENSE).

## Installation

### Building from Source

#### Dependencies

Install the python APIs for AirSim
```
pip install airsim
```

#### Building
* To build from source, clone the latest version from this repository into your catkin workspace
```
cd ~/catkin_ws/src
https://github.com/Mayankm96/airsim_ros_wrapper.git
```
* In order to run AirSim, you will have to change the `Airsim_ip` and `Airsim_port` parameters to match the IP/Ports in which Airsim is running. All these informations can be found in the `settings.json` file (located at `~/Documents/AirSim`) for your Airsim configuration. The ports you are looking for are the "LogViewerPort" and the "UdpPort". Note that the settings.json file have to be configured such that "LogViewerHostIp" and "UdpIp" both have the IP of the computer that will run AirSim.

* To compile the package:
```
cd ~/catkin_ws
catkin_make
```

## Usage

Before running the nodes in the package, you need to run Airsim plugin in the Unreal Engine. In case you are unfamiliar on how to do so, refer to the tutorials available [here](https://github.com/Microsoft/AirSim#tutorials).

### Running the `tf` publisher of drone model (DJI M100)

To use the [`urdf`](urdf) model of the drone used in AirSim simulator, then run:
```
roslaunch airsim_ros_wrapper publish_tf.launch
```

__NOTE:__ In the modified blueprint of the drone for UE4, all cameras are downward-facing.

### Running image publisher

Change the IP and Port configurations in [`pubImages.launch`](launch/pubImages.launch)  to match the settings in which Airsim is running. Then:
```
roslaunch airsim_ros_wrapper pubImages.launch
```

## Nodes

### airsim_imgPublisher

This is a client node at ([`img_publisher.py`](scripts/img_publisher.py)) interfaces with the AirSim plugin to retrieve the drone's pose and camera images **(rgb, depth)**.

#### Published Topics

* **`/airsim/rgb/image_raw`** ([sensor_msgs/Image])

	The rgb camera images.

* **`/airsim/depth`** ([sensor_msgs/Image])

	The depth camera images in 32FC1 encoding.

* **`/airsim/camera_info`** ([sensor_msgs/CameraInfo])

  The rgb camera paramters.

* **`/airsim/depth/camera_info`** ([sensor_msgs/CameraInfo])

  The depth camera paramters.

* **`/tf`**

  tf tree with the origin (`world`), the position/orientation of the quadcoper (`base_link`)

* **`/odom`** ([nav_msgs/Odometry])

  the odometry of the quadcoper (`base_link`) in the `world` frame

* **`/airsim/pose`** ([geometry_msgs/PoseStamped])

  the position/orientation of the quadcoper (`base_link`)

#### Parameters
* **Camera parameters:** `Fx`, `Fy`, `cx`, `cz`, `width`, `height`
* **Publishing frequency:** `loop_rate`
