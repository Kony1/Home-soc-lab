# 🔍 Sysmon Event ID Cheat Sheet 

Tento přehled používám pro korelaci útoků (MITRE ATT&CK) a analýzu logů ve Wazuh SIEMu.


| Event ID | Název události | Význam pro Security Analýzu |
| :--- | :--- | :--- |
| **ID 1** | **Process Creation** | **Klíčové.** Vidím přesný příkaz (`CommandLine`) a rodičovský proces. |
| **ID 3** | **Network Connection** | Spojení programu s IP (L3/L4). Detekce Reverse Shellů a C2 komunikace. |
| **ID 5** | **Process Terminated** | Ukončení procesu. Hacker mohl vypnout bezpečnostní softwar (AV/EDR). |
| **ID 7** | **Image Loaded** | Načtení DLL knihovny. Detekce **DLL Side-loading** útoků. |
| **ID 8** | **CreateRemoteThread** | **Kritické.** Jeden proces vkládá kód do jiného (**Process Injection**). |
| **ID 10** | **ProcessAccess** | Přístup k paměti jiného procesu (např. Mimikatz čte hesla z `lsass.exe`). |
| **ID 11** | **FileCreate** | Vytvoření souboru (např. stažení payloadu nebo vytvoření skriptu). |
| **ID 12/13** | **Registry Event** | **Persistence.** Změny v registrech (např. klíče `Run` nebo `Services`). |
| **ID 22** | **DNS Query** | Dotaz na doménu. Vidím, kam se malware snaží "dovolat" (Exfiltrace). |
| **ID 23** | **FileDelete** | Smazání souboru. Často se používá k zametání stop po útoku. |

---
*Tip: Ve Wazuh Dashboardu filtruji pole `data.win.eventdata.eventID` pro rychlé dohledání těchto událostí.*
