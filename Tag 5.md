# Modul 164 – Datenbanken

## Tag 5

### Auftrag Recap  
🔗 [GitLab Link zum Löschen von Daten](https://gitlab.com/ch-tbz-it/Stud/m164/-/tree/main/5.Tag?ref_type=heads)

#### Zusammenfassung
 
In professionellen Datenbanksystemen wird das **Löschen** von Datensätzen (z. B. per `DELETE`) häufig vermieden, da es zu **Informationsverlust** und Problemen mit der **Datenintegrität** führen kann. Stattdessen werden Datensätze oft als „inaktiv“ markiert oder um **Zeitstempel** bzw. **Statusangaben** (z. B. Stornierungen) erweitert, sodass **historische Abläufe** und Beziehungen nachvollziehbar bleiben.
 
##### Warum nicht einfach löschen? 🚫
 
- **Beziehungen gehen verloren**  

  Wenn ein Mitarbeiterdatensatz gelöscht wird, können alle historischen Aktionen (z. B. Logins, Bestellungen, Buchungen) nicht mehr nachverfolgt werden.  
 
- **Referentielle Integrität**  

  Fremdschlüssel-Beziehungen (FK-Constraints) verhindern das Löschen oft schon technisch, da zugehörige Einträge in Detailtabellen ebenfalls entfernt werden müssten.  
 
- **Rechtliche Aspekte**  

  In bestimmten Bereichen (z. B. Banken) müssen alle Änderungen lückenlos dokumentiert werden. Durch das Löschen könnten Beweise oder Nachverfolgungsmöglichkeiten verloren gehen.
 
##### Alternativen zum Löschen
 
- **Inaktiv-Flag**  

  Anstatt Datensätze zu löschen, markiert man sie als „inaktiv“. Auf diese Weise bleiben sie für Analysen oder spätere Auswertungen vorhanden.  
 
- **Historisierung**  

  Änderungen werden nicht überschrieben, sondern führen zu neuen Einträgen. So kann man zeitliche Abläufe (z. B. mehrfaches Verleihen eines Gegenstands) vollständig dokumentieren.  
 
- **Stornierungen**  

  In Kassensystemen werden anstelle von Löschungen Stornierungen vorgenommen, die die ursprüngliche Buchung neutralisieren, ohne sie zu entfernen. So bleibt die gesamte Transaktionshistorie nachvollziehbar.
 
##### Datenintegrität 🗄️
 
- **Eindeutigkeit & Konsistenz**  

  Jeder Datensatz muss eindeutig identifiziert werden können und darf nicht ungewollt verändert werden.  
 
- **Referenzielle Integrität**  

  Eine Bestellung (Detail) kann nur existieren, wenn ein zugehöriger Kunde (Master) vorhanden ist.  
 
- **Passende Datentypen & Constraints**  

  Daten müssen im korrekten Format gespeichert werden und nur gültige Werte akzeptiert werden.  
 
- **Validierung**  

  Vor dem Einfügen und Ändern werden Daten geprüft, um Fehler zu verhindern.
 
##### Fremdschlüssel-Regeln beim Löschen ⚙️
 
| **Regel**                              | **Beschreibung**                                                                                                                                                  |

|----------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------|

| **ON DELETE NO ACTION (RESTRICT)**     | Ein `DELETE` in der Primärtabelle ist nur möglich, wenn keine verknüpften Detail-Datensätze existieren. (Standard, falls nichts anderes angegeben)               |

| **ON DELETE CASCADE**                  | Das Löschen in der Primärtabelle führt zum automatischen Löschen aller zugehörigen Detail-Datensätze. (Vorsicht: Daten können unbeabsichtigt komplett verschwinden!) |

| **ON DELETE SET NULL (DEFAULT)**       | Beim Löschen in der Primärtabelle wird der Fremdschlüssel in der Detailtabelle auf `NULL` oder einen Default-Wert gesetzt (nur möglich, wenn `NULL` erlaubt ist).  |
 
> **Fazit**: In den meisten Fällen ist es **sinnvoller**, Datensätze zu **historisieren** oder als **inaktiv** zu kennzeichnen, anstatt sie komplett aus der Datenbank zu löschen. So bleibt die **historische Information** erhalten und es gehen keine **wertvollen Daten** verloren. ✨

#### 📚 Datenbank-Lernnotizen  

##### 🗑️ Löschen in Datenbanken  
Beim Löschen von Daten in einer Datenbank gibt es verschiedene Ansätze:  
- **DELETE**: Entfernt Datensätze aus einer Tabelle, aber die Struktur bleibt erhalten.  
- **TRUNCATE**: Löscht alle Daten einer Tabelle, setzt Auto-Increment-Werte zurück, ist aber schneller als DELETE.  
- **DROP**: Löscht eine gesamte Tabelle inkl. Struktur – Vorsicht, dies ist nicht rückgängig zu machen!  

##### 🔗 Datenintegrität  
Datenintegrität stellt sicher, dass die Daten in einer Datenbank **korrekt, konsistent und zuverlässig** sind.  
Wichtige Mechanismen zur Wahrung der Integrität:  
- **Primärschlüssel** (Eindeutigkeit der Datensätze)  
- **Fremdschlüssel** (Beziehungen zwischen Tabellen)  
- **Constraints** wie `NOT NULL`, `UNIQUE`, `CHECK`  
- **Transaktionen** (z. B. mit `COMMIT` und `ROLLBACK`)  

##### 🔄 FK-Constraint-Options  
Fremdschlüssel (FK) regeln, wie abhängige Daten bei Änderungen behandelt werden:  
- `CASCADE` 🔄 → Änderungen/Löschungen vererben sich  
- `SET NULL` 🚫 → Setzt FK-Wert auf NULL, falls möglich  
- `SET DEFAULT` 🎯 → Setzt FK-Wert auf einen Standardwert  
- `RESTRICT` ❌ → Verhindert Löschung, wenn abhängige Daten existieren  
- `NO ACTION` 🏁 → Wie `RESTRICT`, aber mit verzögerter Prüfung