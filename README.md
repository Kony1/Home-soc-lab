# 🛡️ Home SOC & Detection Lab

Tento projekt dokumentuje stavbu a provoz domácího bezpečnostního labu pro simulaci útoků, analýzu logů a nastavování detekčních pravidel v SIEM.

---

## 🏗️ Architektura sítě
Pomocí tohoto diagramu vizualizuji tok dat od útočníka k oběti a následnou detekci.

```mermaid
graph LR
    subgraph External_Network
        A[Kali Linux - Attacker]
    end

    subgraph Internal_Lab
        V[Windows/Linux - Victim]
        S[Wazuh Server - SIEM]
    end

    A -- "Exploits/Scans" --> V
    V -- "Sysmon/Logs" --> S
    S -- "Alerts" --> Dashboard[SOC Dashboard]
