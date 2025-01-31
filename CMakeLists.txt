cmake_minimum_required(VERSION 2.6)

project(menuconfig)

set(CURSES_NEED_NCURSES TRUE)
find_package(Curses REQUIRED)

if (CURSES_HAVE_CURSES_H)
   add_definitions(-DHAVE_CURSES_H=1)
endif ()
if (CURSES_HAVE_NCURSES_H)
   add_definitions(-DHAVE_NCURSES_H=1)
endif ()
if (CURSES_HAVE_NCURSES_CURSES_H)
   add_definitions(-DHAVE_NCURSES_CURSES_H=1)
endif ()
if (CURSES_HAVE_NCURSES_NCURSES_H)
   add_definitions(-DHAVE_NCURSES_NCURSES_H=1)
endif ()

option(DEBUG "是否保留调试信息以便调试")
set(DEBUG ON)
message(DEBUG=${DEBUG})

if(DEBUG)
    set(CMAKE_BUILD_TYPE Debug)
    set(CMAKE_C_FLAGS_DEBUG "${CMAKE_C_FLAGS_DEBUG} -g")
endif()

include_directories(
   "${PROJECT_SOURCE_DIR}/lxdialog"
   ${CURSES_INCLUDE_DIR}
)

add_subdirectory(lxdialog)


add_library(zconf zconf.tab.c)

add_executable(mconf mconf.c)
add_executable(conf conf.c)

target_link_libraries(mconf lxdialog ${CURSES_LIBRARIES} zconf)
target_link_libraries(conf ${CURSES_LIBRARIES} zconf)