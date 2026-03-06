# Kavita and Docker setup

**Last Updated:** 06/03/2026 
**Status:** 🚧 In Progress 

---

## 🎯 Objective
*Setup Kavita to run on the server in a docker container and allow a central location to store books*
---

## 📝 To do list

- [x] Docker installed and configured
- [x] Directory structure created 
- [x] Container deployed and running
- [x] Kavita web UI accessible
- [x] Admin account created
- [ ] Library configured and scanned
- [ ] OPDS feed tested with reader app
- [ ] Backup strategy considered for config data
---

## 🔧 Step-by-Step Implementation

## Step 1. Install docker and dependencies

- First checked if CPU supports kvm virtuilisation as the CPU is on the older side.
```
kvm-ok
Result:
yikes@yikes:~$ kvm-ok
INFO: /dev/kvm exists
KVM acceleration can be used
```
- Used Dockers [documentation](https://docs.docker.com/engine/install/ubuntu#install-using-the-repository) to install Docker and its dependencies on the server. The following commands will mostly be copied directly from the documentation
```
# Add Docker's official GPG key:
sudo apt update
sudo apt install ca-certificates curl
sudo install -m 0755 -d /etc/apt/keyrings
sudo curl -fsSL https://download.docker.com/linux/ubuntu/gpg -o /etc/apt/keyrings/docker.asc
sudo chmod a+r /etc/apt/keyrings/docker.asc

# Add the repository to Apt sources:
sudo tee /etc/apt/sources.list.d/docker.sources <<EOF
Types: deb
URIs: https://download.docker.com/linux/ubuntu
Suites: $(. /etc/os-release && echo "${UBUNTU_CODENAME:-$VERSION_CODENAME}")
Components: stable
Signed-By: /etc/apt/keyrings/docker.asc
EOF

sudo apt update

sudo apt install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
```
- Check if the installation was complete by running a test dokcer image.
```
sudo docker run hello-world
```

## Step 2. Install Kavita as a docker container and configure require settings

-Before installing the docker image I create the directories to store it's configuration data and the location I want my books to be stored.

```
sudo mkdir -p /srv/kavita && sudo mkdir -p /srv/media/books
```

-Used the Kavita [documentation](https://wiki.kavitareader.com/installation/docker/) to guide the installation of the docker container. The following commands were altered to fit my needs and required directories.
```
docker run --name kavita -p 5000:5000 \
-v /srv/media/books:/books \
-v /srv/kavita/config:/kavita/config \
--restart unless-stopped \
-e TZ=Europe/London \
-d jvmilazz0/kavita:latest
```


- TBD If more work on configuration for the Docker image is actually required

## Part 3. Easy transfer of files outside host

-Using sftp for now as it works well enough for my needs.
-TBC
