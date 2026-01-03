![Untitleddesign1-ezgif com-video-to-gif-converter](https://github.com/user-attachments/assets/e69a8bda-9102-4a37-be20-4eb12b4dffae)


# ORB-SLAM3 ROS2 Wrapper Docker

This repository contains a dockerized comprehensive wrapper for ORB-SLAM3 on ROS 2 Humble for Ubuntu 22.04.


# Steps to use this wrapper

## 1. Clone this repository

1. ```git clone https://github.com/suchetanrs/ORB-SLAM3-ROS2-Docker```
2. ```cd ORB-SLAM3-ROS2-Docker```
3. ```git submodule update --init --recursive --remote```

## 2. Install Docker on your system

```bash
cd ORB-SLAM3-ROS2-Docker
sudo chmod +x container_root/shell_scripts/docker_install.sh
./container_root/shell_scripts/docker_install.sh
```

## 3. Build the image with ORB_SLAM3

1. Build the image: ```sudo docker build --build-arg USE_CI=false -t orb-slam3-humble:22.04 .```
2. Add ```xhost +``` to your ```.bashrc``` to support correct x11-forwarding using ```echo "xhost +" >> ~/.bashrc```
3. ```source ~/.bashrc```
4. You can see the built images on your machine by running ```sudo docker images```.

## 4. Running the container

1. ```cd ORB-SLAM3-ROS2-Docker``` (ignore if you are already in the folder)
2. ```sudo docker compose run orb_slam3_22_humble```
3. This should take you inside the container. Once you are inside, run the command ```xeyes``` and a pair of eyes should pop-up. If they do, x11 forwarding has correctly been setup on your computer.

## 5. Building the ORB-SLAM3 Wrapper

Launch the container using steps in (4).
```bash
cd /home/orb/ORB_SLAM3/ && sudo chmod +x build.sh && ./build.sh
cd /root/colcon_ws/ && colcon build --symlink-install && source install/setup.bash
```

## Launching ORB-SLAM3

Launch the container using steps in (4).
If you are inside the container, run the following:

1. ```ros2 launch orb_slam3_ros2_wrapper unirobot.launch.py```
3. You can adjust the robot namespace in the ```unirobot.launch.py``` file.

## Running this with a Gazebo Sim simulation.
Setup the ORB-SLAM3 ROS2 Docker using the steps above. Once you do (1) step in the ```Launching ORB-SLAM3``` section, you should see a window popup which is waiting for images. This is partially indicative of the setup correctly done.

## Running the map_generator package.

This package can be used to generate a global pointcloud from the SLAM.
It subscribes to the published `map_data` and an input pointcloud either from a LiDAR or from a depth camera. It stitches the input pointclouds together based on the latest pose-graph data.

To run the package, these steps can be followed:
1. Make sure the simulation is running. Run the orb_slam3 container and once you are in the bash shell, run the following: `./launch_slam.sh`
2. The top-left terminal contains the launch file to run the slam. This must be launched first.
3. The bottom-left terminal contains the launch file to start the pointcloud_stitcher node. This should run soon after you launch the SLAM.
4. If you wish to publish the global pointcloud at any point during the SLAM's operation, simply run the python file in the top-right terminal. You should be able to view the global pointcloud in rviz (you can launch RViz with the correct configuration from the bottom-right terminal).

### ROS 2 Domain ID

This project uses a custom ROS 2 Domain ID to avoid conflicts with other ROS 2 systems.

Please export the following before running any ROS 2 commands:

```bash
export ROS_DOMAIN_ID=55

