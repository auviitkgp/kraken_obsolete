# Catkin User Guide: http://www.ros.org/doc/groovy/api/catkin/html/user_guide/user_guide.html
# Catkin CMake Standard: http://www.ros.org/doc/groovy/api/catkin/html/user_guide/standards.html
cmake_minimum_required(VERSION 2.8.3)
project(controller_basic)
find_package(catkin REQUIRED COMPONENTS roscpp kraken_msgs resources dynamic_reconfigure)

#set the default path for built executables to the "bin" directory
set(EXECUTABLE_OUTPUT_PATH ${PROJECT_SOURCE_DIR}/bin)
#set the default path for built libraries to the "lib" directory
set(LIBRARY_OUTPUT_PATH ${PROJECT_SOURCE_DIR}/lib)
# ensure CMake creates the bin/ directory automatically
set(CMAKE_RUNTIME_OUTPUT_DIRECTORY ${CMAKE_SOURCE_DIR}/bin)

include_directories(${Boost_INCLUDE_DIRS} include cfg/cpp ${catkin_INCLUDE_DIRS})

add_library(PIDController src/PIDController.cpp)
add_dependencies(PIDController kraken_msgs_gencpp)

add_library(StateController src/StateController.cpp)
add_dependencies(StateController kraken_msgs_gencpp)

add_library(ControlParameters src/ControlParameters.cpp)
add_dependencies(ControlParameters kraken_msgs_gencpp)

add_executable(example src/main.cpp)

add_dependencies(example kraken_msgs_gencpp)

target_link_libraries(example PIDCOntroller StateController
	ControlParameters resources ${catkin_LIBRARIES})

catkin_package(
    DEPENDS roscpp kraken_msgs resources dynamic_reconfigure
    CATKIN_DEPENDS kraken_msgs # TODO
    INCLUDE_DIRS # TODO include
    LIBRARIES # TODO
)

