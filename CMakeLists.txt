# Copyright (C) 2007 Fabien Chereau
# Copyright (C) 2015 Kirill Snezhko
# Copyright (C) 2018 Alexander Wolf
#
# Based on the HelloStelModule

CMAKE_MINIMUM_REQUIRED(VERSION 3.10)

# Set name of project (name of the plugin by the fact)
PROJECT(DynamicPluginTemplate)

# Set version of the plugin
SET(DYNAMICPLUGINTEMPLATE_VERSION "0.2.0")

# define requirements for C/C++ standards
SET(CMAKE_CXX_STANDARD 17)
SET(CMAKE_CXX_STANDARD_REQUIRED ON)
SET(CMAKE_CXX_EXTENSIONS OFF)
SET(CMAKE_C_STANDARD 11)
SET(CMAKE_C_STANDARD_REQUIRED ON)
SET(CMAKE_C_EXTENSIONS OFF)

# Set variables for using inside of plugin
ADD_DEFINITIONS(-DDYNAMICPLUGINTEMPLATE_VERSION="${DYNAMICPLUGINTEMPLATE_VERSION}")
ADD_DEFINITIONS(-DDYNAMICPLUGINTEMPLATE_LICENSE="GPLv2+")

# Requirements
SET(ENABLE_QT6 1 CACHE BOOL "Whether to try building with Qt6. If Qt6 is not found, Qt5 will be used.")
IF(ENABLE_QT6)
    FIND_PACKAGE(Qt6 COMPONENTS Core QUIET)
ENDIF()
if (NOT Qt6_FOUND)
    FIND_PACKAGE(Qt5 REQUIRED COMPONENTS Core)
ENDIF()

IF(Qt6_FOUND)
     SET(MINIMAL_QT_VERSION "6.2.0")
     IF(NOT DEFINED QT_VERSION_MAJOR)
         SET(QT_VERSION_MAJOR 6)
     ENDIF()
ELSE()
     SET(MINIMAL_QT_VERSION "5.12.0")
     IF(NOT DEFINED QT_VERSION_MAJOR)
         SET(QT_VERSION_MAJOR 5)
     ENDIF()
ENDIF()

FIND_PACKAGE(Qt${QT_VERSION_MAJOR} COMPONENTS Core Gui Widgets OpenGL REQUIRED)

# Stellarium source code directory
SET(STELLARIUM_SOURCE_DIR $ENV{STELROOT})

INCLUDE_DIRECTORIES(
     ${CMAKE_BINARY_DIR}
     ${CMAKE_SOURCE_DIR}/src
     ${CMAKE_SOURCE_DIR}/src/gui
     ${STELLARIUM_SOURCE_DIR}/src
     ${STELLARIUM_SOURCE_DIR}/src/core
     ${STELLARIUM_SOURCE_DIR}/src/core/modules
     ${STELLARIUM_SOURCE_DIR}/src/core/planetsephems
     ${STELLARIUM_SOURCE_DIR}/src/external
     ${STELLARIUM_SOURCE_DIR}/src/external/qtcompress
     ${STELLARIUM_SOURCE_DIR}/src/external/qcustomplot
     ${STELLARIUM_SOURCE_DIR}/src/external/qxlsx
     ${STELLARIUM_SOURCE_DIR}/src/external/glues_stel/source/
     ${STELLARIUM_SOURCE_DIR}/src/external/glues_stel/source/libtess
     ${STELLARIUM_SOURCE_DIR}/src/gui
     ${STELLARIUM_SOURCE_DIR}/src/scripting
     ${INTL_INCLUDE_DIR}
     ${CMAKE_BINARY_DIR}/src
     ${ZLIB_INCLUDE_DIR}
)

ADD_SUBDIRECTORY( src )

SET(CMAKE_INSTALL_PREFIX $ENV{HOME}/.stellarium)
INSTALL(FILES "${CMAKE_BINARY_DIR}/src/lib${PROJECT_NAME}.so" DESTINATION "modules/${PROJECT_NAME}")
