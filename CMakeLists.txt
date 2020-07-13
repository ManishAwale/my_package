cmake_minimum_required(VERSION 3.5)
project(my_package)

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
find_package(std_msgs REQUIRED)
find_package(rosidl_default_generators REQUIRED)

set(msg_files
  "msg/Velocity.msg"
  "msg/Acceleration.msg"
)

rosidl_generate_interfaces(${PROJECT_NAME}
  ${msg_files}
)
ament_export_dependencies(rosidl_default_runtime)

# uncomment the following section in order to fill in
# further dependencies manually.
# find_package(<dependency> REQUIRED)

add_executable(my_node src/my_node.cpp src/StubNode.cpp)
ament_target_dependencies(my_node rclcpp std_msgs)

add_executable(algoRun src/algoRun.cpp src/AlgoNode.cpp)
ament_target_dependencies(algoRun rclcpp std_msgs)
add_executable(talker src/publisher_member_function.cpp)
ament_target_dependencies(talker rclcpp std_msgs)
add_executable(listener src/subscriber_member_function.cpp)
ament_target_dependencies(listener rclcpp std_msgs)

include_directories(
# include
  ${CMAKE_CURRENT_SOURCE_DIR}/include/my_package
)

#target_include_directories(my_node PUBLIC
 # $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/include/my_package>
  #$<INSTALL_INTERFACE:include>)

install(TARGETS
  algoRun
  my_node
  talker
  listener
  EXPORT export_${PROJECT_NAME}
  DESTINATION lib/${PROJECT_NAME})

rosidl_target_interfaces(my_node
  ${PROJECT_NAME} "rosidl_typesupport_cpp")
rosidl_target_interfaces(algoRun
  ${PROJECT_NAME} "rosidl_typesupport_cpp")

if(BUILD_TESTING)
  find_package(ament_lint_auto REQUIRED)
  # the following line skips the linter which checks for copyrights
  # uncomment the line when a copyright and license is not present in all source files
  #set(ament_cmake_copyright_FOUND TRUE)
  # the following line skips cpplint (only works in a git repo)
  # uncomment the line when this package is not in a git repo
  #set(ament_cmake_cpplint_FOUND TRUE)
  ament_lint_auto_find_test_dependencies()
endif()

ament_package()
