cmake_minimum_required(VERSION 3.0.2)
if(POLICY CMP0048)
  cmake_policy(SET CMP0048 NEW)
endif()
if(POLICY CMP0057)
  cmake_policy(SET CMP0057 NEW)  # support IN_LISTS which is required for PyBind11 2.6 and newer
endif()

project(python_orocos_kdl VERSION 1.5.1)

# find a matching version of orocos_kdl
find_package(orocos_kdl ${PROJECT_VERSION} EXACT REQUIRED)
include_directories(${orocos_kdl_INCLUDE_DIRS})
link_directories(${orocos_kdl_LIBRARY_DIRS})

set(LIBRARY_NAME "PyKDL")

# catkin-specific configuration (optional)
find_package(catkin QUIET)
if(catkin_FOUND)
  catkin_package(
    SKIP_CMAKE_CONFIG_GENERATION
    SKIP_PKG_CONFIG_GENERATION
  )
  set(CMAKE_LIBRARY_OUTPUT_DIRECTORY ${CATKIN_DEVEL_PREFIX}/${PYTHON_INSTALL_DIR})
  set(PYTHON_SITE_PACKAGES_INSTALL_DIR "${PYTHON_INSTALL_DIR}")
endif()

find_package(Python COMPONENTS Interpreter Development)
find_package(pybind11 CONFIG REQUIRED
pybind11_add_module(${LIBRARY_NAME}
  PyKDL/PyKDL.h
  PyKDL/PyKDL.cpp
  PyKDL/frames.cpp
  PyKDL/kinfam.cpp
  PyKDL/framevel.cpp
  PyKDL/dynamics.cpp)
target_link_libraries(${LIBRARY_NAME} PRIVATE ${orocos_kdl_LIBRARIES})
install(TARGETS ${LIBRARY_NAME} DESTINATION "${PYTHON_SITE_PACKAGES_INSTALL_DIR}")
