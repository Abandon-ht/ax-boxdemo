cmake_minimum_required(VERSION 3.0)
project(ax-box_ppl)
message(STATUS "CMAKE_TOOLCHAIN_FILE = ${CMAKE_TOOLCHAIN_FILE}")
# Include subdirectories
add_subdirectory(box_ppl)

if (CMAKE_BUILD_TYPE MATCHES Debug)
    set(CMAKE_CXX_FLAGS "-fvisibility=hidden -g -O0")
    add_definitions(-D__AX_SKEL_DEBUG__)
elseif (CMAKE_BUILD_TYPE MATCHES Release)
    set(CMAKE_CXX_FLAGS "-fvisibility=hidden -O2 -fdata-sections -ffunction-sections")
endif()
# Define custom targets for install and clean
#add_custom_target(install DEPENDS box_ppl)
#add_custom_target(clean COMMAND ${CMAKE_MAKE_PROGRAM} clean -C box_ppl)

# Set environment variables if needed
#set(ROOTFS_TARGET_PATH "${CMAKE_SOURCE_DIR}/msp/out")