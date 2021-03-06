FIND_PACKAGE (PythonLibs 2 )
FIND_PACKAGE (PythonInterp 2 REQUIRED)
EXECUTE_PROCESS(COMMAND ${PYTHON_EXECUTABLE} -c "from sys import stdout; from distutils import sysconfig; stdout.write(sysconfig.get_python_lib(True))" OUTPUT_VARIABLE PYTHON_INSTALL_DIR)
INCLUDE_DIRECTORIES (${PYTHON_INCLUDE_PATH})

MESSAGE(STATUS "Python install dir is ${PYTHON_INSTALL_DIR}")

set (CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -fno-strict-aliasing")
set (CMAKE_C_FLAGS_DEBUG "${CMAKE_C_FLAGS_DEBUG} -fno-strict-aliasing")
set (CMAKE_C_FLAGS_RELEASE "${CMAKE_C_FLAGS_RELEASE} -fno-strict-aliasing")

SET (librepo_COPIES ${pylibrepo_SRCDIR}/__init__.py)
FILE(COPY ${librepo_COPIES} DESTINATION ./librepo/)

ADD_LIBRARY(_librepomodule SHARED ${librepomodule_SRCS})
SET_TARGET_PROPERTIES(_librepomodule PROPERTIES PREFIX "")
SET_TARGET_PROPERTIES(_librepomodule PROPERTIES LIBRARY_OUTPUT_DIRECTORY "./librepo")
TARGET_LINK_LIBRARIES(_librepomodule librepo)
TARGET_LINK_LIBRARIES(_librepomodule
                        ${EXPAT_LIBRARIES}
                        ${CURL_LIBRARY}
                        ${PYTHON_LIBRARY}
                     )

INSTALL(FILES ${pylibrepo_SRCDIR}/__init__.py DESTINATION ${PYTHON_INSTALL_DIR}/librepo)
INSTALL(TARGETS _librepomodule LIBRARY DESTINATION ${PYTHON_INSTALL_DIR}/librepo)
