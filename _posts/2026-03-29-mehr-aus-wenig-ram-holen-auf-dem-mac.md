---
title: "Mehr aus wenig RAM holen auf dem Mac? Ein kurzer Test mit iogpu.wired_limit_mb"
id: "mac-ram-iogpu-wired-limit"
date: 2026-03-29 00:05:00 +0100
categories: [Homelab, macOS, Performance]
tags: [macos, apple-silicon, ram, sysctl, tuning]
description: "Ein kurzer Praxis-Check zum sysctl-Wert iogpu.wired_limit_mb auf Macs mit 8/16 GB RAM – inklusive Risiken, Teststrategie und Fazit."
image:
  path: /assets/img/posts/2026-03-29-mac-ram-tuning-iogpu/cover-terminal.png
  alt: "Terminal mit sysctl iogpu.wired_limit_mb und gesetztem Wert 14336"
---

# Mehr aus wenig RAM holen auf dem Mac?
## Ein kurzer Test mit `iogpu.wired_limit_mb`

Ich habe ein Video gesehen, in dem zwei Befehle gezeigt wurden, mit denen man auf Macs mit 8 GB oder 16 GB RAM eventuell noch etwas mehr Spielraum bekommt.

**Quelle:** <https://youtu.be/nxCtScEImew?si=lhsH9DffxQkPHPmI>

```bash
# Aktuellen Wert anzeigen
sysctl iogpu.wired_limit_mb

# Temporär neuen Wert setzen (Beispiel)
sudo sysctl iogpu.wired_limit_mb=14336
```

## Was macht dieser Wert?

`iogpu.wired_limit_mb` beeinflusst, wie viel Speicher die GPU als "wired memory" reservieren darf.

Auf Apple-Silicon-Macs mit Unified Memory kann das in bestimmten Workloads helfen – muss es aber nicht. Es ist ein Tuning-Ansatz, kein garantierter Performance-Boost.

## Wichtige Hinweise vor dem Test

- Die Änderung ist **temporär** (nach einem Neustart gilt wieder der Standardwert).
- Ein zu hoher Wert kann den restlichen Systemspeicher stark einschränken.
- Besonders bei 8-GB-Geräten sollte man vorsichtig testen.
- Das ist kein "kostenloses RAM-Upgrade", sondern ein Low-Level-Feintuning.

## Sinnvoll testen statt blind übernehmen

So teste ich solche Tweaks:

1. Ausgangszustand messen (Speicherdruck, Swap, Reaktionszeit im Alltag).
2. Wert setzen.
3. Genau denselben Workflow erneut durchlaufen.
4. Ergebnis vergleichen und nur übernehmen, wenn es **messbar** besser ist.

## Fazit

Der Tweak kann in einzelnen Szenarien nützlich sein, ist aber kein Wundermittel.

Wer ihn ausprobieren will, sollte strukturiert testen und Stabilität priorisieren. Wenn der eigene Workflow nicht profitiert, lieber beim Standard bleiben.
