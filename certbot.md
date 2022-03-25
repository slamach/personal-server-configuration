# Установка и конфигурация certbot

## Установка

```bash
sudo apt install certbot python3-certbot-nginx
```

После установки необходимых сертификатов нужно проверить работоспособность их автообновления:
```bash
sudo systemctl status certbot.timer
sudo certbot renew --dry-run
```

## Добавление нового сертификата

1. Настраиваем конфигурацию сайта без 443 b SSL. Пример простой конфигурации до установки сертификата.
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
2. Устанавливаем сертификат:
```bash
certbot -d example.com -d www.example.com
```
