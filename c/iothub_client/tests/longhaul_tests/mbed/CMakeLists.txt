#Copyright (c) Microsoft. All rights reserved.
#Licensed under the MIT license. See LICENSE file in the project root for full license information.

cmake_minimum_required(VERSION 3.0)
project(mbed_iothubclientlonghaultests)

### Data
set(target_project_name "iothubclientlonghaultests")
set(sdk_base_path ../../../../..)
set(mbed_repo_name "iothubclientlonghaultests_bld")
set(local_repo_path ${CMAKE_CURRENT_BINARY_DIR}/${mbed_repo_name})
set(compile_mbed_tool ${sdk_base_path}/c/tools/compilembed/bin/release/compilembed.exe)
set(mbed_platform "FRDM-K64F")
set(mbed_output_bin_path "./iothubclientlonghaultests.bin")

set(project_files
		${CMAKE_CURRENT_SOURCE_DIR}/main.cpp
		${CMAKE_CURRENT_SOURCE_DIR}/*.c
		${CMAKE_CURRENT_SOURCE_DIR}/*.h
		${CMAKE_CURRENT_SOURCE_DIR}/../*.cpp 
	)

set(common_files 
		${sdk_base_path}/c/common/src/*.c
		${sdk_base_path}/c/common/src/*.h
		${sdk_base_path}/c/common/adapters/*mbed.c
		${sdk_base_path}/c/common/adapters/*mbed.cpp
		${sdk_base_path}/c/common/adapters/wolfssl_connection.*
		${sdk_base_path}/c/iothub_client/src/*.c
		${sdk_base_path}/c/iothub_client/inc/*.h
		${sdk_base_path}/c/testtools/testrunnerswitcher/inc/*.h
		${sdk_base_path}/c/testtools/micromock/inc/*.h
		${sdk_base_path}/c/testtools/micromock/src/*.c*
		${sdk_base_path}/c/testtools/sal/inc/*.h
		${sdk_base_path}/c/testtools/ctest/inc/*.h
		${sdk_base_path}/c/testtools/ctest/src/*.c
		${sdk_base_path}/c/common/testtools/iothub_test/inc/*.h
		${sdk_base_path}/c/common/testtools/iothub_test/src/*.c
	)
	
# Add the paterns of the filenames to be deleted before pushing local update to remote repo.
set(exclude_files
		agenttime.c
		*http*.* 
		*mqtt*.* 
		detours*.*
		*_2*.*
		*wolfssl*
	)

### Include CMAKE library for MBED.
if (NOT DEFINED MBED_CMAKE_LIB)
	include(${sdk_base_path}/c/tools/mbed_build/CMakeLists.txt)
endif()

### Main
if(WIN32)
	verifyRequiredParameters()
	verifyRequiredTools()
	cloneMBEDRepository()
	updateMBEDSourceFiles()
	pushMBEDRepository()
	compileMBEDProject()
else()
	message(FATAL_ERROR "Apologies, but the only currently supported platform is Microsoft Windows.")
endif()

