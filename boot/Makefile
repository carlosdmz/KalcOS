CC=gcc
LD=ld

ARCH    = x86_64
TARGET  = BOOTx64.efi
SRC     = src/main.c src/file.c src/font.c src/graphics.c src/utils.c
SRC_INC = src/includes
OBJ     = $(patsubst %.c,%.o,$(SRC))
STATIC  = src/BOOTx64.so

EFILIB  = gnu-efi
EFIINC := $(EFILIB)/inc
CFLAGS := -I$(EFIINC)          \
		  -I$(SRC_INC)         \
		  -fpic                \
		  -ffreestanding       \
		  -fno-stack-protector \
		  -fno-stack-check     \
		  -fshort-wchar        \
		  -mno-red-zone

LDFLAGS := -shared -Bsymbolic -L$(EFILIB)/x86_64/lib -L$(EFILIB)/x86_64/gnuefi \
		   -T$(EFILIB)/gnuefi/elf_x86_64_efi.lds $(EFILIB)/x86_64/gnuefi/crt0-efi-x86_64.o

OBJFLAGS := -j .text -j .sdata -j .data -j .dynamic -j .dynsym  -j .rel -j .rela \
			-j .rel.* -j .rela.* -j .reloc --target efi-app-x86_64 --subsystem=10

all: $(TARGET)

%.o: %.c
	$(CC) $(CFLAGS) -c $? -o $@

$(STATIC): $(OBJ)
	$(LD) $(LDFLAGS) $? -o $@ -lefi -lgnuefi

$(TARGET): $(STATIC)
	objcopy $(OBJFLAGS) $< $@

clean:
	rm $(OBJ) $(STATIC)
