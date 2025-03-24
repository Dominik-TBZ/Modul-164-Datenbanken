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