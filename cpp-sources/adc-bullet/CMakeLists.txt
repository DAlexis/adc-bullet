cmake_minimum_required(VERSION 2.8)

project(adc-bullet C CXX ASM)

set(EXE_SOURCES

)

include_directories(
    include
)

set(ELF_NAME ${PROJECT_NAME}.elf)
set(BIN_NAME ${PROJECT_NAME}.bin)
set(MAP_NAME ${PROJECT_NAME}.map)

add_definitions(-DGLCD_DEVICE_STM32F10X)
add_definitions(-DGLCD_CONTROLLER_PCD8544)
add_definitions(-DUSE_STDPERIPH_DRIVER)
add_definitions(-DUSE_STDPERIPH_SDCARD)

set(LDSCRIPTS "-T STM32F103RETx_FLASH.ld")
set(LDSCRIPTS_DIR "${PROJECT_SOURCE_DIR}/ldscripts")

set(CMAKE_EXE_LINKER_FLAGS "${CMAKE_EXE_LINKER_FLAGS} ${LDSCRIPTS} -Xlinker --gc-sections -L${LDSCRIPTS_DIR} -Wl,-Map,\"${MAP_NAME}\" --specs=nano.specs")

add_executable(${ELF_NAME} ${EXE_SOURCES})

add_custom_command(TARGET ${ELF_NAME}
                   POST_BUILD
                   COMMAND arm-none-eabi-objcopy ARGS -O binary ${ELF_NAME}  ${BIN_NAME}
                   )

