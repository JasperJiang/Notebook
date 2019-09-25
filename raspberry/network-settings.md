# network settings

### 配置

```bash
raspi-config
```

### 备份并配置/etc/network/interface

```bash
auto lo
iface lo inet loopback
 
auto eth0
iface eth0 inet dhcp
 
auto wlan0
allow-hotplug wlan0
iface wlan0 inet dhcp
wpa-conf /etc/wpa_supplicant/wpa_supplicant.conf
```

### 重启网络服务sudo /etc/init.d/networking restart



