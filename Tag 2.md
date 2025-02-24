# Modul 164 – Datenbanken

## Tag 2

### Auftrag Generalisierung  
🔗 [GitLab Link zu Generalisieren](https://gitlab.com/ch-tbz-it/Stud/m164/-/tree/main/2.Tag)

#### Was ist Generalisierung? 🤔

Die Generalisierung wird verwendet, wenn verschiedene Entitäten ähnliche Attribute oder Eigenschaften teilen. Statt diese Eigenschaften in jeder Entität zu wiederholen, erstellt man eine gemeinsame **Oberklasse** (oder Superklasse), die die gemeinsamen Attribute enthält. Die verschiedenen Entitäten, die diese gemeinsamen Eigenschaften besitzen, werden als **Unterklassen** (oder Subklassen) der Oberklasse betrachtet.

#### Beispiel der Generalisierung:

- **Oberklasse**: `Person` 👤  
  **Attribute**: `Name`, `Geburtsdatum`, `Adresse`
  
- **Unterklassen**:
  - **Mitarbeiter** 💼: `Mitarbeiter-ID`, `Abteilung`
  - **Kunde** 🛍️: `Kundennummer`, `Kaufhistorie`

In diesem Beispiel teilen die Entitäten `Mitarbeiter` und `Kunde` einige Attribute wie `Name`, `Geburtsdatum`, `Adresse` mit der Oberklasse `Person`. Spezifische Attribute (wie `Mitarbeiter-ID` und `Kundennummer`) werden in den jeweiligen Unterklassen definiert.

#### Vorteile der Generalisierung 👍

1. **Reduzierung von Redundanz**:
   - Gemeinsame Attribute oder Eigenschaften müssen nur einmal in der Oberklasse definiert werden. Dies verhindert die wiederholte Speicherung der gleichen Daten in mehreren Tabellen und reduziert die Datenwiederholung.
   
2. **Vereinfachung der Datenstruktur**:
   - Eine Generalisierung vereinfacht das Datenmodell und macht es übersichtlicher. Sie ermöglicht eine hierarchische Struktur, in der gemeinsam genutzte Eigenschaften zentral gespeichert werden.

3. **Erleichterung der Wartung**:
   - Wenn sich das gemeinsame Attribut ändert (z. B. eine Änderung der Struktur des Attributs `Adresse`), muss es nur in der Oberklasse angepasst werden, anstatt in jeder einzelnen Unterklasse.

4. **Förderung der Datenintegrität**:
   - Da die gemeinsamen Attribute nur an einer Stelle gespeichert sind, wird die Konsistenz der Daten gewährleistet. Änderungen an einem gemeinsamen Attribut wirken sich sofort auf alle Unterklassen aus, ohne dass manuelle Synchronisation erforderlich ist.

5. **Verwendung von Vererbung**:
   - Die Unterklassen erben die gemeinsamen Attribute der Oberklasse, was den Prozess der Erweiterung von Entitäten vereinfacht und neue Entitäten hinzufügt, ohne die bestehende Struktur zu verändern.

#### Nachteile der Generalisierung ⚠️

1. **Komplexität der Abfragen**:
   - In relationalen Datenbanken kann die Generalisierung zu komplexeren Abfragen führen, da man die Daten aus mehreren Tabellen (Oberklasse und Unterklassen) zusammenführen muss. Dies kann die Leistung und Komplexität der SQL-Abfragen erhöhen.

2. **Verlust von Flexibilität**:
   - Eine strikte Generalisierung könnte die Flexibilität einschränken, wenn spätere Änderungen eine differenziertere Modellierung erfordern. Zum Beispiel, wenn einige Attribute nur für eine Unterklasse spezifisch sind, aber aufgrund der Generalisierung in der Oberklasse gespeichert werden müssen.

3. **Fehlende klare Trennung**:
   - Wenn zu viele Entitäten in einer einzigen Oberklasse zusammengefasst werden, könnte es schwieriger werden, die genauen Unterschiede zwischen den Entitäten zu erkennen. In manchen Fällen könnte das Modell zu allgemein werden und die spezifischen Anforderungen der einzelnen Entitäten vernachlässigen.

4. **Overhead bei komplexeren Hierarchien**:
   - Wenn viele Entitäten unter einer Oberklasse gruppiert werden, kann die Hierarchie sehr tief und komplex werden. Dies führt zu einer schwierigen Datenmodellierung und Wartung, da bei jeder Änderung in der Oberklasse alle untergeordneten Entitäten betroffen sein könnten.

![Daarstellung Generalisierung](https://github.com/user-attachments/assets/a37eb555-cc57-42ef-9738-7166552adf24)

### Auftrag Identifying Relationship
  
🔗 [GitLab Link zu Generalisieren](https://gitlab.com/ch-tbz-it/Stud/m164/-/tree/main/2.Tag)

#### Kardinalitäten 🔢

In relationalen Datenbanken gibt es verschiedene Arten von **Beziehungen**, die Entitäten miteinander verbinden. Diese Beziehungen beschreiben, wie Tabellen miteinander verknüpft sind und wie Daten aus einer Tabelle auf Daten in einer anderen Tabelle zugreifen können. Es gibt drei Hauptarten von Beziehungen:

##### 1. **1:1-Beziehung (One-to-One)**

Eine **1:1-Beziehung** tritt auf, wenn eine Entität in einer Tabelle genau mit einer Entität in einer anderen Tabelle verbunden ist. Das bedeutet, dass für jedes Element der ersten Tabelle genau ein zugehöriges Element in der zweiten Tabelle existiert.

##### Beispiel:
- **Person** 👤 - **Pass** 🛂
  - Jede Person hat genau einen Pass, und jeder Pass ist einer bestimmten Person zugeordnet.
  - **Person** (ID, Name) <-> **Pass** (Passnummer, PersonID)

##### 2. **1:n-Beziehung (One-to-Many)**

Eine **1:n-Beziehung** tritt auf, wenn eine Entität in einer Tabelle mit mehreren Entitäten in einer anderen Tabelle verbunden ist, jedoch jede Entität der zweiten Tabelle nur einer Entität der ersten Tabelle zugeordnet ist. Eine Entität in der ersten Tabelle (die "1"-Seite) kann also viele zugehörige Entitäten in der zweiten Tabelle (die "n"-Seite) haben.

##### Beispiel:
- **Person** - **Kunde** 🛒
  - Eine Person kann mehrere Kundenbeziehungen haben, aber jeder Kunde ist nur einer Person zugeordnet.
  - **Person** (ID, Name) <-> **Kunde** (Kundennummer, Name, PersonID)

##### 3. **n:m-Beziehung (Many-to-Many)**

Eine **n:m-Beziehung** tritt auf, wenn mehrere Entitäten in einer Tabelle mit mehreren Entitäten in einer anderen Tabelle verbunden sind. Das bedeutet, dass für jede Entität der ersten Tabelle mehrere Entitäten in der zweiten Tabelle existieren können und umgekehrt.

##### Beispiel:
- **Student** 👨‍🎓 - **Kurs** 🏫
  - Ein Student kann mehrere Kurse belegen, und ein Kurs kann von mehreren Studenten besucht werden.
  - **Student** (ID, Name) <-> **Kurs** (KursID, Kursname)

Diese Beziehung wird oft durch eine Zwischentabelle (z. B. "Student_Kurs") realisiert, die beide Tabellen miteinander verbindet.

##### Beispiel für eine Zwischentabelle:
- **Student_Kurs** (StudentID, KursID)
  - Diese Tabelle verbindet Studenten und Kurse und ermöglicht die **n:m-Beziehung**.

![Identifying Relationship](https://github.com/user-attachments/assets/f2466b7d-1b62-4f18-8110-e69f41b0f3ae)

### Auftrag DBMS (Datenbank Management System)  
🔗 [DBMS (Datenbank Management System)](https://gitlab.com/ch-tbz-it/Stud/m164/-/tree/main/2.Tag)

#### Zusammenfassung: Datenbank Management System (DBMS)

Ein **Datenbanksystem (DBS)** dient der elektronischen Verwaltung großer Datenmengen und besteht aus zwei Hauptkomponenten:  
1. **Datenbankmanagementsystem (DBMS)** – die Verwaltungssoftware  
2. **Datenbank (DB)** – die gespeicherten Daten  

#### Merkmale eines DBMS  
- **Integrierte Datenhaltung**: Zentrale Speicherung aller Daten zur Vermeidung von Redundanz  
- **Datenbanksprache**: Ermöglicht Abfragen (DQL), Definition (DDL), Manipulation (DML), Zugriffskontrolle (DCL) und Transaktionen (TCL)  
- **Benutzersichten**: Unterschiedliche Ansichten für verschiedene Benutzergruppen  
- **Konsistenzkontrolle**: Sicherstellung der Datenintegrität durch Integritätsbedingungen  
- **Datenzugriffskontrolle**: Schutz vor unautorisierten Zugriffen  
- **Transaktionen**: Änderungen werden als logische Einheiten verarbeitet (Atomarität & Dauerhaftigkeit)  
- **Mehrbenutzerfähigkeit**: Gleichzeitiger Zugriff mehrerer Nutzer durch Synchronisation  
- **Datensicherung**: Wiederherstellung der Datenbank bei Fehlern  

#### Vorteile eines DBMS  
✅ Nutzung von **Standards**  
✅ **Effizienter Datenzugriff** durch optimierte Speicherung und Abfragen  
✅ **Kürzere Entwicklungszeiten** durch zentrale Funktionen  
✅ **Hohe Flexibilität** durch Anpassung an neue Anforderungen  
✅ **Hohe Verfügbarkeit** für parallele Nutzerzugriffe  
✅ **Wirtschaftlichkeit** durch zentralisierte Verwaltung  

#### Nachteile eines DBMS  
❌ **Hohe Anfangskosten** für Hardware und Software  
❌ **Weniger effizient** für spezialisierte Anwendungen  
❌ **Optimierungskonflikte** zwischen konkurrierenden Anforderungen  
❌ **Mehrkosten** für Sicherheit und Synchronisation  
❌ **Hochqualifiziertes Personal** erforderlich  
❌ **Verwundbarkeit** durch Zentralisierung  

Ein reguläres Dateisystem ist in manchen Fällen sinnvoll, z. B. wenn kein Mehrbenutzerzugriff nötig ist oder strenge Echtzeitanforderungen bestehen.


#### DBMS Übersicht - Tabelle

| DBMS            | Hersteller     | Modell/Charakteristik |
|----------------|--------------|----------------------|
| Adabas        | Software AG  | NF2-Modell (nicht normalisiert) |
| Cache         | InterSystems | hierarchisch, “postrelational” |
| DB2           | IBM          | objektrelational |
| Firebird      | –            | relational, basierend auf InterBase |
| IMS           | IBM          | hierarchisch, Mainframe-DBMS |
| Informix      | IBM          | objektrelational |
| InterBase     | Borland      | relational |
| MS Access     | Microsoft    | relational, Desktop-System |
| MS SQL Server | Microsoft    | objektrelational |
| MySQL         | MySQL AB     | relational |
| Oracle        | ORACLE       | objektrelational |
| PostgreSQL    | –            | objektrelational, hervorgegangen aus Ingres und Postgres |
| Sybase ASE    | Sybase       | relational |
| Versant       | Versant      | objektorientiert |
| Visual FoxPro | Microsoft    | relational, Desktop-System |
| Teradata      | NCR Teradata | relationales Hochleistungs-DBMS, speziell für Data Warehouses |


![Mindmap](https://github.com/user-attachments/assets/d30858a1-ce2f-427d-b69b-d8dfb63150d4))