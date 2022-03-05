## 0) Configure
Just edit .env file and put your parameters into it.
Also you need to generate key and certificate for SSTP:
```bash
openssl req -nodes -new -x509 -subj '/CN=<YOUR HOSTNAME>/' -keyout key.pem -out cert.pem
```
And put theese values into SSTP_VPN_CERT and SSTP_VPN_KEY variables:
```bash
awk 'NF {sub(/\r/, ""); printf "%s\\n",$0;}' cert.pem #SSTP_VPN_CERT
awk 'NF {sub(/\r/, ""); printf "%s\\n",$0;}' key.pem #SSTP_VPN_KEY
```

## 1) Configure firewall
```bash
# Add firewall rules
ufw allow 22 comment "SSH"
ufw allow 443 comment "SSTP"
ufw allow 500 comment "VPN"
ufw allow 4500/udp comment "VPN"
ufw default deny incoming
```

## 2) Run
```bash
docker-compose -f docker-compose.yml up -d
```

## 3) Connect using L2TP VPN
Add following L2TP VPN settings to your device:
|Name|Value|
| --- | --- |
|Shared key|IPSEC_PRE_SHARED_KEY|
|Account| IPSEC_VPN_USERNAME|
|Password| IPSEC_VPN_PASSWORD|

## 4) Connect using SSTP 
### MacOS:
```bash
brew install sstp-client
sudo sstpc  --log-level 4 --log-stderr --cert-warn --ca-cert /tmp/cert.cert --user <SSTP_VPN_USERNAME> --password <SSTP_VPN_PASSWORD> <HOSTNAME> usepeerdns require-mschap-v2 noauth noipdefault defaultroute refuse-eap noccp
```
#### iOS:
I use https://apps.apple.com/de/app/sstp-connect/id1543667909, it is simple and just works.
#### Windows: 
Use built-in SSTP VPN protocol, but first you need to import cert.pem to your trusted store.
You can read about it here: https://docs.microsoft.com/en-us/skype-sdk/sdn/articles/installing-the-trusted-root-certificate