project(PJNATH)

set(SRC
    errno.c 
    ice_session.c 
    ice_strans.c 
    nat_detect.c 
    stun_auth.c
    stun_msg.c 
    stun_msg_dump.c 
    stun_session.c 
    stun_sock.c
    stun_transaction.c 
    turn_session.c 
    turn_sock.c 
    upnp.c
)

list(TRANSFORM SRC PREPEND "src/pjnath/")
add_library(${PROJECT_NAME} ${SRC})
add_library(PJSIP::${PROJECT_NAME} ALIAS ${PROJECT_NAME})
target_include_directories(${PROJECT_NAME} PUBLIC "$<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}>$<INSTALL_INTERFACE:${CMAKE_INSTALL_INCLUDEDIR}>/include")

target_link_libraries(${PROJECT_NAME} PJSIP::PJLIB PJSIP::PJLIB_UTIL)

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