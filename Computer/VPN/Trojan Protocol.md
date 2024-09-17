# Setting Up Fake Web Server
Install packages
```sh
apt update
apt install nginx certbot python3-certbot-nginx
```
Configure nginx
```sh
vi /etc/nginx/sites-available/default
```
Change corresponding fields
```conf
server_name <server_name>;
```
Copy default file _index.nginx-debian.html_ to _index.html_
```sh
cp /var/www/html/index.nginx-debian.html /var/www/html/index.html
```
Run nginx server
```sh
systemctl start nginx
```
Issue certificates. Note that we use `certonly` since certificates will be used later by trojan server.
```sh
certbot certonly --nginx
```
Then the certificates will be stored in _/etc/letsencrypt/live/wardxeladog.work.gd/_
```sh
ls -la /etc/letsencrypt/live/wardxeladog.work.gd/
```
Fix the file permissions
```sh
chmod +rx /etc/letsencrypt/live
chmod +rx /etc/letsencrypt/archive
chmod -R +r /etc/letsencrypt/archive/wardxeladog.work.gd
```
# Setting Up Trojan Go
Download latest release from GitHub
```sh
cd /tmp/
wget https://github.com/p4gefau1t/trojan-go/releases/download/v0.10.6/trojan-go-linux-amd64.zip
unzip trojan-go-linux-amd64.zip -d ./trojan-go
cd ./trojan-go
```
Create necessary files
```sh
mv trojan-go /usr/bin
mkdir /etc/trojan-go
mv example/server.json /etc/trojan-go/
mv geoip.dat geosite.dat /etc/trojan-go/
```
Configure trojan as a service
```sh
vi example/trojan-go@.service
```
Replace the `User` field with `root`
```yaml
User=root
```
And finally register the service
```sh
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
```sh
systemctl start trojan-go@server.service
```
# Advanced settings
Disable two-way ping:
```sh
iptables -A INPUT -p icmp --icmp-type echo-request -j DROP
iptables -A OUTPUT -p icmp --icmp-type echo-reply -j DROP
```
Hide open web proxy ports:

# Configure Client

# Make websites think you are not protected
- [Disable WebRTC in your browser](https://github.com/K3V1991/How-to-disable-WebRTC-in-Chrome-Firefox-Safari-Opera-and-Edge)
# Resources
- [Trojan Go Implementation](https://github.com/p4gefau1t/trojan-go)
- [Trojan C++ Implementation](https://github.com/trojan-gfw/trojan)
- [Setup Cloudflare CDN protected Trojan-Go with Docker on Ubuntu 20.04](https://thematrix.dev/setup-cloudflare-cdn-protected-trojan-go-using-docker-on-ubuntu-20-04/)
- [Trojan Go tutorial | Server and client manual installation and configuration](https://youtu.be/Ymlc_Pjhm8s?si=FTTopRv8TO1K_Z7g)
- [Install Trojan-GFW Server on Ubuntu Linux](https://sedap.github.io/install-trojan-gfw-on-ubuntu.html)
- [How to Install, Configure, and Run Trojan-GFW](https://oilandfish.net/posts/trojan-gfw.html)