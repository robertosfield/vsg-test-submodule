cmake_minimum_required(VERSION 3.7)

project(MyFirstVsgApplication
        VERSION 0.0.0
        DESCRIPTION "Template of how to create a program using VulkanSceneGraph and CMake"
        LANGUAGES CXX
)

# build all examples into the bin directory
set(CMAKE_RUNTIME_OUTPUT_DIRECTORY ${PROJECT_BINARY_DIR}/bin)

# Change the default build type to Release
if(NOT CMAKE_BUILD_TYPE)
    set(CMAKE_BUILD_TYPE Release CACHE STRING "Choose the type of build, options are: None Debug Release RelWithDebInfo MinSizeRel." FORCE)
endif(NOT CMAKE_BUILD_TYPE)

# find the vsg
if (VULKAN_SDK)
    set(ENV{VULKAN_SDK} ${VULKAN_SDK})
endif()

# set the use of C++17 globally as all examples require it
set(CMAKE_CXX_STANDARD 17)

list(APPEND CMAKE_MODULE_PATH ${CMAKE_CURRENT_SOURCE_DIR}/thirdparty/vsg/cmake)
list(APPEND CMAKE_MODULE_PATH ${CMAKE_CURRENT_SOURCE_DIR}/thirdparty/vsg/src/vsg)
set(vsg_DIR ${CMAKE_CURRENT_SOURCE_DIR}/thirdparty/vsg/src/vsg)

message("\n--- CMAKE_MODULE_PATH = ${CMAKE_MODULE_PATH}")
message("--- vsg_DIR = ${vsg_DIR}\n")
message("--- CMAKE_CURRENT_LIST_DIR = ${CMAKE_CURRENT_LIST_DIR}")
message("--- CMAKE_CURRENT_LIST_FILE = ${CMAKE_CURRENT_LIST_FILE}")

set(CMAKE_DISABLE_FIND_PACKAGE_vsg TRUE)
set(CMAKE_DISABLE_FIND_PACKAGE_vsgXchange TRUE)

add_subdirectory(thirdparty/vsg)
#add_subdirectory(thirdparty/osg2vsg)
add_subdirectory(thirdparty/vsgXchange)
#add_subdirectory(thirdparty/vsgImGui)

# find the optional vsgXchange that can be used for reading and range of image and 3d model formats and shader compilation
#find_package(vsgXchange QUIET)
#find_package(vsg)
#find_package(vsgXchange)

# add clobber build target to clear all the non git registered files/directories
add_custom_target(clobber-my-first-vsg-application
    COMMAND git clean -d -f -x
)

set(SOURCES
    src/main.cpp
)

add_executable(myfirstvsgapplication ${SOURCES})

target_link_libraries(myfirstvsgapplication vsg::vsg)

if (vsgXchange_FOUND)
    message("Using vsgXchange")
    target_compile_definitions(myfirstvsgapplication PRIVATE vsgXchange_FOUND)
    target_link_libraries(myfirstvsgapplication vsgXchange::vsgXchange)
endif()

install(TARGETS myfirstvsgapplication
        RUNTIME DESTINATION bin
)

