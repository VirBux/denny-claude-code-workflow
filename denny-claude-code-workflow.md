# Auftrag: Arbeitsweise für dieses Projekt einrichten

Richte in diesem Repository eine feste Arbeitsweise ein: Regeln, Doku-Struktur, Aufgabensystem, Hooks und Skills. Sie stammt aus bestehenden Projekten und hat sich dort bewährt. Übernimm sie **sinngemäss**, nicht blind: Alles Fachliche oder Stack-Spezifische ersetzt du durch Platzhalter oder fragst nach.

Platzhalter: `<projekt>` = Projektname in kebab-case, `<docs>` = `<projekt>-docs/`.

---

## 0. Zuerst fragen (eine Runde, Antworten abwarten)

1. **Projektname und Doku-Ordner** – Heisst der Vault `<projekt>-docs/`? Gibt es schon Anforderungen (z. B. `REQUIREMENTS.md`), die zur Spezifikation werden?
2. **Stack** – schon entschieden? Wenn ja: Backend, Frontend, DB, Build-Werkzeuge (bestimmt coding-standards und Hook-Abschnitt B).
3. **Shell für Hooks** – PowerShell (Windows) oder Bash?
4. **Dateinamen** – Empfehlung: englisch, kebab-case, ohne Umlaute; Inhalt Deutsch mit Umlauten. Schweizer Rechtschreibung (ss) oder deutsche (ß)?
5. **Projektspezifische Schutzregel** (harte Regel 4) – was darf nie als Nebeneffekt passieren? Typisch: schreibende Aktionen auf Infrastruktur (Hypervisor, Server, Runner, Produktions-DB), Finanzdaten, Kundendaten.
6. **Skills** – Welche Fremd-Skills sollen rein (Standard: grilling, council, qa-swarm, review-triage)? Wo liegen sie? Claude Code lädt Projekt-Skills **nur aus `.claude/skills/`**.
7. **Modelle** für Agenten-Skills – Empfehlung: Sonnet für Breite, Opus für riskante Teile, kein Fable, kein Haiku für Reviews.
8. **Git und GitHub** – existiert das Repo schon? Remote vorhanden? Self-hosted Runner?

Weitere Widersprüche in Vorgaben werden ebenfalls gefragt, nicht still aufgelöst.

## 1. Die vier Prinzipien

1. **Wenig im Kontext, viel im Repo.** Dauerhaft geladen ist nur `CLAUDE.md`: harte Regeln plus eine Wegweiser-Tabelle. Alles andere wird gezielt nachgeladen.
2. **Wissen gehört ins Repo, nicht ins Memory.** Das lokale Claude-Memory bleibt leer. Wer klont, hat den vollständigen Stand – auf jedem Gerät.
3. **Regeln, die sich selbst prüfen.** Was maschinell prüfbar ist, läuft als Hook, nicht als Bitte im Prompt.
4. **Grosse Dateien werden gegrept, nicht am Stück gelesen** (ab etwa 500 Zeilen).

## 2. `CLAUDE.md` – Aufbau (Grenze: 4096 Bytes)

Wird `CLAUDE.md` per `/init` angelegt, muss sie mit dem von `/init` vorgegebenen Kopf beginnen; danach folgt dieses Gerüst.

