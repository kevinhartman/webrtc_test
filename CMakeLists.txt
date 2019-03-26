cmake_minimum_required(VERSION 3.10)
project(webrtc_test)

set(CMAKE_CXX_STANDARD 11)
add_definitions("-fno-rtti -fPIC -DWEBRTC_POSIX -DWEBRTC_MAC -DJSONCPP_RELATIVE_PATH")

find_package(Qt5 COMPONENTS Core Widgets Quick REQUIRED)

include_directories(.)
include_directories(backend)
include_directories(../webrtc-checkout/src/)
include_directories(../webrtc-checkout/src/third_party/libyuv/include)
include_directories(../webrtc-checkout/src/third_party/abseil-cpp/)
include_directories(../webrtc-checkout/src/third_party/jsoncpp/source/include)

add_executable(webrtc_test
        backend/conductor.cc
        backend/conductor.h
        backend/defaults.cc
        backend/defaults.h
        backend/flag_defs.h
        backend/main_wnd.h
        backend/peer_connection_client.cc
        backend/peer_connection_client.h
        main.cc
        main_wnd.cc
        main_wnd.h)



#link_directories(../webrtc-checkout/src/out/Release/obj/)

find_library(WEBRTC_LIB webrtc)

target_link_libraries(webrtc_test
        "${PORTAUDIO_LIB}"
        "${WEBRTC_LIB}"
        "${AUDIO_LIB}"
        pthread
        dl
        Qt5::Widgets
        Qt5::Core
        Qt5::Gui)


if(APPLE)
    #file(GLOB_RECURSE FFMPEG_OBJ "${WebRTCOutDir}/../third_party/ffmpeg/ffmpeg_internal/*.o")
    find_library(FoundationLibs Foundation)
    find_library(CoreAudioLibs CoreAudio)
    find_library(AudioToolboxLibs AudioToolbox)
    find_library(CoreGraphicsLibs CoreGraphics)
    find_library(CoreVideoLibs CoreVideo)
    find_library(CoreMediaLibs CoreMedia)
    find_library(VideoToolboxLibs VideoToolbox)
    find_library(AVFoundationLibs AVFoundation)
    find_library(OpenGlLibs OpenGl)
    find_library(CocoaLibs Cocoa)

    target_link_libraries(${PROJECT_NAME}
            #${FFMPEG_OBJ}
            ${FoundationLibs}
            ${CoreAudioLibs}
            ${AudioToolboxLibs}
            ${CoreGraphicsLibs}
            ${CoreVideoLibs}
            ${CoreMediaLibs}
            ${CocoaLibs}
            #"${WebRTCOutDir}/../third_party/expat/libexpat.a"
            ${AVFoundationLibs}
            ${OpenGlLibs}
            )
endif()