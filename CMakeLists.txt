cmake_minimum_required(VERSION 3.17)
project(tensorrt_onnx)

set(CMAKE_CXX_STANDARD 14)

set(TENSORRT_PATH  <your tensorrt dir>)

set(CMAKE_PREFIX_PATH "${TENSORRT_PATH}/lib")
include_directories(${TENSORRT_PATH}/include)
include_directories(${TENSORRT_PATH}/samples/common)


find_library(NVINFER NAMES libnvinfer.so)
find_library(NVPARSERS NAMES nvparsers)
find_library(NVONNXPARSERS NAMES nvonnxparser)
if(NVINFER)
    message("TensorRT is available!")
    message("NVINFER: ${NVINFER}")
    message("NVPARSERS: ${NVPARSERS}")
    message("NVONNXPARSERS: ${NVONNXPARSERS}")
    set(TRT_AVAIL ON)
else()
    message("TensorRT is NOT Available")
    set(TRT_AVAIL OFF)
endif()

find_package(CUDA REQUIRED)
if(${CUDA_FOUND})
    include_directories(${CUDA_INCLUDE_DIRS})
    MESSAGE(${CUDA_INCLUDE_DIRS})
    link_directories($ENV{CUDA_PATH}/lib/x64)
    MESSAGE($ENV{CUDA_PATH}/lib64)

else(${CUDA_FOUND})
    MESSAGE(STATUS "cuda not found!")
endif(${CUDA_FOUND})

SET(LOG_CPP ${TENSORRT_PATH}/samples/common/logger.cpp)

add_executable(tensorrt_onnx sampleOnnxMNIST.cpp ${LOG_CPP})
target_link_libraries(tensorrt_onnx
        ${NVINFER}
        ${NVONNXPARSERS}
        ${CUDA_LIBRARIES}
        )

