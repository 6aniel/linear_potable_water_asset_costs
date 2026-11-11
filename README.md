# Water infrastructure IVI dashboard

This folder is the repository root for https://github.com/6aniel/linear_potable_water_asset_costs.

## First publication

1. In GitHub: Settings → Pages → Build and deployment → Source → GitHub Actions.
2. Commit and push this folder's contents to `main` (including `.github/workflows/publish.yml`).
3. The Publish Quarto dashboard workflow renders and deploys the site.

Expected address: https://6aniel.github.io/linear_potable_water_asset_costs/

If the remote already has commits, fetch and integrate them before pushing. Do not force-push.

## Local preview

Install Quarto, then run `quarto preview` from this folder. No Python runtime is needed to render this dashboard; Observable JS runs the interactive controls in the browser.

## Update results

Run IVI.ipynb in the parent analysis project, then copy the three CSVs from Outputs/IVI into data/ and commit/push those changes. The files here are snapshots, not automatically synchronized with the parent project.

The dashboard uses 2020 constant dollars and a fixed observed 2025 cohort. It describes assessed assets, not complete provincial networks. Raw pipe geometry and analysis notebooks are not required for website deployment.
