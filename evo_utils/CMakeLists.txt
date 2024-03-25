cmake_minimum_required(VERSION 2.8.3)
project(evo_utils)
set(CMAKE_BUILD_TYPE Release)
find_package(catkin_simple REQUIRED)
catkin_simple(ALL_DEPS_REQUIRED)

# find_package(OpenCV REQUIRED)
# include_directories(${OpenCV_INCLUDE_DIRS})
SET(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -O3 -std=c++14")
find_package(Glog REQUIRED CONFIG)
add_compile_definitions(GLOG_USE_GLOG_EXPORT)
include_directories(${GLOG_INCLUDE_DIRS})

set(HEADERS
  include/evo_utils/camera.hpp
  include/evo_utils/geometry.hpp
  include/evo_utils/interpolation.hpp
  include/evo_utils/main.hpp
  include/rpg_common_ros/params_helper.hpp
  ${GLOG_LIBRARIES}
)

set(SOURCES
  src/utils_geometry.cpp
  src/camera.cpp
)

cs_add_library(${PROJECT_NAME} ${SOURCES} ${HEADERS} ${GLOG_LIBRARIES} /usr/lib/x86_64-linux-gnu/libglog.so)

## Uncomment this if the package has a setup.py. This macro ensures
## modules and global scripts declared therein get installed
## See http://ros.org/doc/api/catkin/html/user_guide/setup_dot_py.html
catkin_python_setup()

# include_directories(
#   ${catkin_INCLUDE_DIRS}
# )

cs_install()
cs_export()