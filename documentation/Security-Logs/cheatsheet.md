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
