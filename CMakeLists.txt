cmake_minimum_required(VERSION 3.10)

set(CMAKE_TOOLCHAIN_FILE "/tmp/vcpkg/scripts/buildsystems/vcpkg.cmake" CACHE STRING "Vcpkg toolchain file")

list(APPEND CMAKE_PREFIX_PATH "/tmp/vcpkg/installed/x64-linux")

project(RestfulApi)

set(CMAKE_CXX_STANDARD 17)

find_package(Boost REQUIRED COMPONENTS system filesystem)
find_package(nlohmann_json CONFIG REQUIRED)

include_directories(${Boost_INCLUDE_DIRS})

add_executable(RestfulApi main.cpp)

target_link_libraries(RestfulApi 
    PRIVATE 
    ${Boost_LIBRARIES}
    nlohmann_json::nlohmann_json
)

message(STATUS "CMAKE_PREFIX_PATH: ${CMAKE_PREFIX_PATH}")
message(STATUS "nlohmann_json_DIR: ${nlohmann_json_DIR}")