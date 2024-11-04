cmake_minimum_required(VERSION 3.14)
project(cleancode)

# GoogleTest requires at least C++14
set(CMAKE_CXX_STANDARD 14)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

include(FetchContent)
FetchContent_Declare(
  googletest
  URL https://github.com/google/googletest/archive/03597a01ee50ed33e9dfd640b249b4be3799d395.zip
)
# For Windows: Prevent overriding the parent project's compiler/linker settings
set(gtest_force_shared_crt ON CACHE BOOL "" FORCE)
FetchContent_MakeAvailable(googletest)

enable_testing()
add_compile_options("--coverage")

# Verify that the source files exist before adding them to the executable
set(SOURCE_FILES
    cooling_config.cpp
    breach_detector.cpp
    alert_checker.cpp
    alert_to_controller.cpp
    AlertInEmail.cpp
    typewise-alert.tests.cpp
)

# Check if each source file exists
foreach(SOURCE ${SOURCE_FILES})
  if(NOT EXISTS "${CMAKE_SOURCE_DIR}/${SOURCE}")
    message(FATAL_ERROR "Cannot find source file: ${SOURCE}")
  endif()
endforeach()

# Define the executable only if all sources are found
add_executable(cleancode ${SOURCE_FILES})

# Link libraries
target_link_libraries(
  cleancode
  GTest::gmock_main
  gcov
)

include(GoogleTest)
gtest_discover_tests(cleancode)
