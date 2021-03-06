cmake_minimum_required(VERSION 2.8.3)
project(libphidgets)

find_package(catkin REQUIRED COMPONENTS rosbuild roslib mk rosbuild rospack)

add_custom_target(
    build_phidget21 ALL
    COMMAND cmake -E chdir ${PROJECT_SOURCE_DIR} make -f Makefile.phidgets)

add_custom_target(
    copy_phidget21_plain ALL
    COMMAND cmake -E copy ${PROJECT_SOURCE_DIR}/lib/libphidget21.so.0  ${PROJECT_SOURCE_DIR}/lib/libphidget21.so
)

add_custom_target(
    copy_phidget21 ALL
    COMMAND cmake -E copy ${PROJECT_SOURCE_DIR}/lib/libphidget21.so.0  ${CATKIN_DEVEL_PREFIX}/${CATKIN_GLOBAL_LIB_DESTINATION}/libphidget21.so
)
add_custom_target(
    copy_phidget210 ALL
    COMMAND cmake -E copy ${PROJECT_SOURCE_DIR}/lib/libphidget21.so.0  ${CATKIN_DEVEL_PREFIX}/${CATKIN_GLOBAL_LIB_DESTINATION}/libphidget21.so.0
)
message(STATUS "Copying libs to ${CATKIN_DEVEL_PREFIX}/${CATKIN_GLOBAL_LIB_DESTINATION}")

catkin_package(
    DEPENDS libusb-dev
    INCLUDE_DIRS include
    LIBRARIES phidget21
)

include_directories(${catkin_INCLUDE_DIRS})
add_library(phidget21 SHARED)
set_target_properties(phidget21 PROPERTIES LINKER_LANGUAGE CXX)

add_dependencies(copy_phidget21_plain build_phidget21)
add_dependencies(copy_phidget21 build_phidget21)
add_dependencies(copy_phidget210 build_phidget21)
add_dependencies(phidget21 copy_phidget21_plain)
add_dependencies(phidget21 copy_phidget21)
add_dependencies(phidget21 copy_phidget210)

install(FILES lib/libphidget21.so lib/libphidget21.so.0 lib/libphidget21.so.0.0.0
  DESTINATION ${CATKIN_PACKAGE_LIB_DESTINATION}
)

install(DIRECTORY include/
  DESTINATION ${CATKIN_GLOBAL_INCLUDE_DESTINATION}
)





