# FTP Daily Sync

**Kategorie:** FTP | **Aktionen:** 2

## Was der Job macht

Laedt **taeglich um 06:00** alle neuen `*.csv`-Dateien vom konfigurierten FTP-Server und speichert sie nach Jahr/Monat sortiert lokal ab. Nach dem Download werden die Dateien auf dem FTP in `/archiv` verschoben.

## Anpassen nach dem Import

- **FTP Download → ServerId**: deinen FTP-Server aus der Liste auswaehlen
- **FTP Download → RemotePath / ArchivePath**: Pfade anpassen
- **LocalSave → TargetDirectory**: Ziel-Archivpfad (Platzhalter `{yyyy}/{MM}` erlaubt)
- **Zeitplan**: Startzeit und Wochentage ggf. aendern

## Voraussetzungen

- Mindestens **1 FTP-Server** in Einstellungen konfiguriert
- Schreibrechte auf dem Ziel-Verzeichnis
