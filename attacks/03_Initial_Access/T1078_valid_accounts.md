🚪 Taktika: Initial Access (03)

Technika: T1078 - Valid Accounts (Platné účty)

Poznámka: Tento report slouží jako vzorová technika pro taktiku Initial Access. Dokumentuje moment, kdy útočník získá přístup do sítě pomocí legitimních přihlašovacích údajů.

🧠 Teoretický základ (Co se učím)

Vztah Taktika vs. Technika

Taktika (Proč): Initial Access – Útočník se snaží získat první "nohu ve dveřích" uvnitř cílové sítě.

Technika (Jak): Valid Accounts – Útočník nepoužívá exploit, ale přihlásí se jako legitimní uživatel (heslo získal např. phishingem nebo brute-force útokem).

Co je to Taktika: Initial Access?

Taktika představuje fázi, kdy útočník poprvé vstoupí do interní sítě.

Cíl: Navázat spojení se strojem uvnitř sítě (endpoint, server).

Proč to útočník dělá: Aby mohl začít provádět další kroky jako průzkum sítě zevnitř nebo šíření malwaru.

🌐 Kontext a Vrstvy

OSI Model Context

Vrstva 7 (Application): Protokoly jako RDP (Remote Desktop), SSH nebo SMB (sdílené složky), přes které probíhá autentizace.

Vrstva 4 (Transport): TCP spojení na porty 3389 (RDP) nebo 445 (SMB).

Datové zdroje (Data Sources)

Authentication Logs: Windows Event ID 4624 (Úspěšné přihlášení).

Network Traffic: Sledování neobvyklých časů přihlášení nebo přístupů z neznámých IP adres.

User Accounts: Sledování účtů, které se dlouho nepoužívaly a najednou jsou aktivní.

🛡️ Stav Detekce a Obrany

Stav: 🟢 Dobře detekovatelné (pokud se loguje úspěšné přihlášení).

Detekční logika: Wazuh Rule ID 60106 (Windows: Successful Logon).

📑 Sigma Rule (Detekce přihlášení mimo pracovní dobu)

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


⚔️ [DD.MM.RRRR] - Test 01: Vzdálený přístup přes RDP

🔴 Útok (Simulation)

Nástroj: Kali Linux (Rdesktop / xfreerdp)

Cíl: Windows 11 (192.168.20.20)

Příkaz:

xfreerdp /v:192.168.20.20 /u:admin /p:Heslo123


🔍 Výsledek a Analýza (Detection)

Wazuh Alert: Successful logon (Event ID 4624).

Pozorování: V Dashboardu vidíme zdrojovou IP útočníka (.30) a jméno použitého účtu.

📜 Traceability: Analýza logů (JSON)

Původ: Security log na Windows 11.

Klíčová data:

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


(Poznámka: Logon Type 10 znamená Remote Interactive – typické pro RDP).

⚠️ Analýza falešných pozitiv (False Positives)

Administrátor provádějící údržbu mimo pracovní dobu.

Automatizované skripty pro zálohování běžící pod servisním účtem.

💡 Možnosti zlepšení (Hardening)

MFA: Implementace dvoufázového ověření.

VPN: Nedovolit RDP přímo z jiné sítě bez zabezpečeného tunelu.

Audit: Pravidelná revize práv uživatelů (Least Privilege).
