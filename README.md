
## 1) Install
```bash
docker-compose -f docker-compose.yml up -d
# Add firewall rules
ufw allow 22 comment "SSH"
ufw allow 8388/udp comment "Socks5"
ufw allow 8811 comment "Socks5"
ufw allow 500 comment "VPN"
ufw allow 4500/udp comment "VPN"
ufw allow 444 comment "MTProxy telegram"
ufw default deny incoming
```

## 2) VPN
Add following VPN settings to your device:
|||
|-|-|
|Type| L2TP|
|Shared key|your_ipsec_pre_shared_key|
|Account| your_vpn_username|
|Password| your_vpn_password|

## 3) Telegram
```bash

docker logs telegram-proxy | grep 'auto'
[*]   tg:// link for secret 1 auto configuration: tg://proxy?server=195.234.32.123&port=443&secret=xxxxxxxxxxxxxxxxxxx
```

Change port 443 to 444 and open it. Proxy will be added to telegram client.

## 4) Socks5
Proxy will be available on port 8811 with credentials, defined in docker-compose.yml