# Brand Guidelines

## Brand Overview

KOLEKTA is a **civic technology** brand rooted in **Luanda, Angola**. The brand communicates environmental responsibility, urban innovation, and civic empowerment. Every design decision — from color choices to typography — serves the goal of making waste collection information accessible, trustworthy, and useful for Luanda's citizens.

---

## Brand Name

- **Full name**: **KOLEKTA**
- Always written in **UPPERCASE**
- The name evokes *"colecta"* (collection in Portuguese) with a distinctive **"K"** spelling that creates a memorable, modern identity
- Do not write as "Kolekta", "kolekta", or "KoLekTa"

---

## Tagline

| Language | Tagline |
|----------|---------|
| Portuguese | **"Sabe quando. Vive melhor."** |
| English | **"Know when. Live better."** |

- The tagline should appear with the logo in formal contexts (website header, presentation title slides, printed materials).
- In informal contexts (app icon, favicon, social media avatar), the tagline may be omitted.

---

## Logo

The KOLEKTA logo is the primary visual identifier of the project. It consists of three components:

### Components

| Component | Description |
|-----------|-------------|
| **Logomark** | A map pin icon containing a waste collection truck silhouette, with a curved road element and a green leaf accent. Represents location-aware waste collection. |
| **Logotype** | "KOLEKTA" in bold uppercase sans-serif, with a small leaf accent on the letter "A". |
| **Combined mark** | Logomark + logotype used together — the primary logo for most applications. |

### Logo Files

Available in `assets/branding/`:

| File | Variant | Usage |
|------|---------|-------|
| `kolekta-logo.png` | Standalone logomark (icon only) | Favicon fallback, small UI elements, watermarks |
| `kolekta-logo-horizontal.png` | Full horizontal logo (logomark + logotype + tagline) | Website header, documentation, presentations |
| `kolekta-app-icon.png` | App icon (rounded corners, suitable for mobile/PWA) | App icon, social media profile picture, favicon |

> [!NOTE]
> Additional logo formats (SVG, dark background variants) may be added as the project evolves. Vector source files should be maintained for any future logo modifications.

---

## Color Palette

The color palette is rooted in green — reflecting sustainability, environmental consciousness, and the natural world. Colors were extracted from the brand assets.

### Primary Colors

| Role | Name | Hex | RGB | Usage |
|------|------|-----|-----|-------|
| Primary Dark | Forest Green | `#1B5E20` | 27, 94, 32 | Primary brand color, headlines, dark backgrounds, header bars |
| Primary | Green | `#2E7D32` | 46, 125, 50 | Logo gradient start, primary buttons, links |
| Vibrant | Light Green | `#4CAF50` | 76, 175, 80 | Logo gradient mid-point, interactive elements, hover states |
| Accent | Lime | `#8BC34A` | 139, 195, 74 | Leaf accents, highlights, success states, badges |

### Supporting Colors

| Role | Name | Hex | RGB | Usage |
|------|------|-----|-----|-------|
| Light Background | Pale Green | `#C8E6C9` | 200, 230, 201 | Card backgrounds, subtle accents, secondary backgrounds |
| Text | Near Black | `#1B3A1B` | 27, 58, 27 | Body text, headings on light backgrounds |
| Surface | White | `#FFFFFF` | 255, 255, 255 | Page backgrounds, text on dark surfaces |

### Gradient

The primary brand gradient flows from **Forest Green** to **Vibrant Green**:

```css
background: linear-gradient(135deg, #1B5E20 0%, #4CAF50 100%);
```

This gradient is used in the logo, hero sections, and prominent UI elements.

### Color Accessibility

- **Text on light backgrounds**: Use `#1B3A1B` (contrast ratio > 10:1 on white).
- **Text on dark green backgrounds**: Use `#FFFFFF` (contrast ratio > 7:1 on `#1B5E20`).
- **Interactive elements**: Ensure all clickable elements meet WCAG 2.1 AA contrast requirements (4.5:1 for normal text, 3:1 for large text).

---

## Typography

### Font Stack

| Role | Font | Fallback | Weight | Usage |
|------|------|----------|--------|-------|
| **Headings** | Montserrat | `sans-serif` | Bold (700) | Page titles, section headings, hero text |
| **Body** | Inter | Open Sans, `sans-serif` | Regular (400), Medium (500) | Paragraphs, descriptions, UI labels |
| **Monospace** | JetBrains Mono | `monospace` | Regular (400) | Code blocks, API examples, technical data |

