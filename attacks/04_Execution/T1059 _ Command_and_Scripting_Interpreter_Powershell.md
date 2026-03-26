## MITRE ATT&CK – T1059.001 PowerShell
- Tactic: Execution (TA0002)
- Technique: T1059 – Command and Scripting Interpreter
- Sub-technique: T1059.001 – PowerShell

### Popis
Simulace spuštění PowerShellu s obcházením ExecutionPolicy a následnou síťovou komunikací.
______________________________________________________________________________________________________


### 1️⃣ Prostředí testu

  Datum: 23. 03. 2026
  
- Stanice (endpoint): Windows 11
  IP: 192.168.30.10
  Uživatel: LAB\Petr.Novak

- Domain Controller: Windows Server
  IP: 192.168.20.10
  Doména: LAB.LOCAL
  
Cíl techniky:  
Simulovat chování útočníka, který spouští PowerShell s obcházením bezpečnostních politik a provádí příkazy, které mohou vést k dalšímu průniku, stahování payloadů nebo k laterálnímu pohybu.
__________________________________________________________________________________________________________________________________________________________________________________________________


### 2️⃣ Aktivita útočníka

19:17–19:21

Použité příkazy (PowerShell):


<img width="930" height="77" alt="image" src="https://github.com/user-attachments/assets/595ab62a-71da-4732-b84c-bb1658e57908" />





<img width="1101" height="71" alt="image" src="https://github.com/user-attachments/assets/c2c377c3-aaaa-436d-8c33-31bf8b5c81a0" />


Popis:
Útočník spouští PowerShell s parametrem ExecutionPolicy Bypass, aby obešel restrikce.

Provádí příkaz Get-Process (rekognoskace).

Následně provádí síťovou komunikaci pomocí Invoke-WebRequest.

Tato technika je běžná při Execution, často navazuje na Initial Access.


### 3️⃣ Logy a artefakty

🖥️ Win11 – Lokální logy
Očekávané eventy:
Event ID	Log	Popis

4688	Security	Vytvoření procesu powershell.exe

<img width="815" height="244" alt="image" src="https://github.com/user-attachments/assets/9e3c6c0b-5f71-4f97-ba31-1b227fedb44e" />


4688	Security	Spuštění Invoke-WebRequest

<img width="823" height="258" alt="image" src="https://github.com/user-attachments/assets/2ecd0200-4a4e-46a5-a054-882cb973b917" />

Sysmon 1	Process Creation	Detailní informace o spuštění PowerShellu

<img width="860" height="235" alt="image" src="https://github.com/user-attachments/assets/18e7d4db-54e8-44f7-a5be-ee514856c1ea" />


Sysmon 3	Network Connection	Síťová komunikace PowerShellu



id:1

<img width="837" height="374" alt="image" src="https://github.com/user-attachments/assets/f2bcf2fc-0129-4756-b801-8791a9865766" />

id:3 - powershell navázal tcp spojení 

<img width="591" height="384" alt="image" src="https://github.com/user-attachments/assets/56ce6c0e-3dbb-4c47-845e-603fb58665a4" />


Důležité artefakty:

CommandLine obsahující -ExecutionPolicy Bypass

ParentProcess: explorer.exe nebo cmd.exe

Síťové spojení na externí IP/URL


### 🖥️ DC01 – Doménové logy

U této techniky se obvykle objeví:

Event ID	Popis
4624	Logon Type 3 (pokud PowerShell navazuje síťové spojení přes doménové služby)

DC loguje pouze síťovou autentizaci (LogonType 3).

<img width="464" height="321" alt="image" src="https://github.com/user-attachments/assets/bf078e75-44af-449e-add6-94c4b78b34fa" />



### 🛡️ Wazuh – Alerty

Očekávané detekce:
Alert 1 – Suspicious PowerShell Execution

NewProcessName: C:\Windows\System32\WindowsPowerShell\v1.0\powershell.exe

Event ID: 4688

✔ Alert 1 – Suspicious PowerShell Execution
→ ExecutionPolicy Bypass, Get‑Process

<img width="1171" height="618" alt="image" src="https://github.com/user-attachments/assets/e5d6b420-c89d-4227-b0c1-4049e2b9476c" />



✔ Alert 2 – PowerShell Network Activity
→ Invoke‑WebRequest, Sysmon Event ID 3

<img width="1179" height="436" alt="image" src="https://github.com/user-attachments/assets/e3ecea81-e022-4795-a1f7-daa265eb4722" />

✔ Alert 3 – Process Creation
→ powershell.exe 



<img width="1191" height="470" alt="image" src="https://github.com/user-attachments/assets/80790003-2ea6-4ee6-b9da-d28769cb300d" />



Rule: Suspicious PowerShell network behavior


### 5️⃣ Detekce
Detekce proběhla na všech vrstvách:

✔ Win11 (endpoint)
Sysmon Event ID 1 a 3

Security log 4688

✔ DC01 (doménový řadič)
případné ověřovací logy (4624)

✔ Wazuh (SIEM)
alerty na suspicious PowerShell execution

alerty na síťovou komunikaci PowerShellu

### 6️⃣ Závěr
Detekce PowerShell aktivity proběhla úspěšně napříč celou logovací pipeline:

endpoint → Sysmon + Security logy

DC → autentizační logy

SIEM → korelace a alerty

„Tento run simuluje běžné chování útočníka, který používá PowerShell pro spuštění kódu, obcházení politik a komunikaci s externími zdroji.“
