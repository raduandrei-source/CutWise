# **CutWise — More Than a Plugin**

## Why CutWise exists

Every timber project starts with the same problem.

You have a model — drawn carefully, with correct dimensions, thought through structurally. Then comes the moment when you need to actually build it. You open a spreadsheet and start counting manually: how many pieces of 700mm, how many of 750mm, how many boards do I need to buy. You add a safety margin from instinct. You go to the supplier, buy too much or too little, spend the afternoon making cuts that could have been calculated in advance.

The offcuts pile up. Some will be used, most won't. At the end of the project there's a corner of the workshop full of pieces that are "too good to throw away" but too short to be useful for anything specific.

CutWise was built to solve exactly this — not as a grand ambition, but as a practical answer to a problem that repeats itself on every project.

The idea is simple: if the structure already exists as a 3D model, all the information needed to generate the shopping list and cut plan is already there. The computer should do this calculation, not the builder.

---

## The philosophy: less waste, better planning

Wood is a living material. It takes decades to grow. The idea of cutting a 4-metre board to use 600mm and throwing away the rest — when that could have been avoided with better planning — carries a weight that goes beyond the financial cost.

CutWise approaches the cut optimization problem the same way a careful craftsman would: sort the longest pieces first, fill each board as efficiently as possible, choose stock lengths that make sense for what you actually need. The algorithm (First-Fit Decreasing) isn't new — it's a well-studied solution to the cutting stock problem in operations research. What CutWise does is bring it directly into the design workflow, at the moment when it can actually make a difference.

The result is usually a 10–25% reduction in material waste compared to intuitive planning. On a small project that means a few boards. On a larger one it means real money and real material that doesn't end up in a skip.

There's also something else: when you know exactly what to buy before you start, the project becomes calmer. You go to the supplier with a precise list. You don't buy "a bit extra just in case." You cut with confidence, not with anxiety.

## Who CutWise is for

### Architects and architectural designers

You work in Blender for visualisation or concept modelling, and your projects increasingly include timber elements — cladding, structural frames, brise-soleil, interior joinery. CutWise lets you move from design to material specification without leaving Blender, and without rebuilding the model in a separate tool.

If your office uses Rhino, SketchUp or ArchiCAD, CutWise accepts imports from all of them. You don't need to rebuild the model — you import, assign materials, and generate the list.

### Structural and civil engineers

Timber frame structures, trusses, CLT-adjacent systems, lightweight prefabricated buildings. If you're modelling structural timber in Blender (or importing from a BIM tool via IFC), CutWise gives you a bill of materials that respects modifier-based repetition — Arrays and Mirrors are counted correctly, so a truss with 40 identical members generates one entry with quantity 40, not 40 separate lines.

### Interior designers and furniture makers

A custom bookshelf, a kitchen, a fitted wardrobe. These projects are full of pieces that look similar but aren't quite — slightly different heights, widths that vary by a centimetre. CutWise groups pieces by profile (width × thickness) and optimizes cuts within each group, which is exactly how a furniture maker thinks: all the 18mm boards together, all the 25mm boards together.

If you work with sheet goods (plywood, MDF, particle board panels), note that the current version handles linear cuts only — length optimization along one axis. Panel nesting (2D optimization) is planned for a future version.

### DIY builders and self-builders

You're building a deck, a garden shed, a timber-frame extension. You have a model — maybe rough, maybe precise — and you need to know what to buy before you drive to the builder's merchant. CutWise gives you a list you can print and take with you. No overbuying, no second trip because you ran short.

The free Core version is designed exactly for this: unlimited projects, no time limit, all the core functionality without payment. Download the addon, install it in Blender, and start using it immediately — no registration, no account. Pro features are unlocked by entering a licence key in the Settings tab.

### Makers and workshop regulars

You work with wood regularly — furniture, objects, installations. You've developed an instinct for material, but instinct has limits when projects get complex. CutWise lets you validate your instinct with numbers, quickly. It's not about replacing craft knowledge — it's about giving that knowledge a tool that keeps up with it.

## Feature explanations — what everything means

### Materials and pricing

**Material** in CutWise means a wood species with its physical properties: density (how heavy it is per cubic metre) and price (how much it costs per unit). You can price by kilogram, by cubic metre, or by piece — whichever matches how your supplier invoices.

**Density** is used for two things: calculating the mass of each piece (useful for structural checks and logistics) and calculating cost when you price by weight. Built-in presets cover the most common species. If you work with a species not in the list, you enter the density manually — it's the dry density of the wood, available from any timber supplier's data sheet or the EN 338 standard for structural timber.

**Currency** is display-only. CutWise doesn't convert between currencies — if your supplier prices in EUR, you enter EUR. If you have materials from different suppliers in different currencies, each material carries its own currency and the shopping list shows costs grouped by currency.

### Stock sizes

Stock sizes are the boards available from your supplier — the dimensions they actually sell. For each material you define a list of available lengths, widths and thicknesses, and optionally a price per board.

**Why this matters:** the cut optimizer can only work with stock that you've told it exists. If your supplier sells pine in 4000mm and 5000mm lengths at 150×40mm profile, those are the two entries in your stock list. The optimizer will pick between them automatically based on what fits best.

**Importing stock from a supplier:** most timber merchants and distributors can provide their catalogue as a spreadsheet (Excel or CSV). In the PRO version, you can import this file directly — no manual entry. The CSV format is straightforward: one row per stock size, columns for material name, length, width, thickness and price. If your supplier doesn't offer a download, a quick email to their technical department usually gets you a price list in spreadsheet format. Many larger distributors (Würth, Stark, Bywood, Holz-Her and others) already provide digital catalogues in formats compatible with CutWise's import.

The stock list is saved with your Blender file. You build it once for a given supplier and reuse it across projects. You can also export it to CSV for backup or sharing with colleagues.

### The cut optimizer — how it works in plain language

The optimizer answers one question: given the pieces I need and the boards available, what's the most efficient way to cut?

It works in three steps:

**Step 1 — Group by profile.** All pieces that share the same width and thickness go into the same group. A 700mm piece and a 750mm piece both at 150×40mm are in the same group and will be cut from the same boards. A 700mm piece at 150×40mm and a 700mm piece at 100×50mm are in different groups — you can't cut one from the other.

**Step 2 — Sort largest to smallest.** Within each group, pieces are sorted from longest to shortest. This is the "First-Fit Decreasing" heuristic — it's mathematically proven to produce near-optimal results for one-dimensional cutting problems. Long pieces are placed first because they're hardest to fit; short pieces fill the gaps left at the end.

**Step 3 — Fill boards.** Each piece is placed in the first open board that has enough remaining length. If no open board fits, a new board is opened. When choosing which stock length to open, the optimizer simulates all available lengths and picks the one that will pack the most material — not necessarily the shortest, and not necessarily the longest.

The result is a cut plan: which pieces go into which board, in what order, with the waste at the end of each board clearly shown.

### Kerf

The kerf is the width of material removed by the saw blade with each cut. A typical circular saw blade removes 2–4mm of material per cut. If you ignore kerf in planning, pieces don't fit — a board calculated to hold exactly six 650mm pieces will actually hold only five once you account for five saw cuts consuming 15mm total.

CutWise deducts kerf after every cut except the first on each board (the first piece doesn't require a cut from the board end — it starts at zero). The default is 3mm, which covers most circular saw and table saw blades. For a bandsaw you might use 1–2mm; for a thick-kerf demolition blade, 4–5mm.

### Stock length bonus

Boards sold as "4000mm" are rarely exactly 4000mm. Suppliers cut to nominal lengths with a small excess — typically 10–20mm — so that after the end is squared off, the piece is guaranteed to reach the nominal length. A "4000mm" board is often 4012–4018mm.

The stock length bonus tells CutWise to plan with the actual length rather than the nominal one. Set to 15mm, a 4000mm board is treated as 4015mm for planning purposes. The shopping list still shows "4000mm" (that's what you order), but the cut plan uses the real available length. This prevents the situation where pieces are calculated to fit but don't because the board was exactly nominal.

If you're unsure about your supplier's actual lengths, 15mm is a safe default for most European softwood suppliers. You can also measure a few boards from your next delivery and adjust.

### Measurement tolerance (W×T)

CutWise measures piece dimensions from the 3D model using an oriented bounding box — a method that gives correct results even for rotated or inclined objects. But measurements from 3D geometry are never perfect: rounding in the model, small floating-point differences, or the fact that wood dimensions vary by a millimetre or two in real life.

The measurement tolerance handles this. With a tolerance of 2mm (the default), a piece measured at 152mm will match a 150mm stock profile. Without tolerance, it wouldn't match anything and would appear in the unmatched list.

Keep this value small — 1 to 3mm is appropriate for wood. Setting it to 50mm to force matches is the wrong solution; that just creates incorrect groupings. If pieces aren't matching, the right fix is to add the correct stock profile to the material, or to check whether the model dimensions are what you intended.

### Width variation

Width variation is a separate setting from measurement tolerance, and the distinction matters.

**Measurement tolerance** covers the difference between the modelled dimension and the actual board dimension — both should be the same profile, just measured slightly differently.

**Width variation** covers the case where you intentionally buy stock that is slightly wider than the finished piece, and rip it to width on the table saw. For example: you need pieces that are 148mm wide, but your supplier only stocks 150mm boards. With a width variation of 5mm, CutWise will match your 148mm pieces to 150mm stock, knowing you'll rip off 2mm.

At the default value of 0mm, this is disabled — a 150mm stock will not be offered for a 148mm piece, and a 200mm stock will not be offered for a 150mm piece regardless of how high you set measurement tolerance. This prevents accidental matches between completely different stock sizes.

Only raise width variation when you have a specific ripping step in your workflow.

### Rip-cut warning

When width variation is in use and a piece is matched to stock that is wider than needed, CutWise marks this clearly in the shopping list and the exported CSV. The notation shows the stock width and the target width: "Rip to width: 155mm → 150mm".

This warning exists because ripping on a table saw is a different operation from cross-cutting, and it affects your workflow: you need the right blade, the right fence setup, and enough time. Knowing in advance which boards need ripping avoids surprises.

**Rip Cut Max Width** takes this one step further. If you set it to your saw's maximum rip capacity — for example, 250mm for a fixed circular saw — CutWise will flag any board that exceeds this as a saw-capacity warning. This is shown in red in the panel and marked clearly in the CSV:

*"RIP TO WIDTH 280mm → 150mm — EXCEEDS SAW CAPACITY (250mm)"*

Set to 0 (the default), no capacity check is performed.

### JARCH Vis import (PRO)

JARCH Vis is a Blender addon that creates architectural cladding — siding, floors, roof boarding — as single mesh objects containing many individual boards. CutWise PRO can split these objects into individual measurable pieces automatically.

The import process handles several problems that arise from how JARCH creates geometry:

**Stray faces from boolean cuts.** When siding is trimmed around windows or doors, boolean operations sometimes leave behind isolated 2D faces — flat geometry with no volume. CutWise detects these (by thickness and planarity) and removes them before counting.

**Open mesh edges.** Cutting a board leaves an open face where the cut was made. CutWise fills these holes automatically, which is necessary for correct volume calculations and for some downstream tools.

**Dimension measurement on inclined boards.** Horizontal siding boards are not aligned with the world axes — they sit at an angle on the wall. Standard bounding box measurement gives inflated dimensions for rotated objects (a 20mm thick board inclined at 60° would measure as 40mm using axis-aligned bounding box). CutWise uses Principal Component Analysis (PCA) on the mesh vertices, which finds the three principal axes of the piece regardless of its orientation in the world. This gives correct dimensions — length, width and thickness — whether the board is horizontal, vertical, inclined or fully rotated. PCA works correctly even when transforms have been applied to the mesh, and is unaffected by other modifiers in the stack (Solidify, Bevel, Subdivision).

After import, pieces appear in a dedicated Blender collection named after the original object, ready for material assignment and Part ID generation.

### Viewport labels

The View tab controls labels drawn directly in the 3D viewport for any selected object. You can show any combination of Part ID, object name, dimensions and instance count. Labels are anchored relative to the N-panel (right sidebar) or T-panel (left toolbar) — not to the screen edge — so they never overlap with the CutWise panel or any other Blender panel, even if you resize them.

Shadow and outline options make labels readable over any background colour. Four one-click presets (All, IDs only, Dims+Count, Off) let you switch between information modes quickly during a work session.

All overlay settings update the viewport instantly when changed — no need to move the camera to trigger a refresh.

### The export folder

All files exported by CutWise — shopping list, cut plan, bill of materials — are saved to a folder named after your Blender file, created automatically in the same location:

my\_house.blend

my\_house\_lists/

    Shopping\_List.csv

    BOM\_Detailed.csv

    BOM\_Consolidated.csv

This folder is created the first time you export, and all subsequent exports go to the same place. The "Open Lists Folder" button opens it directly in Explorer (Windows), Finder (Mac) or Files (Linux) — no manual navigation needed.

### The shopping list CSV

Each time you generate a shopping list, CutWise saves a new timestamped file — `Shopping_List_20240615_143022.csv` — so previous versions are never overwritten. This lets you compare different configurations side by side: different kerf settings, different stock lengths, different groupings.

The exported file has three sections:

**Section 1 — Shopping list.** What to order from the supplier: material, profile (width × thickness), stock length, quantity, unit price, subtotal, waste percentage. This is the list you take to the merchant or send by email. If a board needs to be ripped to width, a note appears in the Notes column.

**Section 2 — Cut plan.** For each board purchased, the sequence of cuts: which pieces go into it, in what order, how much waste remains at the end. This is the list you take to the saw.

**Section 3 — Unmatched pieces.** Any pieces that couldn't be matched to a stock size, with the reason. Common causes: no stock defined for that material; the profile doesn't match any stock; the piece is longer than all available stock lengths. This section being empty means the plan is complete.

If you work with **Geometry Nodes** to create arrays, CutWise detects the modifier and shows a warning with an option to convert it to a legacy Array modifier directly from the Parts panel. After conversion, counts are read automatically. This is the recommended workflow for GN-based arrays in projects that need cut optimisation.

### Bills of Materials

**Detailed BOM:** one row per object, showing material, dimensions, quantity (expanded from Array/Mirror modifiers), Part ID and object name. Useful for tracking individual elements or for quantity surveying.

**Consolidated BOM:** one row per unique piece type (material \+ dimensions), with total quantity. Useful for procurement, for structural schedules, and for comparing against the shopping list.

Both files use UTF-8 encoding with BOM, which means they open correctly in Excel on all operating systems without character encoding problems.

## A note on what CutWise doesn't do (yet)

**Sheet goods.** Plywood, OSB, MDF, particle board — optimizing these requires 2D nesting (fitting rectangles into a panel), which is a different and more complex problem than linear cutting. It's on the roadmap.

**Curved or non-prismatic elements.** CutWise measures straight pieces with rectangular cross-sections. Curved beams, tapered elements, or complex profiles need manual handling.

**Structural calculations.** CutWise tells you what material to buy, not whether it will stand up. Structural verification follows its own process and standards (EC5 for timber in Europe). The two are complementary, not competitive.

**Connection hardware.** Bolts, screws, brackets, joist hangers — not in scope for this version.

These are not afterthoughts. They define the boundary of what CutWise promises to do well, which is the one thing it actually does: tell you what timber to buy and how to cut it.

## Activation — Core and Pro

CutWise is distributed as a single file. When installed without a licence key, it runs in Core mode. Entering a valid Pro key in **Settings → Enter / Change Key** unlocks all Pro features instantly — no restart required.

The licence key has the format `CWTW-XXXX-XXXX-XXXX-XXXX`. It is validated locally using a cryptographic signature — no internet connection is required for activation or for continued use.

**To activate Pro:**
1. Purchase a key at [gumroad.com/cutwise](https://gumroad.com/YOUR_LINK)
2. Open Blender → N-panel → CutWise → **Settings**
3. Click **Paste Key** (if the key is in your clipboard) or **Enter / Change Key**
4. The status line changes to "✓ CutWise Pro active"

A single key is licensed for personal use on computers you own. Keys do not expire and remain valid across updates.

---

## Licence

CutWise uses a dual licence model.

### Core — GNU General Public License v3.0

The CutWise Core addon code is free software: you can redistribute it and/or modify it under the terms of the GNU General Public License as published by the Free Software Foundation, version 3 or later.

Full licence text: https://www.gnu.org/licenses/gpl-3.0.html

### Pro Features — Commercial Licence

The Pro features of CutWise — Shopping List optimizer, advanced overlay, wood species presets, Auto-Assign from Import, and all features unlocked by a `CWTW-…` activation key — are not covered by the GPL.

By purchasing a Pro key you agree to the following terms:

- The key is a single-user, non-transferable licence
- You may install and use CutWise Pro on computers you personally own or control
- Redistribution of Pro activation keys is strictly prohibited
- Reverse-engineering or bypassing the activation system is not permitted
- The Pro licence does not expire; future updates are provided at the author's discretion

---

## Final thought

The best tools are the ones you forget you're using. You're thinking about the project — the structure, the joinery, the proportions — and the tool handles the accounting quietly in the background.

CutWise won't make you a better designer. But it will make sure that what you designed is what gets built, at the right cost, with the right amount of material, and with less waste in the skip at the end.

That feels like enough.

*CutWise is developed by an architecture faculty member with an interest in computational design and sustainable timber construction.* *Feedback, feature requests and bug reports are genuinely welcome — this tool grows with its users.*  
