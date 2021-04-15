#
# Project definition
#
project("opentreams-toolkits" LANGUAGES NONE)
cmake_minimum_required(VERSION 3.13)

#
# Module path
#
set(CMAKE_MODULE_PATH ${CMAKE_SOURCE_DIR}/cmake ${CMAKE_MODULE_PATH})

#
# Options
#
set(DOCKER_REGISTRY "docker.io" CACHE STRING "Docker registry") 
set(DOCKER_NAMESPACE "openstreams" CACHE STRING "Docker namespace") 
set(CMAKE_BUILD_TYPE "Debug" CACHE STRING "Build type") 

message(STATUS "Registry: ${DOCKER_REGISTRY}")
message(STATUS "Namespace: ${DOCKER_NAMESPACE}")
message(STATUS "Build: ${CMAKE_BUILD_TYPE}")

#
# Make sure docker is present
#
find_package(Docker REQUIRED)

#
# Subdirectories
#
add_subdirectory(toolkit)
