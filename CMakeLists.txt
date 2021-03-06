cmake_minimum_required(VERSION 2.8.3)
project(moveit_visual_tools)

find_package(Eigen REQUIRED)
find_package(Boost REQUIRED thread system)

# Load catkin and all dependencies required for this package
find_package(catkin REQUIRED COMPONENTS
  eigen_conversions 
  geometry_msgs
  moveit_ros_robot_interaction 
  moveit_core
  roscpp
  tf_conversions
  visualization_msgs
  graph_msgs
  std_msgs
  trajectory_msgs
)

# Catkin
catkin_package(
  LIBRARIES
    ${PROJECT_NAME}
  CATKIN_DEPENDS
    moveit_ros_robot_interaction
    moveit_core
    geometry_msgs
    visualization_msgs 
    graph_msgs
    std_msgs
    moveit_msgs
    trajectory_msgs
  INCLUDE_DIRS include
)

# Enable gdb to show line numbers
set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -Wall -g -W -Wall -Wno-unused-parameter -fno-strict-aliasing")
#set(CMAKE_BUILD_TYPE Debug)

## Build 

include_directories(
  include 
  ${catkin_INCLUDE_DIRS}
)

# Visualization Tools Library
add_library(${PROJECT_NAME}
  src/visual_tools.cpp
)

add_dependencies(${PROJECT_NAME} graph_msgs_generate_messages_cpp)

target_link_libraries(${PROJECT_NAME}
  ${catkin_LIBRARIES} ${Boost_LIBRARIES}
)

# Test executable
add_executable(visual_tools_test src/visual_tools_test.cpp)
target_link_libraries(visual_tools_test 
  ${catkin_LIBRARIES} ${PROJECT_NAME}
)

# Install
install(TARGETS ${PROJECT_NAME} LIBRARY DESTINATION ${CATKIN_PACKAGE_LIB_DESTINATION})

# Mark cpp header files for installation
install(DIRECTORY include/${PROJECT_NAME}/
  DESTINATION ${CATKIN_PACKAGE_INCLUDE_DESTINATION}
  FILES_MATCHING PATTERN "*.h"
)