```markdown
# <Projekt>

<Ein Satz, was das ist.> Stand: Aufbau. Massgeblich ist `<docs>/REQUIREMENTS.md`; bei Widersprüchen gilt sie.

## Harte Regeln

1. **Sprache Deutsch** (Antworten, Kommentare, Commits, Doku). Bezeichner im Code und Dateinamen englisch.
2. **`main` ist geschützt.** Kein Commit, Push oder Merge auf `main`; gearbeitet wird auf `development` oder `feature/<kurzname>`. Hook blockiert.
3. **Secrets nicht lesen, nicht schreiben** (`.env`, Keys, Zugangsdaten). Hook blockiert.
4. **<Schutzregel aus Frage 5 – nie als Nebeneffekt einer anderen Aufgabe.>**
5. **Wissen gehört ins Repo, nicht ins Memory.**
6. **Nichts auf Verdacht nachladen.** Dauerhaft geladen ist nur diese Datei.
7. **Grosse Dateien (ab ~500 Zeilen) greppen**, dann gezielt lesen.
8. **Jede neue Datei unter `<docs>` bekommt eine Zeile** im Wegweiser oder in der Übersicht ihres Ordners. Ausgenommen: Zustandsordner `todos/*/` und `plans/*/`.
9. **`<docs>` ist ein Obsidian-Vault:** Links als `[[dateiname]]`, Dateinamen kebab-case.
10. **Tests nie anpassen, nur damit sie grün werden.** Im Zweifel nachfragen.
11. **Fertig heisst geprüft.** Manuell Prüfbares kommt nach `todos/manual-e2e-testing.md`.
12. **Unsicherheit mit echter Abwägung:** fragen „Soll ich den Council fragen?“. Nicht ungefragt starten.

## Wegweiser – Aufgabe → Datei

| Aufgabe | Datei |
|---|---|
| Fachliche und technische Anforderungen | `REQUIREMENTS.md` |
| Code-Stil, Schichten, Tests, Logging | `coding-standards.md` |
| Ablauf einer Aufgabe, Branches, Commits, PR | `collaboration-rules.md` |
| Stack, Setup, Befehle, lokal starten/testen | `development-environment.md` |
| Welcher Skill wann | `skills-overview.md` |
| Architekturentscheidung begründen | `adr/` → `[[adr-overview]]` |
| Falle, die schon Zeit gekostet hat | `lessons/` → `[[lessons-overview]]` |
| Wie eine grössere Aufgabe umgesetzt wird | `plans/` → `[[plans-overview]]` |
| Was zu tun ist | `todos/` → `[[README]]` |
| Was ein Mensch von Hand prüfen muss | `todos/manual-e2e-testing.md` |
| Ablauf, den ein Mensch abarbeitet | `runbook/` → `[[runbook-overview]]` |
| Regeln, die als Hook laufen | `hooks-overview.md` |
| Subagenten und ihr Zuschnitt | `agents-overview.md` |

## Ablauf, kurz

Aufgabe verstehen → Datei aus dem Wegweiser öffnen → bei Lücken `grilling` → bei grösseren Sachen Plan unter `plans/open/` und bestätigen lassen → umsetzen → prüfen → committen → „PR nach `development`?“ → bei Ja `review-triage` (startet `qa-swarm`) → Todo/Lessons nachziehen.
```

Weitere fachliche Dateien (Datenmodell, Komponentenkarte, Ereigniskatalog …) kommen erst, wenn das Projekt sie braucht – jede mit Wegweiser-Zeile.

## 3. Doku-Konventionen

- Frontmatter in jeder Datei: `titel`, `zweck` (ein Satz). Todos, Pläne und ADRs haben ein eigenes Gerüst.
- Dateinamen englisch, kebab-case, ohne Umlaute. Inhalt Deutsch mit korrekten Umlauten.
- Querverweise `[[dateiname]]`; ein Link auf eine noch fehlende Datei ist erlaubt und markiert eine Lücke.
- Keine gleichnamigen Dateien in verschiedenen Ordnern (Wikilinks werden mehrdeutig).
- **Widersprüchliche Doku wird nicht stillschweigend aufgelöst.** Nachfragen; im Zweifel gilt `REQUIREMENTS.md`.

## 4. `collaboration-rules.md` – Inhalt

Führt die harten Regeln aus, ohne sie zu wiederholen.

### Ablauf einer Aufgabe

**Klein** (ein Handgriff): direkt umsetzen, prüfen, fertig. Kein Plan, kein Todo.

**Grösser** (mehrere Dateien, offene Entscheidungen, mehr als eine Sitzung): Planung und Umsetzung in **getrennten Sitzungen**.

1. **Lücken schliessen** mit `grilling`; Ergebnisse in REQUIREMENTS oder Plan.
2. **Plan schreiben** unter `plans/open/<name>.md` – für eine Session ohne Vorkontext.
3. **Bestätigen lassen.** Erst danach Code; Plan per `git mv` nach `in-progress/`.
4. **Umsetzen.** Der Plan wird mitgeführt, nicht nachträglich passend gemacht.
5. **Prüfen.** Automatisierte Tests grün. Manuelles nach `todos/manual-e2e-testing.md`, **thematisch sortiert** – wie ein Einkauf nach Regalen, damit man beim Abarbeiten nicht zwischen Seiten hin und her springt.
6. **Committen**, dann fragen: „Soll ich einen PR nach `development` stellen?“ Bei Ja: PR + `review-triage`.
7. **Nachziehen.** Todo und Plan per `git mv` nach `done/`, Ergebnisbericht. Falle gefunden → `lessons/`.

