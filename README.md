**Cognitive Skills & Student Performance Dashboard**

**Name: Irugu Jayasree**
---
**Overview**
* Synthetic student dataset: cognitive skills and performance
* Reproducible analysis in Jupyter: EDA, correlations, ML prediction, clustering
* Next.js dashboard: overview stats, charts (bar, scatter, radar), searchable/sortable table, insights
---
**End-to-end flow**
1. Generate or update data in `data/` (or replace `students.csv`).
2. Run the notebook `notebooks/analysis.ipynb` to compute correlations, models, and personas.
3. Export JSONs to `data/exports/` and copy to `dashboard/public/data/`.
4. Run the Next.js dashboard to visualize insights and explore students.
---
**Project Structure**
* `data/`: dataset and generator
  • `generate_dataset.py`
  • `students.csv`
  • `exports/` JSONs for dashboard
* `notebooks/analysis.ipynb`: full analysis and export
* `dashboard/`: Next.js app (App Router, Tailwind)
---
**Prerequisites**
* Python 3.11+ (venv recommended)
* Node.js 18+ (npm or pnpm)
---
**1. Data & Analysis**
```bash
# From repo root
python -m venv .venv
# Windows
.\.venv\Scripts\activate
pip install -U pip pandas numpy scikit-learn matplotlib seaborn jupyter nbconvert
python data/generate_dataset.py
python -m jupyter nbconvert --to notebook --execute --inplace notebooks/analysis.ipynb
```
Exports produced under `data/exports`:
* `students_enriched.json` (per-student records incl. cluster/persona)
* `overview.json` (topline averages)
* `insights.json` (high-level findings)
* `correlations.json` (feature ↔ score correlations)
* `model_metrics.json` (ML evaluation)
* `clusters.json` (cluster summaries)
---
**2. Dashboard**
```bash
cd dashboard
npm install
# copy exports to public
mkdir -p public/data
# Windows PowerShell
Copy-Item ..\data\exports\*.json public\data -Force
npm run dev
```
Visit: https://dashboard-ook1gjciv-jayas-projects-1dd63b8b.vercel.app/
---
**Dashboard Guide**
* Overview: key averages (score, skills, engagement)
* Charts:
  • Bar: skill ↔ score correlations
  • Scatter: attention vs assessment score
  • Radar: individual profile across 5 axes (incl. engagement)
* Students table:
  • Search: name (partial), ID (exact), class (exact; e.g., 9A–12D)
  • Sort: by score or any skill/engagement column
  • Pagination: 5/10/20/50 per page with page groups
  • Persona column: Low/Mid/High Performer
---
**3. GitHub Setup**
```bash
git init
git add .
git commit -m "Initial commit: data, notebook, dashboard"
git branch -M main
git remote add origin <YOUR_REPO_URL>
git push -u origin main
```
---
**4. Deploy (Vercel)**
* Install Vercel CLI: `npm i -g vercel`
* Navigate to dashboard and deploy:
```bash
cd dashboard
vercel
```
* After first deploy, run:
```bash
vercel --prod
```
* Ensure `public/data/*.json` files are present before deploying.
---
**Why Linear Regression?**
* Explainability: Linear models provide transparent coefficients that directly show how each cognitive skill contributes to assessment score.
* Simplicity & baseline: Strong, fast baseline for structured data; avoids overfitting; easy to troubleshoot.
* Complementary: A `RandomForestRegressor` is also used; Linear Regression remains the primary interpretable model.
---
**Clustering (Personas)**
* Students are clustered into three personas (Low, Mid, High) using KMeans on normalized features:
  • comprehension
  • attention
  • focus
  • retention
  • engagement\_time
  • assessment\_score
* Clusters are ordered by mean assessment score and labeled accordingly.
---
**Data Schema (students)**
**Required Columns:**
* `student_id`, `name`, `class` (supports 9A–12D)
* `comprehension`, `attention`, `focus`, `retention` (0–100)
* `engagement_time` (minutes/week)
* `assessment_score` (0–100)
**Derived in exports:**
* `cluster_id` (numeric), `persona_label` (Low/Mid/High)
---
**Troubleshooting**
* **Port in use / site not reachable:**
```bash
npx --yes kill-port 3000 3001
npm run dev
```
* **fs module error in Next.js:**
  Ensure JSON is loaded from `/public/data/*.json` in the client, not using Node `fs`.
* **Tailwind classes not applied:**
  Verify that `globals.css` uses Tailwind v3 directives and the `dark` class toggles on `<html>` element.
---
**FAQ**
* **Can I use my own CSV?**
  Yes. Replace `data/students.csv`, rerun the notebook, and copy exports to `dashboard/public/data/`.
* **How are personas mapped?**
  By ordering clusters by mean `assessment_score`: Low < Mid < High.
---
**Example Findings**
* Attention, focus, and comprehension correlate strongly with assessment score.
* Simple models like Linear Regression and Random Forest achieve high R² on synthetic data.
* KMeans personas distinguish high-attention/focus vs. high-engagement profiles.
---
**Notes**
* The dataset is synthetic. To change its behavior, adjust parameters in `generate_dataset.py`.
* Rerun the notebook after changes to refresh exports.
---
Let me know if you want this turned into a markdown `.md` file or formatted for HTML/README use.
