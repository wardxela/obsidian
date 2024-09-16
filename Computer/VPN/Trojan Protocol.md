# Configure Web Server
Install `nginx`
```sh
apt update
apt install nginx
```
replace _ in line **_server_name _;_** with your domain name; e.g. _hahahaha.com_
```sh
vi /etc/nginx/sites-available/default
```
Copy default file _index.nginx-debian.html_ to _index.html_
```sh
cp /var/www/html/index.nginx-debian.html /var/www/html/index.html
```
run nginx server
```sh
systemctl start nginx
```
install and configure certbot
```sh
apt install certbot python3-certbot-nginx
```
configure certybot
```sh
certbot certonly --nginx
```
then the certificates will be stored in _/etc/letsencrypt/live/hahahaha.com/_
```sh
ls -la /etc/letsencrypt/live/hahahaha.com/
```
fix the file permissions
```sh
chmod +rx /etc/letsencrypt/live
chmod +rx /etc/letsencrypt/archive
chmod -R +r /etc/letsencrypt/archive/hahahaha.com
```
# Install Trojan Go
Download latest release from GitHub on server
```sh
cd /tmp/
wget https://github.com/p4gefau1t/trojan-go/releases/download/v0.10.6/trojan-go-linux-amd64.zip
cd ./trojan-go
```
Create/copy necessary files
```sh
mv trojan-go /usr/bin
mkdir /etc/trojan-go
mv example/server.json /etc/trojan-go/
mv geoip.dat geosite.dat /etc/trojan-go/
```
Configure system service:
```sh
vi example/trojan-go@.service
```
And replace the `User` field with `root`
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
	
	"cert": "<server_cert>",
	"key": "<cert_key",
	"sni": "<your_domain>",
	
	"fallback_addr": "127.0.0.1",
	"fallback_port": 443,
	
	"geoip": "/etc/trojan-go/geoip.dat",
	"geosite": "/etc/trojan-go/geosite.dat"
```
Restart server
```sh
systemctl start trojan-go@server.service
```
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