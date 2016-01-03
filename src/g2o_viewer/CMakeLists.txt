cmake_minimum_required(VERSION 2.8.3)
project(g2o_viewer)

## Find catkin macros and libraries
## if COMPONENTS list like find_package(catkin REQUIRED COMPONENTS xyz)
## is used, also find other catkin packages
find_package(catkin REQUIRED COMPONENTS
  roscpp
  g2o_example
)
catkin_package(
  INCLUDE_DIRS include
  LIBRARIES
  CATKIN_DEPENDS
    roscpp
    g2o_example
)
set(EXECUTABLE_OUTPUT_PATH ${PROJECT_SOURCE_DIR}/bin)
set(LIBRARY_OUTPUT_DIRECTORY ${PROJECT_SOURCE_DIR}/lib)

LIST(APPEND CMAKE_MODULE_PATH ${PROJECT_SOURCE_DIR}/cmake_modules)
SET( G2O_ROOT /usr/local/include/g2o )
FIND_PACKAGE( G2O REQUIRED)
find_package(Eigen3 REQUIRED)
find_package(QGLViewer)
include(${QT_USE_FILE})
QT4_WRAP_UI(UI_HEADERS src/base_main_window.ui src/base_properties_widget.ui)
QT4_WRAP_CPP(UI_SOURCES include/g2o_viewer/main_window.h include/g2o_viewer/properties_widget.h)

include_directories(include include/g2o_viewer ${QGLVIEWER_INCLUDE_DIR}
    ${CMAKE_CURRENT_SOURCE_DIR} ${CMAKE_CURRENT_BINARY_DIR} ${QT_INCLUDES}
    ${G2O_INCLUDE_DIRS} ${EIGEN3_INCLUDE_DIR} ${catkin_INCLUDE_DIRS})
set(VIEWER_LIB_SRC include/g2o_viewer/g2o_viewer_api.h
  src/g2o_qglviewer.cpp include/g2o_viewer/g2o_qglviewer.h
  src/main_window.cpp include/g2o_viewer/main_window.h
  src/stream_redirect.cpp include/g2o_viewer/stream_redirect.h
  src/gui_hyper_graph_action.cpp include/g2o_viewer/gui_hyper_graph_action.h
  src/properties_widget.cpp include/g2o_viewer/properties_widget.h
  src/viewer_properties_widget.cpp include/g2o_viewer/viewer_properties_widget.h
  ${UI_HEADERS} ${UI_SOURCES})

add_library(viewer_library ${VIEWER_LIB_SRC}
)
TARGET_LINK_LIBRARIES(viewer_library ${QGLVIEWER_LIBRARY} ${QT_LIBRARIES} g2o_core g2o_stuff g2o_cli)
SET_TARGET_PROPERTIES(viewer_library PROPERTIES OUTPUT_NAME ${LIB_PREFIX}viewer)

add_executable(g2o_viewer
  src/g2o_viewer.cpp
)
SET_TARGET_PROPERTIES(g2o_viewer PROPERTIES OUTPUT_NAME g2o_viewer${EXE_POSTFIX})
TARGET_LINK_LIBRARIES(viewer_library GL GLU glut ${QGLVIEWER_LIBRARY} ${QT_LIBRARIES} g2o_core g2o_stuff g2o_cli)
TARGET_LINK_LIBRARIES(g2o_viewer viewer_library custom_g2o_types )# g2o_scavislam_types sophus pangolin)

## System dependencies are found with CMake's conventions
# find_package(Boost REQUIRED COMPONENTS system)



## Uncomment this if the package has a setup.py. This macro ensures
## modules and global scripts declared therein get installed
## See http://ros.org/doc/api/catkin/html/user_guide/setup_dot_py.html
# catkin_python_setup()

################################################
## Declare ROS messages, services and actions ##
################################################

## To declare and build messages, services or actions from within this
## package, follow these steps:
## * Let MSG_DEP_SET be the set of packages whose message types you use in
##   your messages/services/actions (e.g. std_msgs, actionlib_msgs, ...).
## * In the file package.xml:
##   * add a build_depend tag for "message_generation"
##   * add a build_depend and a run_depend tag for each package in MSG_DEP_SET
##   * If MSG_DEP_SET isn't empty the following dependency has been pulled in
##     but can be declared for certainty nonetheless:
##     * add a run_depend tag for "message_runtime"
## * In this file (CMakeLists.txt):
##   * add "message_generation" and every package in MSG_DEP_SET to
##     find_package(catkin REQUIRED COMPONENTS ...)
##   * add "message_runtime" and every package in MSG_DEP_SET to
##     catkin_package(CATKIN_DEPENDS ...)
##   * uncomment the add_*_files sections below as needed
##     and list every .msg/.srv/.action file to be processed
##   * uncomment the generate_messages entry below
##   * add every package in MSG_DEP_SET to generate_messages(DEPENDENCIES ...)

