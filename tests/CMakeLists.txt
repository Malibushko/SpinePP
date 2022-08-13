enable_testing()

add_executable(SpinePPTest main.cpp)

target_link_libraries(SpinePPTest GTest::gtest_main)

include(GoogleTest)
gtest_discover_tests(SpinePPTest)