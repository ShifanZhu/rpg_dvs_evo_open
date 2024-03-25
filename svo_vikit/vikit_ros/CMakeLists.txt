project(vikit_ros)
cmake_minimum_required(VERSION 2.8.3)

find_package(catkin_simple REQUIRED)
find_package(Glog REQUIRED)
catkin_simple(ALL_DEPS_REQUIRED)
ADD_DEFINITIONS(
    -std=c++14
    -lglog
)
include(SvoSetup)

set(HEADERS
  include/vikit/img_type_conversion.h
  include/vikit/output_helper.h
  include/vikit/params_helper.h
  )

set(SOURCES
  src/output_helper.cpp
  src/img_type_conversion.cpp
  )
include_directories(${GLOG_INCLUDE_DIRS})

# Create vikit library
cs_add_library(${PROJECT_NAME} ${SOURCES} ${HEADERS})
target_link_libraries(${PROJECT_NAME} glog::glog)

################################################################################
cs_install()
cs_export()
