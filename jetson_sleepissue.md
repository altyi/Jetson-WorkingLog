# Jetson SleepIssue Testing

## Jetson
- #### Get MAC address
  - SSH into Jetson
    - `ifconfig | grep -i hwaddr`
- #### Wake on LAN (WOL)
  - Check WOL Status
    - `ethtool eth0|grep -i wake` 
  - Enable WOL ####(Jetson)
    - `sudo ethtool -s eth0 wol g`
  - Put Jetson into Sleep State (Deep Sleep (SC7))
    - `sudo systemctl suspend`

## Host
- Ping and SSH to see the response
- #### Install wakeonlan
  - `sudo apt-get install wakeonlan`
- #### WOL
  - `wakeonline -i` 
  - `wakeonlan -i <broadcast-address> <mac-address>`


## References  
- [Jetson Sleep State](https://forums.developer.nvidia.com/t/sleep-state/68201/7)
- [Jeton WOL](https://forums.developer.nvidia.com/t/help-jetson-nano-and-wake-on-lan/239871)
