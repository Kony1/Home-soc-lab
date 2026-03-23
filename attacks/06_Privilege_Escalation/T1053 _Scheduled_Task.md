

## 📝 Run #1 – T1053.005 Scheduled Task (Privilege Escalation)

Taktika: Privilege Escalation

Technika: T1053 – Scheduled Task / Job

Subtechnika: T1053.005 – Scheduled Task (Windows)


## 🎯 Cíl útočníka

Útočník se snaží vytvořit nebo upravit plánovanou úlohu ve Windows, aby:

získal vyšší oprávnění (Privilege Escalation)

spustil škodlivý kód mimo dohled uživatele

získal persistenci (opakované spouštění kódu)

obešel bezpečnostní kontroly (např. EDR, AV)

spustil payload v čase, kdy si toho nikdo nevšimne

Plánované úlohy jsou oblíbené, protože:

běží pod SYSTEM nebo jiným privilegovaným účtem

dají se vytvořit i přes CMD/PowerShell

jsou běžnou součástí Windows → méně nápadné

generují specifické logy, které SOC musí umět detekovat

Tahle technika je extrémně častá v reálných útocích (APT, ransomware, red team).
_________________________________________________________________________________



### 1️⃣ Kontext

Datum: 23.03. 2026

Host: Win11  (192.168.30.10)

DC: DC01  (192.168.20.10)

Cíl techniky:  
Útočník vytváří plánovanou úlohu, aby získal vyšší oprávnění, persistenci nebo spustil škodlivý kód mimo dohled uživatele.


### 2️⃣ Aktivita útočníka

Použitý příkaz (CMD nebo PowerShell):

powershell

<img width="892" height="19" alt="image" src="https://github.com/user-attachments/assets/1f1f7559-f093-4268-857b-6191dfcc853c" />

Popis:

Útočník vytváří novou plánovanou úlohu s názvem Updater.

Technika se používá pro privilege escalation nebo persistence.


### 3️⃣Logy a artefakty

🖥️ Win11 – Lokální logy

Očekávané eventy:

```
4688	  Security      	Vytvoření procesu (schtasks.exe)
```

<img width="602" height="427" alt="image" src="https://github.com/user-attachments/assets/4e670789-afae-4302-9952-ad5e8651f7d2" />



Event ID 4698 se v tomto běhu nevygeneroval, protože audit policy pro Task Scheduler není ve Windows defaultně zapnutá. To je běžné chování.




🖥️ DC01 – Doménové logy

U této techniky se obvykle objeví jen:

Event ID	Popis

```
4624	Logon Type 3 (pokud task komunikuje se sítí)
```

<img width="538" height="289" alt="image" src="https://github.com/user-attachments/assets/5b62b7e8-8a1b-49ce-90ea-2b8937d46dd9" />



### 🛡️ Wazuh – Alerty

Očekávané:

#### Alert na spuštění příkazu whoami.exe naplánovanou úlohou
```
NewProcessName: C:\Windows\System32\schtasks.exe

CommandLine: "schtasks.exe" /query /tn Updater

ParentProcess: powershell.exe

User: LAB\Petr.Novak

Integrity Level: S-1-16-8192 (High Integrity)
```
Event ID: 4688 (Process Creation)

<img width="780" height="624" alt="image" src="https://github.com/user-attachments/assets/ae4e1edf-23ca-4762-b2d8-4af330341502" />



#### Alert na spuštění příkazu whoami.exe naplánovanou úlohou

```
NewProcessName: C:\Windows\System32\whoami.exe

CommandLine: whoami

ParentProcess: C:\Windows\System32\cmd.exe

User: LAB\Petr.Novak

Integrity Level: S-1-16-8192 (High Integrity)
```

Event ID: 4688 (Process Creation)


<img width="648" height="566" alt="image" src="https://github.com/user-attachments/assets/f02dfa27-1e0e-4918-8d07-3b58d0720de8" />


Tento log potvrzuje, že naplánovaná úloha byla úspěšně spuštěna a provedla příkaz, což odpovídá technice MITRE T1053






### 5️⃣ Detekce

Detekce proběhla na všech relevantních úrovních:

Win11 – procesní logy (4688)

DC01 – případné ověřovací logy (4624)

Wazuh – alert na vytvoření procesu schtasks.exe / modifikaci tasku


### 6️⃣ Závěr

Detekce proběhla napříč všemi vrstvami infrastruktury — na úrovni endpointu (Win11), doménového řadiče (DC01) i SIEM vrstvy (Wazuh). To potvrzuje správnou konfiguraci logovací pipeline a konzistentní sběr bezpečnostních událostí.

„Tento run věrně simuluje reálné chování útočníka, který využívá plánované úlohy pro privilege escalation nebo persistence.“





