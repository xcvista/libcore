
CFLAGS += -Wall -O2 -g -std=gnu99
LDFLAGS += -shared -g

PREFIX := /usr/local
VERSION := 1.0

C_FILES := vector.c
OBJS := $(subst .c,.o,$(C_FILES))
TARGET := libvector

# Detecting OS.
OS := $(shell uname -s)

ifeq ($(OS),Darwin) # OS X
CC := xcrun clang
LD := xcrun clang
CFLAGS += -arch i386 -arch x86_64
SOEXT = dylib
DBGEXT = dSYM
SONAME = $(TARGET).$(VERSION).$(SOEXT)
LDFLAGS += -install_name $(TARGET).$(SOEXT) -arch i386 -arch x86_64
else
ifeq ($(OS),Linux) # Linux
CC := gcc
LD := gcc
SOEXT := so
DBGEXT = d
SONAME = $(TARGET).$(SOEXT).$(VERSION)
LDFLAGS += -soname $(SONAME)
else
$(error This Makefile is for OS X and Linux only.)
endif
endif

LIBDIR = $(PREFIX)/lib
INCDIR = $(PREFIX)/include

all: $(SONAME) $(TARGET).$(SOEXT)

.PHONY: all clean install uninstall check

$(SONAME): $(OBJS)
	$(LD) $(LDFLAGS) vector.o -o $(SONAME)

$(TARGET).$(SOEXT): $(SONAME)
	ln -s $(SONAME) $(TARGET).$(SOEXT)

%.o: %.c %.h
	$(CC) $(CFLAGS) -c $< -o $@

clean:
	-rm vector.o &> /dev/null
	-rm -r *.$(DBGEXT) &> /dev/null
	-rm $(SONAME) &> /dev/null
	-rm $(TARGET).$(SOEXT) &> /dev/null

install: $(SONAME) $(TARGET).$(SOEXT) vector.h
	cp $(SONAME) $(LIBDIR) &> /dev/null
	cp $(TARGET).$(SOEXT) $(LIBDIR) &> /dev/null
	cp vector.h $(INCDIR) &> /dev/null

uninstall:
	-rm $(LIBDIR)/$(SONAME) &> /dev/null
	-rm $(LIBDIR)/$(TARGET).$(SOEXT) &> /dev/null
	-rm $(INCDIR)/vector.h &> /dev/null

