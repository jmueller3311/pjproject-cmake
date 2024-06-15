project(SRTP)

set(SRC
	crypto/cipher/cipher.c
    crypto/cipher/null_cipher.c
    crypto/cipher/cipher_test_cases.c
    crypto/hash/null_auth.c
    crypto/hash/auth.c
    crypto/hash/auth_test_cases.c
    crypto/replay/rdb.c
    crypto/replay/rdbx.c
    crypto/math/datatypes.c
    pjlib/srtp_err.c
    crypto/kernel/crypto_kernel.c
    crypto/kernel/alloc.c
    crypto/kernel/key.c
    srtp/srtp.c
)

list(TRANSFORM SRC PREPEND "srtp/")
add_library(${PROJECT_NAME} ${SRC})
add_library(PJSIP::${PROJECT_NAME} ALIAS ${PROJECT_NAME})
target_include_directories(${PROJECT_NAME} PUBLIC 
    "$<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}>$<INSTALL_INTERFACE:${CMAKE_INSTALL_INCLUDEDIR}>/build/srtp" 
    "$<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}>$<INSTALL_INTERFACE:${CMAKE_INSTALL_INCLUDEDIR}>/srtp/include"
    "$<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}>$<INSTALL_INTERFACE:${CMAKE_INSTALL_INCLUDEDIR}>/srtp/crypto/include"
)

if(WIN32)
	target_compile_definitions(${PROJECT_NAME} PRIVATE HAVE_CONFIG_H)
endif()

target_link_libraries(${PROJECT_NAME} PJSIP::PJLIB)

file(GLOB_RECURSE hdrs RELATIVE CONFIGURE_DEPENDS 
    ${CMAKE_CURRENT_SOURCE_DIR}/build/srtp/*.hpp 
    ${CMAKE_CURRENT_SOURCE_DIR}/build/srtp/*.h
    ${CMAKE_CURRENT_SOURCE_DIR}/srtp/include/*.hpp 
    ${CMAKE_CURRENT_SOURCE_DIR}/srtp/include/*.h
    ${CMAKE_CURRENT_SOURCE_DIR}/srtp/crypto/include/*.hpp 
    ${CMAKE_CURRENT_SOURCE_DIR}/srtp/crypto/include/*.h
)
target_sources(${PROJECT_NAME} 
  PUBLIC FILE_SET HEADERS
  FILES ${hdrs}
)

string(TOLOWER ${PROJECT_NAME} pn_low)
install(TARGETS ${PROJECT_NAME} EXPORT ${pn_low}_config FILE_SET HEADERS)
install(EXPORT ${pn_low}_config
        NAMESPACE PJSIP::
        DESTINATION cmake
)