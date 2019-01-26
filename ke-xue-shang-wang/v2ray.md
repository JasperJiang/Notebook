# V2ray

### Install v2ray

```bash
bash <(curl -L -s https://install.direct/go.sh)
```

### Change Configurations

```bash
vi /etc/v2ray/config.json
```

ss protocol

```text
"inboundDetour": [{
 "protocol": "shadowsocks",
 "port": <port>,
 "settings": {
  "method": "aes-256-cfb",
  "password": <password>,
  "udp": true,
  "level": 1,
  "ota": false
 }
}]
```

### Install BBR

```bash
wget --no-check-certificate https://github.com/teddysun/across/raw/master/bbr.sh && chmod +x bbr.sh && ./bbr.sh
```



