

cmake_minimum_required(VERSION 3.14)
project(RedHookII LANGUAGES CXX)

set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED 17)

#######################
#    DEPENDENCIES     # 
#######################


# FMT library
add_subdirectory(vendor/fmt)

# MinHook
add_subdirectory(vendor/minhook)


################
#    BUILD     # 
################


option(BUILD_SHARED "Build shared version" OFF)

set(REDHOOK_SOURCES
    source/dllmain.cpp
    source/export.cpp
    
	# core
	source/core/core.cpp

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

add_definitions("/DRH2_EXPORTS")

if (BUILD_SHARED)
    add_library(${PROJECT_NAME} SHARED ${REDHOOK_SOURCES})
else()
    add_library(${PROJECT_NAME} STATIC ${REDHOOK_SOURCES})
endif()

target_include_directories(${PROJECT_NAME} PUBLIC 
    $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/source/>
    $<INSTALL_INTERFACE:source>
)

target_link_libraries(${PROJECT_NAME} PUBLIC
	fmt::fmt
	minhook
)

