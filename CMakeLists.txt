cmake_minimum_required(VERSION 2.8)
set (CMAKE_CXX_STANDARD 14)
project(shape_based_matching)


# debug or release
SET(CMAKE_BUILD_TYPE "Release" "Debug")

#set(OpenCV_DIR D:/opencv/opencv-4.7.0/build/install)
# arm or x86
IF(${CMAKE_SYSTEM_PROCESSOR} MATCHES "arm")
    SET(PLATFORM_COMPILE_FLAGS "-mfpu=neon")
ELSE()
    SET(PLATFORM_COMPILE_FLAGS "-march=native")

    # some places of the algorithm are designed for 128 SIMD
    # so 128 SSE may slightly faster than 256 AVX, you may want this
    #SET(PLATFORM_COMPILE_FLAGS "-msse")  # SSE only -msse2 -msse3 -msse4 -mssse3
ENDIF()

SET(COMMON_COMPILE_FLAGS "-fopenmp") # -fno-strict-aliasing
SET(CMAKE_CXX_FLAGS "${PLATFORM_COMPILE_FLAGS} ${COMMON_COMPILE_FLAGS} $ENV{CXXFLAGS}")
SET(CMAKE_CXX_FLAGS_DEBUG "-O0 -g2 -ggdb")
SET(CMAKE_CXX_FLAGS_RELEASE "-O3")

# opencv
set(CMAKE_PREFIX_PATH ${CMAKE_PREFIX_PATH} /opt/ros/kinetic)
find_package(OpenCV REQUIRED)
include_directories(${OpenCV_INCLUDE_DIRS})

# include MIPP headers
include_directories (${INCLUDE_DIRECTORIES} "${CMAKE_CURRENT_SOURCE_DIR}/MIPP/")
# test exe
#add_executable(${PROJECT_NAME}_test line2Dup.cpp test.cpp)
#target_link_libraries(${PROJECT_NAME}_test ${OpenCV_LIBS})

# test exe
add_executable(${PROJECT_NAME}_test_fusion line2Dup.cpp test_fusion.cpp)
target_link_libraries(${PROJECT_NAME}_test_fusion ${OpenCV_LIBS})

