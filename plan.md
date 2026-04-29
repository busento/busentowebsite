# Update-Plan Ristorante Busento Website

> Status: Zur Genehmigung offen — noch nicht coden!

---

## 1. Pizza-Texte korrigieren

**Problem:** Aktuell suggerieren mehrere Stellen auf der Website, dass im Ristorante Busento **keine Pizza** verkauft wird. Das stimmt laut Kundenwunsch nicht.

**Gefundene Stellen:**

| Datei | Zeile | Aktueller Text |
|-------|-------|----------------|
| `src/pages/index.astro` | 44 | *„Ein Familienbetrieb mit Wurzeln in Kalabrien. Wir servieren keine Pizza – wir servieren Leidenschaft auf dem Teller."* |
| `src/pages/index.astro` | 73 | *„Im Ristorante Busento dreht sich alles um die echte italienische Küche – jenseits von Pizza und Fast-Food."* |
| `src/pages/ueber-uns.astro` | 55 | *„Im Ristorante Busento verzichten wir bewusst auf Pizza. Stattdessen setzen wir auf gehobene Gerichte [...]"* |

**Lösungsvorschlag:**
Die Texte werden umformuliert, sodass Pizza als Teil des Angebots erwähnt wird, ohne die Positionierung als gehobenes Restaurant zu verlieren:

- **index.astro (Hero):** „Ein Familienbetrieb mit Wurzeln in Kalabrien. Von handgemachter Pasta über knusprige Pizza bis zu edlen Fleisch- und Fischspezialitäten – wir servieren Leidenschaft auf dem Teller."
- **index.astro (Intro):** „Im Ristorante Busento dreht sich alles um die echte italienische Küche – weit über Fast-Food hinaus. Neben unserer handgemachten Pasta und klassischen Pizzen stehen handverlesene Zutaten, alte Familienrezepte und die Liebe zum Detail im Mittelpunkt."
- **ueber-uns.astro (Story):** „Im Ristorante Busento verbinden wir Tradition mit Vielfalt. Neben unseren gehobenen Gerichten – von handgemachter Pasta bis zu edlen Fleisch- und Fischspezialitäten – backen wir auch Pizza nach authentischem Rezept im Steinofen."

---

## 2. Galerie-Seite: Hintergrund „Impressionen"

**Problem:** Der Hero-Bereich der Galerie-Seite (`/gallerie`) hat aktuell ein anderes Hintergrundbild als der Hero der Startseite.

**Aktueller Stand (`src/pages/gallerie.astro`):**
- Bild: `restaurant-table.jpg`
- Overlay: `bg-bg/70`

**Ziel:** Derselbe Look wie der Hero auf der Startseite (`index.astro`):
- Bild: `hero.jpg` (aus `../assets/hero.jpg`)
- Overlay: `bg-bg/75`
- Ggf. `loading="eager"` und `fetchpriority="high"` übernehmen, falls gewünscht.

**Lösungsvorschlag:**
In `src/pages/gallerie.astro` wird das importierte Bild von `restaurant-table.jpg` auf `hero.jpg` umgestellt und das Overlay von `bg-bg/70` auf `bg-bg/75` angepasst, damit es exakt dem Startseiten-Hero entspricht.

---

## 3. Logo vergrößern und rund

**Problem:** Das Logo im Header ist aktuell relativ klein (`h-12`) und rechteckig.

**Aktueller Stand (`src/components/Header.astro`):**
```astro
<img src={logoImg.src} alt="Ristorante Busento Logo" class="h-12 w-auto object-contain" />
```

**Ziel:**
- **150 % größer:** Von `h-12` (3 rem) auf `h-18` (4,5 rem) bzw. `h-[4.5rem]`.
- **Rund:** Das Bild soll kreisförmig dargestellt werden.

**Lösungsvorschlag:**
Da das Logo-Asset (`logo.jpeg`) wahrscheinlich nicht quadratisch ist, wird es mit folgenden Tailwind-Klassen dargestellt:
- `h-[4.5rem] w-[4.5rem]` für die 150 %-Vergrößerung bei gleichem Seitenverhältnis
- `rounded-full` für die runde Form
- `object-cover` statt `object-contain`, damit das Bild den Kreis vollständig ausfüllt und nicht oval wird

**Ergebnis-Klassen:** `h-[4.5rem] w-[4.5rem] rounded-full object-cover`

Falls das Logo dadurch abgeschnitten wirkt, kann alternativ ein Wrapper mit `aspect-square` und Padding genutzt werden – die obige Lösung ist jedoch der minimale Eingriff.

---

**Benötigte Dateien (3 Stück):**
1. `src/pages/index.astro`
2. `src/pages/ueber-uns.astro`
3. `src/pages/gallerie.astro`
4. `src/components/Header.astro`

**Geschätzter Aufwand:** Sehr gering (< 15 Minuten). Nur Text- und Klassen-Änderungen, keine neuen Komponenten oder Abhängigkeiten.
