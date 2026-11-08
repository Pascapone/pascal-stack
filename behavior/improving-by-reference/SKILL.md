---
name: improving-by-reference
description: Nutze diesen Skill, wenn der Nutzer ein in einem Bild oder Screenshot betrachtetes optisches Problem anhand einer KI-erstellten Zielreferenz verbessern, umbauen oder verändern möchte, etwa Terrain, eine 2D-/3D-Szene oder eine Website.
---

# Improving by Reference

Ergebnis sind das unveränderte Original, die vom Nutzer gewählte Zielreferenz und die Angaben zum Wiederherstellen derselben Ansicht. Wer implementiert und ob delegiert wird, bestimmt der bestehende Auftrag.

## Ablauf

1. **Ausgangslage sichern.** Betrachte die vorhandene oder neu erstellte Aufnahme. Halte Problem, Änderungsbereich und zu erhaltende Merkmale fest. Speichere das Original unverändert und erfasse den Ansichtsdatensatz unten.
2. **Vier Vorschläge erstellen.** Erzeuge standardmäßig vier eigenständige Varianten mit dem Image-Gen-Tool, jeweils mit dem Original als Bildreferenz. Zeige unterschiedliche plausible Lösungen. Bewahre Kamera, Ausschnitt, Perspektive und nicht betroffene Merkmale. Prüfe die Bilder visuell: Eine verschobene Kamera oder verdeckende Dekoration behebt keinen Terrain-Übergang. Halte alle Varianten einzeln verfügbar.
3. **Zeigen und wählen lassen.** Erstelle ein Sheet als lokale HTML-Bildübersicht mit Varianten 1–4, kurzen Unterschieden und Zugriff auf Einzelbilder und Original. Öffne es über `open_in_codex`. Frage anschließend mit dem Question-Tool, bevorzugt `request_user_input_async`: „Welche der vier Varianten gefällt dir am besten?“ Biete die vier Nummern an und ermögliche Änderungswünsche. Warte auf die Antwort; lege keine Auswahl selbst fest. Überarbeite bei Bedarf und lass erneut wählen. Fehlt ein Werkzeug, benenne die Grenze und nutze für Darstellung oder Auswahl einen verfügbaren Ersatz.
4. **Ziel übergeben.** Sichere die Auswahl als aktuelle Zielreferenz mit Original und Ansichtsdatensatz für den umsetzenden Agenten oder die eigene Weiterarbeit. Arbeitsdateien gehören ins betroffene Projekt, nicht ins Skill-Verzeichnis.

## Reproduzierbare Ansicht

Nutze ein erweiterbares Format, etwa JSON oder YAML. Feldnamen sind frei; folgende Angaben müssen eindeutig vorhanden sein:

- **Herkunft:** stabile Ansichts-ID, Projekt-/Datei-/Szenenpfad oder URL, betroffener Bereich und Datenstand, soweit verfügbar Revision und Generatorparameter/Seed.
- **Bilder:** zugängliche Pfade/IDs von Original und gewähltem Ziel, Variantennummer, Pixelgröße und gegebenenfalls Zuschnitt als Rechteck im Original.
- **Zustand:** Anwendung und Ansicht (z. B. Unity Scene View/Game View), Ausgabegröße/Seitenverhältnis sowie bildwirksame Beleuchtungs-, Render-, UI- und Animationszustände. Zugängliche gespeicherte Presets dürfen diese beschreiben.
- **Wiederherstellung:** konkrete Werte und kurze Aufrufschritte; Koordinatensystem, Achsen, Einheiten und Rotationsdarstellung ausdrücklich angeben. Ergänze je nach Ansicht:

| Ansicht | Erforderliche Angaben |
| --- | --- |
| 3D | Kamera-ID; Weltposition `x,y,z`; Weltrotation als Quaternion mit Komponentenreihenfolge oder Euler-Winkel mit Einheit und Rotationsreihenfolge; Achsen-/Händigkeitskonvention; perspektivische oder orthografische Projektion; vertikales/horizontales FOV samt Einheit bzw. orthografische Größe samt Definition; Seitenverhältnis und Clipping. Bei abweichender Projektion zusätzlich Projektionsmatrix oder wiederladbares Kamerapreset. |
| 2D | Dokument-/Canvas-/Kamera-ID; sichtbarer Mittelpunkt oder Ursprung; Pan/Scroll und Zoom mit Definition; Rotation; sichtbare Grenzen und Viewportgröße; Ursprung, Achsenrichtung und Welt-/Dokument-/Pixeleinheiten. Bei 2D-Szenen mit Kamera deren Projektion ebenfalls erfassen. |
| Web/UI | URL/Route samt relevanten Parametern; Viewport in CSS-Pixeln, Device-Pixel-Ratio und Browserzoom; Scrollposition `x,y` des Dokuments und betroffener verschachtelter Container mit eindeutigen Selektoren. Für dynamische/virtualisierte Inhalte zusätzlich stabiler Anker mit Offset und Wiederherstellungsschritte; relevante Tabs, aufgeklappte Bereiche, Dialoge sowie Lade-/Animationszustand erfassen. |

Lies Werte aus der Anwendung aus; erfinde keine Kamera-/Scrollwerte aus dem Screenshot. Kennzeichne Fehlendes als unbekannt und ermittle oder erfrage es vor dem Umsetzungsvergleich. Prüfe die Wiederherstellung mit einer erneuten Aufnahme; wesentliche Lücken verhindern bestätigte Reproduzierbarkeit.

## Auftrag für die Umsetzung

Übermittle gemeinsam: **Arbeitsdaten/Fundstelle, Original, gewähltes Ziel, Ansichtsdatensatz, gewünschte Veränderungen und zu erhaltende Merkmale** – auch ohne diesen Chat verständlich und zugänglich.

Arbeite am Projekt auf dieses Ziel hin. Vergleiche neue Aufnahmen derselben Ansicht mit Original und Ziel; Zielwechsel erfordern eine neue Nutzerauswahl. Das KI-Bild ist eine visuelle Vorgabe, kein Machbarkeits- oder Umsetzungsnachweis. Berichte verbleibende Abweichungen konkret.
