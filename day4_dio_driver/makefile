# # Makefile để build dự án Blink LED cho STM32F103 (no HAL/SPL)

# # Tên file đầu ra
# TARGET = blinkled

# # Trình biên dịch và các flags
# CC      = arm-none-eabi-gcc
# CFLAGS  = -mcpu=cortex-m3 -mthumb -O0 -g -Wall -ffreestanding -nostdlib \
# 			-ISPL/inc \
# 			-ICore \
# 			-DSTM32F10X_MD -DUSE_STDPERIPH_DRIVER
# LDFLAGS = -T stm32f103.ld -nostdlib -Wl,--gc-sections

# # Danh sách file nguồn
# SRCS_C  = main.c $(wildcard SPL/src/*.c)
# SRCS_S  = startup_stm32f103.s
# OBJS    = $(SRCS_C:.c=.o) $(SRCS_S:.s=.o)

# # Mục tiêu mặc định
# all: $(TARGET).bin

# # Compile file C
# %.o: %.c
# 	$(CC) $(CFLAGS) -c $< -o $@

# # Assemble file ASM
# %.o: %.s
# 	$(CC) $(CFLAGS) -c $< -o $@

# # Link thành ELF
# $(TARGET).elf: $(OBJS) stm32f103.ld
# 	$(CC) $(CFLAGS) $(OBJS) $(LDFLAGS) -o $@

# # Tạo file .bin từ .elf
# $(TARGET).bin: $(TARGET).elf
# 	arm-none-eabi-objcopy -O binary $< $@

# # Nạp firmware vào Blue Pill (dùng file .bin)
# flash: $(TARGET).bin
# 	openocd -f interface/stlink.cfg -f target/stm32f1x.cfg -c "program $(TARGET).bin 0x08000000 verify reset exit"

# # Xóa file tạm
# clean:
# 	rm -f *.o *.elf *.bin SPL/src/*.o

# .PHONY: all clean flash


# Makefile để build dự án Blink LED cho STM32F103 (no HAL/SPL)

# Tên file đầu ra
TARGET = blinkled
BUILD_DIR = build

# Trình biên dịch và các flags
CC      = arm-none-eabi-gcc
CFLAGS  = -mcpu=cortex-m3 -mthumb -O0 -g -Wall -ffreestanding -nostdlib \
            -ISPL/inc \
            -ICore \
            -DSTM32F10X_MD -DUSE_STDPERIPH_DRIVER
LDFLAGS = -T stm32f103.ld -nostdlib -Wl,--gc-sections

# Danh sách file nguồn
SRCS_C  = main.c $(wildcard SPL/src/*.c)
SRCS_S  = startup_stm32f103.s

# Danh sách file object có đường dẫn build folder
OBJS_C  = $(patsubst %.c,$(BUILD_DIR)/%.o,$(SRCS_C))
OBJS_S  = $(patsubst %.s,$(BUILD_DIR)/%.o,$(SRCS_S))
OBJS    = $(OBJS_C) $(OBJS_S)

# Mục tiêu mặc định
all: directories $(BUILD_DIR)/$(TARGET).bin

# Tạo thư mục build và các thư mục con cần thiết
directories:
	@mkdir -p $(BUILD_DIR)
	@mkdir -p $(BUILD_DIR)/SPL/src

# Compile file C
$(BUILD_DIR)/%.o: %.c
	@mkdir -p $(dir $@)
	$(CC) $(CFLAGS) -c $< -o $@

# Assemble file ASM
$(BUILD_DIR)/%.o: %.s
	@mkdir -p $(dir $@)
	$(CC) $(CFLAGS) -c $< -o $@

# Link thành ELF
$(BUILD_DIR)/$(TARGET).elf: $(OBJS) stm32f103.ld
	$(CC) $(CFLAGS) $(OBJS) $(LDFLAGS) -o $@

# Tạo file .bin từ .elf
$(BUILD_DIR)/$(TARGET).bin: $(BUILD_DIR)/$(TARGET).elf
	arm-none-eabi-objcopy -O binary $< $@

# Nạp firmware vào Blue Pill (dùng file .bin)
flash: $(BUILD_DIR)/$(TARGET).bin
	openocd -f interface/stlink.cfg -f target/stm32f1x.cfg -c "program $< 0x08000000 verify reset exit"

# Xóa file tạm
clean:
	rm -rf $(BUILD_DIR)

.PHONY: all clean flash directories