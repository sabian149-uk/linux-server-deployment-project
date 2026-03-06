# Initial Server Setup

**Last Updated:** 06/03/2026 
**Status:** ✅ Complete
---

## 🎯 Objective
*Initial setup of server*
---

## 📝 Prerequisites
*What needs to be in place before starting?*

- [x] Ubuntu Server installed (minimal installation)
- [x] Server reachable via SSH
- [x] Static IP address configured 
- [x] Non-root administrative user created
- [x] SSH key-based authentication configured
- [x] Password authentication disabled
- [x] System fully updated 
- [ ] Port forwarding configured (TBD If required)
---

## 🔧 Step-by-Step Implementation

### 1. Install Ubuntu 24.04.4 LTS
- Downloaded the Ubuntu 24.04.4 LTS ISO from the [Ubuntu website.](https://ubuntu.com/download/desktop)
- Wrote ISO to external USB stick
- Installed a minimal install of Ubuntu on server
- Updated all packages after fresh install
```
sudo apt update && sudo apt upgrade -y
```
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

### 3. Set static IP for server

-Used documentation from the [Ubuntu ](https://ubuntu.com/server/docs/explanation/networking/configuring-networks/)site to apply a static IP on the server. I first acquiured the network adapter name.

```
ifconfig
Result: "wlx9c5322e1d9a2" is my current wifi adapter I want configure a static IP to.
```
-Editted the config file already associated with my wifi adapter
```
sudo nano /etc/netplan/50_config.yaml
```
-In the created file I input and tweaked a config file I got from the Ubuntu server to serve my own purposes.
```
network:
  version: 2
  wifis:
    wlx9c5322e1d9a2:
      addresses: [192.168.1.91/24]
      routes:
        - to: default
          via: 192.168.1.1
      nameservers:
        addresses: [1.1.1.1,8.8.8.8]
      access-points:
        "redacted":
          auth:
            key-management: "psk"
            password: "redacted"

```
-Used netplan to apply changes and restarded system to confirm changes have taken effect.
```
sudo netplan apply 
sudo reboot 
ip a - confirm static IP
Result: 3: 
wlx9c5322e1d9a2: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc noqueue state UP group default qlen 1000
    link/ether 9c:53:22:e1:d9:a2 brd ff:ff:ff:ff:ff:ff
    inet 192.168.1.91/24 brd 192.168.1.255 scope global wlx9c5322e1d9a2
       valid_lft forever preferred_lft forever
    inet6 fe80::9e53:22ff:fee1:d9a2/64 scope link 
       valid_lft forever preferred_lft forever - confirms IP is designated to the server by the router.

```




