cmake_minimum_required(VERSION 3.4)
project(mujoco_ros_msgs)

find_package(catkin REQUIRED COMPONENTS message_generation std_msgs actionlib_msgs)

add_service_files(FILES
    SetPause.srv
    SimStep.srv
    BootstrapNS.srv
    ShutdownNS.srv
)

generate_messages(DEPENDENCIES std_msgs)

catkin_package(CATKIN_DEPENDS message_runtime std_msgs)
