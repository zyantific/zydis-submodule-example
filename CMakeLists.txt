cmake_minimum_required(VERSION "3.15")
project("MyProject")

# Register Zydis dependency
add_subdirectory("deps/zydis")

# Add our project executable
add_executable("MyProject" "myproject.c")

# Have CMake link our project executable against Zydis.
target_link_libraries("MyProject" PRIVATE "Zydis")
