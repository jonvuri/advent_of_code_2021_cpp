cmake_minimum_required(VERSION 3.16)

set(CMAKE_CXX_FLAGS_DEBUG "${CMAKE_CXX_FLAGS_DEBUG} -D_GLIBCXX_DEBUG")

find_package(fmt)
find_package(spdlog REQUIRED)
find_package(docopt REQUIRED)
find_package(doctest REQUIRED)

include_directories(${DOCOPT_INCLUDE_DIRS})
include(doctest)
include(CTest)

set (SOLVER_FILES
  01/part1/solver_01_part1.cpp
  01/part2/solver_01_part2.cpp
  02/part1/solver_02_part1.cpp
  02/part2/solver_02_part2.cpp
  03/part1/solver_03_part1.cpp
  03/part2/solver_03_part2.cpp
  04/part1/solver_04_part1.cpp
  04/part2/solver_04_part2.cpp
  05/part1/solver_05_part1.cpp
  05/part2/solver_05_part2.cpp
  06/part1/solver_06_part1.cpp
  06/part2/solver_06_part2.cpp
  07/part1/solver_07_part1.cpp
  07/part2/solver_07_part2.cpp
  08/part1/solver_08_part1.cpp
  08/part2/solver_08_part2.cpp
  09/part1/solver_09_part1.cpp
  09/part2/solver_09_part2.cpp
  10/part1/solver_10_part1.cpp
  10/part2/solver_10_part2.cpp
  11/part1/solver_11_part1.cpp
  11/part2/solver_11_part2.cpp
  12/part1/solver_12_part1.cpp
  12/part2/solver_12_part2.cpp
  13/part1/solver_13_part1.cpp
  13/part2/solver_13_part2.cpp
  14/part1/solver_14_part1.cpp
  14/part2/solver_14_part2.cpp
  15/part1/solver_15_part1.cpp
  15/part2/solver_15_part2.cpp
  16/part1/solver_16_part1.cpp
  16/part2/solver_16_part2.cpp
  17/part1/solver_17_part1.cpp
  17/part2/solver_17_part2.cpp
  18/part1/solver_18_part1.cpp
  18/part2/solver_18_part2.cpp
  19/part1/solver_19_part1.cpp
  19/part2/solver_19_part2.cpp
  20/part1/solver_20_part1.cpp
  20/part2/solver_20_part2.cpp
  21/part1/solver_21_part1.cpp
  21/part2/solver_21_part2.cpp
  22/part1/solver_22_part1.cpp
  22/part2/solver_22_part2.cpp
)

# Make main executable
add_executable(
  ${MAIN}
  main.cpp
  ${SOLVER_FILES})

target_link_libraries(
  ${MAIN}
  PRIVATE project_options
          project_warnings
          docopt::docopt
          doctest::doctest
          spdlog::spdlog)
          
set(CMAKE_CXX_INCLUDE_WHAT_YOU_USE include-what-you-use)

set_target_properties(${MAIN} PROPERTIES
  CXX_STANDARD 17
  CXX_STANDARD_REQUIRED YES
  CXX_EXTENSIONS NO
  CXX_INCLUDE_WHAT_YOU_USE include-what-you-use
)

target_compile_definitions(${MAIN} PRIVATE DOCTEST_CONFIG_DISABLE=1)

# Parameters
set(TESTFILES        # All .cpp files in tests/
    tests.cpp
    ${SOLVER_FILES}
)
set(TEST_MAIN unit_tests)
set(TEST_RUNNER_PARAMS "")

# Make unit test executable
add_executable(${TEST_MAIN} ${TESTFILES})
target_link_libraries(${TEST_MAIN} PRIVATE doctest::doctest spdlog::spdlog)
set_target_properties(${TEST_MAIN} PROPERTIES RUNTIME_OUTPUT_DIRECTORY ${PROJECT_BINARY_DIR})

set_target_properties(${TEST_MAIN} PROPERTIES
    CXX_STANDARD 17
    CXX_STANDARD_REQUIRED YES
    CXX_EXTENSIONS NO
    CXX_INCLUDE_WHAT_YOU_USE include-what-you-use
)

# Add tests to CTest
doctest_discover_tests(
  ${TEST_MAIN}
  TEST_PREFIX advent
  ADD_LABELS 1
)

# Enable colored compiler output
if ("${CMAKE_CXX_COMPILER_ID}" STREQUAL "GNU")
  add_compile_options (-fdiagnostics-color=always)
elseif ("${CMAKE_CXX_COMPILER_ID}" STREQUAL "Clang")
  add_compile_options (-fcolor-diagnostics)
endif ()

