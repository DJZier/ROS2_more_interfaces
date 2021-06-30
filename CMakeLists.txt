cmake_minimum_required(VERSION 3.5)
project(more_interfaces)

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
# Find the package that generates message code from msg/srv files:
find_package(rosidl_default_generators REQUIRED)
find_package(more_interfaces REQUIRED)
# uncomment the following section in order to fill in
# further dependencies manually.
# find_package(<dependency> REQUIRED)

# Declare the list of messages you want to generate:
set(msg_files
  "msg/AddressBook.msg"
)
# Generate the messages:
rosidl_generate_interfaces(${PROJECT_NAME}
  ${msg_files}
)
# Also make sure you export the message runtime dependency:
ament_export_dependencies(rosidl_default_runtime)

# We need to create a new target for this node (address_book)
find_package(rclcpp REQUIRED)

add_executable(publish_address_book
  src/publish_address_book.cpp
)
add_executable(suscribe_address_book
  src/suscribe_address_book.cpp
)

ament_target_dependencies(publish_address_book
  "rclcpp"
)
ament_target_dependencies(suscribe_address_book
  "rclcpp"
)

install(TARGETS 
  publish_address_book
  suscribe_address_book
 DESTINATION lib/${PROJECT_NAME})


 rosidl_target_interfaces(publish_address_book
 ${PROJECT_NAME} "rosidl_typesupport_cpp")

 rosidl_target_interfaces(suscribe_address_book
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
