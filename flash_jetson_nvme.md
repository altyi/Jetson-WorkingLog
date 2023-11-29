# Flashing Jetson with initrd

## Setup
These scripts are in  ~/nvidia/nvidia_sdk/<JetPack_*version*>/Linux_for_Tegra/tools/kernel_flash/ after installing SDK manager
- Dependencies
```
cd ~/nvidia/nvidia_sdk/<JetPack_*version*>/Linux_for_Tegra/
sudo tools/l4t_flash_prerequisites.sh
```
- Modify `flash_l4t_nvme.xml`
Located in /tools/kernel_flash/ (NUM_SECTORS = size of nvme / sector_size)
```
- <device type="external" instance="0" sector_size="512" num_sectors="NUM_SECTORS"> to
- <device type="external" instance="0" sector_size="512" num_sectors="1000215216"> 
- size of nvme = sector_size * num_sectors = 512 * 1000215216 = 512110190592 bytes
```

