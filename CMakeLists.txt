cmake_minimum_required(VERSION 3.13.4)
project(irparse)

# Set LLVM hint based on your LLVM installation location
# set(LLVM_DIR "C:/Users/songdogwang/Codes/llvm-project/build/lib/cmake/llvm")
# You can also use `-DLLVM_DIR="C:/Users/songdogwang/Codes/llvm-project/build/lib/cmake/llvm"` instead in the cmake command line

find_package(LLVM REQUIRED CONFIG)

message(STATUS "Found LLVM ${LLVM_PACKAGE_VERSION}")
message(STATUS "Using LLVMConfig.cmake in: ${LLVM_DIR}")

# Set your project compile flags.
# E.g. if using the C++ header files
# you will need to enable C++11 support
# for your compiler.

include_directories(${LLVM_INCLUDE_DIRS})
separate_arguments(LLVM_DEFINITIONS_LIST NATIVE_COMMAND ${LLVM_DEFINITIONS})
add_definitions(${LLVM_DEFINITIONS_LIST})

# Now build our tools
add_executable(dependgraph dependgraph.cpp)

add_executable(dependcheck dependencyCheck.cpp)

# Find the libraries that correspond to the LLVM components
# that we wish to use
llvm_map_components_to_libnames(llvm_libs support core irreader analysis)

# Link against LLVM libraries
target_link_libraries(dependgraph ${llvm_libs})
target_link_libraries(dependcheck ${llvm_libs})