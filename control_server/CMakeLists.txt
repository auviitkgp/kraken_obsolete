# Catkin User Guide: http://www.ros.org/doc/groovy/api/catkin/html/user_guide/user_guide.html
# Catkin CMake Standard: http://www.ros.org/doc/groovy/api/catkin/html/user_guide/standards.html
cmake_minimum_required(VERSION 2.8.3)
project(control_server)
find_package(catkin REQUIRED COMPONENTS roscpp controller_basic kraken_msgs actionlib actionlib_msgs resources)

#set the default path for built executables to the "bin" directory
set(EXECUTABLE_OUTPUT_PATH ${PROJECT_SOURCE_DIR}/bin)
#set the default path for built libraries to the "lib" directory
set(LIBRARY_OUTPUT_PATH ${PROJECT_SOURCE_DIR}/lib)

include_directories(${Boost_INCLUDE_DIRS} include ${catkin_INCLUDE_DIRS})

add_library(AuvController src/AuvController.cpp)
target_link_libraries(AuvController ${catkin_LIBRARIES})

add_library(ControlServer src/ControlServer.cpp)
target_link_libraries(ControlServer AuvController)

add_executable(controlServer src/main.cpp)
target_link_libraries(controlServer AuvController ControlServer)

add_executable(client src/test.cpp)
target_link_libraries(client AuvController)

add_executable(client_action src/test_action_lib.cpp)

catkin_package(
    DEPENDS roscpp controller_basic kraken_msgs actionlib actionlib_msgs resources
    CATKIN_DEPENDS # TODO
    INCLUDE_DIRS # TODO include
    LIBRARIES # TODO
)
