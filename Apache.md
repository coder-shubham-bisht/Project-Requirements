# Apache

## 1.1 What is Apache?
open-source, cross-platform web server.

## 1.2 Why Use Apache?

✔ Stability & Reliability – Used in production for decades  
✔ Highly Configurable – Supports modules like mod_rewrite, mod_proxy  
✔ Supports Virtual Hosts – Easily host multiple websites  
✔ Secure – Supports SSL/TLS, authentication, and security modules  
✔ Compatible – Works with PHP, Python, CGI, and other dynamic content  
✔ Cross-platform – Runs on Linux, Windows, macOS  

## Installing Apache
```bash
$ sudo apt update && sudo apt upgrade -y
```

## Enable and Start Apache

```bash
$ sudo systemctl enable apache2
$ sudo systemctl start apache2
```

## Verify Apache Status
```
$ sudo systemctl status apache2
```

**✔ If Apache is running, you should see output like: ** 

```yaml
● apache2.service - The Apache HTTP Server
   Loaded: loaded (/lib/systemd/system/apache2.service; enabled; vendor preset: enabled)
   Active: active (running) since Mon 2024-03-04 12:34:56 UTC; 5min ago

```
## Configure Firewall for Apache
To allow web traffic, open port 80 (HTTP) and 443 (HTTPS):
``` bash
$ sudo ufw allow 'Apache Full'
```
**Check firewall rules:**
```
$ sudo ufw status
```

## Verify Apache Server is working
**Open your browser and go to:**
```
http://YOUR_SERVER_IP  or http://localhost
```
**✔ You should see the Apache default page (/var/www/html/index.html).**

## Running PHP with Apache

**Install PHP**
```
$ sudo apt install php libapache2-mod-php -y
```

**Create a test PHP file**
```
echo "<?php phpinfo(); ?>" >> /var/www/html/info.php
```
**Restart Apache:**
```
sudo systemctl restart apache2
```

**Test in your browser:**
```
http://localhost/info.php
```

## Logs

### Access Logs

```
$ sudo tail -f /var/log/apache2/access.log

```
### Error Logs
```
$ sudo tail -f /var/log/apache2/error.log
```
## Monitoring with GoAccess

**Install**
```
$ sudo apt install goaccess -y
```
**Analyze logs:**
```
$ sudo goaccess /var/log/apache2/access.log --log-format=COMBINED
```

## Enable Apache as a Reverse Proxy

Enable proxy modules:

```
$ sudo a2enmod proxy proxy_http

```
**Create a new site:**
```
$ sudo nano /etc/apache2/sites-available/reverse-proxy.conf
```
**Add this to file**

```
<VirtualHost *:80>
    ServerName proxy.example.com
    ProxyPass / http://backend.example.com/
    ProxyPassReverse / http://backend.example.com/
</VirtualHost>

```
**Enable the site and restart Apache:**
```
$ sudo a2ensite reverse-proxy.conf
$ sudo systemctl restart apache2

```
## Disable Server Signature
```
$ sudo nano /etc/apache2/apache2.conf
```
**Add the following content and save the file**
```
ServerSignature Off
ServerTokens Prod

```
```
$ sudo systemctl restart apache2

```
