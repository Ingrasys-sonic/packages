#
# Makefile for kernel test
#
PWD         := $(shell pwd) 
KVERSION    := $(shell uname -r)
KERNEL_DIR   = /lib/modules/$(KVERSION)/build/

MODULE_NAME  = i2c-mux.o i2c-mux-pca954x.o eeprom_mb.o sff_8436_eeprom.o gpio-pca953x.o
obj-m       := $(MODULE_NAME)

all:
	make -C $(KERNEL_DIR) M=$(PWD) modules
clean:
	make -C $(KERNEL_DIR) M=$(PWD) clean
clean2:
	make -C $(KERNEL_DIR) M=$(PWD) clean
	rm -rf /lib/modules/$(KVERSION)/extra/*.ko
install:
	cp *.ko /lib/modules/$(KVERSION)/extra/
	depmod -a
