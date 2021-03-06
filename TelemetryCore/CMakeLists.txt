cmake_minimum_required(VERSION 3.6)

find_library( log-lib
              log )
include_directories(libs/other)

##########################################################################################################
#TelemetryReceiver
##########################################################################################################
set(TELEMETRY_PATH src/main/cpp/Telemetry)
####################
#C-Files
####################
add_library( ltm_frsky_mavlink_smartport
        SHARED
        ${TELEMETRY_PATH}/cFiles/ltm.c
        ${TELEMETRY_PATH}/cFiles/frsky.c
        ${TELEMETRY_PATH}/cFiles/mavlink2.c
        ${TELEMETRY_PATH}/cFiles/smartport.c
        )
target_link_libraries(ltm_frsky_mavlink_smartport
        ${log-lib}
        android
        log)
#######################################################
include_directories(${TELEMETRY_PATH}/InputOutput)
add_library( TelemetryReceiver
        SHARED
        ${TELEMETRY_PATH}/TelemetryReceiver.cpp
        ${TELEMETRY_PATH}/InputOutput/FileReader.cpp
        ${TELEMETRY_PATH}/InputOutput/UDPReceiver.cpp
        )
target_link_libraries(TelemetryReceiver
        ${log-lib}
        android
        log
        mediandk
        ltm_frsky_mavlink_smartport)

set_target_properties(TelemetryReceiver
        PROPERTIES
        LIBRARY_OUTPUT_DIRECTORY "../../../../jniLibs/${ANDROID_ABI}")


