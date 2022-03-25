# Установка и конфигурация certbot

## Установка Docker Engine

[Инструкция по установке](https://docs.docker.com/engine/install/ubuntu/)

```bash
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg

echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/ubuntu \
  $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null

sudo apt-get install docker-ce docker-ce-cli containerd.io
```

После установки нужно убедиться, что docker запущен:
```bash
systemctl status docker

sudo systemctl enable docker
sudo systemctl start docker
```

## Установка Docker Compose

[Инструкция по установке](https://docs.docker.com/compose/cli-command/#install-on-linux)

```bash
mkdir -p /usr/local/lib/docker/cli-plugins

curl -SL https://github.com/docker/compose/releases/download/v2.3.3/docker-compose-$(uname -s)-$(uname -m) -o /usr/local/lib/docker/cli-plugins/docker-compose

chmod +x /usr/local/lib/docker/cli-plugins/docker-compose
```

## Добавление нового контейнера

Разместить все необходимые файлы, включая docker-compose.yml в папке code проекта.

```bash
docker-compose -p project-name up -d
```
