# Calculus Connections Explorer

An interactive web application that visualizes how **Calculus** topics connect to domain-specific courses in **Computer Science** and **Mechanical Engineering**. Users can explore prerequisite relationships, filter by course or topic, and read rationales explaining why each calculus concept matters for their chosen field.

Updated March 20, 2026

---

## Repository

- **GitHub (public):** [https://github.com/Boxxelf/Calculus-Connections-Explorer-Update-0320](https://github.com/Boxxelf/Calculus-Connections-Explorer-Update-0320)

---

## Public View Links

- **Home (pick CS or ME — default):** [https://boxxelf.github.io/Calculus-Connections-Explorer-Update-0320/](https://boxxelf.github.io/Calculus-Connections-Explorer-Update-0320/)
- **CS:** [https://boxxelf.github.io/Calculus-Connections-Explorer-Update-0320/cs.html](https://boxxelf.github.io/Calculus-Connections-Explorer-Update-0320/cs.html)
- **ME:** [https://boxxelf.github.io/Calculus-Connections-Explorer-Update-0320/meche/](https://boxxelf.github.io/Calculus-Connections-Explorer-Update-0320/meche/)
- **Legacy `home.html`:** redirects to the site root.

---

## Features

- **Interactive concept map** — Zoom, pan, and explore the calculus topic graph
- **Topic filtering** — Select CS or MechE courses and topics to see connected calculus ideas
- **Ranked calculus list** — View connected calculus topics with connection strength
- **Rationales** — Read explanations of how each calculus concept supports the selected domain topics
- **Full concept map view** — Optional graph-based visualization with zoom and pan

---

## Local Development

### Option 1: Python HTTP Server

```bash
# From the project root
python -m http.server 8000
# Or: python3 -m http.server 8000
```

Then open:
- **Home:** http://localhost:8000/
- **CS:** http://localhost:8000/cs.html
- **ME:** http://localhost:8000/meche/

### Option 2: PowerShell (Windows)

```powershell
.\serve.ps1
# Or with custom port: .\serve.ps1 8888
```

Then open:
- **Home:** http://localhost:8000/
- **CS:** http://localhost:8000/cs.html
- **ME:** http://localhost:8000/meche/

---

## Deploy to GitHub Pages

### Step 1: Create a GitHub Repository

1. Go to [https://github.com/new](https://github.com/new)
2. Set the repository name (e.g., `Calculus-Connections-Explorer-Update-0320`)
3. Make it **Public**
4. Do **not** initialize with README, .gitignore, or license

### Step 2: Push the Code

```bash
git init
git add .
git commit -m "Initial commit"
git branch -M main
git remote add origin https://github.com/YOUR_USERNAME/YOUR_REPO_NAME.git
git push -u origin main
```

### Step 3: Enable GitHub Pages

**Option A — Deploy from branch (simplest):**

1. In your GitHub repository, go to **Settings** → **Pages**
2. Under **Source**, select **Deploy from a branch**
3. **Branch:** `main`
4. **Folder:** `/ (root)`
5. Click **Save**

**Option B — GitHub Actions:**

1. In **Settings** → **Pages**, select **GitHub Actions** as the source
2. The `.github/workflows/deploy.yml` workflow will run on each push to `main`

After a few minutes, your site will be live at:
`https://YOUR_USERNAME.github.io/YOUR_REPO_NAME/`

---

## Project Structure

```
├── index.html              # Home: pick CS or ME
├── cs.html                 # CS–Calculus explorer
├── home.html               # Redirects to ./
├── app.js                  # CS application logic
├── style.css               # Shared styles
├── graph_data.json         # CS graph data
├── meche/
│   ├── index.html          # ME version
│   ├── app-meche.js        # ME application logic
│   └── graph_data_meche.json
├── Calculus topic list-Table 1.csv
├── CS topic lists-Table 1.csv   # CS topics
├── ME topic lists.csv           # ME topics
└── README.md
```

---

## Update Notes — March 20, 2026 (Mechanical Engineering track)

This section summarizes changes made to **Calculus Connections Explorer** (Mechanical Engineering track) to align topic lists and in-app rationales with the **MechE–Calculus connection** tables dated **03/06/26** in `MechE-Calc connections_030626/`.

### 1. ME topic lists (Dynamics & Fluid Mechanics)

**Files**

- `ME topic lists.csv` (repository root)
- `MechE-Calc connections_030626/ME topic list-Table 1.csv`

**What changed**

- Topic rows were **regenerated** from the ME topic columns in:
  - `MechE-Calc connections_030626/Dynamics-Calc-Table 1.csv` (column *Dynamics Topic*)
  - `MechE-Calc connections_030626/Fluids-Calc-Table 1.csv` (column *Fluids Topic*)
- Topics are **unique per course**, in **first-appearance order** as they appear in those tables (so names stay consistent with the connection and rationale rows).
- **`Heat Transfer` was removed entirely** from the ME topic lists so the UI matches the current chart set (no Heat section in the new bundle).
- The sheet file `ME topic list-Table 1.csv` was previously empty; it now **mirrors** the root `ME topic lists.csv`. (The `MechE-Calc connections_030626/*.csv` exports omit an Excel-style `Table 1` title row so each file is a single rectangular CSV for tools such as CSVLint.)
- CSV **quoting** is correct for topic names that contain commas (e.g. *Potentials, conservativation of energy, and momentum*).

**Course column values**

| Source file                         | `Course` value in lists |
|------------------------------------|-------------------------|
| Dynamics connection table          | `Dynamics`              |
| Fluids connection table            | `Fluid Mechanics`       |

### 2. Rationales in the MechE graph (`graph_data_meche.json`)

**What changed**

- Each node’s `rationales` object was **filled** from the two connection tables:
  - **Dynamics** → entries under `"Dynamics"`
  - **Fluids** → entries under `"Fluid Mechanics"`
- Each stored entry uses the same shape as the CS site: `{ "cs_topic": "<ME topic name>", "rationale": "<text>" }`  
  (`cs_topic` is the ME filter key expected by `meche/app-meche.js`.)
- Calculus topics in the CSVs were matched to graph nodes by **`label`** (with normalization of curly vs straight apostrophes only for matching).
- **`"Heat Transfer"` was removed** from `rationales` on every node so it matches the updated ME topic list.

**Counts (for verification)**

- **36** Dynamics rationale rows → **36** entries appended across nodes.
- **54** Fluids rationale rows → **54** entries appended across nodes.
- **65** nodes and **174** edges are **unchanged**; only `rationales` (and empty `cs_categories` formatting where rewritten) were updated.

### 3. Documentation

**File:** `meche/README.md`

- Updated to state that rationales come from the **Dynamics** and **Fluids** CSVs merged into `graph_data_meche.json`, and that ME topics are **Dynamics** and **Fluid Mechanics** only (no Heat Transfer in the current bundle).

### 4. What was *not* changed

- **Concept map topology** (nodes and edges) was not recomputed from the CSVs; it remains the existing calculus prerequisite graph in `graph_data_meche.json`.
- **`meche/app-meche.js`** was not modified; it already reads rationales from JSON and ME topics from `ME topic lists.csv`.

### 5. How to refresh data after future CSV edits

1. Update the connection tables in `MechE-Calc connections_030626/`.
2. Rebuild `ME topic lists.csv` / `ME topic list-Table 1.csv` from the ME topic columns (same rules as above) if topic names change.
3. Re-merge rationale rows into `graph_data_meche.json` so `cs_topic` strings **exactly match** the names in `ME topic lists.csv` (including punctuation and apostrophes).

---

## Related Documentation

- [DEPLOYMENT.md](./DEPLOYMENT.md) — Detailed deployment guide
- [GITHUB_PAGES_SETUP.md](./GITHUB_PAGES_SETUP.md) — GitHub Pages configuration options
- [readme_0320.md](./readme_0320.md) — March 20, 2026 update notes (same content as the section above)
- [UPDATE_NOTES0125.md](./UPDATE_NOTES0125.md) — January 2026 update notes
- [UPDATE_NOTES1201.md](./UPDATE_NOTES1201.md) — December 2024 update notes

---

## License

See repository for license information.
