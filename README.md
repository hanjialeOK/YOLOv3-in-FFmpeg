# YOLOv3 in FFmpeg-4.3.1

## Introduction

I am trying to put YOLOv3 in ffplay, a player of FFmpeg, and compile them to myplay. So that it could dectect objects in videos.  
`Darknet.cpp` and `Darknet.h` are files of YOLOv3 which come from [libtorch-yolov3](https://github.com/walktree/libtorch-yolov3). Another three source files come from FFmpeg-4.3.1/fftools, I modified them for a little bit.

## What I am doing

I am testing this project on a GPU machine now.

## Branches

- **master** - Use OpenCV to normallize data (from 0-255 to 0-1) and draw recangle for each detected object.
- **filter** - Use Torch to normalize data, while drawing recangle is implemented completely by ffplay itself. OpenCV is removed.

## Requirements

- CMake >= 3.0
- GNU >= 5.4.0
- LibTorch >= 1.5.0
- SDL2 >= 2.0
- FFmpeg == 4.3.1
- OpenCV >= 3.0 (which is necessary in master, not filter)

## Warning

- FFplay works bad on remote desktop connections. So if you install this project on your remote computer, you might not get a satisfied result unless your remote computer is exactly besides you.
- Strongly suggest you work on a GPU machine with cuda. I got only 1 fps on my CPU machine.

## Installation

### OpenCV

Follow the [Installation in Linux](https://docs.opencv.org/3.4.13/d7/d9f/tutorial_linux_install.html). If you are going to use filter, skip this step.

### SDL2 & yasm

```c
sudo apt install libsdl2-dev
sudo apt install yasm
```

### FFmpeg-4.3.1

```c
wget http://ffmpeg.org/releases/ffmpeg-4.3.1.tar.gz
tar -zxf ffmpeg-4.3.1.tar.gz
cd ffmpeg
mkdir build && cd build
// if cpu
./../configure --prefix=/usr/local --enable-shared
// if gpu
./../configure --prefix=/usr/local --enable-shared --enable-cuda-nvcc
make
sudo make install

export LIBRARY_PATH=/usr/local/lib/:$LIBRARY_PATH
export LD_LIBRARY_PATH=/usr/local/lib/:$LD_LIBRARY_PATH
```

### LibToch

```c
// if cpu
wget https://download.pytorch.org/libtorch/cpu/libtorch-shared-with-deps-1.8.1%2Bcpu.zip
// if gpu
wget https://download.pytorch.org/libtorch/cu102/libtorch-shared-with-deps-1.8.1.zip
wget https://download.pytorch.org/libtorch/cu111/libtorch-shared-with-deps-1.8.1%2Bcu111.zip
```

### Clone

```c
git clone git@github.com:Nugurii/YOLOv3-in-FFmpeg.git MYPLAY-ROOT
// if just need filter
git clone -b filter git@github.com:Nugurii/YOLOv3-in-FFmpeg.git MYPLAY-ROOT
```

### Download weights

```c
cd MYPLAY-ROOT/models
wget https://pjreddie.com/media/files/yolov3.weights
wget https://pjreddie.com/media/files/yolov3-tiny.weights
```

### Configuration

Modify CMakeLists.txt according to your own condition.

- set `FFMPEG_SOURCE` path/to/your/FFmpeg-4.3.1/source
- set `FFMPEG_BUILD` path/to/where/you/build/FFmpeg-4.3.1
- set `Torch PATHS` path/to/your/libtorch

## compile & run

```c
cd MYPLAY-ROOT
mkdir build && cd build
cmake ..
make
// if cpu, it's better to test on videos of which fps is low.
./myplay -v quiet ../videos/fpx_r0.5.mp4
./myplay -v quiet ../videos/fpx_r0.5.gif
./myplay -v quiet ../videos/fpx_r1.mp4
./myplay -v quiet ../videos/fpx_r1.gif
// if gpu, it's better to test on videos of which fps is high.
./myplay -v quiet ../videos/fpx.mp4
./myplay -v quiet ../videos/fpx.gif
```
