cmake_minimum_required(VERSION 2.8.3)
project(points_preprocessor_usi)

find_package(catkin REQUIRED COMPONENTS
  roscpp
  std_msgs
  pcl_ros
  pcl_conversions
  velodyne_pointcloud
  velodyne_pcl
)

catkin_package(CATKIN_DEPENDS
	sensor_msgs
	velodyne_pointcloud
        velodyne_pcl
)

###########
## Build ##
###########

include_directories(
  ${catkin_INCLUDE_DIRS}
)

find_package(Eigen3 REQUIRED)

#SET(CMAKE_CXX_FLAGS "-std=c++11 -O2 -g -Wall ${CMAKE_CXX_FLAGS}")
SET(CMAKE_CXX_FLAGS "-std=c++11 -O2 -g -Wall ${CMAKE_CXX_FLAGS}")
set(CMAKE_CXX_STANDARD 14)

link_directories(${PCL_LIBRARY_DIRS})

#Ground Plane Fitter
find_package(PCL 1.8 REQUIRED)
add_definitions(${PCL_DEFINITIONS})

add_executable(groundplanfit 
	nodes/ground_filter/groundplanfit.cpp
)
target_include_directories(groundplanfit PRIVATE
	${PCL_INCLUDE_DIRS}
)

target_link_libraries(groundplanfit
	${catkin_LIBRARIES}
	${PCL_LIBRARIES}
)

#Scan Line Run
add_executable(scanlinerun 
	nodes/clustering/scanlinerun.cpp
)

target_include_directories(scanlinerun PRIVATE
	${PCL_INCLUDE_DIRS}
)

target_link_libraries(scanlinerun
	${catkin_LIBRARIES}
	${PCL_LIBRARIES}
)


