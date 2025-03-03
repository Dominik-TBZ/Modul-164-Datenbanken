# Modul 164 – Datenbanken

## Tag 3

### Auftrag Datentypen  
🔗 [GitLab Link zu den Datentypen](https://gitlab.com/ch-tbz-it/Stud/m164/-/tree/main/3.Tag)

#### Tabelle zu den Datentypen 

| Datentyp                           | MariaDB (MySQL) Typ       | Beispiel                  | Bemerkung / Einstellungen                                  |
|------------------------------------|--------------------------|--------------------------|------------------------------------------------------------|
| Ganze Zahlen                      | INT, TINYINT, SMALLINT, MEDIUMINT, BIGINT | 42 | Unterschiedliche Speichergrößen für verschiedene Wertebereiche |
| Natürliche Zahlen                 | UNSIGNED INT, UNSIGNED SMALLINT, etc. | 10 | Nur positive Werte erlaubt |
| Festkommazahlen (Dezimalzahlen)    | DECIMAL(M,D), NUMERIC(M,D) | DECIMAL(6,2) → 1234.56 | M = Gesamte Anzahl Stellen, D = Nachkommastellen |
| Aufzählungstypen                   | ENUM, SET                | ENUM('A', 'B', 'C') | ENUM für einzelne Auswahlwerte, SET für mehrere wählbare Werte |
| Boolean (logische Werte)           | BOOLEAN, TINYINT(1)      | TRUE / FALSE, 1 / 0 | Intern als TINYINT(1) gespeichert |
| Zeichen (einzelnes Zeichen)        | CHAR(1)                  | 'A' | Feste Länge, genau ein Zeichen |
| Gleitkommazahlen                   | FLOAT, DOUBLE            | 3.14159 | FLOAT für weniger Präzision, DOUBLE für höhere Präzision |
| Zeichenkette fester Länge          | CHAR(N)                  | CHAR(10) → 'Hallo     ' | Feste Länge, mit Leerzeichen aufgefüllt |
| Zeichenkette variabler Länge       | VARCHAR(N)               | VARCHAR(50) → 'Hallo' | Variable Länge, speichert nur die tatsächliche Zeichenanzahl |
| Datum und/oder Zeit                | DATE, TIME, DATETIME, YEAR | DATE → '2024-03-03' | DATE für Datum, TIME für Zeit, DATETIME für beides |
| Zeitstempel                        | TIMESTAMP                | TIMESTAMP → '2025-03-03 12:00:00' | Automatische Aktualisierung möglich (z. B. `DEFAULT CURRENT_TIMESTAMP`) |
| Binäre Datenobjekte variabler Länge (z.B. Bild) | BLOB, LONGBLOB | BLOB → Binärdaten | Speicherung von Bildern, Dateien oder anderen binären Objekten |
| Verbund                            | JSON                     | JSON → '{"name": "Max"}' | Speicherung strukturierter JSON-Daten |

#### Aufgabe bester Datentyp

##### 1. Welche Datentypen würden Sie für die folgenden Werte jeweils vergeben?  
Wählen Sie den Datentyp möglichst speicherplatzoptimiert.  

1. **53** → `INTEGER`  
2. **983773** → `INTEGER`  
3. **Fritz Schmitt** → `VARCHAR(50)`  
4. **fritz_schmitt** → `VARCHAR(30)`  
5. **müller13** → `VARCHAR(255)`  
6. **07661/66456-2** → `VARCHAR(20)`  
7. **Kirchweg** → `VARCHAR(50)`  
8. **Kirchweg 13** → `VARCHAR(50)`  
9. **Am Anfang, als noch alles dunkel war, wussten wir nicht, was geschehen würde, aber wir hatten zum Glück mächtige Freunde, deren Anliegen es nicht sein konnte, uns zu hintergehen. Deshalb war das Glück uns hold, und schon nach wenigen Tagen konnten wir voller Freude die erste Glühbirne in unserem Wohnzimmer in Betrieb nehmen.** → `TEXT`  
10. **2.88499399** → `FLOAT(10,8)`  
11. **19.03.2009** → `DATE`  
12. **13.02.1983 22:07:12** → `DATETIME`  
13. **64002** → `INTEGER`  

##### 2. Welche Datentypen würden Sie für die folgenden Felder jeweils vergeben?  
Wählen Sie den Datentyp möglichst speicherplatzoptimiert.  

1. **Id** → `INTEGER`  
2. **Vorname** → `VARCHAR()`  
3. **Nachname** → `VARCHAR()`  
4. **Strasse** → `VARCHAR()`  
5. **Hausnummer** → `INTEGER`  
6. **Postleitzahl** → `INTEGER`  
7. **Telefonnummer** → `INTEGER`  
8. **Registrierungsdatum** → `DATE`  
9. **Bestellnummer** → `INT`  
10. **kommentar_an_versender** → `TEXT`  
11. **preis** → `FLOAT(10,3)`  
12. **enthaltene_mehrwertsteuer** → `FLOAT(2,4)`  
13. **roman_vollstaendiger_inhalt** → `TEXT`  

##### 3. Welche MySQL-Datentypen sind in Aufgabe 1 und 2 noch nicht vorgekommen?  
Listen Sie fünf Beispiele auf und schreiben Sie die korrekte Datentypbezeichnung für MySQL.  

- `ENUM`  
- `BOOLEAN`  
- `BLOB`  
- `TIME`  
- `CHAR`  

### Auftrag SQL - Befehle  
🔗 [GitLab Link zu den SQL Befehlen](https://gitlab.com/ch-tbz-it/Stud/m164/-/tree/main/3.Tag)

#### Insert
 
##### **Kurzform:**
 
```sql
INSERT INTO kunden (vorname, nachname, wohnort, land_id) VALUES
('Heinrich', 'Schmitt', 'Zürich', 2),
('Sabine', 'Müller', 'Bern', 2),
('Markus', 'Mustermann', 'Wien', 1);
```
 
##### **Langform:**
 
```sql
INSERT INTO kunden (vorname, nachname, wohnort, land_id) VALUES ('Herr', 'Maier', NULL, NULL);
INSERT INTO kunden (vorname, nachname, wohnort, land_id) VALUES ('Herr', 'Bulgur', 'Sirnach', NULL);
INSERT INTO kunden (vorname, nachname, wohnort, land_id) VALUES ('Maria', 'Manta', NULL, NULL);
```
 
##### Korrektur der fehlerhaften SQL-Befehle
 
- **A**
**Fehler:** Die Tabelle "kunden" wurde nicht spezifiziert.
 
```sql
INSERT INTO kunden (nachname, wohnort, land_id) VALUES ('Fesenkampp', 'Duisburg', 3);
```
 
- **B**
**Fehler:** Falsche Syntax mit Anführungszeichen um "vorname".
 
```sql
INSERT INTO kunden (vorname) VALUES ('Herbert');
```
 
- **C**
**Fehler:** Reihenfolge der Spalten stimmt nicht mit den Werten überein, und es gibt eine zusätzliche Spalte "Deutschland".
 
```sql
INSERT INTO kunden (nachname, vorname, wohnort, land_id) VALUES ('Schülter', 'Albert', 'Duisburg', 3);
```
 
- **D**
**Fehler:** Ein leerer String als "kunde_id" (Primärschlüssel) ist nicht erlaubt.
 
```sql
INSERT INTO kunden (vorname, nachname, land_id, wohnort) VALUES ('Brunhild', 'Sulcher', 1, 'Süderstade');
```
 
- **E**
**Fehler:** Falsche Reihenfolge der Werte und fehlende Spaltenangabe.
 
```sql
INSERT INTO kunden (vorname, nachname, land_id, wohnort) VALUES ('Jochen', 'Schmied', 2, 'Solingen');
```
 
- **F**
**Fehler:** Ein leerer String als Primärschlüsselwert ist nicht erlaubt.
 
```sql
INSERT INTO kunden (nachname, land_id, wohnort) VALUES ('Doppelbrecher', 2, '');
```
 
- **G**
**Fehler:** Fehlende Spalte "vorname" in der Spaltenliste.
 
```sql
INSERT INTO kunden (vorname, nachname, wohnort, land_id) VALUES ('Christoph', 'Fesenkampp', 'Duisburg', 3);
```
 
- **H**
**Fehler:** Doppelte Anweisung, identisch mit B.
 
```sql
INSERT INTO kunden (vorname) VALUES ('Herbert');
```
 
- **I**
**Fehler:** Fehlende Anführungszeichen um Werte.
 
```sql
INSERT INTO kunden (nachname, vorname, wohnort, land_id) VALUES ('Schulter', 'Albert', 'Duisburg', 1);
```
 
- **J**
**Fehler:** "VALUE" anstelle von "VALUES" benutzt und leere Zeichenkette für Primärschlüssel.
 
```sql
INSERT INTO kunden (vorname, nachname, land_id, wohnort) VALUES ('Brunhild', 'Sulcher', 1, 'Süderstade');
```
 
- **K**
**Fehler:** "VALUE" anstelle von "VALUES" und fehlende Anführungszeichen um "Solingen".
 
```sql
INSERT INTO kunden (vorname, nachname, land_id, wohnort) VALUES ('Jochen', 'Schmied', 2, 'Solingen');
```
 
#### SQL Aufgabenlösung Update, delete, alter und drop
 
##### 1. Vervollständigung des Regisseurs
 
```sql
UPDATE regisseur
SET vorname = 'Etan'
WHERE nachname = 'Cohen' AND vorname IS NULL;
```
 
##### 2. Korrektur der Filmlänge
 
```sql
UPDATE filme
SET dauer = 120
WHERE titel = 'Angst' AND dauer <> 120;
```
 
##### 3. Umbenennung der Tabelle von DVD zu Bluray
 
```sql
RENAME TABLE dvd_sammlung TO bluray_sammlung;
```
 
##### 4. Hinzufügen einer neuen Spalte "Preis"
 
```sql
ALTER TABLE filme
ADD COLUMN preis DECIMAL(5,2) DEFAULT NULL;
```
 
##### 5. Entfernung des Films "Angriff auf Rom"
 
```sql
DELETE FROM filme
WHERE titel = 'Angriff auf Rom' AND regisseur = 'Steven Burghofer';
```
 
##### 6. Umbenennung der Spalte "filme" zu "kinoFilme"
 
```sql
ALTER TABLE sammlung
CHANGE COLUMN filme kinoFilme VARCHAR(255);
```
 
##### 7. Löschen der Spalte "Nummer"
 
```sql
ALTER TABLE filme
DROP COLUMN IF EXISTS nummer;
```
 
##### 8. Löschen der gesamten Filmverleih-Tabelle
 
```sql
DROP TABLE IF EXISTS filmverleih;
```
 
#### SQL-SELECT Aufgaben
 
##### Aufgabe a
 
```sql
SELECT * FROM dvd_sammlung;
```
 
##### Aufgabe b
 
```sql
SELECT filmtitel, filmnummer FROM filme;
```
 
##### Aufgabe c
 
```sql
SELECT filmtitel, regisseur FROM filme;
```
 
##### Aufgabe d
 
```sql
SELECT filmtitel FROM filme WHERE regisseur = 'Quentin Tarantino';
```
 
##### Aufgabe e
 
```sql
SELECT filmtitel FROM filme WHERE regisseur = 'Steven Spielberg';
```
 
##### Aufgabe f
 
```sql
SELECT filmtitel FROM filme WHERE regisseur LIKE 'Steven%';
```
 
##### Aufgabe g
 
```sql
SELECT filmtitel FROM filme WHERE laenge > 120;
```
 
##### Aufgabe h
 
```sql
SELECT filmtitel FROM filme WHERE regisseur IN ('Quentin Tarantino', 'Steven Spielberg');
```
 
##### Aufgabe i
 
```sql
SELECT filmtitel FROM filme WHERE regisseur = 'Quentin Tarantino' AND laenge < 90;
```
 
##### Aufgabe j
 
```sql
SELECT filmtitel FROM filme WHERE filmtitel LIKE '%Sibirien%';
```
 
##### Aufgabe k
 
```sql
SELECT filmtitel FROM filme WHERE filmtitel LIKE 'Das große Rennen%';
```
 
##### Aufgabe l
 
```sql
SELECT filmtitel, regisseur FROM filme ORDER BY regisseur;
```
 
##### Aufgabe m
 
```sql
SELECT filmtitel, regisseur FROM filme ORDER BY regisseur, filmtitel;
```
 
##### Aufgabe n
 
```sql
SELECT filmtitel FROM filme WHERE regisseur = 'Quentin Tarantino' ORDER BY laenge DESC;
```