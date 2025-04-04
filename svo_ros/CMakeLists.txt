project(svo_ros)
cmake_minimum_required(VERSION 2.8.3)

find_package(catkin_simple REQUIRED)
find_package(rostest REQUIRED)
catkin_simple()
find_package(OpenCV 4.2 REQUIRED)
include_directories(${OpenCV_INCLUDE_DIRS})
find_package(Glog REQUIRED)
add_compile_definitions(GLOG_USE_GLOG_EXPORT)
# SET(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -O3 -std=c++14")
ADD_DEFINITIONS(
   -std=c++14
)
# find_package(yaml-cpp 0.6 REQUIRED)
# include_directories(${yaml-cpp_INCLUDE_DIRS})
include(SvoSetup)

set(USE_GTSAM false)

set(HEADERS
  include/svo_ros/svo_interface.h
  include/svo_ros/svo_factory.h
  include/svo_ros/visualizer.h
  include/svo_ros/svo_nodelet.h
  include/svo_ros/csv_dataset_reader.h
)

set(SOURCES
  src/svo_interface.cpp
  src/svo_factory.cpp
  src/svo_node_base.cpp
  src/visualizer.cpp
  src/csv_dataset_reader.cpp
)

if(USE_GTSAM)
    ADD_DEFINITIONS(-DSVO_USE_BACKEND)
    list(APPEND HEADERS
      include/svo_ros/backend_visualizer.h
      include/svo_ros/backend_factory.h)
    list(APPEND SOURCES
      src/backend_visualizer.cpp
      src/backend_factory.cpp)
endif()

cs_add_library(${PROJECT_NAME} ${SOURCES} ${HEADERS})
target_link_libraries(${PROJECT_NAME} ${OpenCV_LIBS} ${svo_backend_LIBRARIES} yaml-cpp glog::glog)

cs_add_executable(svo_node src/svo_node.cpp)
target_link_libraries(svo_node ${PROJECT_NAME} ${OpenCV_LIBS} yaml-cpp glog::glog)

cs_add_library(svo_nodelet src/svo_nodelet.cpp)
target_link_libraries(svo_nodelet ${PROJECT_NAME} ${OpenCV_LIBS} yaml-cpp glog::glog)

cs_install()
install(FILES svo_nodelet.xml
  DESTINATION ${CATKIN_PACKAGE_SHARE_DESTINATION}
)
cs_export()
