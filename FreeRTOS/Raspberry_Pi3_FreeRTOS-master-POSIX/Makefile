PREFIX=C:/Program Files/GNU Tools ARM Embedded/4.7 2013q3/lib/gcc/arm-none-eabi/4.7.4
ARMGNU ?= arm-none-eabi
#INCLUDEPATH ?= "D:/Study/Raspberry/uCProject/working/work-ucos/h"
INCLUDEPATH ?= FreeRTOS/Source/include 
INCLUDEPATH2 ?= FreeRTOS/Source/portable/GCC/RaspberryPi
INCLUDEPATH3 ?= Demo/Drivers

INCLUDEPATH4 ?= FreeRTOS-Plus-POSIX/include
INCLUDEPATH5 ?= Demo/include
INCLUDEPATH6 ?= Demo/include/private

#INCLUDEPATH7 ?= FreeRTOS-Plus-POSIX/source

#INCLUDEPATH8 ?= FreeRTOS-Plus-POSIX/include/portable/espressif/esp32_devkitc_esp_wrover_kit
#INCLUDEPATH7 ?= FreeRTOS-Plus-POSIX/include/portable/GCC/POSIX
INCLUDEPATH7 ?= FreeRTOS-Plus-POSIX/include/portable/microchip/curiosity_pic32mzef
COPS = -Wall -O2 -lm -lgcc -ffreestanding  -std=gnu99   -mcpu=cortex-a7 -I  $(INCLUDEPATH) -I $(INCLUDEPATH2) -I $(INCLUDEPATH3) -I $(INCLUDEPATH4) -I $(INCLUDEPATH5) -I $(INCLUDEPATH6) -I $(INCLUDEPATH7) -I $(INCLUDEPATH8)
#--specs=nosys.specs --specs=nano.specs --specs=rdimon.specs -nostartfiles 
gcc : kernel.img

OBJS = build/startup.o 

OBJS +=build/gpio.o  
OBJS +=build/irq.o
OBJS +=build/uart.o
OBJS +=build/led.o
OBJS +=build/main.o
	
OBJS +=build/port.o
OBJS +=build/portisr.o
#OBJS +=build/heap_1.o
#OBJS +=build/heap_2.o
#OBJS +=build/heap_3.o
OBJS +=build/heap_4.o
OBJS +=build/mem_man.o
OBJS +=build/croutine.o
OBJS +=build/list.o
OBJS +=build/queue.o
OBJS +=build/tasks.o
OBJS +=build/timers.o

#OBJS +=build/FreeRTOS_POSIX_pthread.o
OBJS +=build/FreeRTOS_POSIX_clock.o
OBJS +=build/FreeRTOS_POSIX_mqueue.o
OBJS +=build/FreeRTOS_POSIX_pthread_barrier.o
OBJS +=build/FreeRTOS_POSIX_pthread_cond.o
OBJS +=build/FreeRTOS_POSIX_pthread_mutex.o
OBJS +=build/FreeRTOS_POSIX_pthread.o
OBJS +=build/FreeRTOS_POSIX_sched.o
OBJS +=build/FreeRTOS_POSIX_semaphore.o
OBJS +=build/FreeRTOS_POSIX_timer.o
OBJS +=build/FreeRTOS_POSIX_unistd.o
OBJS +=build/FreeRTOS_POSIX_utils.o

clean :
	rm -f build/*.o
	rm -f *.bin
	rm -f *.hex
	rm -f *.elf
	rm -f *.list
	rm -f *.img
	rm -f build/*.bc

build/%.o : Demo/%.s
	$(ARMGNU)-gcc $(COPS) -D__ASSEMBLY__ -c -o $@ $<
	
build/%.o : Demo/Drivers/%.c
	$(ARMGNU)-gcc $(COPS)  -c -o $@ $<
		
build/%.o : Demo/%.c
	$(ARMGNU)-gcc $(COPS)  -c -o $@ $<
	
build/%.o : FreeRTOS/Source/%.c
	$(ARMGNU)-gcc $(COPS)  -c -o $@ $<

build/%.o : FreeRTOS-Plus-POSIX/source/%.c
	$(ARMGNU)-gcc $(COPS)  -c -o $@ $<

build/%.o : FreeRTOS/Source/portable/GCC/RaspberryPi/%.c
	$(ARMGNU)-gcc $(COPS) -c -o $@ $<
           
build/%.o : FreeRTOS/Source/portable/MemMang/%.c
	$(ARMGNU)-gcc $(COPS) -c -o $@ $<


build/%.o : FreeRTOS/Source/%.c
	$(ARMGNU)-gcc $(COPS) -c -o $@ $<

#build/%.o : FreeRTOS-Plus-POSIX\FreeRTOS-Plus-POSIX\include\portable\GCC\RaspberryPi%.c
#	$(ARMGNU)-gcc $(COPS) -c -o $@ $<
           
#build/%.o : FreeRTOS-Plus-POSIX\FreeRTOS-Plus-POSIX\include\portable\MemMang%.c
#	$(ARMGNU)-gcc $(COPS) -c -o $@ $<

kernel.img : raspberrypi.ld $(OBJS)
	$(ARMGNU)-ld $(OBJS) -T raspberrypi.ld -o freertos_bcm2837.elf 
	$(ARMGNU)-objdump -D freertos_bcm2837.elf > freertos_bcm2837.list
	$(ARMGNU)-objcopy freertos_bcm2837.elf -O ihex freertos_bcm2837.hex
	$(ARMGNU)-objcopy freertos_bcm2837.elf -O binary freertos_bcm2837.bin
	$(ARMGNU)-objcopy freertos_bcm2837.elf -O binary kernel.img