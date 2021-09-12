cmake_minimum_required(VERSION 3.13)
project(hamclock-systemd VERSION 2.68.7.6)
include(GNUInstallDirs)

install(FILES resources/systemd/hamclock.service DESTINATION /etc/systemd/system
		COMPONENT HAMCLOCK_SYSTEMD EXCLUDE_FROM_ALL)
install(FILES SourceCode/hamclock.png SourceCode/hamclock.desktop DESTINATION "${CMAKE_INSTALL_DATADIR}/hamclock"
		COMPONENT HAMCLOCK EXCLUDE_FROM_ALL)
install(FILES resources/repository/ve3ysh.list DESTINATION /etc/apt/sources.list.d
		COMPONENT VE3YSH_REPO EXCLUDE_FROM_ALL)
	install(PROGRAMS
	SourceCode/hamclock-fb0-800x480
	SourceCode/hamclock-fb0-1600x960
	SourceCode/hamclock-fb0-2400x1440
	SourceCode/hamclock-fb0-3200x1920
	DESTINATION ${CMAKE_INSTALL_BINDIR}
	COMPONENT HAMCLOCK_SYSTEMD EXCLUDE_FROM_ALL)

install(PROGRAMS
		SourceCode/hamclock-800x480
		SourceCode/hamclock-1600x960
		SourceCode/hamclock-2400x1440
		SourceCode/hamclock-3200x1920
		DESTINATION ${CMAKE_INSTALL_BINDIR}
		COMPONENT HAMCLOCK EXCLUDE_FROM_ALL)

set(CPACK_DEB_COMPONENT_INSTALL 1)
set(CPACK_DEBIAN_ENABLE_COMPONENT_DEPENDS 1)
set(CPACK_DEBIAN_FILE_NAME "DEB-DEFAULT")
set(CPACK_PACKAGE_VERSION "${PROJECT_VERSION_MAJOR}.${PROJECT_VERSION_MINOR}.${PROJECT_VERSION_PATCH}")
set(CPACK_PACKAGE_RELEASE ${PROJECT_VERSION_TWEAK})
set(CPACK_GENERATOR "DEB")
set(CPACK_PACKAGE_NAME "${PROJECT_NAME}")
set(CPACK_DEBIAN_HAMCLOCK_SYSTEMD_PACKAGE_NAME "${PROJECT_NAME}")
set(CPACK_DEBIAN_HAMCLOCK_PACKAGE_NAME "hamclock")
set(CPACK_DEBIAN_VE3YSH_REPO_PACKAGE_NAME "ve3ysh-repo")
set(CPACK_DEBIAN_PACKAGE_SECTION "hamradio")
set(CPACK_RPM_PACKAGE_URL "https://github.com/pa28/hamclock-systemd")
set(CPACK_RPM_PACKAGE_SUMMARY "Autorun HamClock using systemd")
set(CPACK_RPM_PACKAGE_DESCRIPTION "Autorun HamClock using systemd")
set(CPACK_RPM_PACKAGE_VENDOR "www.hrbuckley.net")
set(CPACK_PACKAGE_CONTACT "Richard Buckley")
set(CPACK_RPM_PACKAGE_GROUP "Applications/Productivity")
set(CPACK_DEBIAN_PACKAGE_SHLIBDEPS ON)
set(CPACK_PACKAGING_INSTALL_PREFIX ${CMAKE_INSTALL_PREFIX})
set(CPACK_PACKAGE_HAMCLOCK_SYSTEMD_EXECUTABLES SourceCode/ESPHamClock/hamclock-fb0-800x480)
set(CPACK_PACKAGE_HAMCLOCK_EXECUTABLES SourceCode/ESPHamClock/hamclock-800x480)
set(CPACK_RPM_EXCLUDE_FROM_AUTO_FILELIST_ADDITION
        /usr/local
        /usr/local/bin
        /usr/local/share
        /usr/local/include
        /usr/lib/systemd
        /usr/lib/systemd/system
        )
SET(CPACK_DEBIAN_HAMCLOCK_SYSTEMD_PACKAGE_CONTROL_EXTRA "${CMAKE_CURRENT_SOURCE_DIR}/postinst")

include(CPack)

cpack_add_component(HAMCLOCK_SYSTEMD
		DISPLAY_NAME "hamclock-systemd"
		DESCRIPTION "A systemd way to autostart hamclock on /dev/fb0"
		)
cpack_add_component(HAMCLOCK
		DISPLAY_NAME "hamclock"
		DESCRIPTION "Precompiled X11 hamclock binaries"
		)
cpack_add_componenT(VE3YSH_REPO
		DISPLAY_NAME "ve3ysh-repo"
		DESCRIPTION "Aptitude package repository by VE3YSH")
