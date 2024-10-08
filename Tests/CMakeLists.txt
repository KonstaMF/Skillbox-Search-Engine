cmake_minimum_required(VERSION 3.11)

# Include FetchContent for GoogleTest
include(FetchContent)
FetchContent_Declare(
    googletest
    URL https://github.com/google/googletest/archive/609281088cfefc76f9d0ce82e1ff6c30cc3591e5.zip
)
include_directories(${gtest_SOURCE_DIR}/include ${gtest_SOURCE_DIR})
set(gtest_force_shared_crt ON CACHE BOOL "" FORCE)
FetchContent_MakeAvailable(googletest)

# Set up the executable for tests
add_executable(Tests Tests.cpp Tests.h)

# Set the C++ standard if the CMake version is greater than 3.12
if (CMAKE_VERSION VERSION_GREATER 3.12)
  set_property(TARGET Tests PROPERTY CXX_STANDARD 20)
endif()

# Find Qt5 components
find_package(Qt5 COMPONENTS Widgets REQUIRED)

# Link Qt libraries to the test executable
target_link_libraries(Tests Project_library gtest_main Qt5::Widgets)

# Include GoogleTest for discovering tests
include(GoogleTest)
gtest_discover_tests(Tests)
