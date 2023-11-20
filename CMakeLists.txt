#
# (C) Copyright 2015-2023 Johns Hopkins University (JHU), All Rights Reserved.
#
# --- begin cisst license - do not edit ---
#
# This software is provided "as is" under an open source license, with
# no warranty.  The complete license can be found in license.txt and
# http://www.cisst.org/cisst/license.txt.
#
# --- end cisst license ---

cmake_minimum_required (VERSION 3.10)
project (dvrk_robot VERSION 2.2)

find_package (ament_cmake REQUIRED)
find_package (rclcpp REQUIRED)
find_package (std_msgs REQUIRED)
find_package (std_srvs REQUIRED)
find_package (geometry_msgs REQUIRED)
find_package (sensor_msgs REQUIRED)
find_package (diagnostic_msgs REQUIRED)
find_package (cisst_msgs REQUIRED)
find_package (tf2_ros REQUIRED)
find_package (tf2_msgs REQUIRED)
find_package (crtk_msgs REQUIRED)
find_package (cisst_ros2_bridge REQUIRED)
find_package (cisst_ros2_crtk REQUIRED)

# find cisst and make sure the required libraries have been compiled
set (REQUIRED_CISST_LIBRARIES
     cisstCommon
     cisstCommonXML
     cisstCommonQt
     cisstVector
     cisstVectorQt
     cisstOSAbstraction
     cisstMultiTask
     cisstMultiTaskQt
     cisstParameterTypes
     cisstParameterTypesQt
     cisstRobot
     cisstNumerical
     cisstQt
     )

find_package (cisst 1.2.0 REQUIRED ${REQUIRED_CISST_LIBRARIES})

if (cisst_FOUND_AS_REQUIRED)

  include (${CISST_USE_FILE})


  # sawRobotIO1394 has been compiled within cisst, we should find it automatically
  find_package (sawRobotIO1394          2.2.0 REQUIRED)
  find_package (sawControllers          2.1.0 REQUIRED)
  find_package (sawIntuitiveResearchKit 2.2.0 REQUIRED)

  include_directories (
    ${dvrk_robot_SOURCE_DIR}/include
    ${sawRobotIO1394_INCLUDE_DIR}
    ${sawControllers_INCLUDE_DIR}
    ${sawIntuitiveResearchKit_INCLUDE_DIR}
    )

  link_directories (
    ${sawRobotIO1394_LIBRARY_DIR}
    ${sawControllers_LIBRARY_DIR}
    ${sawIntuitiveResearchKit_LIBRARY_DIR}
    )

  # library
  add_library (dvrk_utilities
    include/dvrk_utilities/dvrk_console.h
    src/dvrk_console.cpp
    )
  target_link_libraries (
    dvrk_utilities
    ${sawRobotIO1394_LIBRARIES}
    ${sawIntuitiveResearchKit_LIBRARIES}
    ${sawControllers_LIBRARIES}
    )
  cisst_target_link_libraries (dvrk_utilities ${REQUIRED_CISST_LIBRARIES})
  ament_target_dependencies (
    dvrk_utilities
    rclcpp
    std_msgs
    geometry_msgs
    sensor_msgs
    diagnostic_msgs
    cisst_msgs
    tf2_ros
    crtk_msgs
    cisst_ros2_bridge
    cisst_ros2_crtk
    )

  # executable
  add_executable (dvrk_console_json src/dvrk_console_json.cpp)
  target_link_libraries (
    dvrk_console_json
    ${sawRobotIO1394_LIBRARIES}
    ${sawIntuitiveResearchKit_LIBRARIES}
    ${sawControllers_LIBRARIES}
    )
  cisst_target_link_libraries (dvrk_console_json ${REQUIRED_CISST_LIBRARIES})
  target_link_libraries (dvrk_console_json dvrk_utilities)
  ament_target_dependencies (
    dvrk_console_json
    rclcpp
    std_msgs
    geometry_msgs
    sensor_msgs
    diagnostic_msgs
    cisst_msgs
    tf2_ros
    crtk_msgs
    cisst_ros2_bridge
    cisst_ros2_crtk
    )

  install (TARGETS dvrk_utilities dvrk_console_json
    LIBRARY DESTINATION lib/${PROJECT_NAME}
    RUNTIME DESTINATION lib/${PROJECT_NAME}
    LIBRARY DESTINATION lib
    INCLUDES DESTINATION include
    )

  ament_package ()

endif (cisst_FOUND_AS_REQUIRED)
