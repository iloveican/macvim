# Makefile for GvimExt, using MSVC
# Options:
#   DEBUG=yes		Build debug version (for VC7 and maybe later)
#   CPUARG=		/arch:IA32/AVX/etc, call from main makefile to set
#   			automatically from CPUNR
#

TARGETOS = WINNT

!ifndef APPVER
APPVER = 5.01
!endif

!if "$(DEBUG)" != "yes"
NODEBUG = 1
!endif

!ifdef PROCESSOR_ARCHITECTURE
# On Windows NT
! ifndef CPU
CPU = i386
!  if !defined(PLATFORM) && defined(TARGET_CPU)
PLATFORM = $(TARGET_CPU)
!  endif
!  ifdef PLATFORM
!   if ("$(PLATFORM)" == "x64") || ("$(PLATFORM)" == "X64")
CPU = AMD64
!   elseif ("$(PLATFORM)" != "x86") && ("$(PLATFORM)" != "X86")
!    error *** ERROR Unknown target platform "$(PLATFORM)". Make aborted.
!   endif
!  endif
! endif
!else
CPU = i386
!endif

!ifdef SDK_INCLUDE_DIR
!include $(SDK_INCLUDE_DIR)\Win32.mak
!else
!include <Win32.mak>
!endif

# include CPUARG
cflags = $(cflags) $(CPUARG)

all: gvimext.dll

gvimext.dll:    gvimext.obj	\
		gvimext.res
# $(implib) /NOLOGO -machine:$(CPU) -def:gvimext.def $** -out:gvimext.lib
# $(link) $(dlllflags) -base:0x1C000000 -out:$*.dll $** $(olelibsdll) shell32.lib gvimext.lib comctl32.lib gvimext.exp
  $(link) $(lflags) -dll -def:gvimext.def -base:0x1C000000 -out:$*.dll $** $(olelibsdll) shell32.lib comctl32.lib
  if exist $*.dll.manifest mt -nologo -manifest $*.dll.manifest -outputresource:$*.dll;2

gvimext.obj: gvimext.h

.cpp.obj:
	$(cc) $(cflags) -DFEAT_GETTEXT $(cvarsmt) $*.cpp

gvimext.res: gvimext.rc
	$(rc) /nologo $(rcflags) $(rcvars)  gvimext.rc

clean:
	- if exist gvimext.dll del gvimext.dll
	- if exist gvimext.lib del gvimext.lib
	- if exist gvimext.exp del gvimext.exp
	- if exist gvimext.obj del gvimext.obj
	- if exist gvimext.res del gvimext.res
	- if exist gvimext.dll.manifest del gvimext.dll.manifest
