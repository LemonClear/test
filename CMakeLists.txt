cmake_minimum_required (VERSION 2.6)
project (test)

# 1.The Version Number.
set (test_VERSION_MAJOR 1)
set (test_VERSION_MINOR 0)

# 2.Debug Options.
set(CMAKE_BUILE_TYPE DEBUG)
set(CMAKE_C_FLAGS_DEBUG "-g -Wall")
set(CMAKE_CXX_FLAGS_DEBUG "-g -Wall")

# 3.Directories.
set (EXECUTABLE_OUTPUT_PATH ${PROJECT_BINARY_DIR}/bin)
set (LIBRARY_OUTPUT_PATH ${PROJECT_BINARY_DIR}/lib)

# 4.Linkages.
link_directories ("${PROJECT_BINARY_DIR}/lib")

# 5.Inlcudes header files
include_directories ("${PROJECT_BINARY_DIR}")

# 6.Configure a header file to pass some of the CMake settings to the source code
configure_file (
        "${PROJECT_SOURCE_DIR}/include/testConfig.h.in"
        "${PROJECT_BINARY_DIR}/include/testConfig.h"
)

# 7.Key Macros definitions shared between cmake && source code
option (USE_MYMATH
        "Use Math Self Iplementation." ON)
option (USE_DEBUGGER
        "Open Debugger." ON)

# 8. Add Sub-Directories
##MathFunctions
if (USE_MYMATH)
        include_directories ("${PROJECT_SOURCE_DIR}/MathFunctions")
        add_subdirectory (MathFunctions)
        set (EXTRA_LIBS ${EXTRA_LIBS} MathFunctions)
endif (USE_MYMATH)
##Debugger
if (USE_DEBUGGER)
        include_directories ("${PROJECT_SOURCE_DIR}/Debugger")
        add_subdirectory (Debugger)
        set (EXTRA_LIBS ${EXTRA_LIBS} Debugger)
endif (USE_DEBUGGER)

# 9.Check system Functions Provides
include (CheckFunctionExists)
check_function_exists (log HAVE_LOG)
check_function_exists (exp HAVE_EXP)

# 10.Compose Executable
add_executable(testA test.c)
target_link_libraries (testA ${EXTRA_LIBS})

# 11.Add the make install targets
install (TARGETS testA DESTINATION bin)
install (FILES "${PROJECT_BINARY_DIR}/testConfig.h" DESTINATION include)

# 12.Build a CPack driven installer package
include (InstallRequiredSystemLibraries)
set (CPACK_RESOURCE_FILE_LICENSE "${PROJECT_SOURCE_DIR}/LICENSE")
set (CPACK_PACKAGE_VERSION_MAJOR "${test_VERSION_MAJOR}")
set (CPACK_PACKAGE_VERSION_MINOR "${test_VERSION_MINOR}")
include (CPack)
