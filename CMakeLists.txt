cmake_minimum_required(VERSION 2.8.3)
project(coop_localization)

set(CMAKE_CXX_FLAGS "-std=c++0x ${CMAKE_CXX_FLAGS}")

find_package( catkin REQUIRED COMPONENTS
  roscpp
  std_msgs
  image_transport
  cv_bridge
  #aruco
  tf
  message_generation
)

set(CMAKE_MODULE_PATH "/usr/local/lib/cmake/")

#message("HERE------------------------------- : " ${CMAKE_MODULE_PATH}) 

find_package(OpenCV REQUIRED)
find_package(cmake_modules REQUIRED) # Required for the Eigen library
find_package(Eigen REQUIRED)	     # Eigen offers matrix/vector arithmetic operations
find_package(aruco REQUIRED)

#set(aruco_INCLUDE_DIRS "/usr/local/include/aruco")

#message("HERE------------------------- :" ${aruco_INCLUDE_DIRS})
#message("HERE------------------------- :" ${OpenCV_INCLUDE_DIRS})
#message("HERE------------------------- :" ${EIGEN_INCLUDE_DIR})

#message("THERE------------------------- :" ${OpenCV_LIBS})
#message("THERE------------------------- :" ${catkin_LIBRARIES})

include_directories(
  include
  coop_localization/include
  ${roscpp_INCLUDE_DIRS}
  ${catkin_INCLUDE_DIRS}
  ${OpenCV_INCLUDE_DIRS}
  ${aruco_INCLUDE_DIRS}
  ${EIGEN_INCLUDE_DIR}
)

add_message_files( FILES  
  rel.msg 
  update.msg
)
add_service_files(
  FILES
  request.srv
)



generate_messages( DEPENDENCIES  std_msgs geometry_msgs )


catkin_package(CATKIN_DEPENDS 
  roscpp 
  cv_bridge 
  opencv2 
  std_msgs 
  image_transport 
  tf 
  message_runtime
)

catkin_package(
  INCLUDE_DIRS include
)


add_executable(cam_merge src/camera_merge_test.cpp)
target_link_libraries( cam_merge
  ${OpenCV_LIBS}
  ${catkin_LIBRARIES}
  ${aruco_LIBS}
  #$ENV{aruco_LIBRARIES}
)

add_executable( rel_meas src/rel_meas.cpp)
target_link_libraries( rel_meas
  ${OpenCV_LIBS}
  ${catkin_LIBRARIES}
  ${aruco_LIBS}
  #$ENV{aruco_LIBRARIES}
)
add_dependencies(rel_meas coop_localization_generate_messages_cpp)

add_executable( abs_meas src/abs_meas.cpp)
target_link_libraries( abs_meas
  ${OpenCV_LIBS}
  ${catkin_LIBRARIES}
  ${aruco_LIBS}
  #$ENV{aruco_LIBRARIES}
)
add_dependencies(abs_meas coop_localization_generate_messages_cpp)


add_executable( abs_meas_2cams src/abs_meas_2cams.cpp)
target_link_libraries( abs_meas_2cams
  ${OpenCV_LIBS}
  ${catkin_LIBRARIES}
  ${aruco_LIBS}
  #$ENV{aruco_LIBRARIES}
)
add_dependencies(abs_meas coop_localization_generate_messages_cpp)


add_executable( sensor_fusion src/sensor_fusion.cpp)
target_link_libraries( sensor_fusion
  ${catkin_LIBRARIES}
)
add_dependencies(sensor_fusion coop_localization_generate_messages_cpp)

add_executable( sensor_fusion_dropouts src/sensor_fusion_dropouts.cpp)
target_link_libraries( sensor_fusion_dropouts
  ${catkin_LIBRARIES}
)
add_dependencies(sensor_fusion_dropouts coop_localization_generate_messages_cpp)

add_executable( simple_nav src/simple_nav.cpp)
target_link_libraries( simple_nav
  ${catkin_LIBRARIES}
)

add_executable( central_processing src/central_processing.cpp)
target_link_libraries( central_processing
  ${catkin_LIBRARIES}
)
add_dependencies(central_processing coop_localization_generate_messages_cpp)

add_executable( coop_move_to src/coop_move_to.cpp)
target_link_libraries( coop_move_to
  ${catkin_LIBRARIES}
)



