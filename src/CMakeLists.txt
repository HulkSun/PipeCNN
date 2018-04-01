include_directories(/home/sh/intelFPGA_pro/16.1/hld/host/include)

add_definitions(-DFPGA_DEVICE -std=c++11 -Wall -g)
link_directories(/home/sh/intelFPGA_pro/16.1/hld/board/de5a_net_i2/linux64/lib /home/sh/intelFPGA_pro/16.1/hld/host/linux64/lib)
add_executable(${PROJECT_NAME} main.cpp layer_config.h common/ocl_util.cpp common/timer.cpp)

target_link_libraries(${PROJECT_NAME} alteracl)
target_link_libraries(${PROJECT_NAME} terasic_a10_mmd)
target_link_libraries(${PROJECT_NAME} elf)
