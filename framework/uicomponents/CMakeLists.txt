#=============================================================================
#  MuseScore
#  Music Composition & Notation
#
#  Copyright (C) 2020 MuseScore BVBA and others
#
#  This program is free software; you can redistribute it and/or modify
#  it under the terms of the GNU General Public License version 2.
#
#  This program is distributed in the hope that it will be useful,
#  but WITHOUT ANY WARRANTY; without even the implied warranty of
#  MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the
#  GNU General Public License for more details.
#
#  You should have received a copy of the GNU General Public License
#  along with this program; if not, write to the Free Software
#  Foundation, Inc., 675 Mass Ave, Cambridge, MA 02139, USA.
#=============================================================================

set(MODULE uicomponents)
set(LIBRARY_TYPE STATIC)
message(STATUS "Configuring " ${MODULE})

include (${PROJECT_SOURCE_DIR}/build/gch.cmake)

if (NOT MSVC)
    set(_all_h_file "${PROJECT_BINARY_DIR}/all.h")
else (NOT MSVC)
    set(_all_h_file "${PROJECT_SOURCE_DIR}/all.h")
endif (NOT MSVC)

include_directories(
    ${PROJECT_SOURCE_DIR}/framework
)

qt5_add_resources(RCC_SOURCES uicomponents.qrc)
set(QML_IMPORT_PATH "${QML_IMPORT_PATH};${CMAKE_CURRENT_LIST_DIR}/qml" CACHE STRING "Qt Creator 4.1 extra qml import paths")

add_library ( ${MODULE} ${LIBRARY_TYPE}
    ${_all_h_file}
    ${PCH}
    ${RCC_SOURCES}
    ${CMAKE_CURRENT_LIST_DIR}/uicomponentsmodule.cpp
    ${CMAKE_CURRENT_LIST_DIR}/uicomponentsmodule.h
    ${CMAKE_CURRENT_LIST_DIR}/cpp/sampleobject.cpp
    ${CMAKE_CURRENT_LIST_DIR}/cpp/sampleobject.h
)

if (NOT MSVC)
    set_target_properties (${MODULE} PROPERTIES COMPILE_FLAGS "${PCH_INCLUDE} -g -Wall -Wextra -Winvalid-pch")
else (NOT MSVC)
    set_target_properties (${MODULE} PROPERTIES COMPILE_FLAGS "${PCH_INCLUDE}" )
endif (NOT MSVC)

xcode_pch(${MODULE} all)

# Use MSVC pre-compiled headers
vstudio_pch( ${MODULE} )

# MSVC does not depend on mops1 & mops2 for PCH
if (NOT MSVC)
    ADD_DEPENDENCIES(${MODULE} mops1)
    ADD_DEPENDENCIES(${MODULE} mops2)
endif (NOT MSVC)
