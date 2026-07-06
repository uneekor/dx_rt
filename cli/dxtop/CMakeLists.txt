# DXTOP
set(target dxtop)
file(GLOB_RECURSE srcs "*.cpp" "*.cc" "*.c")

add_executable(${target} ${srcs})

if (MSVC)
   set(CURSES_INCLUDE_DIR ${CMAKE_SOURCE_DIR}/extern/windows_pdcurses)
 # Find ncurses package
else()
  find_package(Curses REQUIRED)
endif()

target_include_directories(${target} PUBLIC
    ${CMAKE_SOURCE_DIR}/lib/include
    ${CMAKE_SOURCE_DIR}/extern/include
    ${CURSES_INCLUDE_DIR}
  )
target_include_directories(${target} PRIVATE
    ${DXRT_INTERNAL_INCLUDE_DIRS}
    ${CMAKE_SOURCE_DIR}/lib/device_shm
  )
if (MSVC)
    target_link_libraries(${target} PRIVATE user32 advapi32 dxrt pdcurses ${link_libs})
    target_link_options(${target} PRIVATE "/SUBSYSTEM:CONSOLE")
else()
    target_link_libraries(${target} dxrt_static pthread rt ${link_libs} ${CURSES_LIBRARIES})
endif()

install( TARGETS ${target} DESTINATION bin )

