cmake_minimum_required(VERSION 2.8.3)

project(spotdog)

set(CMAKE_CXX_STANDARD 11)

set(CMAKE_BUILD_TYPE RELEASE)

find_package(catkin REQUIRED COMPONENTS
	roscpp
	rospy
	std_msgs

	urdf
	controller_manager
	joint_state_controller
	robot_state_publisher
	angles
	control_toolbox
	gazebo_dev
	hardware_interface
	joint_limits_interface
	pluginlib
	transmission_interface
	gazebo_ros_control
)
find_package(Boost REQUIRED COMPONENTS system)

catkin_package(
	CATKIN_DEPENDS
		angles
		control_toolbox
		controller_manager
		gazebo_dev
		hardware_interface
		joint_limits_interface
		pluginlib
		roscpp
		rospy
		std_msgs
		transmission_interface
		urdf

		gazebo_ros_control
	INCLUDE_DIRS include
	LIBRARIES gazebo_closed_loop_hw_sim
)

find_package(roslaunch)

link_directories(
	$(catkin_LIBRARY_DIRS)
)

include_directories(
	include
	${catkin_INCLUDE_DIRS}
	${Boost_INCLUDE_DIRS}
)

add_library(gazebo_closed_loop_hw_sim src/gazebo_closed_loop_hw_sim.cpp)
target_link_libraries(gazebo_closed_loop_hw_sim ${catkin_LIBRARIES})


#############
## Install ##
#############

INSTALL(TARGETS gazebo_closed_loop_hw_sim 
        LIBRARY DESTINATION ${CATKIN_PACKAGE_LIB_DESTINATION}
)

INSTALL(DIRECTORY include/${PROJECT_NAME}/
        DESTINATION ${CATKIN_PACKAGE_INCLUDE_DESTINATION}
)

INSTALL(FILES gazebo_closed_loop_hw_sim_plugins.xml
        DESTINATION ${CATKIN_PACKAGE_SHARE_DESTINATION}
)

foreach(dir config launch meshes urdf sdf)
	install(DIRECTORY ${dir}/
		DESTINATION ${CATKIN_PACKAGE_SHARE_DESTINATION}/${dir})
endforeach(dir)
