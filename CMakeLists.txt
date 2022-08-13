cmake_minimum_required (VERSION 3.8)

project (SpinePP)

set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED TRUE)

option(BUILD_TESTS "Whether build tests or not" ON)

if (BUILD_TESTS)
    message(STATUS "BUILD_TESTS is ON. Fetching GTest content...")

    if(NOT EXISTS "${CMAKE_BINARY_DIR}/conan.cmake")
      message(STATUS "Downloading conan.cmake from https://github.com/conan-io/cmake-conan")
      file(DOWNLOAD "https://raw.githubusercontent.com/conan-io/cmake-conan/0.18.1/conan.cmake"
                    "${CMAKE_BINARY_DIR}/conan.cmake"
                    TLS_VERIFY ON)
    endif()

    include(${CMAKE_BINARY_DIR}/conan.cmake)

    conan_cmake_configure(REQUIRES gtest/1.12.1
                          GENERATORS cmake_find_package cmake_paths)

    conan_cmake_autodetect(settings)

    conan_cmake_install(PATH_OR_REFERENCE .
                        BUILD missing
                        REMOTE conancenter
                        SETTINGS ${settings})
    
    include(${CMAKE_BINARY_DIR}/conan_paths.cmake)

    find_package(GTest REQUIRED)
endif()

add_subdirectory(src)

if (BUILD_TESTS)
    add_subdirectory(tests)
endif()