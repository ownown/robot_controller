cmake_minimum_required(VERSION 3.5)
project(robot_controller)

# Default to C99
if(NOT CMAKE_C_STANDARD)
  set(CMAKE_C_STANDARD 99)
endif()

# Default to C++14
if(NOT CMAKE_CXX_STANDARD)
  set(CMAKE_CXX_STANDARD 14)
endif()

if(CMAKE_COMPILER_IS_GNUCXX OR CMAKE_CXX_COMPILER_ID MATCHES "Clang")
  add_compile_options(-Wall -Wextra -Wpedantic)
endif()

# find dependencies
find_package(ament_cmake REQUIRED)
find_package(rclcpp REQUIRED)
find_package(rcl REQUIRED)
find_package(robot_interfaces REQUIRED)

add_library(BrickPi3 STATIC src/BrickPi3.cpp src/bp_spi.cpp)
target_include_directories(BrickPi3 PUBLIC
  $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/include>
  $<INSTALL_INTERFACE:include>)

  add_executable(controller src/controller.cpp)
  target_include_directories(controller PUBLIC
    $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/include>
    $<INSTALL_INTERFACE:include>)

target_link_libraries(controller BrickPi3)
ament_target_dependencies(controller rclcpp robot_interfaces)

install(TARGETS
  controller
  DESTINATION lib/${PROJECT_NAME})

ament_package()
