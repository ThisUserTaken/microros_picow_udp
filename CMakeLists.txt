cmake_minimum_required(VERSION 3.12)

include($ENV{PICO_SDK_PATH}/external/pico_sdk_import.cmake)

project(udp_micro_ros_raspberrypi_pico_sdk C CXX ASM)
set(CMAKE_C_STANDARD 11)
set(CMAKE_CXX_STANDARD 17)
set(PICO_BOARD pico_w CACHE STRING "Board type")

pico_sdk_init()

link_directories(libmicroros)
add_executable(udp_micro_ros_raspberrypi_pico_sdk
    udp_micro_ros_raspberrypi_pico_sdk.c
    pico_udp_transport.c
)
target_link_libraries(udp_micro_ros_raspberrypi_pico_sdk
    pico_stdlib
    #pico_cyw43_arch_none
    pico_cyw43_arch_lwip_threadsafe_background
    #adc_read
    hardware_adc
    hardware_pwm
    microros
    #pico_cyw43_arch_lwip_poll 
    pico_lwip
    #lwip
)
target_include_directories(udp_micro_ros_raspberrypi_pico_sdk PRIVATE
        ${CMAKE_CURRENT_LIST_DIR}
        ${CMAKE_CURRENT_LIST_DIR}/.. # for our common lwipopts
        )
target_include_directories(udp_micro_ros_raspberrypi_pico_sdk PUBLIC
    libmicroros/include
)
include_directories( $ENV{PICO_EXTRAS_PATH}/lib/lwip/src/include
                     $ENV{PICO_EXTRAS_PATH}/src/rp2_common/lwip/include
                      )

SET(CMAKE_C_FLAGS  "${CMAKE_C_FLAGS} -ffunction-sections -fdata-sections")
SET(CMAKE_CXX_FLAGS  "${CMAKE_CXX_FLAGS} -ffunction-sections -fdata-sections")

# Configure Pico
pico_enable_stdio_usb(udp_micro_ros_raspberrypi_pico_sdk 1)
pico_enable_stdio_uart(udp_micro_ros_raspberrypi_pico_sdk 0)
add_compile_definitions(PICO_UART_ENABLE_CRLF_SUPPORT=0)
add_compile_definitions(PICO_STDIO_ENABLE_CRLF_SUPPORT=0)
add_compile_definitions(PICO_STDIO_DEFAULT_CRLF=0)

# Generate UF2
pico_add_extra_outputs(udp_micro_ros_raspberrypi_pico_sdk)


