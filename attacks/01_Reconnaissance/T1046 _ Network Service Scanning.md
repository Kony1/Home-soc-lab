E# T1046 – Network Service Scanning

## Popis
Útočník skenuje síť, aby identifikoval otevřené porty a služby (např. SMB, RPC). Pomocí nástroje `nmap` bylo zjištěno, že cíl Win11_client `192.168.30.10` má otevřené porty: 135, 139, 445.

## Použitý příkaz v kali 
```bash
nmap -sS 192.168.30.10

Výstup
PORT    STATE SERVICE
135/tcp open  msrpc
139/tcp open  netbios-ssn
445/tcp open  microsoft-ds


Port 445 (SMB): Možný cíl pro útoky typu bruteforce.
Port 135 (MSRPC): Může být využit pro vzdálenou správu.
