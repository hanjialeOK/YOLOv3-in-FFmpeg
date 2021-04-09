cmake_minimum_required(VERSION 3.0 FATAL_ERROR)
project(myplay)

find_package(SDL2 REQUIRED)

find_package(Torch REQUIRED)

# FFmpeg source
set(FFMPEG_SOURCE ~/ffmpeg-4.3.1)

# FFmpeg build
set(FFMPEG_BUILD ~/ffmpeg-4.3.1/build)

set(CMAKE_CXX_FLAGS   "-fpermissive -w -g")
set(CMAKE_INSTALL_PREFIX /usr/local)

aux_source_directory(. DIR_SRCS)
aux_source_directory(YOLOv3/ DIR_SRCS)

add_executable(myplay ${DIR_SRCS})

target_include_directories(myplay PRIVATE
                          ${SDL2_INCLUDE_DIRS}
                          ${FFMPEG_SOURCE}
                          ${FFMPEG_BUILD}
                          )

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

# install target
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
