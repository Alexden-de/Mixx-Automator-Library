# Wartung: Auto-Update

**Kategorie:** Wartung | **Aktionen:** 1

## Was der Job macht

Prueft **jeden Sonntag um 03:00 Uhr** das konfigurierte Release-Repository auf neue Versionen. Wenn ein Update verfuegbar ist, wird der Installer im Hintergrund heruntergeladen und still ausgefuehrt.

Waehrend der Installation:
- Der Windows-Dienst wird kurz gestoppt (~10 Sekunden)
- Die Programmdateien werden ersetzt
- Der Dienst wird automatisch wieder gestartet
- ConfigTool wird ebenfalls neu gestartet wenn gerade offen

## Anpassen nach dem Import

- **Zeitplan**: Startzeit und Wochentag anpassen wenn 03:00 / Sonntag nicht passt
- **Modus** der Action:
  - `Install` (Standard) — automatisch installieren
  - `Check` — nur pruefen und im Log vermerken (z.B. fuer Folge-Action "E-Mail senden")

## Voraussetzungen

- Gueltige Lizenz fuer den Service
- Der Dienst muss das Update-Repository erreichen koennen (Standard: oeffentlich auf GitHub)
