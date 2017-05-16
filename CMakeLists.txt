project(qtautoupdater-cmake)

set(CMAKE_AUTOMOC ON)
set(CMAKE_INCLUDE_CURRENT_DIR ON)
set(CMAKE_AUTOUIC ON)

cmake_minimum_required(VERSION 2.8.11)

set( CMAKE_CXX_STANDARD 11 )

find_package(Qt5Core REQUIRED)

include_directories("")
link_directories("")

add_executable(AutoUpdaterExample main.cpp ${LIBS})

qt5_use_modules(AutoUpdaterExample Core)
target_link_libraries(AutoUpdaterExample autoUpdater)
