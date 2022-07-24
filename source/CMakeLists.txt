cmake_minimum_required(VERSION 3.20)

# Keep this options before project() command
# Commands to generate warnings in case of in-source builds.
set(CMAKE_DISABLE_SOURCE_CHANGES ON)
set(CMAKE_DISABLE_IN_SOURCE_BUILD ON)

# Don't move of position this command
project(traffic-light-stm32f4 VERSION 0.1.0)

# add_subdirectory(traffic-light-stm32-cube-ide)
# add_subdirectory(traffic-light-code)

# Keep this options after project() command
enable_language(C ASM)


set(CMAKE_C_STANDARD 17)
set(CMAKE_C_STANDARD_REQUIRED ON)               # Define explicitly to use of the standard C11 declared in the previous line
set(CMAKE_C_EXTENSIONS OFF)                     # Disabling vendor-specific extensions

set(CMAKE_CXX_STANDARD 20)
set(CMAKE_CXX_STANDARD_REQUIRED ON)             # Define explicitly to use of the standard C++17 declared in the previous line
set(CMAKE_CXX_EXTENSIONS OFF)                   # Disabling vendor-specific extensions

set(SOURCE_PATH "traffic-light-stm32-cube-ide") # Code produced by STM32CubeIDE

set(STM32CUBEMX_GENERATED_FILES

        ${SOURCE_PATH}/Drivers/STM32F4xx_HAL_Driver/Src/stm32f4xx_hal.c
        ${SOURCE_PATH}/Drivers/STM32F4xx_HAL_Driver/Src/stm32f4xx_hal_cortex.c
        ${SOURCE_PATH}/Drivers/STM32F4xx_HAL_Driver/Src/stm32f4xx_hal_dma.c
        ${SOURCE_PATH}/Drivers/STM32F4xx_HAL_Driver/Src/stm32f4xx_hal_dma_ex.c
        ${SOURCE_PATH}/Drivers/STM32F4xx_HAL_Driver/Src/stm32f4xx_hal_exti.c
        ${SOURCE_PATH}/Drivers/STM32F4xx_HAL_Driver/Src/stm32f4xx_hal_flash.c
        ${SOURCE_PATH}/Drivers/STM32F4xx_HAL_Driver/Src/stm32f4xx_hal_flash_ex.c
        ${SOURCE_PATH}/Drivers/STM32F4xx_HAL_Driver/Src/stm32f4xx_hal_flash_ramfunc.c
        ${SOURCE_PATH}/Drivers/STM32F4xx_HAL_Driver/Src/stm32f4xx_hal_gpio.c
        ${SOURCE_PATH}/Drivers/STM32F4xx_HAL_Driver/Src/stm32f4xx_hal_pcd.c
        ${SOURCE_PATH}/Drivers/STM32F4xx_HAL_Driver/Src/stm32f4xx_hal_pcd_ex.c
        ${SOURCE_PATH}/Drivers/STM32F4xx_HAL_Driver/Src/stm32f4xx_hal_pwr.c
        ${SOURCE_PATH}/Drivers/STM32F4xx_HAL_Driver/Src/stm32f4xx_hal_pwr_ex.c
        ${SOURCE_PATH}/Drivers/STM32F4xx_HAL_Driver/Src/stm32f4xx_hal_rcc.c
        ${SOURCE_PATH}/Drivers/STM32F4xx_HAL_Driver/Src/stm32f4xx_hal_rcc_ex.c
        ${SOURCE_PATH}/Drivers/STM32F4xx_HAL_Driver/Src/stm32f4xx_hal_tim.c
        ${SOURCE_PATH}/Drivers/STM32F4xx_HAL_Driver/Src/stm32f4xx_hal_tim_ex.c
        ${SOURCE_PATH}/Drivers/STM32F4xx_HAL_Driver/Src/stm32f4xx_hal_uart.c
        ${SOURCE_PATH}/Drivers/STM32F4xx_HAL_Driver/Src/stm32f4xx_ll_usb.c

        ${SOURCE_PATH}/Core/Inc/main.h
        ${SOURCE_PATH}/Core/Inc/stm32f4xx_hal_conf.h
        ${SOURCE_PATH}/Core/Inc/stm32f4xx_it.h

        ${SOURCE_PATH}/Core/Src/main.cpp
        ${SOURCE_PATH}/Core/Src/stm32f4xx_hal_msp.c
        ${SOURCE_PATH}/Core/Src/stm32f4xx_it.c
        ${SOURCE_PATH}/Core/Src/syscalls.c
        ${SOURCE_PATH}/Core/Src/sysmem.c
        ${SOURCE_PATH}/Core/Src/system_stm32f4xx.c

        ${SOURCE_PATH}/Core/Startup/startup_stm32f446zetx.s)