**Fertig heisst:** Code steht, alle automatisierten Tests geschrieben und grün. Eine Aufgabe darf abgeschlossen sein, während ihre manuelle Prüfung noch offen ist – nur dann.

### Arbeitsregeln

- Teständerungen brauchen eine ausdrückliche Begründung, im Zweifel eine Rückfrage.
- Der Stack ist gesetzt, sobald entschieden. Kein alternatives Framework, keine neue Bibliothek ohne Frage.
- Nicht ungefragt refaktorieren; keine Abstraktion, die die Aufgabe nicht braucht.
- Externe Dienste richtet der Mensch ein. Lesen ja, schreibende Aktionen in fremden Dashboards nein. Deployments löst der Mensch aus.
- **Annahmen sofort persistieren:** Architekturentscheidungen in REQUIREMENTS bzw. ADR, Detailannahmen in Todo oder Plan. Subagenten ausserhalb von Skills werden ausdrücklich beauftragt, Annahmen zu dokumentieren.
- **Listen und Pläne sind Dateien im Repo**, keine Sitzungsartefakte.
- **Council vorschlagen, nicht starten.**

### Branches und Commits

- Gearbeitet wird auf `development` oder `feature/<kurzname>`. `main` nur per Release-Merge durch den Menschen.
- Commit-Messages Deutsch, Imperativ, eine Zeile Betreff, bei Bedarf ein Absatz Begründung, Aufgabennummer am Ende: `Lege Projektgerüst an (p1-001)`.
- PRs immer nach `development`.

### Abgrenzungen, die verschwimmen

- **plans/ gegen todos/:** Todo = *was* und woran man „fertig“ erkennt. Plan = *wie*, nur wenn nötig.
- **lessons/ gegen runbook/:** Lesson = Falle, in die man getappt ist. Runbook = Ablauf, den ein Mensch wiederholt abarbeitet.
- **adr/ gegen REQUIREMENTS:** REQUIREMENTS = was gilt. ADR = warum, inklusive verworfener Alternativen.

## 5. Aufgabensystem `todos/` (mit `todos/README.md`)

**Eine Datei je Aufgabe, der Zustand steckt im Ordner** – keine zentrale Liste (parallele Sessions in Worktrees erzeugen sonst Merge-Konflikte).

```
todos/
├── open/  ├── in-progress/  ├── done/  └── deleted/
```

- Kein Statusfeld in der Datei; der Ordner ist die Wahrheit. Kein Inhaltsverzeichnis.
- Dateiname `p<prio>-<nummer>-<slug>.md`, z. B. `p1-001-project-scaffold.md`. Priorität p1 hoch bis p3 niedrig. Nummer dreistellig, fortlaufend über alle Ordner, nie neu vergeben:
  `ls <docs>/todos/*/p*.md | sed 's|.*/p[0-9]-\([0-9]\{3\}\)-.*|\1|' | sort -n | tail -1`
- Priorität ändern = umbenennen, Nummer behalten. **Verschieben immer mit `git mv`.**
- Gerüst: Frontmatter `titel`, `prioritaet`, `braucht`, `plan`, `anforderungen`; Abschnitte Kontext, Akzeptanzkriterien (Checkboxen), Betroffene Dateien.
- Für eine Session ohne Vorkontext geschrieben; beim Abschluss ein **Ergebnisbericht** (was gebaut, Entscheidungen, Fallen, Anzahl grüner Tests).

## 6. Pläne `plans/` (mit `plans/plans-overview.md`)

- Dieselben Zustandsordner wie Todos (`open/in-progress/done/deleted`), dieselbe Begründung, **keine** Tabelle aller Pläne.
- Für eine Session ohne Vorkontext: jede Entscheidung mit Datum und Begründung (mit Zahlen, wo es welche gibt); nichts verweist auf ein Gespräch; Voraussetzungen als `[[wikilink]]`.
- Gerüst: Frontmatter `titel`, `zweck`, `todo`; Abschnitte Stand (Pflicht ab in-progress), Ziel, Gehört dazu, Gehört nicht dazu, Entscheidungen, Betroffene Dateien, Fertig wenn, Ergebnisbericht.
- Beim Verschieben Pfadverweise im Repo nachziehen (auch in Code-Kommentaren).

