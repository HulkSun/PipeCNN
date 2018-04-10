
# hw | sw_emu
set(FLOW hw)

include_directories(/home/sh/intelFPGA_pro/16.1/hld/host/include)

if(${FLOW} STREQUAL "sw_emu")
    add_definitions(-DFPGA_DEVICE -std=c++11 -g -DSW_EMU)
else(${FLOW} STREQUAL "sw_emu")
    add_definitions(-DFPGA_DEVICE -std=c++11 -Wall -g)
endif(${FLOW} STREQUAL "sw_emu")


link_directories(/home/sh/intelFPGA_pro/16.1/hld/board/de5a_net_i2/linux64/lib /home/sh/intelFPGA_pro/16.1/hld/host/linux64/lib)

add_executable(${PROJECT_NAME} main.cpp layer_config.h common/ocl_util.cpp common/timer.cpp)

target_link_libraries(${PROJECT_NAME} alteracl)
target_link_libraries(${PROJECT_NAME} terasic_a10_mmd)
target_link_libraries(${PROJECT_NAME} elf)

# compile IP core
add_custom_target(
    rtl
    ALL
    COMMAND aocl library hdl-comp-pkg ${CMAKE_CURRENT_SOURCE_DIR}/RTL/rtl_lib.xml -o rtl_lib.aoco
    COMMAND aocl library create -name rtl_lib rtl_lib.aoco
 )

 # compile kernel
 set(KERNEL_SRC ${CMAKE_CURRENT_SOURCE_DIR}/kernel/conv_pipe.cl)
 set(RTL_DIR ${CMAKE_BINARY_DIR}/src)
 set(KERNEL_OUTPUT ${CMAKE_BINARY_DIR}/bin/conv.aocx)
 
 if(${FLOW} STREQUAL "sw_emu")
     set(OCC_FLAGS -v --report -march=emulator  -L ${RTL_DIR} -l rtl_lib.aoclib)
 else(${FLOW} STREQUAL "sw_emu")
     set(OCC_FLAGS -v --report --profile  -L ${RTL_DIR} -l rtl_lib.aoclib)
 endif(${FLOW} STREQUAL "sw_emu")
 
 add_custom_target(
     fpga
    #  ALL
     COMMAND aoc ${KERNEL_SRC} ${OCC_FLAGS} -o ${KERNEL_OUTPUT}    
 )