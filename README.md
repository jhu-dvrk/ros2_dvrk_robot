dvrk robot
==========

This package contains programs, which fires real robot, publishes and
subscribes ROS 2 topics.

# Depends

* sawIntuititiveResearchKit
* cisst-ros2

IMPORTANT: You first need to make sure you have all your configuration
files ready, very likely in
~/ros2_ws/src/cisst-saw/sawIntuitiveResearchKit/share.  You don't
need to copy your configuration files back and forth anymore.

# How to Run

For the Qt based application without rviz:
```sh
  ros2 run dvrk_robot dvrk_console_json -j <path_to_your_console_config.json>
```

Launch files can be found in the `dvrk_model` package.

We provide a few console configurations for simulated arms in `src/cisst-saw/sawIntuitiveResearchKit/share/console`.

# Using the ROS 2 topics

The best way to figure how to use the ROS topics is to look at the
python ROS wrappers in `dvrk_python` or the Matlab ROS wrappers in
`dvrk_matlab`.  The ROS topics are described in https://github.com/jhu-dvrk/sawIntuitiveResearchKit/wiki/API-2.x