## 7. Lessons `lessons/` (mit `lessons/lessons-overview.md`)

- Eine Datei je Bereich (`lessons/shell.md`, `lessons/<framework>.md`), Vorfälle als Abschnitte.
- Eintrag: `## Falle` → **Symptom**, **Ursache**, **Regel**. Kurz und handlungsfähig.
- Maschinell prüfbar → zusätzlich in den Stop-Hook.
- Darf als Code-Kommentar an der betroffenen Stelle stehen, wenn das einen Rückfall verhindert.
- Übersicht: `| Bereich | Datei | Worum es geht |`.

## 8. Runbook `runbook/` (mit `runbook/runbook-overview.md`)

Abläufe, die ein Mensch ausführt (Zugangsdaten, Entscheidung, etwas ausserhalb des Repos). Nummerierte Schritte, ohne Vorwissen abarbeitbar, Abschnitt „Prüfen, dass es steht“. Mit den Runbooks muss sich die Infrastruktur **lückenlos** nachbauen lassen. Übersicht `| Ablauf | Datei | Wann |`. Erstes Runbook direkt beim Einrichten: `repository-setup.md` (hooksPath, `main` anlegen, GitHub-Repo, Standard-Branch `development`, Branch-Schutz, Runner).

## 9. `todos/manual-e2e-testing.md`

Nach Bereichen/Seiten gruppiert, je Bereich eine Tabelle `| Was | Wie prüfen | Seit |`. Jeder Eintrag nennt konkreten Ablauf und erwartetes Ergebnis, sagt, was schon automatisch geprüft ist. Geprüftes wird gestrichen.

## 10. ADR `adr/` (mit `adr/adr-overview.md`)

Architecture Decision Records ergänzen REQUIREMENTS und Lessons: begründen Architekturentscheidungen samt verworfener Alternativen. Dateiname `adr-0001-<slug>.md`. Nie umschreiben – eine neue Entscheidung ist ein neuer ADR, der alte wird „abgelöst durch“. Gerüst: Kontext, Entscheidung, Alternativen, Konsequenzen. Council-Ergebnisse zu Architekturfragen landen hier.

## 11. `coding-standards.md` – Grundgerüst

- **Schichten Pflicht:** Controller/Resource → Service → Repository (bei DDD mit Domain-Schicht). Geschäftslogik nur in Service/Domain, DB-Zugriff nur im Repository, Entitäten verlassen die Domäne nicht (DTOs, eingehend validiert und ausgehend getrennt). Transaktionen im Service. Fehler als Exceptions, ein zentraler Mapper.
- **Stil:** kleine Schritte; nicht ungefragt refaktorieren; Kommentare nur für das *Warum*. Bezeichner englisch, alles Menschenlesbare deutsch (UI über i18n).
- **Zeit** über injizierte Zeitquelle, nie `now()` im Fachcode.
- **Tests:** Unit/Integration/E2E getrennt ausführbar; Namen `should_<Erwartung>_when_<Bedingung>`; ARRANGE/ACT/ASSERT mit Warum im ARRANGE-Kommentar; Integration gegen echte Infrastruktur; handgeschriebene Doubles statt Mock-Frameworks; kein abgeschalteter Test; E2E zusätzlich, kein Aufruf verlässt den Rechner; Randfälle testen.
- **Logging:** Stufen aus Konfiguration; Dev DEBUG für eigene Pakete, Prod WARN/ERROR; keine Konsolenausgabe; keine Personendaten im Log.
- **Datenbank:** Migrationen nie rückwirkend ändern.
- **Frontend:** keine harten Farben ausserhalb der Token-Datei; globales CSS nur als Imports, Thematisches in Partials; kein `console.log`.
- Regeln, die der Stop-Hook prüft, mit **[Hook]** markieren.

## 12. Hooks – Regeln, die sich selbst prüfen

Skripte unter `.claude/hooks/`, verdrahtet in `.claude/settings.json`, beschrieben in `<docs>/hooks-overview.md`. Jedes Skript hat einen Kopfkommentar **Prüft / Warum / Wirkung**. Gemeinsames in `hook-common.ps1` (dot-sourcing).

