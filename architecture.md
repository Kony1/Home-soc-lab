# Lab 

## Attacker
- **OS:** Kali Linux 2025.1
- **Role:** Útočník
- **Software:** nmap, enum4linux, hydra, Metasploit

## Victim Workstation
- **OS:** Windows 11 Pro
- **Role:** Cíl útoků, klient v doméně
- **Software:** běžné Windows aplikace, testovací uživatelé

## Domain Controller
- **OS:** Windows Server 2022
- **Role:** Active Directory, DNS, DHCP
- **Software:** Active Directory Domain Services, DNS server, Wazuh agent

## SIEM / Logging
- **OS:** Ubuntu Server 22.04
- **Role:** Centralizovaná detekce
- **Software:** Wazuh manager, Elasticsearch, Kibana

## Síť
- IP scheme: 192.168.20.0/24
- Gateway: 192.168.20.1
- Všechny stroje mají **statické IP**
- Testovací VLAN: host-only / internal network (VMware)
