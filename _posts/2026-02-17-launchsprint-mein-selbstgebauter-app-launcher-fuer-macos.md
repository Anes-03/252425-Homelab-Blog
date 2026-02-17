---
title: "LaunchSprint: Mein selbstgebauter App-Launcher für macOS"
id: "launchsprint"
date: 2026-02-17 18:00:00 +0100
categories: [Homelab, Tools, macOS]
tags: [launchsprint, macos, app-launcher, swiftui, appkit, produktivitaet]
description: "LaunchSprint ist mein selbstgebauter App-Launcher für macOS 26 mit Fullscreen-Grid, Ordnern, Drag & Drop und globalem Hotkey."
image:
  path: /assets/img/posts/2026-02-17-launchsprint-mein-selbstgebauter-app-launcher-fuer-macos/launchsprint-cover.png
  alt: "LaunchSprint Fullscreen-Grid mit Einstellungen"
---

# LaunchSprint: Mein selbstgebauter App-Launcher für macOS

## Warum überhaupt?

Mit macOS 26 hat Apple das gute alte Launchpad abgeschafft und die App-Übersicht direkt in Spotlight integriert. Klingt erstmal nach einer smarten Vereinfachung -- weniger Redundanz, alles an einem Ort. Am Anfang dachte ich auch: gewöhnt man sich bestimmt dran.

Hat es aber nicht. Im aktiven Alltag hat mich die neue Spotlight-App-Ansicht ziemlich schnell genervt. Das Problem: Spotlight versucht selbst herauszufinden, welche Apps gerade für mich "relevant" sein könnten. Klingt clever, fühlt sich aber an wie wenn jemand anders entscheidet, was ich sehen will. Ich will **meine** Apps sehen, in **meiner** Reihenfolge -- nicht das, was ein Algorithmus für passend hält.

Als Workaround hab ich dann den Applications-Ordner ins Dock gesetzt. War auch nicht ideal -- eine lange Liste, kein Grid, keine Ordnerstruktur, kein schnelles visuelles Scannen. Mir wurde klar: Was mir fehlt, ist eine **Fullscreen-Grid-Ansicht**, in der ich meine Apps so anordnen kann, wie ich es will.

Dann kamen die Winterferien um den Jahreswechsel 2025/2026 und ich dachte mir: *Ich könnte doch einfach kurz selbst eine App bauen, die genau die Dinge macht, die ich so sehr vermisse.* Spoiler: "Kurz" wurde dann doch ein paar Abende länger -- aber das Ergebnis kann sich sehen lassen.

## Was ist LaunchSprint?

LaunchSprint ist eine native macOS-App, die als Menüleisten-Item läuft. Einfach ein kleines Grid-Icon oben rechts in der Menüleiste. Ein Klick oder `Ctrl+Option+Cmd+L` und ein Fullscreen-Overlay poppt auf, das alle installierten Apps in einem hübschen Grid anzeigt.

Das Besondere: Man kann seine Apps **frei organisieren** -- Drag & Drop, Ordner erstellen, umbenennen, sortieren. Alles wird persistent gespeichert, also genau so wie man es hinterlässt, findet man es beim nächsten Mal wieder vor.

## Features im Überblick

- **Menüleisten-Integration** -- lebt diskret in der Systemleiste
- **Globaler Hotkey** (`Ctrl+Option+Cmd+L`) -- sofort starten, ohne die Maus zu bewegen
- **Echtzeit-Suche** -- einfach lostippen, die Liste filtert sich sofort
- **Tastatur-Navigation** -- Pfeiltasten + Enter, für alle die lieber auf der Tastatur bleiben
- **Ordner** -- Apps gruppieren, wie man es braucht (z.B. "Dev Tools", "Design", "Games")
- **Drag & Drop Sortierung** -- Apps per Drag & Drop in die gewünschte Reihenfolge bringen
- **Einstellbare Icon-Größe** -- von 60px bis 200px, je nach Geschmack
- **Farbige Auswahl-Highlights** -- 5 Farben zur Auswahl (Blau, Lila, Pink, Orange, Grün)
- **Glassmorphism-Design** -- transparenter Blur-Effekt, sieht einfach clean aus

