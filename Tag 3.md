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

1. **53** → `TINYINT UNSIGNED`  
2. **983773** → `MEDIUMINT UNSIGNED`  
3. **Fritz Schmitt** → `VARCHAR(50)`  
4. **fritz_schmitt** → `VARCHAR(30)`  
5. **müller13** → `VARCHAR(255)` (Passwort sollte gehasht gespeichert werden)  
6. **07661/66456-2** → `VARCHAR(20)`  
7. **Kirchweg** → `VARCHAR(50)`  
8. **Kirchweg 13** → `VARCHAR(50)`  
9. **Langer Text** → `TEXT`  
10. **2.88499399** → `FLOAT(10,8)`  
11. **19.03.2009** → `DATE`  
12. **13.02.1983 22:07:12** → `DATETIME`  
13. **64002** → `SMALLINT UNSIGNED`  

##### 2. Welche Datentypen würden Sie für die folgenden Felder jeweils vergeben?  
Wählen Sie den Datentyp möglichst speicherplatzoptimiert.  

1. **Id** → `INT UNSIGNED AUTO_INCREMENT`  
2. **Vorname** → `VARCHAR(50)`  
3. **Nachname** → `VARCHAR(50)`  
4. **Strasse** → `VARCHAR(100)`  
5. **Hausnummer** → `SMALLINT UNSIGNED`  
6. **Postleitzahl** → `MEDIUMINT UNSIGNED`  
7. **Telefonnummer** → `VARCHAR(20)`  
8. **Registrierungsdatum** → `DATE`  
9. **Bestellnummer** → `INT UNSIGNED`  
10. **kommentar_an_versender** → `TEXT`  
11. **preis** → `FLOAT(10,3)`  
12. **enthaltene_mehrwertsteuer** → `DECIMAL(6,4)`  
13. **roman_vollstaendiger_inhalt** → `MEDIUMTEXT`  

##### 3. Welche MySQL-Datentypen sind in Aufgabe 1 und 2 noch nicht vorgekommen?  
Listen Sie fünf Beispiele auf und schreiben Sie die korrekte Datentypbezeichnung für MySQL.  

- `ENUM('Ja', 'Nein')`  
- `BOOLEAN`  
- `BLOB`  
- `TIME`  
- `CHAR(10)`  
