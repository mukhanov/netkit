
1) Build 
```bash
docker-compose -f docker-compose.yml up -d
```

2) Add firewall rules
```bash
ufw allow 22 comment "SSH"
ufw allow 8388/udp comment "Socks5"
ufw allow 8811 comment "Socks5"
ufw allow 8080 comment "OpenVPN"
ufw allow 443 comment "OpenVPN"
ufw allow 1194/udp comment "OpenVPN"
```

3) Share OpenVPN certificate
```bash

$ CID=`docker ps | grep dockvpn | awk '{print $1}'` && docker run -t -i -p 8080:8080 --volumes-from $CID umputun/dockvpn serveconfig

https://195.234.32.123:8080/
```
3) Open OpenVPN configuration on your device by than URL from output

4) Enjoy