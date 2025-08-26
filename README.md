Atomic Sales Dashboard — Assessment Submission
Next.js **15** · **TypeScript** · **Tailwind CSS** · **Recharts** · **Atomic Design**

Objective
Build a basic website following the **atomic structural principle**, show **Sales** for **2022, 2023, 2024** with charts, and provide enhancements:
•	Custom **threshold** input
•	**Multiple chart types** (Bar/Line/Pie)
•	Optional **API** integration / Kaggle CSV mapping

What I Implemented
•	✅ Next.js 15 + TypeScript + Tailwind project with **atomic architecture**
  (atoms → molecules → organisms → templates)
•	✅ Dashboard page (empty shell) with chart organism inserted
•	✅ Multiple chart components using **Recharts** (Bar / Line / Pie)
•	✅ Mock sales data (2022–2024), year selector, min-sales threshold filter
•	✅ API route at `/api/sales?year=YYYY` for future integration
•	✅ Clear README with setup & submission steps

---

Requirements Checklist
•	[x] Next.js 15, TypeScript, Tailwind
•	[x] Atomic structural principle
•	[x] Multiple chart components (Recharts)
•	[x] Empty **/dashboard** page and composed component
•	[x] README (what I did + how to set up)
•	[x] Prepped for GitHub submission

---

Getting Started
1) Prerequisites
•	Node.js 18+ (LTS recommended)
•	pnpm / npm / yarn

2) Install
pnpm install
# or npm install
# or yarn

3) Run Dev
pnpm dev
# or npm run dev
# or yarn dev
Open http://localhost:3000 and click **Go to Dashboard** (or visit `/dashboard`).

4) Build
pnpm build && pnpm start

---

Project Structure (Atomic)
app/
  api/sales/route.ts        # API endpoint returning sales data
  dashboard/page.tsx        # Empty dashboard page; organism is inserted
  globals.css
  layout.tsx
  page.tsx                  # Landing page linking to /dashboard
public/
  data/sales.csv            # (Optional) CSV example for Kaggle mapping
src/
  components/
    atoms/                  # Button, Input, Select
    molecules/              # ChartSwitcher, ThresholdFilter, YearSelect
    organisms/              # ChartBar, ChartLine, ChartPie, SalesExplorer
    templates/              # DashboardTemplate
  data/
    sales.ts                # Mock sales data for 2022–2024
  lib/
    csv.ts                  # Minimal CSV parser (for optional Kaggle import)

---

Using Kaggle Data (CSV Mapping)
The app ships with mock data in `src/data/sales.ts`. To use a Kaggle-style CSV instead:

1) Put your CSV at `public/data/sales.csv` with columns:
year,month,sales
2022,Jan,8200
2022,Feb,7900
...
•	**year:** number (e.g., 2023)
•	**month:** "Jan"..."Dec" or full names (Jan/January etc.)
•	**sales:** number

2) Use this helper to load & parse CSV (already added at `src/lib/csv.ts`):
import { parseSalesCsv } from "@/lib/csv";

async function loadCsv(year: number) {
  const res = await fetch(`/data/sales.csv`);
  const text = await res.text();
  const all = parseSalesCsv(text);            // → SaleRow[]
  return all.filter(r => r.year === year);
}

3) Swap `SalesExplorer` to fetch from CSV instead of `src/data/sales`:
// src/components/organisms/SalesExplorer.tsx (example snippet)
import { useEffect, useState } from "react";
import { parseSalesCsv } from "@/lib/csv"; // add this import

// inside component:
const [data, setData] = useState<any[]>([]);

useEffect(() => {
  (async () => {
    const res = await fetch(`/data/sales.csv`);
    const text = await res.text();
    const all = parseSalesCsv(text);
    setData(all.filter(r => r.year === year).filter(r => r.sales >= threshold));
  })();
}, [year, threshold]);

> **Note:** If your Kaggle months are full names (e.g., "January"), the parser normalizes to "Jan"… "Dec".

Using the API instead of CSV
You can also call the built-in API:
const res = await fetch(`/api/sales?year=${year}`);
const json = await res.json();
setData((json.data ?? []).filter((r:any) => r.sales >= threshold));

---

Enhancements (Ready)
•	Threshold input to filter out low sales
•	Buttons to switch **Bar / Line / Pie**
•	API endpoint scaffold for real data
•	Easy swap to CSV

---

Assessment Submission — **GitHub Link Required**
Your submission **must include a public GitHub repository link**. Without it, it will not be assessed.

A) Create the GitHub repo and push
# from project root
git init
git add .
git commit -m "feat: atomic sales dashboard (assessment)"
# Option 1: create on GitHub first, then add remote:
git remote add origin https://github.com/<YOUR-USERNAME>/<REPO-NAME>.git
git push -u origin main

# Option 2 (GitHub CLI):
# gh repo create <REPO-NAME> --public --source=. --remote=origin --push

B) Make sure the repo is public
•	On GitHub, go to **Settings → General → Danger Zone → Change visibility** if needed.

C) Copy the link to submit
•	The link should look like:
  `https://github.com/<YOUR-USERNAME>/<REPO-NAME>`
•	Paste this link into your **Assessment Submission Form** where it says “GitHub link”.

D) (Optional) Tag your submission
git tag -a v1.0.0 -m "assessment submission"
git push origin v1.0.0

---

Why Atomic?
Smaller, focused components are easier to test and reuse. We build from **atoms** (inputs, buttons) → **molecules** (composed UI controls) → **organisms** (feature sections) → **templates** (page scaffolds).

---

License
MIT
