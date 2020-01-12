# V2ray+ws+tls

## install v2ray

bash &lt;\(curl -L -s https://install.direct/go.sh\)

## Add streamSettings in v2ray configuration

```javascript
"inbounds": [
		{
			"port": port,
			"protocol": "vmess",
			"settings": {
				"clients": [
					{
						"id": "uuid",
						"level": 1,
						"alterId": 233
					}
				]
			},
			"streamSettings": {
				"network": "ws"
			}
		}
	],
```

## Install Caddy

```bash
curl https://getcaddy.com | sudo bash -s personal

sudo mkdir /etc/caddy
sudo touch /etc/caddy/Caddyfile
sudo mkdir /etc/ssl/caddy
sudo chmod 777 /etc/ssl/caddy

#make caddy as a service
sudo curl -s https://raw.githubusercontent.com/mholt/caddy/master/dist/init/linux-systemd/caddy.service -o /etc/systemd/system/caddy.service

# !!!for centos change user & group in  /etc/systemd/system/caddy.service

sudo systemctl daemon-reload
sudo systemctl enable caddy.service
sudo systemctl status caddy.service


```

## edit /etc/caddy/Caddyfile

```javascript
domain.com
{
  tls abc@gmail.com
  proxy / localhost:port {
    websocket
  }
}

```

