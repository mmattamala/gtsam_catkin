gtsam_catkin
============

A catkin wrapper for GTSAM 4.0. 

## Instructions

Include this package in your workspace. When you build it with catkin it will download and compile the source code of GTSAM.

By default it uses the `develop` branch. If you want to test other branch/tag, change [this line](CMakeLists.txt#L12).

## Dependencies

This package requires `catkin_simple`: https://github.com/catkin/catkin_simple.git