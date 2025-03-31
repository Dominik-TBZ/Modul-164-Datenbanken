# Modul 164 – Datenbanken

## Tag 9

### Auftrag Datensicherung  
🔗 [GitLab Link zur Datensicherung](https://gitlab.com/ch-tbz-it/Stud/m164/-/tree/main/7.Tag?ref_type=heads)

#### Bedeutung der Datensicherung
Datenbanksysteme sind essenziell für Webhosting und Unternehmenssoftware, da sie die Verfügbarkeit und Integrität von Daten gewährleisten. Ein Ausfall oder Verlust kann Websites unzugänglich machen, Anwendungen lahmlegen und zu Vertrauensverlust bei Kunden führen. Häufige Ursachen sind technische Defekte oder Benutzerfehler. Um irreversible Schäden zu vermeiden, sind regelmäßige Backups unerlässlich.

#### Backup-Methoden
Es gibt zwei Hauptkategorien der Datensicherung:

- **Online-Backup**: Die Datenbank bleibt während der Sicherung aktiv.
- **Offline-Backup**: Die Datenbank wird für die Dauer des Backups heruntergefahren.

Zusätzlich unterscheidet man drei Backup-Arten:

1. **Voll-Backup**: Alle Daten werden gespeichert, benötigt jedoch viel Speicherplatz.
2. **Differentielles Backup**: Speichert nur geänderte und neue Dateien seit dem letzten Voll-Backup.
3. **Inkrementelles Backup**: Speichert nur Änderungen seit der letzten Sicherung (egal ob Voll- oder inkrementelles Backup).

#### Backup-Tools
Zur Durchführung von Backups gibt es verschiedene Tools:

- **MySQLDump**: Shell-basierte Lösung für schnelle Voll-Backups.
- **phpMyAdmin**: Export von Datenbanken im SQL-Format (bei großen Datenbanken begrenzt).
- **BigDump**: Import großer Backups, jedoch ohne eigene Backup-Funktion.
- **HeidiSQL**: Windows-Tool ohne Automatisierungsfunktionen.
- **Mariabackup**: Open-Source-Lösung für physische Online-Backups von MariaDB.


