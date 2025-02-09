
# 启动carla server
```
docker run --privileged --gpus all --net=host -e DISPLAY=$DISPLAY carlasim/carla:0.9.13 /bin/bash ./CarlaUE4.sh
```


# 启动和进入docker

Env: 
Ubuntu 20.4 (with python 3.8)
ROS galactic
Cala 0.9.13

启动和进入docker
```
cd /home/work/github/apollo-src/apollo
./docker/scripts/dev_start_ros2.sh -l

./docker/scripts/dev_into2.sh
```
# 编译carla-ros-bridge
```
sudo apt install libpcap-dev
sudo apt install python3-opencv
pip3 install pygame

WS_DIR=/home/host_work/github/sim/

# 首次，clone代码
mkdir -p ${WS_DIR}/carla-ros-bridge && cd ${WS_DIR}/carla-ros-bridge
git clone --recurse-submodules https://github.com/Kailthen/ros-bridge.git src/ros-bridge

# 编译
cd ${WS_DIR}/carla-ros-bridge
rosdep update
rosdep install --from-paths src --ignore-src -ry
colcon build

export CARLA_ROOT=${WS_DIR}/carla
export PYTHONPATH=$PYTHONPATH:$CARLA_ROOT/PythonAPI/carla/dist/carla-0.9.13-py3.8-linux-x86_64.egg:$CARLA_ROOT/PythonAPI/carla

source ./install/setup.bash

# start the basic ROS bridge package
ros2 launch carla_ros_bridge carla_ros_bridge.launch.py timeout:=20000 town:=Town01

ros2 launch carla_ros_bridge carla_ros_bridge.launch.py --ros-args --params-file params_file.yaml

# set sensors
ros2 launch carla_spawn_objects carla_spawn_objects.launch.py

# manual_control
ros2 launch carla_manual_control carla_manual_control.launch.py
```

# rviz
```
source ./install/setup.bash
ros2 run rviz2 rviz2
```

### 手动控制车辆
```
ros2 launch carla_manual_control carla_manual_control.launch.py
```

### 录制点云地图
```
参考 https://carla.readthedocs.io/projects/ros-bridge/en/latest/pcl_recorder/

cd /tmp/pcl_capture
rm -fr *

```
