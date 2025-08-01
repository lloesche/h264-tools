cmake_minimum_required(VERSION 3.10)
project(H264Decoder C)

# Get all C source files except decoder_test.c (which has its own main function)
file(GLOB LDECOD_LIB_SRCS "*.c")
list(REMOVE_ITEM LDECOD_LIB_SRCS "${CMAKE_CURRENT_SOURCE_DIR}/decoder_test.c")

# Set compiler flags
set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -std=gnu99 -pedantic -fno-strict-aliasing -fsigned-char -Wall")
set(CMAKE_C_FLAGS_RELEASE "${CMAKE_C_FLAGS_RELEASE} -O3 -march=native -mtune=native")
set(CMAKE_EXE_LINKER_FLAGS_RELEASE "${CMAKE_EXE_LINKER_FLAGS_RELEASE}")

# Check for OpenMP support (optional, since some systems might not have it)
find_package(OpenMP)
if(OpenMP_C_FOUND)
    set(CMAKE_C_FLAGS_RELEASE "${CMAKE_C_FLAGS_RELEASE} -fopenmp")
    set(CMAKE_EXE_LINKER_FLAGS_RELEASE "${CMAKE_EXE_LINKER_FLAGS_RELEASE} -fopenmp")
endif()

# Create the decoder library
add_library(h264decoder_lib STATIC ${LDECOD_LIB_SRCS})
target_link_libraries(h264decoder_lib m)
if(OpenMP_C_FOUND)
    target_link_libraries(h264decoder_lib OpenMP::OpenMP_C)
endif()

# Create the main decoder executable (ldecod)
add_executable(ldecod decoder_test.c)
target_link_libraries(ldecod h264decoder_lib)

# Install targets
install(TARGETS ldecod DESTINATION bin)
install(FILES decoder.cfg DESTINATION share)

