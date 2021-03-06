 ################################################################################
 #    Copyright (C) 2014 GSI Helmholtzzentrum fuer Schwerionenforschung GmbH    #
 #                                                                              #
 #              This software is distributed under the terms of the             #
 #              GNU Lesser General Public Licence (LGPL) version 3,             #
 #                  copied verbatim in the file "LICENSE"                       #
 ################################################################################

set(EX_BIN_DIR ${CMAKE_CURRENT_BINARY_DIR})
set(FAIRMQ_BIN_DIR ${CMAKE_BINARY_DIR}/fairmq)
configure_file(${CMAKE_CURRENT_SOURCE_DIR}/fairmq-start-ex-builtin-devices.sh.in ${CMAKE_CURRENT_BINARY_DIR}/fairmq-start-ex-builtin-devices.sh)

# test

configure_file(${CMAKE_CURRENT_SOURCE_DIR}/test-ex-builtin-devices.sh.in ${CMAKE_CURRENT_BINARY_DIR}/test-ex-builtin-devices.sh)

add_test(NAME Example-Builtin-Devices-zeromq COMMAND ${CMAKE_CURRENT_BINARY_DIR}/test-ex-builtin-devices.sh zeromq)
set_tests_properties(Example-Builtin-Devices-zeromq PROPERTIES TIMEOUT "30" RUN_SERIAL true PASS_REGULAR_EXPRESSION "Configured maximum number of iterations reached")

if(BUILD_NANOMSG_TRANSPORT)
  add_test(NAME Example-Builtin-Devices-nanomsg COMMAND ${CMAKE_CURRENT_BINARY_DIR}/test-ex-builtin-devices.sh nanomsg)
  set_tests_properties(Example-Builtin-Devices-nanomsg PROPERTIES TIMEOUT "30" RUN_SERIAL true PASS_REGULAR_EXPRESSION "Configured maximum number of iterations reached")
endif()

add_test(NAME Example-Builtin-Devices-shmem COMMAND ${CMAKE_CURRENT_BINARY_DIR}/test-ex-builtin-devices.sh shmem)
set_tests_properties(Example-Builtin-Devices-shmem PROPERTIES TIMEOUT "30" RUN_SERIAL true PASS_REGULAR_EXPRESSION "Configured maximum number of iterations reached")

add_test(NAME Example-Builtin-Devices-zeromq-multipart COMMAND ${CMAKE_CURRENT_BINARY_DIR}/test-ex-builtin-devices.sh zeromq true 2)
set_tests_properties(Example-Builtin-Devices-zeromq-multipart PROPERTIES TIMEOUT "30" RUN_SERIAL true PASS_REGULAR_EXPRESSION "Configured maximum number of iterations reached")

if(BUILD_NANOMSG_TRANSPORT)
  add_test(NAME Example-Builtin-Devices-nanomsg-multipart COMMAND ${CMAKE_CURRENT_BINARY_DIR}/test-ex-builtin-devices.sh nanomsg true 2)
  set_tests_properties(Example-Builtin-Devices-nanomsg-multipart PROPERTIES TIMEOUT "30" RUN_SERIAL true PASS_REGULAR_EXPRESSION "Configured maximum number of iterations reached")
endif()

add_test(NAME Example-Builtin-Devices-shmem-multipart COMMAND ${CMAKE_CURRENT_BINARY_DIR}/test-ex-builtin-devices.sh shmem true 2)
set_tests_properties(Example-Builtin-Devices-shmem-multipart PROPERTIES TIMEOUT "30" RUN_SERIAL true PASS_REGULAR_EXPRESSION "Configured maximum number of iterations reached")

# install

# configure run script with different executable paths for build and for install directories
set(EX_BIN_DIR ${CMAKE_INSTALL_PREFIX}/${PROJECT_INSTALL_BINDIR})
set(FAIRMQ_BIN_DIR ${CMAKE_INSTALL_PREFIX}/${PROJECT_INSTALL_BINDIR}/fairmq)
configure_file(${CMAKE_CURRENT_SOURCE_DIR}/fairmq-start-ex-builtin-devices.sh.in ${CMAKE_CURRENT_BINARY_DIR}/fairmq-start-ex-builtin-devices.sh_install)

install(
    PROGRAMS ${CMAKE_CURRENT_BINARY_DIR}/fairmq-start-ex-builtin-devices.sh_install
    DESTINATION ${PROJECT_INSTALL_BINDIR}
    RENAME fairmq-start-ex-builtin-devices.sh
)
