---
title: "GPT-OSS Model mit Codex CLI und Ollama nutzen: Meine Entdeckungsreise zu lokaler KI-Entwicklung"
date: 2025-09-10 18:00:00 +0200
categories: [Homelab, KI]
tags: [gpt-oss, codex, ollama, lokales-ki, cli]
description: "Meine Erfahrungen, wie sich das GPT-OSS Modell mit der Codex CLI und Ollama lokal betreiben lässt – inkl. schneller Einstieg und Profi-Konfiguration."
image:
  path: /assets/img/posts/2025-09-10-gpt-oss/codex-cli-terminal.png
  alt: "Codex CLI mit GPT-OSS Profil"
---

Ich habe schon länger mit dem Gedanken gespielt, dass das GPT-OSS Modell richtig gut ist. Aber eine Frage ließ mir keine Ruhe: Kann man dieses Modell eigentlich mit CLI-Anwendungen wie Codex oder Claude Code nutzen?

Für mich klang es logisch, dass das OSS-Modell irgendwie mit Codex CLI kompatibel sein sollte – schließlich folgen die meisten Modelle ähnlichen Standards. Also habe ich mich auf die Suche gemacht... und tatsächlich: Ich habe es herausgefunden!

![Codex CLI mit GPT-OSS Profil](/assets/img/posts/2025-09-10-gpt-oss/codex-cli-terminal.png)


## Die Lösung war einfacher als gedacht

Es stellte sich heraus, dass GPT-OSS nicht nur kompatibel ist, sondern sogar nahtlos mit Tools zusammenarbeitet. Das Beste daran? Du bekommst einen vollwertigen KI-Coding-Assistenten, der komplett lokal läuft – ohne Internetverbindung, ohne Datenübertragung, ohne Abo-Gebühren.

## Der schnellste Einstieg: Ein Befehl und los geht's

Das Schöne an der Codex CLI ist ihre Einfachheit. Wenn du bereits Ollama installiert hast, brauchst du nur einen einzigen Befehl:

```bash
codex --oss
```

Das war's! Dieser magische Befehl macht alles für dich:
- Prüft, ob Ollama läuft
- Lädt GPT-OSS:20b herunter (falls noch nicht da)
- Verbindet sich automatisch mit deiner lokalen Ollama-Instanz

Als ich das zum ersten Mal ausprobiert habe, war ich ehrlich gesagt überrascht. Meine Vermutung über die Kompatibilität hatte sich nicht nur bestätigt – es war sogar noch einfacher als gedacht! Keine komplizierte Konfiguration, keine endlosen Setup-Schritte – einfach eingeben und loslegen.

## Für Poweruser: Die config.toml meistern

Wenn du mehr Kontrolle brauchst (und wer will das nicht?), dann ist die Konfigurationsdatei dein bester Freund. Erstelle eine `~/.codex/config.toml` und öffne die Tür zu unbegrenzten Möglichkeiten:

```toml
disable_response_storage = true
show_reasoning_content = true

[model_providers.local]
name = "local"
base_url = "http://localhost:11434/v1"

[profiles.oss]
model = "gpt-oss:20b"
model_provider = "local"
```

Was mir besonders gut gefällt: Du kannst verschiedene Profile für verschiedene Modelle erstellen. Hier ist eine erweiterte Konfiguration, die dir maximale Flexibilität gibt:

```toml
# Ollama Provider
[model_providers.ollama]
name = "Ollama"
base_url = "http://localhost:11434/v1"

# Profile für GPT-OSS 20B
[profiles.gpt-oss-20b]
model_provider = "ollama"
model = "gpt-oss:20b"

# Profile für GPT-OSS 120B
[profiles.gpt-oss-120b]
model_provider = "ollama"
model = "gpt-oss:120b"

# Profile für andere Modelle (falls gewünscht)
[profiles.mistral]
model_provider = "ollama"
model = "mistral:latest"
```

## Nutzung der verschiedenen Profile

Mit den Profilen kannst du gezielt verschiedene Modelle verwenden:

```bash
# GPT-OSS 20B verwenden
codex --profile gpt-oss-20b

# GPT-OSS 120B verwenden (benötigt viel RAM/VRAM)
codex --profile gpt-oss-120b

# Direktes Modell-Flag mit --oss
codex --oss -m "gpt-oss:120b"
```

Was ich besonders toll finde: Das Setup beschränkt sich nicht nur auf GPT-OSS! Du kannst in der gleichen Konfiguration auch andere Ollama-Modelle einbinden und je nach Aufgabe das passende Modell wählen. So hast du für verschiedene Coding-Aufgaben immer das optimale Tool zur Hand.

Achtung: Das 120B-Modell ist ein Monster und braucht etwa 80GB VRAM. Falls deine Grafikkarte das nicht schafft, bleib beim 20B-Modell – es ist trotzdem beeindruckend gut!

## Meine bisherigen Erfahrungen: Ehrlich und ungeschönt

Nach einigen Wochen des Testens kann ich ein erstes Fazit ziehen: Was UI-Design angeht, ist das OSS-Modell meinem Eindruck nach etwa vergleichbar mit GPT-3.5. Es ist definitiv brauchbar, aber nicht ganz auf dem Level der neuesten kommerziellen Modelle.

Ich bin ehrlich gesagt noch selbst dabei herauszufinden, wo ich das Modell für Coding-Anwendungen am besten einsetzen könnte. Manche Aufgaben funktionieren überraschend gut, bei anderen merkt man schon die Grenzen. Das ist aber auch Teil des Experimentierens – und hey, es kostet ja nichts außer Rechenzeit!

## Mein Fazit: Die Suche hat sich gelohnt

Meine ursprüngliche Frage – ob GPT-OSS mit Codex CLI kompatibel ist – hat mich auf eine interessante Entdeckungsreise geführt. Nicht nur ist es kompatibel, es funktioniert sogar besser als erwartet!

Klar, es ist nicht perfekt. Bei manchen Aufgaben merke ich die Grenzen im Vergleich zu den Top-Modellen. Aber dafür läuft alles lokal, kostet nichts nach der initialen Hardware-Investition, und ich lerne dabei eine Menge über die Eigenarten verschiedener Modelle.

## Falls du auch neugierig bist...

Wenn du dir ähnliche Fragen gestellt hast wie ich, dann probiere es aus: Installiere Ollama, teste `codex --oss`, und lass dich überraschen. Manchmal sind die besten Lösungen die, nach denen man nicht gesucht hat, sondern die sich aus logischen Überlegungen ergeben.

## Ein kleines Dankeschön an OpenAI

Ich muss ehrlich sagen: Ich feiere es, dass OpenAI uns ein so tolles Open-Weights-Modell zur Verfügung gestellt hat! Und dass Codex selbst Open Source ist, macht das Ganze noch besser. Die Konfiguration ist wirklich nicht schwer zu meistern.

Allerdings ist das Hinzufügen von MCP-Servern eher nicht gerade einheitlich – nicht so elegant wie andere es machen, zum Beispiel die Gemini CLI. Aber das ist eine andere Geschichte für ein anderes Mal.

Bis dahin vergesst nicht: "Feeling the AGI Today🤖"!

Hast du auch schon mal über die Kompatibilität verschiedener KI-Tools nachgedacht? Welche Überraschungen hast du dabei gemacht? Ich bin gespannt auf deine Erfahrungen!
