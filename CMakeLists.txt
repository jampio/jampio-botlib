cmake_minimum_required(VERSION 3.10)
project(jampiobotlib)
list(APPEND CMAKE_MODULE_PATH ${CMAKE_CURRENT_LIST_DIR}/cmake)
include(CheckTypeSize)
check_type_size("void*" CMAKE_SIZEOF_VOID_P)
message(STATUS "sizeof void*: " ${CMAKE_SIZEOF_VOID_P})
message(STATUS "CMAKE_CROSSCOMPILING: " ${CMAKE_CROSSCOMPILING})
message(STATUS "CMAKE_SYSTEM_PROCESSOR: " ${CMAKE_SYSTEM_PROCESSOR})
message(STATUS "CMAKE_LIBRARY_ARCHITECTURE: " ${CMAKE_LIBRARY_ARCHITECTURE})
find_package(jampioshared REQUIRED)
set(SOURCES
	src/be_aas_bspq3.cpp
	src/be_aas_cluster.cpp
	src/be_aas_debug.cpp
	src/be_aas_entity.cpp
	src/be_aas_file.cpp
	src/be_aas_main.cpp
	src/be_aas_move.cpp
	src/be_aas_optimize.cpp
	src/be_aas_reach.cpp
	src/be_aas_route.cpp
	src/be_aas_routealt.cpp
	src/be_aas_sample.cpp
	src/be_ai_char.cpp
	src/be_ai_chat.cpp
	src/be_ai_gen.cpp
	src/be_ai_goal.cpp
	src/be_ai_move.cpp
	src/be_ai_weap.cpp
	src/be_ai_weight.cpp
	src/be_ea.cpp
	src/be_interface.cpp
	src/l_crc.cpp
	src/l_libvar.cpp
	src/l_log.cpp
	src/l_memory.cpp
	src/l_precomp.cpp
	src/l_script.cpp
	src/l_struct.cpp
)
add_library(jampiobotlib STATIC ${SOURCES})
target_include_directories(jampiobotlib PRIVATE ${jampioshared_INCLUDE})
target_compile_definitions(jampiobotlib PRIVATE BOTLIB)
target_link_libraries(jampiobotlib PRIVATE jampioshared)
# custom install enforces lib/<arch> on certain systems
if(USE_CUSTOM_INSTALL)
	include(GNUInstallDirsCustom)
else()
	include(GNUInstallDirs)
endif()
install(TARGETS jampiobotlib EXPORT jampiobotlibConfig DESTINATION ${CMAKE_INSTALL_LIBDIR})
install(DIRECTORY src/ DESTINATION ${CMAKE_INSTALL_INCLUDEDIR}/jampio/botlib FILES_MATCHING PATTERN "*.h")
install(EXPORT jampiobotlibConfig DESTINATION ${CMAKE_INSTALL_LIBDIR}/jampiobotlib)