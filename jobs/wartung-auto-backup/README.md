# Wartung: Auto-Backup

**Kategorie:** Wartung | **Aktionen:** 1

## Was der Job macht

Erstellt **taeglich um 02:00 Uhr** ein passwortgeschuetztes Backup aller Konfigurationen (FTP-Server, Datenbanken, E-Mail-Konten, Jobs) und legt es lokal ab unter `C:\Backups\MixxAutomator\{Jahr}\{Monat}\`. Backups aelter als 30 Tage werden automatisch entfernt.

## Wichtig: Passwort setzen

Nach dem Import **muss** ein Passwort gesetzt werden — ohne laesst sich das Backup nicht erstellen.

## Anpassen nach dem Import

- **Passwort** — Pflichtfeld, zum Verschluesseln. Unbedingt sicher aufbewahren.
- **Ausgabeverzeichnis** — Wohin die `.mxbackup`-Dateien geschrieben werden
- **Behalten (Tage)** — Aufbewahrungszeit alter Backups
- **Zeitplan** — Standard taeglich 02:00, nach Bedarf aendern

## Offsite-Backup via FTP

Wenn die Backups zusaetzlich auf einen externen Server sollen, einfach eine zweite Aktion in der Pipeline hinzufuegen:

1. **Auto-Backup** — wie konfiguriert, `AddToPipeline=True`
2. **FTP-Upload** — die erzeugte Backup-Datei wird automatisch an den konfigurierten FTP-Server hochgeladen

Das gibt dir dann 3-fach-Redundanz: live-Daten auf der Maschine, lokales Backup, Offsite-Backup.

## Wiederherstellung

Einstellungen → Backup → **Backup wiederherstellen** — Datei auswaehlen, Passwort eingeben.
