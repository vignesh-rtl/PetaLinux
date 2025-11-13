# PetaLinux Projects

This directory contains my work related to **PetaLinux-based embedded Linux development** for AMD/Xilinx SoC platforms such as **KV260 Kria Starter Kit** and **ZCU106 MPSoC**.  
It includes project notes, setup procedures, boot instructions, and board-specific workflows used while creating custom Linux images, device trees, and embedded applications.

---

## 1. 📌 What is PetaLinux?

**PetaLinux** is AMD/Xilinx’s embedded Linux development framework for Zynq & Zynq UltraScale+ devices.  
It provides tools to build:

- Custom Linux images  
- Kernel, RootFS, and Device Tree  
- Boot files (FSBL, U-Boot, Image, DTB, WIC)  
- Linux–FPGA integrated workflows using XSA files  

PetaLinux simplifies Linux bring-up on programmable SoCs.

---

## 📚 Official Reference

For complete documentation, refer to the official AMD guide:  
🔗 **https://docs.amd.com/r/2023.1-English/ug1144-petalinux-tools-reference-guide/Overview**

---

# 2. ⚙️ PetaLinux Minimal Workflow (Step-by-Step)

This section summarizes the essential commands needed for creating, configuring, building, and deploying a PetaLinux project.

---

## PetaLinux Minimal Workflow

### Load PetaLinux Environment

```
source <plnx_dir>/petalinux_202x.x/settings.sh
```

*Enables PetaLinux tools and environment.*

---

## CREATE PROJECT

### Create New PetaLinux Project

```
petalinux-create project --template zynqMP --name <project_name>
```

### Import Hardware

```
petalinux-config --get-hw-description <path_to_xsa>
```

---

## CONFIGURE

### Kernel, RootFS & Device Tree Configuration

```
petalinux-config -c kernel
petalinux-config -c rootfs
petalinux-config -c device-tree
```

---

## BUILD

### Build Complete Project

```
petalinux-build
```

---

## PACKAGE

### Generate BOOT.BIN

```
petalinux-package boot --u-boot
```

### Generate Basic WIC Image

```
petalinux-package wic
```

### Board-Specific WIC Image (KV260)

```
petalinux-package --wic --images-dir images/linux/ --bootfiles "ramdisk.cpio.gz.u-boot,boot.scr,Image,system.dtb,system-zynqmp-sck-kv-g-revB.dtb"
```

---

## CLEANUP

### Clean Device Tree

```
petalinux-build -c device-tree -x cleansstate
```

### Full Cleanup

```
petalinux-build -x mrproper
```

---

## YOCTO / BITBAKE

### Source BitBake Environment

```
source components/yocto/layers/core/oe-init-build-env components/yocto
```

---

## FLASH SD CARD MANUALLY

### Step 1: Create Partitions

```
sudo fdisk /dev/sdb
Command (m for help): n
Select (default p): p
<enter>
[Y]es/[N]o: y
n, p → enter → w
```

### Step 2: Format Partitions

```
sudo mkfs.vfat /dev/sdb1
sudo mkfs.ext4 /dev/sdb2
```

### Step 3: Populate RootFS

```
mount /dev/sdb2 /mnt
cd /mnt
tar -xzvf /PATH/TO/rootfs.tar.gz
```
---

📫 **Contact:**  
**Email:** [vignesh.d.off@gmail.com](mailto:vignesh.d.off@gmail.com)  
**LinkedIn:** [linkedin.com/in/vignesh-vlsidev](https://www.linkedin.com/in/vignesh-vlsidev)
