# Initial Server Setup

**Last Updated:** 04/03/2026 
**Status:** 🚧 In Progress 

---

## 🎯 Objective
*Initial setup of server*
---

## 📝 Prerequisites
*What needs to be in place before starting?*

- [x] Ubuntu Server installed (minimal installation)
- [x] Server reachable via SSH
- [ ] Static IP address configured 
- [x] Non-root administrative user created
- [x] SSH key-based authentication configured
- [x] Password authentication disabled
- [ ] System fully updated 
- [ ] Domain name configured 
- [ ] Port forwarding configured 
---

## 🔧 Step-by-Step Implementation

### 1. Install Ubuntu 24.04.4 LTS
- Downloaded the Ubuntu 24.04.4 LTS ISO from the [Ubuntu website.](https://ubuntu.com/download/desktop)
- Wrote ISO to external USB stick
- Installed a minimal install of Ubuntu on server
- Updated all packages after fresh install
```
sudo apt update && sudo apt upgrade -y

### 2. Connect to server and configure SSH
- Installed net-tools on server to acquire server IP. Net-tools is considered outdated but I have gotten used to it from other distros so I will stick to it as long as its supported.
```
sudo apt install net-tools
ifconfig #reveals network adapters and IP assigned by router, will change to static.
```


- Used non-root user to access server through SSH
```
ssh non-root@local.ip
```
- Enabled SSH at server boot to allow accessing the server completely headless
```
sudo systemctl enable ssh
```

- Uploaded SSH key to server from main machine to enable easy access from my main desktop.
```
ssh-copy-id non-root@local.ip
```
- disabled password-based SSH access. Password authentication was overridden by an included config file, so I updated both /etc/ssh/sshd_config and /etc/ssh/sshd_config.d/*.conf to enforce key-only login
```
sudo nano /etc/ssh/sshd_config
sudo nano /etc/ssh/sshd_config.d/*.conf
```
- Verified password login is disabled
```
  ssh -o PreferredAuthentications=password -o PubkeyAuthentication=no non-root@local.ip
  
  Result:non-root@local.ip : Permission denied (publickey).
```
