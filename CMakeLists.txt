cmake_minimum_required(VERSION 3.1)
project(SofaRenderingGL VERSION 1.0)

find_package(SofaOpenglVisual REQUIRED)

set(HEADER_FILES
        src/SofaOpenglVisual2/config.h
        src/SofaOpenglVisual2/PointSplatRenderer.h
	)

set(SOURCE_FILES
        initSofaRenderingGL.cpp
        src/SofaOpenglVisual2/PointSplatRenderer.cpp
	)

set(README_FILE
	Readme.md
	)

# Create the plugin library.
add_library(${PROJECT_NAME} SHARED ${HEADER_FILES} ${SOURCE_FILES} ${README_FILE})

# Link the plugin library to its dependencies (other libraries).
target_link_libraries(${PROJECT_NAME} SofaOpenglVisual)

# If using the build of this plugin with "find_package(implicitsurfaces)",
# then discover includes from '${CMAKE_CURRENT_SOURCE_DIR}/src' directory.
target_include_directories(${PROJECT_NAME} PUBLIC "$<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/src>")

# If using the install of this plugin with "find_package(implicitsurfaces)",
# then discover includes from 'include' directory.
target_include_directories(${PROJECT_NAME} PUBLIC "$<INSTALL_INTERFACE:include>")

# Create package Config, Version & Target files.
# Deploy the headers, resources, scenes & examples.
# Set the plugin 'relocatable' if built within SOFA.
# --> see SofaMacros.cmake
sofa_generate_package(
    NAME ${PROJECT_NAME}
    TARGETS ${PROJECT_NAME}
    VERSION ${PROJECT_VERSION}
    INCLUDE_SOURCE_DIR "src"
    INCLUDE_INSTALL_DIR ${PROJECT_NAME}
    RELOCATABLE "plugins"
    )
