# 🧱 OPNsense Firewall & Suricata Cheat Sheet 

Tento přehled slouží k identifikaci síťových útoků a blokovaného provozu na perimetru sítě.


| Akce / Pole | Význam v SIEMu | SOC Analýza |
| :--- | :--- | :--- |
| **Action: blocked** | Firewall zahodil paket. | Detekce skenování portů nebo nepovolené komunikace. |
| **Action: passed** | Provoz byl povolen. | Sledování legitimního provozu (Flow logs). |
| **Direction: in** | Příchozí provoz z WAN/LAN. | Útoky zvenčí nebo pohyb uvnitř sítě. |
| **Direction: out** | Odchozí provoz. | Detekce exfiltrace dat nebo "volání domů" (C2). |
| **Suricata Alert** | IDS/IPS detekce. | Konkrétní signatura útoku (např. SQL Injection, Nmap). |
| **Protocol: TCP/UDP/ICMP** | Typ přenosu. | ICMP může značit "Ping sweep" (průzkum sítě). |

---
### 🔍 Co vyhledávat ve Wazuh (Filtry):
*   `data.action: "blocked"` – Ukáže vše, co firewall zastavil.
*   `data.ip_protocol: "icmp"` – Hledání pingů a diagnostiky v síti.
*   `data.dest_port: 22 OR 3389` – Sledování pokusů o SSH nebo RDP přístup.
*   `data.alert.signature: "GPL SCAN*"` – Rychlý filtr na skenování portů (Suricata).