| Hook | Läuft bei | Prüft | Wirkung |
|---|---|---|---|
| `block-main-write` | PreToolUse `Bash\|PowerShell` | Push nach main, `push --all/--mirror`, `branch -f/-M/-D main`, `update-ref` main, commit/merge/push/rebase während main ausgecheckt (auch `checkout main && commit` im selben Befehl) | blockiert (Exit 2) |
| `block-secrets` | PreToolUse `Read\|Edit\|Write\|NotebookEdit\|Glob\|Grep\|Bash\|PowerShell` | `.env*` (ausser `.example/.sample/.template`, auch Glob-Muster wie `.env*`), `secrets/`, `credentials*`, `*.pem/key/p12/pfx/jks`, private SSH-Keys – nur als Pfad | blockiert (Exit 2) |
| `rule-check` | Stop | siehe unten | meldet zurück (Exit 2) |
| `hooks-test` | von Hand | jede Prüfung einmal auslösend, einmal schweigend | Exit 1 bei Fehlschlag |

`settings.json` (PowerShell-Variante):

```json
{
  "$schema": "https://json.schemastore.org/claude-code-settings.json",
  "hooks": {
    "PreToolUse": [
      { "matcher": "Bash|PowerShell",
        "hooks": [{ "type": "command", "command": "powershell -NoProfile -ExecutionPolicy Bypass -File \"$CLAUDE_PROJECT_DIR/.claude/hooks/block-main-write.ps1\"" }] },
      { "matcher": "Read|Edit|Write|NotebookEdit|Glob|Grep|Bash|PowerShell",
        "hooks": [{ "type": "command", "command": "powershell -NoProfile -ExecutionPolicy Bypass -File \"$CLAUDE_PROJECT_DIR/.claude/hooks/block-secrets.ps1\"" }] }
    ],
    "Stop": [
      { "hooks": [{ "type": "command", "command": "powershell -NoProfile -ExecutionPolicy Bypass -File \"$CLAUDE_PROJECT_DIR/.claude/hooks/rule-check.ps1\"" }] }
    ]
  }
}
```

**`rule-check` Abschnitt A – stack-unabhängig, sofort bauen:**

1. `CLAUDE.md` > 4096 Bytes.
2. Lokales Memory nicht leer (`~/.claude/projects/<pfad-kodiert>/memory/`: `.md` ausser `MEMORY.md`, oder Listeneinträge in `MEMORY.md`). Kodierung: jedes Zeichen ausser `[A-Za-z0-9]` wird zu `-` (Umlaute auch). Für Tests per Umgebungsvariable überschreibbar.
3. Verwaiste Doku: jede `.md` unter `<docs>` muss mit ihrem Namen in `CLAUDE.md`, `collaboration-rules.md`, `todos/README.md` oder einer `*overview*`-Datei vorkommen (nicht nur in sich selbst). Ausnahmen: Zustandsordner von `todos/` und `plans/`, `.obsidian/`.
4. Zugangsdaten im Git-Index (gleiche Pfadlogik wie `block-secrets`).
5. Offene Änderungen, während `main` ausgecheckt ist.

**Abschnitt B – stack-spezifisch, sobald der Stack steht:** nur geänderte Dateien (`git status --porcelain --untracked-files=all`), nur Text, kein Build. Muster aus den Coding-Standards (Konsolenausgabe, `now()`, abgeschaltete Tests, Testnamen, harte Farben, geänderte Migration). Testbericht älter als letzte Quelländerung → erinnern. Jedes Muster braucht eine Regel in `coding-standards.md`.

**Eigenschaften für jeden Hook (hart erarbeitet):**

