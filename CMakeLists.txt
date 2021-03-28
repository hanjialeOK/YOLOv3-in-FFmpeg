cmake_minimum_required(VERSION 3.0 FATAL_ERROR)
project(myplay)

find_package(OpenCV REQUIRED)
include_directories(${OpenCV_INCLUDE_DIRS})

find_package(SDL2 REQUIRED)
include_directories(${SDL2_INCLUDE_DIRS})

# FFmpeg source
set(FFMPEG_SOURCE ~/ffmpeg-4.3.1)
include_directories(${FFMPEG_SOURCE})

# FFmpeg build
set(FFMPEG_BUILD ~/ffmpeg-4.3.1/build)
include_directories(${FFMPEG_BUILD})

link_directories(${FFMPEG_BUILD}/libavcodec)
link_directories(${FFMPEG_BUILD}/libavdevice)
link_directories(${FFMPEG_BUILD}/libavfilter)
link_directories(${FFMPEG_BUILD}/libavformat)
link_directories(${FFMPEG_BUILD}/libavutil)
link_directories(${FFMPEG_BUILD}/libswresample)
link_directories(${FFMPEG_BUILD}/libswscale)

find_package(Torch REQUIRED PATHS ~/libtorch-1.8.1)

set(CMAKE_CXX_FLAGS   "-fpermissive -w -g")

aux_source_directory(. DIR_SRCS)

add_executable(myplay ${DIR_SRCS})

target_link_libraries(myplay 
                     ${OpenCV_LIBS}
                     ${TORCH_LIBRARIES}
                     -lavcodec
                     -lavdevice
                     -lavfilter
                     -lavformat
                     -lavutil
                     -lswresample
                     -lswscale
                     -lm
                     -lSDL2
                     )

set_property(TARGET myplay PROPERTY CXX_STANDARD 14)

SET(CMAKE_INSTALL_PREFIX /usr/local)

install(TARGETS myplay
        RUNTIME DESTINATION bin  # 可执行文件安装路径
        )

# uninstall target
if(NOT TARGET uninstall)
  configure_file(
    "${CMAKE_CURRENT_SOURCE_DIR}/cmake_uninstall.cmake.in"
    "${CMAKE_CURRENT_BINARY_DIR}/cmake_uninstall.cmake"
    IMMEDIATE @ONLY)

  add_custom_target(uninstall
    COMMAND ${CMAKE_COMMAND} -P ${CMAKE_CURRENT_BINARY_DIR}/cmake_uninstall.cmake)
endif()

