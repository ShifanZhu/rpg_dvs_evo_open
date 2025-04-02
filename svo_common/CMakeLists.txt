project(svo_common)
cmake_minimum_required(VERSION 2.8.3)
ADD_DEFINITIONS(
    -std=c++14
)


find_package(catkin_simple REQUIRED)
catkin_simple()
find_package(Glog REQUIRED)
add_compile_definitions(GLOG_USE_GLOG_EXPORT)
include(SvoSetup)

set(HEADERS
  include/svo/common/camera.h
  include/svo/common/camera_fwd.h
  include/svo/common/feature_wrapper.h
  include/svo/common/frame.h
  include/svo/common/imu_calibration.h
  include/svo/common/logging.h
  include/svo/common/occupancy_grid_2d.h
  include/svo/common/point.h
  include/svo/common/seed.h
  include/svo/common/transformation.h
  include/svo/common/types.h
  )

set(SOURCES
  src/frame.cpp
  src/point.cpp
  )
  include_directories(${GLOG_INCLUDE_DIRS})

cs_add_library(${PROJECT_NAME} ${SOURCES} ${HEADERS})
target_link_libraries(${PROJECT_NAME} glog::glog)
################################################################################
cs_install()
cs_export()
