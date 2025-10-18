---
title: "Mein Weg zum Markdown WebEditor"
date: 2025-10-18 18:00:00 +0200
categories: [Homelab, Tools, KI]
tags: [markdown, editor, web, browser, ki]
description: "Wie ich in einer Woche einen komplett clientseitigen Markdown WebEditor gebaut habe – ohne Registrierung, ohne Server, direkt im Browser."
image:
  path: /assets/img/posts/2025-10-18-mein-weg-zum-markdown-webeditor/markdown-webeditor.png
  alt: "Markdown WebEditor Vorschau"
---

Ich mag Markdown, weil es nicht an einen Hersteller gekoppelt ist – anders als etwa DOCX an Word oder PDF an Adobe. Ein offener Standard mit leicht lesbarer Syntax fühlt sich für mich wie die perfekte Grundlage an, um Texte frei zu verfassen, zu archivieren und zu teilen. Aus dieser Überzeugung ist in einer Woche mein Markdown WebEditor entstanden: eine komplett clientseitige Web-App, die ohne Registrierung, ohne Server und ausschließlich im Browser funktioniert. Die wichtigsten Bausteine und Funktionen stelle ich dir hier aus erster Hand vor.

## Ein leichter Editor direkt im Browser

Meine Grundidee war ein reiner Markdown-Editor, der alles lokal im Browser speichert und keinerlei Backend benötigt. Alle Kernfunktionen – von Editor-Ansicht über Split-View bis hin zur reinen Reader-Ansicht – laufen deshalb komplett clientseitig. Der Editor ist direkt unter [markdown-webeditor.252425.xyz](https://markdown-webeditor.252425.xyz) nutzbar – ohne Installation, ohne Registrierung, einfach im Browser öffnen und loslegen. Wer möchte, kann alternativ die `index.html` lokal öffnen oder einen eigenen Server aufsetzen; mehr Infrastruktur braucht es nicht.

## Bedienoberfläche: Alles im Blick, nichts Überflüssiges

Die Oberfläche besteht aus einer Kopfzeile mit Logo, Rückgängig/Wiederholen und dem Dateibereich für Neu, Öffnen, Speichern, Import und Export. Gleich daneben wähle ich zwischen Editor-, Split- oder Reader-Ansicht, schalte durch alle Themes, aktiviere den Vollbildmodus oder öffne die Einstellungen.

Direkt darunter sitzt meine Toolbar mit Buttons für Überschriften, Fett/Kursiv, Listen, Tabellen, Links, Bilder, Zitate und Trennlinien – inklusive Tastaturkürzeln für die typischen Formatierungen. Zwei Schalter sind speziell: „Generieren" startet die KI-Schreibassistenz, „Chat" klappt das Gesprächsfenster auf. Sobald ich den KI-Modus aktiviere, erscheint eine schlanke Leiste für Prompt, Presets und Start/Stop, damit der Textfluss an Ort und Stelle bleibt.

Der Arbeitsbereich besteht aus dem Markdown-Textarea und einer Live-Vorschau, deren Platz sich je nach Ansicht automatisch anpasst. Eine Statusleiste hält mich mit Meldungen, Cursorposition, Wortzahl und Dateiname auf dem Laufenden – ideal, um lange Artikel zu strukturieren.

![Markdown WebEditor – Hauptansicht](/assets/img/posts/2025-10-18-mein-weg-zum-markdown-webeditor/markdown-webeditor.png)
_Hauptansicht: Editor, Toolbar und Live-Vorschau im Split-View._

## Einstellungen, Datenschutz und Feedback

Weil ich alles ohne Account betreiben wollte, landen die Einstellungen direkt lokal im Browser. Im Seitenpanel kann ich Reader-Eingaben, klebende Toolbars oder die Standardansicht aktivieren und KI-Presets verwalten. Außerdem konfiguriere ich dort alle Anbieter – OpenAI, Anthropic Claude, Ollama, Google Gemini und Mistral – inklusive API-Keys, Basis-URLs und Modelllisten. Unter „Info" dokumentiere ich Tastenkürzel, lokale Speicherung und die eingesetzten Bibliotheken und biete ein kleines Feedback-Formular an, das einfach das Mail-Programm öffnet.

![Einstellungsansicht](/assets/img/posts/2025-10-18-mein-weg-zum-markdown-webeditor/einstellungsansicht.png)
_Einstellungen: Anbieter, Modelle, Presets und Verhalten lokal im Browser._

## Datei-Workflow ohne Medienbruch

Autosave ist Pflicht: In kurzen Abständen schreibe ich den Text samt Zeitstempel in `localStorage`, sodass der Editor beim nächsten Öffnen ungespeicherte Inhalte wiederherstellen kann. Öffnen und Speichern erledige ich über die File System Access API, wenn der Browser sie unterstützt; andernfalls erzeugt der Editor einen Download-Link als Fallback. Damit ich beim schnellen Experimentieren nichts verliere, frage ich vor dem Löschen unsicherer Änderungen nach.

Drag & Drop war mir wichtig: Markdown-Dateien kann ich direkt auf die Oberfläche ziehen und öffnen, PDFs oder DOCX-Dokumente importiere ich genauso, und Bilder landen automatisch als Data-URLs im Text – ganz ohne externe Hoster. Für PDF- und Word-Import lese ich mit pdf.js bzw. mammoth.js den Inhalt aus, versuche Überschriften anhand von Schriftgrößen zu erkennen und füge sogar Seitenbilder ein, falls nötig. So entsteht sofort eine Markdown-Version mit Kontext und Bildern.

Exportieren geht ebenso einfach: Ein Klick erzeugt eine statische HTML-Version mit eingebettetem Highlighting oder erstellt per html2pdf.js eine PDF-Version der Vorschau.

## Themes, Layout und Barrierefreiheit

Ausgangspunkt war ein schlichter Dark/Light-Schalter, doch ich habe den Theme-Cycler erweitert: System, Light, Dark, Pitch Black, High Contrast, Sepia sowie Solarized Light/Dark stehen zur Verfügung. Beim Umschalten wechselt automatisch das passende highlight.js-Stylesheet, und der aktuelle Modus wird im lokalen Speicher gemerkt. Für Leserfreundlichkeit nutze ich CSS-Variablen, passe die Layout-Höhen dynamisch an und reagiere auf Fenstergrößenänderungen, damit Editor, Vorschau, Statusleiste und Chat sauber zusammenspielen.

![Themen-Übersicht](/assets/img/posts/2025-10-18-mein-weg-zum-markdown-webeditor/themes.png)
_Theme-Cycler: schnell zwischen Light, Dark, High Contrast, Solarized u.a. wechseln._

## KI-Unterstützung – aber unter meiner Kontrolle

Mein ursprüngliches Ziel war ein klassischer Markdown-Editor. Doch nach kurzer Zeit war klar, dass KI mir beim Schreiben helfen kann, solange sie lokal konfigurierbar bleibt. Deshalb gibt es zwei Ebenen:

1. **Inline-Generator**: Ich markiere einen Abschnitt oder setze den Cursor, tippe einen Prompt und lasse das Modell Text einfügen, ersetzen oder erweitern. Ich kann jederzeit abbrechen, zum Snapshot zurückkehren oder per Presets wiederkehrende Aufgaben speichern. Der gesamte Prozess streamt in das Textfeld, damit ich live sehe, was passiert.
2. **Chat-Panel**: Hier sammle ich längere Recherchen. Der Chat verwendet optional den aktuellen Editor-Inhalt als Kontext, unterstützt Streaming, Kopieren und sogar Vorlesen über die Web Speech API. Ich kann Gespräche leeren, Vorschläge anklicken und zwischen Anbietern wechseln. Alles geschieht direkt im Browser; ich entscheide, welcher Endpunkt (lokales Ollama oder ein Cloud-Dienst) die Daten erhält.

Die Einstellungsseiten erklären für jeden Anbieter, wie ich API-Keys eintrage und Modelllisten lade. Test-Buttons holen die verfügbaren Modelle ab und aktualisieren die Dropdowns, sodass ich schnell zwischen kleineren oder größeren Varianten wechseln kann.

## Kleine Helferlein für den Alltag

- Der Editor aktualisiert Vorschau, Cursorposition und Wortzahl bei jeder Aktion, damit ich mich nicht im Text verliere.
- Tastenkürzel für Neu, Öffnen, Speichern, Fettdruck, Kursiv, Link oder KI-Aufrufe greifen global und unterdrücken Browser-Defaults, wenn sinnvoll.
- Der Vollbildmodus nutzt die native API und passt die Icons entsprechend an – praktisch für fokussiertes Arbeiten.
- Im Chat lassen sich Antworten kopieren oder vorlesen; außerdem halte ich die Modellbezeichnung stets sichtbar im Badgetext.

## Fazit: Markdown schreiben wie mit Word, nur freier

Ich wollte eine schlanke, browserbasierte Alternative, die Markdown so bequem macht wie ein klassischer Texteditor – nur ohne proprietäre Zwänge. Heute öffne ich meinen WebEditor, schreibe Texte offline, importiere Recherche-PDFs, exportiere HTML oder PDF, springe zwischen Themes, lasse mir Abschnitte von der KI umformulieren und chatte mit meinem lokalen Ollama, ohne jemals ein Konto zu brauchen. Genau so habe ich mir produktives Schreiben mit Markdown vorgestellt.

Jetzt testen: [markdown-webeditor.252425.xyz](https://markdown-webeditor.252425.xyz) – Dein Feedback unten hilft mir beim nächsten Release.
