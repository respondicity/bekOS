

add_executable(kernel boot.S mm.S kernel.cpp peripherals/peripherals.cpp peripherals/gentimer.cpp printf.c peripherals/uart.cpp)
target_include_directories(kernel PRIVATE ${freestanding_include_directories} ${CMAKE_CURRENT_SOURCE_DIR}/../include)
target_compile_options(kernel PRIVATE -ffreestanding -nostdinc -nostdlib -nostartfiles -Wall -Wextra -fno-exceptions -fno-rtti)
set_target_properties(kernel PROPERTIES
        LINK_FLAGS "-nostdlib -nostartfiles -T ${CMAKE_SOURCE_DIR}/kernel-link.ld -pie -lgcc"
        LINK_DEPENDS "${CMAKE_SOURCE_DIR}/kernel-link.ld"
        SUFFIX ".elf")

add_custom_command(TARGET kernel POST_BUILD
        COMMAND "${objcopy_location}" "${CMAKE_CURRENT_BINARY_DIR}/kernel.elf" "-O" "binary" "${CMAKE_BINARY_DIR}/kernel8.img"
        VERBATIM)
