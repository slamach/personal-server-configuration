# Personal server configuration


## Hardware configuration

Sufficient configuration for several small projects:  
- x2 vCPU
- 2-4GB RAM
- 15+GB NVMe

My choice of OS: Ubuntu 20.04


## Software configuration

### Transferring files from a local machine
```bash
rsync -rlptoDzP ./local-directory/ user@example.com:/remote-path
```

### Changing hostname
```bash
sudo hostnamectl set-hostname example-vs01
# Or change it here
sudo vim /etc/hostname

# Change all old references
sudo vim /etc/hosts

exec bash
```

### Creating main user
```bash
sudo adduser user
sudo usermod -aG sudo user
```

### Configuring SSH

```bash
# Add SSH-key to ~/.ssh/authorized_keys

sudo vim /etc/ssh/sshd_config
# Change PasswordAuthentication to no
# Make sure ChallengeResponseAuthentication is set to no

sudo systemctl restart ssh
```

### Configuring firewall

```bash
sudo vim /etc/default/ufw
# Make sure IPV6 is set to yes

ufw allow OpenSSH

ufw enable
```

### Configuring main user

[My Bash configuration](.bashrc)

```bash
touch ~/.hushlogin
touch /root/.hushlogin

# Replace ~/.bashrc and /root/.bashrc
```

### Creating CI/CD user
[Nice article about deploying with Github Actions](https://zellwk.com/blog/github-actions-deploy/)

```bash
sudo adduser deployer
sudo usermod -aG sudo deployer

sudo su deployer

ssh-keygen -b 4096
cat .ssh/key-name.pub >> .ssh/authorized_keys
```

## Installing packages

### Updating existing packages

```bash
sudo apt update && sudo apt upgrade
```

### Nginx
[Detailed instruction](nginx.md)

### Let's Encrypt
[Detailed instruction](certbot.md)

### Docker
[Detailed instruction](docker.md)
