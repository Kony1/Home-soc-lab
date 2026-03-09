# Nmap Scan – Kali → Windows 11Pro

## Cíl
- Cílový stroj: **Windows 11 Pro**  
- IP: 192.168.20.20  
- Účel: zjistit otevřené porty a dostupné služby

## Útočník
- OS: **Kali Linux 2025.1**  
- Role: attacker


## Příkaz
nmap -sS -p 1-500 192.168.20.20

_________________________________
Výsledek 
PORT      STATE      SERVICE
135/tcp   open       msrpc
139/tcp   filtered   netbios-ssn
445/tcp   filtered   microsoft-ds

Port 135 → otevřený (MS RPC)
Porty 139, 445 → filtrovány (firewally nebo Windows Defender)

" Tento scan je základní krok pro recon fázi útoku. "
