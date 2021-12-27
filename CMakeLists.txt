cmake_minimum_required(VERSION 3.21)

project(jsengine LANGUAGES CXX)

# We only care about 1 config type (Release)
set(CMAKE_CONFIGURATION_TYPES Release)

# We dont want NDEBUG... we want asserts
string(REPLACE "/DNDEBUG" "" CMAKE_CXX_FLAGS_RELEASE ${CMAKE_CXX_FLAGS_RELEASE})

# C++20 required
set(CMAKE_CXX_STANDARD 20)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

# Unity (Jumbo) Build
set(CMAKE_UNITY_BUILD TRUE)

# Global compiler flags
set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} /Zi")      # Program Database
set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} /MP")      # Multi-Processor compilation
set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} /W4")      # Level 4 warnings
set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} /WX")      # Warnings as errors
set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} /fp:fast") # Fast floating point

# Disable some warnings...
set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} /wd4100") # unreferenced formal parameter
set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} /wd4127") # conditional expression is constant
set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} /wd4189") # local variable is initialized but not referenced
set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} /wd4244") # conversion from 'T1' to 'T2', possible loss of data
set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} /wd4267") # conversion from 'T1' to 'T2', possible loss of data
set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} /wd4324") # structure was padded due to alignment specifier
set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} /wd4702") # unreachable code

# Global linker flags
set(CMAKE_EXE_LINKER_FLAGS "${CMAKE_EXE_LINKER_FLAGS} /DEBUG:FASTLINK") # Fast Linking

set(ROOT_DIR "${CMAKE_CURRENT_LIST_DIR}/JSEngine4.0")
set(SRC_DIR "${ROOT_DIR}/src")

# message(root: ${ROOT_DIR})
# message(src dir: ${SRC_DIR})

# all binaries for all config types (if any) go to the "bin" folder
foreach( OUTPUTCONFIG ${CMAKE_CONFIGURATION_TYPES} )
    string( TOUPPER ${OUTPUTCONFIG} OUTPUTCONFIG )
    set(CMAKE_ARCHIVE_OUTPUT_DIRECTORY_${OUTPUTCONFIG} "${ROOT_DIR}/bin")
    set(CMAKE_LIBRARY_OUTPUT_DIRECTORY_${OUTPUTCONFIG} "${ROOT_DIR}/bin")
    set(CMAKE_RUNTIME_OUTPUT_DIRECTORY_${OUTPUTCONFIG} "${ROOT_DIR}/bin")
endforeach()

# global libs
# link_directories(${ROOT_DIR})
# link_libraries(ws2_32)

# global include dirs
include_directories(${SRC_DIR})

# Gather src files
file(GLOB_RECURSE ENGINE_SRC "${SRC_DIR}/*.cpp" "${SRC_DIR}/*.h" "${SRC_DIR}/*.hpp")

# Main Engine Proj src files to compile
set(ALL_ENGINE_SRC ${ENGINE_SRC})

###########################################################################

# Main Engine Proj
add_executable(jsengine ${ALL_ENGINE_SRC})
# add_executable(jsengine WIN32 ${ALL_ENGINE_SRC}) # uncomment this line to create a non-console win32 app. remember to define WinMain

# Engine specific libs
# target_link_libraries(jsengine PUBLIC libdir)

# Engine specific include dirs
# target_include_directories(jsengine PUBLIC "${SRC_DIR}/include")
