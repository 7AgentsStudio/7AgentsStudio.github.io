# SignallQ Design System — Conventions

## Setup

No provider or context wrapper required. Components are self-contained with inline styles.

Load Roboto and Material Symbols from Google Fonts (already in `styles.css`). For icons to display, the page must have the Material Symbols font loaded — it is included via the `styles.css` `@import` closure.

```html
<link rel="stylesheet" href="_ds/styles.css">
```

## Styling idiom

**All styling via the `LK` token object and `hexA()` helper — no CSS classes, no Tailwind, no styled-components.**

```tsx
import { LK, hexA } from '@signallq/design-system';

// Token usage
style={{ color: LK.textPrimary, background: LK.bgCard, borderRadius: LK.rCard }}

// Semantic colors
LK.accent      // #6C2BFF — primary CTA, selection, active nav
LK.success     // #22C55E — good connection, tests OK
LK.warning     // #F5A623 — moderate alerts
LK.error       // #FF4D4F — critical failures
LK.accentBlue  // #2563EB — informational badges

// Alpha tints (color at 10–30% opacity)
hexA(LK.success, 0.12)   // e.g. card fill
hexA(LK.success, 0.3)    // e.g. card border
```

**Token files:** `tokens/` in the bundle, `styles.css`, `_ds_bundle.css`. Read `styles.css` for the full token list.

**Per-component docs:** each `components/<group>/<Name>/<Name>.prompt.md`.

## SignallQ AI surfaces

Use `ORB` tokens (not `LK`) for the always-dark AI chat surface:

```tsx
import { ORB, LK } from '@signallq/design-system';
// ORB.bg = #0D0D1A, ORB.surface = #1A0B2E, ORB.card = #1E1130, ORB.text = #F3F4F6
```

## Typography

No CSS classes — compose font shorthand inline:

```tsx
style={{ font: `600 18px/1.3 ${LK.font}` }}   // headline-small
style={{ font: `400 14px/1.5 ${LK.font}` }}   // body-medium
style={{ font: `600 11px/1.3 ${LK.font}`, letterSpacing: '.4px', textTransform: 'uppercase' }}  // overline
```

Use the `Overline` component for section labels. Typography sizes: display 34, headline 24/20/18, title 16/15/14, body 16/14/12, label 14/12/11.

## Idiomatic example

```tsx
import { Card, Overline, Badge, Icon, LK, hexA } from '@signallq/design-system';

<Card style={{ display: 'flex', alignItems: 'center', gap: 12 }}>
  <div style={{
    width: 44, height: 44, borderRadius: '50%',
    background: hexA(LK.success, 0.1),
    display: 'flex', alignItems: 'center', justifyContent: 'center',
  }}>
    <Icon name="wifi" size={22} color={LK.success} />
  </div>
  <div style={{ flex: 1 }}>
    <div style={{ font: `600 15px/1.3 ${LK.font}`, color: LK.textPrimary }}>Luiz-5G</div>
    <div style={{ font: `400 11px/1.3 ${LK.font}`, color: LK.textSecondary }}>RSSI −27 dBm · Canal 36</div>
  </div>
  <Badge color={LK.success}>Forte</Badge>
</Card>
```

## Non-negotiables

- Single accent: `#6C2BFF`. No secondary accents.
- Status colors carry meaning: green = good, amber = moderate, red = critical.
- Icons: Material Symbols Outlined only, 24dp default.
- Copy: Brazilian Portuguese, sentence case, UPPERCASE overlines, no emoji.
- Raw metric always with human verdict: "486 Mbps · Excelente".
- Separator: middle dot `·`.
- Grid: 8dp base (4/8/12/16/24/32px).
- Cards: flat, 16dp radius, hairline border, no drop shadows.

# SignallqDesignSystem (@signallq/design-system@0.1.0)

This design system is the published @signallq/design-system React library, bundled as a single
browser global. All 19 components are the real upstream code.

## Where things are

- `_ds_bundle.js` — the whole-DS bundle at the project root; loads every component to `window.SignallqDesignSystem`. First line is a `/* @ds-bundle: … */` metadata header.
- `styles.css` — the single stylesheet entry: it `@import`s the tokens, fonts, and component styles (`_ds_bundle.css`). Link this one file.
- `components/<group>/<Name>/<Name>.prompt.md` (example JSX + variants), `<Name>.d.ts` (types), `<Name>.html` (variant grid).
- `tokens/*.css` — CSS custom properties, names verbatim from upstream.
- `fonts/` — `@font-face` files + `fonts.css` (when the package ships fonts).

For a specific component, `read_file("components/<group>/<Name>/<Name>.prompt.md")`.

## Loading

Add these two lines to your page once (React must be on the page first):

```html
<link rel="stylesheet" href="styles.css">
<script src="_ds_bundle.js"></script>
```

Components are then available at `window.SignallqDesignSystem.*`. Mount into a dedicated child node (e.g. `<div id="ds-root">`), not the host page's own React root, so the two trees don't collide:

```jsx
const { AjustesScreen } = window.SignallqDesignSystem;
ReactDOM.createRoot(document.getElementById('ds-root')).render(<AjustesScreen />);
```

## Tokens

36 CSS custom properties from @signallq/design-system. Names are
preserved verbatim from upstream. They are declared inside `_ds_bundle.css` (this DS ships one compiled stylesheet rather than separate token files).

- **color** (10): `--bg-primary`, `--bg-secondary`, `--bg-card`, …
- **spacing** (6): `--space-xs`, `--space-sm`, `--space-md`, …
- **typography** (1): `--font-sans`
- **radius** (4): `--radius-card`, `--radius-button`, `--radius-input`, …
- **other** (15): `--accent`, `--accent-blue`, `--success`, …

## Components

### screens
- `AjustesScreen` — Ajustes tab: connection settings, appearance, monitoring toggles, and app info.
- `HistoricoScreen` — Histrico tab: uptime stability grid, AI summary, and recent measurements list.
- `HomeScreen` — Incio tab: network path, last measurement card, quick-action row, signal summary.
- `SignallQScreen` — SignallQ AI chat surface  always dark (0D0D1A). Scripted conversation demo.
- `SinalScreen` — Sinal tab: Wi-Fi networks list with band filters, channel occupancy chart, and mobile signal.
- `SpeedFlow` — Full speed-test flow: idle start button  animated gauge  results with verdicts.

### primitives
- `Avatar` — Circular gradient avatar (accent  blue). Used in the top bar leading slot.
- `Badge` — Inline pill chip with semantic color tint. Used for status labels (Conectado, verdicts).
- `Icon` — Material Symbols Outlined icon. Requires the Material Symbols font loaded from Google Fonts.
- `SignalBars` — 4-bar signal-strength glyph matching the Android SignallQ custom icon.

### layout
- `BottomNav` — 5-tab bottom navigation bar with accent highlight and pill indicator.
- `Card` — Surface card: white background, 1px border, 16dp radius, flat (no shadow).
- `Overline` — Section label: 11px semibold, tertiary color, UPPERCASE with letter-spacing.
- `PhoneFrame` — 390820 device frame for previewing screens in the design system viewer.
- `ScreenScroll` — Scrollable screen body with standard padding and vertical gap between sections.
- `StatusBar` — Android-style status bar with clock, Wi-Fi, 5G, and battery indicators.
- `TopBar` — CenterAligned top app bar with leading avatar, centered title+icon, and optional action.

### animations
- `Thinking` — Three-dot pulsing animation shown while SignallQ AI is processing.
- `TypeOut` — Character-by-character typewriter animation used in SignallQ AI responses.
