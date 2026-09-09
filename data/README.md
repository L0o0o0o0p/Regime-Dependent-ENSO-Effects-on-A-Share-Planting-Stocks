# Data notes

## `nino34_daily.csv`

The file contains an OISST-based daily Niño 3.4 sea-surface-temperature anomaly series used by the analysis notebook.

| Column | Meaning |
|---|---|
| `Date` | Calendar date |
| `Nino34_Daily` | Daily SST anomaly spatially averaged over the Niño 3.4 region (5°S–5°N, 170°W–120°W) |

The notebook creates `Nino34_Lag1D` by shifting this series by one calendar day before merging it with A-share trading dates.

## Provenance and interpretation

- Source grid: NOAA/NCEI [OISST v2.1](https://psl.noaa.gov/rest/data.noaa.oisst.v2.highres.html).
- This is a researcher-constructed daily series, not NOAA's official monthly Oceanic Niño Index (ONI).
- Units are degrees Celsius relative to the climatology used by the source anomaly field.
- Recent OISST observations may be revised by the upstream provider.

The Shenwan Planting Industry Index series is not committed to this repository. It is downloaded when the notebook runs through AKShare's [`index_hist_sw`](https://akshare.akfamily.xyz/data/index/index.html) interface. Users are responsible for reviewing the upstream provider's terms before redistributing a frozen market-data snapshot.

## 中文说明

`nino34_daily.csv` 是基于 NOAA OISST v2.1 网格数据，在 Niño 3.4 区域进行空间平均后构造的日频海温异常序列，并非 NOAA 官方发布的月频 ONI。Notebook 会将该变量滞后一个自然日，再与 A 股交易日精确匹配。
