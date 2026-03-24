# 📊 SOC Monitoring & Hunting Cheat Sheet (Wazuh / SIEM)

Tento přehled obsahuje klíčové vyhledávací dotazy (Queries) pro efektivní analýzu logů z Windows (Sysmon/Security), OPNsense (Suricata/Firewall) a Linuxu.

## 🪟 Windows Endpoint Monitoring (Sysmon & Security)


| Cíl hledání | Wazuh Query (KQL) | Význam |
| :--- | :--- | :--- |
| **Spuštěné procesy** | `data.win.eventdata.eventID: "1"` | Výpis všech spuštěných programů (Sysmon). |
| **Podezřelý PowerShell** | `data.win.eventdata.commandLine: *powershell*` | Hledání skriptů a příkazů v PowerShellu. |
| **Síťová spojení z PC** | `data.win.eventdata.eventID: "3"` | Kam se tvoje virtuálky připojují (C2, Reverse Shell). |
| **Úspěšná přihlášení** | `data.win.system.eventID: "4624"` | Kdo a jak se přihlásil (Typ 10 = RDP, Typ 3 = Síť). |
| **Brute Force (Selhání)** | `data.win.system.eventID: "4625"` | Detekce pokusů o hádání hesel. |
| **Změna v registrech** | `data.win.eventdata.eventID: "13"` | Detekce **Persistence** (zápis do Run klíčů). |
| **Vytvoření uživatele** | `data.win.system.eventID: "4720"` | Útočník si vytváří vlastní účet. |

## 🛡️ Network & Perimetr (OPNsense / Suricata)


| Cíl hledání | Wazuh Query (KQL) | Význam |
| :--- | :--- | :--- |
| **Všechny IDS alerty** | `rule.groups: "ids"` | Výpis detekcí od **Suricaty** (DPI). |
| **Blokované pakety** | `data.action: "blocked"` | Co firewall nepustil (skenování portů, zakázané IP). |
| **Skenování sítě** | `data.alert.signature: *SCAN*` | Vyfiltruje pokusy o průzkum sítě (Nmap atd.). |
| **Podezřelé porty** | `data.dest_port: (22 OR 3389 OR 4444)` | Sledování pokusů o SSH, RDP nebo běžné porty útočníků. |

## 🐧 Linux & Syslog (Infrastruktura)


| Cíl hledání | Wazuh Query (KQL) | Význam |
| :--- | :--- | :--- |
| **SSH Přihlášení** | `rule.groups: "syslog" AND data.protocol: "ssh"` | Sledování přístupů na Linux servery. |
| **Sudo příkazy** | `rule.description: *sudo*` | Kdo na Linuxu spouští příkazy s právy roota. |

## 🚀 "Threat Hunting" Combo (Příklady pro pokročilé)

*   **Hledání útoku z konkrétní IP:**
    `data.win.eventdata.ipAddress: "10.0.0.5" OR data.srcip: "10.0.0.5"`
*   **Vyloučení tvého PC z logů (pro klid v Dashboardu):**
    `NOT agent.name: "Tvuj-Host-PC"`
*   **Všechny kritické chyby (Level 10 a výš):**
    `rule.level: >=10`

---
*Tip: Tato pole si přidej jako sloupce v sekci **Discover**, aby se ti Dashboard neztrácel v textu.*
