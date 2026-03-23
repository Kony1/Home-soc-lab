# 🏗️ Home SOC Lab Environment

Tento dokument detailně popisuje infrastrukturu mého domácího SOC/Red Team labu.
Lab je navržen jako segmentované, izolované a realistické prostředí, které umožňuje bezpečné testování MITRE ATT&CK technik, sběr logů a tvorbu detekcí.

# 🎯 Cíle projektu

Simulace útoků: Praktické provádění MITRE ATT&CK technik z útočné zóny.

Analýza logů: Sběr a korelace dat z klientských i serverových systémů.

Detekce & Monitoring: Wazuh SIEM + Sysmon + Windows audit policy.

Dokumentace: Každá technika má vlastní markdown soubor s příkazy, screenshoty a logy.

# 🛰️ Síťová architektura (Network Design)
Lab běží ve VMware Workstation a je rozdělen do čtyř izolovaných zón, které simulují reálné podnikové prostředí.
Segmentace je řízena přes OPNsense firewall, který zajišťuje:

oddělení sítí

firewall pravidla


monitoring provozu

<img width="1536" height="1024" alt="image" src="https://github.com/user-attachments/assets/51a87504-398b-42eb-88cd-e3e030b83681" />



## 🖥️ Jednotlivé uzly (Nodes)


#### 1. Attack Zone — Kali Linux
192.168.40.10

Výchozí bod pro všechny ofenzivní operace a MITRE ATT&CK techniky.

OS: Kali Linux 2025
Klíčové nástroje:

Nmap (network discovery)

CrackMapExec (lateral movement & enumeration)

Impacket (SMB, Kerberos, AD útoky)

BloodHound (AD graph analysis)

Metasploit Framework



#### 2. Client Workstation — Windows 11 Pro

192.168.30.10

Simulace běžného uživatele v doméně, hlavní cíl útoků.

Role:

User endpoint

Generování autentického provozu

Cíl MITRE technik (Discovery, PrivEsc, Execution…)

Monitoring:

Wazuh Agent

Sysmon (detailní logování procesů, síťových událostí a registru)



####3. Domain Controller — Windows Server 2022

192.168.20.10

Srdce labu — správa identit, autentizace a infrastruktury.

Služby:

Active Directory Domain Services

DNS

DHCP (pro serverovou síť)

Auditování:

Rozšířené logování (4624, 4625, 4672, 4688…)

Logy odesílány do Wazuh SIEM

#### 4. SIEM / Logging — Ubuntu Server (Wazuh)

192.168.20.40

Centrální mozek pro analýzu hrozeb, detekce a vizualizaci.

OS: Ubuntu Server 22.04 LTS
Stack:

Wazuh Manager — sběr logů, real-time detekce

Wazuh Indexer — rychlé vyhledávání v logech

Wazuh Dashboard — vizualizace, alerty, reporting


## 📁 MITRE ATT&CK Struktura (aktuální stav)
Techniky jsou organizované podle taktik (např. Reconnaissance, Privilege Escalation).
Každá taktika má vlastní složku a v ní jsou jednotlivé techniky jako .md soubory.

Příklad:

```
attacks/
   01_Reconnaissance/
      T1087_Account_Discovery.md
      T1046_Network_Service_Scanning.md

   06_Privilege_Escalation/
      T1053_Scheduled_Task.md
```
Každý .md obsahuje:

popis útoku

příkazy (PowerShell/CMD/Kali)

screenshoty

logy z Win11, DC01, Wazuh

troubleshooting

detekční poznámky

Runy jsou zatím psané pod sebou v jednom souboru.
Později budou rozdělené do Run1/Run2/Run3.

#### 🔜 Plánované rozšíření
Oddělení MITRE technik do Run1/Run2/Run3

Přidání Windows 11 ART jako Red Team endpoint

Integrace Suricata IDS/IPS na OPNsense

Cloud SIEM Sentinel 

Přidání Sigma rules

Tvorba hunting queries

Rozšíření detekcí ve Wazuh
