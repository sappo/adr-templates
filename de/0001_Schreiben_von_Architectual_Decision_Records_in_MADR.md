# Schreiben von Architectural Decision Records in MADR

* Status: proposed
* Entscheider: [Lister aller Entscheider]
* Datum: [YYYY-MM-DD]

## Kontext und Problemstellung

Wir möchten Architekturentscheidungen in diesem Projekt aufzeichnen.

Welches Format und Struktur sollen diese aufzeichnen haben?

## Berücksitigte Optionen

* [MADR](https://adr.github.io/madr/) 2.1.0 - Markdown Architectural Decision Records
* [Michael Nygard's template](http://thinkrelevance.com/blog/2011/11/15/documenting-architecture-decisions) - Die erste Inkarnation des Terms "ADR"
* [Sustainable Architectural Decisions](https://www.infoq.com/articles/sustainable-architectural-design-decisions) - Die Y-Statements
* Andere Templates gelistet unter <https://github.com/joelparkerhenderson/architecture_decision_record>
* Formlos - Keine Konventionen für Dateiformat und -struktur

## Entscheidungsergebnis

Gewählte Option: "MADR" 2.1.0, weil

* Implizierte Annahmen sollen explizit gemacht werden.
  Designdokumentation is wichtig, weil es Personen in die Lage versetzt, um Entscheidungen später nachvollziehen zu können.
  Weitere Gründe [A rational design process: How and why to fake it](https://doi.org/10.1109/TSE.1986.6312940).
* Das MADR Format ist schlank und passt in unseren Entwicklungsstil.
* Die MADR Struktur ist Verständlich und unterstützt einfache Benutzung und Wartung.
* Das MADR Projekt ist aktiv.
* Version 2.1.0 ist die neueste als dieses Dokument verfasst wurde.
