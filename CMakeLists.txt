cmake_minimum_required (VERSION 2.6)
project (wxWidgets)

set(ABOUT_TEXT "default about text" CACHE STRING "About text")

CONFIGURE_FILE( config.h.in config.h )
include_directories( ${PROJECT_BINARY_DIR} )

find_package(wxWidgets REQUIRED core)

include(${wxWidgets_USE_FILE})

add_executable(hello MACOSX_BUNDLE hello.cxx)
target_link_libraries(hello ${wxWidgets_LIBRARIES})
set_target_properties( hello PROPERTIES MACOSX_BUNDLE_INFO_PLIST ${PROJECT_SOURCE_DIR}/Info.plist )

INSTALL(CODE "
	include(BundleUtilities)
	set(BU_CHMOD_BUNDLE_ITEMS ON) # For some reason, the copied libs are 555 without this
	FIXUP_BUNDLE(\"${PROJECT_BINARY_DIR}/hello.app\" \"\" \"\")
	")
