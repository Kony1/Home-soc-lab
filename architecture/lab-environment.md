Home SOC Basic Lab

Tento dokument popisuje jednotlivé stroje, software a jejich role v mém domácím SOC labu. Cílem je naučit se:

- simulovat útoky (attacker)
- analyzovat logy (victim + server)
- nastavit SIEM a detekci (monitoring)

---

## 1. Attacker – Kali Linux :heavy_check_mark:

- **OS:** Kali Linux 2025.1 
- **Role:** Útočník
- **IP:** 192.168.20.30 (statická)
- **Software / nástroje:**  
  - Nmap → network scanning  
  - Enum4linux → sběr informací z Windows / AD  
  - Hydra → testování hesel  
  - Metasploit → exploit framework

"Tento stroj je výchozí bod pro všechny útoky v labu"

## 2. Victim Workstation – Windows 11 :heavy_check_mark:

- **OS:** Windows 11 Pro  
- **Role:** Cíl útoků / klient v doméně  
- **IP:** 192.168.20.20 (statická)  
- **Software:** běžné Windows aplikace, testovací uživatelé  

" Tento počítač je připojen k doméně a slouží k testování útoků a následné detekci v logech. "



## 3. Domain Controller – Windows Server 2022 :heavy_check_mark:

- **OS:** Windows Server 2022  
- **Role:** Active Directory / DNS / DHCP  
- **IP:** 192.168.20.10 (statická)  
- **Software:**  
  - Active Directory Domain Services  
  - DNS server  
  - Wazuh agent (posílá logy do SIEM)  

" DC je klíčový pro testování útoků na doménu, správu uživatelů a sledování autentizace. "

---

## 4. SIEM / Logging – Ubuntu Server

- **OS:** Ubuntu Server 22.04  
- **Role:** Centralizovaná detekce a monitoring  
- **IP:** 192.168.20.40 (statická)  
- **Software:**  
  - Wazuh manager → sběr logů a detekce  
  - Elasticsearch → ukládání logů  
  - Kibana → vizualizace alertů  

" Tento stroj monitoruje všechno, co se děje v labu, a umožňuje analyzovat útoky a generovat alerty "

---

## 5. Síť :heavy_check_mark:

- **IP scheme:** 192.168.20.0/24  
- **Gateway:** 192.168.20.1  
- **Všechny stroje mají statické IP**  
- **Virtual Network:** host-only / internal (VMware)  
- **Cíl:** mít izolovanou síť, aby útoky neovlivnily reálnou domácí síť

---

## 6. Diagram labu (stay-tuned)
