## This project was created using these steps

#### Create CMakeLists.txt and myproject.c

CMakeLists.txt
```cmake
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
```

myproject.c
```c
#include <stdio.h>
#include <inttypes.h>
#include <Zydis/Zydis.h>

int main()
{
    ZyanU8 data[] =
    {
        0x51, 0x8D, 0x45, 0xFF, 0x50, 0xFF, 0x75, 0x0C, 0xFF, 0x75,
        0x08, 0xFF, 0x15, 0xA0, 0xA5, 0x48, 0x76, 0x85, 0xC0, 0x0F,
        0x88, 0xFC, 0xDA, 0x02, 0x00
    };

    // Initialize decoder context
    ZydisDecoder decoder;
    ZydisDecoderInit(&decoder, ZYDIS_MACHINE_MODE_LONG_64, ZYDIS_STACK_WIDTH_64);

    // Initialize formatter. Only required when you actually plan to do instruction
    // formatting ("disassembling"), like we do here
    ZydisFormatter formatter;
    ZydisFormatterInit(&formatter, ZYDIS_FORMATTER_STYLE_INTEL);

    // Loop over the instructions in our buffer.
    // The runtime-address (instruction pointer) is chosen arbitrary here in order to better
    // visualize relative addressing
    ZyanU64 runtime_address = 0x007FFFFFFF400000;
    ZyanUSize offset = 0;
    const ZyanUSize length = sizeof(data);
    ZydisDecodedInstruction instruction;
    ZydisDecodedOperand operands[ZYDIS_MAX_OPERAND_COUNT];
    while (ZYAN_SUCCESS(ZydisDecoderDecodeFull(&decoder, data + offset, length - offset,
        &instruction, operands)))
    {
        // Print current instruction pointer.
        printf("%016" PRIX64 "  ", runtime_address);

        // Format & print the binary instruction structure to human-readable format
        char buffer[256];
        ZydisFormatterFormatInstruction(&formatter, &instruction, operands,
            instruction.operand_count_visible, buffer, sizeof(buffer), runtime_address, ZYAN_NULL);
        puts(buffer);

        offset += instruction.length;
        runtime_address += instruction.length;
    }

    return 0;
}
```


## Running the example (Unix based OS)

```shell
mkdir build
cd build
cmake ..
make
./MyProject
```
## Running the example (Windows based OS)

```shell
mkdir bld
cd bld
cmake ..
Now we can open the .sln file with our project and linked Zydis
```

