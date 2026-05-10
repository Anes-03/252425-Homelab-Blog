---
title: "Zweite Codex-Instanz auf dem Mac einrichten: Zwei Accounts sauber trennen"
id: "zweite-codex-instanz-zwei-accounts"
date: 2026-05-10 18:30:00 +0200
categories: [Homelab, Tools, KI]
tags: [codex, codex-cli, account-trennung, workflow, terminal, t3-code]
description: "So richte ich auf jedem Mac eine zweite Codex-Instanz ein, damit zwei Accounts sauber getrennt bleiben – inklusive Alias, T3-Code-Setup und Troubleshooting."
image:
  path: /assets/img/posts/2026-05-10-zweite-codex-instanz/codex-zwei-accounts-cover.png
  alt: "Zweite Codex-Instanz auf dem Mac mit zwei getrennten Accounts und codexwork Alias"
---

Ich wollte zwei Codex-Accounts sauber trennen, ohne meinen normalen Workflow umzubauen. Die Lösung ist einfach: eine zweite Codex-Home-Umgebung mit eigenem Login und einem separaten Terminal-Befehl.

Das Ergebnis:

- Privater Account bleibt in `~/.codex`
- Zweiter Account liegt in `~/.codex_work`
- Neuer Shortcut als Beispiel: `codexwork`

So kann ich auf jedem Rechner sauber zwischen beiden Instanzen wechseln. Der Name `work` ist dabei nur ein Beispiel und kann beliebig angepasst werden.

## Ziel

Diese Anleitung erstellt ein separates Codex-Profil für deinen zweiten Account. Dein normaler `codex`-Befehl bleibt privat, und mit `codexwork` öffnest du gezielt die zweite Instanz.

## Bevor du startest

- Codex CLI ist bereits installiert.
- T3 Code ist installiert oder geöffnet, falls du auch die GUI nutzen willst.
- Deine Login-Daten für den zweiten Account sind bereit.

## Schritt 1: Terminal öffnen

Öffne die Terminal-App. Du kannst alle folgenden Befehle aus jedem Verzeichnis ausführen.

## Schritt 2: Work-Codex-Ordner erstellen

```bash
mkdir -p ~/.codex_work
```

Damit legst du einen separaten Speicherort für den zweiten Account an.

## Schritt 3: In den Work-Account einloggen

```bash
CODEX_HOME=~/.codex_work codex
```

Wenn Codex startet, meldest du dich mit deinem zweiten Account an. Die Anmeldung wird dann in `~/.codex_work` gespeichert.

## Schritt 4: Shortcut `codexwork` anlegen

Damit du die zweite Instanz nicht jedes Mal mit `CODEX_HOME=...` starten musst, legst du einen Alias an.

### Option A: Einzeiler

```bash
echo 'alias codexwork="CODEX_HOME=~/.codex_work codex"' >> ~/.zshrc
source ~/.zshrc
```

### Option B: Manuell

```bash
open ~/.zshrc
```

Diese Zeile unten einfügen, speichern, Datei schließen:

```bash
alias codexwork="CODEX_HOME=~/.codex_work codex"
```

Dann neu laden:

```bash
source ~/.zshrc
```

## Schritt 5: Beide Accounts im Terminal nutzen

```bash
codex        # private Instanz
codexwork    # zweite Instanz (Beispielname)
```

## Schritt 6: Zweite Instanz in T3 Code einrichten

1. Öffne die T3 Code Settings.
2. Gehe zum Bereich für Instanzen bzw. `CODEX_HOME`.
3. Klicke auf `+ Add`.
4. Vergib einen Namen wie `Work`, `Second` oder `Side Account`.
5. Setze den `CODEX_HOME`-Pfad auf `~/.codex_work`.
6. Lass den Binary Path auf `codex`, außer du brauchst einen Custom Path.
7. Speichern und Instanz starten.

## Schritt 7: Setup prüfen

Im Terminal:

```bash
codexwork
```

Wenn alles passt, startet die zweite Instanz. Falls erneut ein Login verlangt wird, einfach mit dem zweiten Account anmelden.

## Quick Troubleshooting

| Problem | Fix |
|---|---|
| `codexwork: command not found` | `source ~/.zshrc` ausführen oder Terminal neu öffnen |
| `codexwork` öffnet privaten Account | `CODEX_HOME=~/.codex_work codex` starten und mit dem zweiten Account einloggen |
| T3 Code zeigt weiter den privaten Account | Prüfen, ob wirklich `~/.codex_work` gesetzt ist (nicht `~/.codex`) |
| `open ~/.zshrc` klappt nicht | Stattdessen `nano ~/.zshrc` verwenden und Alias manuell einfügen |

## Finale Zusammenfassung

```bash
# privat
codex

# zweiter Account (Beispiel)
codexwork

# CODEX_HOME für die zweite Instanz
~/.codex_work
```

Ich wiederhole dieses Setup auf jedem Rechner, auf dem ich zwei Codex-Accounts sauber getrennt halten will.
