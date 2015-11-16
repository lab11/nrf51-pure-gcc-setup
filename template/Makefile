TOOLCHAIN_PREFIX ?= arm-none-eabi

AS = $(TOOLCHAIN_PATH)$(TOOLCHAIN_PREFIX)-as
CC = $(TOOLCHAIN_PATH)$(TOOLCHAIN_PREFIX)-gcc
LD = $(TOOLCHAIN_PATH)$(TOOLCHAIN_PREFIX)-gcc
OBJCOPY = $(TOOLCHAIN_PATH)$(TOOLCHAIN_PREFIX)-objcopy
OBJDUMP = $(TOOLCHAIN_PATH)$(TOOLCHAIN_PREFIX)-objdump
SIZE = $(TOOLCHAIN_PATH)$(TOOLCHAIN_PREFIX)-size
GDB = $(TOOLCHAIN_PATH)$(TOOLCHAIN_PREFIX)-gdb

START_CODE ?= startup_nrf51.s
SYSTEM_FILE ?= system_nrf51.c

# Set hardware memory sizes
RAM_KB ?= 16
FLASH_KB ?= 256

ifeq ($(USE_SOFTDEVICE), s110)
    USE_BLE = 1
endif

ifeq ($(USE_SOFTDEVICE), s120)
    USE_BLE = 1
endif

ifeq ($(USE_SOFTDEVICE), s130)
    USE_BLE = 1
endif

ifeq ($(USE_SOFTDEVICE), s210)
    USE_ANT = 1
endif

ifeq ($(USE_SOFTDEVICE), s310)
    USE_BLE = 1
    USE_ANT = 1
endif


ifeq ($(SDK_VERSION), 9)

	SDK_INCLUDE_PATH += $(SDK_PATH)components/
	SDK_SOURCE_PATH += $(SDK_PATH)components/
	CMSIS_INCLUDE_PATH += $(SDK_PATH)components/toolchain/gcc/
	LIBRARY_PATHS += $(wildcard $(SDK_INCLUDE_PATH)libraries/*/)
	LIBRARY_PATHS += $(wildcard $(SDK_INCLUDE_PATH)drivers_nrf/ble_flash/)
	LIBRARY_PATHS += $(wildcard $(SDK_INCLUDE_PATH)drivers_nrf/clock/)
	LIBRARY_PATHS += $(wildcard $(SDK_INCLUDE_PATH)drivers_nrf/common/)
	LIBRARY_PATHS += $(wildcard $(SDK_INCLUDE_PATH)drivers_nrf/gpiote/)
	LIBRARY_PATHS += $(wildcard $(SDK_INCLUDE_PATH)drivers_nrf/hal/)
	LIBRARY_PATHS += $(wildcard $(SDK_INCLUDE_PATH)drivers_nrf/lpcomp/)
	LIBRARY_PATHS += $(wildcard $(SDK_INCLUDE_PATH)drivers_nrf/ppi/)
	LIBRARY_PATHS += $(wildcard $(SDK_INCLUDE_PATH)drivers_nrf/pstorage/)
	LIBRARY_PATHS += $(wildcard $(SDK_INCLUDE_PATH)drivers_nrf/radio_config/)
	LIBRARY_PATHS += $(wildcard $(SDK_INCLUDE_PATH)drivers_nrf/rng/)
	LIBRARY_PATHS += $(wildcard $(SDK_INCLUDE_PATH)drivers_nrf/rtc/)
	LIBRARY_PATHS += $(wildcard $(SDK_INCLUDE_PATH)drivers_nrf/sdio/)
	LIBRARY_PATHS += $(wildcard $(SDK_INCLUDE_PATH)drivers_nrf/spi_master/)
	LIBRARY_PATHS += $(wildcard $(SDK_INCLUDE_PATH)drivers_nrf/spi_slave/)
	LIBRARY_PATHS += $(wildcard $(SDK_INCLUDE_PATH)drivers_nrf/swi/)
	LIBRARY_PATHS += $(wildcard $(SDK_INCLUDE_PATH)drivers_nrf/timer/)
	LIBRARY_PATHS += $(wildcard $(SDK_INCLUDE_PATH)drivers_nrf/twi_master/)
	LIBRARY_PATHS += $(wildcard $(SDK_INCLUDE_PATH)drivers_nrf/uart/)
	LIBRARY_PATHS += $(wildcard $(SDK_INCLUDE_PATH)drivers_nrf/wdt/)
	LIBRARY_PATHS += $(wildcard $(SDK_INCLUDE_PATH)drivers_ext/*/)
	LIBRARY_PATHS += $(wildcard $(SDK_INCLUDE_PATH)device/)
	LIBRARY_PATHS += $(wildcard $(SDK_INCLUDE_PATH)serialization/*/)
	LIBRARY_PATHS += $(SDK_INCLUDE_PATH)toolchain/gcc/
	LIBRARY_PATHS += $(SDK_INCLUDE_PATH)toolchain/
	SOURCE_PATHS += ../
	SOURCE_PATHS += $(SDK_SOURCE_PATH)
	SOURCE_PATHS += $(TEMPLATE_PATH)
	SOURCE_PATHS += $(wildcard $(SDK_SOURCE_PATH)*/)
	SOURCE_PATHS += $(wildcard $(SDK_SOURCE_PATH)libraries/*/)
	SOURCE_PATHS += $(wildcard $(SDK_SOURCE_PATH)drivers_nrf/*/)
	SOURCE_PATHS += $(wildcard $(SDK_SOURCE_PATH)drivers_ext/*/)
	SOURCE_PATHS += $(wildcard $(SDK_SOURCE_PATH)serialization/*/)

