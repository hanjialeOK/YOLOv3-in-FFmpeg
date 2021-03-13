cmake_minimum_required(VERSION 3.0 FATAL_ERROR)
project(myplay)

find_package(OpenCV REQUIRED)
include_directories(${OpenCV_INCLUDE_DIRS})

find_package(SDL2 REQUIRED)
include_directories(${SDL2_INCLUDE_DIRS})

# FFmpeg source
set(MY_LIBRARY ~/ffmpeg-4.3.1)
include_directories(${MY_LIBRARY})

# config.h
set(MY_BUILD_LIBRARY ~/ffmpeg-4.3.1/build)
include_directories(${MY_BUILD_LIBRARY})

# set(Torch_DIR ~/libtorch-1.0.0)
find_package(Torch REQUIRED PATHS ~/libtorch-1.7.1)

# find_library(FFMPEG_LIBS avcodec)
# message("FFMPEG_LIBS: ${FFMPEG_LIBS}")

set(CMAKE_CXX_FLAGS   "-fpermissive -w")

aux_source_directory(. DIR_SRCS)

add_executable(myplay ${DIR_SRCS})

# target_link_libraries(myplay "${TORCH_LIBRARIES}" "${OpenCV_LIBS}")
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
