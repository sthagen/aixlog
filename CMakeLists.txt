#   __   __  _  _  __     __    ___ 
#  / _\ (  )( \/ )(  )   /  \  / __)
# /    \ )(  )  ( / (_/\(  O )( (_ \
# \_/\_/(__)(_/\_)\____/ \__/  \___/

# This file is part of aixlog
# Copyright (C) 2017-2021 Johannes Pohl

# This software may be modified and distributed under the terms
# of the MIT license.  See the LICENSE file for details.

cmake_minimum_required(VERSION 3.0.0)

project(aixlog VERSION 1.5.0 LANGUAGES CXX)
set(PROJECT_DESCRIPTION "Header-only C++ logging library")
set(PROJECT_URL "https://github.com/badaix/aixlog")

option(BUILD_EXAMPLE "Build example (build aixlog_example demo)" ON)

set(CMAKE_CXX_STANDARD 11)
set(CMAKE_CXX_EXTENSIONS OFF)


if(NOT DEFINED CMAKE_INSTALL_INCLUDEDIR)
    SET(CMAKE_INSTALL_INCLUDEDIR include CACHE
        PATH "Output directory for header files")
endif()


include_directories(
	"include"
)

if (BUILD_EXAMPLE)
	add_executable(aixlog_example aixlog_example.cpp)
	if (${CMAKE_SYSTEM_NAME} MATCHES "Android")
		target_link_libraries(aixlog_example log atomic)
	endif()
endif (BUILD_EXAMPLE)


install(FILES include/aixlog.hpp DESTINATION "${CMAKE_INSTALL_INCLUDEDIR}")

FIND_PROGRAM(CLANG_FORMAT "clang-format")
IF(CLANG_FORMAT)
	set(CHECK_CXX_SOURCE_FILES
	${CMAKE_SOURCE_DIR}/include/aixlog.hpp
	${CMAKE_SOURCE_DIR}/aixlog_example.cpp
	)

    ADD_CUSTOM_TARGET(
        reformat
        COMMAND
        ${CLANG_FORMAT}
        -i
        -style=file
        ${CHECK_CXX_SOURCE_FILES}
        COMMENT "Auto formatting of all source files"
    )
ENDIF()
