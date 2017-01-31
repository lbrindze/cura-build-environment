project(cura-build-environment)

cmake_minimum_required(VERSION 2.8.12)

list(APPEND CMAKE_MODULE_PATH ${CMAKE_SOURCE_DIR}/cmake)

include(ExternalProject)
include(GNUInstallDirs)

include(BuildPlatformDetection)
include(SetProjectDependencies)

option(INCLUDE_DEVEL "Include development tools" OFF)

set(PYTHON_EXECUTABLE ${CMAKE_INSTALL_PREFIX}/bin/python3)

# Build projects step
add_custom_target(projects ALL COMMENT "Building Projects...")

file(GLOB _projects projects/*.cmake)
foreach(_project ${_projects})
    include(${_project})
endforeach()

if(INCLUDE_DEVEL)
    file(GLOB _devel_projects devel/*.cmake)
    foreach(_devel_project ${_devel_projects})
        include(${_devel_project})
    endforeach()
endif()

ProcessProjectDependencies(TARGET projects)