> [!IMPORTANT]
> All fonts must be **open-source or freely available** to align with the project's open-source nature. The fonts listed above are all available under open licenses (SIL Open Font License or similar).

### Typographic Scale

| Element | Size (rem) | Weight | Line Height |
|---------|-----------|--------|-------------|
| H1 | 2.5 | Bold | 1.2 |
| H2 | 2.0 | Bold | 1.3 |
| H3 | 1.5 | Bold | 1.3 |
| H4 | 1.25 | Medium | 1.4 |
| Body | 1.0 | Regular | 1.6 |
| Small / Caption | 0.875 | Regular | 1.5 |

---

## Visual Identity Principles

The KOLEKTA visual identity is guided by five principles:

### 1. Environmental 🌿

The green palette reflects sustainability and environmental consciousness. Every visual element should reinforce the connection between waste management and a cleaner urban environment.

### 2. Civic 🏛️

Design should feel **public, accessible, and community-oriented**. KOLEKTA is a tool for everyone — not a premium product for a select audience. Avoid elitist or exclusive visual language.

### 3. Modern ✨

Clean lines, generous whitespace, and contemporary styling. The interface should feel current and well-crafted, conveying competence and reliability.

### 4. Trustworthy 🤝

Consistent branding builds trust with citizens and operators. Visual consistency across all touchpoints (web app, notifications, documentation) is essential. Never use the brand in misleading contexts.

### 5. Angolan 🇦🇴

The brand originates from Angola and should respect the local context. Design decisions should consider the Luandan audience: mobile-first (many users access via smartphones), data-conscious (minimize unnecessary downloads), and culturally appropriate.

---

## Usage Guidelines

### Logo Usage Rules

| ✅ Do | ❌ Don't |
|-------|---------|
| Use provided logo files at their original proportions | Distort, stretch, or compress the logo |
| Maintain minimum clear space around the logo | Crowd the logo with other elements |
| Place on clean, uncluttered backgrounds | Place on visually busy backgrounds without contrast |
| Use the logomark alone when the brand name is established in context | Use the logomark in isolation when users haven't seen the brand before |

### Minimum Sizes

| Context | Minimum Height |
|---------|---------------|
| Digital (screen) | 32px |
| Print | 10mm |

### Clear Space

Maintain a minimum clear space around the logo equal to the **height of the letter "K"** in the KOLEKTA logotype. No other text, graphics, or visual elements should intrude into this space.

### What NOT to Do

- ❌ Do not rotate the logo.
- ❌ Do not apply drop shadows or outer glows.
- ❌ Do not recolor the logo (use provided variants only).
- ❌ Do not recreate the logo in a different font.
- ❌ Do not add visual effects (gradients, patterns) over the logo.
- ❌ Do not use the logo to imply endorsement by any organization that has not explicitly endorsed the project.

---

## Digital Applications

### Favicon

Use the **app icon variant** (`kolekta-app-icon.png`) as the source for generating favicons. Generate sizes: 16×16, 32×32, 180×180 (Apple Touch), and 192×192 / 512×512 (PWA manifest).

### Social Media Profile

Use the **app icon variant** for profile pictures across all platforms. The rounded-corner version works well within circular and rounded-square crops.

### Open Graph / Social Preview

Use the **horizontal logo** on a white or light green (`#C8E6C9`) background. Recommended dimensions: 1200×630px for optimal display across platforms.

### Loading States

Use an animated version of the logomark for loading indicators. ⏳ Animation design is pending.

### Notification Icon

Use a simplified, monochrome version of the logomark for push notification icons on mobile devices. ⏳ Monochrome variant is pending.

---

## Brand Voice

While not strictly visual, the brand voice is part of the identity:

| Attribute | Description |
|-----------|-------------|
| **Clear** | Use simple, direct language. Avoid jargon when communicating with citizens. |
| **Helpful** | Prioritize actionable information. Tell people what they need to know. |
| **Honest** | Never overstate capabilities. Predictions are estimates, not guarantees. |
| **Respectful** | Acknowledge the civic context. This is a public service tool, not a commercial product. |
| **Bilingual** | Primary language is Portuguese (as spoken in Angola). English is used for the international developer community and documentation. |

---

## Cross-References

- [README.md](../README.md) — project overview and introduction
