# Modul 164 – Datenbanken

## Tag 7

### Auftrag Datensicherung  
🔗 [GitLab Link zur Datensicherung](https://gitlab.com/ch-tbz-it/Stud/m164/-/blob/main/7.Tag/Datensicherung.md)

#### Bedeutung der Datensicherung
Datenbanksysteme sind essenziell für Webhosting und Unternehmenssoftware, da sie die Verfügbarkeit und Integrität von Daten gewährleisten. Ein Ausfall oder Verlust kann Websites unzugänglich machen, Anwendungen lahmlegen und zu Vertrauensverlust bei Kunden führen. Häufige Ursachen sind technische Defekte oder Benutzerfehler. Um irreversible Schäden zu vermeiden, sind regelmäßige Backups unerlässlich.

#### Backup-Methoden
Es gibt zwei Hauptkategorien der Datensicherung:

- **Online-Backup**: Die Datenbank bleibt während der Sicherung aktiv.
- **Offline-Backup**: Die Datenbank wird für die Dauer des Backups heruntergefahren.

Zusätzlich unterscheidet man drei Backup-Arten:

1. **Voll-Backup**: Alle Daten werden gespeichert, benötigt jedoch viel Speicherplatz.
2. **Differentielles Backup**: Speichert nur geänderte und neue Dateien seit dem letzten Voll-Backup.
3. **Inkrementelles Backup**: Speichert nur Änderungen seit der letzten Sicherung (egal ob Voll- oder inkrementelles Backup).

![Daarstellung Backupmethoden](https://github.com/user-attachments/assets/9df8f3f6-ab65-469f-b251-b58a19770c7f)

#### Backup-Tools
Zur Durchführung von Backups gibt es verschiedene Tools:

- **MySQLDump**: Shell-basierte Lösung für schnelle Voll-Backups.
- **phpMyAdmin**: Export von Datenbanken im SQL-Format (bei großen Datenbanken begrenzt).
- **BigDump**: Import großer Backups, jedoch ohne eigene Backup-Funktion.
- **HeidiSQL**: Windows-Tool ohne Automatisierungsfunktionen.
- **Mariabackup**: Open-Source-Lösung für physische Online-Backups von MariaDB.

#### Auftrag 1; Dumbfile aus MySQL Workbench erstellen

##### **--> Tabellen auswählen, von welchen ein Backup gemacht werden soll.**

![Bild1](https://github.com/user-attachments/assets/2ff43646-79e5-472e-a53c-7c7196460d33)

##### **--> Sobald der Balken durchgelaufen ist, sind die Daten erfolgreich gespeichert worden.**

![Bild2](https://github.com/user-attachments/assets/a381258b-9781-4c88-acb7-5637bd51c3c3)

##### **--> Danach werden die Daten in SQL Files gespeichert.**

![Bild3](https://github.com/user-attachments/assets/8af1305d-21c6-402c-b0e4-f65cca48f8db)

#### Auftrag 2; Backup-File analysieren und verifizieren

Viele Codekommentare zur Erklärung der Schritte.
SQL-Befehle zur Erstellung der Datenbankstruktur (CREATE DATABASE, CREATE TABLE) und zum Einfügen von Daten (INSERT INTO).

Die Datenbank konnte nach der Löschung wieder vollständig hergestellt werden.

#### Auftrag 3; Backup Strategien

##### a. Backup-Typ in Aufgabe 1
Logisches Backup (SQL-Befehle zur Erstellung und Befüllung der Datenbank).

##### b. Nachteile dieses Backups
Langsame Wiederherstellung.

Abhängig von der Systemumgebung.

##### Unterschied Online- vs. Offline-Backups
**Online:** Datenbank bleibt während des Backups aktiv.

**Offline:** Datenbank muss für das Backup gestoppt werden.

##### Was ist ein Snapshot-Backup?
Eine Momentaufnahme der Datenbank, die konsistent und schnell ist, ohne dass die Datenbank gestoppt werden muss.

#### Auftrag 4; Physisches Backup (Variante MariaDB, XAMPP)

**Erstellen Sie ein Full-Backup mit dem Backup-Programm mariabackup.exe.**
```
mariabackup --backup --target-dir="C:\Users\dominik.willisch.sch\Documents\Berufsschule (TBZ)\Modul 164; Datenbank erstellen\Prüfungsvorbereitung LB2\Tabellen.sql"
```
**Wozu muss die Backupdatenstruktur vor einem Restore "vorbereitet" (prepare) werden?**
```
mariabackup --prepare --target-dir="C:\Users\dominik.willisch.sch\Documents\Berufsschule (TBZ)\Modul 164; Datenbank erstellen\Prüfungsvorbereitung LB2\Tabellen.sql"
```
**Wie heist der Parameter, um ein Restore zu machen?**
```
mariabackup --copy-back --target-dir="C:\Users\dominik.willisch.sch\Documents\Berufsschule (TBZ)\Modul 164; Datenbank erstellen\Prüfungsvorbereitung LB2\Tabellen.sql"
```
**Ist ein inkrementelles und differentielles Backup möglich? Wenn ja, wie?**
**Inkrementell:** Sichert nur Änderungen seit dem letzten vollständigen Backup.

**Differentiell:** Sichert Änderungen seit dem letzten vollständigen oder differenziellen Backup.

### Auftrag Daten normalisiert einbinden
🔗 [GitLab Link zu Daten normalisiert einbinden](https://gitlab.com/ch-tbz-it/Stud/m164/-/tree/main/7.Tag?ref_type=heads)

#### 1. Erstellen Sie die erste Normalform der EXCEL-Tabelle und exportieren Sie die Daten als CSV-Dateien. (Atomare Felder, Redundanz wird bewusst erzeugt).

![CSV - Dateien](https://github.com/user-attachments/assets/6e911612-2bf2-4481-b2ce-1b18c6df4233)

#### 2. Erstellen Sie das log. ERD. Die (5) Tabellen müssen mind. in der 2.NF sein. Bestimmen Sie die Kardinalitäten.

![Datenmodell](https://github.com/user-attachments/assets/93a75bea-a41a-444a-9a35-ea7cdabc5da2)

#### 3. Erstellen Sie das physische ERD und setzen Sie die ersichtlichen, nötigen NN- und UQ-Eigenschaften (Constraints) bei den Attributen und Fremdschlüsseln. Erzeugen Sie durch z.B. Forward Engineering ein SQL-DDL-Script (Datentypen, FK-Constriants, Constaint-Optionen).

```
-- Datenbank löschen und neu erstellen
DROP DATABASE IF EXISTS Schule;
CREATE DATABASE IF NOT EXISTS Schule;
USE Schule;

-- Tabelle: Schueler
CREATE TABLE Schueler (
    SchuelerNr INT PRIMARY KEY,
    Vorname VARCHAR(50) NOT NULL,
    Nachname VARCHAR(50) NOT NULL,
    GebDatum DATE NOT NULL,
    Klasse VARCHAR(10) NOT NULL,
    Klassenlehrer VARCHAR(50) NOT NULL
);

-- Tabelle: Freifach
CREATE TABLE Freifach (
    FreifachNr INT PRIMARY KEY,
    Beschreibung VARCHAR(100) NOT NULL,
    Tag VARCHAR(10) NOT NULL,
    Zimmer VARCHAR(10) NOT NULL
);

-- Tabelle: Lehrer
CREATE TABLE Lehrer (
    LehrerNr INT PRIMARY KEY,
    Name VARCHAR(50) NOT NULL,
    GebDatum DATE NULL
);

-- Tabelle: Schueler_Freifach (n:m Beziehung)
CREATE TABLE Schueler_Freifach (
    SchuelerNr INT,
    FreifachNr INT,
    PRIMARY KEY (SchuelerNr, FreifachNr),
    FOREIGN KEY (SchuelerNr) REFERENCES Schueler(SchuelerNr),
    FOREIGN KEY (FreifachNr) REFERENCES Freifach(FreifachNr)
);

-- Tabelle: Lehrer_Freifach (n:m Beziehung)
CREATE TABLE Lehrer_Freifach (
    LehrerNr INT,
    FreifachNr INT,
    PRIMARY KEY (LehrerNr, FreifachNr),
    FOREIGN KEY (LehrerNr) REFERENCES Lehrer(LehrerNr),
    FOREIGN KEY (FreifachNr) REFERENCES Freifach(FreifachNr)
);
```

#### 4. Übertragen Sie die Daten der CSV-Dateien in die normalisierten Tabellen mittels LOAD DATA LOCAL INFILE (direkt oder indirekt). Überwachen Sie die korrekte Übertragung der Beziehungen (FK=ID).

![Load Data Infile](https://github.com/user-attachments/assets/89030661-4d9e-4f9e-962c-565a9b141a31)

#### 5. Bereinigen Sie die Daten (Redundanz, leere Werte, Inkonsistenz). Benutzen Sie wenn nötig die Scripte aus Tag 6! als Vorlage.

Ich habe die Daten bereinigt und alle Datensätze mit Null werten gelöscht. 

#### 6. Testen Sie die eingelesenen Daten! Vergleichen Sie die Tabellenwerte der CSV-Dateien mit den gleichen, angepassten Select-Abfragen aus ihrer normalisierten Datenbank. Die Ausgabelisten sollten dieselben sein!

![Daten](https://github.com/user-attachments/assets/dc907660-8eba-48be-b603-6f3c0bb12eaa)