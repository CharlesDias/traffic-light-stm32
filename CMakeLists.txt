cmake_minimum_required(VERSION 3.20)

project(traffic-light-stm32f4 VERSION 0.1.0)

list(APPEND CMAKE_MODULE_PATH "${CMAKE_SOURCE_DIR}/cmake")

# Keep this options after project() command
enable_language(C ASM)

include(NoInSourceBuilds)

add_subdirectory(source bin)
add_subdirectory(test)

set(CMAKE_C_STANDARD 17)
set(CMAKE_C_STANDARD_REQUIRED ON)               # Define explicitly to use of the standard C11 declared in the previous line
set(CMAKE_C_EXTENSIONS OFF)                     # Disabling vendor-specific extensions

set(CMAKE_CXX_STANDARD 20)
set(CMAKE_CXX_STANDARD_REQUIRED ON)             # Define explicitly to use of the standard C++17 declared in the previous line
set(CMAKE_CXX_EXTENSIONS OFF)                   # Disabling vendor-specific extensions