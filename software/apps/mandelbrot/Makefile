#
# Copyright 2011-2015 Jeff Bush
#
# Licensed under the Apache License, Version 2.0 (the "License");
# you may not use this file except in compliance with the License.
# You may obtain a copy of the License at
#
#     http://www.apache.org/licenses/LICENSE-2.0
#
# Unless required by applicable law or agreed to in writing, software
# distributed under the License is distributed on an "AS IS" BASIS,
# WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
# See the License for the specific language governing permissions and
# limitations under the License.
#

TOPDIR=../../../

include $(TOPDIR)/build/target.mk

LIBS=-lc -los
SRCS=mandelbrot.c

OBJS := $(TOPDIR)/software/libs/libc/crt0.o $(SRCS_TO_OBJS)
DEPS := $(SRCS_TO_DEPS)

$(OBJ_DIR)/mandelbrot.hex: $(OBJS)
	$(LD) -o $(OBJ_DIR)/mandelbrot.elf $(LDFLAGS) $(OBJS) $(LIBS) $(LDFLAGS)
	$(ELF2HEX) -o $(OBJ_DIR)/mandelbrot.hex $(OBJ_DIR)/mandelbrot.elf

run: $(OBJ_DIR)/mandelbrot.hex
	$(EMULATOR) -f 640x480 $(OBJ_DIR)/mandelbrot.hex

verirun: $(OBJ_DIR)/mandelbrot.hex
	@rm -f $(OBJ_DIR)/output.bin output.png
	$(VERILATOR) +memdumpfile=$(OBJ_DIR)/output.bin +memdumpbase=0x200000 +memdumplen=0x12C000 +bin=$(OBJ_DIR)/mandelbrot.hex
	@convert -depth 8 -size 640x480 rgba:$(OBJ_DIR)/output.bin output.png

fpgarun: $(OBJ_DIR)/mandelbrot.hex
	$(SERIAL_BOOT) $(SERIAL_PORT) $(OBJ_DIR)/mandelbrot.hex

clean:
	rm -rf $(OBJ_DIR)

-include $(DEPS)

