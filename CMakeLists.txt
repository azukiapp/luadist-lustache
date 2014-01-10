# Copyright (C) 2007-2013 LuaDist.
# Created by Peter Drahoš
# Redistribution and use of this file is allowed according to the terms of the MIT license.
# For details see the COPYRIGHT file distributed with LuaDist.
# Please note that the package source code is licensed under its own license.

cmake_minimum_required ( VERSION 2.8 )
include ( cmake/dist.cmake )
project ( ${DIST_NAME} NONE )
include ( lua )

set ( GIT_ARCHIVE_URL ${DIST_REPOSITORY}/archive )
set ( SOURCE_FILE_NAME v${DIST_VERSION}.tar.gz )
set ( SOURCE_URL ${GIT_ARCHIVE_URL}/${SOURCE_FILE_NAME} )
set ( SOURCE_FILE ${CMAKE_CURRENT_BINARY_DIR}/${SOURCE_FILE_NAME} )
set ( SOURCE_DIR ${CMAKE_CURRENT_BINARY_DIR}/${DIST_NAME}-${DIST_VERSION} )

if ( NOT EXISTS "${SOURCE_FILE}" )
  message ( "${SOURCE_URL}" )
  file( DOWNLOAD "${SOURCE_URL}" "${SOURCE_FILE}" )
endif ()

add_custom_command (
  OUTPUT ${SOURCE_DIR}/src/lustache.lua
  COMMAND tar -xzvf ${SOURCE_FILE}
  DEPENDS ${SOURCE_FILE}
)

add_custom_target( lustache ALL DEPENDS ${SOURCE_DIR}/src/lustache.lua )

install_lua_module ( lustache.init ${SOURCE_DIR}/src/lustache.lua )
install_lua_module ( lustache.context ${SOURCE_DIR}/src/lustache/context.lua )
install_lua_module ( lustache.renderer ${SOURCE_DIR}/src/lustache/renderer.lua )
install_lua_module ( lustache.scanner ${SOURCE_DIR}/src/lustache/scanner.lua )

install_data ( ${SOURCE_DIR}/README.md ${SOURCE_DIR}/LICENSE )
