cmake_minimum_required(VERSION 3.14)
project(project-template VERSION 0.1.0)
set(CMAKE_CXX_STANDARD 11)

# unit test
add_subdirectory(third-party/googletest)
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

# executable target
add_executable(project-template main.cpp)

# package
set(CPACK_PROJECT_NAME ${PROJECT_NAME})
set(CPACK_PROJECT_VERSION ${PROJECT_VERSION})
include(CPack)
