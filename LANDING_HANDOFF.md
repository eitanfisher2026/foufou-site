# 🐾 FouFou Landing Page — Handoff Document

**Target domain:** `fofou.city`
**App URL:** `https://eitanfisher2026.github.io/FouFou/`
**Owner:** Eitan Fisher
**Contact:** eitanfisher100@gmail.com
**Languages:** Hebrew (default RTL) + English (LTR), with auto-detection and toggle
**Brand:** Peach gradient + cat-with-sunglasses logo, matching the app's splash screen

---

## 📋 Overview

A single-file landing page (`index.html`) for the FouFou City Trail Generator app. The page introduces the app, tells the story, shows screenshots, and links visitors to the live app on GitHub Pages.

**Visual identity:** the hero section, phone mockup, and screenshot gradients all use the same peach palette (`#F5946B` → `#FBD9B4`) as the app's loading splash. The cat logo with sunglasses appears in the nav, footer, hero phone mockup, and as a watermark in the story card. The whole thing should feel like a natural extension of the app — not a separate marketing site.

---

## 🎨 Brand Assets — IMPORTANT

### The cat logo

The current `index.html` includes a **hand-rebuilt SVG version** of the FouFou cat (the one with sunglasses from the splash screen). It's defined once as a `<symbol id="foufou-cat">` at the top of `<body>` and reused via `<use href="#foufou-cat"/>` everywhere — nav, hero phone, footer, story card watermark, and favicon.

**This SVG approximates the real logo.** It's recognizable but not pixel-perfect. If you have the original logo file in the FouFou app repo (look in `icons/`, `assets/`, `public/`, or as a base64 data URI inside `app-code.js`/`views.js`), please replace it.

**Two replacement strategies:**

**Option A — Replace the SVG symbol** (best if the original is also SVG)
Find the `<symbol id="foufou-cat">…</symbol>` block and swap its contents. All five appearances on the page update automatically.

**Option B — Use a PNG/JPG file** (if the original is a raster image)
1. Save the logo as `/cat-logo.png` (recommend 256×256 PNG with transparency)
2. Replace every occurrence of:
   ```html
   <span class="cat-logo"><svg><use href="#foufou-cat"/></svg></span>
   ```
   with:
   ```html
   <img class="cat-logo" src="/cat-logo.png" alt="FouFou" />
   ```
3. Replace the hero phone block:
   ```html
   <div class="phone-cat">
     <svg viewBox="0 0 100 100" width="100%" height="100%"><use href="#foufou-cat"/></svg>
   </div>
   ```
   with:
   ```html
   <div class="phone-cat">
     <img src="/cat-logo.png" alt="FouFou" style="width:100%;height:100%;object-fit:contain" />
   </div>
   ```
