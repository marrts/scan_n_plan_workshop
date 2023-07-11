# SNP Implementation with a UR3

This demo uses a UR3 mounted on a table with an Intel RealSense d405 camera to reconstruct the surface of an arbitrary part and generate motion plans for polishing the part in a raster pattern

## Build Setup

1. Follow the [build setup instructions](https://github.com/ros-industrial-consortium/scan_n_plan_workshop#build-setup) for the main repository

1. Clone the application-specific ROS2 dependencies into the same workspace
    ```
    cd <snp_workspace>
    vcs import src < snp_ur3/dependencies.repos
    ```

## Build

```
colcon build --cmake-args -DTESSERACT_BUILD_FCL=OFF
```

## ROS1 Hardware Interface Software Installation

Install the requisite ROS1 driver software to run the application on hardware

**Note: this step is not required to run the application in simulation only**

1. Build the ros1_bridge
    - Create a new workspace, clone this branch of the `ros1_bridge` repository
      ```bash
      git clone -b action_bridge https://github.com/ipa-hsd/ros1_bridge.git
      ```
    - Source the both ROS distros
      ```bash
      source /opt/ros/foxy/setup.bash
      source /opt/ros/noetic/setup.bash
      ```
    - Build the bridge
      ```bash
      colcon build --symlink-install --packages-select ros1_bridge --cmake-force-configure
      ```

1. Build the ROS1 workspace
    - TODO

## Running the system
### Simulation
```bash
ros2 launch snp_ur3 start.launch.xml
```

### On Hardware
1. Start the ROS1 launch file in a new terminal
    ```bash
    cd <ros1_workspace>
    source devel/setup.bash
    TODO
    ```
1. Run the bridge in a second terminal
    ```bash
    cd <bridge_workspace>
    source install/setup.bash
    ros2 run ros1_bridge dynamic_bridge --bridge-all-1to2-topics
    ```
1. Start the ROS2 launch file in a third terminal
    ```bash
    cd <snp_workspace>
    source install/setup.bash
    ros2 launch snp_ur3 start.launch.xml sim_robot:=false sim_vision:=false
    ```
