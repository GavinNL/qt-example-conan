cmake_minimum_required(VERSION 3.1.0)

project(testproject)

################################################################################
# This is needed to run Conan with Cmake.
################################################################################
if( ${CMAKE_BINARY_DIR}/conanbuildinfo.cmake )
endif()
#include(${CMAKE_BINARY_DIR}/conanbuildinfo.cmake)
#set(CONAN_SYSTEM_INCLUDES ON)
#conan_basic_setup(TARGETS)
################################################################################

# Populate a CMake variable with the sources
set(helloworld_SRCS
    mainwindow.ui
    mainwindow.cpp
    main.cpp
)

# Find the QtCore library
find_package(Qt5Core  REQUIRED)
find_package(Qt5Widgets  REQUIRED)


# Find includes in corresponding build directories
set(CMAKE_INCLUDE_CURRENT_DIR ON)
# Instruct CMake to run moc automatically when needed
set(CMAKE_AUTOMOC ON)
# Create code from a list of Qt designer ui files
set(CMAKE_AUTOUIC ON)

# Tell CMake to create the helloworld executable
add_executable(helloworld WIN32 ${helloworld_SRCS})

# Use the Widgets module from Qt 5
target_link_libraries(helloworld Qt5::Widgets Qt5::Core)