ifdef USE_BLE
    LIBRARY_PATHS += $(wildcard $(SDK_INCLUDE_PATH)ble/*/)
    LIBRARY_PATHS += $(wildcard $(SDK_INCLUDE_PATH)ble/ble_services/*/)
    SOURCE_PATHS += $(wildcard $(SDK_SOURCE_PATH)ble/*/)
    SOURCE_PATHS += $(wildcard $(SDK_SOURCE_PATH)ble/ble_services/*/)
    CFLAGS += -DBLE_STACK_SUPPORT_REQD
endif

ifdef USE_ANT
    CFLAGS += -DANT_STACK_SUPPORT_REQD
endif

ifdef USE_SOFTDEVICE
    LIBRARY_PATHS += $(SDK_INCLUDE_PATH)softdevice/common/softdevice_handler/
    LIBRARY_PATHS += $(SDK_INCLUDE_PATH)softdevice/$(USE_SOFTDEVICE)/headers/
    SOURCE_PATHS += $(SDK_INCLUDE_PATH)softdevice/common/softdevice_handler/
    SOURCE_PATHS += $(SDK_INCLUDE_PATH)softdevice/$(USE_SOFTDEVICE)/headers/
    #Mark that we need the linker file for soft device 8.x.x
    SOFTDEVICE_VERSION ?= 8.0.0
else
	LIBRARY_PATHS += $(wildcard $(SDK_INCLUDE_PATH)drivers_nrf/nrf_soc_nosd/)
    USE_SOFTDEVICE = blank
endif




