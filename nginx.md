# Установка и конфигурация nginx

## Установка

```bash
sudo apt install nginx
```

После установки нужно убедиться, что nginx запущен:
```bash
systemctl status nginx

sudo systemctl enable nginx
sudo systemctl start nginx
```

И добавить конфигурации для него в фаервол:
```bash
ufw allow "Nginx Full"
```

## Конфигурация

[Онлайн-конфигуратор](https://www.digitalocean.com/community/tools/nginx)

Структура /etc/nginx:
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

Структура проекта /var/www/domain:
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

## Добавление нового проекта

1. Создаем директорию под новый проект в /var/www, например, example.com.
2. Внутри создаем папки htdocs/code, conf и logs (если нужно, еще и temp).
3. В htdocs размещаем файлы проекта. Если это Docker-контейнер, то запускаем его.
4. Если в проекте используется CI/CD через пользователя deployer, выдаем папке с файлами проекта дополнительные права. Внутри не должно находиться файлов, которыми владеют другие пользователи.
```bash
sudo setfacl -m u:deployer:rwx htdocs
```
5. Настриваем nginx в conf/nginx.conf без 443 порта и SSL.Пример простой конфигурации до установки сертификата.
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
5. Устанавливаем сертификат:
```bash
certbot -d example.com -d www.example.com
```
6. Донастраиваем nginx уже под 443 порт.
