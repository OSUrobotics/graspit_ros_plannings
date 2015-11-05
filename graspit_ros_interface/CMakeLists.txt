cmake_minimum_required(VERSION 2.8.3)
project(graspit_ros_planning)

find_package(catkin REQUIRED COMPONENTS
	roscpp
	rospy
	std_msgs
	sensor_msgs
	geometry_msgs
	manipulation_msgs
	graspit
	graspit_ros_planning_msgs
)

find_package(Qt4 COMPONENTS QTCORE QTGUI)

execute_process(COMMAND rospack find ${PROJECT_NAME} OUTPUT_VARIABLE PLUGIN_DESTINATION_RAW)
string(STRIP ${PLUGIN_DESTINATION_RAW} PLUGIN_DESTINATION_RAW)
set(PLUGIN_DESTINATION ${PLUGIN_DESTINATION_RAW}/plugins)
message(STATUS "Output GraspIt plugin directory: ${PLUGIN_DESTINATION}")

# Graspit include files should now come directly from the graspit package
set(GRASPIT_ROOT $ENV{GRASPIT})
include_directories(${GRASPIT_ROOT})
include_directories(${GRASPIT_ROOT}/src)
include_directories(${GRASPIT_ROOT}/include)
include_directories(include)
#include_directories(${QT_USE_FILE})
#include_directories($ENV{QTDIR}/include)
#include_directories($<1:/usr/share/qt4/include>)
include_directories(${QT_INCLUDES})

## Old rosbuild commands
# We should try to get Qt's include flags automatically somehow
#find_package(Qt4 REQUIRED)
#include(${QT_USE_FILE})
#rosbuild_add_library(ros_graspit_interface src/ros_graspit_interface.cpp src/main.cpp)

## Newer catkin commands
catkin_package(
	CATKIN_DEPENDS roscpp std_msgs geometry_msgs sensor_msgs manipulation_msgs graspit_ros_planning_msgs
#	DEPENDS
)

## Add the plugin as a shared library
add_library(graspit_ros_plugin
	src/ros_graspit_interface.cpp
	src/main.cpp
)
target_link_libraries(graspit_ros_plugin ${catkin_LIBRARIES} ${QT_LIBRARIES})

#############
## Install ##
#############

## Install the plugin library in the package's present directory
install(TARGETS graspit_ros_plugin
	LIBRARY DESTINATION 	#${CATKIN_PACKAGE_LIB_DESTINATION}
				${PLUGIN_DESTINATION}
	PERMISSIONS OWNER_EXECUTE GROUP_EXECUTE OWNER_READ GROUP_READ OWNER_WRITE GROUP_WRITE
)
