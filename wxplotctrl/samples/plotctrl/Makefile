# Makefile for wxstedit example for the wxstedit lib (UNIX).

# ----------------------------------------------------------------------------
# wxWidgets build settings

WXCONFIG := wx-config

WXDIR      = $(shell $(WXCONFIG) --prefix)
WXEXECDIR  = $(shell $(WXCONFIG) --exec-prefix)
WXLIBDIR   = $(WXEXECDIR)/lib

WXBASENAME = $(shell $(WXCONFIG) --basename)
WXVERSION  = $(shell $(WXCONFIG) --version)
WXRELEASE  = $(shell $(WXCONFIG) --release)

WXCXX      = $(shell $(WXCONFIG) --cxx)
WXCXXFLAGS = $(shell $(WXCONFIG) --cxxflags)
WXLIBS     = $(shell $(WXCONFIG) --libs)

# ----------------------------------------------------------------------------

# the location of wxCode, where wxThings is located
WXCODE_DIR = ../../../..
WXTHINGS_DIR = $(WXCODE_DIR)/components/wxthings
WXTHINGS_LIB   = $(WXBASENAME)_wxthings-$(WXRELEASE)

WXPLOTCTRL_LIBNAME = $(WXBASENAME)_plotctrl-$(WXRELEASE)
WXPLOTCTRL_LIBDIR = $(WXLIBDIR)

LIBVERSION_CURRENT=1
LIBVERSION_REVISION=0
LIBVERSION_AGE=0

TARGET_LIBNAME = lib$(WXPLOTCTRL_LIBNAME)

TARGET_LIB_STATIC = $(WXPLOTCTRL_LIBDIR)/$(TARGET_LIBNAME).a
TARGET_LIB_SHARED = $(WXPLOTCTRL_LIBDIR)/$(TARGET_LIBNAME).so.$(LIBVERSION_CURRENT).$(LIBVERSION_REVISION).$(LIBVERSION_AGE)
TARGET_LIB_LINK1  = $(TARGET_LIBNAME).so.$(LIBVERSION_CURRENT)
TARGET_LIB_LINK2  = $(TARGET_LIBNAME).so

# ----------------------------------------------------------------------------

CXXFLAGS = $(WXCXXFLAGS) -MMD -g  -Wall -Wunused -Wunused-parameter
LDLIBS   = $(WXLIBS)
CXX      = $(WXCXX)

# -pg in libs and defs for profiling
PROFILE = #-pg
APPEXTRADEFS = -I../../include -I$(WXTHINGS_DIR)/include $(PROFILE)
APPEXTRALIBS = $(PROFILE)

# ----------------------------------------------------------------------------

PROGRAM = wxplotctrl

OBJECTS = \
    $(WXTHINGS_DIR)/src/block.o \
    $(WXTHINGS_DIR)/src/genergdi.o \
    $(WXTHINGS_DIR)/src/geometry.o \
    $(WXTHINGS_DIR)/src/range.o

DEPFILES=$(OBJECTS:.o=.d)

.cpp.o:
	$(CXX) -c $(CXXFLAGS) $(APPEXTRADEFS) -o $@ $<

all: $(WXPLOTCTRL_LIBNAME) $(PROGRAM)

$(WXPLOTCTRL_LIBNAME) :
	@(cd ../../src && make)

$(WXTHINGS_LIB) :
	@(cd $(WXTHINGS_DIR) && make)

# wxplotctrl program using shared plotctrl library
# notice that we cheat with the wxthings lib and just use the objects that we need
$(PROGRAM): $(PROGRAM).o $(TARGET_LIB_SHARED) $(OBJECTS)
	$(CXX) $(LDFLAGS_EXE) -o $@ $(PROGRAM).o $(OBJECTS) $(LDLIBS) -l$(WXPLOTCTRL_LIBNAME) $(APPEXTRALIBS)
    # -l$(WXTHINGS_LIB)

# wxplotctrl program using static stedit library
#$(PROGRAM): $(PROGRAM).o $(TARGET_LIB_STATIC)
#	$(CXX) $(LDFLAGS_EXE) -o $@ $(PROGRAM).o $(LDLIBS) $(TARGET_LIB_STATIC) $(APPEXTRALIBS)

# wxplotctrl program using just object files and not the stedit lib
#$(PROGRAM): $(PROGRAM).o $(OBJECTS)
#	$(CXX) $(LDFLAGS_EXE) -o $@ $(PROGRAM).o $(OBJECTS) $(LDLIBS) $(APPEXTRALIBS)

clean:
	rm -f $(OBJECTS) $(DEPFILES) $(PROGRAM) $(PROGRAM).o $(PROGRAM).d core

cleanall: clean
	@(cd ../../src && make clean)


-include $(DEPFILES)

