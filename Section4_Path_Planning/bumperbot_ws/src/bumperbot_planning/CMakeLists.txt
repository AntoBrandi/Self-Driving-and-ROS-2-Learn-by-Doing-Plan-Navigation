cmake_minimum_required(VERSION 3.8)
project(bumperbot_planning)

if(CMAKE_COMPILER_IS_GNUCXX OR CMAKE_CXX_COMPILER_ID MATCHES "Clang")
  add_compile_options(-Wall -Wextra -Wpedantic)
endif()

find_package(ament_cmake REQUIRED)
find_package(ament_cmake_python REQUIRED)
find_package(rclcpp REQUIRED)
find_package(rclpy REQUIRED)
find_package(nav_msgs REQUIRED)
find_package(geometry_msgs REQUIRED)
find_package(tf2_ros REQUIRED)

ament_python_install_package(${PROJECT_NAME})

include_directories(include)

set(dependencies
  rclcpp
  nav_msgs
  geometry_msgs
  tf2_ros
)

add_executable(dijkstra_planner src/dijkstra_planner.cpp)
ament_target_dependencies(dijkstra_planner ${dependencies})

add_executable(a_star_planner src/a_star_planner.cpp)
ament_target_dependencies(a_star_planner ${dependencies})

install(
  DIRECTORY include
  DESTINATION include
)

install(TARGETS
  dijkstra_planner
  a_star_planner
  DESTINATION lib/${PROJECT_NAME}
)

install(PROGRAMS
  ${PROJECT_NAME}/a_star_planner.py
  ${PROJECT_NAME}/dijkstra_planner.py
  DESTINATION lib/${PROJECT_NAME}
)

ament_export_include_directories(include)
ament_export_dependencies(${dependencies})
ament_package()
