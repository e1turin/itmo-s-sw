cmake_minimum_required(VERSION 3.12)

project(itmo-sys-sw
        VERSION 0.0.0
        DESCRIPTION "ITMO System Software course lab"
        LANGUAGES CXX
)

set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED True)

set(CXX_COMPILE_FLAGS "-g -O3 -Werror -Wall -Wextra -pthread -pedantic")
set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} ${CXX_COMPILE_FLAGS}")

add_library(producer-consumer)
target_sources(producer-consumer
        PUBLIC
        ${CMAKE_SOURCE_DIR}/producer_consumer.cpp
)
target_include_directories(producer-consumer
        PUBLIC
        ${CMAKE_SOURCE_DIR}
)

add_executable(posix)
target_sources(posix
    PUBLIC
        ${CMAKE_SOURCE_DIR}/main.cpp
)
target_include_directories(posix
        PUBLIC
        ${CMAKE_SOURCE_DIR}
)
target_link_libraries(posix
        producer-consumer
)

add_subdirectory(tests)

add_custom_target(clang_format
        COMMAND "diff -u <(cat *.cpp *.h tests/*.cpp) <(clang-format *.cpp *.h tests/*.cpp)"
        WORKING_DIRECTORY ${CMAKE_SOURCE_DIR}
)

add_custom_target(clang_format_fix
        COMMAND "clang-format -i *.cpp *.h tests/*.cpp"
        WORKING_DIRECTORY ${CMAKE_SOURCE_DIR}
)

add_custom_target(clang_tidy
        COMMAND "clang-tidy *.cpp tests/*.cpp -checks=-*,clang-analyzer-*,-clang-analyzer-cplusplus* -- -I tests -I . -std=c++17 ${CXX_COMPILE_FLAGS}"
        WORKING_DIRECTORY ${CMAKE_SOURCE_DIR}
)

add_custom_target(make_clean
        COMMAND "rm -f posix"
        COMMAND "rm -f tests/tests"
        WORKING_DIRECTORY ${CMAKE_SOURCE_DIR}
)
