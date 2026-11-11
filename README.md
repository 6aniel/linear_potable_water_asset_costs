# Water infrastructure IVI dashboard

This folder is the repository root for https://github.com/6aniel/linear_potable_water_asset_costs.

## IVI formulation

Alegre, Vitorino, and Coelho define IVI as infrastructure current value divided by its replacement cost. Their asset-based formulation is:

$$
\mathrm{IVI}(t)
= \frac{\mathrm{Current\ value}(t)}{\mathrm{Replacement\ cost}(t)}
= \frac{\sum_{i=1}^{N} rc_{i,t}\left(\frac{rul_{i,t}}{eul_i}\right)}{\sum_{i=1}^{N} rc_{i,t}}
\qquad \text{[Alegre et al. 1429, eqs. 1–2]}
$$

Here, $rc_{i,t}$ is asset $i$'s replacement cost, $rul_{i,t}$ is its residual useful life, and $eul_i$ is its expected useful life. We express IVI as a fraction from 0 to 1; multiplying by 100 expresses it as a percentage.

## Formula used in this analysis

For province $p$, the cost rate is:

$$
c_p = \frac{R_{p,2020}}{L_{p,2020}}
\quad [\mathrm{2020\ dollars/metre}],
\qquad RC_i = \ell_i c_p
$$

$R_{p,2020}$ is the CCPI estimated replacement value for potable water pipes; $L_{p,2020}$ is the full reported provincial pipe length in metres, including local, transmission and unknown-diameter pipes without double-counting totals. Both use the same reference year and ownership category, All public organizations. $\ell_i$ is the observed pipe length within its CSD. These rates remain fixed in constant 2020 dollars.

For installation year $y_i$, expected life $E_i$, and evaluation year $t$:

$$
f_{i,t} = \frac{\max\left(0, E_i-(t-y_i)\right)}{E_i},
\qquad CV_{i,t} = RC_i f_{i,t}
$$

For each CSD or province $g$, let $A_g$ contain the eligible observed pipes with valid installation years, positive expected lives, measurable lengths and usable cost rates, installed by 2025. The implemented IVI is:

$$
\mathrm{IVI}_{g,t}
= \frac{\sum_{i\in A_g} CV_{i,t}}{\sum_{i\in A_g} RC_i}
= \frac{\sum_{i\in A_g}\ell_i c_{p(i)}\,\frac{\max(0,E_i-(t-y_i))}{E_i}}{\sum_{i\in A_g}\ell_i c_{p(i)}}
$$

The same eligible assets enter both sums. The denominator is their assessed replacement value, not the full provincial CCPI value. Municipal installation year and expected life are reconstructed as $y_i=2025-\mathrm{age}_i$ and $E_i=\mathrm{age}_{i}+\text{remaining life}_{i,2025}$ (stored as `exp_rem_life`); ODI supplies installation year and expected life directly. Evaluation years are 2025 and 2030, 2040, …, 2100, assuming no renewal, removal or future additions.

Within each CSD or province, the uniform provincial cost rate cancels:

$$
\mathrm{IVI}_{g,t}=\frac{\sum_{i\in A_g}\ell_i f_{i,t}}{\sum_{i\in A_g}\ell_i}
$$

Thus, IVI is effectively length-weighted within these groups, while replacement-value estimates retain their dollar interpretation.

## Service-life-consumed score

$$
\mathrm{Service\ life\ consumed}_{g,t}=100\left(1-\mathrm{IVI}_{g,t}\right)
$$

This is the replacement-cost-weighted average percentage of benchmark service life consumed, capped at 100% for each pipe. For example, IVI = 0.40 gives 60% consumed. It adds no information beyond IVI, but presents ageing in the opposite direction: higher means more service life consumed. It is not the percentage of pipes that have failed, a failure probability, or a percentage of replacement work required. A pipe reaching benchmark life contributes 100% consumed even if it remains in service.

## Works cited

Alegre, Helena, et al. “Infrastructure Value Index: A Powerful Modelling Tool for Combined Long-Term Planning of Linear and Vertical Assets.” *Procedia Engineering*, vol. 89, 2014, pp. 1428–36. https://doi.org/10.1016/j.proeng.2014.11.469.

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

## Canada map

The dashboard includes a CSD choropleth with year and map-extent controls. Darker shading means higher service-life consumption (100 × (1 − IVI)), or higher assessed replacement value when that metric is selected. Unshaded areas have no displayed estimate. Simplified 2021 Statistics Canada CSD boundaries are included in `data/assessed_csds.geojson`; provincial outlines derived from those boundaries are in `data/canada_provinces.geojson`. These are administrative areas, not pipe service areas.
