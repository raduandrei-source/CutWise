<style>
#cv-toc-root {
  position: fixed;
  top: 70px;
  left: 0;
  width: 250px;
  max-height: 85vh;
  overflow-y: auto;
  font-size: 13px;
  line-height: 1.5;
  border-right: 1px solid #d0d7de;
  z-index: 100;
}
#cv-lang-switch {
  padding: 0 18px 14px 18px;
  border-bottom: 1px solid #d0d7de;
  margin-bottom: 10px;
}
#cv-lang-switch label {
  display: block;
  font-size: 11px;
  text-transform: uppercase;
  letter-spacing: 0.04em;
  color: #57606a;
  margin-bottom: 4px;
}
#cv-lang-select {
  width: 100%;
  font-size: 13px;
  padding: 4px 6px;
  border: 1px solid #d0d7de;
  border-radius: 4px;
  background: #fff;
}
#cv-toc { padding: 0 18px 30px 18px; }
.cv-toc-item { margin-bottom: 2px; }
.cv-toc-h2 {
  display: block;
  color: #24292f;
  font-weight: 600;
  text-decoration: none;
  padding: 4px 0;
  position: relative;
  padding-left: 14px;
}
.cv-toc-h2::before {
  content: "\25B8";
  position: absolute;
  left: 0;
  font-size: 10px;
  color: #57606a;
  transition: transform 0.15s ease;
}
.cv-toc-item.open > .cv-toc-h2::before { transform: rotate(90deg); }
.cv-toc-h2:hover { color: #0969da; }
.cv-toc-children {
  padding-left: 14px;
  border-left: 1px solid #d0d7de;
  margin-left: 4px;
}
.cv-toc-h3 {
  display: block;
  color: #57606a;
  font-size: 12px;
  text-decoration: none;
  padding: 3px 0;
}
.cv-toc-h3:hover { color: #0969da; text-decoration: underline; }

.lang-block { display: none; }

@media (max-width: 1150px) {
  #cv-toc-root { display: none; }
}
@media (min-width: 1151px) {
  .markdown-body {
    max-width: 760px !important;
    margin-left: 300px !important;
    margin-right: auto !important;
  }
}
</style>

<div id="cv-toc-root">
  <div id="cv-lang-switch">
    <label for="cv-lang-select">Language</label>
    <select id="cv-lang-select">
      <option value="en">English</option>
      <option value="fr">Français</option>
      <option value="es">Español</option>
      <option value="pt">Português</option>
    </select>
  </div>
  <nav id="cv-toc"></nav>
</div>

<script>
(function () {
  var LANGS = ["en", "fr", "es", "pt"];

  function buildTOC(lang) {
    var toc = document.getElementById("cv-toc");
    toc.innerHTML = "";
    var container = document.getElementById("lang-" + lang);
    if (!container) return;
    var headings = container.querySelectorAll("h2, h3");
    var currentChildWrap = null;

    headings.forEach(function (h, idx) {
      if (!h.id) {
        h.id = lang + "-" + h.textContent.trim().toLowerCase().replace(/[^\w]+/g, "-") + "-" + idx;
      }

      if (h.tagName === "H2") {
        var item = document.createElement("div");
        item.className = "cv-toc-item";

        var link = document.createElement("a");
        link.href = "#" + h.id;
        link.textContent = h.textContent;
        link.className = "cv-toc-h2";

        var childWrap = document.createElement("div");
        childWrap.className = "cv-toc-children";
        childWrap.style.display = "none";

        link.addEventListener("click", function () {
          var isOpen = item.classList.contains("open");
          toc.querySelectorAll(".cv-toc-item").forEach(function (i) {
            i.classList.remove("open");
            i.querySelector(".cv-toc-children").style.display = "none";
          });
          if (!isOpen && childWrap.children.length > 0) {
            item.classList.add("open");
            childWrap.style.display = "block";
          }
        });

        item.appendChild(link);
        item.appendChild(childWrap);
        toc.appendChild(item);
        currentChildWrap = childWrap;
      } else if (h.tagName === "H3" && currentChildWrap) {
        var sub = document.createElement("a");
        sub.href = "#" + h.id;
        sub.textContent = h.textContent;
        sub.className = "cv-toc-h3";
        currentChildWrap.appendChild(sub);
      }
    });
  }

  function setLanguage(lang) {
    LANGS.forEach(function (l) {
      var el = document.getElementById("lang-" + l);
      if (el) el.style.display = (l === lang) ? "block" : "none";
    });
    buildTOC(lang);
    try { localStorage.setItem("cv-lang", lang); } catch (e) {}
  }

  document.addEventListener("DOMContentLoaded", function () {
    var select = document.getElementById("cv-lang-select");
    var saved = "en";
    try { saved = localStorage.getItem("cv-lang") || "en"; } catch (e) {}
    if (LANGS.indexOf(saved) === -1) saved = "en";
    select.value = saved;
    setLanguage(saved);
    select.addEventListener("change", function () {
      setLanguage(this.value);
    });
  });
})();
</script>

<div id="lang-en" class="lang-block" markdown="1">

# **CutVisor — More Than a Plugin**

## Why CutVisor exists

Every timber project starts with the same problem.

You have a model — drawn carefully, with correct dimensions, thought through structurally. Then comes the moment when you need to actually build it. You open a spreadsheet and start counting manually: how many pieces of 700mm, how many of 750mm, how many boards do I need to buy. You add a safety margin from instinct. You go to the supplier, buy too much or too little, spend the afternoon making cuts that could have been calculated in advance.

The offcuts pile up. Some will be used, most won't. At the end of the project there's a corner of the workshop full of pieces that are "too good to throw away" but too short to be useful for anything specific.

CutVisor was built to solve exactly this — not as a grand ambition, but as a practical answer to a problem that repeats itself on every project.

The idea is simple: if the structure already exists as a 3D model, all the information needed to generate the shopping list and cut plan is already there. The computer should do this calculation, not the builder.

---

## The philosophy: less waste, better planning

Wood is a living material. It takes decades to grow. The idea of cutting a 4-metre board to use 600mm and throwing away the rest — when that could have been avoided with better planning — carries a weight that goes beyond the financial cost.

CutVisor approaches the cut optimization problem the same way a careful craftsman would: sort the longest pieces first, fill each board as efficiently as possible, choose stock lengths that make sense for what you actually need. The algorithm (First-Fit Decreasing) isn't new — it's a well-studied solution to the cutting stock problem in operations research. What CutVisor does is bring it directly into the design workflow, at the moment when it can actually make a difference.

The result is usually a 10–25% reduction in material waste compared to intuitive planning. On a small project that means a few boards. On a larger one it means real money and real material that doesn't end up in a skip.

There's also something else: when you know exactly what to buy before you start, the project becomes calmer. You go to the supplier with a precise list. You don't buy "a bit extra just in case." You cut with confidence, not with anxiety.

## Who CutVisor is for

### Architects and architectural designers

You work in Blender for visualisation or concept modelling, and your projects increasingly include timber elements — cladding, structural frames, brise-soleil, interior joinery. CutVisor lets you move from design to material specification without leaving Blender, and without rebuilding the model in a separate tool.

If your office uses Rhino, SketchUp or ArchiCAD, CutVisor accepts imports from all of them. You don't need to rebuild the model — you import, assign materials, and generate the list.

### Structural and civil engineers

Timber frame structures, trusses, lightweight prefabricated buildings. If you're modelling structural timber in Blender (or importing from a BIM tool via IFC), CutVisor gives you a bill of materials that respects modifier-based repetition — Arrays and Mirrors are counted correctly, so a truss with 40 identical members generates one entry with quantity 40, not 40 separate lines. Note that CutVisor optimizes linear members; large panel products such as CLT are sheet goods and fall under the panel-nesting limitation described below.

### Interior designers and furniture makers

A custom bookshelf, a kitchen, a fitted wardrobe. These projects are full of pieces that look similar but aren't quite — slightly different lengths, a leg stock or face-frame strip that varies by a centimetre from the next. CutVisor groups pieces by profile (width × thickness) and optimizes cuts within each group, which is exactly how a furniture maker thinks about solid-wood components: all the 40×18mm battens together, all the 60×25mm rails together.

If your project is built mainly from sheet goods (plywood, MDF, particle board panels) rather than solid-wood components, note that the current version handles linear cuts only — length optimization along one axis. Panel nesting (2D optimization) is planned for a future version.

### DIY builders and self-builders

You're building a deck, a garden shed, a timber-frame extension. You have a model — maybe rough, maybe precise — and you need to know what to buy before you drive to the builder's merchant. CutVisor gives you a list you can print and take with you. No overbuying, no second trip because you ran short.

The free Core version is designed exactly for this: unlimited projects, no time limit, all the core functionality without payment. Download the addon, install it in Blender, and start using it immediately — no registration, no account. Pro features are unlocked by entering a licence key in the Settings tab.

### Makers and workshop regulars

You work with wood regularly — furniture, objects, installations. You've developed an instinct for material, but instinct has limits when projects get complex. CutVisor lets you validate your instinct with numbers, quickly. It's not about replacing craft knowledge — it's about giving that knowledge a tool that keeps up with it.

## Feature explanations — what everything means

### Volume Filter (Parts tab)

Most real imports aren't clean. A model brought in from Rhino, SketchUp or IFC typically lands as one large collection containing not just the timber pieces but every screw, bolt, and bracket modelled along with them — sometimes hundreds of small objects mixed in with the boards you actually want to cut. Sorting these out by hand, one click at a time, doesn't scale.

The Volume Filter gives you a fast way to separate the two by size, since hardware is reliably tiny compared to timber:

**Active object volume** is shown live, with no button to click — select anything in the viewport and its volume reads directly in cm³. Select a screw to see what you're dealing with, select a board to compare.

**Min / Max fields** define a volume range. Next to each is a small eyedropper button: select an object, click the eyedropper, and that object's volume is copied straight into the field — no need to type a number you'd otherwise have to read off the active-object display first.

**Select in Range** selects every eligible object in the scene whose volume falls inside Min–Max. **Exclude in Range** does the opposite on your *current* selection: select everything broadly first, then Exclude in Range to drop out whatever falls in that band — typically the hardware — leaving the timber pieces selected and ready for **Generate Part IDs**, which only ever acts on the current selection.

**Select Same Volume** goes one step further: select one screw, click the button, and every object with a closely matching volume is selected at once — useful when a project uses several different fastener sizes and you want to isolate just one type. The **Tolerance** field (1% by default) controls how close a match needs to be; raise it if your hardware has small modelling variations, lower it if you need an exact match.

None of this requires a material to be assigned first — volume is read directly from the mesh geometry, so filtering can happen as the very first step after import, before any material or Part ID workflow begins.

### Materials and pricing

**Material** in CutVisor means a wood species with its physical properties: density (how heavy it is per cubic metre) and price (how much it costs per unit). You can price by kilogram, by cubic metre, or by piece — whichever matches how your supplier invoices.

**Density** is used for two things: calculating the mass of each piece (useful for structural checks and logistics) and calculating cost when you price by weight. Built-in presets cover the most common species. If you work with a species not in the list, you enter the density manually — it's the dry density of the wood, available from any timber supplier's data sheet or the EN 338 standard for structural timber.

**Currency** is display-only. CutVisor doesn't convert between currencies — if your supplier prices in EUR, you enter EUR. If you have materials from different suppliers in different currencies, each material carries its own currency and the shopping list shows costs grouped by currency.

### Stock sizes

Stock sizes are the boards available from your supplier — the dimensions they actually sell. For each material you define a list of available lengths, widths and thicknesses, and optionally a price per board.

**Why this matters:** the cut optimizer can only work with stock that you've told it exists. If your supplier sells pine in 4000mm and 5000mm lengths at 150×40mm profile, those are the two entries in your stock list. The optimizer will pick between them automatically based on what fits best.

**Importing stock from a supplier:** many timber merchants and distributors can provide their catalogue as a spreadsheet (Excel or CSV). In the PRO version, you can import this file directly — no manual entry. The CSV format is straightforward: one row per stock size, columns for material name, length, width, thickness and price. If your supplier doesn't offer a download, a quick email to their sales or technical department will usually get you a price list in spreadsheet format.

The stock list is saved with your Blender file. You build it once for a given supplier and reuse it across projects. You can also export it to CSV for backup or sharing with colleagues.

### The cut optimizer — how it works in plain language

The optimizer answers one question: given the pieces I need and the boards available, what's the most efficient way to cut?

It works in three steps:

**Step 1 — Group by profile.** All pieces that share the same width and thickness go into the same group. A 700mm piece and a 750mm piece both at 150×40mm are in the same group and will be cut from the same boards. A 700mm piece at 150×40mm and a 700mm piece at 100×50mm are in different groups — you can't cut one from the other.

**Step 2 — Sort largest to smallest.** Within each group, pieces are sorted from longest to shortest. This is the "First-Fit Decreasing" heuristic — it's mathematically proven to produce near-optimal results for one-dimensional cutting problems. Long pieces are placed first because they're hardest to fit; short pieces fill the gaps left at the end.

**Step 3 — Fill boards.** Each piece is placed in the first open board that has enough remaining length. If no open board fits, a new board is opened. When choosing which stock length to open, the optimizer simulates all available lengths and picks the one that will pack the most material — not necessarily the shortest, and not necessarily the longest.

The result is a cut plan: which pieces go into which board, in what order, with the waste at the end of each board clearly shown.

### Kerf

The kerf is the width of material removed by the saw blade with each cut. A typical circular saw or table saw blade removes 2–4mm of material per cut. If you ignore kerf in planning, pieces don't fit — a board calculated to hold exactly six 650mm pieces will actually hold only five once you account for five saw cuts consuming 15mm total.

CutVisor deducts kerf after every cut except the first on each board (the first piece doesn't require a cut from the board end — it starts at zero). The default is 2mm, which covers most circular saw and table saw blades. For a bandsaw, which cuts a much narrower kerf, 1mm is closer to reality; for a wider blade on larger sections, 3–4mm.

### Stock length bonus

Boards sold as "4000mm" are rarely exactly 4000mm. Suppliers typically cut to nominal lengths with a small excess — so that after the end is squared off, the piece is guaranteed to reach the nominal length.

The stock length bonus tells CutVisor to plan with the actual length rather than the nominal one. Set to 15mm, a 4000mm board is treated as 4015mm for planning purposes. The shopping list still shows "4000mm" (that's what you order), but the cut plan uses the real available length. This prevents the situation where pieces are calculated to fit but don't because the board was exactly nominal.

If you're not sure how much excess your supplier leaves, measure a few boards from your next delivery and set this value accordingly — it varies by supplier and by sawmill, so there's no single figure that applies everywhere.

### Measurement tolerance (W×T)

CutVisor measures piece dimensions from the 3D model using an oriented bounding box — a method that gives correct results even for rotated or inclined objects. But measurements from 3D geometry are never perfect: rounding in the model, small floating-point differences, or the fact that wood dimensions vary by a millimetre or two in real life.

The measurement tolerance handles this. With a tolerance of 5mm (the default), a piece measured at 152mm will match a 150mm stock profile. Without tolerance, it wouldn't match anything and would appear in the unmatched list.

If you want tighter control over matching, a smaller value (1–3mm) is appropriate for most timber work. Setting it very high to force matches is the wrong solution; that just creates incorrect groupings. If pieces aren't matching, the right fix is to add the correct stock profile to the material, or to check whether the model dimensions are what you intended.

### Width variation

Width variation is a separate setting from measurement tolerance, and the distinction matters.

**Measurement tolerance** covers the difference between the modelled dimension and the actual board dimension — both should be the same profile, just measured slightly differently.

**Width variation** covers the case where you intentionally buy stock that is slightly wider than the finished piece, and rip it to width on the table saw. For example: you need pieces that are 148mm wide, but your supplier only stocks 150mm boards. With a width variation of 5mm, CutVisor will match your 148mm pieces to 150mm stock, knowing you'll rip off 2mm.

By default, width variation is set to 10mm — a piece up to 10mm narrower than an available stock width is matched automatically, on the assumption you'll rip it down. A 148mm piece will be matched to 150mm stock (2mm to rip off), but a 150mm piece will not be matched to a 200mm stock — 50mm is well outside the default range, which prevents accidental matches between genuinely different stock sizes regardless of how high you set measurement tolerance.

Set it to 0mm if you never rip to width and want only close, non-ripped matches. Raise it if ripping is a routine step in your workflow and your boards differ more from your target widths than the default covers.

### Rip-cut warning

When width variation is in use and a piece is matched to stock that is wider than needed, CutVisor marks this clearly in the shopping list and the exported CSV. The notation shows the stock width and the target width: "Rip to width: 155mm → 150mm".

This warning exists because ripping on a table saw is a different operation from cross-cutting, and it affects your workflow: you need the right blade, the right fence setup, and enough time. Knowing in advance which boards need ripping avoids surprises.

**Rip Cut Max Width** takes this one step further. If you set it to your saw's maximum rip capacity — for example, 250mm for a fixed circular saw — CutVisor will flag any board that exceeds this as a saw-capacity warning. This is shown in red in the panel and marked clearly in the CSV:

*"RIP TO WIDTH 280mm → 150mm — EXCEEDS SAW CAPACITY (250mm)"*

Set to 0 (the default), no capacity check is performed.

### JARCH Vis import (PRO)

JARCH Vis is a Blender addon that creates architectural cladding — siding, floors, roof boarding — as single mesh objects containing many individual boards. CutVisor PRO can split these objects into individual measurable pieces automatically.

The import process handles several problems that arise from how JARCH creates geometry:

**Stray faces from boolean cuts.** When siding is trimmed around windows or doors, boolean operations sometimes leave behind isolated 2D faces — flat geometry with no volume. CutVisor detects these (by thickness and planarity) and removes them before counting.

**Open mesh edges.** Cutting a board leaves an open face where the cut was made. CutVisor fills these holes automatically, which is necessary for correct volume calculations and for some downstream tools.

**Dimension measurement on inclined boards.** Horizontal siding boards are not aligned with the world axes — they sit at an angle on the wall. Standard bounding box measurement gives inflated dimensions for rotated objects (a 20mm thick board inclined at 60° would measure as 40mm using axis-aligned bounding box). CutVisor instead identifies each axis from face area: the dominant flat faces (front/back of the board) give the thickness direction, and the long side edges — by far the next-largest flat surfaces on a simple board — give the width direction; length is whatever's left over. Because this relies on surface area rather than vertex positions, it stays accurate even on finely tessellated imports, bevelled edges, or boards with a chamfer missing at one end — and it works correctly whether the board is horizontal, vertical, inclined or fully rotated.

This measurement is taken from the base mesh, before modifiers are evaluated — non-destructive modifiers like Solidify, Bevel or Subdivision Surface won't be reflected in the reported dimensions. Apply such a modifier first if you need it included in the measurement.

After import, pieces appear in a dedicated Blender collection named after the original object, ready for material assignment and Part ID generation.

### Viewport labels

The View tab controls labels drawn directly in the 3D viewport for any selected object. You can show any combination of Part ID, object name, dimensions and instance count. Labels are anchored relative to the N-panel (right sidebar) or T-panel (left toolbar) — not to the screen edge — so they never overlap with the CutVisor panel or any other Blender panel, even if you resize them.

Shadow and outline options make labels readable over any background colour. Four one-click presets (All, IDs only, Dims+Count, Off) let you switch between information modes quickly during a work session.

All overlay settings update the viewport instantly when changed — no need to move the camera to trigger a refresh.

### The export folder

All files exported by CutVisor — shopping list, cut plan, bill of materials — are saved to a folder named after your Blender file, created automatically in the same location:

my\_house.blend

my\_house\_lists/

    Shopping\_List.csv

    BOM\_Detailed.csv

    BOM\_Consolidated.csv

This folder is created the first time you export, and all subsequent exports go to the same place. The "Open Lists Folder" button opens it directly in Explorer (Windows), Finder (Mac) or Files (Linux) — no manual navigation needed.

### The shopping list CSV

Each time you generate a shopping list, CutVisor saves a new timestamped file — `Shopping_List_20240615_143022.csv` — so previous versions are never overwritten. This lets you compare different configurations side by side: different kerf settings, different stock lengths, different groupings.

The exported file has three sections:

**Section 1 — Shopping list.** What to order from the supplier: material, profile (width × thickness), stock length, quantity, unit price, subtotal, waste percentage. This is the list you take to the merchant or send by email. If a board needs to be ripped to width, a note appears in the Notes column.

**Section 2 — Cut plan.** For each board purchased, the sequence of cuts: which pieces go into it, in what order, how much waste remains at the end. This is the list you take to the saw.

**Section 3 — Unmatched pieces.** Any pieces that couldn't be matched to a stock size, with the reason. Common causes: no stock defined for that material; the profile doesn't match any stock; the piece is longer than all available stock lengths. This section being empty means the plan is complete.

If you work with **Geometry Nodes** to create arrays, CutVisor detects the modifier and shows a warning with an option to convert it to a legacy Array modifier directly from the Parts panel. After conversion, counts are read automatically. This is the recommended workflow for GN-based arrays in projects that need cut optimisation.

### Bills of Materials

**Detailed BOM:** one row per object, showing material, dimensions, quantity (expanded from Array/Mirror modifiers), Part ID and object name. Useful for tracking individual elements or for quantity surveying.

**Consolidated BOM:** one row per unique piece type (material \+ dimensions), with total quantity. Useful for procurement, for structural schedules, and for comparing against the shopping list.

Both files are saved as UTF-8 text with a leading byte-order mark, which means they open correctly in Excel on all operating systems without character encoding problems.

## A note on what CutVisor doesn't do (yet)

**Sheet goods.** Plywood, OSB, MDF, particle board, CLT panels — optimizing these requires 2D nesting (fitting rectangles into a panel), which is a different and more complex problem than linear cutting. It's on the roadmap.

**Curved or non-prismatic elements.** CutVisor measures straight pieces with rectangular cross-sections. Curved beams, tapered elements, or complex profiles need manual handling.

**Structural calculations.** CutVisor tells you what material to buy, not whether it will stand up. Structural verification follows its own process and standards (EC5 for timber in Europe). The two are complementary, not competitive.

**Connection hardware.** Bolts, screws, brackets, joist hangers — not in scope for this version.

These are not afterthoughts. They define the boundary of what CutVisor promises to do well, which is the one thing it actually does: tell you what timber to buy and how to cut it.

## Activation — Core and Pro

CutVisor is distributed as a single add-on package. When installed without a licence key, it runs in Core mode. Entering a valid Pro key in **Settings → Enter / Change Key** unlocks all Pro features instantly — no restart required.

The licence key has the format `CWTW-XXXX-XXXX-XXXX-XXXX`. It is validated locally using a cryptographic signature — no internet connection is required for activation or for continued use.

**To activate Pro:**
1. Purchase a key at https://cutvisor.gumroad.com/l/yiqahd
2. Copy the key from your confirmation email or Gumroad library
3. Open Blender → N-panel → CutVisor → **Settings**
4. Click **Paste Key** (if the key is in your clipboard) or **Enter / Change Key**
5. The status line changes to "✓ CutVisor Pro active"

A single key is licensed for personal use on computers you own. Keys do not expire and remain valid across updates.

---

## Licence

CutVisor uses a dual licence model.

### Core — GNU General Public License v3.0

The CutVisor Core addon code is free software: you can redistribute it and/or modify it under the terms of the GNU General Public License as published by the Free Software Foundation, version 3 or later.

Full licence text: https://www.gnu.org/licenses/gpl-3.0.html

### Pro Features — Commercial Licence

The Pro features of CutVisor — CSV import for materials and stock, advanced overlay styling, wood species presets, saved custom material presets, width variation and rip/cross-cut limits, multi-board splicing, JARCH Vis import, and all features unlocked by a `CWTW-…` activation key — are not covered by the GPL.

By purchasing a Pro key you agree to the following terms:

- The key is a single-user, non-transferable licence
- You may install and use CutVisor Pro on computers you personally own or control
- Redistribution of Pro activation keys is strictly prohibited
- Reverse-engineering or bypassing the activation system is not permitted
- The Pro licence does not expire; future updates are provided at the author's discretion

---

## Final thought

The best tools are the ones you forget you're using. You're thinking about the project — the structure, the joinery, the proportions — and the tool handles the accounting quietly in the background.

CutVisor won't make you a better designer. But it will make sure that what you designed is what gets built, at the right cost, with the right amount of material, and with less waste in the skip at the end.

That feels like enough.

*CutVisor is developed by an architecture faculty member with an interest in computational design and sustainable timber construction.* *Feedback, feature requests and bug reports are genuinely welcome — this tool grows with its users.*


</div>

<div id="lang-fr" class="lang-block" markdown="1">

# **CutVisor — Plus qu'un plugin**

## Pourquoi CutVisor existe

Tout projet en bois commence par le même problème.

Vous avez un modèle — dessiné avec soin, aux dimensions correctes, pensé structurellement. Puis vient le moment de le construire réellement. Vous ouvrez un tableur et commencez à compter manuellement : combien de pièces de 700mm, combien de 750mm, combien de planches faut-il acheter. Vous ajoutez une marge de sécurité à l'instinct. Vous allez chez le fournisseur, achetez trop ou pas assez, passez l'après-midi à faire des coupes qui auraient pu être calculées à l'avance.

Les chutes s'accumulent. Certaines seront réutilisées, la plupart non. À la fin du projet, un coin de l'atelier est rempli de pièces "trop belles pour être jetées" mais trop courtes pour servir à quoi que ce soit de précis.

CutVisor a été conçu pour résoudre exactement cela — non pas comme une grande ambition, mais comme une réponse pratique à un problème qui se répète à chaque projet.

L'idée est simple : si la structure existe déjà comme modèle 3D, toutes les informations nécessaires pour générer la liste d'achats et le plan de coupe sont déjà là. C'est l'ordinateur qui devrait faire ce calcul, pas le constructeur.

---

## La philosophie : moins de gaspillage, une meilleure planification

Le bois est une matière vivante. Il faut des décennies pour qu'il pousse. L'idée de couper une planche de 4 mètres pour n'utiliser que 600mm et jeter le reste — alors qu'une meilleure planification aurait pu l'éviter — porte un poids qui va au-delà du coût financier.

CutVisor aborde le problème d'optimisation des coupes comme le ferait un artisan minutieux : trier les pièces les plus longues d'abord, remplir chaque planche aussi efficacement que possible, choisir des longueurs de stock adaptées à ce dont vous avez réellement besoin. L'algorithme (First-Fit Decreasing) n'est pas nouveau — c'est une solution bien étudiée du problème de découpe en recherche opérationnelle. Ce que fait CutVisor, c'est l'intégrer directement dans le flux de conception, au moment où cela peut vraiment faire une différence.

Le résultat est généralement une réduction de 10 à 25 % du gaspillage de matière par rapport à une planification intuitive. Sur un petit projet, cela représente quelques planches. Sur un plus grand, cela représente de l'argent réel et de la matière qui ne finit pas à la benne.

Il y a aussi autre chose : quand vous savez exactement quoi acheter avant de commencer, le projet devient plus serein. Vous arrivez chez le fournisseur avec une liste précise. Vous n'achetez pas "un peu plus au cas où". Vous coupez avec confiance, pas avec anxiété.

## À qui s'adresse CutVisor

### Architectes et concepteurs

Vous travaillez dans Blender pour la visualisation ou la conception, et vos projets incluent de plus en plus d'éléments en bois — bardage, ossature, brise-soleil, menuiserie intérieure. CutVisor vous permet de passer de la conception à la spécification des matériaux sans quitter Blender, et sans reconstruire le modèle dans un autre outil.

Si votre agence utilise Rhino, SketchUp ou ArchiCAD, CutVisor accepte les imports de tous ces logiciels. Vous n'avez pas besoin de reconstruire le modèle — vous importez, assignez les matériaux, et générez la liste.

### Ingénieurs structure et génie civil

Structures à ossature bois, fermes de charpente, bâtiments préfabriqués légers. Si vous modélisez du bois structurel dans Blender (ou l'importez depuis un outil BIM via IFC), CutVisor vous donne un bordereau de matériaux qui respecte la répétition par modificateur — les Array et Mirror sont comptés correctement, donc une ferme avec 40 éléments identiques génère une seule ligne avec une quantité de 40, pas 40 lignes séparées. Notez que CutVisor optimise des éléments linéaires ; les grands panneaux comme le CLT sont des produits en panneau et relèvent de la limitation d'imbrication 2D décrite plus bas.

### Designers d'intérieur et fabricants de mobilier

Une bibliothèque sur mesure, une cuisine, un dressing intégré. Ces projets regorgent de pièces qui se ressemblent sans être identiques — des longueurs légèrement différentes, une traverse ou un montant qui varie d'un centimètre par rapport au suivant. CutVisor regroupe les pièces par profil (largeur × épaisseur) et optimise les coupes dans chaque groupe, exactement comme un fabricant de meubles raisonne pour les composants en bois massif : tous les tasseaux de 40×18mm ensemble, toutes les traverses de 60×25mm ensemble.

Si votre projet est construit principalement à partir de panneaux (contreplaqué, MDF, panneaux de particules) plutôt que de composants en bois massif, notez que la version actuelle ne gère que les coupes linéaires — optimisation de longueur sur un seul axe. L'imbrication de panneaux (optimisation 2D) est prévue pour une future version.

### Bricoleurs et autoconstructeurs

Vous construisez une terrasse, un abri de jardin, une extension à ossature bois. Vous avez un modèle — peut-être grossier, peut-être précis — et vous devez savoir quoi acheter avant d'aller chez le négociant. CutVisor vous donne une liste que vous pouvez imprimer et emporter. Pas de surachat, pas de second voyage parce que vous en manquiez.

La version Core gratuite est conçue exactement pour cela : projets illimités, pas de limite de temps, toute la fonctionnalité essentielle sans paiement. Téléchargez l'extension, installez-la dans Blender, et commencez à l'utiliser immédiatement — pas d'inscription, pas de compte. Les fonctionnalités Pro se débloquent en saisissant une clé de licence dans l'onglet Settings.

### Makers et habitués de l'atelier

Vous travaillez régulièrement le bois — meubles, objets, installations. Vous avez développé un instinct pour la matière, mais l'instinct a ses limites quand les projets se complexifient. CutVisor vous permet de valider votre instinct avec des chiffres, rapidement. Il ne s'agit pas de remplacer le savoir-faire artisanal — il s'agit de donner à ce savoir-faire un outil qui le suit.

## Explication des fonctionnalités — ce que tout signifie

### Filtre de volume (onglet Parts)

La plupart des imports réels ne sont pas propres. Un modèle importé depuis Rhino, SketchUp ou IFC arrive généralement comme une grande collection contenant non seulement les pièces en bois mais aussi chaque vis, boulon et équerre modélisés avec elles — parfois des centaines de petits objets mélangés aux planches que vous voulez réellement couper. Les trier à la main, un clic à la fois, ne tient pas à l'échelle.

Le Filtre de volume vous donne un moyen rapide de séparer les deux par taille, puisque la quincaillerie est toujours minuscule comparée au bois :

**Le volume de l'objet actif** s'affiche en direct, sans bouton à cliquer — sélectionnez n'importe quoi dans la vue 3D et son volume s'affiche directement en cm³. Sélectionnez une vis pour voir à quoi vous avez affaire, sélectionnez une planche pour comparer.

**Les champs Min / Max** définissent une plage de volume. À côté de chacun se trouve un petit bouton pipette : sélectionnez un objet, cliquez sur la pipette, et le volume de cet objet est copié directement dans le champ — pas besoin de saisir un nombre que vous auriez sinon dû lire sur l'affichage de l'objet actif.

**Select in Range** sélectionne tous les objets éligibles de la scène dont le volume se situe entre Min et Max. **Exclude in Range** fait l'inverse sur votre sélection *actuelle* : sélectionnez d'abord largement, puis utilisez Exclude in Range pour retirer ce qui tombe dans cette plage — typiquement la quincaillerie — laissant les pièces en bois sélectionnées et prêtes pour **Generate Part IDs**, qui n'agit jamais que sur la sélection actuelle.

**Select Same Volume** va encore plus loin : sélectionnez une vis, cliquez sur le bouton, et tout objet ayant un volume proche est sélectionné en une fois — utile quand un projet utilise plusieurs tailles de fixations différentes et que vous voulez en isoler une seule. Le champ **Tolerance** (1 % par défaut) contrôle la précision requise pour une correspondance ; augmentez-le si votre quincaillerie a de légères variations de modélisation, diminuez-le si vous avez besoin d'une correspondance exacte.

Rien de tout cela ne nécessite qu'un matériau soit déjà assigné — le volume est lu directement depuis la géométrie du maillage, donc le filtrage peut être la toute première étape après l'import, avant tout travail de matériaux ou de Part ID.

### Matériaux et tarification

Un **Matériau** dans CutVisor désigne une essence de bois avec ses propriétés physiques : densité (son poids par mètre cube) et prix (son coût par unité). Vous pouvez tarifer au kilogramme, au mètre cube, ou à la pièce — selon la façon dont votre fournisseur facture.

La **densité** sert à deux choses : calculer la masse de chaque pièce (utile pour les vérifications structurelles et la logistique) et calculer le coût quand vous tarifez au poids. Des préréglages intégrés couvrent les essences les plus courantes. Si vous travaillez avec une essence absente de la liste, vous saisissez la densité manuellement — c'est la densité sèche du bois, disponible sur la fiche technique de tout fournisseur de bois ou dans la norme EN 338 pour le bois de structure.

La **devise** est purement informative. CutVisor ne convertit pas entre devises — si votre fournisseur facture en EUR, vous saisissez EUR. Si vous avez des matériaux de différents fournisseurs en différentes devises, chaque matériau porte sa propre devise et la liste d'achats affiche les coûts groupés par devise.

### Tailles de stock

Les tailles de stock sont les planches disponibles chez votre fournisseur — les dimensions qu'il vend réellement. Pour chaque matériau, vous définissez une liste de longueurs, largeurs et épaisseurs disponibles, et optionnellement un prix par planche.

**Pourquoi c'est important :** l'optimiseur de coupe ne peut travailler qu'avec le stock que vous lui avez indiqué. Si votre fournisseur vend du pin en longueurs de 4000mm et 5000mm au profil 150×40mm, ce sont les deux entrées de votre liste de stock. L'optimiseur choisira automatiquement entre elles selon ce qui s'adapte le mieux.

**Importer le stock d'un fournisseur :** de nombreux négociants et distributeurs de bois peuvent fournir leur catalogue sous forme de tableur (Excel ou CSV). Dans la version PRO, vous pouvez importer ce fichier directement — sans saisie manuelle. Le format CSV est simple : une ligne par taille de stock, avec des colonnes pour le nom du matériau, la longueur, la largeur, l'épaisseur et le prix. Si votre fournisseur ne propose pas de téléchargement, un simple e-mail à son service commercial ou technique vous procurera généralement une liste de prix au format tableur.

La liste de stock est enregistrée avec votre fichier Blender. Vous la construisez une fois pour un fournisseur donné et la réutilisez d'un projet à l'autre. Vous pouvez aussi l'exporter en CSV pour la sauvegarder ou la partager avec des collègues.

### L'optimiseur de coupe — comment il fonctionne en termes simples

L'optimiseur répond à une seule question : étant donné les pièces dont j'ai besoin et les planches disponibles, quelle est la façon la plus efficace de couper ?

Il procède en trois étapes :

**Étape 1 — Grouper par profil.** Toutes les pièces qui partagent la même largeur et épaisseur vont dans le même groupe. Une pièce de 700mm et une de 750mm, toutes deux en 150×40mm, sont dans le même groupe et seront coupées dans les mêmes planches. Une pièce de 700mm en 150×40mm et une pièce de 700mm en 100×50mm sont dans des groupes différents — on ne peut pas couper l'une dans l'autre.

**Étape 2 — Trier du plus grand au plus petit.** Dans chaque groupe, les pièces sont triées de la plus longue à la plus courte. C'est l'heuristique "First-Fit Decreasing" — mathématiquement prouvée pour produire des résultats quasi optimaux dans les problèmes de découpe à une dimension. Les pièces longues sont placées d'abord parce qu'elles sont les plus difficiles à insérer ; les pièces courtes comblent les espaces restants à la fin.

**Étape 3 — Remplir les planches.** Chaque pièce est placée dans la première planche ouverte ayant suffisamment de longueur restante. Si aucune planche ouverte ne convient, une nouvelle est ouverte. Pour choisir quelle longueur de stock ouvrir, l'optimiseur simule toutes les longueurs disponibles et choisit celle qui permettra d'utiliser le plus de matière — pas nécessairement la plus courte, ni nécessairement la plus longue.

Le résultat est un plan de coupe : quelles pièces vont dans quelle planche, dans quel ordre, avec la chute en fin de chaque planche clairement indiquée.

### Trait de scie (Kerf)

Le trait de scie est la largeur de matière retirée par la lame à chaque coupe. Une lame de scie circulaire ou de scie à format typique retire 2 à 4mm de matière par coupe. Si vous ignorez le trait de scie dans la planification, les pièces ne rentrent pas — une planche calculée pour contenir exactement six pièces de 650mm n'en contiendra en réalité que cinq une fois pris en compte les cinq traits de scie consommant 15mm au total.

CutVisor déduit le trait de scie après chaque coupe sauf la première sur chaque planche (la première pièce ne nécessite pas de coupe depuis l'extrémité de la planche — elle part de zéro). La valeur par défaut est 2mm, qui couvre la plupart des lames de scie circulaire et de scie à format. Pour une scie à ruban, dont le trait est bien plus fin, 1mm est plus proche de la réalité ; pour une lame plus large sur de grosses sections, 3 à 4mm.

### Bonus de longueur de stock

Les planches vendues comme "4000mm" font rarement exactement 4000mm. Les fournisseurs coupent généralement à des longueurs nominales avec un léger surplus — pour garantir qu'après la mise à longueur, la pièce atteigne bien la longueur nominale.

Le bonus de longueur de stock indique à CutVisor de planifier avec la longueur réelle plutôt que nominale. Réglé à 15mm, une planche de 4000mm est traitée comme 4015mm pour la planification. La liste d'achats affiche toujours "4000mm" (c'est ce que vous commandez), mais le plan de coupe utilise la longueur réellement disponible. Cela évite la situation où des pièces sont calculées pour rentrer mais n'y arrivent pas parce que la planche était exactement à la longueur nominale.

Si vous ne savez pas quel surplus laisse votre fournisseur, mesurez quelques planches de votre prochaine livraison et ajustez cette valeur en conséquence — elle varie selon le fournisseur et la scierie, il n'y a pas de chiffre universel.

### Tolérance de mesure (L×É)

CutVisor mesure les dimensions des pièces à partir du modèle 3D à l'aide d'une boîte englobante orientée — une méthode qui donne des résultats corrects même pour des objets inclinés ou tournés. Mais les mesures issues d'une géométrie 3D ne sont jamais parfaites : arrondis du modèle, petites différences en virgule flottante, ou simplement le fait que les dimensions du bois varient d'un ou deux millimètres dans la réalité.

La tolérance de mesure gère cela. Avec une tolérance de 5mm (valeur par défaut), une pièce mesurée à 152mm correspondra à un profil de stock de 150mm. Sans tolérance, elle ne correspondrait à rien et apparaîtrait dans la liste des non-appariées.

Si vous voulez un contrôle plus strict sur la correspondance, une valeur plus petite (1 à 3mm) convient à la plupart des travaux sur bois. La régler très haut pour forcer des correspondances est la mauvaise solution ; cela ne fait que créer des regroupements incorrects. Si des pièces ne correspondent pas, la bonne solution est d'ajouter le bon profil de stock au matériau, ou de vérifier si les dimensions du modèle sont bien celles voulues.

### Variation de largeur

La variation de largeur est un réglage distinct de la tolérance de mesure, et la distinction compte.

**La tolérance de mesure** couvre l'écart entre la dimension modélisée et la dimension réelle de la planche — les deux devraient être le même profil, juste mesuré légèrement différemment.

**La variation de largeur** couvre le cas où vous achetez intentionnellement un stock légèrement plus large que la pièce finie, pour le refendre à la largeur souhaitée à la scie à format. Par exemple : vous avez besoin de pièces de 148mm de large, mais votre fournisseur ne stocke que des planches de 150mm. Avec une variation de largeur de 5mm, CutVisor associera vos pièces de 148mm au stock de 150mm, sachant que vous refendrez 2mm.

Par défaut, la variation de largeur est réglée à 10mm — une pièce jusqu'à 10mm plus étroite qu'une largeur de stock disponible est associée automatiquement, en supposant que vous la refendrez. Une pièce de 148mm sera associée à un stock de 150mm (2mm à refendre), mais une pièce de 150mm ne sera pas associée à un stock de 200mm — 50mm est bien en dehors de la plage par défaut, ce qui empêche les associations accidentelles entre tailles de stock réellement différentes, quelle que soit la tolérance de mesure réglée.

Réglez-la à 0mm si vous ne refendez jamais à la largeur et voulez uniquement des correspondances proches, sans refente. Augmentez-la si la refente est une étape habituelle de votre flux de travail et que vos planches diffèrent davantage de vos largeurs cibles que ce que couvre la valeur par défaut.

### Avertissement de refente

Quand la variation de largeur est utilisée et qu'une pièce est associée à un stock plus large que nécessaire, CutVisor le signale clairement dans la liste d'achats et le CSV exporté. La notation indique la largeur du stock et la largeur cible : "Rip to width: 155mm → 150mm".

Cet avertissement existe parce que refendre à la scie à format est une opération différente du tronçonnage, et cela affecte votre flux de travail : il vous faut la bonne lame, le bon réglage de guide, et suffisamment de temps. Savoir à l'avance quelles planches nécessitent une refente évite les surprises.

**Rip Cut Max Width** va encore plus loin. Si vous le réglez sur la capacité maximale de refente de votre scie — par exemple 250mm pour une scie circulaire fixe — CutVisor signalera toute planche qui dépasse cette valeur comme un avertissement de capacité de scie. Cela s'affiche en rouge dans le panneau et est clairement indiqué dans le CSV :

*"RIP TO WIDTH 280mm → 150mm — EXCEEDS SAW CAPACITY (250mm)"*

Réglé à 0 (valeur par défaut), aucune vérification de capacité n'est effectuée.

### Import JARCH Vis (PRO)

JARCH Vis est une extension Blender qui crée des revêtements architecturaux — bardage, planchers, toitures en bois — sous forme d'objets mesh uniques contenant de nombreuses planches individuelles. CutVisor PRO peut diviser automatiquement ces objets en pièces individuelles mesurables.

Le processus d'import gère plusieurs problèmes propres à la façon dont JARCH crée la géométrie :

**Faces parasites issues de découpes booléennes.** Quand un bardage est découpé autour de fenêtres ou de portes, les opérations booléennes laissent parfois des faces 2D isolées — une géométrie plate sans volume. CutVisor les détecte (par épaisseur et planéité) et les supprime avant le comptage.

**Arêtes de maillage ouvertes.** Découper une planche laisse une face ouverte à l'endroit de la coupe. CutVisor referme automatiquement ces trous, ce qui est nécessaire pour des calculs de volume corrects et pour certains outils en aval.

**Mesure des dimensions sur des planches inclinées.** Les planches de bardage horizontales ne sont pas alignées avec les axes du monde — elles sont posées en angle sur le mur. Une mesure de boîte englobante standard donne des dimensions exagérées pour les objets tournés (une planche de 20mm d'épaisseur inclinée à 60° mesurerait 40mm avec une boîte englobante alignée aux axes). CutVisor identifie plutôt chaque axe à partir de l'aire des faces : les faces planes dominantes (avant/arrière de la planche) donnent la direction de l'épaisseur, et les longs bords latéraux — de loin les plus grandes surfaces planes restantes sur une planche simple — donnent la direction de la largeur ; la longueur est ce qui reste. Comme cette méthode repose sur la surface plutôt que sur la position des sommets, elle reste précise même sur des imports finement tessellés, des bords chanfreinés, ou des planches avec un chanfrein manquant à une extrémité — et elle fonctionne correctement que la planche soit horizontale, verticale, inclinée ou complètement tournée.

Cette mesure est effectuée sur le maillage de base, avant l'évaluation des modificateurs — les modificateurs non destructifs comme Solidify, Bevel ou Subdivision Surface ne sont pas pris en compte dans les dimensions rapportées. Appliquez un tel modificateur au préalable si vous avez besoin qu'il soit inclus dans la mesure.

Après l'import, les pièces apparaissent dans une collection Blender dédiée, nommée d'après l'objet d'origine, prêtes pour l'assignation de matériau et la génération de Part ID.

### Étiquettes dans la vue 3D

L'onglet View contrôle les étiquettes dessinées directement dans la vue 3D pour tout objet sélectionné. Vous pouvez afficher n'importe quelle combinaison de Part ID, nom de l'objet, dimensions et nombre d'instances. Les étiquettes sont ancrées par rapport au panneau N (barre latérale droite) ou au panneau T (barre d'outils gauche) — pas au bord de l'écran — donc elles ne se superposent jamais au panneau CutVisor ni à aucun autre panneau Blender, même si vous les redimensionnez.

Les options d'ombre et de contour rendent les étiquettes lisibles sur n'importe quel arrière-plan. Quatre préréglages en un clic (All, IDs only, Dims+Count, Off) permettent de changer rapidement de mode d'information pendant une session de travail.

Tous les réglages d'affichage se mettent à jour instantanément dans la vue 3D dès qu'ils sont modifiés — pas besoin de déplacer la caméra pour déclencher un rafraîchissement.

### Le dossier d'export

Tous les fichiers exportés par CutVisor — liste d'achats, plan de coupe, bordereau de matériaux — sont enregistrés dans un dossier nommé d'après votre fichier Blender, créé automatiquement au même emplacement :

ma\_maison.blend

ma\_maison\_lists/

    Shopping\_List.csv

    BOM\_Detailed.csv

    BOM\_Consolidated.csv

Ce dossier est créé lors du premier export, et tous les exports suivants vont au même endroit. Le bouton "Open Lists Folder" l'ouvre directement dans l'Explorateur (Windows), le Finder (Mac) ou Fichiers (Linux) — sans navigation manuelle.

### Le CSV de la liste d'achats

Chaque fois que vous générez une liste d'achats, CutVisor enregistre un nouveau fichier horodaté — `Shopping_List_20240615_143022.csv` — pour que les versions précédentes ne soient jamais écrasées. Cela permet de comparer différentes configurations côte à côte : réglages de trait de scie différents, longueurs de stock différentes, regroupements différents.

Le fichier exporté comporte trois sections :

**Section 1 — Liste d'achats.** Ce qu'il faut commander chez le fournisseur : matériau, profil (largeur × épaisseur), longueur de stock, quantité, prix unitaire, sous-total, pourcentage de chute. C'est la liste que vous apportez chez le négociant ou envoyez par e-mail. Si une planche doit être refendue à la largeur, une note apparaît dans la colonne Notes.

**Section 2 — Plan de coupe.** Pour chaque planche achetée, la séquence des coupes : quelles pièces y entrent, dans quel ordre, et la chute restante à la fin. C'est la liste que vous apportez à la scie.

**Section 3 — Pièces non appariées.** Toute pièce n'ayant pu être associée à une taille de stock, avec la raison. Causes courantes : aucun stock défini pour ce matériau ; le profil ne correspond à aucun stock ; la pièce est plus longue que toutes les longueurs de stock disponibles. Cette section vide signifie que le plan est complet.

Si vous utilisez les **Geometry Nodes** pour créer des réseaux, CutVisor détecte le modificateur et affiche un avertissement avec une option pour le convertir directement en modificateur Array classique depuis le panneau Parts. Après conversion, les quantités sont lues automatiquement. C'est le flux de travail recommandé pour les réseaux basés sur GN dans les projets nécessitant une optimisation de coupe.

### Bordereaux de matériaux

**BOM détaillé :** une ligne par objet, indiquant matériau, dimensions, quantité (issue des modificateurs Array/Mirror), Part ID et nom de l'objet. Utile pour suivre des éléments individuels ou pour le métré.

**BOM consolidé :** une ligne par type de pièce unique (matériau + dimensions), avec quantité totale. Utile pour les achats, les bordereaux structurels, et pour comparer avec la liste d'achats.

Les deux fichiers sont enregistrés en texte UTF-8 avec une marque d'ordre des octets (BOM) en en-tête, ce qui leur permet de s'ouvrir correctement dans Excel sur tous les systèmes d'exploitation sans problème d'encodage de caractères.

## Ce que CutVisor ne fait pas (encore)

**Panneaux.** Contreplaqué, OSB, MDF, panneaux de particules, panneaux CLT — leur optimisation nécessite une imbrication 2D (placer des rectangles dans un panneau), un problème différent et plus complexe que la découpe linéaire. C'est sur la feuille de route.

**Éléments courbes ou non prismatiques.** CutVisor mesure des pièces droites à section rectangulaire. Les poutres courbes, éléments effilés ou profils complexes nécessitent un traitement manuel.

**Calculs structurels.** CutVisor vous dit quelle matière acheter, pas si elle tiendra debout. La vérification structurelle suit son propre processus et ses propres normes (EC5 pour le bois en Europe). Les deux sont complémentaires, pas concurrents.

**Quincaillerie d'assemblage.** Boulons, vis, équerres, sabots de charpente — hors périmètre de cette version.

Ce ne sont pas des oublis. Ils définissent les limites de ce que CutVisor promet de bien faire, qui est la seule chose qu'il fait réellement : vous dire quel bois acheter et comment le couper.

## Activation — Core et Pro

CutVisor est distribué comme une extension unique. Installée sans clé de licence, elle fonctionne en mode Core. Saisir une clé Pro valide dans **Settings → Enter / Change Key** débloque toutes les fonctionnalités Pro instantanément — aucun redémarrage requis.

La clé de licence a le format `CWTW-XXXX-XXXX-XXXX-XXXX`. Elle est validée localement via une signature cryptographique — aucune connexion internet n'est nécessaire pour l'activation ou l'utilisation continue.

**Pour activer Pro :**
1. Achetez une clé sur https://cutvisor.gumroad.com/l/yiqahd
2. Copiez la clé depuis votre e-mail de confirmation ou votre bibliothèque Gumroad
3. Ouvrez Blender → panneau N → CutVisor → **Settings**
4. Cliquez sur **Paste Key** (si la clé est dans votre presse-papiers) ou **Enter / Change Key**
5. La ligne de statut passe à "✓ CutVisor Pro active"

Une clé unique est licenciée pour un usage personnel sur les ordinateurs que vous possédez. Les clés n'expirent pas et restent valides à travers les mises à jour.

---

## Licence

CutVisor utilise un modèle de licence double.

### Core — GNU General Public License v3.0

Le code de l'extension CutVisor Core est un logiciel libre : vous pouvez le redistribuer et/ou le modifier selon les termes de la GNU General Public License publiée par la Free Software Foundation, version 3 ou ultérieure.

Texte complet de la licence : https://www.gnu.org/licenses/gpl-3.0.html

### Fonctionnalités Pro — Licence commerciale

Les fonctionnalités Pro de CutVisor — import CSV pour matériaux et stock, style d'affichage avancé, préréglages d'essences de bois, préréglages de matériaux personnalisés enregistrés, variation de largeur et limites de refente/tronçonnage, jonction de planches multiples, import JARCH Vis, et toutes les fonctionnalités débloquées par une clé d'activation `CWTW-…` — ne sont pas couvertes par la GPL.

En achetant une clé Pro, vous acceptez les conditions suivantes :

- La clé est une licence mono-utilisateur, non transférable
- Vous pouvez installer et utiliser CutVisor Pro sur des ordinateurs que vous possédez ou contrôlez personnellement
- La redistribution des clés d'activation Pro est strictement interdite
- L'ingénierie inverse ou le contournement du système d'activation n'est pas autorisé
- La licence Pro n'expire pas ; les futures mises à jour sont fournies à la discrétion de l'auteur

---

## Pensée finale

Les meilleurs outils sont ceux que l'on oublie d'utiliser. Vous pensez au projet — la structure, les assemblages, les proportions — et l'outil gère la comptabilité discrètement en arrière-plan.

CutVisor ne fera pas de vous un meilleur concepteur. Mais il s'assurera que ce que vous avez conçu est bien ce qui se construit, au juste coût, avec la juste quantité de matière, et avec moins de gaspillage dans la benne à la fin.

Cela semble suffisant.

*CutVisor est développé par un enseignant en faculté d'architecture, avec un intérêt pour la conception computationnelle et la construction bois durable.* *Les retours, suggestions de fonctionnalités et rapports de bugs sont sincèrement bienvenus — cet outil grandit avec ses utilisateurs.*


</div>

<div id="lang-es" class="lang-block" markdown="1">

# **CutVisor — Más que un plugin**

## Por qué existe CutVisor

Todo proyecto en madera empieza con el mismo problema.

Tienes un modelo — dibujado con cuidado, con las dimensiones correctas, pensado estructuralmente. Luego llega el momento de construirlo realmente. Abres una hoja de cálculo y empiezas a contar manualmente: cuántas piezas de 700mm, cuántas de 750mm, cuántos tablones hay que comprar. Añades un margen de seguridad por instinto. Vas al proveedor, compras de más o de menos, pasas la tarde haciendo cortes que podrían haberse calculado de antemano.

Los retales se acumulan. Algunos se usarán, la mayoría no. Al final del proyecto hay un rincón del taller lleno de piezas "demasiado buenas para tirar" pero demasiado cortas para servir para algo concreto.

CutVisor se construyó para resolver exactamente esto — no como una gran ambición, sino como una respuesta práctica a un problema que se repite en cada proyecto.

La idea es simple: si la estructura ya existe como modelo 3D, toda la información necesaria para generar la lista de compra y el plan de corte ya está ahí. Ese cálculo debería hacerlo el ordenador, no quien construye.

---

## La filosofía: menos desperdicio, mejor planificación

La madera es un material vivo. Tarda décadas en crecer. La idea de cortar un tablón de 4 metros para usar 600mm y tirar el resto — cuando una mejor planificación podría haberlo evitado — pesa más que el coste económico.

CutVisor aborda el problema de optimización de cortes igual que lo haría un artesano cuidadoso: ordenar primero las piezas más largas, llenar cada tablón con la mayor eficiencia posible, elegir longitudes de stock que tengan sentido para lo que realmente se necesita. El algoritmo (First-Fit Decreasing) no es nuevo — es una solución bien estudiada del problema de corte en investigación operativa. Lo que hace CutVisor es llevarlo directamente al flujo de diseño, en el momento en que realmente puede marcar la diferencia.

El resultado suele ser una reducción del 10 al 25 % en el desperdicio de material frente a una planificación intuitiva. En un proyecto pequeño eso significa unos pocos tablones. En uno más grande significa dinero real y material que no termina en el contenedor.

Hay algo más: cuando sabes exactamente qué comprar antes de empezar, el proyecto se vuelve más tranquilo. Llegas al proveedor con una lista precisa. No compras "un poco más por si acaso". Cortas con confianza, no con ansiedad.

## Para quién es CutVisor

### Arquitectos y diseñadores arquitectónicos

Trabajas en Blender para visualización o modelado conceptual, y tus proyectos incluyen cada vez más elementos de madera — revestimientos, estructuras, parasoles, carpintería interior. CutVisor te permite pasar del diseño a la especificación de materiales sin salir de Blender, y sin reconstruir el modelo en otra herramienta.

Si tu estudio usa Rhino, SketchUp o ArchiCAD, CutVisor acepta importaciones de todos ellos. No necesitas reconstruir el modelo — importas, asignas materiales, y generas la lista.

### Ingenieros estructurales y de obra civil

Estructuras de entramado de madera, cerchas, edificios prefabricados ligeros. Si modelas madera estructural en Blender (o la importas desde una herramienta BIM vía IFC), CutVisor te da un listado de materiales que respeta la repetición por modificadores — los Array y Mirror se cuentan correctamente, así que una cercha con 40 elementos idénticos genera una sola línea con cantidad 40, no 40 líneas separadas. Ten en cuenta que CutVisor optimiza elementos lineales; los paneles grandes como el CLT son productos en panel y entran en la limitación de anidado 2D descrita más abajo.

### Diseñadores de interiores y fabricantes de muebles

Una librería a medida, una cocina, un armario empotrado. Estos proyectos están llenos de piezas que se parecen pero no son iguales — longitudes ligeramente distintas, un listón o un travesaño que varía un centímetro respecto al siguiente. CutVisor agrupa las piezas por perfil (ancho × espesor) y optimiza los cortes dentro de cada grupo, exactamente como piensa un fabricante de muebles sobre los componentes de madera maciza: todos los listones de 40×18mm juntos, todos los travesaños de 60×25mm juntos.

Si tu proyecto se construye principalmente con tableros (contrachapado, MDF, aglomerado) en lugar de componentes de madera maciza, ten en cuenta que la versión actual solo gestiona cortes lineales — optimización de longitud en un solo eje. El anidado de paneles (optimización 2D) está previsto para una futura versión.

### Autoconstructores y aficionados al DIY

Estás construyendo una terraza, un cobertizo, una ampliación con entramado de madera. Tienes un modelo — quizá tosco, quizá preciso — y necesitas saber qué comprar antes de ir al almacén de materiales. CutVisor te da una lista que puedes imprimir y llevar contigo. Sin comprar de más, sin un segundo viaje por quedarte corto.

La versión Core gratuita está pensada exactamente para esto: proyectos ilimitados, sin límite de tiempo, toda la funcionalidad esencial sin pago. Descarga el complemento, instálalo en Blender, y empieza a usarlo de inmediato — sin registro, sin cuenta. Las funciones Pro se desbloquean introduciendo una clave de licencia en la pestaña Settings.

### Makers y habituales del taller

Trabajas la madera con regularidad — muebles, objetos, instalaciones. Has desarrollado un instinto para el material, pero el instinto tiene límites cuando los proyectos se complican. CutVisor te permite validar tu instinto con números, rápidamente. No se trata de sustituir el conocimiento artesanal — se trata de darle a ese conocimiento una herramienta que lo acompañe.

## Explicación de funciones — qué significa cada cosa

### Filtro de volumen (pestaña Parts)

La mayoría de las importaciones reales no están limpias. Un modelo importado desde Rhino, SketchUp o IFC suele llegar como una gran colección que contiene no solo las piezas de madera sino también cada tornillo, perno y escuadra modelados junto con ellas — a veces cientos de objetos pequeños mezclados con los tablones que realmente quieres cortar. Separarlos a mano, clic a clic, no es viable a esa escala.

El Filtro de volumen te da una forma rápida de separar ambos por tamaño, ya que la ferretería siempre es minúscula comparada con la madera:

**El volumen del objeto activo** se muestra en directo, sin botón que pulsar — selecciona cualquier cosa en el visor y su volumen se lee directamente en cm³. Selecciona un tornillo para ver con qué estás tratando, selecciona un tablón para comparar.

**Los campos Min / Max** definen un rango de volumen. Junto a cada uno hay un pequeño botón cuentagotas: selecciona un objeto, pulsa el cuentagotas, y el volumen de ese objeto se copia directamente en el campo — sin necesidad de escribir un número que de otro modo tendrías que leer primero en la pantalla del objeto activo.

**Select in Range** selecciona todos los objetos elegibles de la escena cuyo volumen cae dentro de Min–Max. **Exclude in Range** hace lo contrario sobre tu selección *actual*: selecciona todo ampliamente primero, luego usa Exclude in Range para descartar lo que caiga en ese rango — normalmente la ferretería — dejando las piezas de madera seleccionadas y listas para **Generate Part IDs**, que solo actúa siempre sobre la selección actual.

**Select Same Volume** va un paso más allá: selecciona un tornillo, pulsa el botón, y todo objeto con un volumen muy similar se selecciona a la vez — útil cuando un proyecto usa varios tamaños de fijaciones distintos y quieres aislar solo uno. El campo **Tolerance** (1 % por defecto) controla cuán cercana debe ser la coincidencia; súbelo si tu ferretería tiene pequeñas variaciones de modelado, bájalo si necesitas una coincidencia exacta.

Nada de esto requiere que haya un material asignado primero — el volumen se lee directamente de la geometría de la malla, así que el filtrado puede ser el primer paso tras la importación, antes de cualquier flujo de materiales o Part ID.

### Materiales y precios

Un **Material** en CutVisor es una especie de madera con sus propiedades físicas: densidad (cuánto pesa por metro cúbico) y precio (cuánto cuesta por unidad). Puedes fijar el precio por kilogramo, por metro cúbico, o por pieza — lo que coincida con cómo factura tu proveedor.

La **densidad** se usa para dos cosas: calcular la masa de cada pieza (útil para comprobaciones estructurales y logística) y calcular el coste cuando fijas el precio por peso. Hay preajustes integrados para las especies más comunes. Si trabajas con una especie que no está en la lista, introduces la densidad manualmente — es la densidad seca de la madera, disponible en la ficha técnica de cualquier proveedor o en la norma EN 338 para madera estructural.

La **moneda** es solo informativa. CutVisor no convierte entre monedas — si tu proveedor factura en EUR, introduces EUR. Si tienes materiales de distintos proveedores en distintas monedas, cada material lleva su propia moneda y la lista de compra muestra los costes agrupados por moneda.

### Tamaños de stock

Los tamaños de stock son los tablones disponibles en tu proveedor — las dimensiones que realmente vende. Para cada material defines una lista de longitudes, anchos y espesores disponibles, y opcionalmente un precio por tablón.

**Por qué importa esto:** el optimizador de corte solo puede trabajar con el stock que le hayas indicado que existe. Si tu proveedor vende pino en longitudes de 4000mm y 5000mm con perfil 150×40mm, esas son las dos entradas de tu lista de stock. El optimizador elegirá automáticamente entre ellas según lo que mejor se ajuste.

**Importar stock de un proveedor:** muchos almacenes y distribuidores de madera pueden facilitar su catálogo como hoja de cálculo (Excel o CSV). En la versión PRO, puedes importar ese archivo directamente — sin entrada manual. El formato CSV es sencillo: una fila por tamaño de stock, con columnas para nombre del material, longitud, ancho, espesor y precio. Si tu proveedor no ofrece descarga, un correo rápido a su departamento comercial o técnico suele conseguirte una lista de precios en formato hoja de cálculo.

La lista de stock se guarda con tu archivo de Blender. La construyes una vez para un proveedor dado y la reutilizas en distintos proyectos. También puedes exportarla a CSV como copia de seguridad o para compartirla con colegas.

### El optimizador de corte — cómo funciona en términos sencillos

El optimizador responde a una sola pregunta: dadas las piezas que necesito y los tablones disponibles, ¿cuál es la forma más eficiente de cortar?

Funciona en tres pasos:

**Paso 1 — Agrupar por perfil.** Todas las piezas que compartan el mismo ancho y espesor van al mismo grupo. Una pieza de 700mm y otra de 750mm, ambas en 150×40mm, están en el mismo grupo y se cortarán de los mismos tablones. Una pieza de 700mm en 150×40mm y una pieza de 700mm en 100×50mm están en grupos distintos — no se puede cortar una de la otra.

**Paso 2 — Ordenar de mayor a menor.** Dentro de cada grupo, las piezas se ordenan de la más larga a la más corta. Esta es la heurística "First-Fit Decreasing" — matemáticamente probada para producir resultados casi óptimos en problemas de corte unidimensional. Las piezas largas se colocan primero porque son las más difíciles de encajar; las piezas cortas rellenan los huecos que quedan al final.

**Paso 3 — Llenar los tablones.** Cada pieza se coloca en el primer tablón abierto que tenga suficiente longitud restante. Si ningún tablón abierto se ajusta, se abre uno nuevo. Para elegir qué longitud de stock abrir, el optimizador simula todas las longitudes disponibles y elige la que aprovechará más material — no necesariamente la más corta, ni necesariamente la más larga.

El resultado es un plan de corte: qué piezas van en qué tablón, en qué orden, con el desperdicio al final de cada tablón claramente indicado.

### Corte de sierra (Kerf)

El kerf es el ancho de material que elimina la hoja de la sierra con cada corte. Una hoja típica de sierra circular o de mesa elimina entre 2 y 4mm de material por corte. Si ignoras el kerf al planificar, las piezas no encajan — un tablón calculado para contener exactamente seis piezas de 650mm en realidad solo contendrá cinco una vez se tienen en cuenta los cinco cortes de sierra que consumen 15mm en total.

CutVisor descuenta el kerf después de cada corte excepto el primero en cada tablón (la primera pieza no requiere un corte desde el extremo del tablón — empieza en cero). El valor por defecto es 2mm, que cubre la mayoría de hojas de sierra circular y de mesa. Para una sierra de cinta, cuyo corte es mucho más fino, 1mm se acerca más a la realidad; para una hoja más ancha en secciones grandes, 3 a 4mm.

### Bonificación de longitud de stock

Los tablones vendidos como "4000mm" raramente miden exactamente 4000mm. Los proveedores suelen cortar a longitudes nominales con un pequeño exceso — para garantizar que, tras escuadrar el extremo, la pieza alcance la longitud nominal.

La bonificación de longitud de stock le indica a CutVisor que planifique con la longitud real en lugar de la nominal. Con un valor de 15mm, un tablón de 4000mm se trata como 4015mm a efectos de planificación. La lista de compra sigue mostrando "4000mm" (eso es lo que pides), pero el plan de corte usa la longitud real disponible. Esto evita la situación en la que se calcula que las piezas encajan pero no lo hacen porque el tablón medía exactamente la longitud nominal.

Si no sabes cuánto exceso deja tu proveedor, mide algunos tablones de tu próxima entrega y ajusta este valor en consecuencia — varía según el proveedor y el aserradero, no hay una cifra universal.

### Tolerancia de medición (A×E)

CutVisor mide las dimensiones de las piezas a partir del modelo 3D usando una caja delimitadora orientada — un método que da resultados correctos incluso para objetos rotados o inclinados. Pero las medidas a partir de geometría 3D nunca son perfectas: redondeos en el modelo, pequeñas diferencias de coma flotante, o simplemente que las dimensiones de la madera varían uno o dos milímetros en la realidad.

La tolerancia de medición gestiona esto. Con una tolerancia de 5mm (valor por defecto), una pieza medida en 152mm coincidirá con un perfil de stock de 150mm. Sin tolerancia, no coincidiría con nada y aparecería en la lista de no coincidentes.

Si quieres un control más estricto sobre las coincidencias, un valor menor (1 a 3mm) es apropiado para la mayoría de trabajos en madera. Subirlo mucho para forzar coincidencias es la solución equivocada; eso solo crea agrupaciones incorrectas. Si las piezas no coinciden, la solución correcta es añadir el perfil de stock correcto al material, o comprobar si las dimensiones del modelo son las que se pretendían.

### Variación de ancho

La variación de ancho es un ajuste distinto de la tolerancia de medición, y la distinción importa.

**La tolerancia de medición** cubre la diferencia entre la dimensión modelada y la dimensión real del tablón — ambas deberían ser el mismo perfil, solo medido de forma ligeramente distinta.

**La variación de ancho** cubre el caso en que compras intencionadamente stock algo más ancho que la pieza final, y lo recortas a medida en la sierra de mesa. Por ejemplo: necesitas piezas de 148mm de ancho, pero tu proveedor solo tiene tablones de 150mm. Con una variación de ancho de 5mm, CutVisor asociará tus piezas de 148mm al stock de 150mm, sabiendo que recortarás 2mm.

Por defecto, la variación de ancho está fijada en 10mm — una pieza hasta 10mm más estrecha que un ancho de stock disponible se asocia automáticamente, asumiendo que la recortarás. Una pieza de 148mm se asociará a un stock de 150mm (2mm a recortar), pero una pieza de 150mm no se asociará a un stock de 200mm — 50mm está muy por fuera del rango por defecto, lo que evita asociaciones accidentales entre tamaños de stock genuinamente distintos, sin importar cuán alta fijes la tolerancia de medición.

Fíjala en 0mm si nunca recortas a medida y solo quieres coincidencias cercanas, sin recorte. Súbela si recortar es un paso habitual en tu flujo de trabajo y tus tablones difieren más de tus anchos objetivo de lo que cubre el valor por defecto.

### Aviso de recorte

Cuando se usa la variación de ancho y una pieza se asocia a un stock más ancho de lo necesario, CutVisor lo marca claramente en la lista de compra y en el CSV exportado. La notación muestra el ancho del stock y el ancho objetivo: "Rip to width: 155mm → 150mm".

Este aviso existe porque recortar a medida en la sierra de mesa es una operación distinta del corte transversal, y afecta a tu flujo de trabajo: necesitas la hoja adecuada, el ajuste de guía adecuado, y suficiente tiempo. Saber de antemano qué tablones necesitan recorte evita sorpresas.

**Rip Cut Max Width** lleva esto un paso más allá. Si lo fijas a la capacidad máxima de recorte de tu sierra — por ejemplo 250mm para una sierra circular fija — CutVisor marcará cualquier tablón que supere ese valor como aviso de capacidad de sierra. Esto se muestra en rojo en el panel y se indica claramente en el CSV:

*"RIP TO WIDTH 280mm → 150mm — EXCEEDS SAW CAPACITY (250mm)"*

Fijado en 0 (valor por defecto), no se realiza ninguna comprobación de capacidad.

### Importación JARCH Vis (PRO)

JARCH Vis es un complemento de Blender que crea revestimientos arquitectónicos — tablazón, suelos, cubiertas de madera — como objetos de malla única que contienen muchos tablones individuales. CutVisor PRO puede dividir estos objetos en piezas individuales medibles automáticamente.

El proceso de importación gestiona varios problemas propios de cómo JARCH crea la geometría:

**Caras espurias de cortes booleanos.** Cuando un revestimiento se recorta alrededor de ventanas o puertas, las operaciones booleanas a veces dejan caras 2D aisladas — geometría plana sin volumen. CutVisor las detecta (por espesor y planaridad) y las elimina antes de contar.

**Bordes de malla abiertos.** Cortar un tablón deja una cara abierta en el punto del corte. CutVisor cierra estos huecos automáticamente, lo cual es necesario para cálculos correctos de volumen y para algunas herramientas posteriores.

**Medición de dimensiones en tablones inclinados.** Los tablones de revestimiento horizontales no están alineados con los ejes del mundo — se asientan en ángulo sobre la pared. La medición estándar por caja delimitadora da dimensiones infladas para objetos rotados (un tablón de 20mm de espesor inclinado a 60° mediría 40mm con una caja alineada a los ejes). CutVisor identifica cada eje a partir del área de las caras: las caras planas dominantes (frente/dorso del tablón) dan la dirección del espesor, y los bordes laterales largos — con diferencia las mayores superficies planas restantes en un tablón simple — dan la dirección del ancho; la longitud es lo que queda. Como esto se basa en la superficie y no en la posición de los vértices, se mantiene preciso incluso en importaciones finamente teseladas, bordes biselados, o tablones con un chaflán ausente en un extremo — y funciona correctamente tanto si el tablón está horizontal, vertical, inclinado o completamente rotado.

Esta medición se toma de la malla base, antes de evaluar los modificadores — los modificadores no destructivos como Solidify, Bevel o Subdivision Surface no se reflejan en las dimensiones reportadas. Aplica ese modificador primero si necesitas que se incluya en la medición.

Tras la importación, las piezas aparecen en una colección de Blender dedicada, nombrada según el objeto original, listas para la asignación de material y la generación de Part ID.

### Etiquetas en el visor 3D

La pestaña View controla las etiquetas dibujadas directamente en el visor 3D para cualquier objeto seleccionado. Puedes mostrar cualquier combinación de Part ID, nombre del objeto, dimensiones y número de instancias. Las etiquetas se anclan en relación con el panel N (barra lateral derecha) o el panel T (barra de herramientas izquierda) — no al borde de la pantalla — así que nunca se solapan con el panel de CutVisor ni con ningún otro panel de Blender, aunque los redimensiones.

Las opciones de sombra y contorno hacen que las etiquetas sean legibles sobre cualquier color de fondo. Cuatro preajustes de un clic (All, IDs only, Dims+Count, Off) permiten cambiar rápidamente entre modos de información durante una sesión de trabajo.

Todos los ajustes de superposición se actualizan al instante en el visor cuando se modifican — no es necesario mover la cámara para forzar un refresco.

### La carpeta de exportación

Todos los archivos exportados por CutVisor — lista de compra, plan de corte, listado de materiales — se guardan en una carpeta con el nombre de tu archivo de Blender, creada automáticamente en la misma ubicación:

mi\_casa.blend

mi\_casa\_lists/

    Shopping\_List.csv

    BOM\_Detailed.csv

    BOM\_Consolidated.csv

Esta carpeta se crea la primera vez que exportas, y todas las exportaciones siguientes van al mismo lugar. El botón "Open Lists Folder" la abre directamente en el Explorador (Windows), Finder (Mac) o Archivos (Linux) — sin navegación manual.

### El CSV de la lista de compra

Cada vez que generas una lista de compra, CutVisor guarda un nuevo archivo con marca de tiempo — `Shopping_List_20240615_143022.csv` — de modo que las versiones anteriores nunca se sobrescriben. Esto permite comparar distintas configuraciones lado a lado: distintos ajustes de kerf, distintas longitudes de stock, distintos agrupamientos.

El archivo exportado tiene tres secciones:

**Sección 1 — Lista de compra.** Qué pedir al proveedor: material, perfil (ancho × espesor), longitud de stock, cantidad, precio unitario, subtotal, porcentaje de desperdicio. Esta es la lista que llevas al almacén o envías por correo. Si un tablón necesita recortarse a medida, aparece una nota en la columna Notes.

**Sección 2 — Plan de corte.** Para cada tablón comprado, la secuencia de cortes: qué piezas entran en él, en qué orden, cuánto desperdicio queda al final. Esta es la lista que llevas a la sierra.

**Sección 3 — Piezas no coincidentes.** Cualquier pieza que no haya podido asociarse a un tamaño de stock, con el motivo. Causas habituales: no hay stock definido para ese material; el perfil no coincide con ningún stock; la pieza es más larga que todas las longitudes de stock disponibles. Que esta sección esté vacía significa que el plan está completo.

Si trabajas con **Geometry Nodes** para crear arrays, CutVisor detecta el modificador y muestra un aviso con la opción de convertirlo directamente a un modificador Array clásico desde el panel Parts. Tras la conversión, las cantidades se leen automáticamente. Este es el flujo de trabajo recomendado para arrays basados en GN en proyectos que necesitan optimización de corte.

### Listados de materiales

**BOM detallado:** una fila por objeto, mostrando material, dimensiones, cantidad (expandida desde los modificadores Array/Mirror), Part ID y nombre del objeto. Útil para seguir elementos individuales o para mediciones de cantidades.

**BOM consolidado:** una fila por tipo de pieza único (material + dimensiones), con cantidad total. Útil para compras, para bordereaux estructurales, y para comparar con la lista de compra.

Ambos archivos se guardan como texto UTF-8 con una marca de orden de bytes (BOM) al inicio, lo que permite que se abran correctamente en Excel en todos los sistemas operativos sin problemas de codificación de caracteres.

## Lo que CutVisor no hace (todavía)

**Tableros.** Contrachapado, OSB, MDF, aglomerado, paneles CLT — optimizarlos requiere anidado 2D (encajar rectángulos en un panel), un problema distinto y más complejo que el corte lineal. Está en la hoja de ruta.

**Elementos curvos o no prismáticos.** CutVisor mide piezas rectas de sección rectangular. Las vigas curvas, elementos ahusados o perfiles complejos necesitan tratamiento manual.

**Cálculos estructurales.** CutVisor te dice qué material comprar, no si va a aguantar. La verificación estructural sigue su propio proceso y sus propias normas (EC5 para madera en Europa). Ambos son complementarios, no competidores.

**Herrajes de conexión.** Pernos, tornillos, escuadras, estribos — fuera del alcance de esta versión.

Estas no son omisiones casuales. Definen el límite de lo que CutVisor promete hacer bien, que es la única cosa que realmente hace: decirte qué madera comprar y cómo cortarla.

## Activación — Core y Pro

CutVisor se distribuye como un único complemento. Instalado sin clave de licencia, funciona en modo Core. Introducir una clave Pro válida en **Settings → Enter / Change Key** desbloquea todas las funciones Pro al instante — sin necesidad de reiniciar.

La clave de licencia tiene el formato `CWTW-XXXX-XXXX-XXXX-XXXX`. Se valida localmente mediante una firma criptográfica — no se requiere conexión a internet para la activación ni para el uso continuado.

**Para activar Pro:**
1. Compra una clave en https://cutvisor.gumroad.com/l/yiqahd
2. Copia la clave desde tu correo de confirmación o tu biblioteca de Gumroad
3. Abre Blender → panel N → CutVisor → **Settings**
4. Pulsa **Paste Key** (si la clave está en tu portapapeles) o **Enter / Change Key**
5. La línea de estado cambia a "✓ CutVisor Pro active"

Una clave se licencia para uso personal en ordenadores que poseas. Las claves no caducan y permanecen válidas a través de las actualizaciones.

---

## Licencia

CutVisor usa un modelo de licencia dual.

### Core — GNU General Public License v3.0

El código de CutVisor Core es software libre: puedes redistribuirlo y/o modificarlo según los términos de la GNU General Public License publicada por la Free Software Foundation, versión 3 o posterior.

Texto completo de la licencia: https://www.gnu.org/licenses/gpl-3.0.html

### Funciones Pro — Licencia comercial

Las funciones Pro de CutVisor — importación CSV para materiales y stock, estilo de superposición avanzado, preajustes de especies de madera, preajustes de materiales personalizados guardados, variación de ancho y límites de recorte/corte transversal, empalme de varios tablones, importación JARCH Vis, y todas las funciones desbloqueadas por una clave de activación `CWTW-…` — no están cubiertas por la GPL.

Al comprar una clave Pro aceptas los siguientes términos:

- La clave es una licencia de un solo usuario, no transferible
- Puedes instalar y usar CutVisor Pro en ordenadores que poseas o controles personalmente
- Está estrictamente prohibida la redistribución de claves de activación Pro
- No está permitida la ingeniería inversa ni eludir el sistema de activación
- La licencia Pro no caduca; las futuras actualizaciones se proporcionan a discreción del autor

---

## Pensamiento final

Las mejores herramientas son las que olvidas que estás usando. Estás pensando en el proyecto — la estructura, las uniones, las proporciones — y la herramienta gestiona la contabilidad discretamente en segundo plano.

CutVisor no te convertirá en un mejor diseñador. Pero se asegurará de que lo que diseñaste sea lo que se construye, al coste correcto, con la cantidad correcta de material, y con menos desperdicio en el contenedor al final.

Eso parece suficiente.

*CutVisor está desarrollado por un profesor de facultad de arquitectura con interés en el diseño computacional y la construcción sostenible en madera.* *Los comentarios, sugerencias de funciones e informes de errores son sinceramente bienvenidos — esta herramienta crece con sus usuarios.*


</div>

<div id="lang-pt" class="lang-block" markdown="1">

# **CutVisor — Mais do que um plugin**

## Por que o CutVisor existe

Todo projeto em madeira começa com o mesmo problema.

Você tem um modelo — desenhado com cuidado, com as dimensões corretas, pensado estruturalmente. Depois chega o momento de construí-lo de fato. Você abre uma planilha e começa a contar manualmente: quantas peças de 700mm, quantas de 750mm, quantas tábuas é preciso comprar. Você acrescenta uma margem de segurança por instinto. Vai ao fornecedor, compra mais ou menos do que precisa, passa a tarde fazendo cortes que poderiam ter sido calculados com antecedência.

As sobras se acumulam. Algumas serão usadas, a maioria não. No final do projeto há um canto da marcenaria cheio de peças "boas demais para jogar fora" mas curtas demais para servir a algo específico.

O CutVisor foi criado para resolver exatamente isso — não como uma grande ambição, mas como uma resposta prática a um problema que se repete em todo projeto.

A ideia é simples: se a estrutura já existe como modelo 3D, todas as informações necessárias para gerar a lista de compras e o plano de corte já estão ali. Esse cálculo deveria ser feito pelo computador, não por quem constrói.

---

## A filosofia: menos desperdício, melhor planejamento

A madeira é um material vivo. Leva décadas para crescer. A ideia de cortar uma tábua de 4 metros para usar 600mm e jogar o resto fora — quando isso poderia ter sido evitado com melhor planejamento — carrega um peso que vai além do custo financeiro.

O CutVisor aborda o problema de otimização de cortes do mesmo jeito que um marceneiro cuidadoso faria: ordenar primeiro as peças mais longas, preencher cada tábua com a maior eficiência possível, escolher comprimentos de estoque que façam sentido para o que você realmente precisa. O algoritmo (First-Fit Decreasing) não é novo — é uma solução bem estudada do problema de corte em pesquisa operacional. O que o CutVisor faz é trazê-lo diretamente para o fluxo de projeto, no momento em que ele pode realmente fazer diferença.

O resultado geralmente é uma redução de 10 a 25% no desperdício de material em comparação a um planejamento intuitivo. Em um projeto pequeno isso significa algumas tábuas. Em um maior, significa dinheiro real e material que não acaba no caçamba.

Há também outra coisa: quando você sabe exatamente o que comprar antes de começar, o projeto fica mais tranquilo. Você chega ao fornecedor com uma lista precisa. Não compra "um pouco mais por precaução". Você corta com confiança, não com ansiedade.

## Para quem é o CutVisor

### Arquitetos e projetistas

Você trabalha no Blender para visualização ou modelagem conceitual, e seus projetos incluem cada vez mais elementos em madeira — revestimentos, estruturas, brise-soleil, marcenaria interna. O CutVisor permite passar do projeto para a especificação de materiais sem saber do Blender, e sem reconstruir o modelo em outra ferramenta.

Se o seu escritório usa Rhino, SketchUp ou ArchiCAD, o CutVisor aceita importações de todos eles. Você não precisa reconstruir o modelo — importa, atribui materiais, e gera a lista.

### Engenheiros estruturais e civis

Estruturas em madeira (wood frame), tesouras, edificações pré-fabricadas leves. Se você modela madeira estrutural no Blender (ou a importa de uma ferramenta BIM via IFC), o CutVisor fornece uma lista de materiais que respeita a repetição por modificadores — Array e Mirror são contados corretamente, então uma tesoura com 40 elementos idênticos gera uma única linha com quantidade 40, não 40 linhas separadas. Note que o CutVisor otimiza elementos lineares; painéis grandes como o CLT são produtos em painel e se encaixam na limitação de encaixe 2D descrita abaixo.

### Designers de interiores e marceneiros

Uma estante sob medida, uma cozinha, um guarda-roupa planejado. Esses projetos estão cheios de peças que parecem iguais mas não são — comprimentos ligeiramente diferentes, um sarrafo ou travessa que varia um centímetro em relação ao próximo. O CutVisor agrupa as peças por perfil (largura × espessura) e otimiza os cortes dentro de cada grupo, exatamente como um marceneiro pensa sobre componentes em madeira maciça: todos os sarrafos de 40×18mm juntos, todas as travessas de 60×25mm juntas.

Se o seu projeto é construído principalmente com painéis (compensado, MDF, aglomerado) em vez de componentes em madeira maciça, note que a versão atual trata apenas cortes lineares — otimização de comprimento em um único eixo. O encaixe de painéis (otimização 2D) está planejado para uma versão futura.

### Construtores autônomos e faça-você-mesmo

Você está construindo um deck, um galpão de jardim, uma ampliação em wood frame. Você tem um modelo — talvez bruto, talvez preciso — e precisa saber o que comprar antes de ir à madeireira. O CutVisor te dá uma lista que pode imprimir e levar com você. Sem comprar demais, sem uma segunda viagem por ter ficado faltando material.

A versão Core gratuita foi pensada exatamente para isso: projetos ilimitados, sem limite de tempo, toda a funcionalidade essencial sem pagamento. Baixe o addon, instale no Blender, e comece a usar imediatamente — sem cadastro, sem conta. Os recursos Pro são desbloqueados ao inserir uma chave de licença na aba Settings.

### Makers e frequentadores de ateliê

Você trabalha com madeira regularmente — móveis, objetos, instalações. Desenvolveu um instinto para o material, mas o instinto tem limites quando os projetos ficam complexos. O CutVisor permite validar seu instinto com números, rapidamente. Não se trata de substituir o conhecimento artesanal — se trata de dar a esse conhecimento uma ferramenta que o acompanhe.

## Explicação das funções — o que cada coisa significa

### Filtro de volume (aba Parts)

A maioria das importações reais não vem limpa. Um modelo importado do Rhino, SketchUp ou IFC geralmente chega como uma grande coleção contendo não só as peças de madeira, mas também todo parafuso, cavilha e suporte modelados junto com elas — às vezes centenas de objetos pequenos misturados com as tábuas que você realmente quer cortar. Separá-los manualmente, um clique por vez, não escala.

O Filtro de volume oferece uma forma rápida de separar os dois por tamanho, já que ferragens são sempre minúsculas comparadas à madeira:

**O volume do objeto ativo** é mostrado em tempo real, sem botão para clicar — selecione qualquer coisa na viewport e o volume aparece direto em cm³. Selecione um parafuso para ver com o que está lidando, selecione uma tábua para comparar.

**Os campos Min / Max** definem uma faixa de volume. Ao lado de cada um há um pequeno botão conta-gotas: selecione um objeto, clique no conta-gotas, e o volume daquele objeto é copiado direto para o campo — sem precisar digitar um número que de outra forma teria que ler primeiro na tela do objeto ativo.

**Select in Range** seleciona todo objeto elegível na cena cujo volume esteja dentro de Min–Max. **Exclude in Range** faz o oposto na sua seleção *atual*: selecione tudo de forma ampla primeiro, depois use Exclude in Range para remover o que cair nessa faixa — normalmente as ferragens — deixando as peças de madeira selecionadas e prontas para **Generate Part IDs**, que sempre age apenas sobre a seleção atual.

**Select Same Volume** vai um passo além: selecione um parafuso, clique no botão, e todo objeto com volume muito próximo é selecionado de uma vez — útil quando um projeto usa vários tamanhos de fixadores diferentes e você quer isolar apenas um tipo. O campo **Tolerance** (1% por padrão) controla quão próxima a correspondência precisa ser; aumente se suas ferragens tiverem pequenas variações de modelagem, diminua se precisar de uma correspondência exata.

Nada disso requer que um material já esteja atribuído — o volume é lido direto da geometria da malha, então a filtragem pode ser o primeiro passo logo após a importação, antes de qualquer fluxo de materiais ou Part ID.

### Materiais e preços

**Material** no CutVisor significa uma espécie de madeira com suas propriedades físicas: densidade (quanto pesa por metro cúbico) e preço (quanto custa por unidade). Você pode precificar por quilograma, por metro cúbico, ou por peça — o que corresponder à forma como seu fornecedor fatura.

A **densidade** é usada para duas coisas: calcular a massa de cada peça (útil para verificações estruturais e logística) e calcular o custo quando você precifica por peso. Predefinições já incluídas cobrem as espécies mais comuns. Se você trabalha com uma espécie que não está na lista, insere a densidade manualmente — é a densidade seca da madeira, disponível na ficha técnica de qualquer fornecedor ou na norma EN 338 para madeira estrutural.

A **moeda** é apenas informativa. O CutVisor não converte entre moedas — se o seu fornecedor cobra em EUR, você insere EUR. Se você tem materiais de fornecedores diferentes em moedas diferentes, cada material carrega sua própria moeda e a lista de compras mostra os custos agrupados por moeda.

### Tamanhos de estoque

Tamanhos de estoque são as tábuas disponíveis no seu fornecedor — as dimensões que ele realmente vende. Para cada material você define uma lista de comprimentos, larguras e espessuras disponíveis, e opcionalmente um preço por tábua.

**Por que isso importa:** o otimizador de corte só pode trabalhar com o estoque que você informou que existe. Se seu fornecedor vende pinus em comprimentos de 4000mm e 5000mm no perfil 150×40mm, essas são as duas entradas na sua lista de estoque. O otimizador escolherá automaticamente entre elas com base no que encaixa melhor.

**Importando estoque de um fornecedor:** muitos comerciantes e distribuidores de madeira conseguem fornecer seu catálogo como planilha (Excel ou CSV). Na versão PRO, você pode importar esse arquivo diretamente — sem digitação manual. O formato CSV é simples: uma linha por tamanho de estoque, com colunas para nome do material, comprimento, largura, espessura e preço. Se seu fornecedor não oferece download, um e-mail rápido ao departamento comercial ou técnico geralmente consegue uma lista de preços em formato de planilha.

A lista de estoque é salva junto com seu arquivo do Blender. Você a monta uma vez para um fornecedor específico e a reutiliza em outros projetos. Também pode exportá-la em CSV para backup ou para compartilhar com colegas.

### O otimizador de corte — como funciona em termos simples

O otimizador responde a uma única pergunta: dadas as peças de que preciso e as tábuas disponíveis, qual é a forma mais eficiente de cortar?

Funciona em três etapas:

**Etapa 1 — Agrupar por perfil.** Todas as peças que compartilham a mesma largura e espessura entram no mesmo grupo. Uma peça de 700mm e uma de 750mm, ambas em 150×40mm, estão no mesmo grupo e serão cortadas das mesmas tábuas. Uma peça de 700mm em 150×40mm e uma peça de 700mm em 100×50mm estão em grupos diferentes — não é possível cortar uma a partir da outra.

**Etapa 2 — Ordenar do maior para o menor.** Dentro de cada grupo, as peças são ordenadas da mais longa para a mais curta. Essa é a heurística "First-Fit Decreasing" — matematicamente comprovada para produzir resultados quase ótimos em problemas de corte unidimensional. Peças longas são posicionadas primeiro porque são as mais difíceis de encaixar; peças curtas preenchem os espaços que restam no final.

**Etapa 3 — Preencher as tábuas.** Cada peça é colocada na primeira tábua aberta que tenha comprimento restante suficiente. Se nenhuma tábua aberta encaixar, uma nova é aberta. Para escolher qual comprimento de estoque abrir, o otimizador simula todos os comprimentos disponíveis e escolhe aquele que vai aproveitar mais material — não necessariamente o mais curto, nem necessariamente o mais longo.

O resultado é um plano de corte: quais peças vão em qual tábua, em que ordem, com o desperdício no final de cada tábua claramente indicado.

### Espessura de corte (Kerf)

O kerf é a largura de material removida pela lâmina da serra a cada corte. Uma lâmina típica de serra circular ou serra de mesa remove de 2 a 4mm de material por corte. Se você ignorar o kerf no planejamento, as peças não encaixam — uma tábua calculada para conter exatamente seis peças de 650mm na verdade vai conter apenas cinco, depois de contabilizados os cinco cortes de serra que consomem 15mm no total.

O CutVisor desconta o kerf depois de cada corte, exceto o primeiro em cada tábua (a primeira peça não exige um corte a partir da extremidade da tábua — ela começa do zero). O valor padrão é 2mm, que cobre a maioria das lâminas de serra circular e serra de mesa. Para uma serra fita, cujo corte é bem mais fino, 1mm está mais próximo da realidade; para uma lâmina mais larga em seções grandes, 3 a 4mm.

### Bônus de comprimento de estoque

Tábuas vendidas como "4000mm" raramente medem exatamente 4000mm. Os fornecedores costumam cortar em comprimentos nominais com uma pequena sobra — para garantir que, depois de esquadrejar a ponta, a peça atinja o comprimento nominal.

O bônus de comprimento de estoque diz ao CutVisor para planejar com o comprimento real em vez do nominal. Definido em 15mm, uma tábua de 4000mm é tratada como 4015mm para fins de planejamento. A lista de compras continua mostrando "4000mm" (é isso que você pede), mas o plano de corte usa o comprimento realmente disponível. Isso evita a situação em que as peças são calculadas para encaixar mas não encaixam porque a tábua tinha exatamente o comprimento nominal.

Se você não sabe quanta sobra seu fornecedor deixa, meça algumas tábuas da sua próxima entrega e ajuste esse valor de acordo — varia por fornecedor e por serraria, não existe um número universal.

### Tolerância de medição (L×E)

O CutVisor mede as dimensões das peças a partir do modelo 3D usando uma caixa delimitadora orientada — um método que dá resultados corretos mesmo para objetos rotacionados ou inclinados. Mas medições a partir de geometria 3D nunca são perfeitas: arredondamentos no modelo, pequenas diferenças de ponto flutuante, ou simplesmente o fato de que as dimensões da madeira variam um ou dois milímetros na realidade.

A tolerância de medição trata disso. Com uma tolerância de 5mm (padrão), uma peça medindo 152mm vai corresponder a um perfil de estoque de 150mm. Sem tolerância, ela não corresponderia a nada e apareceria na lista de não correspondentes.

Se você quiser um controle mais rígido sobre a correspondência, um valor menor (1 a 3mm) é adequado para a maioria dos trabalhos em madeira. Definir um valor muito alto para forçar correspondências é a solução errada; isso só cria agrupamentos incorretos. Se as peças não estão correspondendo, a solução correta é adicionar o perfil de estoque correto ao material, ou verificar se as dimensões do modelo são as pretendidas.

### Variação de largura

Variação de largura é uma configuração separada da tolerância de medição, e a distinção importa.

**A tolerância de medição** cobre a diferença entre a dimensão modelada e a dimensão real da tábua — ambas deveriam ser o mesmo perfil, apenas medido de forma um pouco diferente.

**A variação de largura** cobre o caso em que você compra intencionalmente um estoque um pouco mais largo do que a peça final, e o desdobra na medida na serra de mesa. Por exemplo: você precisa de peças com 148mm de largura, mas seu fornecedor só tem tábuas de 150mm em estoque. Com uma variação de largura de 5mm, o CutVisor vai associar suas peças de 148mm ao estoque de 150mm, sabendo que você vai desdobrar 2mm.

Por padrão, a variação de largura está definida em 10mm — uma peça até 10mm mais estreita do que uma largura de estoque disponível é associada automaticamente, na suposição de que você vai desdobrá-la. Uma peça de 148mm será associada a um estoque de 150mm (2mm a desdobrar), mas uma peça de 150mm não será associada a um estoque de 200mm — 50mm está bem fora da faixa padrão, o que evita associações acidentais entre tamanhos de estoque genuinamente diferentes, independente de quão alta você definir a tolerância de medição.

Defina em 0mm se você nunca desdobra na medida e quer apenas correspondências próximas, sem desdobramento. Aumente se desdobrar for uma etapa rotineira no seu fluxo de trabalho e suas tábuas diferirem mais das larguras desejadas do que o padrão cobre.

### Aviso de desdobramento

Quando a variação de largura está em uso e uma peça é associada a um estoque mais largo do que o necessário, o CutVisor marca isso claramente na lista de compras e no CSV exportado. A notação mostra a largura do estoque e a largura desejada: "Rip to width: 155mm → 150mm".

Esse aviso existe porque desdobrar na serra de mesa é uma operação diferente de cortar transversalmente, e isso afeta seu fluxo de trabalho: você precisa da lâmina certa, do ajuste de guia certo, e de tempo suficiente. Saber com antecedência quais tábuas precisam de desdobramento evita surpresas.

**Rip Cut Max Width** vai um passo além. Se você definir como a capacidade máxima de desdobramento da sua serra — por exemplo, 250mm para uma serra circular fixa — o CutVisor vai marcar qualquer tábua que exceda esse valor como um aviso de capacidade da serra. Isso aparece em vermelho no painel e é marcado claramente no CSV:

*"RIP TO WIDTH 280mm → 150mm — EXCEEDS SAW CAPACITY (250mm)"*

Definido em 0 (padrão), nenhuma verificação de capacidade é feita.

### Importação JARCH Vis (PRO)

JARCH Vis é um addon do Blender que cria revestimentos arquitetônicos — siding, pisos, forros de telhado em madeira — como objetos de malha única contendo muitas tábuas individuais. O CutVisor PRO pode dividir esses objetos em peças individuais mensuráveis automaticamente.

O processo de importação trata vários problemas que surgem de como o JARCH cria a geometria:

**Faces espúrias de cortes booleanos.** Quando um revestimento é recortado em volta de janelas ou portas, operações booleanas às vezes deixam faces 2D isoladas — geometria plana sem volume. O CutVisor detecta essas faces (por espessura e planaridade) e as remove antes da contagem.

**Bordas de malha abertas.** Cortar uma tábua deixa uma face aberta no ponto do corte. O CutVisor preenche esses buracos automaticamente, o que é necessário para cálculos corretos de volume e para algumas ferramentas posteriores.

**Medição de dimensões em tábuas inclinadas.** Tábuas de revestimento horizontais não estão alinhadas com os eixos do mundo — elas se posicionam em ângulo na parede. A medição padrão por caixa delimitadora dá dimensões infladas para objetos rotacionados (uma tábua de 20mm de espessura inclinada a 60° mediria 40mm usando uma caixa alinhada aos eixos). O CutVisor identifica cada eixo a partir da área das faces: as faces planas dominantes (frente/verso da tábua) dão a direção da espessura, e as bordas laterais longas — de longe as maiores superfícies planas restantes em uma tábua simples — dão a direção da largura; o comprimento é o que resta. Como isso se baseia na área da superfície em vez da posição dos vértices, permanece preciso mesmo em importações finamente tesseladas, bordas chanfradas, ou tábuas com um chanfro ausente em uma extremidade — e funciona corretamente se a tábua estiver horizontal, vertical, inclinada ou completamente rotacionada.

Essa medição é feita a partir da malha base, antes da avaliação dos modificadores — modificadores não destrutivos como Solidify, Bevel ou Subdivision Surface não são refletidos nas dimensões reportadas. Aplique esse tipo de modificador antes, se precisar que ele seja incluído na medição.

Depois da importação, as peças aparecem em uma coleção dedicada do Blender, nomeada de acordo com o objeto original, prontas para atribuição de material e geração de Part ID.

### Etiquetas na viewport

A aba View controla as etiquetas desenhadas diretamente na viewport 3D para qualquer objeto selecionado. Você pode mostrar qualquer combinação de Part ID, nome do objeto, dimensões e contagem de instâncias. As etiquetas são ancoradas em relação ao painel N (barra lateral direita) ou ao painel T (barra de ferramentas esquerda) — não à borda da tela — então nunca se sobrepõem ao painel do CutVisor nem a nenhum outro painel do Blender, mesmo que você os redimensione.

Opções de sombra e contorno tornam as etiquetas legíveis sobre qualquer cor de fundo. Quatro predefinições de um clique (All, IDs only, Dims+Count, Off) permitem alternar rapidamente entre modos de informação durante uma sessão de trabalho.

Todas as configurações de overlay são atualizadas instantaneamente na viewport quando alteradas — não é preciso mover a câmera para forçar uma atualização.

### A pasta de exportação

Todos os arquivos exportados pelo CutVisor — lista de compras, plano de corte, lista de materiais — são salvos em uma pasta nomeada de acordo com o seu arquivo do Blender, criada automaticamente no mesmo local:

minha\_casa.blend

minha\_casa\_lists/

    Shopping\_List.csv

    BOM\_Detailed.csv

    BOM\_Consolidated.csv

Essa pasta é criada na primeira vez que você exporta, e todas as exportações seguintes vão para o mesmo lugar. O botão "Open Lists Folder" a abre diretamente no Explorer (Windows), Finder (Mac) ou Arquivos (Linux) — sem navegação manual.

### O CSV da lista de compras

Cada vez que você gera uma lista de compras, o CutVisor salva um novo arquivo com data e hora — `Shopping_List_20240615_143022.csv` — para que versões anteriores nunca sejam sobrescritas. Isso permite comparar diferentes configurações lado a lado: diferentes ajustes de kerf, diferentes comprimentos de estoque, diferentes agrupamentos.

O arquivo exportado tem três seções:

**Seção 1 — Lista de compras.** O que pedir ao fornecedor: material, perfil (largura × espessura), comprimento de estoque, quantidade, preço unitário, subtotal, porcentagem de desperdício. Essa é a lista que você leva ao comerciante ou envia por e-mail. Se uma tábua precisar ser desdobrada na medida, uma nota aparece na coluna Notes.

**Seção 2 — Plano de corte.** Para cada tábua comprada, a sequência de cortes: quais peças entram nela, em que ordem, quanto desperdício resta no final. Essa é a lista que você leva até a serra.

**Seção 3 — Peças não correspondentes.** Qualquer peça que não pôde ser associada a um tamanho de estoque, com o motivo. Causas comuns: nenhum estoque definido para esse material; o perfil não corresponde a nenhum estoque; a peça é mais longa do que todos os comprimentos de estoque disponíveis. Essa seção vazia significa que o plano está completo.

Se você trabalha com **Geometry Nodes** para criar arrays, o CutVisor detecta o modificador e mostra um aviso com a opção de convertê-lo diretamente em um modificador Array clássico a partir do painel Parts. Depois da conversão, as quantidades são lidas automaticamente. Esse é o fluxo de trabalho recomendado para arrays baseados em GN em projetos que precisam de otimização de corte.

### Listas de materiais

**BOM detalhado:** uma linha por objeto, mostrando material, dimensões, quantidade (expandida a partir dos modificadores Array/Mirror), Part ID e nome do objeto. Útil para acompanhar elementos individuais ou para levantamento de quantitativos.

**BOM consolidado:** uma linha por tipo único de peça (material + dimensões), com quantidade total. Útil para compras, para cronogramas estruturais, e para comparação com a lista de compras.

Ambos os arquivos são salvos como texto UTF-8 com uma marca de ordem de bytes (BOM) no início, o que faz com que abram corretamente no Excel em todos os sistemas operacionais sem problemas de codificação de caracteres.

## O que o CutVisor ainda não faz

**Painéis.** Compensado, OSB, MDF, aglomerado, painéis CLT — otimizá-los requer encaixe 2D (encaixar retângulos em um painel), um problema diferente e mais complexo do que o corte linear. Está no roteiro de desenvolvimento.

**Elementos curvos ou não prismáticos.** O CutVisor mede peças retas com seção transversal retangular. Vigas curvas, elementos afunilados ou perfis complexos precisam de tratamento manual.

**Cálculos estruturais.** O CutVisor diz o que comprar, não se vai aguentar em pé. A verificação estrutural segue seu próprio processo e suas próprias normas (EC5 para madeira na Europa). As duas coisas são complementares, não concorrentes.

**Ferragens de conexão.** Parafusos, pregos, cantoneiras, suportes de viga — fora do escopo desta versão.

Essas não são lacunas esquecidas. Elas definem o limite do que o CutVisor promete fazer bem, que é a única coisa que ele realmente faz: dizer que madeira comprar e como cortá-la.

## Ativação — Core e Pro

O CutVisor é distribuído como um único addon. Instalado sem chave de licença, ele funciona em modo Core. Inserir uma chave Pro válida em **Settings → Enter / Change Key** desbloqueia todos os recursos Pro instantaneamente — sem precisar reiniciar.

A chave de licença tem o formato `CWTW-XXXX-XXXX-XXXX-XXXX`. Ela é validada localmente por meio de uma assinatura criptográfica — nenhuma conexão à internet é necessária para a ativação ou para o uso contínuo.

**Para ativar o Pro:**
1. Compre uma chave em https://cutvisor.gumroad.com/l/yiqahd
2. Copie a chave do seu e-mail de confirmação ou da sua biblioteca no Gumroad
3. Abra o Blender → painel N → CutVisor → **Settings**
4. Clique em **Paste Key** (se a chave estiver na área de transferência) ou **Enter / Change Key**
5. A linha de status muda para "✓ CutVisor Pro active"

Uma única chave é licenciada para uso pessoal em computadores que você possui. As chaves não expiram e permanecem válidas em todas as atualizações.

---

## Licença

O CutVisor usa um modelo de licença dual.

### Core — GNU General Public License v3.0

O código do CutVisor Core é software livre: você pode redistribuí-lo e/ou modificá-lo nos termos da GNU General Public License publicada pela Free Software Foundation, versão 3 ou posterior.

Texto completo da licença: https://www.gnu.org/licenses/gpl-3.0.html

### Recursos Pro — Licença comercial

Os recursos Pro do CutVisor — importação CSV para materiais e estoque, estilo avançado de overlay, predefinições de espécies de madeira, predefinições personalizadas de materiais salvas, variação de largura e limites de desdobramento/corte transversal, emenda de várias tábuas, importação JARCH Vis, e todos os recursos desbloqueados por uma chave de ativação `CWTW-…` — não são cobertos pela GPL.

Ao comprar uma chave Pro, você concorda com os seguintes termos:

- A chave é uma licença de usuário único, não transferível
- Você pode instalar e usar o CutVisor Pro em computadores que possui ou controla pessoalmente
- A redistribuição de chaves de ativação Pro é estritamente proibida
- Não é permitida engenharia reversa ou contornar o sistema de ativação
- A licença Pro não expira; atualizações futuras são fornecidas a critério do autor

---

## Pensamento final

As melhores ferramentas são aquelas que você esquece que está usando. Você está pensando no projeto — a estrutura, as ligações, as proporções — e a ferramenta cuida da contabilidade discretamente em segundo plano.

O CutVisor não vai te tornar um projetista melhor. Mas vai garantir que o que você projetou seja o que se constrói, no custo certo, com a quantidade certa de material, e com menos desperdício na caçamba no final.

Isso já parece suficiente.

*O CutVisor é desenvolvido por um docente de faculdade de arquitetura com interesse em design computacional e construção sustentável em madeira.* *Comentários, sugestões de recursos e relatos de bugs são genuinamente bem-vindos — esta ferramenta cresce com seus usuários.*


</div>
