cmake_minimum_required(VERSION 3.1.0)

project(testproject)

################################################################################
# This is needed to run Conan with Cmake.
################################################################################
include(${CMAKE_BINARY_DIR}/conanbuildinfo.cmake)
set(CONAN_SYSTEM_INCLUDES ON)
conan_basic_setup(TARGETS)
################################################################################

# Find includes in corresponding build directories
set(CMAKE_INCLUDE_CURRENT_DIR ON)
# Instruct CMake to run moc automatically when needed
set(CMAKE_AUTOMOC ON)
# Create code from a list of Qt designer ui files
set(CMAKE_AUTOUIC ON)

# Populate a CMake variable with the sources
set(helloworld_SRCS
    mainwindow.ui
    mainwindow.cpp
    main.cpp
)

# Find the QtCore library
find_package(Qt5Core CONFIG REQUIRED)
find_package(Qt5Widgets CONFIG REQUIRED)

#add_executable(${PROJECT_NAME} test_package.cpp greeter.h)
#target_link_libraries(${PROJECT_NAME} Qt5::Core)

# Tell CMake to create the helloworld executable
add_executable(helloworld WIN32 ${helloworld_SRCS})

# Use the Widgets module from Qt 5
target_link_libraries(helloworld Qt5::Widgets Qt5::Core)
target_link_libraries(helloworld CONAN_PKG::qt)
