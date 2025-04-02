project(dvs_tracking)
cmake_minimum_required(VERSION 2.8.3)

find_package(catkin_simple REQUIRED)
catkin_simple(ALL_DEPS_REQUIRED)

find_package(Boost REQUIRED COMPONENTS system)
include_directories(${Boost_INCLUDE_DIRS})

find_package(OpenCV 4.2 REQUIRED)
include_directories(${OpenCV_INCLUDE_DIRS})

find_package(PCL 1.7 REQUIRED)
include_directories(${PCL_INCLUDE_DIRS})



find_package(Glog REQUIRED)
add_compile_definitions(GLOG_USE_GLOG_EXPORT)

SET(CMAKE_BUILD_TYPE Release) # Release, Release)
ADD_DEFINITIONS(
    -Wno-ignored-qualifiers
    -std=c++14
)

SET(CMAKE_CXX_FLAGS "-pthread")
SET(CMAKE_CXX_FLAGS "-D_LINUX -D_REENTRANT -march=native -Wno-unused-variable -Wno-unused-but-set-variable")

IF(DEFINED ENV{ARM_ARCHITECTURE})
  SET(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -mfpu=neon -march=armv7-a")
  ADD_DEFINITIONS(-DHAVE_FAST_NEON)
ENDIF()

IF(CMAKE_COMPILER_IS_GNUCC)
  SET(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -std=c++0x")
ELSE()
  SET(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -std=c++14")
ENDIF()

SET(CMAKE_CXX_FLAGS_RELEASE "${CMAKE_CXX_FLAGS} -O3 -fsee -fomit-frame-pointer -fno-signed-zeros -fno-math-errno -funroll-loops")

################################################################################
set(HEADERS
  include/dvs_tracking/dvs_tracking_nodelet.h

  include/dvs_tracking/lk_se3.hpp
  include/dvs_tracking/tracker.hpp
  include/dvs_tracking/weight_functions.hpp
)

set(SOURCES
  src/lk_se3.cpp
  src/tracker.cpp
)

cs_add_library(${PROJECT_NAME} ${SOURCES} ${HEADERS})

# Nodes
################################################################################

cs_add_executable(dvs_tracking_ros src/dvs_tracking_ros.cpp)
target_link_libraries(dvs_tracking_ros ${PROJECT_NAME}
  ${OpenCV_LIBS}
  ${PCL_LIBRARIES}
  glog::glog
)

cs_add_library(dvs_tracking_nodelet src/dvs_tracking_nodelet.cpp)
target_link_libraries(dvs_tracking_nodelet ${PROJECT_NAME} glog::glog)


################################################################################
cs_install()
cs_export()
