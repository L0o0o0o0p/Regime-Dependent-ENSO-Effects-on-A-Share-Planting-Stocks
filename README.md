# ENSO 对 A 股种植业股票收益的状态依赖型影响  
# Regime-Dependent ENSO Effects on A-Share Planting Stocks

---

# 中文版

## 项目简介

本项目研究 ENSO（厄尔尼诺—南方涛动）是否包含能够解释或预测 A 股农业板块股票收益的信息，并进一步考察这种关系是否会随着市场环境的变化而发生改变。

农业行业与气候条件存在天然联系。ENSO 可以通过改变全球温度、降水、干旱和洪涝分布，进一步影响农作物产量、农业商品供给以及农产品价格。然而，从气候冲击到股票收益之间并不存在简单的一对一关系。

股票市场交易的并不是气候变量本身，而是投资者在当时政治、经济、商品价格、政策和市场情绪等综合环境下，对气候冲击未来经济影响的预期。

因此，本项目并不预设：

> ENSO 上升一定导致农业股票上涨或下跌。

相反，本项目重点研究：

> **ENSO 对农业股票的影响是否具有行业差异，以及这种影响是否具有明显的状态依赖性。**

---

## 研究问题

本项目主要回答三个问题：

1. **ENSO 是否与 A 股农业板块未来收益存在稳定关系？**  
   首先使用申万农林牧渔一级指数作为研究对象，检验 Daily Niño 3.4 SST Anomaly 与不同期限未来收益之间的关系。

2. **ENSO 的影响是否集中在气候暴露更直接的农业子行业？**  
   由于农林牧渔一级指数包含种植业、养殖业、饲料、渔业等不同商业模式，而 ENSO 对这些行业的影响路径甚至可能方向相反，因此进一步将研究对象缩小至申万种植业指数。

3. **ENSO 的 Beta 是否会随着市场环境发生变化？**  
   如果 ENSO 对股票收益的影响并非长期固定，那么使用一个长期样本估计单一 Beta 可能掩盖真实关系。因此，本项目进一步利用事件窗口和交互项模型检验 ENSO Beta 是否存在结构变化。

---

## 数据

### 股票市场数据

研究对象主要为：

- 申万农林牧渔一级行业指数：`801010`
- 申万种植业二级行业指数：`801016`

股票价格数据通过 AKShare 获取，并构造未来收益率：

> **FwdRₜ,ₕ = Pₜ₊ₕ / Pₜ-1**

主要研究 1、5、20、60 和 90 个交易日的未来累计收益。

### ENSO 数据

气候数据来自 NOAA OISST v2.1 日度海表温度异常数据。

本项目根据 NOAA 定义的 Niño 3.4 区域：

> **5° S-5° N, 170° W-120° W**

对该区域的日度 SST anomaly 进行空间平均，从而自行构建：

**OISST-based Daily Niño 3.4 SST Anomaly**

需要强调的是，这不是 NOAA 官方发布的标准月度 Niño 3.4 Index，而是根据 NOAA 日度 OISST 数据构造的高频 ENSO 指标。

为了降低前视偏差，回归中使用：

> **Nino34ₜ₋₁**

即前一日的 Daily Niño 3.4 anomaly。

---

## 基础回归方法

基本模型为：

> **FwdRₜ,ₕ = α + β Nino34ₜ₋₁ + εₜ**

由于 5D、20D、60D、90D 等未来收益存在明显的 overlapping returns，例如相邻两个 60D 收益有 59 个交易日重叠，因此普通 OLS 标准误并不可靠。

项目使用 **HAC / Newey-West robust standard errors** 修正异方差和序列相关问题。

本项目重点关注：

- Beta
- HAC standard error
- z-statistic
- p-value
- R²

---

## 第一阶段发现：整个农业板块没有稳定 ENSO 信号

首先在申万农林牧渔一级指数上检验 ENSO。

结果显示，无论使用连续 Daily Niño 3.4、传统 ONI，还是不同预测期限，ENSO 与整个农业板块未来收益之间都没有表现出稳定显著的关系。

这一结果提示：

> 将整个农林牧渔行业作为统一资产进行分析，可能掩盖了不同子行业之间截然不同的 ENSO 暴露。

例如，当 ENSO 影响粮食价格时：

