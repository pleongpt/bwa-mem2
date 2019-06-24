cmake_minimum_required(VERSION 3.14)
project(bwa_mem2)

set(CMAKE_CXX_STANDARD 14)

include_directories(.)

set(THREADS_PREFER_PTHREAD_FLAG ON)
find_package(Threads REQUIRED)

SET (ARCH_FLAGS "-msse4.1")
SET (SWA_FLAGS "-DDEB=0 -DRDT=0 -DMAXI=0 -DNEW=1 -DSORT_PAIRS=0")
SET (MEM_FLAGS "-DPAIRED_END=1 -DMAINY=0 -DSAIS=1")
SET (CPPFLAGS "-DENABLE_PREFETCH ${MEM_FLAGS} ${SWA_FLAGS}")

SET(GCC_COVERAGE_COMPILE_FLAGS "-g -O3 -fpermissive ${ARCH_FLAGS} ${CPPFLAGS}")
SET(CMAKE_CXX_FLAGS  "${CMAKE_CXX_FLAGS} ${GCC_COVERAGE_COMPILE_FLAGS}")

SET (PROJECT_ROOT "${PROJECT_SOURCE_DIR}")
SET (PROJECT_SOURCE_DIR "${PROJECT_SOURCE_DIR}/src")
SET (PROJECT_BINARY_DIR "${PROJECT_BINARY_DIR}/bin")

include_directories("${PROJECT_SOURCE_DIR}")

add_executable(bwa_mem2
        ${PROJECT_SOURCE_DIR}/fastmap.cpp
        ${PROJECT_SOURCE_DIR}/bwtindex.cpp
        ${PROJECT_SOURCE_DIR}/main.cpp
        ${PROJECT_SOURCE_DIR}/utils.cpp
        ${PROJECT_SOURCE_DIR}/kthread.cpp
        ${PROJECT_SOURCE_DIR}/kstring.cpp
        ${PROJECT_SOURCE_DIR}/ksw.cpp
        ${PROJECT_SOURCE_DIR}/bntseq.cpp
        ${PROJECT_SOURCE_DIR}/bwamem.cpp
        ${PROJECT_SOURCE_DIR}/profiling.cpp
        ${PROJECT_SOURCE_DIR}/bandedSWA.cpp
        ${PROJECT_SOURCE_DIR}/FMI_search.cpp
        ${PROJECT_SOURCE_DIR}/read_index_ele.cpp
        ${PROJECT_SOURCE_DIR}/bwamem_pair.cpp
        ${PROJECT_SOURCE_DIR}/kswv.cpp
        ${PROJECT_SOURCE_DIR}/bwa.cpp
        ${PROJECT_SOURCE_DIR}/bwamem_extra.cpp
        ${PROJECT_SOURCE_DIR}/bwtbuild.cpp)

if (${CMAKE_SYSTEM_NAME} MATCHES "Linux")
    target_link_libraries(bwa_mem2 Threads::Threads z m rt)
else()
    target_link_libraries(bwa_mem2 ZLIB::ZLIB Threads::Threads m)
endif()
