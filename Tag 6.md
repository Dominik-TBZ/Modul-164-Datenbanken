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

### Auftrag Load Date INFILE
🔗 [GitLab Link Load Date INFILE](https://gitlab.com/ch-tbz-it/Stud/m164/-/tree/main/6.Tag?ref_type=heads)

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

### Auftrag Checkpoint  
🔗 [GitLab Link zum Checkpoint](https://gitlab.com/ch-tbz-it/Stud/m164/-/tree/main/6.Tag?ref_type=heads)
 
#### 1. Was sind die Vorteile der Arbeit mit Branches?
Branches ermöglichen eine parallele Entwicklung von neuen Features oder Bugfixes, ohne den Hauptzweig (z.B. `main` oder `master`) zu beeinträchtigen. Dadurch wird die Zusammenarbeit im Team effizienter, und Konflikte bleiben überschaubar. Nach Abschluss einer Änderung können die Commits des Branches durch einen Merge wieder in den Hauptzweig integriert werden.
 
#### 2. Wann und warum sollte man einen Merge durchführen?
Ein Merge wird durchgeführt, sobald die Arbeit in einem separaten Branch abgeschlossen ist und die Änderungen in den Hauptzweig einfließen sollen. So wird der Code-Stand aktualisiert und alle Teammitglieder können auf derselben Basis weiterarbeiten. Regelmäßige Merges verhindern große Konflikte und machen die Entwicklung transparenter.
 
#### 3. Wofür verwendet man Tags in Git?
Tags werden eingesetzt, um wichtige Versionen oder Meilensteine zu kennzeichnen, etwa Release-Versionen. Dadurch lassen sich bestimmte Zustände im Projekt leicht wiederfinden und referenzieren. Ein Tag (z.B. `v1.0.0`) kann später schnell ausgecheckt werden, um exakt diesen Stand des Codes zu reproduzieren.
  
#### 4. Was ist der Unterschied zwischen `git merge` und `git rebase`?
- **Merge**: Hier wird ein zusätzlicher Merge-Commit erzeugt, der die Entwicklungen beider Zweige zusammenführt. Die Historie zeigt eine Verzweigung und einen zusammenführenden Commit.  
- **Rebase**: Dieser Befehl „hängt“ die Commits eines Branches auf einen anderen Branch und erzeugt somit eine lineare Historie, aber verändert die ursprüngliche Commit-Historie des Quell-Branches.
 
#### 5. Was ist ein 'Detached HEAD'-Zustand und wie löst man ihn?
Der 'Detached HEAD'-Zustand tritt auf, wenn man auf einen Commit oder Tag wechselt, anstatt auf einen Branch. Git kann dann nicht zuordnen, in welchem Branch neue Commits abgelegt werden sollen. Man behebt dies, indem man entweder zurück in einen existierenden Branch wechselt (`git checkout main`) oder aus dem aktuellen Zustand einen neuen Branch erstellt (`git checkout -b neuer-branch`).
 
#### 6. Was tut `.gitignore`?
Die `.gitignore`-Datei bestimmt, welche Dateien oder Ordner Git ignorieren soll, etwa temporäre Dateien oder Logs. Das hält das Repository schlank und schützt sensible oder unnötige Daten davor, versioniert zu werden.
 
#### 7. Wofür wird der Befehl `git stash` verwendet?
`git stash` legt aktuelle, ungesicherte Änderungen in einem Zwischenspeicher (Stash) ab und stellt den Arbeitszweig wieder auf den letzten Commit-Zustand zurück. Das ist hilfreich, wenn man schnell den Branch wechseln muss, ohne die momentanen Änderungen zu verlieren. Später können die gestashten Änderungen mit `git stash pop` oder `git stash apply` wiederhergestellt werden.
 
#### 8. Warum sollte man regelmäßig Commits erstellen?
Regelmäßige Commits machen Änderungen nachvollziehbar und erleichtern die Fehlersuche. Bei Bedarf kann man zu einem funktionierenden Stand zurückkehren. Außerdem bleiben Merge-Konflikte überschaubar, wenn kleine Änderungen in kurzen Abständen committet werden.
 
#### Zusammenfassung ganzer Tag 6
Git ist ein verteiltes Versionskontrollsystem, das die Zusammenarbeit an Softwareprojekten vereinfacht. Branches erlauben paralleles Arbeiten, ohne sich gegenseitig zu blockieren. Durch regelmäßige Commits bleibt der Projektfortschritt klar nachvollziehbar. Mit Merge und Rebase können Branches effizient zusammengeführt werden, wobei Rebase eine lineare Historie erzeugt. Tags markieren wichtige Meilensteine wie Release-Versionen. Dank `.gitignore` werden unerwünschte Dateien nicht ins Repository aufgenommen. `git stash` ermöglicht einen schnellen Kontextwechsel ohne Datenverlust. Durch strukturiertes Branch-Management und das Lösen von Merge-Konflikten bleibt der Code konsistent. Am Ende sorgt Git für einen übersichtlichen und sicheren Entwicklungsprozess. So wird die Zusammenarbeit in Teams einfacher und effizienter.