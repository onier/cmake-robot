
## [main]
cmake_minimum_required(VERSION 3.4...3.15)

project(RootDictExample LANGUAGES CXX)

set(CMAKE_CXX_STANDARD 11)
set(CMAKE_CXX_STANDARD_REQUIRED ON)
set(CMAKE_CXX_EXTENSIONS OFF)
set(CMAKE_PLATFORM_INDEPENDENT_CODE ON)

find_package(ROOT CONFIG REQUIRED)
include("${ROOT_DIR}/modules/RootNewMacros.cmake")

root_generate_dictionary(G__DictExample DictExample.h LINKDEF DictLinkDef.h)

add_library(DictExample SHARED DictExample.cxx DictExample.h G__DictExample.cxx)
target_include_directories(DictExample PUBLIC "${CMAKE_CURRENT_SOURCE_DIR}")
target_link_libraries(DictExample PUBLIC ROOT::Core)

## [main]

message(STATUS "Found ROOT: ${ROOT_VERSION} at ${ROOT_DIR}")
message(STATUS "Original flags: ${ROOT_CXX_FLAGS}")
message(STATUS "Original definitions: ${ROOT_DEFINITIONS}")
message(STATUS "Original exe flags: ${ROOT_EXE_LINKER_FLAGS}")

enable_testing()
add_test(NAME RootDictExample
    WORKING_DIRECTORY "${CMAKE_CURRENT_BINARY_DIR}"
    COMMAND "${ROOT_root_CMD}" -b -l -q "${CMAKE_CURRENT_SOURCE_DIR}/CheckLoad.C")
