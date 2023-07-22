# raspberry-pi-4
Raspberry Pi 4 Setup Notes

[Installing MongoDB](https://www.mongodb.com/developer/products/mongodb/mongodb-on-raspberry-pi/)

[Automate Continuous MongoDB to S3](https://www.mongodb.com/developer/products/atlas/automated-continuous-data-copying-from-mongodb-to-s3/)

### Ethernet over USB-C

https://www.hardill.me.uk/wordpress/2019/11/02/pi4-usb-c-gadget/

https://www.reddit.com/r/RGNets/comments/tsmx97/raspberry_pi_use_usb_c_port_as_ethernet/

Edit `/boot/config.txt` and add the following to the end of the file:
```
dtoverlay=dwc2
```

Edit `/boot/cmdline.txt` and add the following on a new line at the end of the file:
```
modules-load=dwc2
```

Editing may require `sudo nano`

Edit `/etc/modules` and add the following the the end of the file:
```
libcomposite
```

Edit `/etc/dhcpcd.conf` and add the following to the end of the file:
```
denyinterfaces usb0
```

#### Install `dnsmasq`
```
sudo apt-get install dnsmasq
```

Create the following file:
```
sudo touch /etc/dnsmasq.d/usb
```
```
interface=usb0
dhcp-range=10.55.0.2,10.55.0.6,255.255.255.248,1h
dhcp-option=3
leasefile-ro
```

Create `/etc/network/interfaces.d/usb0` with the following content:
```
auto usb0
allow-hotplug usb0
iface usb0 inet static
  address 10.55.0.1
  netmask 255.255.255.248
```

Switch to root user:
```
sudo su
```

Create the following file and edit permissions:
```
sudo touch /root/usb.sh
sudo chmod +x /root/usb.sh
```
```
#!/bin/bash
cd /sys/kernel/config/usb_gadget/
mkdir -p pi4
cd pi4
echo 0x1d6b > idVendor # Linux Foundation
echo 0x0104 > idProduct # Multifunction Composite Gadget
echo 0x0100 > bcdDevice # v1.0.0
echo 0x0200 > bcdUSB # USB2
echo 0xEF > bDeviceClass
echo 0x02 > bDeviceSubClass
echo 0x01 > bDeviceProtocol
mkdir -p strings/0x409
echo "fedcba9876543211" > strings/0x409/serialnumber
echo "Ben Hardill" > strings/0x409/manufacturer
echo "PI4 USB Device" > strings/0x409/product
mkdir -p configs/c.1/strings/0x409
echo "Config 1: ECM network" > configs/c.1/strings/0x409/configuration
echo 250 > configs/c.1/MaxPower
# Add functions here
# see gadget configurations below
# End functions
mkdir -p functions/ecm.usb0
HOST="00:dc:c8:f7:75:14" # "HostPC"
SELF="00:dd:dc:eb:6d:a1" # "BadUSB"
echo $HOST > functions/ecm.usb0/host_addr
echo $SELF > functions/ecm.usb0/dev_addr
ln -s functions/ecm.usb0 configs/c.1/
udevadm settle -t 5 || :
ls /sys/class/udc > UDC
ifup usb0
service dnsmasq restart
```

Next edit `/etc/rc.local`:

Add `sh /root/usb.sh` right before the line `exit 0`

### Connect

```
ssh <your-username>@10.55.0.1
```

Code Server
```
10.55.0.1:8080
```
