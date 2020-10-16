# spdlog Pollution Repro with an Installed Drake

## Instructions

To use `ament_cmake` and `colcon` from the ROS 2 Eloquent package archive, install
the required packages and configure your environment as follows:
```
sudo ../scripts/setup/linux/ubuntu/bionic/install_prereqs --ros-eloquent
source /opt/ros/eloquent/setup.bash
```

To build the `spdlog_pollution_repro` library:
```
colcon build --cmake-args "-DCMAKE_PREFIX_PATH=/path/to/drake;$CMAKE_PREFIX_PATH"
```

*If the Drake binary package is installed to `/opt/drake`, you may omit the
`--cmake-args <args>` argument.*

To run the `spdlog_pollution_repro` executable:
```
source install/setup.bash
spdlog_pollution_repro
```

## Impressions

Segmentation faults occur within `rcl_logging_spdlog` API implementations.
As of ROS 2 Eloquent, `rcl_logging_spdlog` is built against header-only 
[`spdlog` v1.3.1](https://github.com/ros2/rcl_logging/tree/eloquent/rcl_logging_spdlog), 
while latest Drake uses compiled [`spdlog` v1.8.1](https://github.com/RobotLocomotion/drake/tree/24b05efb0a13328bfdec380d779a4a09f08f0302/tools/workspace/spdlog).
Weak (W) `spdlog` symbols in `librcl_logging_spdlog.so` (coming from C++ template instantiations) 
appear to be overriden by regular text (T) symbols in `libdrake_spdlog.so`.
