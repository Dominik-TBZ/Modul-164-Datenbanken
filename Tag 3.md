# Modul 164 – Datenbanken

## Tag 3

### Auftrag Datentypen  
🔗 [GitLab Link zu den Datentypen](https://gitlab.com/ch-tbz-it/Stud/m164/-/tree/main/3.Tag)

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
