---
title: "Neue Logos für mein Homelab"
date: 2025-09-04 18:00:00 +0200
categories: [Homelab, Design]
tags: [logo, design, branding, homelab]
description: "Ich habe neue Logos für mein Homelab entworfen – erste Entwürfe, Varianten und nächste Schritte."
image:
  path: "/assets/img/posts/2025-09-04-neue-logos/20250821_2246_Invertiertes Logo Design_remix_01k37616v7fmhrjjz3gbhq7nf9.png"
  alt: "Homelab Logo"
compress_html: false
---

In den letzten Tagen habe ich einige neue Logo-Entwürfe für mein Homelab gestaltet. Mir war wichtig, etwas zu finden, das schlicht, wiedererkennbar und auch in kleinen Größen gut lesbar ist. Außerdem sollen die Logos sowohl in Farbe als auch als Monochrom-Variante funktionieren.

Was ich ausprobiert habe:

- Verschiedene Symbolideen rund um „Home“, „Netzwerk“ und „Lab“
- Runde und eckige Formen für bessere Skalierbarkeit
- Farbvarianten (hell/dunkel) und klare Kontraste

Nächste Schritte:

- Feinabstimmung der Proportionen und Abstände
- Test in unterschiedlichen Größen (Favicon, Header, Social)
- Auswahl einer finalen Farbpalette für das Theme

Sobald die finale Variante steht, aktualisiere ich das Theme hier im Blog.

## Bilder-Serie

<style>
  /* Scoped slider styles (only this post) */
  #logos-slider {
    position: relative;
    margin: 1rem 0 1.5rem 0;
    border-radius: 12px;
    overflow: hidden;
    background: var(--card-bg, #0f1216);
    border: 1px solid var(--chip-border, rgba(255,255,255,0.08));
  }
  #logos-slider .viewport {
    display: grid;
    place-items: center;
    min-height: clamp(240px, 60vh, 720px);
  }
  #logos-slider img {
    max-width: 100%;
    max-height: 70vh;
    display: block;
  }
  #logos-slider .caption {
    padding: .6rem .9rem;
    font-size: .95rem;
    color: var(--text-muted, #cbd5e1);
    background: rgba(0,0,0,.35);
    backdrop-filter: blur(6px);
  }
  /* Display slide as background (no <img>, no theme popup) */
  #slide-frame {
    width: 100%;
    height: clamp(240px, 60vh, 720px);
    background-size: contain;
    background-position: center center;
    background-repeat: no-repeat;
  }
  /* Disable preview image zoom for this page */
  a.img-link.preview-img { pointer-events: none; cursor: default; }
  #logos-slider button.nav {
    position: absolute;
    top: 50%;
    transform: translateY(-50%);
    background: rgba(0,0,0,.55);
    border: 1px solid rgba(255,255,255,.15);
    color: #fff;
    width: 42px; height: 42px;
    border-radius: 50%;
    cursor: pointer;
    display: grid; place-items: center;
    font-size: 22px; line-height: 1;
  }
  #logos-slider .prev { left: 12px; }
  #logos-slider .next { right: 12px; }
  #logos-slider .dots {
    position: absolute; bottom: 10px; left: 0; right: 0;
    display: flex; gap: 6px; justify-content: center; align-items: center;
  }
  #logos-slider .dot {
    width: 8px; height: 8px; border-radius: 50%;
    background: rgba(255,255,255,.35);
  }
  #logos-slider .dot.active { background: #fff; }
  @media (max-width: 600px) {
    #slide-frame { height: 58vh; }
  }
</style>

<div markdown="0">
<div id="logos-slider" aria-label="Logo-Bilderserie">
  <div class="viewport">
    <div id="slide-frame" role="img" aria-label="Logo-Entwurf"></div>
  </div>
  <div class="caption" id="slide-caption">Bild 1</div>
  <button class="nav prev" aria-label="Vorheriges Bild" title="Vorheriges">‹</button>
  <button class="nav next" aria-label="Nächstes Bild" title="Nächstes">›</button>
  <div class="dots" id="slide-dots" aria-hidden="true"></div>
</div>

<script>
  (function () {
    const slides = [
      {
        src: "/assets/img/posts/2025-09-04-neue-logos/20250426_1544_252425 Technisches Logo_simple_compose_01jss5e2ryecwr99fdqye2n5bd.png",
        alt: "Bild 1",
        caption: "Bild 1"
      },
      {
        src: "/assets/img/posts/2025-09-04-neue-logos/20250426_1544_252425 Technisches Logo_simple_compose_01jss5e2rzf81rbg6wkjnmwhzf.png",
        alt: "Bild 2",
        caption: "Bild 2"
      },
      {
        src: "/assets/img/posts/2025-09-04-neue-logos/20250603_1924_Logo für 252425 Homelab_simple_compose_01jwvd4csge4kbg8hme8rbmkw9.png",
        alt: "Bild 3",
        caption: "Bild 3"
      },
      {
        src: "/assets/img/posts/2025-09-04-neue-logos/20250603_1930_Haus mit Fenstern_remix_01jwvdeagcfqgb6e8rqqq5y0ew.png",
        alt: "Bild 4",
        caption: "Bild 4"
      },
      {
        src: "/assets/img/posts/2025-09-04-neue-logos/20250603_1930_Haus mit Fenstern_remix_01jwvdeagde7m9cga47q0cxa6t.png",
        alt: "Bild 5",
        caption: "Bild 5"
      },
      {
        src: "/assets/img/posts/2025-09-04-neue-logos/20250821_2246_Invertiertes Logo Design_remix_01k37616v7fmhrjjz3gbhq7nf9.png",
        alt: "Bild 6",
        caption: "Bild 6"
      }
    ];

    const frame = document.getElementById('slide-frame');
    const caption = document.getElementById('slide-caption');
    const btnPrev = document.querySelector('#logos-slider .prev');
    const btnNext = document.querySelector('#logos-slider .next');
    const dotsEl = document.getElementById('slide-dots');
    let idx = 0;

    function renderDots() {
      dotsEl.innerHTML = slides.map((_, i) => `<span class="dot${i===idx?' active':''}" data-i="${i}"></span>`).join('');
    }

    function show(i) {
      idx = (i + slides.length) % slides.length;
      const s = slides[idx];
      frame.style.backgroundImage = `url('${s.src}')`;
      caption.textContent = s.caption;
      renderDots();
    }

    btnPrev.addEventListener('click', () => show(idx - 1));
    btnNext.addEventListener('click', () => show(idx + 1));
    document.getElementById('logos-slider').addEventListener('keydown', (e) => {
      if (e.key === 'ArrowLeft') show(idx - 1);
      if (e.key === 'ArrowRight') show(idx + 1);
    });
    dotsEl.addEventListener('click', (e) => {
      const t = e.target; if (t.classList.contains('dot')) show(+t.dataset.i);
    });

    // Initialize
    show(0);
  })();
</script>
</div>

Wenn du Feedback oder spontane Ideen hast, freue ich mich darüber!
