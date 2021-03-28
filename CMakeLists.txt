cmake_minimum_required(VERSION 3.0 FATAL_ERROR)
project(myplay)

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
