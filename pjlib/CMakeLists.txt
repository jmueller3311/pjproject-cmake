project(PJLIB)

set(DARWIN_SRC
	guid_simple.c 
	os_core_unix.c 
	os_error_unix.c 
	os_time_unix.c 
	os_timestamp_posix.c 
	ioqueue_select.c 
	file_access_unistd.c 
	file_io_ansi.c
	sock_qos_darwin.c
)
set(LINUX_SRC
	file_access_unistd.c 
	file_io_ansi.c 
	guid_simple.c 
	os_core_unix.c 
	os_error_unix.c 
	os_time_unix.c 
	os_timestamp_posix.c 
	sock_qos_bsd.c
)

if ( LINUX_POLL )
	list(APPEND LINUX_SRC ioqueue_epoll.c)
else()
	list(APPEND LINUX_SRC ioqueue_select.c)
endif()

set(RTEMS_SRC
	guid_simple.c 
	os_core_unix.c 
	os_error_unix.c 
	os_time_unix.c 
	os_timestamp_posix.c 
	ioqueue_select.c 
	file_access_unistd.c 
	file_io_ansi.c
	sock_qos_bsd.c
)
set(SUNOS_SRC
	file_access_unistd.c 
	file_io_ansi.c 
	guid_simple.c 
	os_core_unix.c 
	os_error_unix.c 
	os_time_unix.c 
	os_timestamp_posix.c 
	ioqueue_select.c
	sock_qos_bsd.c
)
set(WIN32_SRC
	guid_win32.c 
	unicode_win32.c 
	os_core_win32.c 
	os_error_win32.c 
	os_time_win32.c 
	os_timestamp_win32.c 
	ioqueue_select.c
	file_io_win32.c 
	file_access_win32.c
	sock_qos_bsd.c
)
set(SRC
	sock_bsd.c 
	sock_select.c
	pool_policy_malloc.c 
	os_timestamp_common.c 
	log_writer_stdout.c 
	addr_resolv_sock.c
	activesock.c 
	array.c 
	config.c 
	ctype.c 
	errno.c 
	except.c 
	fifobuf.c
	guid.c 
	hash.c 
	ip_helper_generic.c 
	list.c
	lock.c 
	log.c 
	os_time_common.c 
	os_info.c 
	pool.c 
	pool_buf.c 
	pool_caching.c 
	pool_dbg.c 
	rand.c
	rbtree.c 
	sock_common.c 
	sock_qos_common.c 
	ssl_sock_common.c 
	ssl_sock_ossl.c 
	ssl_sock_gtls.c 
	ssl_sock_dump.c 
	ssl_sock_darwin.c 
	string.c 
	timer.c 
	types.c
)
file(TOUCH include/pj/config_site.h)

if(${CMAKE_SYSTEM_NAME} STREQUAL "Darwin")
	list(APPEND SRC ${DARWIN_SRC})
elseif(${CMAKE_SYSTEM_NAME} STREQUAL "Linux")
	list(APPEND SRC ${LINUX_SRC})
elseif(${CMAKE_SYSTEM_NAME} STREQUAL "SunOS")
	list(APPEND SRC ${SUNOS_SRC})
elseif(${CMAKE_SYSTEM_NAME} STREQUAL "Windows")
	list(APPEND SRC ${WIN32_SRC})
else()
	message(FATAL_ERROR "FAILED to recognize OS.")
endif()

list(TRANSFORM SRC PREPEND "src/pj/")
add_library(${PROJECT_NAME} ${SRC})
add_library(PJSIP::${PROJECT_NAME} ALIAS ${PROJECT_NAME})
target_include_directories(${PROJECT_NAME} PUBLIC "$<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}>$<INSTALL_INTERFACE:${CMAKE_INSTALL_INCLUDEDIR}>/include")

if(WIN32)
  target_link_libraries(${PROJECT_NAME} wsock32 ws2_32)
endif()


file(GLOB_RECURSE hdrs RELATIVE ${CMAKE_CURRENT_SOURCE_DIR} CONFIGURE_DEPENDS ${CMAKE_CURRENT_SOURCE_DIR}/include/*.hpp ${CMAKE_CURRENT_SOURCE_DIR}/include/*.h)
target_sources(${PROJECT_NAME} 
  PUBLIC FILE_SET HEADERS
  BASE_DIRS include
  FILES ${hdrs}
)

string(TOLOWER ${PROJECT_NAME} pn_low)
install(TARGETS ${PROJECT_NAME} EXPORT ${pn_low}_config FILE_SET HEADERS)
install(EXPORT ${pn_low}_config
        NAMESPACE PJSIP::
        DESTINATION cmake
)

project(PJLIB_TEST)

set(DARWIN_SRC
	main.c
)
set(LINUX_SRC
	main.c
)
set(RTEMS_SRC
	main_rtems.c
)
set(SUNOS_SRC
	main.c
)
set(WIN32_SRC
	main.c
)
set(SRC
	activesock.c 
	atomic.c 
	echo_clt.c 
	errno.c 
	exception.c 
	fifobuf.c 
	file.c 
	hash_test.c 
	ioq_perf.c 
	ioq_udp.c 
	ioq_stress_test.c 
	ioq_unreg.c 
	ioq_tcp.c 
	list.c 
	mutex.c 
	os.c 
	pool.c 
	pool_perf.c 
	rand.c 
	rbtree.c 
	select.c 
	sleep.c 
	sock.c 
	sock_perf.c 
	ssl_sock.c 
	string.c 
	test.c 
	thread.c 
	timer.c 
	timestamp.c 
	udp_echo_srv_sync.c 
	udp_echo_srv_ioqueue.c 
	util.c
)

if(${CMAKE_SYSTEM_NAME} STREQUAL "Darwin")
	list(APPEND SRC ${DARWIN_SRC})
elseif(${CMAKE_SYSTEM_NAME} STREQUAL "Linux")
	list(APPEND SRC ${LINUX_SRC})
elseif(${CMAKE_SYSTEM_NAME} STREQUAL "SunOS")
	list(APPEND SRC ${SUNOS_SRC})
elseif(${CMAKE_SYSTEM_NAME} STREQUAL "Windows")
	list(APPEND SRC ${WIN32_SRC})
else()
	message(FATAL_ERROR "FAILED to recognize OS.")
endif()

list(TRANSFORM SRC PREPEND "src/pjlib-test/")
add_executable(${PROJECT_NAME} ${SRC})
target_include_directories(${PROJECT_NAME} PUBLIC include)
target_link_libraries(${PROJECT_NAME} PUBLIC PJSIP::PJLIB)

add_test(NAME ${PROJECT_NAME} COMMAND ${PROJECT_NAME})