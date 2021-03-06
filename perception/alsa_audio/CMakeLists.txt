cmake_minimum_required(VERSION 2.4.6)
include($ENV{ROS_ROOT}/core/rosbuild/rosbuild.cmake)

# Set the build type.  Options are:
#  Coverage       : w/ debug symbols, w/o optimization, w/ code-coverage
#  Debug          : w/ debug symbols, w/o optimization
#  Release        : w/o debug symbols, w/ optimization
#  RelWithDebInfo : w/ debug symbols, w/ optimization
#  MinSizeRel     : w/o debug symbols, w/ optimization, stripped binaries
#set(ROS_BUILD_TYPE RelWithDebInfo)

rosbuild_init()

rosbuild_genmsg()
rosbuild_gensrv()

#set the default path for built executables to the "bin" directory
set(EXECUTABLE_OUTPUT_PATH ${PROJECT_SOURCE_DIR}/bin)
#set the default path for built libraries to the "lib" directory
set(LIBRARY_OUTPUT_PATH ${PROJECT_SOURCE_DIR}/lib)

find_package(Eigen REQUIRED)
include_directories(${EIGEN_INCLUDE_DIRS})
add_definitions(${EIGEN_DEFINITIONS})

rosbuild_add_library(${PROJECT_NAME}
  src/audio_processor.cpp
)
target_link_libraries(${PROJECT_NAME} asound fftw3)
# target_link_libraries(${PROJECT_NAME} asound fftw3 profiler)

rosbuild_add_executable(audio_processor_node
  src/audio_processor_node.cpp
)
target_link_libraries(audio_processor_node ${PROJECT_NAME})

#rosbuild_add_boost_directories()
#rosbuild_link_boost(${PROJECT_NAME} thread)
