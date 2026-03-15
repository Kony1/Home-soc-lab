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


📝 Lab Update: Wazuh Agent Hardening & FIM Configuration

🛠️ Task: Network Migration & Agent Optimization

Migrace Windows Serveru z Bridged (pro stažení balíčků) zpět do izolovaného labu Host-only (VMnet1) a konfigurace hloubkového monitoringu.


🔧 Wazuh Agent Configuration (ossec.conf)

Pro zajištění detekce útoků v reálném čase a sledování manipulace se soubory byly upraveny následující sekce:
File Integrity Monitoring (FIM): Zapnut režim realtime="yes" pro kritické cesty

ossec.conf
uprava realtime, check_all, report changes v syschecku 

Security Configuration Assessment (SCA): Aktivován audit shody s bezpečnostními standardy (aktuální skóre DC01: 37 %).

_________________________________________________________________________________________________________________________________
__________________________________________________________________________________________________________________________________

Instalace Wazuh-indexer, server a dashboard na Ubuntu server 

https://documentation.wazuh.com/current/installation-guide/index.html

curl -sO https://packages.wazuh.com/4.14/wazuh-install.sh
curl -sO https://packages.wazuh.com/4.14/config.yml
__________________________________________________

můj yml:
indexer:
  - name: node-1
    ip: "192.168.20.40"

server:
  - name: wazuh-1
    ip: "192.168.20.40"

dashboard:
  - name: dashboard
    ip: "192.168.20.40"
_________________________

Generuj konfigurační soubory (klíče, certifikáty, hesla):
sudo bash wazuh-install.sh --generate-config-files
__________________________

Instalování komponent: 
Indexer: sudo bash wazuh-install.sh --wazuh-indexer node-1
sudo bash wazuh-install.sh --start-cluster

Manager: sudo bash wazuh-install.sh --wazuh-server wazuh-1

Dashboard: sudo bash wazuh-install.sh --wazuh-dashboard dashboard

kontrola služeb:
sudo systemctl status wazuh-indexer
sudo systemctl status wazuh-manager
sudo systemctl status wazuh-dashboard
______________________________________

Přihlášení do Dashboardu a přidání agentů 


Příkazy:

seznam agentů ubuntu
sudo /var/ossec/bin/manage_agents -l

powershell win
rychla zmena ip v ossec.conf agenta
(Get-Content "C:\Program Files (x86)\ossec-agent\ossec.conf") -replace "<address>.*</address>", "<address>192.168.20.40</address>" | Set-Content "C:\Program Files (x86)\ossec-agent\ossec.conf"








    














