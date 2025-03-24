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

#### Auftrag 1

**Welches ist das teuerste Buch in der Datenbank?**

SELECT * FROM buecher ORDER BY einkaufspreis DESC LIMIT 1;

**Welches ist das billigste Buch in der Datenbank?**

SELECT * FROM buecher ORDER BY einkaufspreis ASC LIMIT 1;

**Lassen Sie sich alle Bücher ausgeben, deren Einkaufspreis über dem durchschnittlichen Einkaufspreis aller Bücher in der Datenbank liegt.**

SELECT * FROM buecher WHERE einkaufspreis > (SELECT AVG(einkaufspreis) FROM buecher);

**Lassen Sie sich alle Bücher ausgeben, deren Einkaufspreis über dem durchschnittlichen Einkaufspreis der Thriller liegt.**

SELECT * FROM buecher 
WHERE einkaufspreis > (SELECT AVG(einkaufspreis) FROM buecher WHERE genre = 'Thriller');

**Lassen Sie sich alle Thriller ausgeben, deren Einkaufspreis über dem durchschnittlichen Einkaufspreis der Thriller liegt.**

SELECT * FROM buecher 
WHERE genre = 'Thriller' 
AND einkaufspreis > (SELECT AVG(einkaufspreis) FROM buecher WHERE genre = 'Thriller');

**Lassen Sie sich alle Bücher ausgeben, bei denen der Gewinn überdurchschnittlich ist; bei der Berechnung des Gewinndurchschnitts berücksichtigen Sie NICHT das Buch mit der id 22.**

SELECT * FROM buecher 
WHERE (verkaufspreis - einkaufspreis) > 
      (SELECT AVG(verkaufspreis - einkaufspreis) 
       FROM buecher WHERE id <> 22);

#### Auftrag 2