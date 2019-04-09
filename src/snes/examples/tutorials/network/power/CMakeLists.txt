cmake_minimum_required (VERSION 2.6.3)
project (POWER)

list (APPEND CMAKE_MODULE_PATH "${CMAKE_SOURCE_DIR}/cmake-modules")

find_package (PETSc REQUIRED)
find_package(MPI REQUIRED)

message (STATUS "PETSC COMPILER ${PETSC_COMPILER}")

add_definitions (-std=c++0x)
message (STATUS "CPP COMPILER ${PETSC_COMPILER}")

include_directories ("${CMAKE_SOURCE_DIR}" ${PETSC_INCLUDES})
add_definitions (${PETSC_DEFINITIONS})
add_definitions (-g)
message (STATUS "PETSC_DEFINITIONS ${PETSC_DEFINITIONS}")

set(INCLUDE_FILES power.h)

set(SOURCE_FILES
        pffunctions.c
        PFReadData.c
        power.c)

add_executable(power ${INCLUDE_FILES} ${SOURCE_FILES})
target_include_directories(power PRIVATE ${MPI_INCLUDE_PATH})
target_link_libraries(power ${PETSC_LIBRARIES})
target_link_libraries(power m)
target_link_libraries(power ${MPI_LIBRARIES})

add_library(powersl SHARED ${INCLUDE_FILES} ${SOURCE_FILES})
target_include_directories(powersl PRIVATE ${MPI_INCLUDE_PATH})
target_link_libraries(powersl ${PETSC_LIBRARIES})
target_link_libraries(powersl m)
target_link_libraries(powersl ${MPI_LIBRARIES})
set_target_properties(powersl PROPERTIES SOVERSION 1)
