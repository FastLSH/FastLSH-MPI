cmake_minimum_required(VERSION 3.2)
project(FastLSH)

#import OpenMP
find_package(OpenMP)
if (OPENMP_FOUND)
    set (CMAKE_C_FLAGS "${CMAKE_C_FLAGS} ${OpenMP_C_FLAGS}")
    set (CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} ${OpenMP_CXX_FLAGS}")
endif()

#complie with openmp
SET(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -fopenmp")

find_package(MPI REQUIRED)
if (MPI_FOUND)
    include_directories(SYSTEM ${MPI_INCLUDE_PATH})
else (MPI_FOUND)
    message(SEND_ERROR "This application cannot compile without MPI")
endif (MPI_FOUND)


set(CMAKE_CXX_STANDARD 11)

set(BoostMPI_FILES src/boostmpiCompute.cpp)

#find boost library
find_package (Boost 1.55.0 REQUIRED COMPONENTS system filesystem mpi)
if(Boost_FOUND)
    include_directories(${Boost_INCLUDE_DIRS})
endif()

set(Boost_USE_STATIC_LIBS ON)
set(Boost_USE_MULTITHREADED ON)
set(Boost_USE_STATIC_RUNTIME ON)

add_executable(boostmpiLSH ${BoostMPI_FILES})

target_link_libraries(boostmpiLSH ${Boost_LIBRARIES} ${MPI_LIBRARIES})


