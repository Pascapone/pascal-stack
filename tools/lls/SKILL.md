---
name: lls
description: Lokale Audio-Transkription und Bildgenerierung (gpt-image-2) über den Local Little Service (LLS) auf diesem Rechner. Nutze den Skill, wenn eine lokale Audiodatei in Text umgewandelt oder ein Bild erzeugt bzw. nach Referenzbildern bearbeitet werden soll.
---

# LLS: Transkription und Bildgenerierung

Der Local Little Service läuft lokal unter `http://127.0.0.1:17843` (Windows und Linux) und nutzt die ChatGPT-Anmeldung des Nutzers. Keine API-Schlüssel nötig; beide Fähigkeiten verbrauchen Abo-Kontingent.

Zuerst prüfen:

```bash
curl -s http://127.0.0.1:17843/health
```

- `authenticated: true` ist Voraussetzung.
- Die Liste `modules` zeigt, ob `image-generation` installiert und bereit ist.
- Fehlt die Anmeldung, den Nutzer bitten, sich auf `http://127.0.0.1:17843/` anzumelden. Auf einem Server vom eigenen Rechner aus tunneln: `ssh -L 127.0.0.2:17843:127.0.0.1:17843 -L 1455:127.0.0.1:1455 SERVER`, dann im eigenen Browser `http://127.0.0.2:17843/` öffnen. `127.0.0.2` vermeidet die Kollision mit einem lokal laufenden LLS; einen anderen Port (z. B. 17845) lehnt LLS mit `local_requests_only` ab.
- Fehlt ein Modul, ist es dort nicht installiert. Keinen Ersatz erfinden.

## Audio transkribieren

Nur absolute lokale Dateipfade, max. 25 MiB; WAV, mp3, m4a, mp4, webm, ogg, flac.

```bash
curl -s -X POST http://127.0.0.1:17843/v1/audio/transcriptions -H "content-type: application/json" -d '{"file_path":"/absoluter/pfad/aufnahme.wav"}'
```

Antwort: `{"text":"…"}`. Automatische Spracherkennung, keine Übersetzung. Unter Windows `C:/…` mit Schrägstrichen schreiben; Backslash-Pfade kommen in Bash fehlerhaft an (`invalid_arguments`).

## Bilder generieren

Kurzlebiger MCP-Client aus dem LLS-Quellordner (Node). Die Anfrage als JSON-Datei übergeben:
- Pflicht sind `prompt` und `output_directory` (absolut).
- Optional: `quality`, `background`, `n`, `output_format`.
- Zum Bearbeiten zusätzlich `referenced_image_paths` (1–5 absolute Pfade).

JSON-Dateien mit einem Schreibwerkzeug anlegen. Manche Shell-Umgebungen verändern Backslashes in Heredocs.

```bash
node "$HOME/Coding/local-little-service/scripts/imagegen-client.mjs" generate --file /pfad/request.json
node "$HOME/Coding/local-little-service/scripts/imagegen-client.mjs" edit --file /pfad/request.json
node "$HOME/Coding/local-little-service/scripts/imagegen-client.mjs" job JOB_ID
```

- `generate`/`edit` liefern sofort eine `job_id`. Mit `job` abfragen, bis `succeeded`/`failed` erscheint; `outputs` enthält die gespeicherten Dateipfade. Ergebnisbild danach tatsächlich ansehen.
- Die Ausgabe hat derzeit faktisch ca. 1,57 Megapixel, z. B. 1254×1254 oder 1536×1024, unabhängig von `size`.
- Fehlgeschlagene, abgelaufene oder abgebrochene Aufträge **nicht automatisch neu senden**: Kontingent kann schon verbraucht sein.
- Höchstens zwei parallele Aufträge, 600 s je Auftrag.
