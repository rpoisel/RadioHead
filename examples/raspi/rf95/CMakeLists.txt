cmake_minimum_required(VERSION 3.6)
project(rf95)

set(RADIOHEADBASE ${CMAKE_CURRENT_LIST_DIR}/../../..)

add_library(bcm2835 INTERFACE)

find_library(BCM2835 name bcm2835 PATHS /tmp/libbcm2835-1.57/lib NO_DEFAULT_PATH)
target_link_libraries(bcm2835 INTERFACE
  ${BCM2835}
)

target_include_directories(bcm2835 INTERFACE
  /tmp/libbcm2835-1.57/include
)

add_library(RadioHead STATIC
  ${RADIOHEADBASE}/RHutil/RasPi.cpp
  ${RADIOHEADBASE}/RH_RF95.cpp
  ${RADIOHEADBASE}/RHDatagram.cpp
  ${RADIOHEADBASE}/RHHardwareSPI.cpp
  ${RADIOHEADBASE}/RHSPIDriver.cpp
  ${RADIOHEADBASE}/RHGenericDriver.cpp
  ${RADIOHEADBASE}/RHGenericSPI.cpp
)

target_compile_definitions(RadioHead PUBLIC
  RASPBERRY_PI
  BCM2835_NO_DELAY_COMPATIBILITY
  __BASEFILE__=""
)

target_link_libraries(RadioHead PUBLIC
  bcm2835
)

target_include_directories(RadioHead PUBLIC
  ${RADIOHEADBASE}
)

add_executable(rf95_client
  rf95_client.cpp
)

target_link_libraries(rf95_client PUBLIC
  RadioHead
)

add_executable(rf95_server
  rf95_server.cpp
)

target_link_libraries(rf95_server PUBLIC
  RadioHead
)
