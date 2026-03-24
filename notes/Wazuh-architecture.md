# 🏗️ Wazuh Architecture (Dev Perspective)

Tento přehled vysvětluje, jak SOC Lab funguje z pohledu (Backend/Frontend/Data).


| Součást | Role v Labu | Dev Přirovnání | Technologie |
| :--- | :--- | :--- | :--- |
| **Wazuh Agent** | Sběrač dat na endpointu | **Client / Edge App** | C / C++ |
| **Wazuh Manager** | Zpracování a analýza logů | **Backend API / Controller** | C, Python |
| **Wazuh Indexer** | Ukládání a vyhledávání | **NoSQL Database** | OpenSearch (Elastic) |
| **Wazuh Dashboard** | Vizualizace a správa | **Frontend Framework** | Kibana (React-based) |

---

### 🔄 Data Flow (JSON Pipeline)
1. **Event:** Na PC vznikne událost (např. Sysmon ID 1).
2. **Push:** Agent zabalí událost do **JSON** a pošle na Manager.
3. **Process:** Manager provede **Regex parsing** (Decoders) a porovná s **Rules**.
4. **Store:** Pokud vznikne alert, uloží se jako dokument do **Indexeru**.
5. **Query:** Analytik píše **KQL dotaz** ve Frontendu, který tahá data z DB.

### 🔍 Porovnání Query Jazyků
*   **SQL:** `SELECT * FROM logs WHERE eventID = 1;`
*   **KQL:** `data.win.eventdata.eventID: 1`
*   **Elastic DSL:** (Surový JSON query, který běží pod kapotou).
