# gns3-host
Be sure to check this out for standalone server: https://docs.gns3.com/docs/getting-started/installation/remote-server

<h2> Initial installation </h2>

This guide is based on running GNS3 in a ubuntu host. 
Depending on your usage the host will need a bit of ram and some cpu.

If you plan on running a simulation with cisco images and vms i would recommend up to 12 cores and 32 gigabytes of ram.
You will also need a lot of storage if you plan on running vm-s. I plan on running up to 4 checkpoint firewall vm-s and a few Cisco/arista IOU-s. Therfore i have allocated 1tb of ssd storage for the ubuntu host.


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
The following commands are run on the virtualization host (proxmox host)

To check if nested virtulizatoin is enabled, bring up terminal/SSH/Shell, execute following command
```
cat /sys/module/kvm_intel/parameters/nested
```
If it returns "N" it's disabled.
To enable
```
# Intel
echo "options kvm-intel nested=Y" > /etc/modprobe.d/kvm-intel.conf
 
# AMD
echo "options kvm-amd nested=1" > /etc/modprobe.d/kvm-amd.conf
```
Then
```
modprobe -r kvm_intel
modprobe kvm_intel
```
Note: If error returns, just reboot the PVE host

To enable nested virtulizatoin for guest VMs

Intel CPU:
– Set the CPU type for VMs to “host”

AMD CPU:
– Set the CPU type for VMs to “host”
– Add following flags to the configuration file

args: -cpu host,+svm

5 We can use following command to check/verify hardware virtualization support is enabled or not on Linux OSs
```
egrep '(vmx|svm)' --color=always /proc/cpuinfo
```
