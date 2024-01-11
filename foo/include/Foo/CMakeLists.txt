# inlcude/Foo cmake

set (FILES "${CMAKE_CURRENT_SOURCE_DIR}/Hello.h")
set (SUBDIRECTORIES "")
set (RELATIVE_PATH "include/Foo")

# Add all subdirectories
foreach (VAR ${SUBDIRECTORIES})
	add_subdirectory (${VAR})
endforeach()

# Check any files in this directory
if(FILES)
	# Add source files to target
	target_sources ("${PROJECT_NAME}" PRIVATE ${FILES})
	
	# Install out source files
	install (FILES ${FILES} DESTINATION ${RELATIVE_PATH})
endif()

