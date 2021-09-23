#
# Copyright 2021, Unikie
#
# SPDX-License-Identifier: BSD-2-Clause
#

cmake_minimum_required(VERSION 3.8.2)

project(camkes-debug-tool C ASM)

find_package(camkes-tool REQUIRED)
find_package(global-components REQUIRED)
find_package(sel4_projects_libs REQUIRED)
camkes_tool_setup_camkes_build_environment()
sel4_projects_libs_import_libraries()
global_components_import_project()

# We need to preprocess our .camkes spec to insert the debug components
set(CAmkESCPP ON CACHE BOOL "" FORCE)

file(
    GLOB
        debugrouter_src
        ${CMAKE_CURRENT_SOURCE_DIR}/components/DebugRouter/src/*.c
)

file(
    GLOB
        debugrouter_inc
        ${CMAKE_CURRENT_SOURCE_DIR}/components/DebugRouter/include/*.h
)

# Declare the CAmkES DebugRouter component
DeclareCAmkESComponent(
    DebugRouter
    SOURCES
    ${debugrouter_src}
    INCLUDES
    ${debugrouter_inc}
    LIBS
    sel4camkes
    ethdrivers
    picotcp
    sel4vspace
)
