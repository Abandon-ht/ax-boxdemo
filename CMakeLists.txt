cmake_minimum_required(VERSION 3.0)
project(ax-box_ppl)

# Include subdirectories
add_subdirectory(box_ppl)

# Define custom targets for install and clean
#add_custom_target(install DEPENDS box_ppl)
#add_custom_target(clean COMMAND ${CMAKE_MAKE_PROGRAM} clean -C box_ppl)

# Set environment variables if needed
#set(ROOTFS_TARGET_PATH "${CMAKE_SOURCE_DIR}/msp/out")