cmake_minimum_required(VERSION 3.5.0 FATAL_ERROR)
project(lxqt-kwin-session)

list(APPEND CMAKE_MODULE_PATH "${CMAKE_CURRENT_SOURCE_DIR}/cmake")

include(GNUInstallDirs)

set(CMAKE_AUTOMOC ON)
set(CMAKE_AUTOUIC ON)
set(CMAKE_INCLUDE_CURRENT_DIR ON)
set(CMAKE_POSITION_INDEPENDENT_CODE ON)

# Minimum Versions
set(LXQT_MINIMUM_VERSION "2.0.0")

find_package(lxqt ${LXQT_MINIMUM_VERSION} REQUIRED)
#find_package(kwin REQUIRED)

find_package(PkgConfig REQUIRED)

# Please don't move, must be after lxqt
#find_package(XdgUserDirs REQUIRED)

# Patch Version
set(LXQT_KWIN_SESSION_PATCH_VERSION 0)

set(LXQT_KWIN_SESSION_VERSION 0.1)
add_definitions(
    "-DLXQT_kwin_SESSION_VERSION=\"${LXQT_kwin_SESSION_VERSION}\""
)

include(LXQtPreventInSourceBuilds)
include(LXQtCompilerSettings NO_POLICY_SCOPE)
include(GNUInstallDirs)
include(LXQtConfigVars)
file(GLOB DESKTOP_FILES_IN *.desktop.in)

find_program(KWIN_EXECUTABLE kwin_wayland)

if(KWIN_EXECUTABLE)
    execute_process(
        COMMAND ${KWIN_EXECUTABLE} --version
        OUTPUT_VARIABLE KWIN_VERSION
        OUTPUT_STRIP_TRAILING_WHITESPACE
    )
    message("Found kwin version: ${KWIN_VERSION}")
endif()


# startlxqt<compositor> script
set(PREDEF_XDG_DATA_DIRS "$XDG_DATA_HOME")
if(NOT("${LXQT_DATA_DIR}" MATCHES "^/usr(/local)?/share$"))
    set(PREDEF_XDG_DATA_DIRS "${PREDEF_XDG_DATA_DIRS}:${LXQT_DATA_DIR}")
endif()
set(PREDEF_XDG_DATA_DIRS "${PREDEF_XDG_DATA_DIRS}:/usr/local/share:/usr/share")
set(PREDEF_XDG_CONFIG_DIRS "/etc:${LXQT_ETC_XDG_DIR}:/usr/share")

configure_file(startlxqtkwin.in startlxqtkwin @ONLY)
configure_file(toggledropdown.in toggledropdown @ONLY)
configure_file(lxqt2kwinexporter.in lxqt2kwinexporter @ONLY)

install(PROGRAMS
    "${CMAKE_CURRENT_BINARY_DIR}/startlxqtkwin"
    "${CMAKE_CURRENT_BINARY_DIR}/toggledropdown"
    "${CMAKE_CURRENT_BINARY_DIR}/lxqt2kwinexporter"
    DESTINATION "${CMAKE_INSTALL_BINDIR}"
    COMPONENT Runtime
)

install(FILES
    startlxqtkwin.1
    DESTINATION "${CMAKE_INSTALL_MANDIR}/man1"
    COMPONENT Runtime
)


install(FILES
    lxqt-kwin.desktop
    DESTINATION "${CMAKE_INSTALL_DATADIR}/wayland-sessions"
    COMPONENT Runtime
)
