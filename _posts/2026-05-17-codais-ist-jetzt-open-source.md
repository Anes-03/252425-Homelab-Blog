---
title: "Codais ist jetzt Open Source: Warum ich meine Codex Companion App geöffnet habe"
id: "codais-open-source-codex-companion"
date: 2026-05-17 18:00:00 +0200
categories: [Homelab, Tools, KI]
tags: [codais, codex, open-source, swiftui, rust, typescript, apple, agenten]
description: "Warum ich Codais öffentlich gemacht habe, was mich seit April an Codex so begeistert und wohin sich mein Apple-first Codex Companion entwickeln könnte."
image:
  path: /assets/img/posts/2026-05-17-codais-open-source/codais-open-source-cover.png
  alt: "Codais App Icon in einem Apple-first Codex Workflow Artikelbild"
---

Seit April nutze ich Codex immer mehr. Nicht nur für kleine Experimente, sondern für sehr viele Projekte gleichzeitig. Eines davon ist [Codais](https://github.com/Anes-03/Codais), und genau dieses Projekt habe ich jetzt öffentlich gemacht.

Codais ist aus einem ziemlich einfachen Wunsch entstanden: Ich wollte mein Codex-Setup auch unterwegs nutzen können. Nicht als getrennte Cloud-Umgebung, sondern so, dass mein Mac weiterhin die eigentliche Arbeitsmaschine bleibt. Also dort, wo die Dateien liegen, wo die Shell läuft und wo Codex wirklich Zugriff auf meine lokalen Projekte hat.

## Warum überhaupt Codais?

Codex kann inzwischen sehr viel. Besonders seit dem Update im April fühlt es sich für mich so an, als wäre das Tool noch einmal deutlich mächtiger geworden. Ich nutze es für Blog-Arbeit, App-Ideen, kleine Automationen, Debugging, UI-Experimente und alles Mögliche dazwischen.

Gleichzeitig habe ich OpenClaw seit April immer weniger genutzt. Einerseits war das Codex-Limit dort schnell erreicht, andererseits haben sich einige Workflows für mich nicht mehr so gut angefühlt, seit Anthropic die Nutzung der Claude-Code-Sub darüber nicht mehr erlaubt. Dazu kam, dass mein eigenes OpenClaw-Setup irgendwie immer fragiler wurde. Es brauchte viel Pflege, viel Setup und man musste ständig aufpassen, dass alles sauber bleibt.

Was für mich noch stärker ins Gewicht fällt: Codex deckt inzwischen grundsätzlich fast alles ab, was ich mir wünsche. Das ist bei mir meistens Arbeit direkt in Projekten, aber mittlerweile nutze ich es auch immer öfter für private Dinge. Dadurch verwende ich sogar ChatGPT selbst weniger, weil ich viele Gedanken, Aufgaben und kleine Experimente direkt in meinem lokalen Codex-Workflow erledige.

Hermes Agent gewinnt gerade an Popularität, und ich möchte mir das sicher noch genauer anschauen. Aber ganz ehrlich: Wenn Codex fast alles abdeckt, was ich mir im Alltag wünsche, dann brauche ich nicht zwingend noch ein weiteres großes Agenten-Setup daneben.

## Was mir bei Codex-Automationen noch fehlt

Mir ist bewusst, dass es in Codex bereits Automatisierungen gibt. Ich habe damit auch schon herumgespielt, und für viele geplante Aufgaben ist das nützlich. Man kann Tasks zu einer festen Zeit starten lassen, was für Blog-Arbeit, Homelab-Checks oder wiederkehrende Codepflege durchaus spannend ist.

Was mir im Vergleich zu OpenClaw oder Hermes Agent noch fehlt, ist aber eine andere Art von Automatisierung: ein Agent, der bei einem laufenden Task immer wieder reinschaut und dabei den früheren Kontext behalten kann.

Genau das finde ich an diesen Tools interessant. Wenn man übertrieben gesagt ein KI-Modell trainiert, einen langen Build laufen lässt oder einen größeren Prozess überwacht, dann möchte man nicht unbedingt jedes Mal eine neue Session starten. Man möchte eher, dass der Agent wiederkommt, den Stand prüft, die vorherige Einschätzung noch kennt und dann sinnvoll weiterarbeitet.

Bei Codex fühlt es sich aktuell eher so an, als würde bei einer Automation eine neue Session gestartet. Das kann für viele Aufgaben völlig okay sein, aber es ist nicht dasselbe wie ein Agent, der über längere Zeit an einem Prozess dranbleibt.

Wenn Codex so eine Art wiederkehrendes Reinschauen mit erhaltenem Kontext direkt unterstützen würde, dann bräuchte ich wahrscheinlich wirklich kein OpenClaw oder Hermes Agent mehr. Dann wäre Codex für meinen Workflow fast komplett.

Über mein OpenClaw-Erlebnis möchte ich später noch einen eigenen Artikel schreiben. Da gibt es genug Punkte, die man sauber auseinandernehmen kann.

## Dann kam die ChatGPT-App

Der Grund, warum ich Codais jetzt öffentlich gemacht habe, hängt auch mit OpenAI selbst zusammen. Der Codex-Tab in der ChatGPT-App wurde erweitert. Früher war dieser Tab eher mit Codex Web verknüpft. Das war schon praktisch, weil man vom iPhone aus an einem Projekt arbeiten konnte. Für mich war es aber nie die ideale Lösung, weil beim lokalen Testen und späteren Zusammenführen schnell Merge-Konflikte entstehen konnten.

Mit der neuen ChatGPT-Version und der aktuellen Codex-Version fühlt sich das anders an. Man kann eine Codex-Instanz auf dem eigenen Rechner steuern. Die Oberfläche im Chat wirkt vertraut: Modellauswahl, damit verbundene Optionen, Planmodus, Plugins und sogar Slash-Befehle sind da.

Genau das war im Kern etwas, das ich mir schon lange gewünscht habe: Codex lokal auf dem Rechner, aber von unterwegs steuerbar.

Und damit war für mich klar: Codais muss nicht länger privat bleiben. Es ist aktuell eher ein Konzept und Prototyp als eine fertige App, aber vielleicht ist genau das der richtige Zeitpunkt, es offen zu zeigen.

## Was Codais aktuell ist

Ich sehe Codais als unabhängigen Apple-first Prototyp für Codex-basierte Agenten-Workflows. Wichtig ist dabei: Codais ist kein offizielles OpenAI-Projekt und auch nicht mit OpenAI verbunden.

Technisch ist es ein Monorepo aus drei Teilen:

- `apple-client`: eine SwiftUI-App für iOS, iPadOS und visionOS
- `mac-companion`: ein Rust/Axum-Dienst auf dem Mac, der lokale HTTP- und WebSocket-APIs rund um Codex-Sessions anbietet
- `relay-service`: ein Node.js/TypeScript-Relay für Pairing, Token-Refresh, Widerruf und WebSocket-Weiterleitung

Die Idee dahinter gefällt mir weiterhin sehr: Das Apple-Gerät ist die Steueroberfläche, aber die Ausführung bleibt auf dem Mac. Der Mac ist also weiterhin die Trust Boundary. Dort liegen Workspace-Zugriff, Shell und Codex CLI. Das Relay soll nur beim Pairing und Routing helfen, aber keine Shell-Befehle oder Codex-Workloads ausführen.

Aktuell kann Codais unter anderem Apple-Geräte mit dem Mac Companion verbinden, Codex-Sessions starten, fortsetzen, abbrechen und streamen, Modell und Reasoning Effort auswählen, Session-Verlauf und Tool-Events anzeigen und Workspace-Dateien browsen oder als Vorschau öffnen.

Der Stack passt auch gut zu dem, was ich bauen wollte: Swift als Hauptsprache, dazu Rust für den Companion und TypeScript für das Relay. Ich habe Codais unter der MIT-Lizenz veröffentlicht. Der Code liegt jetzt öffentlich unter [github.com/Anes-03/Codais](https://github.com/Anes-03/Codais).

## Warum ich es trotzdem weiter spannend finde

Auch wenn OpenAI inzwischen sehr viel von dem abdeckt, was ich mit Codais ursprünglich wollte, sehe ich das Projekt nicht als erledigt. Eher hat sich die Rolle verändert.

Codais könnte mehr ein Experimentierfeld werden:

- Wie sieht eine richtig gute mobile Codex-Steuerung aus?
- Wie kann man mehrere Provider unterstützen?
- Wie verbindet man lokale Workspaces sauber mit Remote-Steuerung?
- Wie weit kann man iPhone, iPad oder Vision Pro als Agenten-Control-Surface denken?
- Wie kann man Sessions, Dateien, Logs und Automationen übersichtlich zusammenbringen?

Bei Provider-Unterstützung musste ich auch an [T3 Code](https://github.com/pingdotgg/t3code?tab=readme-ov-file) denken. Einige Elemente aus dieser Richtung könnten gut zu Codais passen. Auch Learnings aus meinem eigenen ähnlichen Projekt, dem [Code WebEditor](https://code-webeditor.252425.xyz), können in Codais einfließen. Gerade wenn es um Editor-UI, Provider-Konfiguration, lokale Einstellungen und einfache Workflows geht, habe ich dort schon einiges gelernt.

## Es ist eines von vielen Projekten

Codais ist nur eines von vielen Projekten, an denen ich gerade arbeite. Genau das macht diese Phase für mich so spannend. Ich habe seit langer Zeit nicht mehr so viel Freude an Tech gehabt.

Ich möchte in nächster Zeit noch über einige Themen schreiben, die daraus entstanden sind. Zum Beispiel:

- wie man ein Android-Smartphone mit Codex nutzen kann
- wie man mit Codex Videos erstellen und bearbeiten kann
- was bei OpenClaw für mich gut und was weniger gut funktioniert hat
- welche Rolle mobile Agenten-Workflows in meinem Homelab spielen könnten

Für den Moment ist Codais öffentlich. Nicht als fertiges Produkt, sondern als offenes Konzept, an dem man sehen kann, wohin meine Gedanken bei Codex, lokalen Workflows und Apple-Geräten gerade gehen.

Code: [Anes-03/Codais](https://github.com/Anes-03/Codais)
