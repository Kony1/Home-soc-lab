
## 📝 Run #1 – T1087 Account Discovery

Discovery → Account Discovery (Local + Domain)  

Date: 2026‑03‑22

User: LAB\Petr.Novak

Host: Win11_Victim (192.168.30.10)

Domain Controller: DC01.lab.local

### 1. Attack Narrative

Útočník má platný doménový účet Petr.Novak a provádí průzkum účtů na kompromitovaném stroji Win11.

Cílem je zjistit:
```
lokální účty

lokální skupiny

doménové účty

doménové skupiny

členy privilegovaných skupin
```

Tato aktivita odpovídá MITRE technice:

T1087 – Account Discovery

T1087.001 – Local Account Discovery

T1087.002 – Domain Account Discovery

### 2. Local Account Discovery (T1087.001)

🔹 PowerShell – lokální skupiny a členové
Příkaz:

```
Get-LocalGroupMember -Group "Administrators"
```

Očekávaný log:

<img width="654" height="266" alt="image" src="https://github.com/user-attachments/assets/a50f587d-c66c-4ef6-95eb-5567ec655419" />


### 3. Domain Account Discovery (T1087.002) – CMD (20:00)

🔹 Doménoví uživatelé

```
net user /domain
```

Logy:

4688 – Process Creation (net.exe)

4624 – Logon Type 3 (DC01)

4662 – Directory Service Access (LDAP)

(Sem vlož screenshoty)

### 🔹 Doménové skupiny
```
net group /domain
```
Logy:

4688 – net.exe

4662 – LDAP dotazy

4624 – Network Logon


(Sem vlož screenshoty)


### 🔹 Členové Domain Admins (HLAVNÍ ARTEFAKT)

```
net group "Domain Admins" /domain
```

Logy:

Win11:
4799 – Group Membership Enumeration

TargetUserName: Domain Admins

SubjectUserName: Petr.Novak

DC01:
4662 – LDAP dotaz na Domain Admins

4624 – Logon Type 3

4688 – net.exe

(Sem vlož hlavní screenshot 4799 + DC logy)


6. Wazuh Detection
Wazuh zachytil aktivitu jako:

T1087 – Account Discovery

Reconnaissance

Suspicious enumeration

Screenshot sem →

### 🧩 . Závěr
Útočník provedl kompletní průzkum účtů a skupin v doméně.
Detekce proběhla na:

Win11 (lokální logy)

DC01 (LDAP + logon)

Wazuh (MITRE alerty)


Tento run mi pomohl pochopit, jak Windows a DC logují průzkum účtů.
Zjistil jsem několik důležitých věcí:

Běžný doménový uživatel může enumerovat lokální i doménové účty pomocí net příkazů.

I bez RSATu se na DC01 generují LDAP dotazy a Directory Service Access logy.

Win11 loguje enumeraci skupin (4799), což jsem dřív neznal.

Wazuh automaticky mapuje tyto akce na MITRE T1087 a označuje je jako reconnaissance.

Celý proces je bezpečný a ideální pro učení — nic jsem nepoškodil, jen jsem sledoval logy.

Beru to jako další krok v učení.
Není to dokonalé, ale je to reálné, pochopitelné 






PowerShell enumeration activity

(Sem vlož screenshoty Wazuh alertů)
