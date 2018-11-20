################################################################################
# Author: Joshua Shlemmer
# Copyright 2017, DigiPen Institute of Technology
################################################################################
################################################################################
# Set required version of cmake and any version specific policies
################################################################################
cmake_minimum_required(VERSION 3.9.1 FATAL_ERROR)
set_property(GLOBAL PROPERTY USE_FOLDERS ON)

set(CMAKE_CONFIGURATION_TYPES Debug Release RelWithDebInfo)

################################################################################
# Declare project(s) and set used languages
################################################################################
project(Testbed CXX)

#paths
set(cmake_include_dir ${CMAKE_CURRENT_SOURCE_DIR}/ZeroCMake)
set(cmake_utilities_dir ${cmake_include_dir}/Utilities)
set(cmake_os_dir ${cmake_include_dir}/OS)
set(cmake_flags_dir ${cmake_include_dir}/CommonFlags)
set(cmake_config_dir ${cmake_include_dir}/Configuration)
set(cmake_compiler_dir ${cmake_include_dir}/Compiler)


set(zero_core_path ${CMAKE_CURRENT_SOURCE_DIR})
message("CorePath: ${zero_core_path}\n")
set(zero_build_out ${zero_core_path}/BuildOutput)
set(supported_configs $<$<CONFIG:Debug>:Debug>$<$<CONFIG:Release>:Release>)
set(zero_intermediates_dir ${zero_build_out}/Int/${supported_configs})
set(zero_unit_test_dir ${zero_core_path}/UnitTests)

include(${cmake_include_dir}/Platform_Options.cmake)
include(${cmake_utilities_dir}/zero_source_group.cmake)
include(${cmake_utilities_dir}/Zero_Mulitarget_Functions.cmake)
include(${cmake_utilities_dir}/Zero_Custom_Commands.cmake)

################################################################################
# Define any user options
################################################################################

option(generate_spirv_projects "If set to 'ON', spirv tools projects will be created. Allows static linking or generation of dll for tools." ON)
option(use_spirv_shared_library "If set to 'ON', project will link against a dll of spirv (allows release dll in debug mode)." OFF)

if(NOT generate_spirv_projects AND NOT use_spirv_shared_library)
  message(FATAL_ERROR "spirv project cannot be statically linked without also generating the spirv projects.")
endif()

add_subdirectory(SPIRV-Cross/ZeroCMake)
if(generate_spirv_projects)
  add_subdirectory(SPIRV-Tools/ZeroCMake)
endif()

################################################################################
# Separate Projects into folders for IDEs.
################################################################################
set_property(GLOBAL PROPERTY USE_FOLDERS ON)

