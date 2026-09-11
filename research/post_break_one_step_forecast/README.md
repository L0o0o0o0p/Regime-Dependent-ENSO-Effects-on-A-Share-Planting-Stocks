# 2026-07-03 后 ENSO 单因子一步预测
# Post-2026-07-03 ENSO Single-Factor One-Step-Ahead Forecast

## 中文简介

本子项目延续主仓库的状态依赖研究。在外部 ENSO 信息事件事先确定 2026-07-03 为候选断点、且已有 beta-shift 检验确认断点前后定价 beta 不同的前提下，本分析只使用断点后的数据估计主预测模型，不使用 2026-07-03 之前的数据训练。

主回归为：

```math
R_{\mathrm{plant},t+1}
=
\alpha
+
\beta\,\mathrm{Nino34}_{t}
+
\varepsilon_{t+1}
```

其中，因变量是申万种植业指数（801016）下一交易日收益率，自变量是从 NOAA OISST v2.1 日度海温异常构造的 Daily Niño 3.4。Niño 3.4 区域为 5°S–5°N、170°W–120°W，并使用 `cos(latitude)` 进行面积加权。

### 信息时点与日期对齐

日期为 `d` 的 OISST 数据被视为在北京时间 `d+1` 日 17:00 后可得，因此只能预测该时点之后的第一个 A 股交易日。例如：

`2026-09-08 ENSO → 2026-09-09 17:00 后可得 → 预测 2026-09-10 收益率`

代码不再对 ENSO 序列额外机械执行 `.shift(1)`，而是显式建立 `Factor_Date → Known_After_Date → Target_Trade_Date` 映射，避免重复滞后并确保 `t+1` 表示下一可交易日。

### 方法和当前结果

- Post-break 全样本回归：49 个有效交易日。
- Expanding-window 初始窗口：20 个交易日；pseudo-OOS 预测：29 次。
- ENSO 模型 OOS RMSE 为 0.03539，MAE 为 0.02703，方向准确率为 62.07%。
- 相对 zero-return benchmark 的 OOS \(R^2\) 为 1.31%，但 DM 检验不显著（双侧 `p=0.914`）。
- 相对 expanding historical-mean benchmark 的 OOS \(R^2\) 为 -1.49%，且 DM 与 Clark–West 检验均不显著。
- ENSO 模型与历史均值基准的逐日预测方向完全相同，当前没有方向预测增量。

当前样本很短，结果应视为断点后新定价状态下的探索性证据。历史日期优先使用当前 final 文件、近期使用 preliminary 文件，因此这是使用当前最佳 OISST 序列的回溯分析，不是严格保存历史 vintage 的实时交易回测。

### 文件

- `ENSO_Post_Break_One_Step_Forecast.ipynb`：含详细中文注释的完整下载、构造、对齐、回归和 OOS 预测代码。
- `results/ENSO_Post_Break_Results.xlsx`：回归、OOS 指标、预测比较检验、逐日预测、对齐数据和 ENSO 日度数据汇总。
- `results/post_break_recursive_forecast.png`：实际收益和 ENSO 一步预测图。

运行 Notebook 前请在本目录启动 Jupyter。结果会自动写入本目录下的 `results/`。

## English Introduction

This subproject extends the repository's regime-dependent ENSO analysis. The candidate break date, 3 July 2026, was identified from an external ENSO-related information event before the forecasting exercise, and a separate beta-shift test found that the post-break pricing beta differed from the earlier regime. The primary forecasting model is therefore estimated only with post-break observations.

The predictive regression is:

$$
R_{\mathrm{plant},t+1}
=
\alpha
+
\beta\,\mathrm{Nino34}_{t}
+
\varepsilon_{t+1}
$$

The dependent variable is the next-trading-day return of the Shenwan Planting Industry Index (801016). The predictor is a Daily Niño 3.4 series constructed from NOAA OISST v2.1 daily SST anomalies over 5°S–5°N and 170°W–120°W, using `cos(latitude)` area weights.

### Information timing and alignment

The OISST observation dated `d` is assumed to become available after 17:00 Beijing time on `d+1`. It is therefore mapped to the first A-share trading day after that availability time. For example:

`2026-09-08 ENSO → available after 17:00 on 2026-09-09 → forecasts the 2026-09-10 return`

The code does not apply an additional mechanical `.shift(1)`. Instead, it explicitly maps `Factor_Date → Known_After_Date → Target_Trade_Date`, preventing a duplicate lag and respecting the trading calendar.

### Method and current findings

- The post-break full-sample regression contains 49 aligned trading-day observations.
- The expanding-window forecast uses 20 initial observations and produces 29 pseudo-OOS forecasts.
- The ENSO model has an OOS RMSE of 0.03539, MAE of 0.02703, and directional accuracy of 62.07%.
- Its OOS \(R^2\) versus the zero-return benchmark is 1.31%, but the DM comparison is not statistically significant (`p=0.914`).
- Its OOS \(R^2\) versus the expanding historical-mean benchmark is -1.49%; neither the DM nor Clark–West comparison is statistically significant.
- The ENSO and expanding-mean forecasts have the same direction on every OOS date, so the ENSO factor currently adds no directional forecast improvement.

The sample is short, so these results are exploratory evidence for the post-break pricing regime. Historical dates primarily use currently available final OISST files, with preliminary files for recent dates. This is a retrospective analysis using the current best OISST series, not a strict real-time backtest with archived historical vintages.

### Contents

- `ENSO_Post_Break_One_Step_Forecast.ipynb`: fully documented code for downloading data, constructing the factor, aligning dates, estimating the regression, and generating recursive forecasts.
- `results/ENSO_Post_Break_Results.xlsx`: consolidated regression, OOS metrics, comparison tests, daily forecasts, aligned observations, and Daily Niño 3.4 data.
- `results/post_break_recursive_forecast.png`: realized returns and ENSO one-step-ahead forecasts.

Start Jupyter from this directory before running the Notebook. Generated files will be written to the local `results/` folder.
