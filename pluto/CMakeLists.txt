cmake_minimum_required(VERSION 2.8.3)
project(pluto)


find_package(catkin REQUIRED COMPONENTS
  roscpp
  rospy
  std_msgs
)

catkin_package(
  INCLUDE_DIRS include
#  LIBRARIES pluto
#  CATKIN_DEPENDS roscpp rospy std_msgs
#  DEPENDS system_lib
)

include_directories( 
  include
  ${catkin_INCLUDE_DIRS}
)


