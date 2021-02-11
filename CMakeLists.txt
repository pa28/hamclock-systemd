cmake_minimum_required(VERSION 3.13)
project(hamclock-systemd VERSION 2.57.3.4)
include(GNUInstallDirs)

install(FILES resources/systemd/hamclock.service DESTINATION /etc/systemd/system)
install(PROGRAMS
	SourceCode/hamclock-fb0-800x480
	SourceCode/hamclock-fb0-1600x960
	SourceCode/hamclock-fb0-2400x1440
	SourceCode/hamclock-fb0-3200x1920
	DESTINATION ${CMAKE_INSTALL_BINDIR})

set(CPACK_DEBIAN_FILE_NAME "DEB-DEFAULT")
set(CPACK_PACKAGE_VERSION "${PROJECT_VERSION_MAJOR}.${PROJECT_VERSION_MINOR}.${PROJECT_VERSION_PATCH}")
set(CPACK_PACKAGE_RELEASE ${PROJECT_VERSION_TWEAK})
set(CPACK_GENERATOR "DEB")
set(CPACK_PACKAGE_NAME "${PROJECT_NAME}")
set(CPACK_DEBIAN_PACKAGE_SECTION "hamradio")
set(CPACK_RPM_PACKAGE_URL "https://github.com/pa28/hamclock-systemd")
set(CPACK_RPM_PACKAGE_SUMMARY "Autorun HamClock using systemd")
set(CPACK_RPM_PACKAGE_DESCRIPTION "Autorun HamClock using systemd")
set(CPACK_RPM_PACKAGE_VENDOR "www.hrbuckley.net")
set(CPACK_PACKAGE_CONTACT "Richard Buckley")
set(CPACK_RPM_PACKAGE_GROUP "Applications/Productivity")
set(CPACK_DEBIAN_PACKAGE_SHLIBDEPS ON)
set(CPACK_PACKAGING_INSTALL_PREFIX ${CMAKE_INSTALL_PREFIX})
set(CPACK_PACKAGE_EXECUTABLES SourceCode/ESPHamClock/hamclock-fb0-800x480)
set(CPACK_RPM_EXCLUDE_FROM_AUTO_FILELIST_ADDITION
        /usr/local
        /usr/local/bin
        /usr/local/share
        /usr/local/include
        /usr/lib/systemd
        /usr/lib/systemd/system
        )
SET(CPACK_DEBIAN_PACKAGE_CONTROL_EXTRA "${CMAKE_CURRENT_SOURCE_DIR}/postinst")

include(CPack)

