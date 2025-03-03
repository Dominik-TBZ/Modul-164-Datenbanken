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

#### Auftrag Tourenplaner
 
##### 1. Tabellen erstellen
 
```SQL
CREATE TABLE Tabellenname (
    Spalte1,
    Spalte2,
    Spalte3,
    ...
);
```
 
##### 2. Daten einfügen
 
```SQL
INSERT INTO Tabellenname (Spalte1, Spalte2, Spalte3, ...)
VALUES (Wert1, Wert2, Wert3, ...);
```
 
##### 3. Daten abfragen
 
```SQL
SELECT Spalte1, Spalte2, ...
FROM Tabellenname
WHERE Bedingung;
```
 
##### Tabelle erstellen
 
```SQL
CREATE TABLE Kunden (
    KundenID,
    Name,
    Stadt
);
```
 
##### Daten einfügen
 
```SQL
INSERT INTO Kunden (KundenID, Name, Stadt)
VALUES (1, 'Max Mustermann', 'Zürich');
```
 
##### Daten abfragen
 
```SQL
SELECT Name, Stadt
FROM Kunden
WHERE Stadt = 'Zürich';
```