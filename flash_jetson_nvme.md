# Flashing Jetson with initrd

## Setup
These scripts are in  `~/nvidia/nvidia_sdk/<JetPack_*version*>/Linux_for_Tegra/tools/kernel_flash/` after installing SDK manager
- Dependencies
  ```
  cd ~/nvidia/nvidia_sdk/<JetPack_*version*>/Linux_for_Tegra/
  sudo tools/l4t_flash_prerequisites.sh
  ```
- Modify `flash_l4t_nvme.xml` 
  - `NUM_SECTORS` = size of nvme / sector_size
  ```
  - <device type="external" instance="0" sector_size="512" num_sectors="NUM_SECTORS"> to
  - <device type="external" instance="0" sector_size="512" num_sectors="1000215216"> 
  - size of nvme = sector_size * num_sectors = 512 * 1000215216 = 512110190592 bytes
  ```
## Flash Steps 
1. Put Jetson in Force Recovery Mode (Jump pin 3 and 4 on seeed studio A203 v2 board 14 pins connector)
2. Connect to host computer via micro usb on the carrier board
3. Run `lsusb` on the host to see if `Nvidia Crop.` shows up
4. Remove the jumper cable for the pins while remaining powered on
5. Run the `l4t_initrd_flash.sh` flashing script
    - Run the following command in `~/nvidia/nvidia_sdk/<JetPack_*version*>/Linux_for_Tegra/`:
      ```
      sudo ADDITIONAL_DTB_OVERLAY_OPT="BootOrderNvme.dtbo" ./tools/kernel_flash/l4t_initrd_flash.sh \
      --external-device nvme0n1p1 -c ./tools/kernel_flash/flash_l4t_nvme.xml --external-only -S 476GiB \
      jetson-xavier-nx-devkit-emmc nvme0n1p1
      ```
7. Connect hdmi to jetson and complete the setup

