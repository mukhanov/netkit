
## 1) Install
```bash
docker-compose -f docker-compose.yml up -d
# Add firewall rules
ufw allow 22 comment "SSH"
ufw allow 8388/udp comment "Socks5"
ufw allow 8811 comment "Socks5"
ufw allow 8080 comment "OpenVPN"
ufw allow 443 comment "OpenVPN"
ufw allow 1194/udp comment "OpenVPN"
ufw allow 444 comment "MTProxy telegram"
```

## 2) OpenVPN
```bash

$ CID=`docker ps | grep dockvpn | awk '{print $1}'` && docker run -t -i -p 8080:8080 --volumes-from $CID umputun/dockvpn serveconfig

https://195.234.32.123:8080/
```
and open OpenVPN configuration on your device by that URL from output

## 3) Telegram
```bash

docker logs telegram-proxy | grep 'auto'
[*]   tg:// link for secret 1 auto configuration: tg://proxy?server=195.234.32.123&port=443&secret=xxxxxxxxxxxxxxxxxxx
```

Change port 443 to 444 and open it. Proxy will be added to telegram client.

## 4) Socks5
Proxy will be available on port 8811 with credentials, defined in docker-compose.yml