1. `stop_hook_active` → sofort Exit 0.
2. Jede Prüfung in eigenem `try`; eine kaputte Prüfung blockiert nie.
3. Kein Sicherheitsversprechen, sondern Auffangnetz – steht so in der Doku; der echte Schutz ist Branch-Protection auf GitHub.
4. **Jede Prüfung braucht einen auslösenden Test.** Eine stumme Prüfung ist von einer kaputten nicht zu unterscheiden.
5. Vor Mustersuche Kommentare und Zeichenketten entfernen (bei `block-main-write`: Anführungszeichen-Inhalte, damit Commit-Messages nicht auslösen).
6. Secret-Hook trifft nur Pfade (Basisname vor Endung Pflicht), Here-Doc-Rümpfe und PowerShell-Here-Strings vorher entfernen, `id_*.pub` ausnehmen.
7. Kein teurer Stop-Hook – Zeitstempel statt Build.
8. **PowerShell 5.1:**
   - Stdin **als UTF-8-Byte-Stream** lesen: `New-Object IO.StreamReader([Console]::OpenStandardInput(), (New-Object Text.UTF8Encoding($false)))`. `[Console]::In` liefert leer, sobald `InputEncoding` gesetzt wurde; `$input` verstümmelt Umlaute im Pfad → `git -C` scheitert still.
   - `[Console]::OutputEncoding` nur in `try` setzen (ohne Konsole wirft es).
   - Hilfsfunktion nie wie das Programm nennen (`function git` → Rekursion); Programm über `Get-Command -CommandType Application` auflösen.
   - Programmausgabe in Funktionen mit `| Out-Host`, sonst landet sie im Rückgabewert.
   - `.ps1` ohne Umlaute im Code (oder mit BOM). `.gitattributes`: `*.ps1 eol=crlf`, `.githooks/* eol=lf`.
   - Testläufer startet Hooks per `System.Diagnostics.Process` mit umgeleitetem Stdin (UTF-8-Bytes) und eigener `CLAUDE_PROJECT_DIR`; Testfälle in Wegwerf-Repos unter `%TEMP%`.
9. Fehlalarm-Muster entschärfen = bewusste Entscheidung → Commit-Text.

**Git-Hook als zweite Ebene:** `.githooks/pre-push` ruft `scripts/verify.sh` (alle Tests + Produktionsbau) und blockiert bei Fehlern; fehlt das Skript noch, Hinweis und durchlassen. Reine Lösch-Pushes (Null-SHA) durchlassen. Verdrahtung je Klon: `git config core.hooksPath .githooks`.

## 13. Skills `.claude/skills/<name>/SKILL.md`

**Ort:** ausschliesslich `.claude/skills/`. Tools wie `npx skills` installieren nach `.agents/skills/` – dort findet Claude Code sie nicht. Codex-Metadaten (`agents/openai.yaml`) sind für Claude überflüssig.

### Zeiger-Skills (eigene, kein Wissensspeicher)

- **`task-workflow`** – „Eine grössere Aufgabe planen, starten oder abschliessen …“ → `collaboration-rules.md`, `plans-overview.md`, `todos/README.md`, `manual-e2e-testing.md`, `skills-overview.md`.
- **`project-docs`** – „Etwas in der Projektdoku ablegen, aktualisieren oder wiederfinden.“ → Wegweiser, Abgrenzungen, Konventionen; erinnert an die Wegweiser-Zeile. (Nicht `docs` nennen – kollidiert leicht mit globalen Skills.)
- **`lesson`** – „Einen Fallstrick festhalten …“ → `lessons-overview.md`; eine Datei je Bereich; maschinell prüfbar → Hook.

Jeder endet mit: „Öffne die genannten Dateien, statt aus dem Gedächtnis zu antworten.“

### Fremd-Skills (Standardsatz)

| Skill | Quelle | Wofür | Anpassung beim Einrichten |
|---|---|---|---|
| `grilling` + `grill-me` | mattpocock/skills | Anforderungen in Interview-Runden schärfen, Lücken schliessen | Hinweisblock: Deutsch, REQUIREMENTS vorher greppen, Ergebnisse sofort in REQUIREMENTS/ADR/Plan persistieren |
| `council` | Karpathys LLM Council | Beratung bei Unsicherheit: 5 Berater parallel → anonyme Gegenprüfung → Vorsitzender | Name = Ordnername, Deutsch, **nur auf Zustimmung** (Claude fragt „Soll ich den Council fragen?“), Berater/Prüfer Sonnet, Vorsitz Opus, keine HTML-Ausgabe, Architekturergebnis als ADR |
| `qa-swarm` | pauldambra/dotfiles | PR-Review: Router (Sonnet) über den ganzen Diff, Delegation riskanter Hunks (Opus) an Linsen `qa-team`, `security-audit`, `reviewer`, `xp-reviewer`; Inline-Kommentare + eine Sticky-Zusammenfassung | **Flach statt verschachtelt** (siehe unten), Linsen **lokal** unter `references/` neu schreiben (Stack-spezifisch), `coding-standards.md` als Pflichtlektüre, Basis `development`, Pflicht-Delegation bei Auth/Migration/Geld/Personendaten/CI/>400 Zeilen, Bot-Kennung auf Deutsch, Review per `gh api … --input <json>` posten, `event: COMMENT` |
| `review-triage` | pauldambra/dotfiles | Einstieg nach jedem PR: startet qa-swarm, sortiert Threads (umsetzbar → fixen und pushen; Kleinigkeit → antworten und schliessen; unklar → Autonomie-Leiter; menschlich → nie anfassen); äussere Schleife max. 3 Runden; Bericht mit offenen Punkten sortiert für den Menschen | `paul-pair` durch lokale `references/autonomy-ladder.md` ersetzen (Pflicht-Rückfrage bei Architektur, Schema, Auth, Geld, Abhängigkeiten, Infra), `pr-shepherd`/`ci-shepherd`/`stamphog` entfernen, Fixes nur bei grünen Tests pushen |

