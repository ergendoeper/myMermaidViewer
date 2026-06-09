# myMermaidViewer

Lokaler Mermaid-Viewer (ohne Build-Prozess) mit Fokus auf `architecture-beta`-Diagramme.

## Überblick

Die Datei [MermaidViewer.html](MermaidViewer.html) enthält eine vollständige Single-File-Webanwendung mit:

- Live-Rendering für Mermaid-Code
- Splittbarem Layout (Editor links, Vorschau rechts)
- Zoom-Funktionen
- Export als SVG, PNG und JPG
- Erweiterter Verarbeitung für `architecture-beta` (insbesondere mehrzeilige Labels)

Verwendete Mermaid-Version: `11.7.0` (CDN).

## Starten

1. [MermaidViewer.html](MermaidViewer.html) im Browser öffnen.
2. Mermaid-Code im linken Editor anpassen.
3. Die Vorschau wird automatisch nach kurzer Verzögerung neu gerendert.

Es sind keine zusätzlichen Abhängigkeiten oder ein lokaler Server erforderlich.

## Bedienung

### Editor und Vorschau

- Der Editor befindet sich links, die Diagramm-Vorschau rechts.
- Mit dem vertikalen Trenner zwischen beiden Bereichen kann die Breite interaktiv verändert werden.
- Die minimale Breite der linken Seite ist begrenzt, damit beide Bereiche nutzbar bleiben.

### Live-Rendering

- Änderungen im Textfeld lösen ein verzögertes Re-Rendering (Debounce, 500 ms) aus.
- Bei leerem Editor wird die Ausgabe geleert.
- Syntaxfehler werden unterhalb des Editors angezeigt.

### Zoom

- `Zoom +` erhöht den Zoom.
- `Zoom -` verringert den Zoom (Untergrenze 0.2).
- `Reset Zoom` setzt auf Faktor 1 zurück.

Hinweis zur Browser-Kompatibilität:

- Standardmäßig wird CSS `zoom` genutzt.
- Für Firefox wird zusätzlich ein Fallback über `transform: scale(...)` mit `transform-origin: 0 0` verwendet.

## Export

Unterstützte Formate:

- SVG
- PNG
- JPG

Exportverhalten:

- Vor dem Export wird temporär auf Zoom 1 zurückgesetzt, damit die Ausgabe in Originalgröße erfolgt.
- Raster-Exporte (PNG/JPG) werden über ein Canvas in höherer Auflösung erzeugt.
- Der Hintergrund wird für Raster-Exporte weiß gefüllt.

Speicher-Workflow:

- Wenn verfügbar, wird `showSaveFilePicker` verwendet (native "Speichern unter"-Dialoge).
- Falls nicht verfügbar oder fehlgeschlagen, erfolgt ein klassischer Download in den Standard-Downloadordner.

## `architecture-beta` Unterstützung

Für `architecture-beta` ist eine zusätzliche Vor- und Nachverarbeitung implementiert:

1. **Pre-Processing:**
	Label-Inhalte in `[...]` werden durch Platzhalter ersetzt, damit Mermaid die Struktur stabil rendert.
2. **Post-Processing im SVG:**
	Platzhalter werden in der finalen SVG-Ausgabe wieder durch Originaltext ersetzt.
3. **Mehrzeilige Labels:**
	Zeilenumbrüche (z. B. `\n`) werden in mehrere `tspan`-Elemente aufgeteilt.
4. **Vertikales Feintuning:**
	Der erzeugte Textblock wird nach unten verschoben, um die Lesbarkeit in der Architekturansicht zu verbessern.

Damit lassen sich komplexere Architektur-Labels robuster darstellen als mit Standard-Rendering allein.

## Aktueller Stand

- Das Projekt ist bewusst minimal gehalten und besteht aktuell aus:
  - [MermaidViewer.html](MermaidViewer.html)
  - [README.md](README.md)
