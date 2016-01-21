CMAKE_MINIMUM_REQUIRED(VERSION 2.8.10)
PROJECT(OOGL)

SET(VERSION_MAJOR 0)
SET(VERSION_MINOR 2)
SET(VERSION_PATCH 0)

FIND_PACKAGE(JPEG REQUIRED)
FIND_PACKAGE(PNG REQUIRED)
FIND_PACKAGE(ZLIB REQUIRED)

# Supported options:
OPTION(BUILD_SAMPLES "Build the OOGL Samples." ON)

# This directory contains our header files and ipp files
INCLUDE_DIRECTORIES(${CMAKE_CURRENT_SOURCE_DIR}/src/include)

# Get all our source files
FILE(GLOB_RECURSE src "src/**.cpp" include/**.hpp)

# Include our include directory while compiling
INCLUDE_DIRECTORIES(${CMAKE_SOURCE_DIR}/include)
INCLUDE_DIRECTORIES(${JPEG_INCLUDE_DIRS})
INCLUDE_DIRECTORIES(${PNG_INCLUDE_DIRS})
INCLUDE_DIRECTORIES(${ZLIB_INCLUDE_DIRS})

ADD_LIBRARY(oogl ${src})
TARGET_LINK_LIBRARIES(oogl ${JPEG_LIBRARIES} ${PNG_LIBRARIES} ${ZLIB_LIBRARIES})

# If we're building the samples, make an executable for each one
IF(BUILD_SAMPLES)

    ADD_EXECUTABLE(ShadowMapping ${CMAKE_SOURCE_DIR}/ShadowMapping/main.cpp")
    TARGET_LINK_LIBRARIES(ShadowMapping oogl)

    ADD_EXECUTABLE(StencilReflection ${CMAKE_SOURCE_DIR}/StencilReflection/main.cpp")
    TARGET_LINK_LIBRARIES(StencilReflection oogl)

    ADD_EXECUTABLE(TransformFeedback ${CMAKE_SOURCE_DIR}/TransformFeedback/main.cpp")
    TARGET_LINK_LIBRARIES(TransformFeedback oogl)

    ADD_EXECUTABLE(Triangle ${CMAKE_SOURCE_DIR}/Triangle/main.cpp")
    TARGET_LINK_LIBRARIES(Triangle oogl)

ENDIF(BUILD_SAMPLES)

# Install the headers and templates that nuclear depends on
INSTALL(DIRECTORY ${CMAKE_CURRENT_SOURCE_DIR}/include/ DESTINATION include PATTERN "*.hpp")
