# ASUS Router Replacement Guide for ExperiaBox

Welcome to the ASUS Router Replacement Guide for ExperiaBox. This comprehensive guide provides detailed instructions, configuration files, and valuable tips to seamlessly replace your ExperiaBox with an ASUS router, ensuring you maintain your internet connection and IPTV services.

**Tested with an ASUS RT-AX86U Pro** running Asuswrt-Merlin version **RT-AX86U_PRO_3004_388.4_0**.

Please note that this guide is specifically designed for KPN Internet and IPTV and does not cover VOIP configurations. In the future, we plan to add configuration files for other ASUS routers.

## Compatibility with Other Providers

This guide may also be compatible with providers such as Telfort and XS4All. However, it may require adjustments to VLAN IDs and other configurations. For more information on these changes, please refer to the [main forum discussion](https://gathering.tweakers.net/forum/list_messages/1772709/0).

### Note for Configuration Files

In the forums, it is suggested that you need to have either the file `wan-start` or `services-start`, depending on your provider and old or new router. In my case, I used `services-start`.

## Compatibility with Various ASUS Routers

This guide may also be compatible with ASUS AC and AX routers. However, you may need to make adjustments in the files and file names like `igmpproxy.conf` or `mcpd.conf` and configurations. For more information on these changes, please refer to the [main forum discussion](https://gathering.tweakers.net/forum/list_messages/1772709/0).

Also, for routers with 8 ports or other models like the **ASUS RT-AX58U**, you may need to change values in the file to correspond to the correct port layout (e.g., changing `/sbin/vconfig add eth0 4` to `/sbin/vconfig add eth4 4`). Refer to the `services-start` file and check the `interfaces` file for more details.

If you have an **ASUS RT-AC88U**, please refer to [zmoox/ASUS_KPN_IPTV](https://github.com/zmoox/ASUS_KPN_IPTV).

## Finding the Perfect Router

My journey to finding a suitable replacement for the KPN Experiabox led me to recommend ASUS routers due to their greater customizability and versatility. After careful consideration, we settled on the ASUS RT-AX86U Pro, which met our essential requirements:

- **Future-Proof**: Supports WiFi 6 and features a 2.5G port.
- **VLAN Support**: Essential for network segmentation.
- **Third-Party Firmware Compatibility**: Compatible with [Asuswrt-Merlin](https://www.asuswrt-merlin.net/), offering enhanced functionality.

[ASUS RT-AX86U Pro](https://www.asus.com/networking-iot-servers/wifi-routers/asus-gaming-routers/rt-ax86u-pro/)

However, we encountered an unexpected challenge: getting IPTV to work seamlessly. To address this issue, we conducted extensive research on various sites and forums, and here's what we discovered...

## Requirements for IPTV

For this guide, we focus on Routed IPTV to enable both internet and IPTV with just one Ethernet cable. Please ensure that your switch supports **IGMP Snooping**.

## Notice / Warning

When editing the files using tools like VSCode or Notepad++, make sure that the End of Line (EOL) Sequences are set to **LF** (Line Feed) and not CRLF (Carriage Return + Line Feed) to avoid errors.

## Documentation Sources

### General
- [Asuswrt-Merlin](https://www.asuswrt-merlin.net/)
- [Netwerkje - Eigen Router](https://netwerkje.com/eigen-router)
- [Netwerkje - Routed IPTV](https://netwerkje.com/routed-iptv)
- [Tweakers Main Forum](https://gathering.tweakers.net/forum/list_messages/1772709/0)

### Specific
- [Guide for AX routers](https://gathering.tweakers.net/forum/list_message/63414488#63414488)
- [Files for 4-port router](https://bashoogers.nl/tweakers/ASUS_SCRIPTS_NOVOIP.rar)
- [Manual for setting up an ASUS router with KPN network](https://bashoogers.nl/tweakers/V4_HANDLEIDING_EIGENROUTERKPN.pdf)
- [Inspiring repo with configuration and guide for an ASUS RT-AC88U](https://github.com/zmoox/ASUS_KPN_IPTV)

### Research for My Use Case
- [Tweakers Forum Post 1](https://gathering.tweakers.net/forum/list_messages/1772709/35)
- [Tweakers Forum Post 2](https://gathering.tweakers.net/forum/list_messages/1772709/36)
- [Tweakers Forum Post 3](https://gathering.tweakers.net/forum/list_messages/1772709/37)
- [Tweakers Forum Post 4](https://gathering.tweakers.net/forum/list_messages/1772709/38)
- [Tweakers Forum Post 5](https://gathering.tweakers.net/forum/list_message/71784070#71784070)
- [Tweakers Forum Post 6](https://gathering.tweakers.net/forum/list_messages/1772709/2?data%5Bfilter_keywords%5D=ax86u&data%5Bboolean%5D=)

## Guide

Make sure to carefully read through the steps before configuring!

### Pre-setup

Before proceeding with the setup, ensure that the router has only the WAN cable inserted into the WAN port on the back.

### Internet Access
Follow steps 2 and 5 from the [guide](https://bashoogers.nl/tweakers/V4_HANDLEIDING_EIGENROUTERKPN.pdf). Additionally, take a look at step 1 of the written [manual](https://github.com/zmoox/ASUS_KPN_IPTV/blob/main/README.md).

### Routed IPTV
Now, let's set up the configuration for IPTV.

Before starting, clone this repository or download the 'routers' directory/folder to your device.

**There is only one adjustment needed in the 'dnsmasq.conf.add' file: Change the '<IP>' value to correspond to the IP address of your router's configurable/custom range (e.g., 192.168.1.1), and change the digits after the last '.' to 255 (e.g., 192.168.1.255).**

Ensure you have SSH or Putty installed and follow step 2 of the [manual](https://github.com/zmoox/ASUS_KPN_IPTV/blob/main/README.md). For more context, refer to steps 3 and 4 of the [guide](https://bashoogers.nl/tweakers/V4_HANDLEIDING_EIGENROUTERKPN.pdf).

Tip: When in the CLI, make sure you are in the correct directory of jffs and not in the tmp/jffs by changing the directory to 'cd /'.

### Testing

To ensure everything is working correctly, follow these steps:

- Check which port is up/down: `ethctl phy-map`
- Verify if the correct `mcpd.conf` is loaded: `cat /var/mcpd.conf`
- Verify if VLAN-4 has been assigned an IP address: `ifconfig vlan4`
- Verify if VLAN-6 (depending on your provider) has been assigned an IP address: `ifconfig vlan6`

These tests will help confirm that your IPTV setup is functioning as expected.

### Setup

Follow these steps to set up your IPTV:

1. Reboot the router.

2. While the router is rebooting, disconnect the IPTV devices from the power outlet.

3. Wait for 30 seconds.

4. Attach the switch or IPTV devices to the router's port(s).

If everything is configured correctly, you should now have a working IPTV setup!
