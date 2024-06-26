project(dvs_mapping)
cmake_minimum_required(VERSION 2.8.3)

find_package(catkin_simple REQUIRED)
find_package(OpenCV REQUIRED)
find_package(Glog REQUIRED)
catkin_simple(ALL_DEPS_REQUIRED)
SET(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -O3 -std=c++14")

SET(CMAKE_BUILD_TYPE Release) # Release, Release
ADD_DEFINITIONS(
    -std=c++14
    -fopenmp
    -w
    -Wfatal-errors
)
add_compile_definitions(GLOG_USE_GLOG_EXPORT)

################################################################################

set(HEADERS
  include/dvs_depth_from_defocus/depth_defocus_node.hpp
  include/dvs_depth_from_defocus/depth_vector.hpp
  include/dvs_depth_from_defocus/dvs_mapping_nodelet.h
)

set(SOURCES
  src/depth_defocus_node.cpp
)
# include_directories(${GLOG_INCLUDE_DIRS})

cs_add_library(${PROJECT_NAME} ${SOURCES} ${HEADERS})  
# /usr/lib/x86_64-linux-gnu/libgflags.a /usr/lib/x86_64-linux-gnu/libglog.a
# Executables
################################################################################

cs_add_executable(dvs_mapping_ros src/dvs_mapping_ros.cpp)
target_link_libraries(dvs_mapping_ros ${PROJECT_NAME} ${OpenCV_LIBS} # /usr/lib/x86_64-linux-gnu/libgflags.so
glog::glog)


cs_add_library(dvs_mapping_nodelet src/dvs_mapping_nodelet.cpp)
target_link_libraries(dvs_mapping_nodelet ${PROJECT_NAME} glog::glog)

################################################################################
# Tests

catkin_add_gtest(${PROJECT_NAME}_tests
  test/test_median_filter
  test/test_depth_vector
)
target_link_libraries(${PROJECT_NAME}_tests ${PROJECT_NAME} glog::glog)

################################################################################
cs_install()
cs_export()

