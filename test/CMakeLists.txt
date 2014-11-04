project(aaki-test)

cmake_minimum_required(VERSION 2.8)
set(CMAKE_BUILD_TYPE "Debug")
set(CMAKE_CXX_FLAGS "-g -Wall")

FUNCTION(fn_find_cfg result source_dir cfg_file)
	STRING(REPLACE "/" ";" temp_list1 ${source_dir})
	SET(temp_list2 ${temp_list1})

	foreach(temp1 ${temp_list1})
		unset(search_dir)
		foreach(temp2 ${temp_list2})
			set(search_dir ${search_dir}/${temp2})
		endforeach(temp2)
		if(EXISTS ${search_dir}/${cfg_file})
			message("EXISTS_file : " ${search_dir}/${cfg_file})
			set(${result} ${search_dir} PARENT_SCOPE)
			break()
		else()
			list(REMOVE_AT temp_list2 -1)
		endif()
	endforeach(temp1)
ENDFUNCTION(fn_find_cfg)

if(NOT CFG_ROOT)
	fn_find_cfg(CFG_ROOT ${CMAKE_CURRENT_SOURCE_DIR} aaki-config.cmake)
endif(NOT CFG_ROOT)

if(CFG_ROOT)
	include(${CFG_ROOT}/aaki-config.cmake OPTIONAL)
	message("CFG_ROOT : " ${CFG_ROOT})
	message("CFG_ROOT2 : " ${CFG_ROOT2})
else(CFG_ROOT)
	message("CFG_ROOT : None!")
	return()
endif(CFG_ROOT)

message("CFG_ROOT : None!!")

aux_source_directory(. SRC_LIST)
add_executable(${PROJECT_NAME} ${SRC_LIST})

message("CFG_ROOT : None!!!")