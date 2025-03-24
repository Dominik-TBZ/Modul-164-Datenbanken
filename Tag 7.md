# Modul 164 – Datenbanken

## Tag 7

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

#### Schritt 4: Daten in die neuen Tabellen kopieren
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
DELETE FROM tbl_OrtWHERE Ort_ID NOT IN (SELECT t.Ort_ID FROM temp_Orte t);
```
**Erklärung:**
Die erste Abfrage zeigt Orte, die mehrfach eingetragen sind.
In der temporären Tabelle wird jeder Ortsbezeichnung die kleinste (erste) Ort_ID zugewiesen.
Mit dem UPDATE wird tbl_Str an diese eindeutigen IDs angepasst, damit jede Straße der richtigen Stadt zugeordnet bleibt.
Anschließend werden alle doppelten Einträge gelöscht, sodass jeder Ortsname nur noch einmal existiert.
 