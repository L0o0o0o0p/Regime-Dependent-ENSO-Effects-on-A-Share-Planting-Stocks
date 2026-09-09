# Regime-Dependent ENSO Factor in A-Share Planting Stocks

[中文简介](#中文简介) · [Notebook](notebooks/ENSO_Planting_Regime_Analysis.ipynb) · [Data notes](data/README.md) · [Result tables](results/tables)

This research project tests whether changes in the Niño 3.4 sea-surface-temperature anomaly are associated with forward returns of the Shenwan Planting Industry Index (801016), and whether that exposure changes across market regimes.

The main result is evidence of a **regime-dependent association**, not a claim that ENSO consistently predicts returns or that the selected event caused the beta shift.

## Research design

- **Climate signal:** one-calendar-day-lagged Daily Niño 3.4 SST anomaly, constructed from NOAA OISST v2.1.
- **Equity series:** Shenwan Planting Industry Index (801016), downloaded through AKShare.
- **Outcomes:** 1-, 5-, 20-, 60-, and 90-trading-day forward returns.
- **Inference:** OLS with HAC/Newey–West standard errors; the baseline lag length for an overlapping `h`-day return is `h - 1`.
- **Stability checks:** recent 5-year and 15-year samples.
- **Formal regime test:** a 5-day forward-return interaction regression around the WMO update dated 3 July 2026.

The formal model is:

> **Fwd5Dₜ = α + β₁ Nino34ₜ₋₁ + β₂ Postₜ + β₃(Nino34ₜ₋₁ × Postₜ) + εₜ**

Here, **β₃** tests whether the ENSO beta changes after the candidate breakpoint, and **βpost = β₁ + β₃**.

## Main findings

The fully rerun notebook, with climate data ending on 24 August 2026, indicates:

- The recent 5-year sample shows negative ENSO exposure at the 60- and 90-day horizons, while the same relationship is not stable in the 15-year sample.
- In the 5-day interaction model, the estimated pre-event beta is about `-0.0054`, the beta change is about `+0.0867`, and the implied post-event beta is about `+0.0813`.
- The beta-change p-value is about `0.023`, but the post-event estimate is based on only 32 observations.

| Test | Beta | HAC p-value | N |
|---|---:|---:|---:|
| Recent 5Y, 60D forward return | -0.0389 | 0.0002 | 1,142 |
| Recent 5Y, 90D forward return | -0.0409 | 0.0044 | 1,112 |
| Regime test: pre-event 5D beta | -0.0054 | 0.0131 | 1,165 |
| Regime test: beta change | +0.0867 | 0.0228 | 32 post-event observations |

These findings are best read as preliminary evidence that the association between ENSO and planting-sector returns varies over time. They do not establish event causality, a tradable strategy, or out-of-sample predictability.

## Repository structure

```text
enso-a-share-planting-factor/
├── README.md
├── requirements.txt
├── .gitignore
├── data/
│   ├── README.md
│   └── nino34_daily.csv
├── notebooks/
│   └── ENSO_Planting_Regime_Analysis.ipynb
└── results/
    ├── README.md
    └── tables/
```

## Reproduce the analysis

Python 3.11 or later is recommended.

```bash
python -m venv .venv
source .venv/bin/activate
python -m pip install -r requirements.txt
jupyter lab
```

Open `notebooks/ENSO_Planting_Regime_Analysis.ipynb` and run all cells. The notebook reads the local climate CSV, downloads the current 801016 history through AKShare, and writes summary tables to `results/tables/`.

Because the market series is downloaded live, a later run can differ from the committed outputs if the upstream provider revises its history. For a fully frozen replication, archive the exact market-data snapshot and document its redistribution terms.

## Data provenance

- NOAA/NCEI, [Daily Optimum Interpolation Sea Surface Temperature (OISST), Version 2.1](https://psl.noaa.gov/rest/data.noaa.oisst.v2.highres.html)
- AKShare, [`index_hist_sw` documentation](https://akshare.akfamily.xyz/data/index/index.html)
- WMO, [El Niño is forecast to intensify — 3 July 2026](https://public.wmo.int/news/media-centre/el-nino-forecast-intensify-increasing-likelihood-of-extreme-weather)

## Limitations

- The post-break sample is short and statistical power is limited.
- The breakpoint was selected from a real-world information date, but the regression does not prove that the announcement caused the change.
- Overlapping forward returns create strong serial dependence; HAC errors reduce, but do not eliminate, model risk.
- Commodity prices, macro conditions, policy news, seasonality, and other omitted variables may drive the estimated relationship.
- Statistical significance in-sample is not evidence of a profitable strategy after costs.

## 中文简介

本项目研究 Daily Niño 3.4 海温异常是否与申万种植业指数（801016）的未来收益相关，并检验这种敏感度是否会随市场状态变化。

核心结论应谨慎表述为：**现有样本支持 ENSO 与 A 股种植业收益之间存在状态依赖型关系的初步证据**。这不代表 ENSO 在所有时期都能稳定预测收益，也不证明 2026 年 7 月 3 日的 WMO 信息发布导致了 Beta 改变。事件后样本仅有 32 个观测值，因此结果仍需更长样本、控制变量与样本外检验验证。

## Disclaimer

This repository is for research and educational purposes only. It is not investment advice.
