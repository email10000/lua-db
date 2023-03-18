CFLAGS ?= -g -std=gnu99 -Wall -Wextra -Wpedantic
#CFLAGS ?= -O3 -std=gnu99 -Wall -Wextra -Wpedantic -march=native -mtune=native
LIBFLAG ?= -shared -fpic -lm -Wl,--as-needed

# Lua configuration
LUA_CFLAGS ?= -I/usr/include/lua5.1
LUA_LIBS ?= -llua5.1
#LUA_CFLAGS = $(pkg-config --cflags luajit)
#LUA_LIBS = $(pkg-config --libs luajit)


LDB_MODULES ?= core module_fb module_sdl module_gfx module_drm

STRIP ?= strip



.PHONY: all
all: $(LDB_MODULES)

.PHONY: help
help:
	@echo "Available make targets: "
	@echo " help (this message)"
	@echo " all (build ldb and all modules)"
	@echo " core (only build core module)"
	@echo " module_gfx (only build extended graphics primitives module)"
	@echo " module_sdl (only build sdl module)"
	@echo " module_fb (only build framebuffer module)"
	@echo " clean (remove build artifacts)"


.PHONY: core
core: ldb_core.so
	$(STRIP) $^
	@echo "-> Building core module finished"

.PHONY: module_gfx
module_gfx: ldb_gfx.so
	$(STRIP) $^
	@echo "-> Building extended graphics primitives module finished"

.PHONY: module_fb
module_fb: ldb_fb.so
	$(STRIP) $^
	@echo "-> Building Framebuffer module finished"

.PHONY: clean
clean:
	@echo "-> Cleaning up build artifacts"
	rm -f ldb_core.o ldb_gfx.o ldb_fb.o
	rm -f ldb_core.so ldb_gfx.so ldb_fb.so

ldb_core.o: ldb_core.c
	$(CC) -o $@ -fPIC $(CFLAGS) $(LUA_CFLAGS) -c $^

ldb_core.so: ldb_core.o
	$(CC) -o $@ $(CFLAGS) $(LUA_CFLAGS) $^ $(LIBFLAG) $(LUA_LIBS)

ldb_gfx.o: ldb_gfx.c
	$(CC) -o $@ -fPIC $(CFLAGS) $(LUA_CFLAGS) -c $^

ldb_gfx.so: ldb_gfx.o ldb_core.o
	$(CC) -o $@ $(CFLAGS) $(LUA_CFLAGS) $^ $(LIBFLAG) $(LUA_LIBS)

ldb_fb.o: ldb_fb.c
	$(CC) -o $@ -fPIC $(CFLAGS) $(LUA_CFLAGS) -c $^

ldb_fb.so: ldb_fb.o ldb_core.o
	$(CC) -o $@ $(CFLAGS) $(LUA_CFLAGS) $^ $(LIBFLAG) $(LUA_LIBS)