> **粮价上涨 → 种植企业收入可能改善**

但同时：

> **玉米/豆粕上涨 → 养殖企业饲料成本增加**

因此，不同子行业的影响可能在一级指数内部相互抵消。

---

## 第二阶段发现：ENSO 信号主要集中在种植业

进一步将因变量替换为申万种植业指数之后，结果发生明显变化。

在最近五年的样本中：

> **β₆₀D = -0.0359**

对应：

> **p=0.000426**

90D 结果为：

> **β₉₀D = -0.0409**

对应：

> **p=0.003929**

60D 和 90D 均表现出显著负向关系。

这意味着，在这一时期内，较高的 Daily Niño 3.4 anomaly 与之后约 60–90 个交易日较低的种植业累计收益存在显著统计关系。

与此同时：

- 1D：不显著
- 5D：不显著
- 20D：不显著

这说明 ENSO 与种植业之间的关系并不像一个即时交易信号，而更可能通过较长的农业经济传导链逐渐反映到股票价格中。

可能的传导路径为：

> **ENSO → 天气条件 → 作物单产和供给预期 → 农产品价格 → 企业盈利预期 → 种植业股票**

---

## 第三阶段发现：这种关系并不具有长期稳定性

当样本扩展到更长的历史时期后，前述显著关系并没有保持稳定。

例如，最近五年存在显著关系，而十五年样本并没有得到同样结果。

因此，本项目不能得出：

> ENSO 是一个长期稳定的 A 股种植业预测因子。

相反，这一结果提出了新的研究假设：

> **ENSO 的 Beta 可能随市场状态变化**

也就是说，ENSO 可能不是一个具有固定 Beta 的静态因子，而是一个：

**Regime-Dependent Factor**

---

## 第四阶段：近期市场状态出现明显变化

为了进一步研究这种状态依赖性，项目选择外部事件作为市场状态节点，而不是根据回归 p-value 倒推出最佳日期。

其中重点研究的状态切换节点为：

**2026 年 7 月 3 日**

之所以选择这一日期，是因为世界气象组织（WMO）在当日确认热带太平洋已形成 El Niño 条件，并预计其将在随后数月快速增强，同时明确提示农业等气候敏感行业需要关注潜在影响。

因此，该日期来自外部、公开且事先存在的 ENSO 信息冲击，而不是根据回归结果或 p-value 反向筛选得到，可以作为相对客观的市场状态划分节点。

需要强调的是，这一日期仅用于定义事件前后的市场状态，并不意味着 WMO 公告本身被证明导致 ENSO Beta 发生变化。

随后只使用事件发生后的市场数据进行 5D forward-return 回归。

在修正窗口外信息泄漏以后，2026 年 7 月 3 日至研究截止日期的有效样本为 32 个交易日。

结果为：

> **β=0.0813**

> **p=0.0325**

> **R²=23.15%**

这一时期 ENSO Beta 与此前五年样本中的负向中期 Beta 出现明显差异，并表现为显著正向的短期关系。

但需要强调：

> 32 个有效观测仍然是较短样本，因此这一结果本身不足以证明 ENSO 已经成为稳定的正向短期预测因子。

它更重要的作用是提出：

> **ENSO 的市场定价机制可能已经发生改变。**

---

## 正式 Beta 变化检验

为了避免仅通过比较两个独立回归判断 Beta 是否改变，本项目进一步构建交互项模型：

> **Fwd5Dₜ = α + β₁ Nino34ₜ₋₁ + β₂ Postₜ + β₃ ( Nino34ₜ₋₁ × Postₜ ) + εₜ**

其中：

> **Postₜ=0**

表示状态节点之前；

> **Postₜ=1**

表示状态节点之后。

其中最重要的是：

> **β₃**

因为它直接检验：

> **H₀: β(pre) = β(post)**

即事件前后的 ENSO Beta 是否相同。

实证结果显示：

> **种植业 ENSO Beta 在该状态节点前后发生了 5% 显著性水平下的统计显著变化。**

因此，相比单纯说“事件后的回归显著”，这一结果提供了更正式的结构变化证据。

---

## 行业层面的进一步验证

为了验证这种状态变化是否适用于整个农业行业，本项目使用相同方法对申万农林牧渔一级指数进行检验。

