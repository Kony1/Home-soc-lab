
🔍 Taktika: Reconnaissance (01)

Technika: T1595 - Active Scanning (Nmap)

Tento report dokumentuje techniku aktivního skenování sítě za účelem zjištění otevřených portů a běžících služeb.

🧠 Teoretický základ

Taktika: Reconnaissance (Průzkum) – Útočník mapuje terén.

Technika: Active Scanning – Přímé "klepání" na porty cílového systému.

Cíl: Identifikovat operační systém a verze služeb (např. zjistit, že na portu 445 běží zranitelná verze SMB).

🌐 Kontext a Vrstvy

OSI Model: Vrstva 3 (IP/ICMP) a Vrstva 4 (TCP/UDP).

Datové zdroje: Síťový provoz (Netflow), logy z Firewallu, IDS/IPS systémy.

🛡️ Stav Detekce (Wazuh)

Stav: ✅ Detekováno

Pravidlo: Rule ID 31103 (Multiple network events from same source IP).

Význam: Wazuh identifikoval, že jedna IP adresa (Kali) se pokusila v krátkém čase o příliš mnoho spojení na různé porty.

⚔️ Praktický Test (Simulation)

🔴 Útok

Nástroj: Nmap

Příkaz:

nmap -sV -Pn --top-ports 100 192.168.20.20


🔍 Analýza logu (JSON)

Toto je část logu, kterou uvidíš ve Wazuhu po úspěšné detekci:

{
  "rule": {
    "level": 10,
    "description": "Multiple network events from same source IP.",
    "id": "31103"
  },
  "data": {
    "srcip": "192.168.20.30",
    "dstport": "445",
    "protocol": "tcp"
  },
  "agent": {
    "id": "001",
    "name": "Windows-11-Lab"
  }
}


💡 Hardening (Obrana)

Firewall: Omezit povolené IP adresy, které mohou přistupovat k citlivým portům.

IPS: Zapnout automatické blokování IP adres (Active Response), které vykazují známky skenování.
