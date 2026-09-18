# IVU.webDS — Design System

**Source of truth:** Pipeline Design System (Figma) + `webUI/` Angular component library (`@ivu-webui/source`, MIT).

IVU.webDS is the internal design system that backs **IVU Traffic Technologies'** transit-operations products. It is a collection of components, guidelines and resources to help UX designers, and developers build consistent user experiences across IVU web products.

## Sources used

| Source | Where | Role |
| --- | --- | --- |
| `webUI/` (Nx workspace, Angular 21) | Mounted via Import → `webUI/libs/webUI/` | Canonical CSS tokens, icon font, component templates, Storybook stories. Treat as source of truth. |
| `Pipeline Design System.fig` | Mounted as VFS, 115 pages | Visual reference for component variants, spacing, redlines. Keep in mind: \~80% of the file is the `/Colours` exploration; production tokens live in code. |
| Internal docs | `confluence.ivu.de/x/cIJSLw` (referenced from `webUI/README.md`) | Setup / install — not accessible from here. |

## Index — what's in this folder

| File / folder | Purpose |
| --- | --- |
| `README.md` | This file. Visual + content fundamentals, iconography, principles. |
| `SKILL.md` | Claude-skill manifest — invoke to spin up a design task. |
| `colors_and_type.css` | Every token: colors, type scale, fonts, shadows, radii, spacing, semantic classes (`.ivu-body`, `.ivu-heading`, etc). Loads the woff2s. |
| `fonts/` | Noto Sans woff2 in 6 weights (300–800). |
| `assets/IconFont74.ttf` | The IVU icon font — \~330 single-color glyphs. |
| `assets/icons.css` | Class-name → codepoint mapping for every icon (`.icon-bus`, `.icon-warning`, etc). |
| `preview/` | Small HTML cards rendered into the Design System tab — palettes, type specimens, component states. |
| `ui_kits/webDS/` | A working hi-fi UI kit. `index.html` boots a fake dispatcher control-room screen with NavBar, AppBar, DataTable, SidePanel, Buttons, Chips, Toasts, ModalDialog. |

## Brand context

IVU AG is a Berlin software company; its corporate identity uses two blues — a deep navy (`#01518c` *primary*) and a vivid cyan (`#009ee3` *secondary*) — paired with neutral grays. The web design system extends that with operational status colors (warning orange, error red, success green) and a maygreen/chartreuse "activated" state that's unique to IVU and signals an active filter. The aesthetic is **Carbon-adjacent**: 2px corner radii, tight rows, a single typeface across the entire surface, very few illustrations, no gradients.

---

## CONTENT FUNDAMENTALS

**Audience:** transit operators, dispatchers, planners, schedulers. Domain-fluent. Multilingual (DE/EN primary, locale system exists in `libs/webUI/src/lib/locale/`).

**Voice:** terse, functional, third-person. Labels are nouns or imperatives, never marketing copy. Compare:

- ✅ "Cancel trip", "Open requests", "Unassigned for x hours"
- ✅ "Created today", "Closed today", "Activated today"
- ✅ "Follow vehicle", "Focus on incident"
- ❌ "Take charge of your fleet today!"
- ❌ "Welcome back, Sarah 👋"

**Casing:** **Sentence case** for buttons, labels, menu items, headers. *No* Title Case. Acronyms stay upper (KPI, SEV, HVAC, ID, AM/PM).

**Tone:** neutral and operational. No "you", no "we", no first person. Status messages describe what *is* — they don't address the user. "Microphone off." "3 vehicles offline." "Connection failed."

**Numbers & units:** always show units inline (`12 km`, `08:24`, `4 h 30 min`). Times use 24h format. Distances metric. Durations spelled out (`min`, `h`, never `m`/`hr`).

**Emoji:** never in product UI. (Figma file uses ✅ to mark "shipped" status of design tokens — internal only.)

**Empty states:** named in code as `EmptyState`, `ErrorPageState`. Short body, sometimes an action button. "No incidents." not "Looks like you don't have any incidents yet — try creating one!"

