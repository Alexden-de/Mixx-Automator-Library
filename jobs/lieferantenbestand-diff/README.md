# Lieferantenbestand: Diff + Ameise

**Kategorie:** JTL Integration | **Aktionen:** 6

## Was der Job macht

Taeglich um 04:00 Uhr:

1. **FTP Download** — holt die aktuelle Bestandsdatei vom Lieferanten-FTP
2. **Daten-Mapping** — bringt das CSV in ein einheitliches Format (SKU, Bestand) und prueft die Struktur (Pflichtspalten, numerische Werte)
3. **Lieferanten-Bestand Diff** — liest alle Lieferantenartikel aus `tliefartikel` und erstellt eine Diff-CSV, die nur die **geaenderten** Bestaende enthaelt
4. **JTL-Ameise Import** — importiert das Diff ueber die stabile Ameise-Schnittstelle (Schutz vor Schema-Aenderungen der DB)
5. **Lokal archivieren** — Diff-Dateien werden nach Jahr/Monat sortiert archiviert (90 Tage Aufbewahrung)
6. **E-Mail-Benachrichtigung (nur bei Fehler)** — dank `RunOnError=true` wird dieser Schritt auch ausgefuehrt, wenn eine der vorherigen Aktionen fehlschlaegt

## Vorteile gegenueber "kompletter Ameise-Import"

- **Viel schneller** bei grossen Lieferanten: nur geaenderte Artikel werden geschrieben
- **Struktur-Pruefung** im DataMapping erkennt wenn der Lieferant sein CSV-Format aendert und **deaktiviert** den Job automatisch, statt Tausende falsche Updates zu schreiben
- **Schutz vor verstuemmelten Dateien**: `MinRowsThreshold` verhindert den "zero out" Effekt wenn der Lieferant nur einen Teilexport schickt
- **Detaillierter Report** pro Lauf (`*.report.csv`) mit Status Update/Equal/NoInfo

## Anpassen nach dem Import

Je nach deiner Situation:

### Zwingend anpassen
- **FTP Download → ServerId**: deinen FTP-Server auswaehlen
- **DataMapping → MappingRules**: Spalten deines Lieferanten auf SKU / Bestand mappen (wenn der Lieferant z.B. Artikelnummer statt SKU schickt)
- **DataMapping → RequiredInputColumns**: anpassen wenn andere Namen
- **JtlSupplierStockDiff → DatabaseId**: deine JTL-Datenbank
- **JtlSupplierStockDiff → SupplierId**: kLieferant des konkreten Lieferanten
- **AmeiseImport → DatabaseId**: deine JTL-Datenbank
- **AmeiseImport → TemplateId**: ID deiner Ameise-Importvorlage "Lieferantenbestand"
- **SmtpEmail → AccountId / To**: E-Mail-Konto und Empfaenger fuer Alarme

### Optional
- **JtlSupplierStockDiff → UnsentStockBehavior**:
  - `Ignore` (Standard) — Artikel die nicht im CSV sind bleiben unveraendert
  - `SetToValue` — auf festen Wert setzen (z.B. 0 zum Deaktivieren)
- **JtlSupplierStockDiff → MinRowsThreshold**: z.B. 100 — wenn CSV weniger Zeilen hat, Abbruch
- **Zeitplan**: Startzeit anpassen

## Voraussetzungen

- Gueltige Lizenz
- FTP-Server konfiguriert
- JTL-Datenbank-Verbindung konfiguriert
- E-Mail-Konto konfiguriert (fuer Fehlerbenachrichtigung)
- Ameise-Importvorlage "Lieferantenbestand" existiert in JTL-Wawi
