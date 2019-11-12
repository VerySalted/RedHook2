

cmake_minimum_required(VERSION 3.14)
project(RedHook LANGUAGES CXX)


################
#    BUILD     # 
################


option(BUILD_SHARED "Build shared version" OFF)

set(REDHOOK_SOURCES
    source/dllmain.cpp
    source/export.cpp
    
    # hooking
    source/hooking/command-hook.cpp
    source/hooking/detour-hook.cpp
    source/hooking/input-hook.cpp

    # invoker
    source/invoker/crossmapping.cpp
    source/invoker/invoker.cpp

    # memory
    source/memory/helpers.cpp
    source/memory/memory-location.cpp
    source/memory/patternscan.cpp

    # rage
    source/rage/scrThread.cpp

    # scripting
    source/scripting/script.cpp

    # util
    source/util/fiber.cpp
)

if (BUILD_SHARED)
    add_library(${PROJECT_NAME} SHARED ${REDHOOK_SOURCES})
else()
    add_library(${PROJECT_NAME} STATIC ${REDHOOK_SOURCES})
endif()

target_include_directories(${PROJECT_NAME} PUBLIC 
    $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/include/>
    $<INSTALL_INTERFACE:include>
)

