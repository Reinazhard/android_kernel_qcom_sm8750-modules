# Makefile for use with Android's kernel/build system

# Define module source path relative to KERNEL_SRC
# Ensure generated files are included, using srctree for correct pathing.
LINUXINCLUDE += -I$(srctree)/$(M)

KBUILD_OPTIONS += CAMERA_KERNEL_ROOT?=$(PWD)
KBUILD_OPTIONS += KERNEL_ROOT?=$(ROOT_DIR)/$(KERNEL_DIR)
KBUILD_OPTIONS += MODNAME=camera
KBUILD_OPTIONS += KBUILD_EXTRA_SYMBOLS+=$(M)/../mmrm-driver/Module.symvers
KBUILD_OPTIONS += KBUILD_EXTRA_SYMBOLS+=$(M)/../securemsm-kernel/Module.symvers
KBUILD_OPTIONS += KBUILD_EXTRA_SYMBOLS+=$(M)/../synx-kernel/Module.symvers

all: modules

CAMERA_COMPILE_TIME = $(shell date)
CAMERA_COMPILE_BY = $(shell whoami | sed 's/\\/\\\\/')
CAMERA_COMPILE_HOST = $(shell uname -n)

# Generate header in the source directory (M) using srctree if needed
# We must ensure the file is generated in the actual source dir, not output dir
# Use an absolute path for the generated file
$(srctree)/$(M)/cam_generated_h:
	echo '#define CAMERA_COMPILE_TIME "$(CAMERA_COMPILE_TIME)"' > $(srctree)/$(M)/cam_generated_h
	echo '#define CAMERA_COMPILE_BY "$(CAMERA_COMPILE_BY)"' >> $(srctree)/$(M)/cam_generated_h
	echo '#define CAMERA_COMPILE_HOST "$(CAMERA_COMPILE_HOST)"' >> $(srctree)/$(M)/cam_generated_h

modules: $(srctree)/$(M)/cam_generated_h
	$(MAKE) -C $(KERNEL_SRC) M=$(M) modules $(KBUILD_OPTIONS)

modules_install:
	$(MAKE) M=$(M) -C $(KERNEL_SRC) modules_install

clean:
	$(MAKE) -C $(KERNEL_SRC) M=$(M) clean
	rm -f $(srctree)/$(M)/cam_generated_h

