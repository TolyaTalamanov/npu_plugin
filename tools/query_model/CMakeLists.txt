#
# Copyright (C) 2022 Intel Corporation
# SPDX-License-Identifier: Apache 2.0
#

set(TARGET_NAME query_model)

find_package(gflags QUIET)

add_tool_target(
    NAME ${TARGET_NAME}
    ROOT ${CMAKE_CURRENT_SOURCE_DIR}
    DEPENDENCIES
        openvino_intel_vpux_plugin
    LINK_LIBRARIES
        gflags
        IE::inference_engine_transformations
        openvino::runtime
    )

# Additional linkage required: https://en.cppreference.com/w/cpp/filesystem#:~:text=GNU%20implementation%20prior,lstdc%2B%2Bfs
if ((${CMAKE_CXX_COMPILER_ID} STREQUAL GNU) AND (CMAKE_CXX_COMPILER_VERSION VERSION_LESS 9.1))
    add_compile_definitions(GNU_LESS_9_1)
    target_link_libraries(${TARGET_NAME} PRIVATE stdc++fs)
endif()
