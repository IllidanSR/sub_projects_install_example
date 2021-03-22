cmake_minimum_required(VERSION 3.17)
project(subproject_test
        VERSION 0.1
        DESCRIPTION "controller"
        LANGUAGES CXX
        )
set(CMAKE_CXX_STANDARD 20)
add_subdirectory(src)
add_subdirectory(app)