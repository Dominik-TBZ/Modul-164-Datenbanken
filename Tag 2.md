# Modul 164 – Datenbanken

## Tag 2

### Auftrag Generalisierung  
🔗 [GitLab Link zu Generalisieren](https://gitlab.com/ch-tbz-it/Stud/m164/-/tree/main/2.Tag)

#### Was ist Generalisierung?

Die Generalisierung wird verwendet, wenn verschiedene Entitäten ähnliche Attribute oder Eigenschaften teilen. Statt diese Eigenschaften in jeder Entität zu wiederholen, erstellt man eine gemeinsame **Oberklasse** (oder Superklasse), die die gemeinsamen Attribute enthält. Die verschiedenen Entitäten, die diese gemeinsamen Eigenschaften besitzen, werden als **Unterklassen** (oder Subklassen) der Oberklasse betrachtet.

#### Beispiel der Generalisierung:

- **Oberklasse**: `Person`  
  **Attribute**: `Name`, `Geburtsdatum`, `Adresse`
  
- **Unterklassen**:
  - **Mitarbeiter**: `Mitarbeiter-ID`, `Abteilung`
  - **Kunde**: `Kundennummer`, `Kaufhistorie`

In diesem Beispiel teilen die Entitäten `Mitarbeiter` und `Kunde` einige Attribute wie `Name`, `Geburtsdatum`, `Adresse` mit der Oberklasse `Person`. Spezifische Attribute (wie `Mitarbeiter-ID` und `Kundennummer`) werden in den jeweiligen Unterklassen definiert.

#### Vorteile der Generalisierung

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

#### Nachteile der Generalisierung

1. **Komplexität der Abfragen**:
   - In relationalen Datenbanken kann die Generalisierung zu komplexeren Abfragen führen, da man die Daten aus mehreren Tabellen (Oberklasse und Unterklassen) zusammenführen muss. Dies kann die Leistung und Komplexität der SQL-Abfragen erhöhen.

2. **Verlust von Flexibilität**:
   - Eine strikte Generalisierung könnte die Flexibilität einschränken, wenn spätere Änderungen eine differenziertere Modellierung erfordern. Zum Beispiel, wenn einige Attribute nur für eine Unterklasse spezifisch sind, aber aufgrund der Generalisierung in der Oberklasse gespeichert werden müssen.

3. **Fehlende klare Trennung**:
   - Wenn zu viele Entitäten in einer einzigen Oberklasse zusammengefasst werden, könnte es schwieriger werden, die genauen Unterschiede zwischen den Entitäten zu erkennen. In manchen Fällen könnte das Modell zu allgemein werden und die spezifischen Anforderungen der einzelnen Entitäten vernachlässigen.

4. **Overhead bei komplexeren Hierarchien**:
   - Wenn viele Entitäten unter einer Oberklasse gruppiert werden, kann die Hierarchie sehr tief und komplex werden. Dies führt zu einer schwierigen Datenmodellierung und Wartung, da bei jeder Änderung in der Oberklasse alle untergeordneten Entitäten betroffen sein könnten.

![Daarstellung Generalisierung](https://github.com/user-attachments/assets/22ba0dad-cfce-4710-af60-caca1b437d27)