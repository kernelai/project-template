cmake_minimum_required(VERSION 3.14)
project(project-template VERSION 0.1.0)
set(CMAKE_CXX_STANDARD 11)

# Update submodules as needed
find_package(Git QUIET)
if(GIT_FOUND AND EXISTS "${PROJECT_SOURCE_DIR}/.git")
    option(GIT_SUBMODULE "Check submodules during build" ON)
    if(GIT_SUBMODULE)
        message(STATUS "Submodule update")
        execute_process(COMMAND ${GIT_EXECUTABLE} submodule update --init --recursive
                        WORKING_DIRECTORY ${CMAKE_CURRENT_SOURCE_DIR}
                        RESULT_VARIABLE GIT_SUBMOD_RESULT)
        if(NOT GIT_SUBMOD_RESULT EQUAL "0")
            message(FATAL_ERROR "git submodule update --init --recursive failed with ${GIT_SUBMOD_RESULT}, please checkout submodules")
        endif()
    endif()
endif()

# unit test
option(ENABLE_UNIT_TESTS "Enable unit tests" ON)
message(STATUS "Enable testing: ${ENABLE_UNIT_TESTS}")
if(ENABLE_UNIT_TESTS)
  add_subdirectory(third/googletest)
  enable_testing()
  
  add_executable(
    main_test
    main_test.cpp
  )
  target_link_libraries(
    main_test
    gtest_main
  )
  
  include(GoogleTest)
  gtest_discover_tests(main_test)
endif()

# executable target
add_executable(project main.cpp hello.h hello.cpp)

# benchmark
add_subdirectory(third/benchmark)
add_executable(hello_bm hello_bm.cpp hello.h hello.cpp)
target_link_libraries(hello_bm benchmark::benchmark )

# package
set(CPACK_PROJECT_NAME ${PROJECT_NAME})
set(CPACK_PROJECT_VERSION ${PROJECT_VERSION})
include(CPack)