else
ifeq ($(SDK_VERSION), 7)
#this is for version 7

	SDK_INCLUDE_PATH += $(SDK_PATH)components/
	SDK_SOURCE_PATH += $(SDK_PATH)components/
	CMSIS_INCLUDE_PATH += $(SDK_PATH)components/toolchain/gcc/
	LIBRARY_PATHS += $(wildcard $(SDK_INCLUDE_PATH)libraries/*/)
	LIBRARY_PATHS += $(wildcard $(SDK_INCLUDE_PATH)drivers_nrf/ble_flash/)
	LIBRARY_PATHS += $(wildcard $(SDK_INCLUDE_PATH)drivers_nrf/hal/)
	LIBRARY_PATHS += $(wildcard $(SDK_INCLUDE_PATH)drivers_nrf/pstorage/)
	LIBRARY_PATHS += $(wildcard $(SDK_INCLUDE_PATH)drivers_nrf/radio_config/)
	LIBRARY_PATHS += $(wildcard $(SDK_INCLUDE_PATH)drivers_nrf/sdio/)
	LIBRARY_PATHS += $(wildcard $(SDK_INCLUDE_PATH)drivers_nrf/simple_uart/)
	LIBRARY_PATHS += $(wildcard $(SDK_INCLUDE_PATH)drivers_nrf/spi_master/)
	LIBRARY_PATHS += $(wildcard $(SDK_INCLUDE_PATH)drivers_nrf/spi_slave/)
	LIBRARY_PATHS += $(wildcard $(SDK_INCLUDE_PATH)drivers_nrf/twi_master/)
	LIBRARY_PATHS += $(wildcard $(SDK_INCLUDE_PATH)drivers_nrf/uart/)
	LIBRARY_PATHS += $(wildcard $(SDK_INCLUDE_PATH)drivers_ext/*/)
	LIBRARY_PATHS += $(wildcard $(SDK_INCLUDE_PATH)device/)
	LIBRARY_PATHS += $(wildcard $(SDK_INCLUDE_PATH)serialization/*/)
	LIBRARY_PATHS += $(SDK_INCLUDE_PATH)toolchain/gcc/
	LIBRARY_PATHS += $(SDK_INCLUDE_PATH)toolchain/
	SOURCE_PATHS += ../
	SOURCE_PATHS += $(SDK_SOURCE_PATH)
	SOURCE_PATHS += $(TEMPLATE_PATH)
	SOURCE_PATHS += $(wildcard $(SDK_SOURCE_PATH)*/)
	SOURCE_PATHS += $(wildcard $(SDK_SOURCE_PATH)libraries/*/)
	SOURCE_PATHS += $(wildcard $(SDK_SOURCE_PATH)drivers_nrf/*/)
	SOURCE_PATHS += $(wildcard $(SDK_SOURCE_PATH)drivers_ext/*/)
	SOURCE_PATHS += $(wildcard $(SDK_SOURCE_PATH)serialization/*/)

ifdef USE_BLE
    LIBRARY_PATHS += $(wildcard $(SDK_INCLUDE_PATH)ble/*/)
    LIBRARY_PATHS += $(wildcard $(SDK_INCLUDE_PATH)ble/ble_services/*/)
    SOURCE_PATHS += $(wildcard $(SDK_SOURCE_PATH)ble/*/)
    SOURCE_PATHS += $(wildcard $(SDK_SOURCE_PATH)ble/ble_services/*/)
    CFLAGS += -DBLE_STACK_SUPPORT_REQD
endif

ifdef USE_ANT
    CFLAGS += -DANT_STACK_SUPPORT_REQD
endif

ifdef USE_SOFTDEVICE
    LIBRARY_PATHS += $(SDK_INCLUDE_PATH)softdevice/common/softdevice_handler/
    LIBRARY_PATHS += $(SDK_INCLUDE_PATH)softdevice/$(USE_SOFTDEVICE)/headers/
    SOURCE_PATHS += $(SDK_INCLUDE_PATH)softdevice/common/softdevice_handler/
    SOURCE_PATHS += $(SDK_INCLUDE_PATH)softdevice/$(USE_SOFTDEVICE)/headers/
    SOFTDEVICE_VERSION ?= 7.3.0
else
	LIBRARY_PATHS += $(wildcard $(SDK_INCLUDE_PATH)drivers_nrf/nrf_soc_nosd/)
    USE_SOFTDEVICE = blank
endif

else
#this is for version 6(default)

SDK_INCLUDE_PATH += $(SDK_PATH)Include/
SDK_SOURCE_PATH += $(SDK_PATH)Source/
CMSIS_INCLUDE_PATH += $(SDK_PATH)Include/gcc

LIBRARY_PATHS += ../
LIBRARY_PATHS += $(SDK_INCLUDE_PATH)
LIBRARY_PATHS += $(SDK_INCLUDE_PATH)sdk/
SOURCE_PATHS += ../ $(SDK_SOURCE_PATH) $(TEMPLATE_PATH) $(wildcard $(SDK_SOURCE_PATH)*/)

ifdef USE_BLE
    LIBRARY_PATHS += $(SDK_INCLUDE_PATH)ble/
    LIBRARY_PATHS += $(SDK_INCLUDE_PATH)ble/ble_services/
    LIBRARY_PATHS += $(SDK_INCLUDE_PATH)ble/device_manager/
    SOURCE_PATHS += $(SDK_SOURCE_PATH)ble/ble_services/
    SOURCE_PATHS += $(SDK_SOURCE_PATH)ble/device_manager/
    CFLAGS += -DBLE_STACK_SUPPORT_REQD
