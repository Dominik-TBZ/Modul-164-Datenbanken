# Modul 164 – Datenbanken

## Tag 9

### Auftrag Common Table Expressions  
🔗 [GitLab Link zu Common Table Expressions](https://gitlab.com/ch-tbz-it/Stud/m164/-/tree/main/9.Tag?ref_type=heads)
 
#### 🧠 Was ist CTE
 
Eine **CTE (Common Table Expression)** ist eine **Zwischentabelle**, die man mit dem Befehl **WITH** in SQL erstellt.  
Sie hilft dabei, **komplizierte Abfragen** übersichtlicher zu machen.  
CTEs sind **nur vorübergehend** – sie gelten nur während der Abfrage.
 
#### Vorteile:
- Klarer und besser lesbarer SQL-Code  
- Kein mehrfaches Schreiben derselben Abfrage  
- Bessere Struktur bei komplexen Datenabfragen

#### Beispiel

```
DROP DATABASE IF EXISTS Autoladen;
CREATE DATABASE Autoladen;
USE Autoladen;
 
CREATE TABLE Auto (
    AutoID INT PRIMARY KEY,
    Marke VARCHAR(50),
    Modell VARCHAR(50),
    Baujahr INT,
    Preis DECIMAL(10, 2)
);
 
CREATE TABLE Verkaeufer (
    VerkaeuferID INT PRIMARY KEY,
    Name VARCHAR(100),
    Ort VARCHAR(100)
);
 
CREATE TABLE Verkauf (
    VerkaufID INT PRIMARY KEY,
    AutoID INT,
    VerkaeuferID INT,
    Verkaufsdatum DATE,
    FOREIGN KEY (AutoID) REFERENCES Auto(AutoID),
    FOREIGN KEY (VerkaeuferID) REFERENCES Verkaeufer(VerkaeuferID)
);
 
INSERT INTO Auto VALUES
(1, 'BMW', '3er', 2018, 23000.00),
(2, 'Audi', 'A4', 2019, 25000.00),
(3, 'VW', 'Golf', 2016, 15000.00),
(4, 'Tesla', 'Model 3', 2021, 35000.00);
 
INSERT INTO Verkaeufer VALUES
(1, 'Max Müller', 'Zürich'),
(2, 'Anna Schmidt', 'Bern'),
(3, 'Lukas Weber', 'Basel');
 
INSERT INTO Verkauf VALUES
(1, 1, 1, '2024-06-10'),
(2, 2, 2, '2024-07-15'),
(3, 4, 3, '2025-01-20');
 
WITH AlleAutos AS (
    SELECT AutoID, Marke, Modell, Preis
    FROM Auto
)
SELECT * FROM AlleAutos;
 
WITH BMWVerkaeufer AS (
    SELECT ve.Name AS VerkäuferName, ve.Ort AS VerkäuferOrt
    FROM Auto a
    JOIN Verkauf v ON a.AutoID = v.AutoID
    JOIN Verkaeufer ve ON v.VerkaeuferID = ve.VerkaeuferID
    WHERE a.Marke = 'BMW' AND a.Modell = '3er'
)
SELECT * FROM BMWVerkaeufer;
```

### Auftrag Stored Procedures 
🔗 [GitLab Link zu Stored Procedures](https://gitlab.com/ch-tbz-it/Stud/m164/-/tree/main/9.Tag?ref_type=heads)
 
#### 🧠 Was ist eine Stored Procedure?
Eine Stored Procedure ist ein in der Datenbank gespeichertes, vordefiniertes SQL-Programm, das beliebig oft aufgerufen werden kann, um wiederkehrende Aufgaben zentral auszuführen. Es ist wie im klassischen Programmieren eine "Methode" welche man beliebig oft und mit Parametern aufrufen kann.
 
#### Vorteile
- **Wiederverwendbarkeit** – einmal erstellt, stets ohne Neuschreiben aufrufbar  
- **Zentralisierte Geschäftslogik** – Logik liegt in der Datenbank, nicht in jeder Anwendung  
- **Performance** – Ausführung direkt auf dem Server spart Netzwerkverkehr und ermöglicht RDBMS-Optimierung  
- **Sicherheit** – Nutzer benötigen nur Ausführungsrechte, nicht zwingend Tabellenrechte  
 
#### Grundaufbau (RDBMS-abhängige Syntax)
1. `CREATE PROCEDURE Name(Parameter …);`  
2. `BEGIN … SQL-Befehle … END;`  
3. Aufruf mit `CALL ProcedureName(args);`
 
#### Beispiel
```
DROP DATABASE IF EXISTS Firma;
CREATE DATABASE Firma;
USE Firma;

CREATE TABLE Kunden (
    KundenID INT PRIMARY KEY,
    Name VARCHAR(100),
    Adresse VARCHAR(200),
    Telefon VARCHAR(20)
);

INSERT INTO Kunden VALUES
(1, 'Max Meier', 'Bahnhofstrasse 10, Zürich', '044 123 45 67'),
(2, 'Anna Müller', 'Hauptstrasse 5, Bern', '031 987 65 43'),
(3, 'Lukas Steiner', 'Seestrasse 8, Luzern', '041 555 44 33');

DELIMITER $$

CREATE PROCEDURE GetKundendaten(IN kunden_id INT)
BEGIN
    SELECT Name, Adresse, Telefon
    FROM Kunden
    WHERE KundenID = kunden_id;
END $$

DELIMITER ;

CALL GetKundendaten(1);
```

### Abschluss

Nun ist das Modul zuende. Ich habe dieses Modul sehr spannend gefunden, ich habe sehr vieles neu gelernt. In der Firma ist SQL eher weniger das Thema, wehalb das alles neu für mich war. Mit dem Modul und dem gleichzeitigen ÜK bin ich doch gut reingekommen und weiss jetzt viel mehr, als noch im Winter.