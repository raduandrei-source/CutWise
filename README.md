# CutVisor — Timber Cut Optimizer for Blender

**design & craft with minimum waste**

CutVisor is a Blender addon for timber structure projects. Assign part IDs to your components, calculate mass/volume/cost, export Bills of Materials, and generate an optimized cut plan — so you know exactly what boards to buy and how to cut them.

![CutVisor hero](docs/hero.png)

---

## Free vs Pro

| Feature | Core (free) | Pro |
|---|---|---|
| Materials & Stock Sizes | ✓ | ✓ |
| Generate Part IDs | ✓ | ✓ |
| Detailed & Consolidated BOM export | ✓ | ✓ |
| Shopping List generation | ✓ | ✓ |
| Kerf, Stock Bonus, Tolerance settings | ✓ | ✓ |
| Viewport overlay (IDs, names, dims, count) | ✓ | ✓ |
| View presets (All / IDs / Dims / Off) | ✓ | ✓ |
| Convert GN Array → Legacy | ✓ | ✓ |
| Diagnose Active Object | ✓ | ✓ |
| Lists CSV export | ✓ | ✓ |
| Multi-language UI (14 languages) | ✓ | ✓ |
| **Rip Cut, Cross-Cut, Long Rip settings** | — | ✓ |
| **Width Variation** | — | ✓ |
| **Multi-Board Splicing** | — | ✓ |
| **Wood species density presets** | — | ✓ |
| **Overlay: color, shadow, outline** | — | ✓ |
| **CSV import (materials & stock)** | — | ✓ |

👉 **[Get CutVisor Pro on Gumroad →](https://cutvisor.gumroad.com/l/yiqahd)**

---

## Installation

1. Download `cutvisor.zip` from [Releases](../../releases)
2. In Blender: **Edit → Preferences → Add-ons → Install**
3. Select the zip file → **Install Add-on**
4. Enable **CutVisor** in the add-ons list
5. Open the **N-panel** in any 3D Viewport → **CutVisor** tab

> **Pro users:** go to Settings tab → paste your activation key → Pro features unlock instantly.

---

## Quick Start

### 1. Add a material
Go to **Materials tab** → **+** → set name, density, pricing mode and stock sizes.

### 2. Assign materials to objects
Select your mesh objects → Materials tab → **Assign to Selected**.

### 3. Generate Part IDs
**Parts tab** → **Generate Part IDs** — assigns a unique ID to each piece and reads dimensions automatically.

The status bar shows how many objects have IDs and flags any without a material assigned.

### 4. Export BOM
**Planning tab** → **Export Detailed BOM** or **Export Consolidated BOM** — saves a CSV file next to your .blend file.

### 5. Generate Shopping List
**Planning tab** → set kerf, stock bonus and tolerance → **Generate Shopping List** — runs the cut optimizer and exports a timestamped CSV with the full cut plan.

---

## Supported Modifiers

CutVisor reads instance counts and dimensions from:

- **Array (Fixed Count)** — count read directly from modifier
- **Array (Fit Length)** — count estimated from evaluated mesh
- **Mirror** — axes read from modifier parameters
- **Geometry Nodes** — count cannot be auto-detected; set manually in the Parts list

---

## Compatibility

- Blender **4.0 and newer**, including **5.x**
- Windows, macOS, Linux
- Metric and Imperial units

---

## Language Support

CutVisor ships with translations for **14 languages:**
English · Français · Deutsch · Română · Polski · Español · Italiano · Português · Русский · 中文 · 日本語 · 한국어 · العربية · Türkçe

To add a new language or fix a translation:
1. **Settings → Export Translations CSV**
2. Add a new column with your language name
3. Fill in the translations
4. **Settings → Import Translations CSV**

---

## File Structure

```
cutvisor/
  __init__.py    — Blender addon registration & bl_info
  addon.py       — All addon logic (~7500 lines)
```

---

## Changelog

### v1.0.1
- New: Volume Filter (Parts tab) — live volume readout, Min/Max range select/exclude, "Select Same Volume" with adjustable tolerance
- New: dedicated JARCH Import tab, separated from Parts; tab bar now spans two rows for readability
- Improved: L×W×T dimension measurement reworked to use face area instead of vertex positions — fixes incorrect dimensions on bevelled, rotated, or inconsistently-modelled pieces
- Improved: Shift-click to extend selection in the Shopping List's Unmatched section, with highlighting for currently-selected rows

### v1.0.0
- Initial release
- Core: Materials, Stock Sizes, Part IDs, BOM export, viewport overlay, Shopping List with FFD cut optimizer
- Pro: CSV import, advanced overlay styling, wood presets, width variation, rip/cross-cut limits, multi-board splicing, JARCH Vis import

---

## License

CutVisor uses a dual license model:

### Core — GNU General Public License v3.0

The CutVisor Core addon code is free software: you can redistribute it
and/or modify it under the terms of the GNU General Public License as
published by the Free Software Foundation, either version 3 of the
License, or (at your option) any later version.

This program is distributed in the hope that it will be useful,
but WITHOUT ANY WARRANTY; without even the implied warranty of
MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.

See the full license text at: https://www.gnu.org/licenses/gpl-3.0.html

### Pro Features — Commercial License

The Pro features of CutVisor (CSV import for materials and stock,
advanced overlay styling, wood species presets, width variation and
rip/cross-cut limits, multi-board splicing, JARCH Vis import, and all
features unlocked by a `CWTW-…` activation key) are **not covered by the GPL**.

By purchasing a Pro key you agree to the following terms:

- The key is a **single-user, non-transferable** license
- You may install and use CutVisor Pro on computers you personally own or control
- **Redistribution of Pro activation keys is strictly prohibited**
- Reverse-engineering or bypassing the activation system to unlock Pro
  features without a valid key is not permitted
- The Pro license does not expire; future updates are provided at the
  author's discretion

To purchase a Pro key: [CutVisor Pro on Gumroad](https://cutvisor.gumroad.com/l/yiqahd)

---

## Links

- 🛒 [CutVisor Pro on Gumroad](https://cutvisor.gumroad.com/l/yiqahd)
- 🎬 [Video demo on YouTube](https://youtu.be/TOB81vilmtw)
- 🐛 [Report a bug](../../issues)
- 💬 [Discussions](../../discussions)
