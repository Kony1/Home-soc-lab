# 🏗️ Home SOC Lab Architecture (2025)
Tento dokument popisuje aktuální architekturu mého domácího SOC/Red Team labu.
Lab je navržen jako segmentované, izolované a realistické prostředí, které umožňuje:

bezpečné testování MITRE ATT&CK technik,

sběr a analýzu logů,

tvorbu detekcí,

a dokumentaci reálných útokových scénářů.

# 🎯 Cíle projektu
Simulace útoků: Praktické provádění MITRE ATT&CK technik (Privilege Escalation, Discovery, Execution…).

Detekce & Monitoring: Wazuh SIEM + Sysmon + Windows audit policy.

Analýza logů: Korelace událostí mezi klientem, serverem a útočníkem.

Dokumentace: Každá technika má vlastní „Run“ se screenshoty, logy a analýzou.

# 🛰️ Síťová topologie
Lab běží ve VMware Workstation a je rozdělen do izolovaných zón, které simulují reálné podnikové prostředí.


<img width="598" height="393" alt="image" src="https://github.com/user-attachments/assets/f3f646c6-acf0-4acb-826c-844814e91844" />

                          
# 🔌 OPNsense – síťová rozhraní + Zóny


<img width="723" height="330" alt="image" src="https://github.com/user-attachments/assets/05bf41d6-56cc-42ea-a517-189803c3a4c3" />



# 🖥️ Jednotlivé uzly (Nodes)

## 1. Domain Controller – Windows Server 2022

192.168.20.10

Active Directory DS

DNS

DHCP (pro serverovou síť)

Rozšířené auditování (4624/4625, 4672, 4688…)

Logy odesílány do Wazuh

## 2. Wazuh SIEM – Ubuntu Server

192.168.20.40

Wazuh Manager

Wazuh Indexer

Wazuh Dashboard

Centrální sběr logů z Win11, DC01, útočníka

Používá se pro detekce MITRE ATT&CK technik

## 3. Client Workstation – Windows 11 Pro

192.168.30.10

Simulace běžného uživatele

Sysmon + Wazuh agent

Cíl útoků (Discovery, PrivEsc, Execution…)

Generuje autentické logy pro SIEM

## 4. Attack Zone – Kali Linux 

Kali Linux
192.168.40.10

# 🧪 MITRE ATT&CK Runs

Každá technika má vlastní složku:

```
attacks/
   06_Privilege_Escalation/
      T1053_Scheduled_Task.md   ← všechny runy v jednom souboru
```

Každý run obsahuje:

popis útoku

přesné příkazy

screenshoty

logy z Win11, DC01, Wazuh

detekční poznámky

troubleshooting

