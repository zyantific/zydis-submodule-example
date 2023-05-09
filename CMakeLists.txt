cmake_minimum_required(VERSION "3.15")

include(FetchContent)

project("MyProject")

# Register Zydis dependency.
FetchContent_Declare(
  Zydis
  GIT_REPOSITORY https://github.com/zyantific/zydis.git
  GIT_TAG        master
)
# Disable build of tools and examples.
set(ZYDIS_BUILD_TOOLS OFF CACHE BOOL "" FORCE)
set(ZYDIS_BUILD_EXAMPLES OFF CACHE BOOL "" FORCE)
# Make available
FetchContent_MakeAvailable(Zydis)

# Add our project executable
add_executable("MyProject" "myproject.c")

# Have CMake link our project executable against Zydis. ${PROJECT_NAME} it's our name on the fifth line
target_link_libraries(${PROJECT_NAME} PRIVATE "Zydis")
target_include_directories(${PROJECT_NAME} PRIVATE "Zydis")
