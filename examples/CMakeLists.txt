add_executable(SimpleExample simple.c)
target_link_libraries(SimpleExample RingBuffer)
add_test(NAME SimpleExample COMMAND SimpleExample)