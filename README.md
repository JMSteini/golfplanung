# Golf Jahresplanung

Eigenständige, browserbasierte Golf-Jahresplanung mit Vanilla JavaScript und Vite. Die App speichert Daten lokal im Browser-Profil und benötigt zur Laufzeit keinen Server und keinen externen Dienst.

Eine eigenständig öffnbare HTML-Ansicht liegt in `turnierplanung.html`. Die Import- und Exportaktionen sind im Menü **Import / Export** gebündelt. Neben Excel-/CSV-Dateien kann dort eine vollständige, eigenständige HTML-Momentaufnahme exportiert werden. Sie enthält eine Zeitstempel-Version, Erstellungsdatum und alle geladenen Termine unabhängig von Filtern. Die Snapshot-Datei ist nur lesbar und funktioniert offline. Für Excel-Import/-Export wird SheetJS beim Öffnen der Planungs-App von einem CDN geladen; dafür ist eine Internetverbindung erforderlich. Beim Aufruf über einen Webserver versucht die Seite, `../golf-jahresplanung-2027.xlsx` zu laden. Bei direktem Öffnen als `file://` bitte die Arbeitsmappe über **Excel importieren** auswählen.

## Voraussetzungen und Start

- Node.js 18 oder neuer
- npm

Im Projektordner:

```sh
npm install
npm run dev
```

Für einen Produktionsbuild: `npm run build`; Vorschau: `npm run preview`.

## Funktionen

- Jahresübersicht und Monatskalender mit Kategorie- und Kalenderblattfiltern
- Mehrspieler-Turniere: ein gemeinsames Turnier kann mehreren Spielern zugeordnet sein
- alternative Turniere aus der Excel-Kategorie `Altern.`; alle bleiben sichtbar und sind markiert
- Termine erstellen, bearbeiten und löschen, auch über mehrere Tage
- Importierte und manuell angelegte Termine direkt im Kalender anklicken, bearbeiten oder löschen
- automatische Speicherung der aktuellen Version in `localStorage`; importierte Termine bleiben nach einem Neuladen erhalten
- JSON-Sicherung und Wiederherstellung
- CSV-Import und -Export, XLSX-Import sowie Import von Ferien/Feiertagen aus `.ics`

ICS-Dateien werden als allgemeine Termine eingelesen. Ganztägige mehrtägige Einträge berücksichtigen das exklusive `DTEND`-Datum aus dem Kalenderformat. Wiederholte Einträge werden anhand UID oder Datum/Titel erkannt und übersprungen; vor dem Import erscheint eine Bestätigung.

## Excel-Eingabe

Die Jahresplanung unterstützt das 2027-Format mit den Spalten `Von (YYYY-MM-DD)`, `Bis (YYYY-MM-DD)`, `Spieler / Kategorie`, `Turnierkategorie`, `Termin`, `Notiz`, `Alternativgruppe`, `Ort` und `GC`. Die Turnierkategorie, Ortsangabe und der Golfclub bleiben beim Import, Bearbeiten sowie Excel-/CSV-Export erhalten. Datumszellen als Excel-Datum, `YYYY-MM-DD` oder `DD.MM.YYYY` werden unterstützt. Die älteren Tabellen ohne die zusätzlichen Felder bleiben ebenfalls lesbar.

Zeilen mit derselben Zeitspanne und demselben Turniernamen werden zu einem gemeinsamen Turnier zusammengefasst und den beteiligten Spielern zugeordnet. `Allg.` wird als allgemeiner Termin ohne Spieler geführt; `Altern.` kennzeichnet ein separates alternatives Turnier. Vermutete Duplikate gegenüber der bereits gespeicherten Planung werden in der Vorschau übersprungen; sie können über die Checkbox zusätzlich importiert werden. Bei einem erneut erkannten Turnier werden neue Spieler-Zuordnungen an das vorhandene Turnier angefügt.

Im Turnierformular lassen sich mehrere Spieler auswählen und eine optionale Alternativgruppe eintragen. Das Feld `Alternativgruppe` wird nicht automatisch aus der Excel-Datei abgeleitet, weil die Arbeitsmappe keine explizite Gruppenspalte enthält.

Die HTML-Ansicht speichert die aktuelle Planung in `localStorage`. Die Daten bleiben an den Browser und die jeweilige Origin gebunden. Sie werden nicht automatisch zwischen Geräten oder unterschiedlichen lokalen Adressen synchronisiert. Für eine zusätzliche Sicherung Excel oder CSV exportieren.

## Tests

`npm test` führt die Unit-Tests aus. `npm run build` prüft zusätzlich den Produktionsbuild.

## Daten

Es werden keine Beispieldaten automatisch geladen. Die vorhandene Arbeitsmappe im übergeordneten Ordner `new03` wird nicht verändert oder automatisch importiert. Importierte Dateien werden lokal im Browser verarbeitet.
