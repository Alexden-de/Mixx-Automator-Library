# Report: Bestand-Export

Erzeugt taeglich um 06:00 eine CSV mit dem verfuegbaren Bestand aller
aktiven Artikel auf einem bestimmten Warenlager — ohne Stuecklisten.

## Was genau wird exportiert

Eine Zeile pro Artikel auf dem konfigurierten Warenlager mit:

| Spalte                  | Quelle                                                      |
| ----------------------- | ----------------------------------------------------------- |
| Artikelnummer           | `tArtikel.cArtNr`                                           |
| EAN                     | `tArtikel.cBarcode`                                         |
| HerstellerArtNr         | `tArtikel.cHAN`                                             |
| Artikelname             | `tArtikelBeschreibung.cName` in Wawi-Standard-Sprache       |
| Bestand                 | `fBestand` im Lager                                         |
| GesperrtVerfuegbar      | `fVerfuegbarGesperrt` — fuer Verfuegbarkeit gesperrt        |
| GesperrtAuslieferung    | `fAuslieferungGesperrt` — fuer Auslieferung blockiert       |
| **VerfuegbarerBestand** | Bestand abzgl. beider Sperrmengen                           |
| VK_Netto                | `fVKNetto`                                                  |
| EK_Netto                | `fEKNetto`                                                  |

Die SELECT-Abfrage ist mit Filtern kombiniert:
- `cAktiv = 'Y'` — nur aktive Artikel
- `kStueckliste = 0 OR IS NULL` — keine Stuecklisten-Artikel (deren Bestand
  ergibt sich aus den Komponenten)

## Nach dem Import anpassen

### 1. Datenbank auswaehlen
In der SqlQuery-Aktion bei **"Datenbank"** die JTL-Wawi-Datenbank setzen.

### 2. Warenlager festlegen
Die Abfrage beginnt mit:
```sql
DECLARE @kWarenLager INT = 1;  -- 1 = Standardlager
```
Standard ist `1` (typischerweise "Standardlager"). Wenn mehrere Lager vorhanden,
den richtigen Schluessel rausfinden:
```sql
SELECT kWarenLager, cKuerzel, cName, cLagerTyp FROM dbo.tWarenLager;
```
Gewuenschten Wert in der `DECLARE`-Zeile eintragen.

### 3. Was mit der CSV geschehen soll
Der Job legt die Datei nur im Pipeline-Temp-Verzeichnis ab. Meist will man die
Datei **versenden** oder **hochladen** — Folge-Aktion hinzufuegen:

- **FTP-Upload**: Zu einem Ziel-FTP/SFTP uebertragen
- **E-Mail**: Per SmtpEmail als Anhang verschicken (dazu beim
  SmtpEmailAction `AttachPipelineFiles = True` setzen)
- **Lokal speichern**: LocalSave-Aktion mit Zielpfad

### 4. Aktivieren
- Haken bei **Aktiv** setzen
- **Zeitplan aktiv** fuer taeglichen Lauf
- Start-Zeit anpassen falls nicht 06:00 gewuenscht

## Sicherheit

- Die Query ist **read-only** (SELECT), keine Wawi-Tabellen werden veraendert.
- Die Aktion hat **AllowWrite = False** — Mixx-Automator blockiert jegliche
  INSERT/UPDATE/DELETE-Befehle zusaetzlich als Defense-in-Depth.
- MaxRows steht auf 500.000 — fuer sehr grosse Sortimente ggf. erhoehen.

## Anforderungen

- Mindestversion Mixx-Automator: **v1.0.61** (SELECT-Guard, Platzhalter in
  SQL, MaxRows-Sicherheit).
- Getestet auf JTL-Wawi **1.9.6.5**. Schema-Details fuer neuere Versionen
  gelegentlich pruefen (vor allem `tlagerbestandProLagerLagerartikel`).
