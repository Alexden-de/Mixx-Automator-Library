# JTL-Dienst Watchdog

Periodisch (alle 5 Minuten) pruefen, ob die wichtigen JTL-Wawi-Windows-Dienste
laufen. Falls ein Dienst gestoppt ist, automatisch neu starten und per E-Mail
den Admin informieren.

## Enthaltene Aktionen

1. **ServiceMonitor: JTL-wawi-WorkerService** — startet den Dienst wenn gestoppt.
2. **ServiceMonitor: JTL-DBUpdater** — startet den Dienst wenn gestoppt.
3. **SmtpEmail (RunOnError)** — nur wenn mindestens eine der vorangegangenen
   Aktionen einen Fehler gemeldet hat (Dienst war ausgefallen oder konnte nicht
   gestartet werden), wird eine Info-Mail verschickt.

## Nach dem Import anpassen

- **Dienstnamen** pruefen und ggf. erweitern/umbenennen: die exakten Namen
  variieren leicht zwischen JTL-Versionen. Im ConfigTool bei der Aktion
  auf die "..."-Schaltflaeche klicken und "Nur JTL anzeigen" verwenden, um
  die tatsaechlichen Namen auf dem System zu sehen.
- **E-Mail-Konto** und **Empfaengeradresse** in der SMTP-Aktion setzen.
- **Aktiv** + **Zeitplan aktiv** einschalten, wenn der Job produktiv laufen soll.
- Optional: weitere `ServiceMonitor`-Aktionen fuer andere JTL-Dienste
  hinzufuegen (JTL-Shipping, JTL-Connector fuer Shopware etc.).

## Typische JTL-Wawi-Dienste

| Name                          | Aufgabe                                                |
| ----------------------------- | ------------------------------------------------------ |
| `JTL-wawi-WorkerService`      | Hintergrundverarbeitung (Workflows, Mail-Versand etc.) |
| `JTL-DBUpdater`               | Datenbank-Migration nach Updates                       |
| `JTL-Shipping-Label-Service`  | Versandlabel-Generierung (falls installiert)           |

## Anforderungen

- Mindestversion: Mixx-Automator **v1.0.58** (enthaelt die ServiceMonitor-Aktion).
- Der Mixx-Automator-Dienst laeuft als LocalSystem und hat damit ausreichend
  Rechte, um Windows-Dienste zu starten/stoppen.
