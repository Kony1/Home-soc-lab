## 📝 T1531 – Account Access Removal (Impact)


Analýza Windows Event ID 4625 (Failed Logon) zachycená pomocí Wazuh


### 🧩 Popis situace
Na monitorovaném Windows zařízení došlo k neúspěšnému ověření uživatelského účtu.
Událost byla zachycena pomocí Wazuh agenta a odeslána do Wazuh Manageru, kde byla automaticky klasifikována jako MITRE ATT&CK technika:


T1531 – Account Access Removal (Impact)
Tato technika se používá v situacích, kdy dochází k pokusům o manipulaci s přístupem k účtům nebo k opakovaným selháním ověření, která mohou indikovat problém s dostupností účtu.



###  🧪 Testovací aktivita v rámci labu

V rámci testování jsem z jiného sítového  segmentu (Kali Linux) provedl  ověření vůči Windows stanici Win11_Victim.
Cílem bylo ověřit, jak Windows zaznamenává neúspěšné pokusy o přihlášení a jak je následně zpracuje Wazuh.


Použitý příkaz:




<img width="516" height="20" alt="image" src="https://github.com/user-attachments/assets/547123fc-a502-43a3-abb3-7e4447f7d818" />






###  🖥️ Co se stalo


Na cílovém systému Win11_Victim proběhl pokus o ověření účtu:

``` Uživatel: Petr.Novak

Zdrojová IP: 192.168.40.10

Ověřovací metoda: NTLM (síťové ověření = LogonType 3)

Výsledek: Neúspěšné ověření

Důvod: Neznámé uživatelské jméno nebo chybné heslo


Windows událost:

Event ID: 4625

Status: 0xC000006D (bad credentials)

SubStatus: 0xC0000064 (user not found)
```

###  🔍 Proč je LogonType 3 důležitý

LogonType: 3 znamená:

síťové ověření (např. NTLM handshake),

nejedná se o RDP přihlášení,

Windows odmítl ověření ještě před tím, než by došlo k interaktivnímu přihlášení.

Proto se tato událost neklasifikuje jako brute‑force (T1110), ale jako Impact.



###  🧠 Proč Wazuh použil MITRE T1531

Wazuh má vlastní mapování MITRE ATT&CK a některé typy událostí 4625 automaticky zařazuje do:



T1531 – Account Access Removal
Důvod:

jde o selhání ověření účtu,

může indikovat pokus o přístup k neexistujícímu nebo blokovanému účtu,

není to brute‑force (chybí opakované pokusy a LogonType 10).



###  🕵️ Detekce ve Wazuhu

Filtry použité pro nalezení události:
```
agent.name: "Win11_Victim"

event.code: 4625

data.win.eventdata.logonType: 3

rule.id: 60122

description: Logon Failure - Unknown user or bad password
```

###  1️⃣ Wazuh 



<img width="454" height="210" alt="image" src="https://github.com/user-attachments/assets/1e426cfa-35dc-48e5-a8e3-dc2c95f11fb6" />












<img width="535" height="210" alt="image" src="https://github.com/user-attachments/assets/fe4ade6e-f557-46cc-8763-83bab957304a" />









### 🧾 Závěr

Tato událost představuje běžné neúspěšné ověření účtu přes síťové ověřování NTLM.
Není to brute‑force ani RDP útok.
Wazuh správně klasifikoval událost jako T1531 – Account Access Removal, protože jde o selhání přístupu k účtu v rámci taktiky Impact.
