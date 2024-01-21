# USB dongle works on Ubuntu 22.04.3 LTS (jammy)

To get the USB dongle (Bus 003 Device 002: ID 10d7:b012 Actions general adapter) working on Ubuntu 22.04.3 LTS (jammy), it is necessary to install a newer kernel and the corresponding packages. Also, make sure that your Dockerfile matches the following format:

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

This information will help users to quickly set up their system to work with the USB dongle on Ubuntu 22.04.3 LTS (jammy).
