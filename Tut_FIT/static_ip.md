# Set static IP for ubuntu interface

## check interface names 
```
ip link
```

## check default route 
```
ip route | grep default
```

## locate config file
```
cd /etc/netplan 
```

## locate the associated config file 
```
grep -iH IFACE_NAME *.*
```

## edit the config file
```
sudo nano 00-installer-config.yaml
```

## get the DNS server ip
```
nslookup www.google.com | grep server
```

## config
```
network:
  ethernets:
    eth0:
    dhcp4: no
      addresses:
        - 172.19.215.19/20
      gateway4: 192.168.108.1
      nameservers:
        addresses: [127.0.0.53, 8.8.8.8]
  version: 2
```
## Apply settings
```
sudo netmap apply
```
