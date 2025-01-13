cmake_minimum_required(VERSION 3.16)

# Set the project name
project(p2000t-ide LANGUAGES CXX)

set(CMAKE_INCLUDE_CURRENT_DIR ON)

set(CMAKE_AUTOMOC ON)
set(CMAKE_AUTORCC ON)

set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

# get Git HASH
execute_process(
    COMMAND git log -1 --format=%h
    WORKING_DIRECTORY ${CMAKE_CURRENT_LIST_DIR}
    OUTPUT_VARIABLE GIT_HASH
    OUTPUT_STRIP_TRAILING_WHITESPACE
)
add_compile_definitions(GIT_HASH="${GIT_HASH}")

# Find Qt6 packages
find_package(Qt6 REQUIRED COMPONENTS Widgets SerialPort Network)

add_executable(p2000t-ide
    src/dialogslotselection.cpp
    src/assemblyhighlighter.cpp
    src/codeeditor.cpp
    src/main.cpp
    src/mainwindow.cpp
    src/qhexview.cpp
    src/searchwidget.cpp
    src/threadcompile.cpp
    src/threadrun.cpp
    resources.qrc
)

if (${CMAKE_SYSTEM_NAME} MATCHES "Darwin")
    set(MACOSX TRUE)
    set_target_properties(p2000t-ide PROPERTIES
        MACOSX_BUNDLE TRUE
    )
endif()

target_link_libraries(p2000t-ide PRIVATE Qt6::Core Qt6::Widgets Qt6::SerialPort Qt6::Network)

set_target_properties(p2000t-ide PROPERTIES
    WIN32_EXECUTABLE ON
    MACOSX_BUNDLE ON
)
