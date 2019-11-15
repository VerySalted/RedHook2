
cmake_minimum_required(VERSION 3.14)
project(RedHook2 LANGUAGES CXX)

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

# Currently there's only support for only shared
# If you want to implement this library statically, you would need to do some changes to the actual lib
option(BUILD_SHARED "Build shared version" ON)

set(REDHOOK_SOURCES
    source/dllmain.cpp
    source/export.cpp
    
	# core
	source/core/core.cpp
	source/core/logs.cpp

    # hooking
    source/hooking/command-hook.cpp
    source/hooking/detour-hook.cpp
    source/hooking/input-hook.cpp

    # invoker
    source/invoker/crossmapping.cpp
    source/invoker/invoker.cpp

	# logger
	source/logger/file-log.cpp
	source/logger/helpers.cpp
	source/logger/log.cpp
	source/logger/log-mgr.cpp

    # memory
    source/memory/helpers.cpp
    source/memory/memory-location.cpp
    source/memory/patternscan.cpp

    # rage
	source/rage/scrProgram.cpp
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
    $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/source/>
    $<INSTALL_INTERFACE:source>
)

target_link_libraries(${PROJECT_NAME} PUBLIC
	fmt::fmt
	minhook
)