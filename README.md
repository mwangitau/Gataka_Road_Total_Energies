# ⛽ Kiryan Energy Ltd — TotalEnergies Gataka Road Dashboards

![Status](https://img.shields.io/badge/Status-Active-success)
![License](https://img.shields.io/badge/License-Proprietary-red)
![Dashboards](https://img.shields.io/badge/Dashboards-4-blue)
![Version](https://img.shields.io/badge/Version-1.0-blueviolet)

**Comprehensive performance analytics dashboards for TotalEnergies Gataka Road Service Station operations, Kajiado, Kenya.**

Built from the proven Shell Mangu Road dashboard suite, reconfigured for Gataka's products, pump layout, and cylinder sizes.

---

## 📁 Repository Structure

```
totalenergies_gataka_rd/
├── index.html                           # Landing page — links to all dashboards
├── README.md                            # This file
├── LICENSE.txt                          # Proprietary licence
├── staff_dashboard.html                 # Staff shortages & balance tracking
├── fuel/
│   └── fuel_dashboard_easy_config.html  # Per-pump volume & target dashboard
├── Product/
│   └── product_dashboard.html           # Fuel product inventory & variance
└── Gas/
    └── gas_dashboard_pivot_format.html  # LPG gas cylinder dashboard
```

---

## ⚠️ STEP 1 — PASTE YOUR GOOGLE SHEETS CSV LINKS

Every dashboard currently has a **placeholder** instead of a live data source. Open each file, find the `dataSources` (or `DATA_SOURCES` / `FUEL_SOURCES` / `GAS_SOURCES`) object near the top of the `<script>` section, and replace the placeholder string with your published CSV link:

| File | Variable | Used for |
|---|---|---|
| `fuel/fuel_dashboard_easy_config.html` | `dataSources` | Pump dashboard |
| `Product/product_dashboard.html` | `dataSources` | Product dashboard |
| `Gas/gas_dashboard_pivot_format.html` | `dataSources` | Gas dashboard |
| `staff_dashboard.html` | `DATA_SOURCES` | Staff dashboard |
| `index.html` | `FUEL_SOURCES`, `GAS_SOURCES` | Landing page live ticker |

**Publishing a Google Sheet as CSV:** File → Share → Publish to web → select the specific tab → CSV format → copy the link.

Add one entry per month: `"June 2026": "https://docs.google.com/.../pub?output=csv"`. The newest month should appear at the **top** of the object.

---

## ⚠️ STEP 2 — CONFIRM / ADJUST YOUR TARGETS AND SPLITS

Numbers below were set from what you provided. Anything marked **(placeholder)** is an estimate that should be replaced once you have real data.

### ⛽ 1. Pump Performance Dashboard
**File:** `fuel/fuel_dashboard_easy_config.html`

Gataka's nozzle layout (as you described it):
| Pump | Nozzles |
|------|---------|
| Pump 1 | AGO1, AGO2, PMS1, PMS2 |
| Pump 2 | AGO3, AGO4, PMS3, PMS4 *(centre of island — highest traffic)* |
| Pump 3 | AGO5 only |

The CSV format this dashboard reads is **pump-level**, not nozzle-level (one volume figure + one CSA name per pump per day — same as Mangu's proven format). If you need true per-nozzle tracking, that's a bigger rebuild — ask and I'll extend it.

**Monthly target split (litres) — placeholder, weighted for Pump 2's central-island advantage:**
| Pump | Share | Litres |
|------|:---:|:---:|
| Pump 1 | 35% | 63,000 |
| Pump 2 | 45% | 81,000 |
| Pump 3 | 20% | 36,000 |
| **Station Total** | | **180,000** |

👉 Edit the `PUMP_TARGETS` object in the file once you know actual per-pump litreage.

---

### 📊 2. Fuel Product Analysis Dashboard
**File:** `Product/product_dashboard.html`

**Monthly targets (litres):**
| Product | Target |
|---------|:------:|
| PMS — Petrol | 108,000 |
| AGO — Diesel | 72,000 |
| LUBES — Lubricants | 45,000 *(from 1,500 L/day)* |

Same CSV format as before: `DATE, PRODUCT, OPEN DIP, DELIVERY, CLOSE DIP, DIP SALE, METER SALE, VAR, EXPLAIN > 5%`, with `PRODUCT` values `PMS`, `AGO`, `LUBES`.

**Note:** Lubricants are tracked through the same open/close-dip inventory model as PMS/AGO. If lubes are actually sold as packaged units (not dipped tanks), let me know and I'll switch that product to a simple sales-count model instead.

---

### 🔥 3. LPG Gas Dashboard
**File:** `Gas/gas_dashboard_pivot_format.html`

**Monthly refill target: 2,000 KG/month** (refills only — from 66.7 KG/day).

**Cylinder sizes supported:** 3KG · 6KG · 13KG · 22.5KG · 50KG (Gataka's actual sizes — different from Mangu's 6/13/25/45).

**Target split — placeholder, mirrors the dominant-13KG pattern from Mangu:**
| Size | Share | KG |
|------|:---:|:---:|
| 13KG | 45% | 900 |
| 6KG | 22% | 440 |
| 22.5KG | 15% | 300 |
| 50KG | 10% | 200 |
| 3KG | 8% | 160 |

👉 Edit the `t13/t6/t225/t50/t3` constants once you know Gataka's actual size mix. The Mangu version had a special "fixed weekly-supply, 1 customer" rule for its 45KG size — that assumption was **removed** here since it wasn't confirmed for Gataka's 50KG. Tell me if Gataka has a similar bulk customer and I'll add it back.

Pivot CSV format unchanged: Row 1 = dates (`2026.06.01` etc.), Column A = product names (e.g. `13KG REFILL`, `6KG EMPTY`), cells = pieces sold. Any product name containing `EMPTY` = new customer, not gas volume.

---

### 👥 4. Staff Shortages Dashboard
**File:** `staff_dashboard.html`

CSA/staff names are read automatically from your CSV's column headers — no hardcoded names to update. Just paste your CSV link(s) into `DATA_SOURCES`.

---

### 🏠 5. Landing Page
**File:** `index.html`

Live ticker pulls today's average PMS/AGO/LUBES sales and LPG KG directly from the Product and Gas CSVs. Needs `FUEL_SOURCES` and `GAS_SOURCES` filled in (Step 1).

---

## 🎨 Design System

TotalEnergies brand palette applied throughout:

| Token | Hex | Usage |
|-------|-----|-------|
| Deep navy | `#0a1628` | Header background |
| TotalEnergies blue | `#003DA5` | Primary accent, Pump 1 |
| TotalEnergies red | `#E2001A` | Primary accent, PMS, Pump 2 |
| Amber | `#d97706` | Lubricants, warnings |
| Green | `#059669` | On track / above target |

Fonts: Barlow Condensed (headings, numbers) · Barlow (body) — unchanged from the original design system.

---

## ⚙️ Adding a New Month

Same pattern across all dashboards: open the file, add a new entry at the top of the relevant data-source object, save, and redeploy. The new month appears automatically in the filter dropdown.

---

## 🌐 Deployment

Deploy the same way as the original suite — push to GitHub Pages from `main`, or host on Vercel/Netlify. No build step required; these are static HTML files.

```bash
git init
git add .
git commit -m "Initial commit: TotalEnergies Gataka Road dashboard suite"
git remote add origin <your-new-repo-url>
git push -u origin main
```

---

## 🔒 Security & Confidentiality

> ⚠️ **These dashboards contain sensitive business data for official use only.**

- Do not commit real CSV links to a public repository — they expose raw operational data. Keep the repo private, or strip links before pushing and re-paste locally.
- For external sharing (auditors, TotalEnergies) — export to PDF via browser print, do not share live dashboard links.

---

## 📞 Contact

| | |
|--|--|
| Email | mwangijohngitau1@gmail.com |
| Station Manager | John Gitau (Knee) |
| Location | TotalEnergies Gataka Road Service Station, Kajiado, Kenya |

---

## 📄 Licence

**PROPRIETARY SOFTWARE — ALL RIGHTS RESERVED**

Copyright © 2026 Kiryan Energy Ltd · TotalEnergies Gataka Road Service Station, Kenya

Unauthorised copying, reproduction, distribution, modification, or use is strictly prohibited under the Kenya Copyright Act (Cap. 130) and the Computer Misuse and Cybercrimes Act 2018.

**Permitted use:** Internal use by authorised Kiryan Energy Ltd personnel only.

---

<div align="center">

**Built for operational excellence at TotalEnergies Gataka Road Service Station**

**© 2026 Kiryan Energy Ltd — All Rights Reserved**

</div>
