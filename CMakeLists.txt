cmake_minimum_required(VERSION 3.15)

project(RATS)

find_package(EXPAT REQUIRED)


if(WIN32)

  find_library(EXPAT_DLL NAMES libexpat.dll REQUIRED)

  set(CMAKE_INSTALL_SYSTEM_RUNTIME_LIBS 
    ${EXPAT_DLL}
  )


endif()

set(CMAKE_INSTALL_SYSTEM_RUNTIME_COMPONENT Runtime)
set(CMAKE_INSTALL_SYSTEM_RUNTIME_DESTINATION ${CMAKE_INSTALL_BINDIR})
set(CMAKE_INSTALL_SYSTEM_RUNTIME_LIBS_NO_WARNINGS TRUE)

if(MSVC)
  set(CMAKE_INSTALL_SYSTEM_RUNTIME_DESTINATION 
  #${CMAKE_INSTALL_BINDIR}/${CMAKE_BUILD_TYPE}
  )
endif()

if(DEBUG)
  set(CMAKE_INSTALL_DEBUG_LIBRARIES TRUE)
endif()
include(InstallRequiredSystemLibraries)


add_executable(rats 
    engine.c
    getopt.c
    hash.c
    kazhash.c
    lex.yyc.c
    lex.yyp.c
    lex.yyperl.c
    lex.yyphp.c
    lex.yyruby.c
    main.c
    report.c
    vuln_db.c
)


set(DATADIR "share/rats" CACHE STRING "Data directory")

target_compile_definitions(rats PRIVATE 
  -DHAVE_EXPAT_H
  -DYY_NO_UNISTD_H
  )

target_link_libraries(rats PRIVATE EXPAT::EXPAT)

install(TARGETS rats DESTINATION bin)
install(FILES rats-c.xml rats-openssl.xml rats-perl.xml rats-python.xml rats-ruby.xml DESTINATION "${DATADIR}")


include(cpack)