结果并未发现 ENSO Beta 存在显著结构变化。

因此，目前的证据并不支持：

> ENSO 状态依赖效应已经扩展到整个 A 股农业行业。

相反，它更可能集中在：

> **种植业**

这一与天气、作物产量和农产品价格关系更直接的子行业。

---

## 核心结论

目前的实证结果支持以下解释：

> **ENSO 并不是一个能够在所有历史时期稳定解释整个 A 股农业板块收益的静态因子。**

更具体地说：

> **ENSO 的市场影响具有明显的行业异质性，其统计关系主要集中在气候暴露更直接的种植业。**

同时：

> **种植业对 ENSO 的收益敏感度并不是长期固定的。以 2026 年 7 月 3 日作为外生状态节点后，交互项检验发现 ENSO Beta 出现统计显著变化。**

因此，本项目目前最核心的发现可以概括为：

> **ENSO may be a sector-specific and regime-dependent factor**

中文可以概括为：

> **ENSO 更可能是一个具有行业选择性和状态依赖性的农业股票因子，而不是长期固定、适用于整个农业板块的统一因子。**

近期 A 股农业与种植板块的价格表现对 ENSO 相关信息、极端天气风险和粮价预期的敏感度有所上升，市场也表现出更强的气候风险再定价特征。

结合本项目中事件后 ENSO Beta 的变化以及交互项对 Beta 结构变化的显著检验，可以将这些结果视为“近期市场更敏感地交易 ENSO 相关风险”的统计支持。

但这一证据并不能证明交易者单独围绕 ENSO 进行交易，也不能据此建立 ENSO 与股票收益之间的因果关系。

---

## 这个结论不意味着什么

当前结果不能证明：

> ENSO 导致种植业股票上涨或下跌。

也不能证明：

> 2026 年 7 月 3 日的事件导致 ENSO Beta 改变。

现在能够说明的是：

> 以该日期作为外生状态划分节点时，事件前后的 ENSO Beta 存在显著统计差异。

因此，本研究识别的是：

> **association + structural change**

而不是：

> **causality**

---

## 当前项目的最终定位

本项目不应被简单概括为：

> “用 ENSO 预测 A 股农业股票。”

更准确的定位是：

> **本项目利用 NOAA 日度海温异常数据构造高频 Daily Niño 3.4 气候因子，并通过 HAC 回归、行业拆分、事件窗口和交互项结构变化检验，研究 ENSO 在 A 股种植业股票中的状态依赖型定价关系。**

---

# English Version

## Project Overview

This project investigates whether ENSO (El Niño–Southern Oscillation) contains information that can help explain or predict returns in China’s A-share agricultural sector, and whether this relationship changes across market regimes.

Agriculture is naturally exposed to climate conditions. ENSO can alter global temperature, precipitation, drought, and flood patterns, which can in turn affect crop yields, agricultural supply, and commodity prices. However, the transmission from climate shocks to equity returns is not a simple one-to-one relationship.

Equity markets do not trade climate variables in isolation. Investors price the expected economic consequences of climate shocks within the broader political, macroeconomic, commodity-price, policy, and market-sentiment environment.

Therefore, this project does not assume that:

> A stronger ENSO signal must mechanically lead agricultural stocks to rise or fall.

Instead, the project focuses on whether:

> **ENSO effects differ across agricultural subsectors and whether those effects are regime-dependent.**

---

## Research Questions

The project addresses three main questions:

1. **Is ENSO stably related to future returns in the broader A-share agricultural sector?**  
   The analysis first uses the Shenwan Agriculture, Forestry, Animal Husbandry and Fishery Index and tests the relationship between the Daily Niño 3.4 SST Anomaly and forward returns over multiple horizons.

2. **Is the ENSO effect concentrated in agricultural subsectors with more direct climate exposure?**  
   The broad agriculture index combines planting, livestock breeding, feed, fisheries, and other business models. Because ENSO may affect these industries through different and even opposing channels, the analysis then narrows the dependent variable to the Shenwan Planting Industry Index.

3. **Does the ENSO beta change with the market environment?**  
   If the effect of ENSO on equity returns is not time-invariant, estimating a single beta over a long sample may mask the underlying relationship. The project therefore uses event windows and an interaction model to formally test for structural changes in the ENSO beta.

