# PetaLinux

This directory contains documentation and helper files related to PetaLinux projects used with Xilinx FPGA/SoC designs. The README explains what PetaLinux is, lists prerequisites, and provides quick start instructions and common commands for building and packaging embedded Linux images using the Xilinx PetaLinux tools.

## About PetaLinux

PetaLinux is Xilinx's embedded Linux development flow and set of tools used to build, configure, and package Linux images for Xilinx Zynq and Zynq UltraScale+ (MPSoC) devices. It integrates device-tree generation, kernel configuration, root filesystem generation, bootloader packaging, and image packaging so you can create images that boot on target hardware.

PetaLinux is commonly used to:
- Bring up Linux on custom FPGA/SoC designs
- Customize the kernel, device tree, and root filesystem
- Package boot artifacts (BOOT.BIN, image.ub, rootfs) for SD/QSPI/eMMC boot
- Automate builds in CI for hardware-specific images

> Note: PetaLinux is distributed by Xilinx and requires installation on a Linux host. Always use the PetaLinux release that matches your Vivado/SDK/HLS toolchain and target hardware.

## Prerequisites

- Supported Linux host (Ubuntu 18.04, 20.04, or the versions recommended by your PetaLinux release)
- Xilinx PetaLinux tools installed and licensed
- Vivado (or the exported XSA/HDF) for hardware/platform generation when building device trees or FPGA bitstreams
- Sufficient disk space (tens of GBs) and memory (8+ GB recommended; 16+ GB preferred for complex builds)

## Quick start

1. Install PetaLinux following Xilinx documentation and source the environment:

   ```bash
   source /opt/petalinux/settings.sh
   ```

2. Clone this repository and change into the project directory:

   ```bash
   git clone https://github.com/vignesh-rtl/PetaLinux.git
   cd PetaLinux/petalinux
   ```

3. If a PetaLinux project isn't already present, create one (example using zynqMP template):

   ```bash
   petalinux-create -t project --template zynqMP --name my_project
   cd my_project
   ```

4. Import or copy the hardware description (XSA/HDF) into the project or configure the platform BSP as needed.

5. Configure and build:

   ```bash
   petalinux-config
   petalinux-config -c kernel
   petalinux-config -c rootfs
   petalinux-build
   ```

6. Package boot artifacts for deployment:

   ```bash
   petalinux-package --boot --fsbl <fsbl.elf> --fpga <system.bit> --u-boot
   ```

7. The generated images are typically under `images/linux/` in your project directory.

## Common commands

- petalinux-create -t project --template <type> --name <name>
- petalinux-config (top-level configuration)
- petalinux-config -c kernel (kernel configuration)
- petalinux-config -c rootfs (rootfs configuration)
- petalinux-build (build everything)
- petalinux-package (create BOOT.BIN / image.ub)



