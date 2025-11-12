
********************************PETALINUX COMMANDS*************************************




------------------------------project flow commands-----------------------------------

source <plnx_dir>/petalinux_2024.2/settings.sh 
petalinux-create project --template zynqMP --name test_3
petalinux-config --get-hw-description <<path to xsa directory>>
petalinux-config -c kernel
petalinux-config -c rootfs
petalinux-build
petalinux-config -c device-tree
petalinux-package boot --u-boot
petalinux-package wic

source components/yocto/layers/core/oe-init-build-env components/yocto   ----- to source bitbake

***INFO***-to configure node in system-user.dtsi, first create project, define .xsa , add or change whatever want in system-user.dtsi in 
<<plnx-proj-root>/project-spec/meta-user/recipes-bsp/device-tree/files/system-user.dtsi> , then build-device tree then , build project, also dont do wic , flash mannually

petalinux-build -c device-tree -x cleansstate
petalinux-build -c device-tree

petalinux-build -x mrproper

sudo dd if=petalinux-sdimage.wic of=/dev/sdb conv=fsync status=progress   ----- for wic image etching

petalinux-package --wic --images-dir images/linux/ --bootfiles "ramdisk.cpio.gz.u-boot,boot.scr,Image,system.dtb,system-zynqmp-sck-kv-g-revB.dtb"   ----- for creating wic image

-------------------------setting up environment--------------------------------

mmc dev 1
setenv bootargs "console=ttyPS0,115200 root=/dev/mmcblk0p2 rw rootwait earlyprintk"  or
setenv bootargs "console=ttyPS0,115200 root=/dev/mmcblk1p2 rw rootwait"
setenv bootargs "console=ttyPS0,115200 root=/dev/mmcblk1p2 rw rootwait earlycon"
saveenv
fatload mmc 1:1 0x8000000 Image

fatload mmc 1:1 0xA000000 system-zynqmp-sck-kv-g-revB.dtb
fatload mmc 1:1 0xA000000 system.dtb

booti 0x8000000 - 0xA000000


printenv loadaddr
iminfo 0x4000000
fatls mmc 1:1
ext4ls mmc 1:2 /
env default -a
saveenv
mmc dev

----------------------------partition sd card----------------------------------

sudo fdisk /dev/sdb
Command (m for help): n
Select (default p): p
21111220
[Y]es/[N]o: y
n,p --> enter w
sudo mkfs.vfat /dev/sdb1
sudo mkfs.ext4 /dev/sdb2

-> mount /dev/sdb2 /mnt
-> cd /mnt
-> tar -xzvf /PATH/TO/rootfs.tar.gz

 ----------------------------bootargs related----------------------------------


dtc -I dtb -O dts -o system.dts system.dtb      --- decompile
dtc -I dts -O dtb -o new_system.dtb system.dts    ----- compile


chosen {
                bootargs = "earlycon console=ttyPS0,115200 cpuidle.off=1 maxcpus=1 root=/dev/mmcblk1p2 ro rootwait cma=1000M";
                stdout-path = "ttyPS0:115200n8";
        };
        
        
bootargs = "earlycon console=ttyPS0,115200 cpuidle.off=1 maxcpus=1 root=/dev/mmcblk1p2 ro rootwait cma=1000M";


-----------------commands for generating the DTSI from the xsa using --xsct-------------------

git clone https://github.com/Xilinx/device-tree-xlnx
cd device-tree-xlnx
git checkout xlnx_rel_v<version>

git clone https://git.kernel.org/pub/scm/utils/dtc/dtc.git
cd dtc
make
export PATH=$PATH:/<path-to-dtc>/dtc

source <vitis to run xsct>
hsi open_hw_design <design_name>.<xsa|hdf>
hsi set_repo_path <path to device-tree-xlnx repository>
set procs [hsi get_cells -hier -filter {IP_TYPE==PROCESSOR}]
puts "List of processors found in XSA is $procs"
hsi create_sw_design device-tree -os device_tree -proc <choose processor>
hsi generate_target -dir my_dts
hsi close_hw_design [hsi current_hw_design]
exit

---------------------------configuring on errors--------------------------------

ERROR: Nothing RPROVIDES 'libvCU-OMXIL' ----> <proj-root>/project-spec/meta-user/conf/petalinuxbsp.conf

add line : MACHINE_FEATURES:append = " vcu"

-------------

Waiting for root device dev/mmcblk<x>p<x>   ---> add node or flash mannually

add line in system-user.dtsi -->

&sdhci1 {
	no-1-8-v;
	bus-width = <4>;
	disable-wp;
	cd-gpios = <&gpio 45 1>;	// cd-gpios = <&gpio 45 GPIO_ACTIVE_LOW>;
	cd-debounce-delay-ms = <500>;
	max-frequency = <100000000>;
	cap-sd-highspeed;
	sd-uhs-sdr12;
	sd-uhs-sdr25;
	sd-uhs-sdr50;
	sd-uhs-ddr50;
	keep-power-in-suspend;
	status = "okay";
};

-------------


