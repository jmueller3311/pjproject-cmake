project(PJLIB_UTIL)

set(SRC
  base64.c
  cli.c
  cli_console.c
  cli_telnet.c
  crc32.c
  errno.c
  dns.c
  dns_dump.c
  dns_server.c
  getopt.c
  hmac_md5.c
  hmac_sha1.c
  http_client.c
  json.c
  md5.c
  pcap.c
  resolver.c
  scanner.c
  sha1.c
  srv_resolver.c
  string.c
  stun_simple.c
  stun_simple_client.c
  xml.c
)

list(TRANSFORM SRC PREPEND "src/pjlib-util/")
add_library(${PROJECT_NAME} ${SRC})
add_library(PJSIP::${PROJECT_NAME} ALIAS ${PROJECT_NAME})
target_include_directories(${PROJECT_NAME} PUBLIC "$<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}>$<INSTALL_INTERFACE:${CMAKE_INSTALL_INCLUDEDIR}>/include")

target_link_libraries(${PROJECT_NAME} PJSIP::PJLIB)

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

project(PJLIB_UTIL_TEST)

set(SRC
	xml.c 
	encryption.c 
	stun.c 
	resolver_test.c 
	test.c
	json_test.c 
	http_client.c
	main.c
)


list(TRANSFORM SRC PREPEND "src/pjlib-util-test/")
add_executable(${PROJECT_NAME} ${SRC})
target_include_directories(${PROJECT_NAME} PUBLIC include)
target_link_libraries(${PROJECT_NAME} PUBLIC PJSIP::PJLIB PJSIP::PJLIB_UTIL)

add_test(NAME ${PROJECT_NAME} COMMAND ${PROJECT_NAME})