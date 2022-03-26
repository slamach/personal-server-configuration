# Let's Encrypt Certbot installation and configuration

## Installing cerbot and its nginx plugin

```bash
sudo apt install certbot python3-certbot-nginx
```

After installing the required certificates, check if their auto-renewal is working:
```bash
sudo systemctl status certbot.timer
sudo certbot renew --dry-run
```

## Adding new sertificate

1. Configure website without 443 port and SSL. Example of a simple configuration before installing a certificate:
```nginx
server {
        listen 80;
        listen [::]:80;

        server_name example.com;
        root /var/www/example.com/htdocs;
        location / {
                try_files $uri $uri/ =404;
        }
}
```
2. Install certificate:
```bash
certbot -d example.com -d www.example.com
```
