
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

Win11 log:

<img width="654" height="266" alt="image" src="https://github.com/user-attachments/assets/a50f587d-c66c-4ef6-95eb-5567ec655419" />


### 3. Domain Account Discovery (T1087.002) – CMD (20:00)

🔹 Doménoví uživatelé

```
net user /domain
```

Logy:

Win11:
4688 – Process Creation (net.exe

<img width="601" height="395" alt="image" src="https://github.com/user-attachments/assets/ba29f5b1-bf3c-4a98-88d9-1a933f71106e" />


DC01:
4624 – Network Logon


<img width="524" height="394" alt="image" src="https://github.com/user-attachments/assets/0a7fb609-1be4-4dbd-a344-122e794cfb03" />





### 🔹 Doménové skupiny
```
net group /domain
```
Logy:
Win11: 
4688 – net.exe

<img width="559" height="382" alt="image" src="https://github.com/user-attachments/assets/63d184d2-03f6-42f9-9469-a191de2f572b" />



DCO1:
4624 – Network Logon

<img width="524" height="394" alt="image" src="https://github.com/user-attachments/assets/39a686d9-e2e9-4291-8489-0aa0e249e1ff" />






### 🔹 Členové Domain Admins (HLAVNÍ ARTEFAKT)

```
net group "Domain Admins" /domain
```

Logy:


Win11:
4688 – net.exe

<img width="547" height="384" alt="image" src="https://github.com/user-attachments/assets/0eaba045-ca72-44b2-8fd0-d4ab152f89bd" />



DC01:


4624 – Logon Type 3

<img width="524" height="394" alt="image" src="https://github.com/user-attachments/assets/7d6f9bcb-3b2d-4e37-8a5b-b332f5257728" />






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
