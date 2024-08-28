cmake_minimum_required(VERSION 3.12)

### Hunter snapshot that will be used ###
include("cmake/HunterGate.cmake")
HunterGate(
        URL "https://github.com/cpp-pm/hunter/archive/v0.25.6.tar.gz"
        SHA1 "69e4a05bd514bb05cb2c5aa02ce8ac420516aaf7"
)

cmake_policy(SET CMP0144 NEW)

project(HunterSimple)

### Download dependencies ###
hunter_add_package(GTest)
### Find dependencies ###
find_package(GTest CONFIG REQUIRED) # GTest::gtest_main


### Targets ###
add_executable(simple simple.cpp)
target_link_libraries(simple PUBLIC  GTest::gtest_main)

### Testing ###
enable_testing()

if(IOS)
  # Extra cmake modules needed for iOS testing:
  # * https://github.com/ruslo/sugar/tree/master/cmake/core#sugar_add_ios_gtest
  add_test(
      NAME SimpleTest COMMAND "${CMAKE_COMMAND}" -E echo "iOS testing ignored"
  )
elseif(ANDROID)
  # Building .apk, testing: https://github.com/forexample/android-cmake
  add_test(
      NAME SimpleTest
      COMMAND "${CMAKE_COMMAND}" -E echo "Android testing ignored"
  )
else()
  add_test(NAME SimpleTest COMMAND simple)
endif()
