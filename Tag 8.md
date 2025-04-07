# Modul 164 – Datenbanken

## Tag 8

### Auftrag Recap  
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