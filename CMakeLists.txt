cmake_minimum_required(VERSION 3.25 FATAL_ERROR)

project(
  "cannyEdge"
  VERSION 1.0.0
  DESCRIPTION "Our lucky algorithm to sell to the government"
  LANGUAGES CXX
)

# Find GDAL
find_package(GDAL REQUIRED)
include_directories(${GDAL_INCLUDE_DIRS})

# Find OpenCV
find_package(OpenCV REQUIRED)
include_directories(${OpenCV_INCLUDE_DIRS})

set(CMAKE_CXX_STANDARD 20)
set(CMAKE_CXX_STANDARD_REQUIRED ON)
set(CMAKE_EXPORT_COMPILE_COMMANDS ON) # for clangd lsp

# Search for GDAL
find_package(GDAL)
if(NOT GDAL_FOUND)
    message(FATAL_ERROR "Failed to find GDAL. Please install it manually. See INSTALL.md")
endif()

# Search for OpenCV
find_package(OpenCV)
if(NOT OpenCV_FOUND)
    message(FATAL_ERROR "Failed to find OpenCV. Please install OpenCV. See INSTALL.md")
endif()

include_directories(include)

file(GLOB_RECURSE SOURCES "src/*.cpp")

# Add library
add_library(${PROJECT_NAME} SHARED ${SOURCES})

# Add executable
#add_executable(${PROJECT_NAME} ${SOURCES})

# Link libraries
target_link_libraries(${PROJECT_NAME} ${GDAL_LIBRARIES} ${OpenCV_LIBS})

# Add subdirectory of tests
if(RUN_TESTS EQUAL 1)
  add_subdirectory(tests)
endif()