4. The `<symbol>` block can stay (it's invisible) or be deleted to save bytes
5. Update the favicon `<link rel="icon">` in `<head>` to point to a real `.ico` or `.png`

### The peach gradient

Defined in CSS variables at the top:
```css
--peach-1:    #F5946B;   /* deep peach (top) */
--peach-2:    #F8B287;   /* mid peach */
--peach-3:    #FBD9B4;   /* light apricot (bottom) */
```

These match the splash screen exactly. If the app's gradient gets tuned later, sync these three values here too.

---

## 🌐 Internationalization (i18n)

Fully bilingual. All text uses `data-lang-he` and `data-lang-en` attributes; CSS shows the correct one based on `<html lang="...">`.

### Language detection priority (in order)

1. **URL query parameter** — `?lang=he` or `?lang=en` (great for sharing)
2. **localStorage** — `foufou.lang` key (remembers user's choice)
3. **Browser language** — `navigator.languages`, matched on first 2 letters
4. **Default** — Hebrew

### User toggle

A pill-shaped toggle in the nav (`עב | EN`) switches the page instantly. The choice persists across visits.

### Adding/editing copy

Always pair the two languages:
```html
<h2 data-lang-he>איך פופו נולד</h2>
<h2 data-lang-en>How FouFou was born</h2>
```

---

## 🎯 What Claude Code Needs to Do on the Local Machine

### Tasks (in order)

#### 1. Create the new repo

Recommended: a separate repo `fofou-site` so the production app (`FouFou`) stays frozen.

```bash
mkdir fofou-site && cd fofou-site
# place index.html here
```

#### 2. Pull assets from the FouFou app repo

| Asset             | Likely location in FouFou repo                          | What to do                                  |
|-------------------|---------------------------------------------------------|---------------------------------------------|
| Cat logo (real)   | `icons/`, `assets/`, base64 in `app-code.js`           | See "Brand Assets" above — replace the SVG symbol or use as `<img>` |
| Favicon           | `favicon.ico`, `favicon.png`                            | Copy to repo root, update `<link rel="icon">` in `<head>` |
| Apple touch icon  | `icon-192.png`, `icon-512.png`                          | Copy to repo root, add `<link rel="apple-touch-icon">` |
| Screenshots       | ✅ Already included (10 real screenshots, HE + EN)     | `/screens/01-topics.jpg`, `/screens/01-topics-he.jpg`, … `/screens/05-place-he.jpg` |

**Screenshot setup (already done — bilingual):**
The bundle includes **10 real screenshots** from the app — 5 Hebrew + 5 English versions of the same flow. All compressed to ~90-180 KB each (1.2 MB total). The page automatically swaps the screenshot to match the current language:

| Step | English file              | Hebrew file                  | Shows                                    |
|------|---------------------------|------------------------------|------------------------------------------|
| 1    | `screens/01-topics.jpg`   | `screens/01-topics-he.jpg`   | Topic selection ("What interests you?")  |
| 2    | `screens/02-area.jpg`     | `screens/02-area-he.jpg`     | Area selection ("Plan your trip")        |
| 3    | `screens/03-trail.jpg`    | `screens/03-trail-he.jpg`    | Active trail with stops list             |
| 4    | `screens/04-map.jpg`      | `screens/04-map-he.jpg`      | Favorites map (English) / map view (Hebrew) |
| 5    | `screens/05-place.jpg`    | `screens/05-place-he.jpg`    | Place detail (English: Wat Pho) / places list (Hebrew) |

**How the swap works:** Each `.screen-frame` contains two `<img>` tags, one with `data-lang-he` and one with `data-lang-en`. The CSS hides both by default, then shows only the one matching `<html lang="...">`. Switching language via the toggle reveals the other image instantly — no reload, no flicker.

**To take fresh screenshots later:**
1. Take both Hebrew and English versions of each screen
2. Compress to ~720px wide JPEG (under 200 KB each)
3. Replace files in `/screens/` keeping the `-he.jpg` / `.jpg` naming
4. The HTML doesn't need to change — the `<img>` paths stay the same

If you want to add or remove screens, the gallery uses CSS Grid auto-fit — just add or remove `.screen-item` blocks in pairs (one HE image + one EN image). Captions are bilingual; update both `data-lang-he` and `data-lang-en` together.

#### 3. Verify the live app link

All CTAs point to `https://eitanfisher2026.github.io/FouFou/`. If the app moves, search-and-replace.

#### 4. Test locally

```bash
python3 -m http.server 8080
# open http://localhost:8080
```

#### Verification checklist

**Visual brand alignment:**
- [ ] Hero peach gradient matches the app's splash screen (open both side-by-side to compare)
- [ ] Cat logo in nav looks correct (the SVG approximation OR the replaced real logo)
- [ ] Phone mockup in hero looks like the actual app loading screen
- [ ] Story card watermark cat is visible but subtle (12% opacity)

**Layout & rendering:**
- [ ] Page loads without console errors
- [ ] Screenshot frames either show real images **or** fall back to peach gradient placeholders
- [ ] Mobile view (375 px width): no horizontal scroll, nav links collapse, language toggle visible

**Hebrew (default):**
- [ ] Page opens in Hebrew when no preference exists
- [ ] Layout is RTL: logo on right, nav-cta on left
- [ ] Drop cap on first paragraph of "About" floats right

**English:**
- [ ] Click `EN` in the nav → entire page switches to English
- [ ] Layout flips to LTR: logo on left, nav-cta on right
- [ ] Drop cap floats left
- [ ] Browser tab title updates to English

**Persistence:**
- [ ] Switch to English → reload → still in English
- [ ] DevTools → Application → Local Storage → see `foufou.lang: "en"`
- [ ] Clear localStorage → reload → returns to detected language

**Browser detection:**
- [ ] Browser set to English → incognito → opens in English
- [ ] Browser set to Hebrew → incognito → opens in Hebrew

**Shareable URLs:**
- [ ] `?lang=en` → opens in English regardless of saved preference
- [ ] `?lang=he` → opens in Hebrew regardless

**Performance:**
- [ ] Lighthouse: 95+ on Performance, Accessibility, SEO

#### 5. Deploy to `fofou.city`

**Option A — GitHub Pages + custom domain** (simplest)
1. Push the new repo to GitHub
2. Settings → Pages → enable from `main` branch
3. Add a `CNAME` file in repo root containing exactly: `fofou.city`
4. At domain registrar, add DNS A records:
   - `185.199.108.153`
   - `185.199.109.153`
   - `185.199.110.153`
   - `185.199.111.153`
5. Wait 10-30 min, then Settings → Pages → "Enforce HTTPS" ✅

**Option B — Netlify / Vercel / Cloudflare Pages**
Drag-and-drop the folder, point the domain. Faster CDN.

---

## 🎨 Design System Reference

### Color palette (CSS variables, top of `<style>`)

```css
--peach-1:    #F5946B;   /* hero gradient top */
--peach-2:    #F8B287;   /* hero gradient middle */
--peach-3:    #FBD9B4;   /* hero gradient bottom */
--coral:      #E76F51;   /* CTAs, accents */
--coral-deep: #D14B30;   /* eyebrow text */
--saffron:    #F4A261;   /* secondary highlight */
--turquoise:  #2A9D8F;   /* feature icons, status dot */
--plum:       #6B2D5C;   /* feature icon variation */
--cream:      #FFF4E6;   /* page background */
--paper:      #FFFBF2;   /* cards, nav background */
--ink:        #2A1F1A;   /* text + borders (warm dark brown) */
--ink-soft:   #5C4A41;   /* secondary text */
```

### Visual style

- **Shadows:** hard 4px offset (no blur), neo-brutalist. Hover lifts to 6-7px and translates -2px,-2px.
- **Borders:** 2px solid `--ink` everywhere — gives the hand-drawn cohesion.
- **Typography:** Fraunces (display, serif) + Heebo (Hebrew body). English mode also uses Fraunces for body.
- **Motion:** cat bobs gently in hero phone (4s loop), spinner spins (1s), emoji wiggle (3s), feature cards reveal-on-scroll. All respect `prefers-reduced-motion`.

---

## 📝 Content Sources

All Hebrew copy taken verbatim or lightly edited from Eitan's "About" text inside the FouFou app. English is a faithful translation. The cat-name story, "מנהל מוצר," the Shaul Amsterdamski podcast reference, and the "3 months roller coaster" framing are all preserved.

**Single source of truth:** the app's "About" modal. If it changes, sync both `data-lang-he` and `data-lang-en` versions here.

---

## 🔧 Known TODOs

- [ ] **Replace the rebuilt SVG cat with the real logo** (see Brand Assets section above)
- [x] **Real screenshots** — 10 included (5 HE + 5 EN), auto-swap on language toggle
- [ ] **Real favicon + apple-touch-icon**
- [ ] **OG image** for social sharing (1200×630 px) — currently no `og:image` tag
- [x] **Bilingual screenshots** — done
- [ ] **Analytics?** — none installed. Plausible / GoatCounter recommended (privacy-friendly)

---

## 🚨 Things to NOT Touch (per FouFou-dev policy)

This site is **separate** from the production FouFou codebase. The freeze on `FouFou` (production) and the bug-fix-only rule on `FouFou-dev` do **not** apply here. But:

- Do **not** modify the live app to add a "back to landing" link before launch
- Do **not** point `fofou.city` directly at the app — the app stays at GitHub Pages; only the **landing** lives at the apex domain

---

## 🙋 Quick FAQ

**Q: How do I update the gradient if the app's splash changes?**
A: Three CSS variables at the top: `--peach-1`, `--peach-2`, `--peach-3`. Change those and everything (hero background, phone mockup, screenshot placeholders, screens section bottom) updates together.

**Q: How do I add a third language (e.g., French)?**
A:
1. Add `'fr'` to the `SUPPORTED` array in the script
2. Duplicate every `data-lang-he` element with `data-lang-fr` translated
3. Add a third toggle button: `<button data-set-lang="fr">FR</button>`

**Q: Can I change the default language from Hebrew to English?**
A: Find `return 'he';` near the bottom of the script, change to `'en'`.

**Q: What about Google Play store version?**
A: When approved, add a "Get on Google Play" badge to the hero CTA row. Official badges: `https://play.google.com/intl/en_us/badges/`

---

*Generated as a deployment handoff. Update freely as the site evolves.*
