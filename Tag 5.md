# Modul 164 – Datenbanken

## Tag 5

### Auftrag Recap  
🔗 [GitLab Link zum Löschen von Daten](https://gitlab.com/ch-tbz-it/Stud/m164/-/tree/main/5.Tag?ref_type=heads)

### 📚 Datenbank-Lernnotizen  

#### 🗑️ Löschen in Datenbanken  
Beim Löschen von Daten in einer Datenbank gibt es verschiedene Ansätze:  
- **DELETE**: Entfernt Datensätze aus einer Tabelle, aber die Struktur bleibt erhalten.  
- **TRUNCATE**: Löscht alle Daten einer Tabelle, setzt Auto-Increment-Werte zurück, ist aber schneller als DELETE.  
- **DROP**: Löscht eine gesamte Tabelle inkl. Struktur – Vorsicht, dies ist nicht rückgängig zu machen!  

#### 🔗 Datenintegrität  
Datenintegrität stellt sicher, dass die Daten in einer Datenbank **korrekt, konsistent und zuverlässig** sind.  
Wichtige Mechanismen zur Wahrung der Integrität:  
- **Primärschlüssel** (Eindeutigkeit der Datensätze)  
- **Fremdschlüssel** (Beziehungen zwischen Tabellen)  
- **Constraints** wie `NOT NULL`, `UNIQUE`, `CHECK`  
- **Transaktionen** (z. B. mit `COMMIT` und `ROLLBACK`)  

#### 🔄 FK-Constraint-Options  
Fremdschlüssel (FK) regeln, wie abhängige Daten bei Änderungen behandelt werden:  
- `CASCADE` 🔄 → Änderungen/Löschungen vererben sich  
- `SET NULL` 🚫 → Setzt FK-Wert auf NULL, falls möglich  
- `SET DEFAULT` 🎯 → Setzt FK-Wert auf einen Standardwert  
- `RESTRICT` ❌ → Verhindert Löschung, wenn abhängige Daten existieren  
- `NO ACTION` 🏁 → Wie `RESTRICT`, aber mit verzögerter Prüfung