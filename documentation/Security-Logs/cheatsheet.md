# 🔐 Windows Security Event ID Cheat Sheet 

Tento přehled obsahuje nativní Windows události (Security Log), které jsou klíčové pro detekci průniku do systému a pohybu útočníka.


| Event ID | Název události |  Význam pro SOC Analýzu |
| :--- | :--- | :--- |
| **4624** | **Logon Success** | Úspěšné přihlášení. Sleduji `Logon Type` (3 = Síť, 10 = RDP). |
| **4625** | **Logon Failure** | **Brute Force!** Detekce pokusů o hádání hesel. |
| **4688** | **New Process** | Podobné jako Sysmon ID 1. Vytvoření nového procesu (pokud je zapnuto auditování). |
| **4698** | **Scheduled Task Created** | **Persistence.** Detekce vytvoření naplánované úlohy (tvůj útok). |
| **4720** | **User Created** | Útočník si vytvořil vlastní uživatelský účet. |
| **4732** | **Added to Group** | Uživatel přidán do citlivé skupiny (např. `Administrators`). **Kritické!** |
| **4672** | **Special Privileges** | Přihlášení s administrátorskými právy. |
| **1102** | **Audit Log Cleared** | **Zametání stop.** Útočník smazal záznamy v Event Vieweru. |

---
### 💡 Klíčové Logon Typy (u ID 4624):
*   **Typ 2:** Lokální přihlášení (přímo u klávesnice).
*   **Typ 3:** Síťové přihlášení (sdílené složky, IIS).
*   **Typ 7:** Odemčení zamknutého PC.
*   **Typ 10:** **Remote Desktop (RDP)** - nejdůležitější pro detekci vzdáleného útoku.


_______________________________________________________________________________________

update 


## 👤 Autentizace & Přihlášení

| Event ID	| Název	| Význam pro SOC |
| :--- | :--- | :--- |
| **4624** |	**Logon Success** |	Úspěšné přihlášení. Sleduj Logon Type (3 = síť, 10 = RDP). |
| **4625** | Logon Failure	| Neúspěšné přihlášení. Brute force, špatné heslo. |
| **4648** |	Explicit Credential Logon	| runas, skripty, laterální pohyb. |
| **4672** |	Special Privileges  Assigned	| Admin login. Kritické. |
| **4634** |	Logoff |	Odhlášení uživatele. |
|**4647** |	User Initiated Logoff |	Uživatel se odhlásil ručně. |


## 🔑 Klíčové Logon Typy (u 4624)

| Typ |	Význam |
| :--- | :--- | 
| **2** |	Lokální přihlášení (u klávesnice). |
| **3** |	Síťové přihlášení (SMB, IIS, služby). |
| **7** |	Odemčení zamknutého PC. |
| **10** |	RDP přihlášení. |
| **11** |	Cached logon (offline). |


##⚙️ Procesy & Systém

| Event | ID | Název | Význam |
| :--- | :--- | :--- |
| **4688** | New Process Created | Klíčové pro detekci útoku. Podobné jako Sysmon ID 1. |
| **4689** | Process Terminated | Proces skončil. |
| **4697** | Service Installed | Persistence – instalace služby. |
4698	Scheduled Task Created	Persistence – vytvoření úlohy.
4702	Scheduled Task Updated	Úprava úlohy (taky persistence).
1102	Audit Log Cleared	Útočník maže stopy.


##🌐 Síť & Scanning (DŮLEŽITÉ PRO NMAP)

Tyhle eventy budou padat při scanu z Kali.

| Event | ID	| Název |	Význam |
| :--- | :--- | :--- |
| **5152** | Filtering Platform Blocked Packet | Firewall zablokoval paket (často SYN při port scanu). |
| **5156** | Filtering Platform Allowed Connection |	Povoleno spojení. Uvidíš při ICMP, SYN, OS detection. |
| **5157** | Filtering Platform Blocked Connection |	Blokované spojení (často při port scanu). |
| **5158** | Filtering Platform Allowed Bind | Proces si otevřel port (užitečné při malware). |


##💡 Co uvidíš při Nmap scanu
| :--- | :--- | :--- |
Typ scanu	Co se objeví v logách
Ping sweep (-sn)	5156 (ICMP), Sysmon ID 3 (ARP/ICMP)
SYN scan (-sS)	5156 + 5157 (SYN/SYN-ACK/RESET)
Service detection (-sV)	5156 (více handshake pokusů)
OS detection (-O)	5156/5157 (speciální fingerprint pakety)


##🛡️ Účty & Skupiny

Event ID	Název	Význam
| :--- | :--- | :--- |
4720	User Created	Útočník vytvořil účet.
4722	User Enabled	Účet byl aktivován.
4723	Password Change Attempt	Pokus o změnu hesla.
4724	Password Reset	Reset hesla (často zneužití).
4732	Added to Group	Uživatel přidán do skupiny (např. Administrators).
4733	Removed from Group	Odebrání ze skupiny.


## 📁 Sdílené Složky & SMB

Event ID	Název	Význam
| :--- | :--- | :--- |
5140	Network Share Access	Přístup ke sdílené složce.
5142	Share Created	Vytvoření sdílené složky.
5143	Share Modified	Úprava sdílené složky.
5144	Share Deleted	Smazání sdílené složky.

## 🔥 Doporučené použití v SOC

Nmap scanning → sleduj 5156 + 5157

Laterální pohyb → 4624 (Type 3/10), 4648

Privilege escalation → 4672, 4732

Persistence → 4697, 4698, 4702

Malware → 4688 (procesy), 5158 (bind port)

Zametání stop → 1102
