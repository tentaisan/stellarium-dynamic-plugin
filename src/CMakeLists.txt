# Copyright (C) 2007 Fabien Chereau
# Copyright (C) 2015 Kirill Snezhko
# Copyright (C) 2018 Alexander Wolf
#
# Based on the HelloStelModule

INCLUDE_DIRECTORIES(.)

LINK_DIRECTORIES(${CMAKE_BINARY_DIR}/src)

SET(DynamicPluginTemplate_SRCS
     # Source code
     DynamicPluginTemplate.hpp
     DynamicPluginTemplate.cpp
     gui/DynamicPluginTemplateWindow.cpp
     gui/DynamicPluginTemplateWindow.hpp
)

SET(DynamicPluginTemplate_UIS
     # Forms
     gui/dynamicPluginTemplateWindow.ui
)

SET(DynamicPluginTemplate_RES_SXX
     # Resources
     # ../resources/DynamicPluginTemplate.qrc
)

# Wrapping UI
# compiles resources files
IF (${QT_VERSION_MAJOR} EQUAL "5")
     QT5_WRAP_UI(DynamicPluginTemplater_UIS_H ${DynamicPluginTemplate_UIS})
     QT5_ADD_RESOURCES(DynamicPluginTemplate_RES_CXX ${DynamicPluginTemplate_RES})
ELSE()
     QT_WRAP_UI(DynamicPluginTemplate_UIS_H ${DynamicPluginTemplate_UIS})
     QT_ADD_RESOURCES(DynamicPluginTemplate_RES_CXX ${DynamicPluginTemplate_RES})
ENDIF()

# Library properties
ADD_LIBRARY(DynamicPluginTemplate-dynamic MODULE ${DynamicPluginTemplate_SRCS} ${DynamicPluginTemplate_RES_SXX} ${DynamicPluginTemplate_UIS_H})
TARGET_LINK_LIBRARIES(DynamicPluginTemplate-dynamic Qt${QT_VERSION_MAJOR}::Core Qt${QT_VERSION_MAJOR}::Gui Qt${QT_VERSION_MAJOR}::Widgets Qt${QT_VERSION_MAJOR}::OpenGL)
SET_TARGET_PROPERTIES(DynamicPluginTemplate-dynamic PROPERTIES OUTPUT_NAME "${PROJECT_NAME}")
SET_TARGET_PROPERTIES(DynamicPluginTemplate-dynamic PROPERTIES COMPILE_FLAGS "-DQT_DYNAMICPLUGIN")
SET_TARGET_PROPERTIES(DynamicPluginTemplate-dynamic PROPERTIES AUTOMOC TRUE)
SET_TARGET_PROPERTIES(DynamicPluginTemplate-dynamic PROPERTIES FOLDER "plugins/${PROJECT_NAME}")
