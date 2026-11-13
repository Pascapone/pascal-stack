# Pascal-Stack

Deutschsprachige Skills für die Zusammenarbeit mit AI-Agents: vier Behavior Skills und ein Werkzeug-Skill.
Die Skilltexte wurden unverändert aus ihrem bisherigen Stand übernommen.

## Skills

| Skill | Zweck |
| --- | --- |
| [assigned-manager](behavior/assigned-manager/SKILL.md) | Explizit übertragene Managerrolle und Koordination von Subagents. |
| [back-on-track](behavior/back-on-track/SKILL.md) | Ziele und Vorgehen prüfen, wenn die Arbeit vom Kurs abkommt. |
| [dialog-modus](behavior/dialog-modus/SKILL.md) | Im laufenden Dialog schnell zusammenarbeiten und Recherche delegieren. |
| [improving-by-reference](behavior/improving-by-reference/SKILL.md) | Visuelle Varianten erstellen, auswählen lassen und eine reproduzierbare Zielreferenz sichern. |

## Werkzeug-Skills

| Skill | Zweck |
| --- | --- |
| [lls](tools/lls/SKILL.md) | Transkription und Bildgenerierung über einen lokal laufenden Local Little Service (LLS). Setzt eine LLS-Installation auf dem Rechner voraus. |

## Installation

Alle Skills global für Codex und die gemeinsame Agent-Skill-Installation installieren:

```sh
npx skills add Pascapone/pascal-stack --global --agent codex universal --yes
```

Verfügbare Skills anzeigen, ohne sie zu installieren:

```sh
npx skills add Pascapone/pascal-stack --list
```

Einen einzelnen Skill installieren:

```sh
npx skills add Pascapone/pascal-stack --global --agent codex universal --skill back-on-track --yes
```

Für andere unterstützte Agents lässt sich `--agent codex universal` durch deren
Agentnamen ersetzen, etwa `--agent claude-code`. Ohne `--agent` bietet die CLI
eine Auswahl an. Ohne `--global` erfolgt eine Installation im aktuellen Projekt.

Die Skills verwenden das offene [Agent-Skills-Format](https://agentskills.io).
Die CLI verwaltet Installation, Herkunft und Updates; die gemeinsame globale
Installation liegt unter `~/.agents/skills/`. Für andere Agent-Verzeichnisse
verwaltet sie die nötigen Verknüpfungen.

Nach dem Veröffentlichen von Änderungen lassen sich die vier Skills mit demselben
Installationsbefehl erneut aus GitHub installieren. Gezielte Updates:

```sh
npx skills update assigned-manager back-on-track dialog-modus improving-by-reference --global --yes
```

## Aufbau und Pflege

```text
tools/
  lls/SKILL.md
behavior/
  assigned-manager/SKILL.md
  back-on-track/SKILL.md
  dialog-modus/SKILL.md
  improving-by-reference/SKILL.md
skills.sh.json
```

`behavior/` ist die Kategorie direkt im Hauptverzeichnis dieses Monorepos.
`skills.sh.json` macht sie für die [Skills-CLI](https://github.com/vercel-labs/skills)
auffindbar. Neue Kategorien können später über zusätzliche `paths` ergänzt werden.

Änderungen gehören in dieses Repository. Anschließend committen, pushen und über
die CLI aktualisieren; installierte globale Kopien nicht separat bearbeiten.
Vor jedem öffentlichen Commit alle betroffenen Dateien auf Zugangsdaten,
personenbezogene Informationen, private Pfade und interne Projektdaten prüfen.
Die Git-Autorenadresse sollte eine öffentliche GitHub-No-Reply-Adresse sein.

## Voraussetzungen und Herkunft

Die Anweisungen sind auf Deutsch. `dialog-modus` nennt konkrete Modellrollen;
`improving-by-reference` benötigt Bildgenerierung sowie eine Anzeige- und
Auswahlmöglichkeit. Die tatsächlich verfügbaren Werkzeuge und Modelle hängen
vom jeweiligen Agent-System ab. Die unveränderten Texte garantieren keine
identischen Fähigkeiten aller Hosts.

`design-md` ist kein Bestandteil dieses Stacks. Dieser fremde Skill wird bereits
im Projekt [google-labs-code/stitch-skills](https://github.com/google-labs-code/stitch-skills/tree/main/plugins/stitch-utilities/skills/design-md)
gepflegt und bleibt eine separate Installation.
