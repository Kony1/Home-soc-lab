# 🏗️ Home SOC Lab Architecture
Tento dokument detailně popisuje infrastrukturu mého detekčního labu. Lab je navržen jako izolované prostředí pro bezpečné testování útoků a monitorování síťové aktivity.

## 🎯 Cíle projektu
Simulace útoků: Testování ofenzivních technik (Attacker).
Analýza logů: Sběr dat z koncových bodů (Victim + Server).
Detekce & Monitoring: Konfigurace SIEM pravidel a vizualizace hrozeb.
##🛰️ Síťová topologie (Network Design)
Lab běží v izolované virtuální síti (VMware Host-only), aby byla zajištěna bezpečnost hlavní sítě.

Network Scheme: 192.168.20.0/24
Gateway: 192.168.20.1
DHCP/DNS: Windows Server 2022
Statické IP: Všechny uzly mají pevně definované adresy pro konzistentní logování.


## 🖥️ Jednotlivé uzly (Nodes)

#### 1. Attacker — Kali Linux 192.168.20.30 ✅
Výchozí bod pro všechny ofenzivní operace.


####OS: Kali Linux 2025.1

Klíčové nástroje:
Nmap — Síťové skenování a objevování služeb.
Hydra — Brute-force útoky na protokoly.


#### 2. Victim Workstation — Windows 11 192.168.20.20 ✅

Koncový bod simulující běžného uživatele v doméně.

OS: Windows 11 Pro
Role: Cíl útoků, generování klientského provozu.
Monitoring: Wazuh Agent + Sysmon (pro detailní logování procesů).


#### 3. Domain Controller — Windows Server 2022 192.168.20.10 ✅

Srdce labu, správa identit a autentizace.

Služby: Active Directory DS, DNS, DHCP.
Auditování: Nastaveno rozšířené logování úspěšných i neúspěšných přihlášení (Event ID 4624/4625).


###  4. SIEM / Logging — Ubuntu Server 192.168.20.40

Centrální mozek pro analýzu a vizualizaci hrozeb.

OS: Ubuntu Server 22.04 LTS
Stack:
Wazuh Manager: Sběr logů a real-time detekce.
Indexer (Elasticsearch): Rychlé vyhledávání v milionech logů.
Dashboard (Kibana): Tvorba reportů a sledování alertů.
