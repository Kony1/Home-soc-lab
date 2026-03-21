T1531 – Account Access Removal (Windows Failed Logon 4625)
Popis
Windows generuje událost Event ID 4625, když se nepodaří ověřit přihlášení.
V tomto případě šlo o síťové ověření (LogonType 3) přes NTLM, které selhalo z důvodu neexistujícího účtu nebo špatného hesla.

Detaily události
event.code: 4625

data.win.eventdata.logonType: 3

data.win.eventdata.authenticationPackageName: NTLM

data.win.eventdata.status: 0xC000006D

data.win.eventdata.subStatus: 0xC0000064

data.win.eventdata.ipAddress: 192.168.40.10

data.win.eventdata.targetUserName: Petr.Novak

Interpretace
LogonType 3 znamená síťové přihlášení, nikoli RDP.
K RDP by došlo pouze při LogonType 10.

Událost tedy reprezentuje:

pokus o ověření účtu

přes NTLM

který byl odmítnut ještě před interaktivní fází

MITRE ATT&CK
Wazuh automaticky mapoval tuto událost na:

T1531 – Account Access Removal

Tactic: Impact

Detekce ve Wazuhu
Filtry:

Kód
event.code: 4625
Kód
data.win.eventdata.logonType: 3
Kód
agent.name: "Win11_Victim"
Poznámka
Tato událost není Remote Desktop přihlášení (LogonType 10) a není klasifikována jako brute‑force.
