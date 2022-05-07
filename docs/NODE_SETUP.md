# Node setuo

# Create SSD image

1. Download Raspberry Pi Imager
2. Create a SSD boot with Ubuntu Server (currently 22.04 LTS)

# Boot your device

1. Wait until initial startup configuration is done
2. Log in using default (ubuntu/ubuntu) or your own login

# Fix keyboard layout

1. Run the command `sudo dpkg-reconfingure keyboard-configuration`
2. Follow the steps and select the default hotkeys

# Configure network

1. Check network interface using `ls /sys/class/net`
2. Locate the correct Netplan configuration files using `ls /etc/netplan/`, name is typically 50-cloud-init.yaml
3. Edit the configuration file accordingly:

```
network:
    ethernets:
        eth0:
            dhcp4: true
            optional: true
    version: 2
    wifis:
        wlp3s0:
            optional: true
            access-points:
                "SSID-NAME-HERE":
                    password: "PASSWORD-HERE"
            dhcp4: false
	    addresses:
	      - 192.168.0.XXX/24
	    route:
	      - to: default
	      - via: 192.168.1.1
	    nameservers:
		addresses: [8.8.8.8, 8.8.4.4, 192.168.1.1]
```
4. Apply the new configuration running `sudo netplan apply`
5. Test your connection by running a ping after some time

# Connect to device with SSH

1. Run `ssh ubuntu@192.168.0.XXX` and enter your password
2. You should now be connected to the cluster device

