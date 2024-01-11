# foo cmake 

# BUILD_SHARED_LIBS = True : SHARED Library 
# BUILD_SHARED_LIBS	= False : STATIC Library

# add_library ("${PROJECT_NAME}" STATIC|SHARED "SOURCE_CODE")
add_library ("${PROJECT_NAME}" STATIC "")
add_library (Foo::Foo ALIAS "${PROJECT_NAME}")



# specify include directory
	# Generator expression : $<key: value>
target_include_directories ( "${PROJECT_NAME}" PUBLIC   # PUBLIC (PRIVATE + INTERFACE) | PRIVATE (Build Requirement) | INTERFACE (Usage Requirement)
	"$<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/include>"   # generator expression
	"$<BUILD_INTERFACE:${CMAKE_CURRENT_BINARY_DIR}>" # foo_export.h
	"$<INSTALL_INTERFACE:include>"  # ${CMAKE_INSTALL_PREFIX}/include
)

if(NOT BUILD_SHARED_LIBS)
	set (STATIC_POSTFIX "-s")
	set_target_properties ("${PROJECT_NAME}" PROPERTIES 
	COMPILE_FLAGS -DFOO_STATIC_DEFINE)
endif()

# our target name is "Foo" and change the properties of target name
# Rename target Foo
set_target_properties ("${PROJECT_NAME}" PROPERTIES
	DEBUG_POSTFIX "${STATIC_POSTFIX}-d"
	RELEASE_POSTFIX "${STATIC_POSTFIX}"
	MINSIZEREL_POSTFIX "${STATIC_POSTFIX}-mr"    # MINIMUM SIZE RELEASE POSTFIX
	RELWITHDEBINFO_POSTFIX "${STATIC_POSTFIX}-rd"    # RELEASE WITH DEBUG INFORMATION POSTFIX
)

include ("GenerateExportHeader")
generate_export_header ("${PROJECT_NAME}")
install (FILES "${CMAKE_CURRENT_BINARY_DIR}/foo_export.h" DESTINATION "include")

add_subdirectory ("include")
add_subdirectory ("src")