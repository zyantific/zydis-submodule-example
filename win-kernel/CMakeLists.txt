cmake_minimum_required(VERSION "3.15")
project("zydisRing0")

list(APPEND CMAKE_MODULE_PATH "${CMAKE_CURRENT_LIST_DIR}/../FindWdk")
find_package(WDK REQUIRED)

option(ZYAN_NO_LIBC ON)
option(ZYDIS_FEATURE_FORMATTER "" ON)
option(ZYDIS_BUILD_TOOLS "" OFF)
option(ZYDIS_BUILD_EXAMPLES "" OFF)

add_subdirectory("deps/zydis")

# include_directories(${CMAKE_CURRENT_LIST_DIR})
# include_directories(zydis)
# include_directories(zydis/include)
# include_directories(zydis/src)

# include_directories(zydis/dependencies/zycore/include)

wdk_add_driver("ZydisDriver" "ZydisDriver.c")
