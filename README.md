# Конфигурация личного сервера


## Аппаратная конфигурация сервера

Аппаратные характеристики, которые сейчас тестируются на [Beget](https://beget.com/ru):  
- x2 vCPU
- 2GB RAM
- 30GB NVMe
- 250 MB/s Unlimited Traffic

Операционная система: Ubuntu 20.04


## Программная конфигурация сервера

### Перенос файлов с локальной машины
```bash
rsync -rlptoDzP ./local-directory/ slamach@vs01.dmitrysviridov.ru:/remote-path
```

### Смена имени машины
```bash
sudo hostnamectl set-hostname slamach-vs01
# Or change it here
sudo vim /etc/hostname

# Change all old references
sudo vim /etc/hosts

exec bash
```

### Создание рабочего пользователя
```bash
sudo adduser slamach
sudo usermod -aG sudo slamach
```

### Конфигурация SSH

```bash
# Add SSH-key to ~/.ssh/authorized_keys

sudo vim /etc/ssh/sshd_config
# Change PasswordAuthentication to no
# Make sure ChallengeResponseAuthentication is set to no

sudo systemctl restart ssh
```

### Конфигурация файрвола

```bash
sudo vim /etc/default/ufw
# Make sure IPV6 is set to yes

ufw allow OpenSSH

ufw enable
```

### Конфигурация рабочего пользователя

[Конфигурация Bash](.bashrc)

```bash
touch ~/.hushlogin
touch /root/.hushlogin

# Replace ~/.bashrc and /root/.bashrc
```

### Создания пользователя для CI/CD
[Отличная статья для Github Actions](https://zellwk.com/blog/github-actions-deploy/)

```bash
sudo adduser deployer
sudo usermod -aG sudo deployer

sudo su deployer

ssh-keygen -b 4096
cat .ssh/key-name.pub >> .ssh/authorized_keys
```

## Установка пакетов

### Обновление предустановленных пакетов

```bash
sudo apt update && sudo apt upgrade
```

### Nginx
[Подробная инструкция](nginx.md)

### Бот Let's Encrypt
[Подробная инструкция](certbot.md)

### Docker
[Подробная инструкция](docker.md)
