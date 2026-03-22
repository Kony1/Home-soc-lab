
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



## Run #1 –  2026-03-22 13:24

### Popis techniky

Útočník provádí aktivní síťový průzkum (ping sweep + SYN scan) s cílem identifikovat živé hosty a zjistit, které služby jsou dostupné. Ping sweep využívá ICMP Echo Request k detekci aktivních zařízení. SYN scan zneužívá první krok TCP handshake (SYN) k ověření, zda je port otevřený, aniž by dokončil celé navázání spojení.
## Použité příkazy
<img width="562" height="302" alt="image" src="https://github.com/user-attachments/assets/a12fd24a-6edb-45e4-8f01-bf22aa6e7029" />


### Výsledky
- 192.168.20.1 – OPNsense LAN gateway  
- 192.168.20.10 – Windows 11  
- 192.168.20.40 – Ubuntu  
- 192.168.30.1 – OPNsense CLIENT gateway  
- 192.168.30.10 – Windows klient  

 

### Závěr

Útok proběhl 13:24 

Suricata detekovala ICMP ping sweep.

OPNsense firewall blokoval TCP SYN pakety.

Wazuh zachytil logy



<img width="447" height="184" alt="image" src="https://github.com/user-attachments/assets/c87a7e12-200a-428b-b541-12a3e0bd6af5" />
<img width="533" height="197" alt="image" src="https://github.com/user-attachments/assets/5afdbb12-a2c8-405c-82ab-940953c578b7" />






