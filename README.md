# gns3-host

<h2> Initial installation </h2>


The host uses 16gig ram and 6 cpu cores

Install default ubuntu server installation, and copy over ssh keys.

Install gns3ser:
```
sudo add-apt-repository ppa:gns3/ppa
sudo apt update                                
sudo apt install gns3-server gns3-gui
```
Install IOU support:
```
sudo dpkg --add-architecture i386
sudo apt update
sudo apt install gns3-iou
```

Install docker support:
```
Basis packages:
sudo apt -y install apt-transport-https ca-certificates curl gnupg-agent software-properties-common

Add repository:
sudo apt remove docker docker-engine docker.io containerd runc
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable"


Install:
sudo apt update
sudo apt install docker-ce docker-ce-cli containerd.io


Add user to docker group
sudo usermod -aG docker $USER
newgrp docker

Add gns3 users to docker group:
for i in ubridge libvirt kvm wireshark docker; do
  sudo usermod -aG $i $USER
done
```


<h2>Nested Virtualization</h2>

If you run the ubuntu host virtualized we need nested virtualization enabled:
