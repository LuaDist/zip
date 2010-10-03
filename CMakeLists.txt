# Copyright (C) 2007-2009 LuaDist.
# Submitted by David Manura
# Redistribution and use of this file is allowed according to the terms of the MIT license.
# For details see the COPYRIGHT file distributed with LuaDist.
# Please note that the package source code is licensed under its own license.

# warning: compilation options might not be optical

PROJECT ( zip C )
CMAKE_MINIMUM_REQUIRED(VERSION 2.6)
INCLUDE ( dist.cmake )

SET ( SRC_ZIP
  crc32.c crypt.c deflate.c fileio.c globals.c trees.c ttyio.c
  util.c zip.c zipfile.c zipup.c )
IF ( WIN32 AND NOT CYGWIN )
  SET ( SRC_ZIP ${SRC_ZIP}
        win32/nt.c win32/win32.c win32/win32i64.c win32/win32zip.c )
  ADD_DEFINITIONS(-DWIN32 -DNO_ASM)
ELSE ( WIN32 AND NOT CYGWIN )
  SET ( SRC_ZIP ${SRC_ZIP} unix/unix.c )
  ADD_DEFINITIONS(-DUNIX)
  ADD_DEFINITIONS(-DNO_OFF_T) # simpler
ENDIF ( WIN32 AND NOT CYGWIN )

INCLUDE_DIRECTORIES(.)

INCLUDE(CheckFunctionExists)
CHECK_FUNCTION_EXISTS(lchmod HAVE_LCHMOD)
IF ( NOT HAVE_LCHMOD )
  ADD_DEFINITIONS(-DNO_LCHMOD)
ENDIF ( NOT HAVE_LCHMOD )

ADD_EXECUTABLE ( zip ${SRC_ZIP} )

INSTALL ( TARGETS zip RUNTIME DESTINATION ${INSTALL_BIN} )
INSTALL ( FILES README LICENSE DESTINATION ${INSTALL_DATA} )
