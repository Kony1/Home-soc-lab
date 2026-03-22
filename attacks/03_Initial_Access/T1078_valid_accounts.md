## 🚪 Taktika: Initial Access (03)

Technika: T1078 - Valid Accounts (Platné účty)

Poznámka: Tento report slouží jako vzorová technika pro taktiku Initial Access. Dokumentuje moment, kdy útočník získá přístup do sítě pomocí legitimních přihlašovacích údajů.

### 🧠 Teoretický základ (Co se učím)

Vztah Taktika vs. Technika

Taktika (Proč): Initial Access – Útočník se snaží získat první "nohu ve dveřích" uvnitř cílové sítě.

Technika (Jak): Valid Accounts – Útočník nepoužívá exploit, ale přihlásí se jako legitimní uživatel (heslo získal např. phishingem nebo brute-force útokem).

Co je to Taktika: Initial Access?

Taktika představuje fázi, kdy útočník poprvé vstoupí do interní sítě.

Cíl: Navázat spojení se strojem uvnitř sítě (endpoint, server).

Proč to útočník dělá: Aby mohl začít provádět další kroky jako průzkum sítě zevnitř nebo šíření malwaru.

### 🌐 Kontext a Vrstvy

OSI Model Context

Vrstva 7 (Application): Protokoly jako RDP (Remote Desktop), SSH nebo SMB (sdílené složky), přes které probíhá autentizace.

Vrstva 4 (Transport): TCP spojení na porty 3389 (RDP) nebo 445 (SMB).

Datové zdroje (Data Sources)

Authentication Logs: Windows Event ID 4624 (Úspěšné přihlášení).

Network Traffic: Sledování neobvyklých časů přihlášení nebo přístupů z neznámých IP adres.

User Accounts: Sledování účtů, které se dlouho nepoužívaly a najednou jsou aktivní.

### 🛡️ Stav Detekce a Obrany

Stav: 🟢 Dobře detekovatelné (pokud se loguje úspěšné přihlášení).

Detekční logika: Wazuh Rule ID 60106 (Windows: Successful Logon).

### 📑 Sigma Rule (Detekce přihlášení mimo pracovní dobu)

```
title: Logon at Unusual Time
status: experimental
description: Detekuje úspěšné přihlášení uživatele v neobvyklý čas (např. noc).
logsource:
    product: windows
    service: security
detection:
    selection:
        EventID: 4624
    time_range:
        time: '22:00-06:00'
    condition: selection and time_range
```

⚔️ [DD.MM.RRRR] - Test 01: Vzdálený přístup přes RDP

### 🔴 Útok (Simulation)

Nástroj: Kali Linux (Rdesktop / xfreerdp)

Cíl: Windows 11 (192.168.20.20)

Příkaz:

xfreerdp /v:192.168.20.20 /u:admin /p:Heslo123


### 🔍 Výsledek a Analýza (Detection)

Wazuh Alert: Successful logon (Event ID 4624).

Pozorování: V Dashboardu vidíme zdrojovou IP útočníka (.30) a jméno použitého účtu.

### 📜 Traceability: Analýza logů (JSON)

Původ: Security log na Windows 11.

Klíčová data:
```json
{
  "win": {
    "system": {
      "eventID": "4624",
      "message": "An account was successfully logged on."
    },
    "eventdata": {
      "targetUserName": "admin",
      "ipAddress": "192.168.20.30",
      "logonType": "10" 
    }
  }
}
```

(Poznámka: Logon Type 10 znamená Remote Interactive – typické pro RDP).

### ⚠️ Analýza falešných pozitiv (False Positives)

Administrátor provádějící údržbu mimo pracovní dobu.

Automatizované skripty pro zálohování běžící pod servisním účtem.

### 💡 Možnosti zlepšení (Hardening)

MFA: Implementace dvoufázového ověření.


_____________________________________________________________________________________________________________________________________________________________________________________________________
_____________________________________________________________________________________________________________________________________________________________________________________________________




## 📝 Run #1 – T1078 / T1021.001 / T1550.002

Valid Accounts → Remote Desktop Protocol → NTLM Authentication (Possible Pass‑the‑Hash)

Date: 2026‑03‑22

Attacker: Kali (192.168.40.10)

Victim: Win11_Victim (192.168.30.10
)
Domain Controller: DC01.lab.local

### 1. Attack (Kali)

Útočník má k dispozici platné doménové přihlašovací údaje:

```
LAB\Petr.Novak
```

Z Kali se naváže RDP spojení na Win11
```bash
xfreerdp /v:192.168.30.10 /u:LAB\\Petr.Novak /p:'Heslo123!'
```

<img width="492" height="43" alt="image" src="https://github.com/user-attachments/assets/dbb9d73f-fb02-426c-b960-71e8cf5d60a2" />








### 2. Authentication on Win11 (Event ID 4624)
Windows 11 zaznamenal úspěšné síťové ověření:

```
Event ID: 4624
LogonType: 3
AuthenticationPackage: NTLM
WorkstationName: kali
IpAddress: 192.168.40.10
TargetUserName: Petr.Novak
```
### 3. Domain Controller (DC01) –
 objevil event potvrzující ověření doménového účtu:
```
Event ID: 4624
LogonType: 3
TargetUserName: Petr.Novak
IpAddress: 192.168.30.10
AuthenticationPackage: Kerberos / NTLM (podle handshake)

```
<img width="299" height="187" alt="image" src="https://github.com/user-attachments/assets/2d90a1f1-6248-4003-83a8-6f5c695e9fec" />

### 4. Wazuh Detection
Wazuh agent zachytil 

```
Event ID: 4624
LogonType: 3
Authentication: NTLMv2
Source: kali (192.168.40.10)
Target: LAB\Petr.Novak****

```

Wazuh ji vyhodnotil jako:

T1078.002 – Valid Accounts (Domain Accounts)

T1021.001 – Remote Desktop Protocol

T1550.002 – Pass‑the‑Hash

<img width="472" height="644" alt="image" src="https://github.com/user-attachments/assets/65345ba1-fd3f-462c-b8f5-e51e697be4fa" />

<img width="1313" height="470" alt="image" src="https://github.com/user-attachments/assets/d5d9a3a9-2647-442e-9179-f3415e9ef533" />







## 🧩 Závěr 
Během útoku jsem si uvědomil několik důležitých věcí:

RDP přihlášení nemusí vždy vytvořit LogonType 10 — pokud proběhne NTLM handshake, Windows to zaloguje jako LogonType 3, což je normální.

DC01 nikdy neukáže LogonType 10, protože RDP probíhá na cílovém stroji, ne na doménovém řadiči.

I když jsem některé logy hledal déle, nakonec jsem našel správný event na DC01 (4624 Type 3), který potvrzuje ověření účtu v doméně.

Wazuh mi pomohl pochopit, jak se jednotlivé události propojují a jak automaticky mapuje MITRE techniky.
