CC=../../buildtools/arm-eabi-4.7/bin/arm-eabi-
FUNCTIONS = copy_hook
FUNCTIONS_BIN = $(FUNCTIONS:=.bin)

all: brcmfmac43430-sdio.bin

brcmfmac43430-sdio.bin: patcher.py patch.elf text.bin $(FUNCTIONS_BIN)
	./patcher.py

#patch.o: patch.c ../include/*.h
patch.o: patch.c
	$(CC)gcc -Wall -Werror -O2 -nostdlib -nostartfiles -ffreestanding -mthumb -march=armv7-m -ffunction-sections -fdata-sections -c $< -o $@

#../wrapper/wrapper.o: ../include/wrapper.h
#	make -C ../wrapper

#patch.elf: patch.ld patch.o ../wrapper/wrapper.o
patch.elf: patch.ld patch.o
	$(CC)ld -T $< -o $@ --gc-sections --print-gc-sections -M

$(FUNCTIONS_BIN): patch.elf
	$(CC)objcopy -O binary -j .text.$(basename $@) $< $@

text.bin: patch.elf
	$(CC)objcopy -O binary -j .text $< $@

#fw_bcmdhd.complete.bin : brcmfmac43430-sdio.bin ../../bootimg_src/firmware/rom.bin
#	dd if=../../bootimg_src/firmware/rom.bin of=$@ bs=1
#	dd if=brcmfmac43430-sdio.bin of=$@ bs=1 seek=1572864

#fw_bcmdhd.complete.elf : fw_bcmdhd.complete.bin
#	$(CC)objcopy -O elf32-littlearm -Barm -I binary $< $@
#	$(CC)objcopy --set-section-flags .data=code $@

check-nexmon-setup-env:
ifndef NEXMON_SETUP_ENV
	$(error run 'source setup_env.sh' first)
endif

clean:
	rm -f brcmfmac43430-sdio.bin fw_bcmdhd.complete.bin patch.o patch.elf text.bin $(FUNCTIONS_BIN)

FORCE:
