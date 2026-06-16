# CutWise — Timber Cut Optimizer for Blender

**design & craft with minimum waste**

CutWise is a Blender addon for timber structure projects. Assign part IDs to your components, calculate mass/volume/cost, export Bills of Materials, and generate an optimized cut plan — so you know exactly what boards to buy and how to cut them.

![CutWise hero](docs/hero.png)

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
| Lists CSV import/export | ✓ | ✓ |
| Multi-language UI (14 languages) | ✓ | ✓ |
| **Rip Cut, Cross-Cut, Long Rip settings** | — | ✓ |
| **Width Variation** | — | ✓ |
| **Multi-Board Splicing** | — | ✓ |
| **Wood species density presets** | — | ✓ |
| **Overlay: color, shadow, outline** | — | ✓ |
| **Import CSV stock dimensions** | — | ✓ |

👉 **[Get CutWise Pro on Gumroad →](https://cutwise.gumroad.com/l/yiqahd)**

---

## Installation

1. Download `cutwise_v1.zip` from [Releases](../../releases)
2. In Blender: **Edit → Preferences → Add-ons → Install**
3. Select the zip file → **Install Add-on**
4. Enable **CutWise** in the add-ons list
5. Open the **N-panel** in any 3D Viewport → **CutWise** tab

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

### 5. Generate Shopping List *(Pro)*
**Planning tab** → set kerf, stock bonus and tolerance → **Generate Shopping List** — runs the cut optimizer and exports a timestamped CSV with the full cut plan.

---

## Supported Modifiers

CutWise reads instance counts and dimensions from:

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

CutWise ships with translations for **14 languages:**
English · Français · Deutsch · Română · Polski · Español · Italiano · Português · Русский · 中文 · 日本語 · 한국어 · العربية · Türkçe

To add a new language or fix a translation:
1. **Settings → Export Translations CSV**
2. Add a new column with your language name
3. Fill in the translations
4. **Settings → Import Translations CSV**

---

## File Structure

```
cutwise_v1/
  __init__.py    — Blender addon registration & bl_info
  addon.py       — All addon logic (~7500 lines)
```

---

## Changelog

### v1.0.0
- Initial release
- Core: Materials, Stock Sizes, Part IDs, BOM export, viewport overlay
- Pro: Shopping List with FFD cut optimizer, advanced overlay, wood presets

---

## License

CutWise uses a dual license model:

### Core — GNU General Public License v3.0

The CutWise Core addon code is free software: you can redistribute it
and/or modify it under the terms of the GNU General Public License as
published by the Free Software Foundation, either version 3 of the
License, or (at your option) any later version.

This program is distributed in the hope that it will be useful,
but WITHOUT ANY WARRANTY; without even the implied warranty of
MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.

See the full license text at: https://www.gnu.org/licenses/gpl-3.0.html

### Pro Features — Commercial License

The Pro features of CutWise (Shopping List optimizer, advanced overlay,
wood species presets, Auto-Assign from Import and all features unlocked
by a `CWTW-…` activation key) are **not covered by the GPL**.

By purchasing a Pro key you agree to the following terms:

- The key is a **single-user, non-transferable** license
- You may install and use CutWise Pro on computers you personally own or control
- **Redistribution of Pro activation keys is strictly prohibited**
- Reverse-engineering or bypassing the activation system to unlock Pro
  features without a valid key is not permitted
- The Pro license does not expire; future updates are provided at the
  author's discretion

To purchase a Pro key: [CutWise Pro on Gumroad](https://gumroad.com/YOUR_LINK)

---

## Links

- 🛒 [CutWise Pro on Gumroad](https://cutwise.gumroad.com/l/yiqahd)
- 🎬 [Video demo on YouTube](https://youtu.be/flGE_SqiUrQ)
- 🐛 [Report a bug](../../issues)
- 💬 [Discussions](../../discussions)
