project(unixcoreconsole)

include_directories(../unixcoreruncommon)

add_compile_options(-fPIE)

set(CORECONSOLE_SOURCES 
    coreconsole.cpp 
)

add_executable(coreconsole
    ${CORECONSOLE_SOURCES}
)

# FreeBSD and NetBSD implement dlopen(3) in libc
if(NOT CMAKE_SYSTEM_NAME STREQUAL FreeBSD AND NOT CMAKE_SYSTEM_NAME STREQUAL NetBSD)
    target_link_libraries(coreconsole 
        dl
    )
endif(NOT CMAKE_SYSTEM_NAME STREQUAL FreeBSD AND NOT CMAKE_SYSTEM_NAME STREQUAL NetBSD)

# Libc turns locks into no-ops if pthread was not loaded into process yet. Loading
# pthread by the process executable ensures that all locks are initialized properly.
target_link_libraries(coreconsole
    unixcoreruncommon
    pthread
)

add_dependencies(coreconsole
    coreclr
)

install (TARGETS coreconsole DESTINATION .)