## Generate messages in the 'msg' folder
# add_message_files(
#   FILES
#   Message1.msg
#   Message2.msg
# )

## Generate services in the 'srv' folder
# add_service_files(
#   FILES
#   Service1.srv
#   Service2.srv
# )

## Generate actions in the 'action' folder
# add_action_files(
#   FILES
#   Action1.action
#   Action2.action
# )

## Generate added messages and services with any dependencies listed here
# generate_messages(
#   DEPENDENCIES
#   std_msgs  # Or other packages containing msgs
# )

################################################
## Declare ROS dynamic reconfigure parameters ##
################################################

## To declare and build dynamic reconfigure parameters within this
## package, follow these steps:
## * In the file package.xml:
##   * add a build_depend and a run_depend tag for "dynamic_reconfigure"
## * In this file (CMakeLists.txt):
##   * add "dynamic_reconfigure" to
##     find_package(catkin REQUIRED COMPONENTS ...)
##   * uncomment the "generate_dynamic_reconfigure_options" section below
##     and list every .cfg file to be processed

## Generate dynamic reconfigure parameters in the 'cfg' folder
# generate_dynamic_reconfigure_options(
#   cfg/DynReconf1.cfg
#   cfg/DynReconf2.cfg
# )

###################################
## catkin specific configuration ##
###################################
## The catkin_package macro generates cmake config files for your package
## Declare things to be passed to dependent projects
## INCLUDE_DIRS: uncomment this if you package contains header files
## LIBRARIES: libraries you create in this project that dependent projects also need
## CATKIN_DEPENDS: catkin_packages dependent projects also need
## DEPENDS: system dependencies of this project that dependent projects also need


###########
## Build ##
###########

## Specify additional locations of header files
## Your package locations should be listed before other locations
# include_directories(include)
## Declare a C++ library
# add_library(g2o_viewer
#   src/${PROJECT_NAME}/g2o_viewer.cpp
# )

## Add cmake target dependencies of the library
## as an example, code may need to be generated before libraries
## either from message generation or dynamic reconfigure
# add_dependencies(g2o_viewer ${${PROJECT_NAME}_EXPORTED_TARGETS} ${catkin_EXPORTED_TARGETS})

## Declare a C++ executable
# add_executable(g2o_viewer_node src/g2o_viewer_node.cpp)

## Add cmake target dependencies of the executable
## same as for the library above
# add_dependencies(g2o_viewer_node ${${PROJECT_NAME}_EXPORTED_TARGETS} ${catkin_EXPORTED_TARGETS})

## Specify libraries to link a library or executable target against
# target_link_libraries(g2o_viewer_node
#   ${catkin_LIBRARIES}
# )

#############
## Install ##
#############

# all install targets should use catkin DESTINATION variables
# See http://ros.org/doc/api/catkin/html/adv_user_guide/variables.html

## Mark executable scripts (Python etc.) for installation
## in contrast to setup.py, you can choose the destination
# install(PROGRAMS
#   scripts/my_python_script
#   DESTINATION ${CATKIN_PACKAGE_BIN_DESTINATION}
# )

## Mark executables and/or libraries for installation
# install(TARGETS g2o_viewer g2o_viewer_node
#   ARCHIVE DESTINATION ${CATKIN_PACKAGE_LIB_DESTINATION}
#   LIBRARY DESTINATION ${CATKIN_PACKAGE_LIB_DESTINATION}
#   RUNTIME DESTINATION ${CATKIN_PACKAGE_BIN_DESTINATION}
# )

## Mark cpp header files for installation
# install(DIRECTORY include/${PROJECT_NAME}/
#   DESTINATION ${CATKIN_PACKAGE_INCLUDE_DESTINATION}
#   FILES_MATCHING PATTERN "*.h"
#   PATTERN ".svn" EXCLUDE
# )

## Mark other files for installation (e.g. launch and bag files, etc.)
# install(FILES
#   # myfile1
#   # myfile2
#   DESTINATION ${CATKIN_PACKAGE_SHARE_DESTINATION}
# )

#############
## Testing ##
#############

## Add gtest based cpp test target and link libraries
# catkin_add_gtest(${PROJECT_NAME}-test test/test_g2o_viewer.cpp)
# if(TARGET ${PROJECT_NAME}-test)
#   target_link_libraries(${PROJECT_NAME}-test ${PROJECT_NAME})
# endif()

## Add folders to be run by python nosetests
# catkin_add_nosetests(test)
