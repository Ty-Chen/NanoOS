MAKEFLAGS = -sR
MKDIR = mkdir
RMDIR = rmdir
CP = cp
CD = cd
DD = dd
RM = rm

ASM		= nasm
CC		= gcc
LD		= ld
OBJCOPY	= objcopy

ASMBFLAGS	= -f elf -w-orphan-labels
CFLAGS		= -c -Os -std=c99 -m32 -Wall -Wshadow -W -Wconversion -Wno-sign-conversion  -fno-stack-protector -fomit-frame-pointer -fno-builtin -fno-common  -ffreestanding  -Wno-unused-parameter -Wunused-variable
LDFLAGS		= -s -static -T main.lds -n -Map NanoOS.map 
OJCYFLAGS	= -S -O binary

OS_OBJS :=
OS_OBJS += entry.o main.o vgastr.o
OS_ELF = NanoOS.elf
OS_BIN = NanoOS.bin

.PHONY : build clean all link bin

all: clean build link bin

clean:
	$(RM) -f *.o *.bin *.elf

build: $(OS_OBJS)

link: $(OS_ELF)
$(OS_ELF): $(OS_OBJS)
	$(LD) $(LDFLAGS) -o $@ $(OS_OBJS)
bin: $(OS_BIN)
$(OS_BIN): $(OS_ELF)
	$(OBJCOPY) $(OJCYFLAGS) $< $@

%.o : %.asm
	$(ASM) $(ASMBFLAGS) -o $@ $<
%.o : %.c
	$(CC) $(CFLAGS) -o $@ $<

update_image:
	sudo mount floppy.img /mnt/kernel
	sudo cp HelloOS.bin /mnt/kernel/hx_kernel
	sleep 1
	sudo umount /mnt/kernel

umount_image:
	sudo umount /mnt/kernel	

qemu:
	qemu-system-i386 -fda floppy.img -boot a
	#add '-nographic' option if using server of linux distro, such as fedora-server,or "gtk initialization failed" error will occur.
