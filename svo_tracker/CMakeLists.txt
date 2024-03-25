project(svo_tracker)
cmake_minimum_required(VERSION 2.8.3)

find_package(catkin_simple REQUIRED)
catkin_simple()
find_package(Glog REQUIRED)
add_compile_definitions(GLOG_USE_GLOG_EXPORT)
ADD_DEFINITIONS(
    -std=c++14
)
include(SvoSetup)

set(HEADERS
  include/svo/tracker/feature_tracking_types.h
  include/svo/tracker/feature_tracking_utils.h
  include/svo/tracker/feature_tracking_viz.h
  include/svo/tracker/feature_tracker.h
  )

set(SOURCES
  src/feature_tracking_types.cpp
  src/feature_tracking_utils.cpp
  src/feature_tracking_viz.cpp
  src/feature_tracker.cpp
  )

cs_add_library(${PROJECT_NAME} ${SOURCES} ${HEADERS})

################################################################################
cs_install()
cs_export()

