# Shadowsocks

### Install Shadowsocks

```bash
apt-get update
apt-get install python-pip
export LC_ALL=C
pip install shadowsocks
```

### Change configurations

```bash
vi /etc/shadowsocks.json
```

```text
{
 "server":"0.0.0.0",
 "local_address":"127.0.0.1",
 "local_port":1080,
 "port_password": {
     “prot”: “password”, 
 }, 
 "timeout":600,
 "fast_open": false,
 "method":"aes-256-cfb"
}
```

### Start Shadowsocks

```bash
ssserver -c /etc/shadowsocks.json -d restart
```

