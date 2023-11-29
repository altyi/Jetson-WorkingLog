# Flashing Jetson with initrd

## Setup
These scripts are in  `~/nvidia/nvidia_sdk/<JetPack_*version*>/Linux_for_Tegra/tools/kernel_flash/` after installing SDK manager
- Dependencies
  ```
  cd ~/nvidia/nvidia_sdk/<JetPack_*version*>/Linux_for_Tegra/
  sudo tools/l4t_flash_prerequisites.sh
  ```
- Modify `flash_l4t_nvme.xml` 
  - `NUM_SECTORS` = size of nvme / `sector_size`
  - Change `<device type="external" instance="0" sector_size="512" num_sectors="NUM_SECTORS">` to \
    `<device type="external" instance="0" sector_size="512" num_sectors="1000215216"> `
  - size of nvme = `sector_size` * `num_sectors` = 512 * 1000215216 = 512110190592 bytes

## Flash Steps 
1. Put Jetson in Force Recovery Mode (Jump pin 3 and 4 on seeed studio A203 v2 board 14 pins connector)
2. Connect to host computer via micro usb on the carrier board
3. Run `lsusb` on the host to see if `Nvidia Crop.` shows up
4. Remove the jumper cable for the pins while remaining powered on
5. Run the `l4t_initrd_flash.sh` flashing script
    - Run the following line in `~/nvidia/nvidia_sdk/<JetPack_*version*>/Linux_for_Tegra/`:
      ```
      sudo ADDITIONAL_DTB_OVERLAY_OPT="BootOrderNvme.dtbo" ./tools/kernel_flash/l4t_initrd_flash.sh \
      --external-device nvme0n1p1 -c ./tools/kernel_flash/flash_l4t_nvme.xml --external-only -S 476GiB \
      jetson-xavier-nx-devkit-emmc nvme0n1p1
      ```
7. Connect hdmi to jetson and complete the setup

## Arguments in Step 5
- `ADDITIONAL_DTB_OVERLAY_OPT="BootOrderNvme.dtbo"`
	- boot from NVME
- `--external-device nvme0n1p1`
	- is the name of the external storage device you want to flash as it appears in the '/dev/' folder (i.e nvme0n1, sda)
- `-c ./tools/kernel_flash/flash_l4t_nvme.xml` 
	- -c <external-partition-layout> is the partition layout for the external storage device in XML format
- `--external-only` 
	- flash only the external storage device
- `-S 476GiB` 
	- `-S <APP-size>` the size of the partition that contains the operating system in bytes. \
	  KiB, MiB, GiB shorthand are allowed, for example, 1GiB means 1024 * 1024 * 1024 bytes. \
	  This size cannot be bigger than "num_sectors" * "sector_size" specified in the <external-partition-layout> \
	  (512110190592 bytes = 512GB = 476GiB)
- `jetson-xavier-nx-devkit-emmc`
	- `<board-name>` [Board Name Table](https://files.seeedstudio.com/wiki/A20X/6.png)
- `nvme0n1p1`
	- `<rootdev>` can be set to `"mmcblk0p1"` or `"internal"` for booting from internal \
	  device or `"external"`, `"sda1"` or `"nvme0n1p1"` for booting from external device



 *Modified from `/tools/kernel_flash/README_initrd_flash.txt` (Workflow 3: How to flash to an external storage)*

