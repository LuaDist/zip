# Copyright (C) 2007-2012 LuaDist.
# Submitted by David Manura
# Redistribution and use of this file is allowed according to the terms of the MIT license.
# For details see the COPYRIGHT file distributed with LuaDist.
# Please note that the package source code is licensed under its own license.

# warning: compilation options might not be optical

project ( zip C )
cmake_minimum_required ( VERSION 2.8 )
include ( cmake/dist.cmake )

set ( SRC_ZIP crc32.c crypt.c deflate.c fileio.c globals.c trees.c ttyio.c util.c 
  zip.c zipfile.c zipup.c )
if ( WIN32 AND NOT CYGWIN )
  set ( SRC_ZIP ${SRC_ZIP} win32/nt.c win32/win32.c win32/win32i64.c win32/win32zip.c )
  add_definitions ( -DWIN32 -DNO_ASM )
else ( WIN32 AND NOT CYGWIN )
  set ( SRC_ZIP ${SRC_ZIP} unix/unix.c )
  add_definitions ( -DUNIX )
  add_definitions ( -DNO_OFF_T )
  # simpler
endif ( WIN32 AND NOT CYGWIN )

include_directories ( . )

include ( CheckFunctionExists )
check_function_exists ( lchmod HAVE_LCHMOD )
if ( NOT HAVE_LCHMOD )
  add_definitions ( -DNO_LCHMOD )
endif ( NOT HAVE_LCHMOD )

add_executable ( zip ${SRC_ZIP} )

install_executable ( zip  )
install_data ( README LICENSE )
