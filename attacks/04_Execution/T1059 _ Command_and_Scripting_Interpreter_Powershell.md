## MITRE ATT&CK – T1059.001 PowerShell
- Tactic: Execution (TA0002)
- Technique: T1059 – Command and Scripting Interpreter
- Sub-technique: T1059.001 – PowerShell

### Popis
Simulace spuštění PowerShellu s obcházením ExecutionPolicy a následnou síťovou komunikací.
______________________________________________________________________________________________________

### Použité příkazy
powershell.exe -NoProfile -ExecutionPolicy Bypass -Command "Get-Process"
powershell.exe -NoProfile -ExecutionPolicy Bypass -Command "Invoke-WebRequest http://example.com/test"

### Logy
- Sysmon Event ID 1 – Process Creation  
- Sysmon Event ID 3 – Network Connection  
- Wazuh alerty na suspicious PowerShell execution



### 1️⃣ Kontext

Datum: 23. 03. 2026
Host: Win11 (192.168.30.10)
DC: DC01 (192.168.20.10)

Cíl techniky:  
Simulovat chování útočníka, který spouští PowerShell s obcházením bezpečnostních politik a provádí příkazy, které mohou vést k dalšímu průniku, stahování payloadů nebo k laterálnímu pohybu.
__________________________________________________________________________________________________________________________________________________________________________________________________


### 2️⃣ Aktivita útočníka

Použité příkazy (PowerShell):
Kód
powershell.exe -NoProfile -ExecutionPolicy Bypass -Command "Get-Process"
Kód
powershell.exe -NoProfile -ExecutionPolicy Bypass -Command "Invoke-WebRequest http://example.com/test"
Popis:
Útočník spouští PowerShell s parametrem ExecutionPolicy Bypass, aby obešel restrikce.

Provádí příkaz Get-Process (rekognoskace).

Následně provádí síťovou komunikaci pomocí Invoke-WebRequest.

Tato technika je běžná při Execution, ale často navazuje na Initial Access.


###3️⃣ Logy a artefakty

🖥️ Win11 – Lokální logy
Očekávané eventy:
Event ID	Log	Popis
4688	Security	Vytvoření procesu powershell.exe
4688	Security	Spuštění Invoke-WebRequest
Sysmon 1	Process Creation	Detailní informace o spuštění PowerShellu
Sysmon 3	Network Connection	Síťová komunikace PowerShellu

Důležité artefakty:
CommandLine obsahující -ExecutionPolicy Bypass

ParentProcess: explorer.exe nebo cmd.exe

Síťové spojení na externí IP/URL


### 🖥️ DC01 – Doménové logy

U této techniky se obvykle objeví:

Event ID	Popis
4624	Logon Type 3 (pokud PowerShell navazuje síťové spojení přes doménové služby)


### 🛡️ Wazuh – Alerty

Očekávané detekce:
Alert 1 – Suspicious PowerShell Execution

NewProcessName: C:\Windows\System32\WindowsPowerShell\v1.0\powershell.exe

CommandLine: obsahuje ExecutionPolicy Bypass

ParentProcess: explorer.exe nebo cmd.exe

Event ID: 4688

Alert 2 – PowerShell Network Activity

Sysmon Event ID: 3

Destination IP: externí adresa

Process: powershell.exe

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
