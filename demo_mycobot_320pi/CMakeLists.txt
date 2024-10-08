cmake_minimum_required(VERSION 3.8)
project(demo_mycobot_320pi)

if(CMAKE_COMPILER_IS_GNUCXX OR CMAKE_CXX_COMPILER_ID MATCHES "Clang")
  add_compile_options(-Wall -Wextra -Wpedantic)
endif()

# find dependencies
find_package(ament_cmake REQUIRED)
find_package(rclcpp REQUIRED)
find_package(moveit_ros_planning_interface REQUIRED)
find_package(moveit_task_constructor_core REQUIRED)

add_executable(demo_mycobot_320pi_moveit src/demo_mycobot_320pi_moveit.cpp)
target_include_directories(demo_mycobot_320pi_moveit PUBLIC
  $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/include>
  $<INSTALL_INTERFACE:include>)
target_compile_features(demo_mycobot_320pi_moveit PUBLIC c_std_99 cxx_std_17)  # Require C99 and C++17
ament_target_dependencies(demo_mycobot_320pi_moveit "moveit_ros_planning_interface" "rclcpp")
install(TARGETS demo_mycobot_320pi_moveit DESTINATION lib/${PROJECT_NAME}) 

add_executable(pick_place src/pick_place.cpp)
target_include_directories(pick_place PUBLIC
  $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/include>
  $<INSTALL_INTERFACE:include>)
target_compile_features(pick_place PUBLIC c_std_99 cxx_std_17)  # Require C99 and C++17 
ament_target_dependencies(pick_place moveit_task_constructor_core rclcpp)
install(TARGETS pick_place  DESTINATION lib/${PROJECT_NAME})

add_executable(pick_place_2 src/pick_place_2.cpp)
target_include_directories(pick_place_2 PUBLIC
  $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/include>
  $<INSTALL_INTERFACE:include>)
target_compile_features(pick_place_2 PUBLIC c_std_99 cxx_std_17)  # Require C99 and C++17
ament_target_dependencies(pick_place_2 moveit_task_constructor_core rclcpp)
install(TARGETS pick_place_2  DESTINATION lib/${PROJECT_NAME})


if(BUILD_TESTING)
  find_package(ament_lint_auto REQUIRED)
  # the following line skips the linter which checks for copyrights
  # comment the line when a copyright and license is added to all source files
  set(ament_cmake_copyright_FOUND TRUE)
  # the following line skips cpplint (only works in a git repo)
  # comment the line when this package is in a git repo and when
  # a copyright and license is added to all source files
  set(ament_cmake_cpplint_FOUND TRUE)
  ament_lint_auto_find_test_dependencies()
endif()

install(DIRECTORY 
  config
  launch 
  DESTINATION share/${PROJECT_NAME}/
)

ament_package()
