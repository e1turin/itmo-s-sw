cmake_minimum_required(VERSION 3.12)

project(tests)


add_executable(tests)
target_sources(tests
        PUBLIC
        ${CMAKE_SOURCE_DIR}/tests/tests.cpp
)
target_include_directories(tests
        PUBLIC
        ${CMAKE_SOURCE_DIR}/tests
)
target_link_libraries(tests producer-consumer)
