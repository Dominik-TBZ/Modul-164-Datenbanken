# Modul 164 – Datenbanken

## Tag 4

### Auftrag Mengenlehre  
🔗 [GitLab Link zur Mengenlehre](https://gitlab.com/ch-tbz-it/Stud/m164/-/blob/main/4.Tag/Auftrag_Mengenlehre.md)

![Mengenlehre](https://github.com/user-attachments/assets/d8ca3726-a06c-48d1-9e61-73ef0069d3f7)
 
#### Aufgabe 1
 
##### Gegebene Mengen:
- **A** = { c, e, z, r, d, g, u, x }  
- **B** = { c, e, g }  
- **C** = { r, d, g, t }  
- **D** = { e, z, u }  
- **E** = { z, r, u }
 
##### Aussagen prüfen:
 
| Aussage | Wahrheitswert | Erklärung                      |
|---------|---------------|--------------------------------|
| B ⊂ A   | ✅ Wahr       | Alle Elemente von B in A.      |
| C ⊂ A   | ❌ Falsch     | Element t fehlt in A.          |
| E ⊂ A   | ✅ Wahr       | Alle Elemente von E in A.      |
| B ⊂ C   | ❌ Falsch     | Elemente c, e fehlen in C.     |
| E ⊂ C   | ❌ Falsch     | Elemente z, u fehlen in C.     |
 
#### Aufgabe 2
 
##### Gegebene Mengen:
- **A** = { 1, 2, 3, 4, 5 }  
- **B** = { 2, 5 }  
- **C** = { 3, 5, 7, 9 }
 
##### Lösungen:
 
- **a)** A ∩ B = { 2, 5 }
- **b)** A ∪ C = { 1, 2, 3, 4, 5, 7, 9 }
- **c)** Komplement von B bezüglich A = { 1, 3, 4 }
- **d)** B \ C = { 2 }
- **e)** C \ B = { 3, 7, 9 }
 
#### Aufgabe 3 (Eigene Beispiele)
 
| Symbol | Beispiel | Erklärung |
|--------|----------|-----------|
| ⊂      | {Apfel, Banane} ⊂ {Apfel, Banane, Orange} | Alle Elemente links sind rechts enthalten. |
| ∩      | {Minecraft, GTA} ∩ {GTA, Schach} = {GTA} | Gemeinsames Element beider Mengen. |
| ∪      | {Netflix, Disney+} ∪ {Disney+, ARD} = {Netflix, Disney+, ARD} | Vereinigung beider Mengen ohne Dopplungen. |
| \      | {Hund, Katze, Vogel} \ {Katze, Maus} = {Hund, Vogel} | Elemente aus erster Menge ohne Elemente der zweiten Menge. |

### Auftrag Join 1  
🔗 [GitLab Link zu Join 1](https://gitlab.com/ch-tbz-it/Stud/m164/-/blob/main/4.Tag/Auftrag_select_join.md)

#### Warum gibt die folgende Abfrage kein sinnvolles Ergebnis?  

**SQL-Befehl:**  
```
SELECT * FROM kunden  
INNER JOIN orte;  
```
**Erklärung:**  
Diese Abfrage führt zu einem **kartesischen Produkt**, da keine **JOIN-Bedingung** angegeben wurde.  
Jede Zeile der Tabelle `kunden` wird mit jeder Zeile der Tabelle `orte` kombiniert, was zu fehlerhaften Zuordnungen führt.  

Die **korrekte Variante** ist:  

**SQL-Befehl:**  
```
SELECT * FROM kunden  
INNER JOIN orte ON kunden.fk_ort_postleitzahl = orte.id_postleitzahl;  
```
#### **Einfache Abfragen über zwei Tabellen**

##### **a) Geben Sie Name, Postleitzahl und Wohnort aller Kunden aus.**  

**SQL-Befehl:**  
```
SELECT kunden.name, kunden.fk_ort_postleitzahl, orte.name  
FROM kunden  
JOIN orte ON kunden.fk_ort_postleitzahl = orte.id_postleitzahl;  
```
##### **b) Geben Sie Name und Wohnort aller Kunden aus, die die Postleitzahl 79312 haben.**  

**SQL-Befehl:**  
```
SELECT kunden.name, orte.name  
FROM kunden  
JOIN orte ON kunden.fk_ort_postleitzahl = orte.id_postleitzahl  
WHERE kunden.fk_ort_postleitzahl = '79312';
```
##### **c) Geben Sie Name und Wohnort aller Kunden aus, die in Emmendingen wohnen.**  
*(Einschränkungskriterium ist NICHT die Postleitzahl, sondern 'Emmendingen')*  

**SQL-Befehl:**  
```
SELECT kunden.name, orte.name  
FROM kunden  
JOIN orte ON kunden.fk_ort_postleitzahl = orte.id_postleitzahl  
WHERE orte.name = 'Emmendingen';
```

##### **d) Geben Sie Name, Wohnort und Einwohnerzahl für alle Kunden aus, die in einem Ort mit mehr als 70.000 Einwohnern wohnen.**  

**SQL-Befehl:**  
```
SELECT kunden.name, orte.name, orte.einwohnerzahl  
FROM kunden  
JOIN orte ON kunden.fk_ort_postleitzahl = orte.id_postleitzahl  
WHERE orte.einwohnerzahl > 70000;  
```
##### **e) Geben Sie alle Orte aus, die weniger als 1.000.000 Einwohner haben.**  

**SQL-Befehl:**  
```
SELECT name  
FROM orte  
WHERE einwohnerzahl < 1000000;  
```

##### **f) Geben Sie Kundename und Ortname aus für alle Kunden, die in Orten mit einer Einwohnerzahl zwischen 100.000 und 1.500.000 leben.**  

**SQL-Befehl:**  
```
SELECT kunden.name, orte.name  
FROM kunden  
JOIN orte ON kunden.fk_ort_postleitzahl = orte.id_postleitzahl  
WHERE orte.einwohnerzahl BETWEEN 100000 AND 1500000;  
```

##### **g) Geben Sie Kundename, Postleitzahl und Ortname aus für alle Kunden, deren Name ein "e" enthält und alle Orte, die ein "u" oder ein "r" enthalten.**  

**SQL-Befehl:**  
```
SELECT kunden.name, kunden.fk_ort_postleitzahl, orte.name  
FROM kunden  
JOIN orte ON kunden.fk_ort_postleitzahl = orte.id_postleitzahl  
WHERE kunden.name LIKE '%e%'  
AND (orte.name LIKE '%u%' OR orte.name LIKE '%r%');  
```