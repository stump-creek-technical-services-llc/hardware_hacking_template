# FLAVOR: typically "", "reset", or "update".
ifeq ($(FLAVOR),)
    FLAVOR_EXT = bin
    $(info FLAVOR is default)
else
    FLAVOR_EXT = $(FLAVOR).bin
    $(info FLAVOR is "$(FLAVOR)")
endif

GIT_DIR := $(shell git rev-parse --show-toplevel)
include $(GIT_DIR)/device.sh

BIN_FILENAME=$(DEV_TAG_SC).$(FLAVOR_EXT)

FW_DIR=$(GIT_DIR)/../../firmware-copyrighted/$(DEV_TAG)
BIN_PATH=$(FW_DIR)/$(BIN_FILENAME)

LOG_PATH=$(GIT_DIR)/log/flashrom.$(FLAVOR_EXT).log

TEE=tee -a $(LOG_PATH)

all: bin

bin: $(BIN_FILENAME)

$(FW_DIR):
	mkdir -p $(FW_DIR)

$(BIN_FILENAME): $(BIN_PATH)
	ln -sf $(BIN_PATH) $(BIN_FILENAME)
	LANG=C bin2hex $(BIN_FILENAME) | $(TEE)

$(BIN_PATH): $(FW_DIR)
	flashrom_dump $(BIN_PATH) | $(TEE)
