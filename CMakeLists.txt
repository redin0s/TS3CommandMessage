# CMakeList.txt : Top-level CMake project file, do global configuration
# and include sub-projects here.
#
cmake_minimum_required (VERSION 3.14)

project ("HotkeyChatMessage" LANGUAGES CXX)

# Find includes in corresponding build directories
set(CMAKE_INCLUDE_CURRENT_DIR ON)

include_directories(
    "include"
    "src"
)

set (PLUGIN_SRC
    "src/plugin.hpp"
    "src/plugin.cpp"
)
source_group("plugin" FILES ${PLUGIN_SRC})

# Turn on the ability to create folders to organize projects (.vcproj)
# It creates "CMakePredefinedTargets" folder by default and adds CMake
# defined projects like INSTALL.vcproj and ZERO_CHECK.vcproj
set_property(GLOBAL PROPERTY USE_FOLDERS ON)

add_library (HotkeyChatMessage SHARED 
	${PLUGIN_SRC})

set_target_properties(HotkeyChatMessage PROPERTIES PREFIX "libchanmsg_plugin_")

if("${CMAKE_SIZEOF_VOID_P}" STREQUAL "4")
    message( FATAL_ERROR "Only supports 64bit")
else()
    if(UNIX AND NOT APPLE)
        set_target_properties(HotkeyChatMessage PROPERTIES OUTPUT_NAME "linux_amd64")
    elseif(WIN32)
        set_target_properties(HotkeyChatMessage PROPERTIES OUTPUT_NAME "win64")
    else()
        message ( FATAL_ERROR "Unsupported platform" )
    endif()
endif()
