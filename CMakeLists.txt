cmake_minimum_required(VERSION 2.8.3)
project(serial)



## Sources
set(serial_SRCS
    src/serial.cc
    include/serial/serial.h
    include/serial/v8stdint.h
    src/impl/unix.cc
    src/impl/list_ports/list_ports_linux.cc
)


## Add serial library
add_library(${PROJECT_NAME} ${serial_SRCS})
target_link_libraries(${PROJECT_NAME} rt pthread)

## Uncomment for example
add_executable(serial_example examples/serial_example.cc)
add_dependencies(serial_example ${PROJECT_NAME})
target_link_libraries(serial_example ${PROJECT_NAME})

## Include headers
include_directories(include)

## Install executable
install(TARGETS ${PROJECT_NAME} EXPORT serial-targets
    ARCHIVE DESTINATION lib
    LIBRARY DESTINATION lib
    RUNTIME DESTINATION bin
)

## Install headers
install(FILES include/serial/serial.h include/serial/v8stdint.h
  DESTINATION include/serial)

# Export the library interface
install(
	EXPORT serial-targets
	DESTINATION lib/cmake/serial
)

install(
	FILES serial-config.cmake
	DESTINATION lib/cmake/serial
)