# Add a file properties to avoid the unused parameter error in the 3rd files
set_source_files_properties(
        ${SOURCE_PATH}/Core/Src/main.cpp
        PROPERTIES
        COMPILE_FLAGS -Wno-missing-field-initializers
        )

set_source_files_properties(
        ${SOURCE_PATH}/Core/Src/syscalls.c
        ${SOURCE_PATH}/Drivers/STM32F4xx_HAL_Driver/Src/stm32f4xx_hal_exti.c
        ${SOURCE_PATH}/Drivers/STM32F4xx_HAL_Driver/Src/stm32f4xx_hal_flash_ex.c
        ${SOURCE_PATH}/Drivers/STM32F4xx_HAL_Driver/Src/stm32f4xx_hal_pwr.c
        PROPERTIES
        COMPILE_FLAGS -Wno-unused-parameter
        )

set(EXECUTABLE ${PROJECT_NAME}.out)

add_executable(${EXECUTABLE} ${STM32CUBEMX_GENERATED_FILES})

target_compile_definitions(${EXECUTABLE} PRIVATE
        -D USE_HAL_DRIVER
        -D STM32F446xx
        )

target_include_directories(${EXECUTABLE} PRIVATE
        ${SOURCE_PATH}/Core/Inc
        ${SOURCE_PATH}/Drivers/CMSIS/Include
        ${SOURCE_PATH}/Drivers/CMSIS/Device/ST/STM32F4xx/Include
        ${SOURCE_PATH}/Drivers/STM32F4xx_HAL_Driver/Inc
        )

target_compile_options(${EXECUTABLE} PRIVATE -H # This -H option show each path that are used to include a requested header
        -mcpu=cortex-m4
        -mthumb
        -mfpu=fpv4-sp-d16
        -mfloat-abi=hard
        
        -fdata-sections                         # Place data in their own sections
        -ffunction-sections                     # Place functions in their own sections
        -fstack-usage
        -specs=nano.specs

        -Wall                                   # Enable all warnings about questionable constructions
        -Wextra                                 # Enable extra warnings
        -pedantic                               # Issue all warnings demanded by strict ISO and ISO C++

        # C++ options
        # Generator expressions  # $<EXPRESSION:arg1,arg2,arg3>
        $<$<COMPILE_LANGUAGE:CXX>:-fno-exceptions>      # Disable handling exceptions
        $<$<COMPILE_LANGUAGE:CXX>:-fno-rtti>            # Disable generation of information about every class with virtual functions
        $<$<COMPILE_LANGUAGE:CXX>:-fno-use-cxa-atexit>  # Do not use __cxa_atexit for registering static destructors

        $<$<CONFIG:Debug>:-O0 -g3>
        )

target_link_options(${EXECUTABLE} PRIVATE
        -T${CMAKE_SOURCE_DIR}/${SOURCE_PATH}/STM32F446ZETX_FLASH.ld
        -mcpu=cortex-m4
        -mthumb
        -mfpu=fpv4-sp-d16
        -mfloat-abi=hard
        -specs=nano.specs                       # Firmware has no syscall implementation
        -specs=nosys.specs
        -static
        -Wl,--start-group -lc -lm -lstdc++ -lsupc++ -Wl,--end-group
        #-lnosys                                 # syscalls are empty stubs
        -Wl,-Map=${PROJECT_NAME}.map,--cref     # Generate map file
        -Wl,--gc-sections                       # Discard unused sections
        -Wl,--print-memory-usage
        )
        
# Print executable size
add_custom_command(TARGET ${EXECUTABLE}
        POST_BUILD
        COMMAND arm-none-eabi-size ${EXECUTABLE})

# Create .hex and .bin file
add_custom_command(TARGET ${EXECUTABLE}
        POST_BUILD
        COMMAND arm-none-eabi-objcopy -O ihex ${EXECUTABLE} ${PROJECT_NAME}.hex
        COMMAND arm-none-eabi-objcopy -O binary ${EXECUTABLE} ${PROJECT_NAME}.bin
        COMMAND arm-none-eabi-objdump -h -S  ${EXECUTABLE} > ${PROJECT_NAME}.list)

# Improve clean target
set_target_properties(${EXECUTABLE} PROPERTIES ADDITIONAL_CLEAN_FILES
"${PROJECT_NAME}.bin;${PROJECT_NAME}.hex;${PROJECT_NAME}.map;${PROJECT_NAME}.list")