**Truncation:** ellipsis + tooltip is the standard pattern (`ivuEllipsisWatcher` directive). Never wrap a single-line label.

---

## VISUAL FOUNDATIONS

### Typography

- **Single typeface family:** *Noto Sans* (latin), weights 300/400/500/600/700/800. Fallback stack `'Noto Sans', 'Helvetica Neue', Arial, sans-serif`.
- **Scale (px / line-height):** 12/16 (caption), 14/18 (small + label), 16/21 (body + heading), 18/? (lg), 20/26 (section heading), 24, 30 (display light), 60 (rare hero).
- **Weight pairings:** body uses 400; medium (500) for labels and button text; semibold (600) is the "heading" weight; bold (700) and extra-bold (800) reserved for emphasis and rare display. **Light (300)** is the IVU signature for big numeric displays — KPIs and dashboard counts use `text-3xl font-light`.
- **Letter spacing:** `--tracking-label: 0.01em` on labels and button text — a hair of openness, no more.
- **Mono:** none in product UI. (Figma explorations show Noto Sans Mono, but it isn't wired into the theme.)

### Color

- **Two-blue identity.** Primary navy `#01518c` for chrome (NavBar = `#003A64` deep variant) and primary action fills. Secondary cyan `#009ee3` for hover, focus, selection, links, and the "interactive accent" everywhere — selection state on rows uses `--selected: #CCECF9`.
- **Neutrals:** 13-step gray scale named by lightness (`gray-10` … `gray-90`), not by 50/100/200/.... `--ivu-white: #fafafa` (NOT pure white) is the canvas. The viewArea background sits one shade darker at `#EBEEF0` so cards on it pop without a heavy shadow.
- **Status:** error `#C00B00`, warning `#D76F00`, success `#258135`. Each ships with `-700` (hover/pressed depth), `-900` (max depth), and `-10`/`-16` tinted surfaces for backgrounds.
- **Activated vs Selected** — a critical distinction inherited from IVU's filtering metaphor. *Selected* = "this row/chip is the current focus" (cyan blue). *Activated* = "this filter / state is in effect" (maygreen `#EFF2B3` pale chartreuse + `#A2BB0C` border). Both states have hovered + pressed depths.
- **No gradients.** Anywhere. The system uses solid fills and 10–16% tints only.
- **Opacity tints** rather than alpha overlays — e.g. `--primary-500-16` is a pre-mixed `#D6E3EC` solid, not `rgba(1,81,140,0.16)`.

### Spacing & layout

- **Tight by default.** Table rows are \~32–40 px tall, button heights 24/32. Section padding follows a 4/8/12/16 px rhythm, occasionally 24 or 48 (`--spacing-aa`).
- **Density first.** Side panels typically 320–400 px, data tables fill the rest. Multi-column "draft form" layout exists with capped column widths (`--width-columnDraftGeneralInformation1920Max: 42.6875rem`).
- **Breakpoint:** Tailwind defaults extended with `3xl: 1920px` — control-room monitors are the design target.
- **Border radii:** `--radius-regular: 2px` is the only radius used in production. Buttons, inputs, cards, dialogs — all 2px. Avatars and pictograms are circular. (Tailwind's `rounded-full` and `rounded-xs` are the only two radii you'll see in templates.)

### Borders

- **1 px regular, 2 px focus.** Borders are functional, never decorative. `--border-regular: 1px`, `--border-focus: 2px`.
- **Border colors:** `gray-30` (`#c4c4c4`) is the default; `gray-20` for read-only/disabled separation; `primary-500` for focus; `error-500` for invalid; `selected-border-color: #009EE3` for selection.

### Shadows — three roles

- `--shadow-10/20/30` (3px y-offset, 6px blur, varying alpha 10/20/30%) — **stacked surfaces**: dropdowns, hover lift, menus.
- `--shadow-content: 0 0 6px rgba(0,0,0,0.4)` — **uniform 360° glow** for floating panels and toasts (no directional offset because they appear above arbitrary content).
- `--shadow-90: 0 0 6px rgba(0,0,0,0.9)` — **hard shadow** behind the expanded NavBar when it overlays content. Heavy on purpose.
- No inset shadows in the system **except** one autofill hack (`--autofill-input-inset-shadow`) that paints over the browser's yellow autofill background.

### Animation

- **Minimal and fast.** Two motion primitives:
  - `fadeIn` — 300 ms `ease-in-out`, often with a 180 ms delay before opacity is animated (used in nav-bar collapse so labels don't smear).
  - `transformPanel` — 120 ms `cubic-bezier(0, 0, 0.2, 1)`, used by Menus to scale-Y from 80% to 100%.
- `fade-out` = the same fadeIn played **in reverse at 10 ms** — effectively instant. The system prefers immediate dismissal over symmetric exits.
- `slide-down`/`slide-up` for the message bar (300 ms linear).
- **No bouncing, springing, parallax, or scroll-linked motion.** Easing curves are limited to `ease-in-out`, `linear`, and one cubic-bezier.

### Interaction states (universal)

Codified by the `/Interaction-States` Figma page; consistent across components.

| State | Indicator |
| --- | --- |
| Default | Base fill, `gray-30` border (where bordered) |
| **Hover** on dark fills | Step to `-700` depth (`primary-500` → `primary-700`) |
| **Hover** on white surfaces | Add `--hover-white` (`#E3F1F7`) wash; cyan border on inputs |
| **Pressed** | Step to `-900` depth, or `--pressed-white: #C8E8F5` wash |
| **Focused** | 2 px `primary-500` ring, inset (`focus:ring-inset`), `focus-visible:outline-hidden` to avoid double-rings |
| **Selected** | `--selected: #CCECF9` fill + `selected-border-color: #009EE3` border |
| **Activated** (filters) | `--activated: #EFF2B3` maygreen tint + `--activated-border-color: #A2BB0C` |
| **Disabled** | `bg-gray-20` fill, `text-gray-40` content. No borders, no hover. |
| **Read-only** | `bg-gray-15` fill, `gray-20` border |
| **Auto-filled** (browser autofill) | `bg-warning-500-5` warm cream + a 1000px inset shadow trick |

### Cards & containers

- Cards = `bg-ivu-white` + 1 px `gray-20` border + optional `--shadow-10`. **No rounding past 2 px.** Headers inside cards use `text-base font-semibold`.
- Side panels = full-height, fixed-width (usually `320–400 px`), `--shadow-content` from the right.
- The ModalDialog uses `--shadow-content` + a scrim of `rgba(0,0,0,0.5)`. Centered with `dialog { margin: auto }`.

### Imagery, illustrations, patterns

- **Almost none in product UI.** No hero photography, no illustrations, no patterns or textures. The only "imagery" is:
  - The icon font (\~330 glyphs, monochromatic, all stroke-and-fill consistent).
  - Map tiles (OpenLayers) inside `/Map` components — these *are* full-bleed but rendered by `ol`, not styled by the design system.
  - **Pictographs** — a small set of larger illustrative icons for empty states, error pages. Same monochrome treatment as icons.

### Transparency & blur

- Used sparingly. The `/9` opacity utility (`bg-hover/9`, `bg-secondary-500/9`) gives a faint 9% wash for hover indication where a solid fill would be too heavy. **No backdrop-blur** in the system.

### Layout rules (fixed elements)

- **NavBar** (left): collapsible 64 ↔ 256 px, full height, `bg-frame-primary` (`#003A64`). Auto-collapses on hover-out. Always visible above content (z-index 150).
- **AppBar / Header** (top): 64 px, `bg-frame-secondary` (`#E7EFF4`), 1 px bottom border. Title + actions only — no search, no global nav.
- **Toasts** appear bottom-right with `--shadow-content` and `fadeIn`. Stack vertically with 8 px gap.
- **Status bar** (bottom, where present) is thin (\~24 px), `gray-15` background.

### What this system *isn't*

- It isn't a content/marketing system — there's no rich type hierarchy, no editorial layouts, no photo treatments.
- It isn't expressive — there are no "moods" or themes; one dark navbar, one light canvas, the whole way through.
- It isn't AA-only — accessibility patterns exist (`/Accessibility-guidelines`, focus rings, ellipsis-watch tooltips) but density and small text mean designers must verify contrast on each surface.

---

## ICONOGRAPHY

**Single icon font, in-codebase: `IconFont74.ttf` (\~330 glyphs).** This is the source of truth. Every icon used in product UI lives in this font. Glyphs are referenced by class:

```html
<i class="ivu-icon icon-bus"></i>
<i class="ivu-icon icon-warning text-error-500"></i>
```

The full class list lives in `assets/icons.css`. Categories include:

- **Transit objects** — `icon-bus`, `icon-tram`, `icon-train`, `icon-metro`, `icon-trainset`, `icon-tramset`, `icon-vehicles`, `icon-passenger`, `icon-driver`
- **Actions** — `icon-edit`, `icon-delete`, `icon-add`, `icon-close`, `icon-refresh`, `icon-undo`, `icon-redo`, `icon-publish`, `icon-duplicate`
- **Status / state** — `icon-warning`, `icon-error`, `icon-success`, `icon-info`, `icon-emergency`, `icon-busy`, `icon-wait`, `icon-broadcast`, `icon-online-operational`, `icon-offline`
- **Navigation / direction** — `icon-arrow-north/south/east/west`, `icon-arrow-up/down/left/right`, `icon-arrow-*-double`, `icon-compass`
- **Time** — `icon-clock`, `icon-current-time`, `icon-am`, `icon-pm`, `icon-too-early`, `icon-early`, `icon-late`, `icon-too-late`, `icon-deviation-time`
- **Workflow** — `icon-open-status`, `icon-in-progress`, `icon-resolved`, `icon-active-incident`, `icon-closed-incident`, `icon-archive`, `icon-draft`
- **Infrastructure** — `icon-charge-point`, `icon-depot`, `icon-stop-point`, `icon-line`, `icon-trip`, `icon-block`, `icon-duty`, `icon-bank`
- **Comms** — `icon-call`, `icon-microphone-on/off`, `icon-speak`, `icon-listen`, `icon-message`, `icon-wifi`, `icon-signal-0` … `icon-signal-4`, `icon-5G/4G/3G/2G`

**Style:** uniform stroke / fill weight, \~24×24 design grid, no color (use the parent text color). Sizes per `--text-icon-{xs|sm|lg|xl|6xl|8xl}` — 14, 18, 24, 30, 64, 96 px.

**Pictographs** are a small set of larger illustrative icons (`/Pictograph` Figma page, code at `libs/webUI/src/lib/components/pictograph/`). Used in empty states and error pages. Same monochrome style, just bigger and slightly more detailed.

**No CDN icon library** is loaded — Lucide, Heroicons, etc. are *not* in use. **Never substitute** a CDN icon for an `ivu-icon-*` glyph in production design work; the font already covers the use case.

**Unicode / emoji:** never. The Figma file uses ✅ internally to mark design-token status; that's the only place.

**Brand logo:** the IVU corporate logo isn't bundled with the design system — it's owned by the marketing team. Use the deep blue `frame-primary: #003A64` + the wordmark "IVU" set in Noto Sans ExtraBold as a placeholder in mocks.

---

## Caveats

- The Figma file (`Pipeline Design System.fig`) has 115 pages but \~80% of the volume is colour exploration — the *production* colour tokens are the ones in `colors_and_type.css`, sourced from `_theme.css`.
- The `Cover` page in Figma uses Comic Sans for an internal joke ("always") — not part of the system.
- Some Figma typography pages reference Inter and Roboto — these are sample/legacy references, **not** part of the production stack.
