cmake_minimum_required(VERSION 3.4.1)

PROJECT(crypto-aes)

AUX_SOURCE_DIRECTORY(src/main/cpp/modes SRC_LIST)
AUX_SOURCE_DIRECTORY(src/main/cpp/aes SRC_LIST)
SET(SRC_LIST
        ${SRC_LIST}
        src/main/cpp/aes.h
        src/main/cpp/modes.h
        src/main/cpp/modes_int.h
        src/main/cpp/aes_core.c)


SET_TARGET_PROPERTIES(${SHARED_LIBRARY_NAME}
        PROPERTIES
        LIBRARY_OUTPUT_DIRECTORY "libs/${ANDROID_ABI}")


IF (${ANDROID_ABI} MATCHES "armeabi-v7a")
    MESSAGE("building armeabi-v7a        ------------------------")
    SET(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -DLIBYUV_NEON")
    SET(SRC_LIST
            ${SRC_LIST}
            src/main/cpp/aes_core.c
            )
ELSEIF (${ANDROID_ABI} MATCHES "arm64-v8a")
    MESSAGE("building arm64-v8a        ------------------------")
    SET(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -DLIBYUV_NEON")
    SET(SRC_LIST
            ${SRC_LIST}
            src/main/cpp/aes_core.c
            )
ELSEIF (${ANDROID_ABI} MATCHES "x86" OR ${ANDROID_ABI} MATCHES "x86_64")
    MESSAGE("building x86 or  x86_64   ------------------------")
    SET(SRC_LIST
            ${SRC_LIST}
            )
ENDIF ()

find_library(log-lib log)

ADD_LIBRARY(crypto-aes SHARED ${SRC_LIST})

TARGET_LINK_LIBRARIES(crypto-aes ${log-lib})