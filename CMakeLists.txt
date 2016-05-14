project( bcp )
cmake_minimum_required( VERSION 2.8.3 )

set( CMAKE_COLOR_MAKEFILE ON )
set_property( GLOBAL PROPERTY USE_FOLDERS ON )

# Allow advanced users to generate Makefiles printing detailed commands
mark_as_advanced( CMAKE_VERBOSE_MAKEFILE )

# large address aware option setting
if( ${MINGW} )
	set( LARGEADDRESSAWARE "--large-address-aware" )
else( ${MINGW} )
	if ( WIN32 )
		set( LARGEADDRESSAWARE "/LARGEADDRESSAWARE" )
	else ( WIN32 )
		set( LARGEADDRESSAWARE " " )
	endif ( WIN32 )
endif( ${MINGW} )

if ( WIN32)
	SET( APP_OPT "WIN32" )
else ( WIN32 )
	if ( APPLE )
		#SET( APP_OPT "MACOSX_BUNDLE" )
		SET( APP_OPT "EXCLUDE_FROM_ALL" )
	else ( APPLE )
		SET( APP_OPT " " )
	endif ( APPLE )
endif ( WIN32 )

find_package( Boost REQUIRED )
include_directories( ${Boost_INCLUDE_DIR} )
link_directories( ${Boost_LIBRARY_DIRS} )
link_directories( ${Boost_LIBRARY_DIR} )
link_directories( "D:/boost_1_58_0/lib64-msvc-10.0" )
set( Boost_USE_STATIC_LIBS        OFF )
set( Boost_USE_MULTITHREADED      ON )
set( Boost_USE_STATIC_RUNTIME     ON )
set( BOOST_ALL_DYN_LINK           OFF )

include_directories( ${CMAKE_SOURCE_DIR} )
include_directories( ${CMAKE_BINARY_DIR} )
include_directories( ${CMAKE_SOURCE_DIR}/src )
include_directories( ${CMAKE_BINARY_DIR}/src )

link_directories( ${CMAKE_BINARY_DIR}/ )

# Setup output directories
set( LIBRARY_OUTPUT_PATH 
	${PROJECT_BINARY_DIR}/bin
	CACHE
	PATH
	"Single directory for all libraries" )
	
set( EXECUTABLE_OUTPUT_PATH 
	${PROJECT_BINARY_DIR}/bin
	CACHE
	PATH
	"Single directory for all executables" )

file( GLOB SRCS *.c *.cpp *.cc *.h *.hpp )
add_executable( ${PROJECT_NAME} ${SRCS} )
target_link_libraries( ${PROJECT_NAME} ${Boost_LIBRARIES} )
