# tplink-archer-c7-openwrt

tl;dr This is a guide which steps through installing OpenWRT to TP-Link Archer C7

You will need
---

TP-Link Archer C7 AC1750 Ver:2.0: http://www.tp-link.com/en/products/details/cat-9_Archer-C7.html
 
OpenWRT: http://wiki.openwrt.org/toh/tp-link/archer-c5-c7-wdr7500

Direct firmware download link: https://downloads.openwrt.org/latest/ar71xx/generic/openwrt-15.05.1-ar71xx-generic-archer-c7-v2-squashfs-factory.bin

UPDATE Nov 17:
---
There seems to be a bug in ath10k kernel module causing connection drops becuse of no ack received from wireless station (eg. your laptop) to AP (the router).
```
Mar 27 08:13:56 xxxxxx daemon.info hostapd: wlan0: STA xx:xx:xx:xx:xx:xx
IEEE 802.11: disconnected due to excessive missing ACKs
```
https://forum.openwrt.org/viewtopic.php?id=43188

Ack's are sent correctly but bug in ath10k causes them to disappear and low_ack is fired causing station auto-disassociation. Workaround for this is to disable 'disassoc_low_ack' feature (with a value of '0') to /etc/config/wireless under desired wifi-iface configuration.
```
# cat /etc/config/wireless
config wifi-iface
  option device 'radio0'
  option network 'lan'
  option mode 'ap'
  option disassoc_low_ack '0'
  ...
```

Retail box
---
![unboxed](https://raw.githubusercontent.com/enyone/tplink-archer-c7-openwrt/master/unboxed.JPG)

Labels at box
---
Labels telling the SN and HW revision. Check that the revision is:
```
Ver:2.0
```

![revision_labels](https://raw.githubusercontent.com/enyone/tplink-archer-c7-openwrt/master/revision_labels.JPG)

The unit itself and the backpanel
---
![theunit](https://raw.githubusercontent.com/enyone/tplink-archer-c7-openwrt/master/theunit.JPG)
![backpanel](https://raw.githubusercontent.com/enyone/tplink-archer-c7-openwrt/master/backpanel.JPG)

Stock firmware login
---
Connect unit's LAN1 (yellow) port to your computer with a RJ45 cable. Do not use a static ip address at your computer, use DHCP instead. Power up the device, wait 60secs and then navigate to http://192.168.0.1/ and login with admin:admin

![stock_login](https://raw.githubusercontent.com/enyone/tplink-archer-c7-openwrt/master/stock_login.jpg)

Stock firmware update
---
Navigate to System Tools -> Firmware Upgrade and select downloaded *-factory.bin file. Then click Upgrade.

![stock_update](https://raw.githubusercontent.com/enyone/tplink-archer-c7-openwrt/master/stock_update.jpg)

Upgrade process
---
It takes ~2mins to complete. After the update the unit reboots.
![stock_updating](https://raw.githubusercontent.com/enyone/tplink-archer-c7-openwrt/master/stock_updating.jpg)

OpenWRT login
---
After the unit's reboot you need to unplug and then re-plug RJ45 cable, this causes the DHCP to renew the ip address which is now changed to http://192.168.1.1/ Now navigate there and login with root:root

![openwrt_login](https://raw.githubusercontent.com/enyone/tplink-archer-c7-openwrt/master/openwrt_login.jpg)

Further reading
---
In fact OpenWRT is designed to be used through a command line interface but someone has developed a very decent web interface called LuCI.

OpenWRT LuCI web interface tour https://www.youtube.com/watch?v=kLLtZH2Bc0w
![openwrt_login](https://raw.githubusercontent.com/enyone/tplink-archer-c7-openwrt/master/openwrt_working.jpg)
