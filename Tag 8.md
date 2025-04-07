# Modul 164 – Datenbanken

## Tag 8

### Auftrag Daten normalisiert einbinden  
🔗 [GitLab Link zu Daten normalisiert einbinden](https://gitlab.com/ch-tbz-it/Stud/m164/-/tree/main/7.Tag?ref_type=heads)

#### 7. Erzeugen Sie weitere 290 Datensätze an SchülerInnen (Name, Geb.datum, Klassennummer, Freifachnummer) mit einem Testdatengenerator 8. (evtl. geeignete Datenwerte anpassen, bzw. durchmischen in EXCEL). Führen Sie die obigen Schritte von Punkt 4,5 und 6 erneut durch.

Ich habe durch ChatGPT mir 50 Datensätze für die Tabelle der SchülerInnen erstellen lassen. Diese Daten habe ich in ein Excel eingefügt und mit einem neuen Infile in die Datenbank eingefügt.

![50 Datensätze](https://github.com/user-attachments/assets/cd6b87f7-7018-4b21-96e3-4ae38948bc8c)

#### 8. Führen Sie folgenden SELECT-Aufgaben auf diese Daten aus: 

**1. Geben Sie aus, wieviele Teilnehmer Inge Sommer in ihrem Freifach hat.**

```
SELECT COUNT(DISTINCT Schueler_Freifach.SchuelerNr) AS Teilnehmerzahl
FROM Lehrer
JOIN Lehrer_Freifach ON Lehrer.LehrerNr = Lehrer_Freifach.LehrerNr
JOIN Schueler_Freifach ON Lehrer_Freifach.FreifachNr = Schueler_Freifach.FreifachNr
WHERE Lehrer.Name = 'Inge Sommer';
```

**2. Geben Sie eine Liste aller Klassen aus, mit jeweils der Anzahl SchülerInnen (→ FK_SchülerIn) in den Freifächern. Die Liste soll nach den Klassen sortiert werden.**

```
SELECT Schueler.Klasse, COUNT(DISTINCT Schueler_Freifach.SchuelerNr) AS AnzahlSchueler
FROM Schueler
JOIN Schueler_Freifach ON Schueler.SchuelerNr = Schueler_Freifach.SchuelerNr
JOIN Freifach ON Schueler_Freifach.FreifachNr = Freifach.FreifachNr
GROUP BY Schueler.Klasse
ORDER BY Schueler.Klasse;
```

**3. Welche SchülerInnen besuchen das Freifach "Chor" oder "Elektronik"?**

```
SELECT DISTINCT Schueler.SchuelerNr, Schueler.Vorname, Schueler.Nachname
FROM Schueler
JOIN Schueler_Freifach ON Schueler.SchuelerNr = Schueler_Freifach.SchuelerNr
JOIN Freifach ON Schueler_Freifach.FreifachNr = Freifach.FreifachNr
WHERE Freifach.Beschreibung IN ('Chor', 'Elektronik');
```

#### 9. Speichern Sie die Ausgaben von Punkt 8 in eine Datei. Benutzen Sie den Befehl SELECT INTO OUTFILE dazu. Ein Error 13 "Permission denied" könnte mit fehlenden Berechtigungen auf One-Drive-Ordner zusammenhängen. Wahlen Sie z.B. den Export-Ordner C:/M164/outfile.csv. Siehe Manual

```
SELECT Schueler.SchuelerNr, Schueler.Vorname, Schueler.Nachname
INTO OUTFILE 'C:\Users\dominik.willisch.sch\Documents\Berufsschule (TBZ)\Modul 164; Datenbank erstellen\Prüfungsvorbereitung LB2\yalla2.csv'
FIELDS TERMINATED BY ','
ENCLOSED BY '"'
LINES TERMINATED BY '\n'
FROM Schueler
JOIN Schueler_Freifach ON Schueler.SchuelerNr = Schueler_Freifach.SchuelerNr
JOIN Freifach ON Schueler_Freifach.FreifachNr = Freifach.FreifachNr
WHERE Freifach.Beschreibung IN ('Chor', 'Elektronik');
```

#### 10. Machen Sie ein Backup der Datenbank Freifaecher.

```
"C:\xampp\mysql\bin\mysqldump.exe" -u root -p schule > "C:\Users\dominik.willisch.sch\Documents\Berufsschule (TBZ)\Modul 164; Datenbank erstellen\Prüfungsvorbereitung LB2\schueler_backup.sql"
```

### Auftrag Steuerdaten  
🔗 [GitLab Link zu dem Auftrag Steuerdaten](https://gitlab.com/ch-tbz-it/Stud/m164/-/tree/main/8.Tag?ref_type=heads)

#### 1. Analysieren Sie die CSV-Datei und normalisieren Sie wo nötig.
Ich habe mir die verschiedenen Spalten angeschaut und mir ein Bild der Datenbank gemacht.
#### 2. Identifizieren Sie die Attribute und legen Sie die Datentypen fest. Bereinigen sie die Quelldatei (1.NF, Überflüssige Daten raus).

```
create table steuern (

stichtag int,

sort int,

cd varchar(5),

quarlang varchar(255),

tarifsort int,

tarifcd varchar(5),

tariflang varchar(255),

SteuerEinkommen_p50 decimal(5,2),

SteuerEinkommen_p25 decimal(5,2),

SteuerEinkommen_p75 decimal(5,2)
```

#### 3. Erstellen Sie ein phys. ERD mit dem dazugehörigen DDL-Script. Importieren Sie nun die Steuerdaten in die Datenbank per Bulkimport. Schreiben Sie ein DML Skript.

![ERD](https://github.com/user-attachments/assets/5822bd4e-036a-4788-b7a8-abecac4c0266)

```
DROP DATABASE IF EXISTS steuern;
CREATE DATABASE steuern;
USE steuern;

create table steuern (

stichtag int,

sort int,

cd varchar(5),

quarlang varchar(255),

tarifsort int,

tarifcd varchar(5),

tariflang varchar(255),

SteuerEinkommen_p50 decimal(5,2),

SteuerEinkommen_p25 decimal(5,2),

SteuerEinkommen_p75 decimal(5,2)
);
 
LOAD DATA LOCAL INFILE 'C:/steuerdaten.csv'
INTO TABLE steuern
FIELDS TERMINATED BY ',' 
LINES TERMINATED BY '\n'
IGNORE 1 ROWS 
(stichtag, sort, cd, quarlang, tarifsort, tarifcd, tariflang, SteuerEinkommen_p50, SteuerEinkommen_p25, SteuerEinkommen_p75);
```

#### 4. Analysieren Sie nun die Daten. Was bedeuten die Felder _p25, _p50, _p75? 

![Daten](https://github.com/user-attachments/assets/e094ee73-7583-4602-849f-207ef827c4f5)

Die Daten Stellen die verschiedenen Tarife auf.

| Quartier      | Tarif              | p25  | p50   | p75   | Interpretation                              |
|---------------|--------------------|------|-------|-------|----------------------------------------------|
| Rathaus       | Grundtarif         | 18.7 | 39.9  | 65.2  | Moderate Einkommen, breite Verteilung        |
| Hochschulen   | Verheiratetentarif | 52   | 89.2  | 178.7 | Viel höhere Einkommen, große Streuung        |
| Langstrasse   | Einelternfamilie   | 15.8 | 30.05 | 46    | Geringe Einkommen, relativ kompakt           |


#### 5. Lösen Sie folgende Fragen: 
**a.	Ermitteln Sie das Quartier mit maximalem Steuereinkommen für _p75?**

```
SELECT quarlang AS Quartier, tariflang AS Tarif, SteuerEinkommen_p75
FROM steuern
WHERE SteuerEinkommen_p75 IS NOT NULL
ORDER BY SteuerEinkommen_p75 DESC
LIMIT 1;
```

**b.	Welches Quartier hat das niedrigste Steuereinkommen für _p50?**

```
SELECT quarlang AS Quartier, tariflang AS Tarif, SteuerEinkommen_p50
FROM steuern
WHERE SteuerEinkommen_p50 IS NOT NULL
ORDER BY SteuerEinkommen_p50 ASC
LIMIT 1;
```

**c.	Welches Quartier hat das höchste Steuereinkommen für _p50?**

```
SELECT quarlang AS Quartier, tariflang AS Tarif, SteuerEinkommen_p50
FROM steuern
WHERE SteuerEinkommen_p50 IS NOT NULL
ORDER BY SteuerEinkommen_p50 DESC
LIMIT 1;
```

#### 6. Erstellen Sie ein Backup der Datenbank.

```
"C:\xampp\mysql\bin\mysqldump.exe" -u root -p steuern > "C:\Users\dominik.willisch.sch\Documents\Berufsschule (TBZ)\Modul 164; Datenbank erstellen\Prüfungsvorbereitung LB2\Backup.sql"
```

![Backup Datei](https://github.com/user-attachments/assets/fbfa90a4-7b88-41df-8455-3e115daaacdd)