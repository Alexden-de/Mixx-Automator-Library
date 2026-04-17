# Mixx-Automator Library

Oeffentliche Sammlung wiederverwendbarer Jobs fuer [Mixx-Automator](https://github.com/Alexden-de/Mixx-Automator).

## Nutzung

In Mixx-Automator:
1. **Jobs** → **Aus Bibliothek installieren**
2. Gewuenschten Job aus der Liste auswaehlen
3. Parameter anpassen (FTP-Server, Datenbank, etc.)

## Struktur

```
/
├── index.json              ← Uebersicht aller Jobs mit Metadaten
└── jobs/
    └── <job-id>/
        ├── job.mxjob       ← Job-Definition (JSON)
        └── README.md       ← Beschreibung, Anforderungen
```

## Beitragen

Pull Requests willkommen. Fuer einen neuen Job:
1. Neuer Ordner `jobs/<meine-job-id>/`
2. `job.mxjob` (aus Mixx-Automator exportiert)
3. `README.md` mit Beschreibung und Voraussetzungen
4. Eintrag in `index.json` ergaenzen

Secrets (Passwoerter, API-Keys) bitte entfernen vor dem Export.
