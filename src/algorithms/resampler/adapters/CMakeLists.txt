# Copyright (C) 2012-2019  (see AUTHORS file for a list of contributors)
#
# This file is part of GNSS-SDR.
#
# GNSS-SDR is free software: you can redistribute it and/or modify
# it under the terms of the GNU General Public License as published by
# the Free Software Foundation, either version 3 of the License, or
# (at your option) any later version.
#
# GNSS-SDR is distributed in the hope that it will be useful,
# but WITHOUT ANY WARRANTY; without even the implied warranty of
# MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the
# GNU General Public License for more details.
#
# You should have received a copy of the GNU General Public License
# along with GNSS-SDR. If not, see <https://www.gnu.org/licenses/>.
#


set(RESAMPLER_ADAPTER_SOURCES
    direct_resampler_conditioner.cc
    mmse_resampler_conditioner.cc
)

set(RESAMPLER_ADAPTER_HEADERS
    direct_resampler_conditioner.h
    mmse_resampler_conditioner.h
)

list(SORT RESAMPLER_ADAPTER_HEADERS)
list(SORT RESAMPLER_ADAPTER_SOURCES)

source_group(Headers FILES ${RESAMPLER_ADAPTER_HEADERS})

add_library(resampler_adapters
    ${RESAMPLER_ADAPTER_SOURCES}
    ${RESAMPLER_ADAPTER_HEADERS}
)

target_link_libraries(resampler_adapters
    PUBLIC
        Gnuradio::runtime
        resampler_gr_blocks
    PRIVATE
        Gflags::gflags
        Glog::glog
        Volk::volk
)

target_include_directories(resampler_adapters
    PUBLIC
        ${CMAKE_SOURCE_DIR}/src/core/interfaces
)

if(NOT (GNURADIO_VERSION VERSION_LESS "3.8"))
    target_compile_definitions(resampler_adapters PUBLIC -DGR_GREATER_38=1)
endif()

if(ENABLE_CLANG_TIDY)
    if(CLANG_TIDY_EXE)
        set_target_properties(resampler_adapters
            PROPERTIES
                CXX_CLANG_TIDY "${DO_CLANG_TIDY}"
        )
    endif()
endif()

set_property(TARGET resampler_adapters
    APPEND PROPERTY INTERFACE_INCLUDE_DIRECTORIES
        $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}>
)
