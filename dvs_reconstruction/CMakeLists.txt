project(dvs_reconstruction)
cmake_minimum_required(VERSION 2.8.3)

find_package(catkin_simple REQUIRED)
catkin_simple(ALL_DEPS_REQUIRED)

find_package(Glog REQUIRED)
include_directories(${GLOG_INCLUDE_DIRS})

SET(CMAKE_BUILD_TYPE Release) # Release, Release
ADD_DEFINITIONS(
    -std=c++14
    # -O3
)

add_compile_definitions(GLOG_USE_GLOG_EXPORT)

################################################################################
set(HEADERS
  include/dvs_reconstruction/poisson_solver/laplace.h
  include/dvs_reconstruction/mosaic.hpp
)

set(SOURCES
  src/mosaic.cpp
  src/poisson_solver/laplace.cpp
)

cs_add_library(${PROJECT_NAME} ${SOURCES} ${HEADERS} ${GLOG_LIBRARIES})

# Nodes
################################################################################

cs_add_executable(dvs_reconstruction_ros src/dvs_reconstruction_ros.cpp)
target_link_libraries(dvs_reconstruction_ros ${PROJECT_NAME}
  ${OpenCV_LIBS}
  ${PCL_LIBRARIES}
  fftw3 fftw3_threads
  # ${GLOG_LIBRARIES}
  glog::glog
  # /usr/lib/x86_64-linux-gnu/libgflags.a 
  # /usr/lib/x86_64-linux-gnu/libglog.a
)


################################################################################
cs_install()
cs_export()
