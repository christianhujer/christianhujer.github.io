---
layout: post
title: Converting Strings to Uppercase in GNUmake
---

GNUmake doesn't have a feature to convert Strings to uppercase. But hey, UNIX world is all easy. Just use a good shell, like bash :)

The demo use case is to pass a define to the C compiler that contains the filename stem converted to uppercase.

~~~
override CPPFLAGS+=-DFILE=FID_$${MODULE^^}

SHELL:=bash
export SHELLOPTS:=errexit:pipefail
sources:=$(shell find -name "*.c")

%.o: export MODULE=$*

.PHONY: all
all: foo

foo: foo.o $(sources:.c=.o)

.PHONY: clean
clean:
        rm -f foo $(sources:.c=.o)
~~~

This simple Makefile compiles and links a binary named foo from all C source files.
The rule-specific variable `MODULE` is defined for `%.o` rules and contains the stem of the implicit rule match `%.o: %.c`.
The variable `MODULE` is exported to make it available to the bash subshells.
The variable `CPPFLAGS` which is used by the shell scripts for the implicit rule `%.o: %.c` contains the expression `${MODULE^^}`.
The bash shell will expand this to the value of the variable `MODULE` and because of `^^` convert it to uppercase.
Btw. this Makefile is complete and will work for a lot of simple C projects.

You can also easily extend this Makefile to support an incremental build:

~~~
override CPPFLAGS+=-MMD -DFILE=FID_$${MODULE^^}

SHELL:=bash
export SHELLOPTS:=errexit:pipefail
sources:=$(shell find -name "*.c")

%.o: export MODULE=$*

.PHONY: all
all: foo

foo: foo.o $(sources:.c=.o)

-include $(sources:.c=.d)

.PHONY: clean
clean:
        rm -f foo $(sources:.c=.o) $(sources:.c=.d)
~~~

