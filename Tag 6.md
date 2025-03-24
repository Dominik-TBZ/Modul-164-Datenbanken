# Modul 164 – Datenbanken

## Tag 6

### Auftrag Subquerys  
🔗 [GitLab Link zu Subquerys](https://gitlab.com/ch-tbz-it/Stud/m164/-/blob/main/6.Tag/select_Subquery.md)

#### Zusammenfassung Subquery

Ein **Subselect** ist eine SQL-Abfrage innerhalb einer anderen Abfrage. Es gibt zwei Hauptarten von Subselects:

1. **Skalare Unterabfrage** – Gibt genau **eine Zeile mit einer Spalte** zurück.
2. **Nicht-skalare Unterabfrage** – Gibt **mehrere Zeilen oder mehrere Spalten** zurück.

Eine skalare Unterabfrage gibt nur **eine einzige Spalte mit genau einer Zeile** zurück. Sie kann mit Vergleichsoperatoren wie `=`, `>`, `<`, `>=`, `<=` verwendet werden.

### **📌 Beispiel: Günstigere Reiseziele als ein bestimmtes Ticket**

```sql
SELECT city_destination, ticket_price
FROM one_way_ticket
WHERE ticket_price < (
    SELECT ticket_price 
    FROM one_way_ticket
    WHERE city_destination = 'Bariloche' AND city_origin = 'Paris'
)
AND city_origin = 'Paris';
```

![Subquery Bild](https://github.com/user-attachments/assets/a7977c4c-2a36-4985-92bf-2d45d619619d)

#### Aufgabe 1

**1. Welches ist das teuerste Buch in der Datenbank?**
```
SELECT * FROM buecher ORDER BY einkaufspreis DESC LIMIT 1;
```
**2. Welches ist das billigste Buch in der Datenbank?**
```
SELECT * FROM buecher ORDER BY einkaufspreis ASC LIMIT 1;
```
**3. Lassen Sie sich alle Bücher ausgeben, deren Einkaufspreis über dem durchschnittlichen Einkaufspreis aller Bücher in der Datenbank liegt.**
```
SELECT * FROM buecher WHERE einkaufspreis > (SELECT AVG(einkaufspreis) FROM buecher);
```
**4. Lassen Sie sich alle Bücher ausgeben, deren Einkaufspreis über dem durchschnittlichen Einkaufspreis der Thriller liegt.**
```
SELECT * FROM buecher 
WHERE einkaufspreis > (SELECT AVG(einkaufspreis) FROM buecher WHERE genre = 'Thriller');
```
**5. Lassen Sie sich alle Thriller ausgeben, deren Einkaufspreis über dem durchschnittlichen Einkaufspreis der Thriller liegt.**
```
SELECT * FROM buecher 
WHERE genre = 'Thriller' 
AND einkaufspreis > (SELECT AVG(einkaufspreis) FROM buecher WHERE genre = 'Thriller');
```
**6. Lassen Sie sich alle Bücher ausgeben, bei denen der Gewinn überdurchschnittlich ist; bei der Berechnung des Gewinndurchschnitts berücksichtigen Sie NICHT das Buch mit der id 22.**
```
SELECT * FROM buecher 
WHERE (verkaufspreis - einkaufspreis) > 
      (SELECT AVG(verkaufspreis - einkaufspreis) 
       FROM buecher WHERE id <> 22);
```
#### Aufgabe 2

**1. Summe der durchschnittlichen Einkaufspreise der Sparten (ohne Humor und mit Einkaufspreis > 10 Euro)**
```
SELECT SUM(durchschnittspreis) AS summe_durchschnittspreise
FROM (
    SELECT sparten.bezeichnung, AVG(buecher.einkaufspreis) AS durchschnittspreis
    FROM buecher
    JOIN buecher_has_sparten ON buecher.buecher_id = buecher_has_sparten.buecher_buecher_id
    JOIN sparten ON buecher_has_sparten.sparten_sparten_id = sparten.sparten_id
    GROUP BY sparten.bezeichnung
    HAVING sparten.bezeichnung != 'Humor' AND AVG(buecher.einkaufspreis) > 10
) AS subquery;
```
**2. Anzahl der "bekannten Autoren" (mehr als 4 Bücher veröffentlicht)**
```
SELECT COUNT(*) AS anzahl_bekannte_autoren
FROM (
    SELECT autoren.vorname, autoren.nachname, COUNT(*) AS anzahl_buecher
    FROM autoren
    JOIN autoren_has_buecher ON autoren.autoren_id = autoren_has_buecher.autoren_autoren_id
    GROUP BY autoren.autoren_id
    HAVING COUNT(*) > 4
) AS bekannte_autoren;
```
**3. Verlage mit durchschnittlichem Gewinn < 10 Euro und Überprüfung des Chef-Verdachts**
```
SELECT verlage.name, AVG(buecher.verkaufspreis - buecher.einkaufspreis) AS durchschnittsgewinn
FROM verlage
JOIN buecher ON verlage.verlage_id = buecher.verlage_verlage_id
GROUP BY verlage.verlage_id
HAVING durchschnittsgewinn < 10;
```

### Auftrag Daten Importieren  
🔗 [GitLab Link zu Daten Importieren](https://gitlab.com/ch-tbz-it/Stud/m164/-/tree/main/6.Tag?ref_type=heads)

##### Bulkimport: Mysql Data Loader LOAD DATA INFILE
 
##### MySQL/MariaDB: Daten schnell und effizient aus CSV-Dateien importieren
 
Mit den Befehlen `LOAD DATA INFILE` (serverseitig) und `LOAD DATA LOCAL INFILE` (clientseitig) lassen sich CSV-Dateien sehr performant in MySQL-/MariaDB-Tabellen laden. Im Folgenden sind die wichtigsten Hinweise, Einstellungen und ein kurzes Beispiel-Tutorial zusammengefasst.

---

###### 1. Unterschiede: Serverseitiger vs. Clientseitiger Import
 
###### Serverseitig: `LOAD DATA INFILE`
- **Syntax-Beispiel**:
  ```sql
  LOAD DATA INFILE '/Pfad/zur/datei.csv'
  INTO TABLE deine_tabelle
  FIELDS TERMINATED BY ','
  OPTIONALLY ENCLOSED BY '"'
  LINES TERMINATED BY '\n'
  IGNORE 1 LINES
  (spalte1, spalte2, …);
 
 
**Zusammenfassung:**  
- **LOAD DATA INFILE** (ohne LOCAL) lädt eine CSV-Datei vom Server-Dateisystem in MySQL/MariaDB.  
  - Pfadangaben in MySQL/MariaDB **immer** mit Schrägstrichen `/` verwenden.  
  - Sonderzeichen (z.B. Umlaute) in Dateinamen vermeiden (z.B. `Schueler.csv` statt `Schüler.csv`).  
  - Bei Fehlermeldung `Error 13 "Permission denied"` ggf. anderen Ordner nutzen (z.B. `C:/M164/`).  
 
- **LOAD DATA LOCAL INFILE** (mit `LOCAL`) lädt eine CSV-Datei vom **Client-Dateisystem** und sendet sie an den Server.  
 
- **Voraussetzungen/Einstellungen**:  
  1. **Serverseitig**  
     - `SET GLOBAL local_infile = 1;`  
     - Mit `SHOW GLOBAL VARIABLES LIKE 'local_infile';` prüfen.  
     - `SHOW VARIABLES LIKE 'secure_file_priv';` sollte keinen Pfad enthalten (oder `secure_file_priv = ""` in der `my.ini` im Abschnitt `[mysqld]`).  
 
  2. **Clientseitig**  
     - In der `my.ini` (Abschnitt `[mysql]`) `MYSQL_OPT_LOCAL_INFILE=1` setzen.  
     - Falls MySQL Workbench genutzt wird, in den **Connection-Einstellungen** unter **Advanced > Others** `OPT_LOCAL_INFILE=1` hinzufügen.  
     - Bei anderen Clients/Bibliotheken entsprechende Dokumentation beachten.  
 
- **Serverneustart** (z.B. über XAMPP oder Dienstmanager) erforderlich, um Änderungen zu übernehmen.
 
#### Wichtigste Begriffe und Kernelemente des Auftrags
 
##### Zeichensatz (`character_set_database`)
- Standard bei MySQL 8 oft `utf8mb4`.
- Abweichungen (z. B. `latin1`) erfordern eine explizite Angabe via `CHARACTER SET`.
 
##### CSV-Import mit `LOAD DATA INFILE`
- **Trennzeichen (Delimiter)**, z. B. `,` oder `;`.
- **Text-Qualifier**, z. B. `"` (Anführungszeichen).
- **Spaltennamen** in der ersten Zeile (ggf. `IGNORE 1 LINES`).
- **Richtige Angabe** des Zeichenformats (CSV ≠ DB-Standard).
- **Datumsumwandlung** mit `STR_TO_DATE` (z. B. `%d.%m.%Y`).
 
##### Prüfung nach dem Import
- `SELECT COUNT(*)` zum Zählen der Datensätze (z. B. 500 + 100).
- Mögliche Fehler bei **doppelten IDs**, **Zeichensatzproblemen**, **falschen Datumsformaten** oder **fehlenden Spalten**.
 
##### Löschen der Datenbank
- `DROP DATABASE meine_datenbank;`  
  *(Achtung: irreversibel!)*
 
##### Dokumentation im Lernportfolio
- **Skripte** (`CREATE TABLE`, `LOAD DATA`, `DROP DATABASE`)
- **Ergebnisse** (z. B. `SELECT COUNT(*)`, `SHOW WARNINGS;`)
- **Erläuterung** der Fehlerursachen beim erneuten Einspielen von Datensätzen
 
#### Fazit
- **Zeichensatz und CSV-Format** unbedingt vorab prüfen.
- **Passende Tabellenspalten** erstellen (korrekte Datentypen).
- **`LOAD DATA INFILE`**: Delimiter, Enclosure, Zeichensatz und Datumsumwandlung beachten.
- **Zusätzlicher Import** kann scheitern durch:
  - Duplikate (z. B. doppelte Primärschlüssel),
  - Falsches Datumsformat,
  - Zeichensatzkonflikte.
- **Datenbank aufräumen** (z. B. `DROP DATABASE …`) nach abgeschlossener Übung.

### Auftrag Load Date INFILE
🔗 [GitLab Link Load Date INFILE](https://gitlab.com/ch-tbz-it/Stud/m164/-/tree/main/6.Tag?ref_type=heads)

#### Zusammenfassung

- **CSV-Import mit `LOAD DATA LOCAL INFILE`**: Zeichensatz, Delimiter und Zeilenende angeben; Kopfzeilen mit `IGNORE X ROWS` überspringen.
- **Variablen & Datumsformat**: Via `@var` einlesen, anschließend mit `STR_TO_DATE(@var, '%d.%m.%Y')` in Spalten speichern.
- **Spalten überspringen**: Unnötige CSV-Spalten als Dummy (`@Skip`) definieren; fehlende Felder per `SET` (z. B. `FS = 1`) ergänzen.
- **Constraints/Warnungen**: Bei Fremdschlüsseln ggf. `SET FOREIGN_KEY_CHECKS=0;` verwenden; `SHOW WARNINGS;` nach dem Import prüfen.
- **Normalisierung**: Daten aus einer Zwischentabelle mittels `INSERT ... SELECT ...` in mehrere Tabellen verteilen.

#### 1. **Leere Felder und Datumsformat anpassen**

```sql
SET FOREIGN_KEY_CHECKS=0;
Truncate tbl_Beispiel;

LOAD DATA LOCAL INFILE 'C:/M164/bsp1.csv'
REPLACE
INTO TABLE tbl_Beispiel
CHARACTER SET utf8mb4
FIELDS TERMINATED BY ','
LINES TERMINATED BY '\r\n'
IGNORE 1 ROWS
(ID_Beispiel, Name, @GD, Zahl, FS)
SET Geb_Datum = STR_TO_DATE(@GD, '%d.%m.%Y');

SELECT * FROM tbl_Beispiel;
```

In dieser Aufgabe wird das Datumsformat in der CSV-Datei mit der Funktion STR_TO_DATE() angepasst. Dies stellt sicher, dass das Datum im richtigen Format für das Geb_Datum-Feld gespeichert wird.

#### 2. **Spaltenreihenfolge ändern**

```sql
Kopieren
Truncate tbl_Beispiel; -- Leert Tabelle

LOAD DATA LOCAL INFILE 'C:/M164/bsp1.csv'
REPLACE
INTO TABLE tbl_Beispiel
CHARACTER SET utf8mb4
FIELDS TERMINATED BY ','
LINES TERMINATED BY '\r\n'
IGNORE 1 ROWS
(ID_Beispiel, @GD, Name, Zahl, FS)
SET Geb_Datum = STR_TO_DATE(@GD, '%d.%m.%Y');

SELECT * FROM tbl_Beispiel;
```

Die Reihenfolge der Spalten in der CSV-Datei stimmt nicht mit der Reihenfolge der Attribute in der Tabelle überein. Wir tauschen sie mit Hilfe der Variablen @GD und passen das Datum für das Geb_Datum-Feld an.

### 3. **Spalten auslassen**

```sql
LOAD DATA LOCAL INFILE 'C:/M164/bsp3.csv'
REPLACE
INTO TABLE tbl_Beispiel
CHARACTER SET utf8mb4
FIELDS TERMINATED BY ','
LINES TERMINATED BY '\r\n'
IGNORE 1 ROWS
(ID_Beispiel, Name, @GD, Zahl, FS, @SkipPW)
SET Geb_Datum = STR_TO_DATE(@GD, '%d.%m.%Y');

SELECT * FROM tbl_Beispiel;
```

In diesem Beispiel überspringen wir bestimmte Spalten der CSV-Datei, indem wir Dummy-Variablen wie @SkipPW verwenden.

### 4. **Attribut hinzufügen**

```sql
Kopieren
LOAD DATA LOCAL INFILE 'C:/M164/bsp4.csv'
REPLACE
INTO TABLE tbl_Beispiel
CHARACTER SET utf8mb4
FIELDS TERMINATED BY ','
LINES TERMINATED BY '\r\n'
IGNORE 1 ROWS
(Name, Geb_Datum, Zahl)
SET FS = 1; -- FS wird bei jedem DS auf 1 gesetzt

SELECT * FROM tbl_Beispiel;
```

Hier wird ein fehlendes Attribut FS hinzugefügt, indem es für jede Zeile auf 1 gesetzt wird.

### 5. **Werte ändern**

```sql
LOAD DATA LOCAL INFILE 'C:/M164/bsp5.csv'
REPLACE
INTO TABLE tbl_Beispiel
CHARACTER SET utf8mb4
FIELDS TERMINATED BY ','
LINES TERMINATED BY '\r\n'
IGNORE 1 ROWS
(ID_Beispiel, @GD, Name, Zahl, @Ort)
SET Geb_Datum = STR_TO_DATE(@GD, '%d.%m.%Y'),
FS = CASE
    WHEN @Ort = 'Zuerich' THEN 1
    WHEN @Ort = 'Basel' THEN 2
    WHEN @Ort = 'St.Gallen' THEN 3
    ELSE NULL
END;

SELECT * FROM tbl_Beispiel;
```

### Auftrag Datenbasis DB_Adressen  
🔗 [GitLab Link zu Datenbasis DB_Adressen](https://gitlab.com/ch-tbz-it/Stud/m164/-/tree/main/6.Tag?ref_type=heads)

#### **Kurzzusammenfassung / Wichtige Stichpunkte**
 
#### Ziel
- **DB `db_adressen`** erstellen  
- **CSV-Datei (`adressen.csv`)** mit 10 Datensätzen importieren  
- Daten in **3. Normalform** aufteilen (Person, Straße, Ort)
 
#### Ablauf
1. **Temporäre Tabelle `tbl_Adr`** anlegen  
2. **CSV** mit `LOAD DATA LOCAL INFILE` importieren (inkl. Prüfung Sonderzeichen/Umlaute)  
3. **Normalisieren**: Tabellen `tbl_Person`, `tbl_Str`, `tbl_Ort` erstellen  
4. Daten per `INSERT ... SELECT` aus `tbl_Adr` in die normalisierten Tabellen übertragen (FK-Beziehungen beachten)  
5. Überprüfen per `SELECT ... JOIN ... JOIN ...`
 
#### Direktimport
- **Alternative**: CSV direkt in die drei Normalform-Tabellen laden  
- Entsprechende Skip/Set-Attribute im `LOAD DATA`-Befehl beachten
 
#### Redundanz
- Finden und Bereinigen doppelter Orts-Einträge mittels `GROUP BY`, `TEMPORARY TABLE`, `UPDATE` und `DELETE`
 
#### Wichtige SQL-Stichworte
- `CREATE DATABASE db_adressen;`
- `LOAD DATA LOCAL INFILE ...` (Zeichensatz, Delimiter usw. prüfen)
- `INSERT INTO ... SELECT ...`
- `JOIN` (Prüfung der importierten Daten)
- `TEMPORARY TABLE` (zur Zusammenführung doppelter Datensätze)
- `DELETE` (zur Entfernung redundanter Einträge)

---

#### Schritt 1: Temporäre Tabelle erstellen

Erstellt eine Zwischentabelle, in die die Daten aus der CSV-Datei geladen werden:
```
TABLE db_adressen.tbl_Adr (
    ID INT AUTO_INCREMENT PRIMARY KEY,
    Name VARCHAR(255),
    Age INT,
    Street VARCHAR(255),
    Number VARCHAR(10),
    City VARCHAR(255),
    PostalCode VARCHAR(10),
    CONSTRAINT nn_AdrID DEFAULT 0 FOR ColID
);
```

#### Schritt 2: Daten importieren

Lädt die CSV-Daten in die temporäre Tabelle:
```
DATA LOCAL INFILE 'path/to/csv.csv' INTO TABLE db_adressen.tbl_Adr;
```

#### Schritt 3: Tabellen normalisieren

Neue Tabellen anlegen für Person, Straße und Ort:
```
CREATE TABLE db_adressen.tbl_Person (
    ID INT,
    Name VARCHAR(255),
    Age INT,
    PRIMARY KEY (ID)
);
```
```
CREATE TABLE db_adressen.tbl_Str (
    ID INT,
    Street VARCHAR(255),
    Number VARCHAR(10),
    FOREIGN KEY (ID) REFERENCES tbl_Adr(ID),
    PRIMARY KEY (ID)
);
```
```
CREATE TABLE db_adressen.tbl_Ort (
    ID INT,
    City VARCHAR(255),
    PostalCode VARCHAR(10),
    FOREIGN KEY (ID) REFERENCES tbl_Adr(ID),
    PRIMARY KEY (ID)
);
```

#### Schritt 4: Daten in die neuen Tabellen einfügen
```
INSERT INTO db_adressen.tbl_PersonSELECT ID, Name, Age FROM db_adressen.tbl_Adr;
```
```
INSERT INTO db_adressen.tbl_StrSELECT ID, Street, Number FROM db_adressen.tbl_Adr;
```
```
INSERT INTO db_adressen.tbl_OrtSELECT ID, City, PostalCode FROM db_adressen.tbl_Adr;
```

#### Schritt 5: Daten prüfen

Zeigt an, ob alle Daten korrekt verteilt wurden:
```
SELECT p.ID, p.Name, p.Age, s.Street, s.Number, o.City, o.PostalCodeFROM db_adressen.tbl_Person pJOIN db_adressen.tbl_Str s ON p.ID = s.ID JOIN db_adressen.tbl_Ort o ON p.ID = o.ID;
```

#### Schritt 6: Daten direkt laden, ohne temporäre Tabelle

Löscht den Inhalt aller drei Tabellen und lädt erneut aus der CSV:
```
CREATE TABLE db_adressen.tbl_Person, db_adressen.tbl_Str, db_adressen.tbl_Ort;LOAD DATA LOCAL INFILE 'path/to/csv.csv' INTO TABLE db_adressen.tbl_Person;LOAD DATA LOCAL INFILE 'path/to/csv.csv' INTO TABLE db_adressen.tbl_Str;
```
```
INFILE 'path/to/csv.csv' INTO TABLE db_adressen.tbl_Ort;
```
Entfernen von Duplikaten (Beispiel für doppelte Ortsnamen)

#### Schritt 1: Doppelte Einträge für Ortsnamen ermitteln
```
SELECT Ortsbezeichnung, COUNT(*) AS AnzahlFROM tbl_OrtGROUP BY OrtsbezeichnungHAVING COUNT(*) > 1;
```
#### Schritt 2: Temporäre Tabelle mit eindeutigen IDs erstellen
```
CREATE TEMPORARY TABLE temp_Orte (
    Ort_ID INT,
    Ortsbezeichnung VARCHAR(255)
);
INSERT INTO temp_OrteSELECT MIN(Ort_ID), OrtsbezeichnungFROM tbl_OrtGROUP BY Ortsbezeichnung;
```

#### Schritt 3: Verknüpfung von Straßen mit eindeutigem Ort
```
UPDATE tbl_StrSET Ort_FK = (
    SELECT t.Ort_ID
    FROM temp_Orte t
    WHERE t.Ortsbezeichnung = tbl_Ort.Ortsbezeichnung
);
```
#### Schritt 4: Redundante Orts-Einträge löschen
```
DELETE FROM tbl_Ort WHERE Ort_ID NOT IN (SELECT t.Ort_ID FROM temp_Orte t);
```
**Erklärung:**
Die erste Abfrage zeigt Orte, die mehrfach eingetragen sind.
In der temporären Tabelle wird jeder Ortsbezeichnung die kleinste (erste) Ort_ID zugewiesen.
Mit dem UPDATE wird tbl_Str an diese eindeutigen IDs angepasst, damit jede Straße der richtigen Stadt zugeordnet bleibt.
Anschließend werden alle doppelten Einträge gelöscht, sodass jeder Ortsname nur noch einmal existiert.

### Auftrag Checkpoint  
🔗 [GitLab Link zum Checkpoint](https://gitlab.com/ch-tbz-it/Stud/m164/-/tree/main/6.Tag?ref_type=heads)
 
#### **1. Erklärung des Einsatzes von Subqueries**

Subqueries (Unterabfragen) ermöglichen es, eine Abfrage innerhalb einer anderen Abfrage auszuführen, um komplexe Datenabfragen zu realisieren oder Zwischenergebnisse zu berechnen, die als Grundlage für die äußere Abfrage dienen.

#### **2. Unterschied zwischen skalaren und nicht-skalaren Subqueries**

- **Skalare Subquery:** Gibt genau einen einzelnen Wert zurück (eine Zeile, eine Spalte). Sie wird verwendet, wenn ein einzelner Wert für den Vergleich oder die Berechnung benötigt wird.  
- **Nicht-skalare Subquery:** Gibt mehrere Werte zurück (mehrere Zeilen oder mehrere Spalten). Diese Art von Subquery wird verwendet, wenn mehrere Werte für den Vergleich oder die Bedingung benötigt werden.

#### **3. Gefahren bei der Verwendung von Subselects**

- **Leistungsprobleme:** Subqueries können die Datenbankleistung beeinträchtigen, insbesondere bei großen Datensätzen oder häufig ausgeführten Abfragen, da die Datenbank zusätzliche Verarbeitungsschritte ausführen muss.  
- **Komplexität:** Subqueries können die Lesbarkeit und Wartbarkeit der SQL-Abfragen erschweren, was die Fehlersuche und Anpassungen komplizierter macht.  
- **Fehleranfälligkeit:** Fehler können auftreten, wenn die Subquery unerwartet mehrere Werte zurückgibt, während die äußere Abfrage nur einen einzelnen Wert erwartet.

#### 4. Bedeutung von `IGNORE 1 LINES` in `LOAD DATA INFILE`

Der Zusatz `IGNORE 1 LINES` weist MySQL an, die erste Zeile der CSV-Datei zu überspringen. Dies ist nützlich, wenn die Datei eine Kopfzeile mit Spaltennamen enthält, die nicht importiert werden soll.

#### **5. Folgen bei falschem Zeilenende bei CSV-Import**

Wenn eine Windows-CSV-Datei mit Zeilenenden `\r\n` vorliegt, der Import aber mit `LINES TERMINATED BY '\n'` konfiguriert ist, bleibt das Carriage Return (`\r`) am Ende der letzten Spalte erhalten. Dies kann zu Problemen bei der Datenverarbeitung führen, da zusätzliche Zeichen die Genauigkeit von Vergleichen oder Berechnungen beeinflussen können.

#### **6. Einstellungen für den CSV-Import vom Client zum Server**

Um eine CSV-Datei vom Client zum Server zu importieren, muss der Benutzer die Berechtigung `FILE` besitzen. Außerdem muss die Servervariable `secure_file_priv` überprüft werden, um festzustellen, ob der Import aus dem gewünschten Verzeichnis erlaubt ist. Die CSV-Datei muss sich im erlaubten Verzeichnis befinden, damit der Import erfolgreich durchgeführt werden kann.

#### **7. Importieren von Spalten in anderer Reihenfolge**

Um Spalten in einer anderen Reihenfolge zu importieren, muss die Spaltenliste in der `LOAD DATA INFILE`-Anweisung explizit angegeben werden. Dies stellt sicher, dass die Spalten gemäß der angegebenen Reihenfolge in die Datenbanktabelle importiert werden, unabhängig von der Reihenfolge der Spalten in der CSV-Datei.