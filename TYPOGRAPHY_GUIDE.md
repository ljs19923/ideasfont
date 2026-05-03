# Guide Typographique pour Apps de Lecture (Substack / Medium grade)

> Recommandations concrètes pour résoudre les problèmes de `font-size`, `line-height`, hiérarchie de titres et confort de lecture sur **desktop ET mobile**.

---

## 1. Les 6 principes fondamentaux

1. **Le confort de lecture > l'esthétique pure.** Une app comme Ideas.xyz mise tout sur le contenu : la typo doit disparaître au profit du sens.
2. **Le rythme vertical compte plus que le `font-size`.** Un texte à 17px avec `line-height: 1.7` se lit mieux qu'un 19px à 1.3.
3. **Longueur de ligne (measure) idéale = 60-80 caractères, ou ~700-740px en absolu.** Au-delà, l'œil se perd. En dessous, la lecture est saccadée.
4. **Mobile ≠ Desktop réduit.** Sur mobile, on monte le `font-size` (paradoxalement) parce que la distance œil-écran est plus courte mais la concentration plus fragile.
5. **Pas de noir pur (#000) sur blanc pur (#FFF).** Trop contrasté = fatigue. Préférer `#1a1a1a` ou `#202020` sur `#FFFFFF` (ou `#FAFAFA`).
6. **Hiérarchie par le poids ET la taille, pas que la taille.** Un H2 en 600 weight à 28px est souvent plus lisible qu'un 36px en 400.

---

## 2. Tableau de référence : Tailles & Line-heights

### Desktop (≥ 1024px)

| Élément       | Font-size      | Line-height | Font-weight | Letter-spacing | Margin |
| ------------- | -------------- | ----------- | ----------- | -------------- | ------ |
| **Display / Hero**     | `52-64px` (3.25rem - 4rem) | `1.08` - `1.1` ⭐ | 800 | `-0.025em` à `-0.03em` | `24px` bottom |
| **H1** (titre article) | `44-52px` (2.75rem - 3.25rem) | `1.1` - `1.15` | 700 - 800 | `-0.022em` | `24px` bottom |
| **H2** (section)       | `32-36px` (2rem - 2.25rem) | `1.2` - `1.25` | 700 | `-0.018em` | `60-96px` top, `20px` bottom |
| **H3** (sous-section)  | `24-26px` (1.5rem - 1.625rem) | `1.3` | 600 - 700 | `-0.012em` | `40-60px` top, `12px` bottom |
| **H4**                 | `20px` (1.25rem) | `1.4` | 600 | `-0.005em` | `8px` |
| **Body / paragraph**   | `18-20px` (1.125rem - 1.25rem) ⭐ | `1.6` - `1.65` | 400 | `-0.003em` ou `0` | `1.4em` entre paragraphes |
| **Lead paragraph** (intro) | `21-23px` (max-width plus étroit que le body) | `1.5` - `1.55` | 400 | `-0.005em` | `2em` |
| **Caption / meta**     | `14-15px` | `1.5` | 400 - 500 | `0` ou `0.01em` | / |
| **Blockquote**         | `21-24px` | `1.5` | 400 italique | `-0.01em` | `2em` vertical |
| **Code inline**        | `0.9em` du parent | hérité | 400 | `0` | / |

⭐ **Sweet spot body desktop : `19px` (1.1875rem) avec `line-height: 1.65`.** C'est exactement ce qu'utilise Substack. Medium pousse à 20-21px (parce qu'ils tablent sur des sessions très longues), mais 19px est plus universel et moins fatigant pour des sessions courtes/moyennes (cards, listing, articles courts).

### Mobile (≤ 640px)

| Élément       | Font-size      | Line-height | Font-weight |
| ------------- | -------------- | ----------- | ----------- |
| **Display / Hero** | `36-48px` | `1.13` - `1.18` ⭐ | 800 |
| **H1**        | `28-32px`      | `1.15` - `1.2`  | 700 - 800 |
| **H2**        | `24-26px`      | `1.25` | 700 |
| **H3**        | `19-21px`      | `1.35` | 600 |
| **H4**        | `17px`         | `1.4` | 600 |
| **Body**      | `17px` ⭐      | `1.6` | 400 |
| **Lead**      | `18-19px`      | `1.55` | 400 |
| **Caption**   | `13-14px`      | `1.5` | 400 |
| **Blockquote**| `18-20px`      | `1.5` | 400 italique |

⭐ **Pourquoi 17px et pas 16px sur mobile ?** Avec un body en **serif** (Source Serif 4, Charter, etc.), le x-height est plus petit qu'avec un sans-serif comme Inter. À taille égale, un serif paraît visuellement ~6-8% plus petit. Donc 16px serif est perçu comme du 15px Inter. Pour compenser : **17px en serif = 16px en sans visuellement**. Si tu utilises Inter pour le body sur mobile, tu peux rester à 16px sans souci. Pour du long-form en serif : 17px est le sweet spot.

---

## 3. La règle d'or du `line-height`

```
line-height = f(font-size), inversement proportionnel
```

| Font-size | Line-height idéal | Cas d'usage |
| --------- | ----------------- | ----------- |
| 13-14px   | 1.5 - 1.55        | Captions, meta |
| 16-17px   | 1.6               | Body mobile |
| 18-20px   | 1.6 - 1.65        | Body desktop |
| 22-24px   | 1.5 - 1.55        | Lead, blockquote |
| 26-32px   | 1.3 - 1.4         | H3, H2 mobile |
| 36-48px   | **1.13 - 1.18** (sweet spot mobile hero) | H2 desktop, hero mobile |
| 52-64px   | 1.08 - 1.12       | H1 desktop, hero desktop |
| 64-80px+  | **1.05 - 1.08**   | Display ultra-large |

**Le piège classique** (qu'on voit sur Ideas.xyz) : appliquer `line-height: 1.5` partout, ce qui donne :
- Des titres trop étalés (mou, peu impactant)
- Des paragraphes trop tassés (lecture fatigante)

---

## 4. Fluid Typography avec `clamp()` (méthode moderne)

Au lieu de mediaqueries, utilise `clamp()` pour des transitions fluides desktop ↔ mobile :

```css
:root {
  /* Body : 17px sur mobile → 19px sur desktop (serif) */
  --fs-body: clamp(1.0625rem, 0.95rem + 0.36vw, 1.1875rem);

  /* H1 : 32px sur mobile → 52px sur desktop */
  --fs-h1: clamp(2rem, 1.4rem + 3vw, 3.25rem);

  /* H2 : 26px → 36px */
  --fs-h2: clamp(1.625rem, 1.3rem + 1.6vw, 2.25rem);

  /* H3 : 20px → 26px */
  --fs-h3: clamp(1.25rem, 1.1rem + 0.75vw, 1.625rem);

  /* Lead : 18px → 22px */
  --fs-lead: clamp(1.125rem, 1rem + 0.65vw, 1.375rem);
}
```

---

## 5. Stack de polices recommandé (combo retenu pour Ideas)

> **Combo final : Source Serif 4 pour le body + Inter pour les titres.**
> C'est le combo des essais de Stripe Press, du blog The Browser Company, ou de Figma research. Le body en serif apporte le rythme de lecture longue (la serif guide l'œil sur l'horizontale), les titres en sans-serif apportent la modernité et l'impact visuel. Au-dessus de 24px, Inter chante avec ses tracking négatifs ; en-dessous de 21px, Source Serif tient mieux la longueur que n'importe quelle sans.

### Body / paragraphes → Source Serif 4 (ou Charter en fallback)
```css
font-family: "Source Serif 4", "Charter", "Iowan Old Style", Georgia, "Times New Roman", serif;
font-feature-settings: "kern", "liga", "calt", "onum";
/*  onum = old-style numerals (chiffres minuscules, alignés sur la x-height)
*/
```
→ Source Serif 4 est la version optical-size de la famille d'Adobe (gratuite via Google Fonts). Elle "respire" en grand et reste lisible en petit. Apparente rondeur, très bonne x-height.

### Titres → Inter
```css
font-family: "Inter", -apple-system, BlinkMacSystemFont, "SF Pro Text", "Segoe UI", sans-serif;
font-feature-settings: "ss01", "cv11", "cv02", "kern";
/*  ss01 = humanist alternates (a à un étage, l avec queue)
    cv11 = open digit zero
*/
font-weight: 700-800;
letter-spacing: -0.018em à -0.028em;  /* compense l'étalement des grandes tailles */
```
→ Inter est la référence sans-serif pour l'UI et la lecture digitale en 2026 (Linear, Figma, GitHub, Vercel).

### Stack système ultra-rapide (zero loading, fallback)
```css
font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, "Helvetica Neue", Arial, sans-serif;
```

### Mono (code inline / blocs)
```css
font-family: "SF Mono", Menlo, Consolas, "Liberation Mono", monospace;
```

---

## 6. Largeur de colonne (CRUCIAL)

```css
.article-content {
  /* 740px max sur desktop, viewport - 2rem en mobile (breathing room) */
  width: min(740px, 100% - 2rem);
  margin-inline: auto;
}
```

**Pourquoi `min()` plutôt que `clamp()` ?**
La formule `min(740px, 100% - 2rem)` donne exactement le comportement attendu :
- Desktop (viewport ≥ 772px) : largeur = **740px exactement**.
- Mobile (viewport < 772px) : largeur = `viewport - 2rem` (16px de marge de chaque côté).

**Piège classique avec `clamp()`** : une formule comme `clamp(60ch, 55ch + 12vw, 740px)` n'atteint le cap de 740px qu'à des viewports très larges (>1900px). Sur un écran 1280px tu plafonnes à ~670px, alors que tu pensais avoir 740. Le `min()` direct évite ce piège.

**Pourquoi 740px et pas plus ?** Au-delà de ~750-760px avec un body à 19-20px, l'œil saccade en fin de ligne et perd le retour à la ligne suivante. Medium (~680px), Substack (~720px), Stripe (~620px), tous restent sous 750px.

---

## 7. Couleurs & contraste pour la lecture

### Light mode
```css
--color-text:        #1a1a1a;     /* body, pas #000 */
--color-text-muted:  #57606a;     /* meta, captions */
--color-heading:     #0d0d0d;     /* plus dense que le body */
--color-bg:          #ffffff;     /* ou #fafaf7 pour réduire la fatigue */
--color-border:      #e5e7eb;
--color-link:        #1a73e8;     /* ou la couleur de la marque */
```

### Dark mode
```css
--color-text:        #e6e6e6;     /* pas #fff pur */
--color-text-muted:  #9ca3af;
--color-heading:     #f5f5f5;
--color-bg:          #0f0f0f;     /* pas #000 pur */
--color-border:      #262626;
```

**Contraste WCAG AA :** ratio ≥ 4.5:1 pour le body, ≥ 3:1 pour les titres ≥ 18px.

---

## 8. Le rythme vertical : l'espace entre les sections

C'est l'élément le plus sous-estimé d'une mise en page de lecture. Un bon rythme vertical fait qu'on "scanne" un long article sans effort, parce que l'œil sait instantanément où finit une section et où commence la suivante.

### Les 3 niveaux d'espacement

| Niveau | Avant un… | Espace recommandé | Pourquoi |
| ------ | --------- | ----------------- | -------- |
| **Inter-paragraphe** | `<p>` | `1.4em` (≈ 1.4 × font-size) | Le défaut `1em` est trop serré, on devine pas le retour à la ligne |
| **Sub-section** | `<h3>` | `40-60px` fluide (mobile → desktop) | Sépare nettement sans casser la sous-hiérarchie |
| **Section** | `<h2>` | `60-96px` fluide ⭐ | Crée un vrai "respire" qui annonce un nouveau chapitre |
| **Hero → Article** | premier `<p>` | `48-80px` | Pause cognitive entre branding et contenu |

⭐ **Astuce qui change tout :** la majorité des sites ont `margin-top: 2em` (≈ 32-40px) avant un H2. Tu doubles cette valeur, c'est immédiatement plus pro. Substack et Medium tournent à 64-80px avant un H2.

### Implémentation fluide

```css
:root {
  --space-section:    clamp(3.75rem, 2.5rem + 4.5vw, 6rem);   /* 60 → 96px */
  --space-subsection: clamp(2.5rem,  1.9rem + 2.2vw, 3.75rem); /* 40 → 60px */
}

h2 {
  margin-top: var(--space-section);
  margin-bottom: 1.25rem;
}

h3 {
  margin-top: var(--space-subsection);
  margin-bottom: 0.75rem;
}

p { margin-bottom: 1.4em; }

/* Évite l'orpheline juste après un titre */
h2 + p, h3 + p { margin-top: 0; }
```

---

## 9. Détails qui font la différence (le 10% qui change tout)

```css
body {
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-rendering: optimizeLegibility;
  font-feature-settings: "kern", "liga", "calt";
  hanging-punctuation: first last; /* ponctuation en marge */
}

p {
  text-wrap: pretty;        /* évite les "veuves" en fin de paragraphe */
  hyphens: auto;
  overflow-wrap: break-word;
}

h1, h2, h3 {
  text-wrap: balance;       /* équilibre les lignes des titres */
}
```

### Astuce visuelle : le lead paragraph plus étroit

```css
.lead {
  font-size: var(--fs-lead);
  max-width: 44ch;          /* ~ 60% du body, crée une hiérarchie visuelle */
  margin-inline: auto;      /* (si centré) sinon laisse aligné */
}
```

Le lead/intro plus étroit que le body donne une lecture en "entonnoir" qui guide naturellement vers le corps de l'article. Substack, NYTimes, et le blog de Stripe utilisent tous cette technique.

---

## 10. Accessibilité & UX

- **Respecte `prefers-reduced-motion`** (pas d'animations sur le scroll de lecture).
- **`prefers-color-scheme`** pour dark mode auto.
- **Utilise `rem`** pour le sizing (respect du zoom utilisateur).
- **Focus visible** clair sur tous les liens : `outline: 2px solid; outline-offset: 2px;`.
- **Tap targets ≥ 44×44px** sur mobile (Apple HIG).

---

## 11. Comparatif : ce que font les meilleures apps de lecture

| App        | Body font-size | Body line-height | Body font          | Titres            | Max-width   |
| ---------- | -------------- | ---------------- | ------------------ | ----------------- | ----------- |
| **Medium** | 20-21px        | 1.58             | Charter (serif)    | Sohne / Inter     | ~680px      |
| **Substack** | 19px         | 1.6              | Source Serif       | Spectral          | ~680-720px  |
| **NYTimes**| 18-19px        | 1.5              | Imperial (serif)   | Cheltenham        | ~600px      |
| **Stratechery** | 17px      | 1.5              | Georgia            | Georgia           | ~640px      |
| **Linear blog** | 17px      | 1.6              | Inter              | Inter             | ~640px      |
| **Stripe blog** | 18px      | 1.65             | Sohne (sans)       | Sohne             | ~620px      |
| **Stripe Press** | 19px     | 1.6              | Sohne (sans)       | Sohne             | ~700px      |
| **🎯 Reco Ideas** | **19px / 17px mob** | **1.65 / 1.6 mob** | **Source Serif 4** | **Inter (sans)**   | **740px**  |

---

## 12. Quick fix pour Ideas.xyz (les 5 changements prioritaires)

Si on devait corriger Ideas.xyz en quelques lignes de CSS, voici ce que je ferais :

```css
:root {
  --fs-body:  clamp(1.0625rem, 0.95rem + 0.36vw, 1.1875rem); /* 17→19px (serif) */
  --fs-lead:  clamp(1.125rem, 1rem + 0.65vw, 1.375rem); /* 18→22px */
  --lh-body:  1.65;
  --measure:  min(740px, 100% - 2rem);

  --ff-body:  "Source Serif 4", "Charter", Georgia, serif;
  --ff-head:  "Inter", -apple-system, BlinkMacSystemFont, "Segoe UI", sans-serif;

  /* Rythme vertical */
  --space-section:    clamp(3.75rem, 2.5rem + 4.5vw, 6rem);    /* 60→96 */
  --space-subsection: clamp(2.5rem,  1.9rem + 2.2vw, 3.75rem);  /* 40→60 */
}

article {
  font-family: var(--ff-body);
  width: var(--measure);
  margin-inline: auto;
}

article p {
  font-size: var(--fs-body);
  line-height: var(--lh-body);
  color: #1a1a1a;
  text-wrap: pretty;
  margin-bottom: 1.4em;
}

article .lead {
  font-size: var(--fs-lead);
  max-width: 44ch; /* lead plus étroit que le body, crée la hiérarchie */
}

article h1, article h2, article h3, article h4 {
  font-family: var(--ff-head);
  text-wrap: balance;
}

article h1 { font-size: clamp(2rem, 1.4rem + 3vw, 3.25rem);       line-height: 1.1;  letter-spacing: -0.022em; }
article h2 { font-size: clamp(1.625rem, 1.3rem + 1.6vw, 2.25rem); line-height: 1.2;  letter-spacing: -0.018em; margin-top: var(--space-section); }
article h3 { font-size: clamp(1.25rem, 1.1rem + 0.75vw, 1.625rem); line-height: 1.3; margin-top: var(--space-subsection); }
```

---

## 13. Checklist finale avant prod

- [ ] Body = **17px sur mobile (serif)** ou 16px (sans), 19px sur desktop
- [ ] Line-height body entre 1.55 et 1.7
- [ ] Line-height des hero titles : **1.1 desktop, 1.15 mobile** (les petits titres demandent plus d'air)
- [ ] `max-width` 740px desktop (`min(740px, 100% - 2rem)`)
- [ ] **Lead** plus étroit que le body (44ch vs 60-80ch)
- [ ] **Espace avant H2** entre 60 et 96px (clamp), pas le default 32px
- [ ] **Espace avant H3** entre 40 et 60px
- [ ] Body en **Source Serif 4** (serif), titres en **Inter** (sans)
- [ ] Hiérarchie de titres avec `letter-spacing` négatif sur les gros titres
- [ ] `text-wrap: balance` sur les titres, `pretty` sur les paragraphes
- [ ] Pas de `#000` sur `#fff` (préférer `#1a1a1a`)
- [ ] Espace inter-paragraphe ≥ 1.4em (pas le default 1em)
- [ ] Stack de polices avec fallback système (FOIT/FOUT minimisés)
- [ ] `font-display: swap` sur les `@font-face`
- [ ] Test sur écran portrait mobile + tablette landscape

---

## TL;DR : Les 3 chiffres + le combo de polices à retenir

> **19 / 1.65 / 740px**
> = font-size 19px, line-height 1.65, max-width 740px sur desktop.
> Body en **Source Serif 4**, titres en **Inter**.
> Espace avant H2 : **60-96px**. Lead plus étroit que le body : **44ch**.
> Hero title line-height : **1.1 desktop / 1.15 mobile**.

Sur mobile : **17px / 1.6 / pleine largeur avec padding 20px** (serif). Si tu utilises un sans-serif comme Inter pour le body, tu peux descendre à 16px sans perte de lisibilité.