endif

ifdef USE_ANT
    CFLAGS += -DANT_STACK_SUPPORT_REQD
endif

ifdef USE_SOFTDEVICE
    LIBRARY_PATHS += $(SDK_INCLUDE_PATH)$(USE_SOFTDEVICE)
    LIBRARY_PATHS += $(SDK_INCLUDE_PATH)app_common/
    LIBRARY_PATHS += $(SDK_INCLUDE_PATH)sd_common/
else
    USE_SOFTDEVICE = blank
endif

ifdef USE_EXT_SENSORS
LIBRARY_PATHS += $(SDK_INCLUDE_PATH)ext_sensors/
SOURCE_PATHS += $(wildcard $(SDK_SOURCE_PATH)ext_sensors/*/)
endif

endif
endif

print-% : ; @echo $* = $($*)

LINKER_SCRIPT ?= gcc_nrf51_$(RAM_KB)_$(FLASH_KB)_$(USE_SOFTDEVICE)_$(SOFTDEVICE_VERSION).ld
OUTPUT_NAME ?= $(addsuffix _$(USE_SOFTDEVICE), $(PROJECT_NAME))

LIBRARY_INCLUDES = $(addprefix -I,$(LIBRARY_PATHS))
CMSIS_INCLUDE = $(addprefix -I,$(CMSIS_INCLUDE_PATH))

VPATH = $(SOURCE_PATHS)

OUTPUT_PATH = _build/

CPUFLAGS = -mthumb -mcpu=cortex-m0 -march=armv6-m
CFLAGS += -std=gnu99 -c $(CPUFLAGS) -Wall -D$(DEVICE) -D$(BOARD) -s -ffunction-sections -fdata-sections $(CMSIS_INCLUDE) $(LIBRARY_INCLUDES) -MD
LDFLAGS += $(CPUFLAGS) -L $(TEMPLATE_PATH) -T $(LINKER_SCRIPT) -Wl,--gc-sections -Wl,-Map=$(OUTPUT_PATH)$(OUTPUT_NAME).Map

HEX = $(OUTPUT_PATH)$(OUTPUT_NAME).hex
ELF = $(OUTPUT_PATH)$(OUTPUT_NAME).elf
BIN = $(OUTPUT_PATH)$(OUTPUT_NAME).bin

SRCS = $(SYSTEM_FILE) $(notdir $(APPLICATION_SRCS))
OBJS = $(addprefix $(OUTPUT_PATH), $(SRCS:.c=.o)) $(addprefix $(OUTPUT_PATH),$(APPLICATION_LIBS))
DEPS = $(addprefix $(OUTPUT_PATH), $(SRCS:.c=.d))
SRCS_AS = $(START_CODE)
OBJS_AS = $(addprefix $(OUTPUT_PATH), $(SRCS_AS:.s=.os))

JLINK_OPTIONS = -device nrf51822 -if swd -speed 1000

all: $(OBJS) $(OBJS_AS) $(HEX)

debug: CFLAGS += -g
debug: $(OBJS) $(OBJS_AS) $(HEX)


rebuild: clean all

ifeq ($(OS),Windows_NT)
include $(TEMPLATE_PATH)Makefile.windows
else
include $(TEMPLATE_PATH)Makefile.posix
endif

$(HEX): $(OBJS) $(OBJS_AS)
	$(LD) $(LDFLAGS) $(OBJS_AS) $(OBJS) -o $(ELF)
	$(OBJCOPY) -Oihex $(ELF) $(HEX)
	$(OBJCOPY) -Obinary $(ELF) $(BIN)
	$(SIZE) $(ELF)

size: $(ELF)
	$(SIZE) $(ELF)

$(OUTPUT_PATH)%.o: %.c
	$(MAKE_BUILD_FOLDER)
	$(CC) $(LDFLAGS) $(CFLAGS) $< -o $@

$(OUTPUT_PATH)%.os: %.s
	$(MAKE_BUILD_FOLDER)
	$(AS) $< -o $@

-include $(DEPS)

.PHONY: all clean rebuild size
