cmake_minimum_required(VERSION 3.0)
project(BoxDemo)

# Set paths
set(CUR_PATH "${CMAKE_CURRENT_SOURCE_DIR}")
set(HOME_PATH "${SDK_PATH}")
set(DEMO_SRC_PATH "${HOME_PATH}/app/demo/src")
set(THIRD_PARTY_PATH "${HOME_PATH}/third-party")
set(THIRD_PARTY_LIB_PATH "${THIRD_PARTY_PATH}/lib")
set(THIRD_PARTY_INCLUDE_PATH "${THIRD_PARTY_PATH}/live/out/include")
set(THIRD_PARTY_LIB_PATH "${THIRD_PARTY_PATH}/live/out/lib")
set(ROOTFS_TARGET_PATH "${HOME_PATH}/msp/out")

# Source files
file(GLOB_RECURSE SRCCPPS
        "${CUR_PATH}/demo/src/*.cpp"
        "${CUR_PATH}/src/api/*.cpp"
        "${CUR_PATH}/src/mgr/*.cpp"
        "${CUR_PATH}/src/utils/*.cpp"
        "${CUR_PATH}/src/inference/*.cpp"
        "${CUR_PATH}/src/pipeline/*.cpp"
        "${CUR_PATH}/src/pipeline/hvcfp/*.cpp"
        "${CUR_PATH}/src/tracker/*.cpp"
        "${DEMO_SRC_PATH}/rtsp/AXRTSPClient.cpp"
        "${DEMO_SRC_PATH}/vdec/*.cpp"
        "${DEMO_SRC_PATH}/vdec/stream/*.cpp"
        "${DEMO_SRC_PATH}/log/*.cpp"
        "${DEMO_SRC_PATH}/stage/*.cpp"
        "${DEMO_SRC_PATH}/vo/*.cpp"
        "${DEMO_SRC_PATH}/encoder/VideoEncoder.cpp"
        "${DEMO_SRC_PATH}/mp4enc/Mpeg4Encoder.cpp"
        "${DEMO_SRC_PATH}/pool/AXPoolManager.cpp"
        "${DEMO_SRC_PATH}/utils/ElapsedTimer.cpp"
        "${DEMO_SRC_PATH}/utils/yuv/BmpOSD.cpp"
        "${DEMO_SRC_PATH}/utils/yuv/YuvHandler.cpp"
        "${DEMO_SRC_PATH}/utils/SpsParser.cpp"
        "${DEMO_SRC_PATH}/config/ini/*.cpp"
        "${DEMO_SRC_PATH}/ppl/nvr/hal/rtspdamon.cpp"
        "${DEMO_SRC_PATH}/ppl/nvr/hal/linker.cpp"
        "${DEMO_SRC_PATH}/utils/ping4.cpp"
        "${DEMO_SRC_PATH}/utils/DiskHelper.cpp"
        "${DEMO_SRC_PATH}/utils/TimestampHelper.cpp"
)

# Include directories
include_directories(
        "${CUR_PATH}/inc"
        "${CUR_PATH}/src"
        "${CUR_PATH}/src/3rdparty/eigen3"
        "${CUR_PATH}/demo/include"
        "${ROOTFS_TARGET_PATH}/include"
        "${DEMO_SRC_PATH}/header"
        "${DEMO_SRC_PATH}/rtsp"
        "${DEMO_SRC_PATH}/vdec"
        "${DEMO_SRC_PATH}/encoder"
        "${DEMO_SRC_PATH}/vdec/stream"
        "${DEMO_SRC_PATH}/vdec/drv"
        "${DEMO_SRC_PATH}/utils"
        "${DEMO_SRC_PATH}/utils/yuv"
        "${DEMO_SRC_PATH}/log"
        "${DEMO_SRC_PATH}/stage"
        "${DEMO_SRC_PATH}/vo"
        "${DEMO_SRC_PATH}/pool"
        "${DEMO_SRC_PATH}/encoder"
        "${DEMO_SRC_PATH}/mp4enc"
        "${DEMO_SRC_PATH}/config/ini"
        "${DEMO_SRC_PATH}/ppl/nvr/hal"
        "${THIRD_PARTY_PATH}/ffmpeg/include"
        "${THIRD_PARTY_PATH}/openssl/include"
        "${THIRD_PARTY_PATH}/mp4/include"
        "${THIRD_PARTY_INCLUDE_PATH}/BasicUsageEnvironment"
        "${THIRD_PARTY_INCLUDE_PATH}/liveMedia"
        "${THIRD_PARTY_INCLUDE_PATH}/UsageEnvironment"
        "${THIRD_PARTY_INCLUDE_PATH}/groupsock"
)

# Compile options
add_compile_options(
        --std=c++11
        -Wall
        -DAPP_BUILD_VERSION=\"${SDK_VERSION}\"
        -D__VDEC_PP_FRAME_CTRL__
        -D__LINUX_IO_API__
        -D__RECORD_VB_TIMESTAMP__
)

# Debug flags
if (CMAKE_BUILD_TYPE STREQUAL "Debug")
    add_compile_options(-O0 -ggdb3)
else()
    add_compile_options(-O2)
endif()

# Linking
add_executable(${PROJECT_NAME} ${SRCCPPS})
target_link_libraries(${PROJECT_NAME} PRIVATE
        stdc++
        pthread
        m
        rt
        ax_interpreter
        ax_sys
        ax_vdec
        ax_ivps
        ax_venc
        ax_engine
        ax_vo
        drm
        swresample
        avcodec
        avutil
        avformat
        ssl
        crypto
        mp4
        ${THIRD_PARTY_LIB_PATH}/libliveMedia.a
        ${THIRD_PARTY_LIB_PATH}/libgroupsock.a
        ${THIRD_PARTY_LIB_PATH}/libBasicUsageEnvironment.a
        ${THIRD_PARTY_LIB_PATH}/libUsageEnvironment.a
)

# Library paths
target_link_directories(${PROJECT_NAME} PRIVATE
        "${ROOTFS_TARGET_PATH}/lib"
        "${THIRD_PARTY_PATH}/ffmpeg/lib"
        "${THIRD_PARTY_PATH}/openssl/lib"
        "${THIRD_PARTY_PATH}/mp4/lib"
        "${THIRD_PARTY_PATH}/drm/lib"
)

# Install target
install(TARGETS ${PROJECT_NAME} DESTINATION "${ROOTFS_TARGET_PATH}/bin/BoxDemo")