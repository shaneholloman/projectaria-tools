# Copyright (c) Meta Platforms, Inc. and affiliates.
#
# Licensed under the Apache License, Version 2.0 (the "License");
# you may not use this file except in compliance with the License.
# You may obtain a copy of the License at
#
#     http://www.apache.org/licenses/LICENSE-2.0
#
# Unless required by applicable law or agreed to in writing, software
# distributed under the License is distributed on an "AS IS" BASIS,
# WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
# See the License for the specific language governing permissions and
# limitations under the License.


include(FetchContent)
FetchContent_Declare(
    pybind11
    GIT_REPOSITORY https://github.com/pybind/pybind11.git
    GIT_TAG master
)
# We are assuming here that python has been found by CMake already
# so it avoid to make pybind11/CMake & CIBUILDWHEEL to use different python versions
set(PYBIND11_FINDPYTHON OFF)
FetchContent_MakeAvailable(pybind11)

add_subdirectory(${pybind11_SOURCE_DIR} ${CMAKE_CURRENT_BINARY_DIR}/pybind)
pybind11_add_module(_core_pybinds ${CMAKE_CURRENT_SOURCE_DIR}/bindings.cpp)
target_link_libraries(_core_pybinds PUBLIC mps vrs_data_provider image_debayer image_devignetting image_color_correct vrs_health_check)