---

## Data

### Equity Market Data

The main market indices are:

- Shenwan Agriculture, Forestry, Animal Husbandry and Fishery Index: `801010`
- Shenwan Planting Industry Index: `801016`

Equity price data are obtained through AKShare. Forward returns are defined as:

> **FwdRₜ,ₕ = Pₜ₊ₕ / Pₜ-1**

The main horizons are 1, 5, 20, 60, and 90 trading days.

### ENSO Data

Climate data come from NOAA OISST v2.1 daily sea-surface-temperature anomaly data.

Using NOAA’s Niño 3.4 region definition:

> **5° S-5° N, 170° W-120° W**

the project spatially averages daily SST anomalies over the region to construct an:

**OISST-based Daily Niño 3.4 SST Anomaly**

This series is not the official NOAA monthly Niño 3.4 Index. It is a higher-frequency ENSO indicator constructed from NOAA daily OISST data.

To reduce look-ahead bias, the regressions use:

> **Nino34ₜ₋₁**

that is, the previous day’s Daily Niño 3.4 anomaly.

---

## Baseline Regression

The baseline specification is:

> **FwdRₜ,ₕ = α + β Nino34ₜ₋₁ + εₜ**

Multi-day forward returns create substantial overlap. For example, two adjacent 60-day forward returns share 59 trading days. As a result, conventional OLS standard errors are not reliable.

The project therefore uses **HAC / Newey-West robust standard errors** to account for heteroskedasticity and serial correlation.

The main statistics of interest are:

- Beta
- HAC standard error
- z-statistic
- p-value
- R²

---

## Stage 1 Finding: No Stable ENSO Signal in the Broad Agriculture Index

The analysis first tests ENSO against the broad Shenwan agriculture index.

Across continuous Daily Niño 3.4 measures, conventional ONI measures, and multiple forecasting horizons, the relationship between ENSO and future returns of the broad agriculture index is not consistently significant.

This suggests that:

> Treating the entire agriculture sector as a single asset may obscure materially different ENSO exposures across subsectors.

For example, if ENSO contributes to higher grain prices:

> **Higher grain prices → potentially stronger planting-company revenues**

while at the same time:

> **Higher corn/soymeal prices → higher feed costs for livestock producers**

These opposing channels may partially offset one another within the broad industry index.

---

## Stage 2 Finding: The ENSO Signal Is Concentrated in the Planting Sector

When the dependent variable is changed to the Shenwan Planting Industry Index, the results change materially.

In the recent five-year sample:

> **β₆₀D = -0.0359**

with:

> **p=0.000426**

For the 90-day horizon:

> **β₉₀D = -0.0409**

with:

> **p=0.003929**

Both the 60-day and 90-day coefficients are significantly negative.

This means that, within this sample, higher Daily Niño 3.4 anomalies are statistically associated with lower cumulative planting-sector returns over the following 60–90 trading days.

At the same time:

- 1D: not significant
- 5D: not significant
- 20D: not significant

This suggests that the ENSO relationship does not behave like an immediate trading signal. Instead, the effect may be transmitted gradually through the agricultural economic chain.

A possible mechanism is:

> **ENSO → Weather Conditions → Crop Yield and Supply Expectations → Agricultural Commodity Prices → Corporate Earnings Expectations → Planting Stocks**

---

## Stage 3 Finding: The Relationship Is Not Stable Over Long Samples

When the sample is extended further back in time, the previously significant relationship does not remain stable.

For example, the relationship is significant in the recent five-year sample but is not replicated in the 15-year sample.

Therefore, the project does not conclude that:

> ENSO is a stable long-run predictor of A-share planting-sector returns.

Instead, the evidence motivates a different hypothesis:

> **The ENSO beta may vary across market regimes**

In other words, ENSO may be better understood as a:

**Regime-Dependent Factor**

rather than a static factor with a constant beta through time.

---

## Stage 4: Evidence of a Recent Regime Shift

To investigate regime dependence, the project uses externally defined events as market-state breakpoints rather than selecting dates based on regression p-values.

The main breakpoint is:

**3 July 2026**

This date is chosen because the World Meteorological Organization (WMO) confirmed El Niño conditions in the tropical Pacific and expected the event to strengthen rapidly over the following months, while highlighting potential implications for climate-sensitive sectors such as agriculture.

