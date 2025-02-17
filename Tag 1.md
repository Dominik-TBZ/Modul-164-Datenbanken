# Modul 164 – Datenbanken

## Tag 1

### Auftrag Recap  
🔗 [GitLab Link zum Recap](https://gitlab.com/ch-tbz-it/Stud/m164/-/blob/main/1.Tag/Recap/Recap.md)

---

## 1. Stufen der Wissenstreppe und Beispiel mit einem Wechselkurs

Die **Wissenstreppe** beschreibt den Prozess der Transformation von Daten zu Wissen und Kompetenz. Die Stufen in der richtigen Reihenfolge sind:

1. **Zeichen** – Einzelne Zeichen oder Symbole ohne Bedeutung.  
2. **Daten** – Zeichen mit einer Syntax, z. B. `1.12`.  
3. **Information** – Daten mit Bedeutung, z. B. `1 EUR = 1.12 USD`.  
4. **Wissen** – Informationen mit Kontext, z. B. `Der Wechselkurs schwankt täglich`.  
5. **Können** – Wissen, das in Fähigkeiten umgesetzt wird, z. B. `Ich kann Wechselkurse analysieren`.  
6. **Handeln** – Bewusstes Anwenden von Wissen und Können, z. B. `Ich tausche Geld zum besten Zeitpunkt`.  
7. **Kompetenz** – Nachhaltige, reflektierte Handlungskompetenz, z. B. `Ich bin Devisenhändler und treffe fundierte Entscheidungen`.

---

## 2. Netzwerk-Beziehungen im logischen Modell

Im **logischen Modell** werden Netzwerk-Beziehungen durch **relationale Tabellen** mit **Primär- und Fremdschlüsseln** abgebildet. Man verwendet:

- **1:1-Beziehungen** – Ein Datensatz ist mit genau einem anderen verknüpft.  
- **1:n-Beziehungen** – Ein Datensatz kann mit mehreren anderen verknüpft sein.  
- **m:n-Beziehungen** – Zwei Tabellen sind über eine Zwischentabelle miteinander verbunden.

📌 **Beispiel:**  
Eine Tabelle **„Kunden“** ist mit einer Tabelle **„Bestellungen“** über eine **1:n-Beziehung** verknüpft.

---

## 3. Anomalien in einer Datenbasis und ihre Arten

Anomalien treten auf, wenn eine Datenbank nicht gut normalisiert ist. Es gibt drei Hauptarten:

1. **Einfügeanomalie** – Neue Daten können nicht eingefügt werden, weil abhängige Daten fehlen.  
   *Beispiel:* Man kann keinen neuen Kunden anlegen, weil er noch keine Bestellung hat.

2. **Änderungsanomalie** – Änderungen müssen mehrfach vorgenommen werden.  
   *Beispiel:* Eine Preisänderung muss an mehreren Stellen aktualisiert werden.

3. **Löschanomalie** – Beim Löschen eines Datensatzes gehen auch wichtige Informationen verloren.  
   *Beispiel:* Löscht man eine Bestellung, geht auch die Kundeninformation verloren.

---

## 4. Gibt es redundante Daten? Warum?

Ja, **redundante Daten** existieren, wenn dieselben Informationen mehrfach in einer Datenbank gespeichert sind. Dies geschieht oft durch:

- **Schlechte Normalisierung** – Daten werden nicht effizient strukturiert.  
- **Performance-Optimierung** – Manche Redundanzen werden bewusst eingeführt, um Abfragen zu beschleunigen.  
- **Fehlendes Datenmanagement** – Unstrukturierte Speicherung kann Redundanzen erzeugen.

🔴 **Problem:**  
Redundanzen führen zu **Inkonsistenzen**, da Änderungen an einer Stelle nicht überall aktualisiert werden.

---

## 5. Datenstrukturierung bei der Erhebung und Ablage von Daten

Es gibt zwei wesentliche Aspekte der Datenstrukturierung:

1. **Inhaltliche Strukturierung** – Wie Daten logisch organisiert sind (Tabellen, Felder).  
2. **Technische Strukturierung** – Wie Daten physisch gespeichert werden (Dateiformate, Speicherorte).

### Kategorien der Strukturierung:

- **Unstrukturierte Daten** – Textdokumente, Bilder, Videos.  
- **Semistrukturierte Daten** – JSON, XML, Log-Dateien.  
- **Strukturierte Daten** – Tabellen, relationale Datenbanken.

### Anforderungen für eine Datenbank:

Daten müssen **normalisiert** sein, um Redundanzen und Anomalien zu vermeiden. Typische Normalformen:

- **1. Normalform (1NF)** – Keine mehrfachen Werte in Spalten.  
- **2. Normalform (2NF)** – Alle Nicht-Schlüssel-Attribute hängen vom gesamten Primärschlüssel ab.  
- **3. Normalform (3NF)** – Keine transitiven Abhängigkeiten.

---

## 6. Beschreibung eines Bildes mit Fachbegriffen

![Datenbank-Tabelle mit Fachbegriffen](https://gitlab.com/ch-tbz-it/Stud/m164/-/raw/main/1.Tag/Recap/Tabelle_labelled.png)


1️⃣ **Tabellenname**  
2️⃣ **Datensätze (Tupel, Zeilen)**  
3️⃣ **Spaltenüberschriften (Attribute, Felder)**  
4️⃣ **Attributwerte (Feldwerte)**  
5️⃣ **Primärschlüssel (MitarbeiterId)**
