# Modul 164 – Datenbanken

## Tag 5

### Auftrag Löschen  
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
 
| **Regel**                          | **Beschreibung**                                                                                                                                                   |
|------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **ON DELETE NO ACTION (RESTRICT)** | Ein `DELETE` in der Primärtabelle ist nur möglich, wenn keine verknüpften Detail-Datensätze existieren. (Standard, falls nichts anderes angegeben)               |
| **ON DELETE CASCADE**              | Das Löschen in der Primärtabelle führt zum automatischen Löschen aller zugehörigen Detail-Datensätze. ⚠️ (Vorsicht: Daten können unbeabsichtigt komplett verschwinden!) |
| **ON DELETE SET NULL (DEFAULT)**   | Beim Löschen in der Primärtabelle wird der Fremdschlüssel in der Detailtabelle auf `NULL` oder einen Default-Wert gesetzt. (Nur möglich, wenn `NULL` erlaubt ist)   |
 
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

### Referentielle Datenintegrität
🔗 [GitLab Link zum Löschen von Daten](https://gitlab.com/ch-tbz-it/Stud/m164/-/blob/main/5.Tag/Referentielle_Datenintegritaet.md)

#### Aufgabe 1

##### ❌ Warum können in professionellen Datenbanken nicht einfach Daten gelöscht werden?  

In professionellen Datenbanken ist das **direkte Löschen von Daten eingeschränkt**, weil:  

1. **Referentielle Integrität 🔗** – Ein Datensatz kann mit anderen Tabellen verknüpft sein. Unkontrolliertes Löschen könnte zu **inkonsistenten oder fehlerhaften Daten** führen.  
2. **Datenhistorie & Nachvollziehbarkeit 📜** – Viele Systeme müssen aus rechtlichen oder geschäftlichen Gründen **Änderungen nachvollziehbar** speichern (z. B. durch Logging oder Archivierung).  
3. **Verhinderung von Datenverlust 🚨** – Unbedachtes Löschen kann **kritische Geschäftsdaten** vernichten, was fatale Folgen haben kann.  
4. **Leistung & Performance ⚡** – Statt Daten zu löschen, werden sie oft nur als **"inaktiv" markiert** (Soft-Delete), um **Abfragen effizienter** zu gestalten.  

##### 🛡️ Wer stellt die referentielle Integrität sicher?  

Die **referentielle Integrität** wird durch mehrere Mechanismen gewährleistet:  

- **Datenbank-Constraints ⚖️** – Fremdschlüssel (`FOREIGN KEY`) mit Optionen wie `ON DELETE RESTRICT` oder `CASCADE`.  
- **Triggers 🔥** – Datenbank-Trigger können verhindern, dass Daten **unzulässig gelöscht oder geändert** werden.  
- **Transaktionen 🔄** – `COMMIT` und `ROLLBACK` stellen sicher, dass Daten nur **konsistent** gespeichert oder zurückgesetzt werden.  
- **Anwendungslogik 🏗️** – Software-Entwickler implementieren oft zusätzliche Sicherheitsprüfungen in der Anwendungsschicht.  

#### Aufgabe 2 -  🛠️ Fehlerkorrektur in der Datenbank  

##### ❌ Versuch, „Basel“ aus `tbl_orte` zu löschen  

Beim Versuch, den Eintrag `4000 Basel` aus der Tabelle `tbl_orte` zu löschen, tritt ein Fehler auf:  

```sql
DELETE FROM tbl_orte WHERE Ortsbezeichnung = 'Basel';
```

##### 🧐 Beobachtung  
Die Datenbank gibt eine Fehlermeldung aus, weil die Ortschaft `Basel` (`ID_Ort = 5`) in der Tabelle `tbl_stationen` über eine **Fremdschlüssel-Referenz** (`FS_ID_Ort`) mit bestehenden Fahrten verknüpft ist.  
**Referentielle Integrität** verhindert, dass ein Datensatz gelöscht wird, solange es abhängige Einträge gibt.  

##### ✅ Korrektur: Richtiges Vorgehen  

Damit der falsche Ort ersetzt und gelöscht werden kann, müssen folgende Schritte durchgeführt werden:  

###### 1️⃣ Den neuen Ort "Bern" hinzufügen  
Zuerst wird der neue, korrekte Ort `3000 Bern` in die Tabelle `tbl_orte` eingefügt:  

```sql
INSERT INTO tbl_orte (PLZ, Ortsbezeichnung) VALUES ('3000', 'Bern');
```

###### 2️⃣ Die `ID_Ort` von „Bern“ herausfinden  
Nach dem Einfügen müssen wir die automatisch vergebene `ID_Ort` für `Bern` herausfinden:  

```sql
SELECT ID_Ort FROM tbl_orte WHERE Ortsbezeichnung = 'Bern';
```

Angenommen, `Bern` hat nun die `ID_Ort = 6`, verwenden wir diese im nächsten Schritt.  

###### 3️⃣ Fahrten aktualisieren, die aktuell auf „Basel“ verweisen  
Die bestehenden Datensätze in `tbl_stationen`, die auf `Basel` (`ID_Ort = 5`) verweisen, müssen auf die neue `ID_Ort` von `Bern` (`ID_Ort = 6`) geändert werden:  

```sql
UPDATE tbl_stationen 
SET FS_ID_Ort = 6 
WHERE FS_ID_Ort = 5;
```

###### 4️⃣ Den alten Eintrag „Basel“ löschen  
Nun ist `Basel` nicht mehr referenziert und kann sicher entfernt werden:  

```sql
DELETE FROM tbl_orte WHERE ID_Ort = 5;
```

##### 🎯 Fazit  
Dank dieser schrittweisen Anpassung bleibt die referentielle Integrität der Datenbank erhalten, und der Fehler in den Ortsdaten wurde erfolgreich korrigiert. 🚀

### SELECT Alias
🔗 [GitLab Link zu SELECT Alias](https://gitlab.com/ch-tbz-it/Stud/m164/-/blob/main/5.Tag/SELECT_ALIAS.md)

#### Statement 1: Ausgabe von kunde_id, Name und Postleitzahl für Kunden mit PLZ > 80000

SELECT kunde_id, name, ort_postleitzahl 
FROM kunde AS kundenliste 
WHERE kundenliste.ort_postleitzahl > 80000;

#### Statement 2: Ausgabe von Ort-Name und Kunden-Name für Orte, deren Name auf 'n' endet

SELECT o.name, k.name 
FROM ort AS o
JOIN kunde AS k 
    ON o.postleitzahl = k.ort_postleitzahl
WHERE o.name LIKE '%n';

#### Statement 3:

SELECT hrgs.kunde_id, hrgs.name, prfz.name 
FROM kunde AS hrgs 
INNER JOIN ort AS prfz 
    ON prfz.postleitzahl = hrgs.ort_postleitzahl 
ORDER BY hrgs.kunde_id;

#### Statement 4:

SELECT k.name, o.postleitzahl, o.name 
FROM kunde AS k, ort AS o 
WHERE k.name LIKE '%a%' 
  AND o.name LIKE '%u%' 
  AND k.ort_postleitzahl = o.postleitzahl;

### Aggregatsfunktionen
🔗 [GitLab Link zu den Aggregatsfunktionen](https://gitlab.com/ch-tbz-it/Stud/m164/-/blob/main/5.Tag/Aggregatsfunktionen.md)

#### 1. Welches ist das niedrigste/höchste Gehalt eines Lehrers?

SELECT MIN(Gehalt) AS niedrigstes_Gehalt, MAX(Gehalt) AS höchstes_Gehalt FROM Lehrer;
 
#### 2. Niedrigstes Gehalt eines Mathelehrers:
 
SELECT MIN(Gehalt) AS niedrigstes_Gehalt FROM Lehrer WHERE Fach = 'Mathematik';
 
#### 3. Bester Notendurchschnitt in Deutsch/Mathe:
 
SELECT MIN((Deutsch + Mathe) / 2) AS bester_Notendurchschnitt FROM Noten;
 
#### 4. Höchste/niedrigste Einwohnerzahl eines Ortes:
 
SELECT MAX(Einwohnerzahl) AS höchste_Einwohnerzahl, MIN(Einwohnerzahl) AS niedrigste_Einwohnerzahl FROM Orte;
 
#### 5. Differenz zwischen größtem und kleinstem Ort:
 
SELECT (MAX(Einwohnerzahl) - MIN(Einwohnerzahl)) AS Differenz FROM Orte;
 
#### 6. Anzahl der Schüler in der Datenbank:
 
SELECT COUNT(*) AS Anzahl_Schueler FROM Schueler;
 
#### 7. Anzahl der Schüler mit einem Smartphone:
 
SELECT COUNT(*) AS Anzahl_mit_Smartphone FROM Schueler WHERE Smartphone IS NOT NULL;
 
#### 8. Anzahl der Schüler mit einem Samsung- oder HTC-Smartphone:
 
SELECT COUNT(*) AS Anzahl_Samsung_HTC FROM Schueler WHERE Smartphone IN ('Samsung', 'HTC');
 
#### 9. Anzahl der Schüler in Waldkirch:
 
SELECT COUNT(*) AS Anzahl_Waldkirch FROM Schueler WHERE Wohnort = 'Waldkirch';
 
#### 10. Schüler bei Herrn Bohnert, die in Emmendingen wohnen:
 
SELECT COUNT(*) AS Anzahl_Schueler_Bohnert_Emmendingen  FROM Schueler  WHERE Lehrer_ID = (SELECT Lehrer_ID FROM Lehrer WHERE Name = 'Bohnert')  AND Wohnort = 'Emmendingen';
 
#### 11. Anzahl der Schüler, die Frau Zelawat unterrichtet:
 
SELECT COUNT(*) AS Anzahl_Schueler_Zelawat  FROM Schueler  WHERE Lehrer_ID = (SELECT Lehrer_ID FROM Lehrer WHERE Name = 'Zelawat');
 
#### 12. Anzahl der Schüler russischer Nationalität bei Frau Zelawat:
 
SELECT COUNT(*) AS Anzahl_russischer_Schueler_Zelawat  FROM Schueler  WHERE Lehrer_ID = (SELECT Lehrer_ID FROM Lehrer WHERE Name = 'Zelawat')  AND Nationalität = 'Russisch';
 
#### 13. Lehrer mit dem höchsten Gehalt:
 
SELECT Name, Gehalt FROM Lehrer WHERE Gehalt = (SELECT MAX(Gehalt) FROM Lehrer);