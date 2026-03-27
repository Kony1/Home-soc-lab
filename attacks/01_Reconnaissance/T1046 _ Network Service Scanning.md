## 🛰️ T1046 – Network Service Scanning

Síťový recon provedený z Kali Linuxu proti celé interní síti.

### 🖥️ Prostředí testu

Útočný stroj: Kali Linux

IP: 192.168.40.10

Cíle:

Windows 11 – 192.168.30.10

Windows Server (DC) – 192.168.20.10

Ubuntu Server – 192.168.20.40

Další hosty v síti

Doména: LAB.LOCAL

#### 🎯 Cíl techniky

Útočník provádí aktivní síťový scanning, aby:

identifikoval živé hosty

zjistil otevřené porty

zjistil běžící služby

___________________________________________________________________________________________

🧪 Použité příkazy (Kali Linux)

🔍 1) Ping sweep – hledání živých hostů
nmap -sn 192.168.20.0/24

<img width="534" height="149" alt="image" src="https://github.com/user-attachments/assets/62ac2121-7c47-44c8-9dc9-5be4c760e241" />

nmap -sn 192.168.30.0/24

<img width="555" height="136" alt="image" src="https://github.com/user-attachments/assets/e8d445ee-57e1-472d-a67a-27fe8f3c4d4a" />


🔍 2) Port scanning (rychlý)


nmap -sS 192.168.20.10

<img width="557" height="157" alt="image" src="https://github.com/user-attachments/assets/0769d4b1-182d-4d8f-a40c-3c209512a065" />

nmap -sS 192.168.30.10

<img width="464" height="151" alt="image" src="https://github.com/user-attachments/assets/b2b07344-2b9a-4844-875d-73e5aa4f7296" />


🔍 3) Service detection

<img width="746" height="288" alt="image" src="https://github.com/user-attachments/assets/abc74beb-b9d6-4f47-a447-0ad6998fae2f" />



nmap -sV 192.168.20.10



🔍 4) OS fingerprinting

nmap -O 192.168.20.10

<img width="545" height="125" alt="image" src="https://github.com/user-attachments/assets/9b3863f6-ca99-4186-9902-e1b9bb417dbe" />




🔍 5) Full scan (pokud chceš)



nmap -p- -sV -O 192.168.20.10


#### 📄 Logy – Windows 11 / Windows Server

🔹 Sysmon Event ID 3 – Síťová komunikace
Sem doplníš logy typu:

příchozí SYN pakety

spojení na porty 135, 445, 3389, 80, 443…

zdrojová IP = Kali

<img width="603" height="439" alt="image" src="https://github.com/user-attachments/assets/79cd615d-089a-4215-85cf-a46fb3d5ae8f" />


#### 📄 Logy – Security log (Windows)

Event ID 5156 – Filtering Platform Connection

nmap testuje por 156 jako jeden z prvnich 

<img width="590" height="189" alt="image" src="https://github.com/user-attachments/assets/8e86f223-3d0a-4a9e-b40b-c1830a5dbb76" />


Windows Firewall loguje:

povolené spojení

blokované spojení



#### 📄 Logy – Wazuh



<img width="865" height="719" alt="image" src="https://github.com/user-attachments/assets/82dd5e7d-2c05-4547-adc4-e3ebe2040a41" />





#### 🔗 Analýza

🔹 Windows (Sysmon + Security log)
Sysmon Event ID 3 zachytil příchozí TCP pokusy z Kali.

Security log (5156/5157) po zapnutí auditu logoval povolené i blokované pokusy.

Windows tak poskytl detailní pohled na síťovou aktivitu na úrovni hosta.

🔹 OPNsense Firewall
Firewall blokoval opakované SYN pokusy na různé cíle.

Logy z filterlog jasně ukázaly laterální scanning napříč celou sítí 192.168.20.x.

🔹 Wazuh SIEM
Koreloval více blokovaných pokusů z jedné IP.

Vygeneroval alert rule 87702 – Multiple pfSense firewall blocks, level 10.

MITRE ATT&CK mapování: T1110 – Multiple Attempts, což odpovídá scanningu.

Wazuh tak poskytl centrální detekci a kontext.



#### 📝 Závěr

Technika T1046 byla úspěšně provedena.
Celý řetězec ukazuje, jak se síťový scanning projeví v logech a jak jej lze spolehlivě detekovat
