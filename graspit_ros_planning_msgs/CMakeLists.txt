cmake_minimum_required(VERSION 2.8.3)
project(graspit_ros_planning_msgs)

find_package(catkin REQUIRED COMPONENTS
	geometry_msgs
	sensor_msgs
	manipulation_msgs
	message_generation
)

#add_message_files(
#	FILES
#	)

add_service_files(
	FILES
	ClearBodies.srv
	LoadDatabaseModel.srv
	SimulateScan.srv
	VerifyGrasp.srv
	GenerateGrasp.srv
	LoadObstacle.srv
	TestGrasp.srv
)

generate_messages(
	DEPENDENCIES
	geometry_msgs
	sensor_msgs
	manipulation_msgs
)

catkin_package(
#	INCLUDE
#	LIBRARIES
	CATKIN_DEPENDS geometry_msgs sensor_msgs manipulation_msgs message_runtime
#	DEPENDS
)

include_directories( ${catkin_INCLUDE_DIRS} )

#install(DIRECTORY msg DESTINATION ${CATKIN_PACKAGE_SHARE_DESTINATION})
install(DIRECTORY srv DESTINATION ${CATKIN_PACKAGE_SHARE_DESTINATION})