## Der Tech Stack

Da ich eh im Apple-Ökosystem unterwegs bin, war die Wahl klar:

| Komponente | Technologie |
|---|---|
| UI-Framework | **SwiftUI** |
| Menüleiste & Fensterverwaltung | **AppKit** (NSStatusBar, NSWindow) |
| Globaler Hotkey | **HotKey** (Swift Package) |
| Zustandsverwaltung | **Combine** (@Published, ObservableObject) |
| Persistenz | **UserDefaults** (JSON-encoded) |

Die App scannt automatisch alle Standard-Verzeichnisse (`/Applications`, `/System/Applications`, `~/Applications` + Utilities) und lädt die Icons dynamisch über `NSWorkspace`. So sind die Icons immer aktuell, auch nach App-Updates.

## Wie es aussieht

Wenn man LaunchSprint öffnet, bekommt man ein Fullscreen-Overlay mit Blur-Hintergrund. Oben gibt es eine Suchleiste und Buttons zum Sortieren (A-Z oder eigene Reihenfolge). Die Apps werden in einem responsive LazyVGrid dargestellt -- passt sich automatisch an die Fenstergröße und die eingestellte Icon-Größe an.

Im **Bearbeitungsmodus** kann man:
- Neue Ordner erstellen
- Apps in Ordner verschieben
- Apps aus Ordnern rausholen
- Ordner umbenennen oder auflösen

Die **Einstellungen** erreicht man über das Zahnrad-Icon -- dort lässt sich die Icon-Größe per Slider anpassen und die Highlight-Farbe wählen.

## Architektur-Entscheidungen

Ein paar Entscheidungen, die ich bewusst getroffen habe:

**UserDefaults statt Core Data:** Für eine App-Liste mit Ordnerstruktur ist UserDefaults mit JSON völlig ausreichend. Kein Overhead, kein Schema-Management, einfach serialisieren und fertig.

**Deterministische UUIDs:** Jede App bekommt eine UUID, die aus ihrem Dateipfad generiert wird. So bleibt die ID stabil, auch wenn die App neu entdeckt wird -- und die Ordnerstruktur geht nicht verloren.

**Smarter Abgleich:** Beim Start gleicht die App den gespeicherten Zustand mit dem Filesystem ab. Neue Apps werden automatisch hinzugefügt, gelöschte entfernt -- ohne die manuelle Sortierung kaputt zu machen.

**Rahmenloses Fullscreen-Fenster:** Das Overlay ist ein rahmenloses Fenster auf `statusBar`-Level, sichtbar auf allen Spaces. Ein Klick daneben schließt es sofort.

## Download & Installation

LaunchSprint ist Closed Source, aber kostenlos als DMG verfügbar. Einfach runterladen, mounten, in den Applications-Ordner ziehen -- fertig.

**Download:** [launchsprint.252425.xyz](https://launchsprint.252425.xyz)

Nach dem ersten Start nistet sich die App in der Menüleiste ein und bleibt dort, bis man sie beendet.

**Voraussetzungen:**
- macOS 26+

> Ältere macOS-Versionen wie Sequoia (15) oder früher werden aktuell nicht offiziell unterstützt. Ist aber auch nicht wirklich nötig -- dort gibt es ja noch das gute alte Launchpad. LaunchSprint löst ein Problem, das erst mit macOS 26 entstanden ist.

## Fazit

Was als kleines Winterferien-Projekt angefangen hat, ist mittlerweile fester Bestandteil meines Alltags am Mac. Kein Spotlight-Roulette mehr, kein Scrollen durch den Dock-Ordner -- einfach Hotkey drücken, tippen, Enter, fertig. Genau das Launchpad-Feeling, das Apple mir genommen hat, nur besser.

Wenn du seit macOS 26 auch das alte Launchpad vermisst oder einfach einen schnellen, anpassbaren Launcher suchst -- probier es gern aus. Die App gibt es kostenlos auf [launchsprint.252425.xyz](https://launchsprint.252425.xyz).

---

*LaunchSprint v1.0.0 -- Gebaut mit SwiftUI & AppKit*
