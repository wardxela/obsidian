# Server
## Set Up Fake Web Server
Install packages
```bash
apt update
apt install nginx certbot python3-certbot-nginx
```
Configure `nginx`
```bash
vi /etc/nginx/sites-available/default
```
Replace `server_name` with your domain, e.g *wardxeladog.work.gd*
```nginx
server_name wardxeladog.work.gd;
```
Copy default html
```bash
cp /var/www/html/index.nginx-debian.html /var/www/html/index.html
```
Run nginx server
```bash
systemctl start nginx
```
Then issue the certificates. Note that we use `certonly` subcommand since the certificates won't be used directly by nginx but by the Trojan Server that we are gonna set up later.
```bash
certbot certonly --nginx
```
The certificates are stored in _/etc/letsencrypt/live/your_domain/_
```bash
ls -la /etc/letsencrypt/live/wardxeladog.work.gd/
```
Fix the file permissions
```bash
chmod +rx /etc/letsencrypt/live
chmod +rx /etc/letsencrypt/archive
chmod -R +r /etc/letsencrypt/archive/wardxeladog.work.gd
```
## Set Up Trojan Go
Download latest release from GitHub
```bash
cd /tmp/
wget https://github.com/p4gefau1t/trojan-go/releases/download/v0.10.6/trojan-go-linux-amd64.zip
unzip trojan-go-linux-amd64.zip -d ./trojan-go
cd ./trojan-go
```
Create necessary files
```bash
mv trojan-go /usr/bin
mkdir /etc/trojan-go
mv example/server.json /etc/trojan-go/
mv geoip.dat geosite.dat /etc/trojan-go/
```
Configure trojan as a service
```bash
vi example/trojan-go@.service
```
Replace `User` with `root`
```yaml
User=root
```
And register
```bash
mv example/trojan-go@.service /etc/systemd/system
systemctl daemon-reload
systemctl enable trojan-go@server.service
```
Configure `server.json`
```json
"password": [ "<your_password>" ],
"ssl": {
	"cert": "/etc/letsencrypt/live/wardxeladog.work.gd/fullchain.pem",
	"key": "/etc/letsencrypt/live/wardxeladog.work.gd/privkey.pem",
	"sni": "wardxeladog.work.gd",
	"fallback_addr": "127.0.0.1",
	"fallback_port": 443,
},
"router": {
	"geoip": "/etc/trojan-go/geoip.dat",
	"geosite": "/etc/trojan-go/geosite.dat"
}
```
Restart server
```bash
systemctl start trojan-go@server.service
```
## Advanced Security Settings
Disable two-way ping:
```bash
iptables -A INPUT -p icmp --icmp-type echo-request -j DROP
iptables -A OUTPUT -p icmp --icmp-type echo-reply -j DROP
```
Hide open web proxy ports:
```bash
iptables -A INPUT -p tcp --dport 80 -j DROP
```
# Configure Client
Configuring client is much easier and straightforward. Just install binary, adjust config and run. That's all. Text me if you wish to know more.

## Make websites think you are not protected
- [Disable WebRTC in your browser](https://github.com/K3V1991/How-to-disable-WebRTC-in-Chrome-Firefox-Safari-Opera-and-Edge)
# Resources
- [Trojan Go Implementation](https://github.com/p4gefau1t/trojan-go)
- [Trojan C++ Implementation](https://github.com/trojan-gfw/trojan)
- [Setup Cloudflare CDN protected Trojan-Go with Docker on Ubuntu 20.04](https://thematrix.dev/setup-cloudflare-cdn-protected-trojan-go-using-docker-on-ubuntu-20-04/)
- [Trojan Go tutorial | Server and client manual installation and configuration](https://youtu.be/Ymlc_Pjhm8s?si=FTTopRv8TO1K_Z7g)
- [Install Trojan-GFW Server on Ubuntu Linux](https://sedap.github.io/install-trojan-gfw-on-ubuntu.html)
- [How to Install, Configure, and Run Trojan-GFW](https://oilandfish.net/posts/trojan-gfw.html)
- [Trojan go](https://note.kiui.moe/web/proxy/trojan-go/#clash-client-recommended)