### qa-swarm flach aufbauen – eine Persona, ein Agent

Das Original sieht zwei Ebenen vor: die Hauptsession startet vier Reviewer parallel, und einer davon (`qa-team`) startet intern nochmals eigene Spezialisten (Security, Datenbank, Performance …). Diese zweite Ebene wird hier **entfernt**, aus zwei Gründen:

1. Sie ist technisch unzuverlässig (siehe Stolperfallen) und degradiert im Fehlerfall still zu einem einzigen geteilten Kontext.
2. Sie verdichtet Befunde zweimal. Jede Verdichtung ist eine Stelle, an der Zeilennummern verrutschen, Formulierungen geglättet und Details aus dem Gedächtnis ergänzt werden.

**Aufbau stattdessen:**

- Der Router läuft in der Hauptsession und liefert nur die Zuordnung Hunk → Linse. Er delegiert nicht selbst.
- Die Hauptsession ruft **je Linse genau einen Agenten** auf – auch für die Spezialisten, die im Original innerhalb von `qa-team` liefen. `qa-team` als Zwischenebene entfällt und wird durch seine Einzellinsen ersetzt.
- Jede Linse bekommt ihr Modell ausdrücklich gesetzt (nicht vom Aufrufer geerbt): Opus für Auth, Migrationen, Geld, Personendaten; Sonnet für Stil, Benennung, Beobachtbarkeit.
- **Rückgabe knapp halten, Befunde ins Repo.** Jede Linse schreibt ihre Befunde nach `.qa-review/<runde>/<linse>.md` und gibt an die Hauptsession nur eine Zeile zurück: Pfad, Anzahl Befunde, höchste Schwere. Die Hauptsession liest danach gezielt die Dateien, die sie für Deduplizierung und Verdikt braucht. Das spart Kontext wie das Nisten, aber ohne die zweite Verdichtung – und der Rohbefund bleibt nachlesbar, statt nur als Zusammenfassung zu existieren.
- **Ausgefallene Linsen werden benannt.** Läuft eine Linse nicht, steht das im Bericht und in der Sticky-Zusammenfassung. Ein stiller Ausfall ist von einer sauberen Runde nicht zu unterscheiden – dieselbe Logik wie bei den Hooks.

**Sicherheitsprüfung jedes Fremd-Skills vor Übernahme:**

1. Versteckte Zeichen suchen (Zero-Width `U+200B–200F`, Bidi `U+202A–202E`, `U+2060–2064`, BOM mitten im Text, Tag-Zeichen `U+E0000–E007F`).
2. Nach Nachladen fremder Inhalte suchen (`curl`, `wget`, `http`, MCP-Aufrufe auf fremde Stores, „fetch its body and follow it“). **Das Original von qa-swarm/review-triage lud Reviewer-Texte zur Laufzeit aus dem PostHog-Skill-Store und führte sie aus** – Supply-Chain-Risiko. Alles, was ein Skill als Anweisung ausführt, muss lokal und versioniert sein.
3. Schreibende Aktionen nach aussen auflisten (Kommentare posten, pushen, Threads schliessen) und gegen die Branch-Regeln prüfen.
4. Verweise auf nicht vorhandene Skills auflösen: selbst schreiben (lokal) oder entfernen – und dem Menschen melden.
5. Ergebnis in `skills-overview.md` festhalten (Datum, Befunde, Anpassungen).

**Stolperfallen:**

