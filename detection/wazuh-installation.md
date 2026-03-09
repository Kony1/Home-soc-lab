# Wazuh Installation (Ubuntu Server)

## Environment

SIEM server:

OS: Ubuntu Server 22.04 LTS  
IP: 192.168.20.40  
Role: Wazuh SIEM
_____________________________

##preStep - VMware
custom VMnet0 "Bridged" 


## Step 1 – Update system

sudo apt update && sudo apt upgrade -y


##Step 2 - Download Wazuh installer + alternativ

curl -sO https://packages.wazuh.com/4.7/wazuh-install.sh   

wget https://packages.wazuh.com/4.7/wazuh-install.sh



##Step 3 – Run installation + alternativ

sudo bash wazuh-install.sh -a

sudo bash wazuh-install.sh -a --ignore-check

User:admin



![wazuhpass](https://github.com/user-attachments/assets/2a8e5a49-c8c5-4996-ae9a-ef6f0094f61f)



##Step 5 
Custom Vmet1 (host-only)  "lab network"

open netplan: 
sudo nano /etc/netplan/*.yaml

yaml
network:
  version: 2
  renderer: networkd
  ethernets:
    ens33:
      addresses:
        - 192.168.20.40/24
        

sudo netplan apply 

" sudo chmod 600 /etc/netplan/*.yaml "

" sudo netplan apply "

alternativ to static ip address 

sudo ip addr add 192.168.20.40/24 dev ens33
sudo ip link set ens33 up


##Step 6 acces to dashboard

available dashboard open in DC01 - WIN server
https://192.168.20.40

![openWazuh](https://github.com/user-attachments/assets/cb3b734f-806e-4f51-bb00-020810582804)

snapshot ubuntu OS: Ubuntu Server (LTS) --- Wazuh-AIO-Static-192.168.20.40-Functional
_________________________________________________________________________________________________




