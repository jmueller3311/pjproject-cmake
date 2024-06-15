project(PJSIP)

set(SRC
	sip_config.c
	sip_multipart.c
	sip_errno.c
	sip_msg.c
	sip_parser.c
	sip_tel_uri.c
	sip_uri.c
	sip_endpoint.c
	sip_util.c
	sip_util_proxy.c
	sip_resolve.c
	sip_transport.c
	sip_transport_loop.c
	sip_transport_udp.c
	sip_transport_tcp.c
	sip_transport_tls.c
	sip_auth_aka.c
	sip_auth_client.c
	sip_auth_msg.c
	sip_auth_parser.c
	sip_auth_server.c
	sip_transaction.c
	sip_util_statefull.c
	sip_dialog.c
	sip_ua_layer.c
)

list(TRANSFORM SRC PREPEND "src/pjsip/")
add_library(${PROJECT_NAME} ${SRC})
add_library(PJSIP::${PROJECT_NAME} ALIAS ${PROJECT_NAME})
target_include_directories(${PROJECT_NAME} PUBLIC "$<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}>$<INSTALL_INTERFACE:${CMAKE_INSTALL_INCLUDEDIR}>/include")

target_link_libraries(${PROJECT_NAME} PJSIP::PJLIB PJSIP::PJLIB_UTIL )

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


project(PJSIP_SIMPLE)

set(SRC
	errno.c 
	evsub.c 
	evsub_msg.c 
	iscomposing.c 
	mwi.c 
	pidf.c 
	dialog_info.c 
	presence.c 
	dlg_event.c 
	presence_body.c
	publishc.c 
	rpid.c 
	xpidf.c
)

list(TRANSFORM SRC PREPEND "src/pjsip-simple/")
add_library(${PROJECT_NAME} ${SRC})
add_library(PJSIP::${PROJECT_NAME} ALIAS ${PROJECT_NAME})
target_include_directories(${PROJECT_NAME} PUBLIC "$<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}>$<INSTALL_INTERFACE:${CMAKE_INSTALL_INCLUDEDIR}>/include")

target_link_libraries(${PROJECT_NAME} PJSIP::PJLIB PJSIP::PJLIB_UTIL )

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

project(PJSIP_UA)

set(SRC
	sip_inv.c 
	sip_reg.c 
	sip_replaces.c 
	sip_xfer.c
	sip_100rel.c 
	sip_timer.c
)

list(TRANSFORM SRC PREPEND "src/pjsip-ua/")
add_library(${PROJECT_NAME} ${SRC})
add_library(PJSIP::${PROJECT_NAME} ALIAS ${PROJECT_NAME})
target_include_directories(${PROJECT_NAME} PUBLIC "$<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}>$<INSTALL_INTERFACE:${CMAKE_INSTALL_INCLUDEDIR}>/include")

target_link_libraries(${PROJECT_NAME} PJSIP::PJLIB PJSIP::PJLIB_UTIL PJSIP::PJMEDIA PJSIP::PJSIP PJSIP::PJSIP_SIMPLE)

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

project(PJSUA)

set(SRC
	pjsua_acc.c 
	pjsua_call.c 
	pjsua_core.c
	pjsua_im.c 
	pjsua_media.c 
	pjsua_pres.c
	pjsua_dump.c 
	pjsua_aud.c 
	pjsua_vid.c
)

list(TRANSFORM SRC PREPEND "src/pjsua-lib/")
add_library(${PROJECT_NAME} ${SRC})
add_library(PJSIP::${PROJECT_NAME} ALIAS ${PROJECT_NAME})
target_include_directories(${PROJECT_NAME} PUBLIC "$<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}>$<INSTALL_INTERFACE:${CMAKE_INSTALL_INCLUDEDIR}>/include")

target_link_libraries(${PROJECT_NAME} PJSIP::PJSIP_SIMPLE PJSIP::PJSIP PJSIP::PJMEDIA_AUDIODEV PJSIP::PJMEDIA_VIDEODEV PJSIP::PJMEDIA_CODEC PJSIP::PJMEDIA PJSIP::PJNATH PJSIP::PJLIB_UTIL PJSIP::PJLIB)

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

project(PJSUA2)

set(SRC
	account.cpp 
	endpoint.cpp 
	json.cpp 
	persistent.cpp 
	types.cpp 
	siptypes.cpp 
	call.cpp 
	presence.cpp 
	media.cpp
)

list(TRANSFORM SRC PREPEND "src/pjsua2/")
add_library(${PROJECT_NAME} ${SRC})
add_library(PJSIP::${PROJECT_NAME} ALIAS ${PROJECT_NAME})
target_include_directories(${PROJECT_NAME} PUBLIC "$<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}>$<INSTALL_INTERFACE:${CMAKE_INSTALL_INCLUDEDIR}>/include")

target_link_libraries(${PROJECT_NAME} PJSIP::PJSIP_UA PJSIP::PJSIP_SIMPLE PJSIP::PJSIP PJSIP::PJMEDIA_AUDIODEV PJSIP::PJMEDIA_VIDEODEV PJSIP::PJMEDIA_CODEC PJSIP::PJMEDIA PJSIP::PJNATH PJSIP::PJLIB_UTIL PJSIP::PJLIB)

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