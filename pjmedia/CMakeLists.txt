project(PJMEDIA)

set(SRC
	alaw_ulaw.c
	alaw_ulaw_table.c
	avi_player.c
	bidirectional.c
	clock_thread.c
	codec.c
	conference.c
	conf_switch.c
	converter.c
	converter_libswscale.c
	converter_libyuv.c
	delaybuf.c
	echo_common.c
	echo_port.c
	echo_suppress.c
	echo_webrtc.c
	echo_webrtc_aec3.cpp
	endpoint.c
	errno.c
	event.c
	format.c
	ffmpeg_util.c
	g711.c
	jbuf.c
	master_port.c
	mem_capture.c
	mem_player.c
	null_port.c
	plc_common.c
	port.c
	splitcomb.c
	resample_resample.c
	resample_libsamplerate.c
	resample_speex.c
	resample_port.c
	rtcp.c
	rtcp_xr.c
	rtcp_fb.c
	rtp.c
	sdp.c
	sdp_cmp.c
	sdp_neg.c
	session.c
	silencedet.c
	sound_legacy.c
	sound_port.c
	stereo_port.c
	stream_common.c
	stream.c
	stream_info.c
	tonegen.c
	transport_adapter_sample.c
	transport_ice.c
	transport_loop.c
	transport_srtp.c
	transport_udp.c
	types.c
	vid_codec.c
	vid_codec_util.c
	vid_port.c
	vid_stream.c
	vid_stream_info.c
	vid_conf.c
	wav_player.c
	wav_playlist.c
	wav_writer.c
	wave.c
	wsola.c
	audiodev.c
	videodev.c
)

list(TRANSFORM SRC PREPEND "src/pjmedia/")
add_library(${PROJECT_NAME} ${SRC})
add_library(PJSIP::${PROJECT_NAME} ALIAS ${PROJECT_NAME})
target_include_directories(${PROJECT_NAME} PUBLIC "$<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}>$<INSTALL_INTERFACE:${CMAKE_INSTALL_INCLUDEDIR}>/include" PRIVATE $<BUILD_INTERFACE:${CMAKE_SOURCE_DIR}>)

target_link_libraries(${PROJECT_NAME} PJSIP::PJNATH PJSIP::PJLIB_UTIL PJSIP::SRTP)

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

project(PJMEDIA_AUDIODEV)

set(SRC
	audiodev.c 
	audiotest.c 
	errno.c 
	legacy_dev.c 
	null_dev.c 
	pa_dev.c 
	wmme_dev.c 
	alsa_dev.c 
	bb10_dev.c 
	bdimad_dev.c 
	android_jni_dev.c 
	opensl_dev.c 
	oboe_dev.cpp
)

list(TRANSFORM SRC PREPEND "src/pjmedia-audiodev/")
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

project(PJMEDIA_VIDEODEV)

set(SRC
	videodev.c 
	errno.c 
	avi_dev.c 
	ffmpeg_dev.c 
	colorbar_dev.c 
	v4l2_dev.c 
	opengl_dev.c 
	util.c
)

list(TRANSFORM SRC PREPEND "src/pjmedia-videodev/")
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


project(PJSDP)

set(SRC
	errno.c 
	sdp.c 
	sdp_cmp.c 
	sdp_neg.c
)

list(TRANSFORM SRC PREPEND "src/pjmedia/")
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

project(PJMEDIA_CODEC)

set(SRC
	audio_codecs.c 
	ffmpeg_vid_codecs.c 
	openh264.cpp
	h263_packetizer.c 
	h264_packetizer.c 
	vpx_packetizer.c 
	ipp_codecs.c 
	silk.c 
	opus.c
	g7221_sdp_match.c 
	amr_sdp_match.c 
	passthrough.c 
	vpx.c
)

list(TRANSFORM SRC PREPEND "src/pjmedia-codec/")
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