- **Verschachtelte Subagenten sind unzuverlässig – Hierarchie flach halten.** Offiziell dürfen Subagenten eigene Subagenten starten (Tiefe bis 5, `Agent` muss im `tools`-Frontmatter stehen). In der Praxis wird das `Agent`/`Task`-Werkzeug je nach Subagenten-Typ und Version still weggefiltert; der Fan-out fällt dann **lautlos** auf einen einzigen Kontext zurück, in dem ein Agent alle Personas nacheinander abarbeitet. Genau das soll nicht passieren: geteilter Kontext heisst gegenseitige Beeinflussung und mehr Halluzinationen, und der Ausfall ist von aussen nicht sichtbar. Deshalb gilt hier: **mehrstufige Abläufe (Council, Review-Linsen) startet ausschliesslich die Hauptsession, ein Aufruf je Persona.** Kein Subagent versucht, weitere Subagenten zu starten.
- `disable-model-invocation: true` verhindert, dass Claude den Skill nach einem „Ja“ des Nutzers selbst startet – nur für reine Aliase (`grill-me`) verwenden.
- Lokale Anpassungen werden beim Update über `skills-lock.json` überschrieben.

### `skills-overview.md`

Grafik (Mermaid-Flowchart: Idee → grilling → REQUIREMENTS → Council? → ADR → task-workflow → Umsetzung → Tests/Commit → PR? → review-triage ⟲ qa-swarm → Merge durch Mensch) plus Tabellen: Stadium/Skill/Wann/Start, Modelle, Herkunft/Anpassung, Sicherheitsprüfung. Zusätzlich als PDF (z. B. HTML mit marked + mermaid von jsdelivr, gedruckt mit `msedge --headless=new --print-to-pdf --virtual-time-budget=20000`).

## 14. Subagenten `<docs>/agents-overview.md`

Anfangs **bewusst keiner**. Die Übersicht liegt im Vault, **nicht** unter `.claude/agents/` – dort wird jede `.md` als Agent-Definition gelesen. Ein Subagent lohnt sich nur, wenn ein Bereich ein eigenes Kontextfenster verdient (viele Dateien, Ergebnis in wenigen Sätzen). Tabelle möglicher Kandidaten mit Voraussetzung. Anlegen: `.claude/agents/<name>.md` mit `name`, `description` (beschreibt den **Anlass**), `tools`, optional `model`; danach Zeile in der Übersicht.

## 15. Reihenfolge beim Einrichten

1. Fragen aus Schritt 0 stellen, Antworten abwarten.
2. `git init -b development` (bzw. auf `development` wechseln). `main` entsteht nach dem ersten Commit per `git branch main` (Runbook).
3. `CLAUDE.md` und `<docs>` mit `REQUIREMENTS.md` (vorhandene Anforderungen übersichtlich und orthografisch korrekt umschreiben: nummerierte Anforderungs-IDs, Tabellen, Abschnitt „Offene Fragen“), `collaboration-rules.md`, `coding-standards.md`, `development-environment.md`, `hooks-overview.md`, `skills-overview.md` (+ PDF), `agents-overview.md`, `todos/README.md`, `todos/manual-e2e-testing.md`, `plans/plans-overview.md`, `lessons/lessons-overview.md`, `runbook/runbook-overview.md` + `runbook/repository-setup.md`, `adr/adr-overview.md` – alle mit Frontmatter.
4. Zustandsordner `todos/` und `plans/` (`open`, `in-progress`, `done`, `deleted`) mit `.gitkeep`.
5. Hooks + `settings.json` + `hooks-test`; **Test ausführen und Ergebnis zeigen.**
6. Skills nach `.claude/skills/`, Sicherheitsprüfung, Anpassungen.
7. `.gitignore` (`.env*` ausser `.env.example`, Keys, Build-Ausgaben, `.obsidian/workspace*.json`, `.claude/settings.local.json`, `.qa-review/`) und `.gitattributes`.
8. Stop-Hook einmal durchlaufen lassen: keine verwaisten Dateien, `CLAUDE.md` < 4096 Bytes, Memory leer.
9. **Keinen** konkreten Plan und **kein** konkretes Todo anlegen – nur die Struktur, damit sichtbar ist, dass alles initialisiert ist.
10. Nicht committen – zeigen, was entstanden ist, und auf „commit“ warten.

## 16. Nach Abschluss
Dieses File soll gelöscht werden, sobald das Projekt initialisiert ist.
