cmake_minimum_required(VERSION 2.8.3)
project(xarm_hardware_interface)

## Compile as C++11, supported in ROS Kinetic and newer
add_compile_options(-std=c++11)

## Find catkin macros and libraries
## if COMPONENTS list like find_package(catkin REQUIRED COMPONENTS xyz)
## is used, also find other catkin packages
find_package(catkin REQUIRED COMPONENTS
  roscpp
  std_msgs
  hardware_interface
  controller_manager
  control_msgs
  trajectory_msgs
  actionlib
  pluginlib
  transmission_interface
  joint_limits_interface
	
)

catkin_package(
  INCLUDE_DIRS include
  LIBRARIES xarm_hardware_interface
  CATKIN_DEPENDS roscpp std_msgs hardware_interface
    controller_manager
    control_msgs
    trajectory_msgs
    actionlib
    pluginlib
    transmission_interface
    joint_limits_interface
)


include_directories(
  include
  ${catkin_INCLUDE_DIRS}
)

add_library(
  xarm
	include/${PROJECT_NAME}/xarm.h
	src/xarm.cpp
)

target_link_libraries(xarm ${catkin_LIBRARIES})

add_library(xarm_hardware_interface src/xarm_hardware_interface.cpp)
add_dependencies(xarm_hardware_interface xarm )
target_link_libraries(
  xarm_hardware_interface
	xarm
  hidapi-hidraw
	${catkin_LIBRARIES}
)

add_executable(xarm_hardware_interface_node src/xarm_hardware_interface_node.cpp)
add_dependencies(xarm_hardware_interface_node xarm_hardware_interface)

# target_link_libraries(
#   xarm_hardware_interface_node
# 	xarm_hardware_interface
# 	${catkin_LIBRARIES}
# )
  
target_link_libraries(
  xarm
  hidapi-hidraw
	${catkin_LIBRARIES}
)

# target_link_libraries(
#   ${catkin_LIBRARIES}
# )

# target_link_libraries(${PROJECT_NAME}_node
#   ${catkin_LIBRARIES}
# )
