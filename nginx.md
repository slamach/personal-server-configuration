# Nginx installation and configuration

## Installing nginx

```bash
sudo apt install nginx
```

After installation, make sure that nginx is enabled and started:
```bash
systemctl status nginx

sudo systemctl enable nginx
sudo systemctl start nginx
```

Also add its configuration to firewall:
```bash
ufw allow "Nginx Full"
```

## Configuring nginx

[Online configuration tool](https://www.digitalocean.com/community/tools/nginx)

Structure of /etc/nginx:
```bash
  conf.d
    # Legacy configs
  modules-enabled
    # Modules enabled
  sites-enabled
    # Sites enabled
  snippets
    # Configuration snippets
  nginx.conf
```

Structure of a project /var/www/domain:
```bash
htdocs/code
  # Project files
conf
  nginx.conf
  # And other configuration files
logs
  error.log
  # And other log files
temp
  # Directory for temporary files
```

## Adding new project

1. Create a directory for the new project in /var/www, for example, example.com.
2. Inside create the folders htdocs or code, conf and logs (and temp if needed).
3. Place project files in htdocs or code. If it is a Docker container, run it.
4. If the project uses CI/CD through the deployer user, give additional rights to the folder with the project files. There must not be any files inside that are owned by other users.
```bash
sudo setfacl -m u:deployer:rwx htdocs
```
5. Configure nginx in conf/nginx.conf without 443 port and SSL. Example of a simple configuration before installing the certificate:
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
5. Install the certificate:
```bash
certbot -d example.com -d www.example.com
```
6. Finish nginx configuration with 443 port.
