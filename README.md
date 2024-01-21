# USB dongle works on Ubuntu 22.04.3 LTS (jammy)

To get the USB dongle (
```Bus 003 Device 002: ID 10d7:b012 Actions general adapter```
) working on Ubuntu 22.04.3 LTS (jammy), it is necessary to install a newer kernel and the corresponding packages. Also, make sure that your Dockerfile matches the following format:

```
homeassistant:
  container_name: homeassistant
  image: "ghcr.io/home-assistant/home-assistant:latest"
  cap_add:
    - ALL
  volumes:
    - ./config:/config
    - /etc/localtime:/etc/localtime:ro
    - /var/run/dbus/:/var/run/dbus/
    - /dev/vhci:/dev/vhci
    - /dev/bus/usb:/dev/bus/usb
    - /run/udev:/run/udev
    - /sys/fs/cgroup:/sys/fs/cgroup
    - /sys:/sys
  restart: unless-stopped
  privileged: true
  network_mode: host
```
## Instructions for installing a newer kernel and packages

To install a newer kernel and the necessary packages on Ubuntu 22.04.3 LTS (jammy), execute the following commands:

```
sudo apt update
sudo apt install linux-headers-6.6.10-060610 linux-image-unsigned-6.6.10-060610-generic linux-modules-6.6.10-060610-generic
```

After installing the newer kernel and packages, the USB dongle should be fully functional on your distribution.

you can check this with command(inside container):

```
hciconfig -a
```

it should answer like this:

```

root@smallguy:~# docker exec -it homeassistant bash
smallguy:/config# hciconfig -a
hci0:   Type: Primary  Bus: USB
        BD Address: F4:4E:FC:01:B1:89  ACL MTU: 679:6  SCO MTU: 240:20
        UP RUNNING
        RX bytes:15021500 acl:0 sco:0 events:705328 errors:0
        TX bytes:204453 acl:0 sco:0 commands:28007 errors:0
        Features: 0xbf 0xfe 0x0d 0xfe 0xdb 0xfd 0x7b 0x87
        Packet type: DM1 DM3 DM5 DH1 DH3 DH5 HV1 HV2 HV3
        Link policy: RSWITCH SNIFF
        Link mode: PERIPHERAL ACCEPT
        Name: 'smallguy'
        Class: 0x00010c
        Service Classes: Unspecified
        Device Class: Computer, Laptop
        HCI Version:  (0xc)  Revision: 0x201
        LMP Version:  (0xc)  Subversion: 0x201
        Manufacturer: Actions (Zhuhai) Technology Co., Limited (992)
smallguy:/config#

```

This information will help users to quickly set up their system to work with the bluetooth 5.3 USB dongle on Ubuntu 22.04.3 LTS (jammy).