The breakpoint is therefore based on an external, public, and pre-existing ENSO information event rather than being selected ex post from the regression results.

Importantly, the date is used only to define pre- and post-event market regimes. The analysis does not claim that the WMO announcement itself caused the ENSO beta to change.

The project then runs a 5-day forward-return regression using only post-event data.

After correcting for window leakage, the effective post-event sample contains 32 observations.

The estimated results are:

> **β=0.0813**

> **p=0.0325**

> **R²=23.15%**

The post-event ENSO beta differs substantially from the negative medium-horizon beta found in the preceding five-year sample and becomes significantly positive at the short horizon.

However:

> A sample of 32 effective observations is still small, so this result alone is not sufficient to establish ENSO as a stable positive short-term forecasting factor.

Its main value is that it motivates the hypothesis that:

> **The market pricing mechanism of ENSO may have changed.**

---

## Formal Beta-Shift Test

To avoid relying only on comparisons across separate regressions, the project estimates an interaction model:

> **Fwd5Dₜ = α + β₁ Nino34ₜ₋₁ + β₂ Postₜ + β₃ ( Nino34ₜ₋₁ × Postₜ ) + εₜ**

where:

> **Postₜ=0**

before the regime breakpoint, and:

> **Postₜ=1**

after the breakpoint.

The key coefficient is:

> **β₃**

because it directly tests:

> **H₀: β(pre) = β(post)**

That is, whether the ENSO beta is unchanged across the two regimes.

The empirical result shows that:

> **The ENSO beta for planting-sector returns changes significantly across the breakpoint at the 5% significance level.**

This provides stronger evidence of a structural change than simply observing that the post-event regression is significant.

---

## Sector-Level Robustness Check

To test whether the regime shift applies to the entire agricultural sector, the same interaction framework is applied to the broad Shenwan Agriculture, Forestry, Animal Husbandry and Fishery Index.

The broader index does not exhibit a statistically significant ENSO beta shift.

Therefore, the current evidence does not support the conclusion that regime-dependent ENSO pricing extends across the entire A-share agricultural sector.

Instead, the effect appears to be concentrated in:

> **The Planting Sector**

which is more directly exposed to weather conditions, crop yields, and agricultural commodity prices.

---

## Core Conclusion

The empirical evidence supports the following interpretation:

> **ENSO is not a static factor that consistently explains returns across the entire A-share agricultural sector and across all historical periods.**

More specifically:

> **ENSO effects are heterogeneous across subsectors, with the strongest statistical relationship concentrated in the planting sector.**

At the same time:

> **The sensitivity of planting-sector returns to ENSO is not constant through time. Using 3 July 2026 as an externally defined regime breakpoint, the interaction test identifies a statistically significant change in the ENSO beta.**

The main finding can therefore be summarized as:

> **ENSO may be a sector-specific and regime-dependent factor**

Recent A-share agriculture and planting-stock performance also appears to have become more sensitive to ENSO-related information, extreme-weather risk, and grain-price expectations, indicating stronger climate-risk repricing.

Together with the post-event beta shift and the statistically significant interaction term, the results provide support for the interpretation that recent market pricing has become more sensitive to ENSO-related risks.

However, this evidence does not prove that investors are trading ENSO in isolation, nor does it establish a causal relationship between ENSO and equity returns.

---

## What the Results Do Not Establish

The current evidence does not prove that:

> ENSO causes planting stocks to rise or fall.

Nor does it prove that:

> The 3 July 2026 event caused the ENSO beta to change.

What the analysis shows is that:

> When 3 July 2026 is used as an externally defined regime breakpoint, the ENSO beta differs significantly between the pre- and post-event periods.

Therefore, the project identifies:

> **association + structural change**

rather than:

> **causality**

---

## Final Project Positioning

This project should not be summarized simply as:

> “Using ENSO to predict A-share agricultural stocks.”

A more accurate description is:

> **This project constructs a high-frequency Daily Niño 3.4 climate factor from NOAA daily SST anomaly data and uses HAC regressions, sector decomposition, event-window analysis, and interaction-based structural-break tests to study regime-dependent ENSO pricing in A-share planting stocks.**
