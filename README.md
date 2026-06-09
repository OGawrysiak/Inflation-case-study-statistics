# Household Responses to Inflation
### EU Consumer Expectations Survey, 2024 — Statistics Case Study 6

> **How much do households actually notice rising prices, and do their perceptions match official statistics?**

This repository contains the full statistical analysis, Python notebook, and written report for Case Study 6.
We use individual-level survey microdata from four EU countries to test whether households overestimate inflation,
whether lower-income groups feel price increases more acutely, and whether past experience shapes future expectations.

---

## Table of Contents

1. [Background and Motivation](#1-background-and-motivation)
2. [Dataset](#2-dataset)
3. [How to Run](#4-how-to-run)
4. [Analysis Overview](#5-analysis-overview)
   - [Descriptive Findings](#51-descriptive-findings)
   - [H1 — Income and Inflation](#52-h1--income-and-inflation)
   - [H2 — Expectation Anchoring](#53-h2--expectation-anchoring)
   - [H3 — Age Effect](#54-h3--age-effect)
   - [Cross-Country Differences](#55-cross-country-differences)
   - [Consequences of Perceived Inflation](#56-consequences-of-perceived-inflation)
5. [Key Findings at a Glance](#6-key-findings-at-a-glance)
6. [Connection to the Literature](#7-connection-to-the-literature)
7. [Statistical Methods](#8-statistical-methods)
8. [Limitations](#9-limitations)

---

## 1. Background and Motivation

The 2022–2024 inflation surge was one of the most widely felt economic episodes in recent EU history.
Yet research consistently shows a gap between what households *feel* and what price indices *measure*.
Four media and academic sources motivate this case study:

| Source | Core claim |
|---|---|
| *"Falling inflation still doesn't feel real for households"* | Even as headline inflation fell, households continued to report high perceived price increases — perception lagged reality |
| *"Latest ECB consumer expectations survey shows surging inflation expectations"* | Household expectations of future inflation frequently exceed official forecasts, with significant cross-country heterogeneity |
| *"Household perceptions and expectations in the wake of the inflation surge"* | The inflationary surge created persistent shifts in household expectations that outlasted the actual price shock |
| *"People's understanding of inflation"* | Consumers systematically overestimate inflation, with lower-income households reporting higher perceived price increases due to differential exposure to food and energy prices |

Our analysis directly tests the patterns described in these sources using microdata from 2,000 EU households.

---

## 2. Dataset

**Source:** EU Consumer Expectations Survey, 2024  
**Observations:** 2,000 respondents  
**Countries:** Germany (DE, n=500) · Greece (EL, n=500) · Spain (ES, n=500) · Portugal (PT, n=500)

| Variable | Type | Description |
|---|---|---|
| `price_past` | Ordinal (1–5) | Perceived price change in past 12 months |
| `price_exp` | Ordinal (1–5) | Expected price change in next 12 months |
| `sit_last_12months` | Ordinal (1–5) | Self-reported financial situation change (past) |
| `sit_next_12months` | Ordinal (1–5) | Expected financial situation change (future) |
| `reduced_consumption` | Binary | Whether household reduced spending |
| `increased_income` | Binary | Whether household increased income |
| `increased_spending` | Binary | Whether household increased spending on durables |
| `income` | Ordinal (5 levels) | Income quintile (1st = lowest) |
| `age_group` | Ordinal (4 levels) | 18–34, 35–49, 50–70, 71 and older |
| `country_code` | Nominal | DE, EL, ES, PT |

> **Note:** The open-ended numeric variables (`price_past_open`, `price_exp_open`) were not available in this extract. All analysis uses the five-point ordinal scales.

---

## 4. How to Run

**Prerequisites:** Python 3.9+, VS Code with the Jupyter extension

```bash
# Install required libraries
pip install pandas numpy matplotlib seaborn scipy

# Open the notebook in VS Code
code case_study_6.ipynb
```

Run cells **in order** from top to bottom. The first three cells (imports, data load, encodings)
must be run before any figure cell.

| Cell | Content |
|---|---|
| Cell 1 | Imports + global style settings + `ALPHA = 0.05` |
| Cell 2 | Load CSV, sanity checks |
| Cell 3 | Ordinal encodings, shared colour palette |
| Figure 1 | Distribution of perceptions and expectations |
| Figure 2 | Coping strategy adoption and co-occurrence |
| Figure 3 | Income vs perceptions (Spearman + Kruskal-Wallis + Dunn) |
| Figure 4 | Cross-country comparison (Kruskal-Wallis) |
| Figure 5 | Consumption reduction + financial situation (chi-square + Spearman) |
| Figure 6 | Expectation anchoring + age effect (Spearman) |
| Export | Optional PNG export at 300 dpi |

---

## 5. Analysis Overview

### 5.1 Descriptive Findings

> *Linked to: "Falling inflation still doesn't feel real for households"*

**88.2%** of respondents reported prices rising in the past 12 months.
**81.5%** expect prices to continue rising over the next year.
The most extreme category ("prices went up a lot") is by far the modal response in both variables,
consistent with the well-documented consumer tendency to overestimate inflation.

Household adaptation was widespread:

| Strategy | Adoption rate |
|---|---|
| Reduced consumption (cheaper or fewer goods) | **54.2%** |
| Increased income (raise or second job) | **36.0%** |
| Increased spending on durables | **8.4%** |
| Used **two** strategies simultaneously | **26.2%** |
| Used **all three** simultaneously | **2.4%** |

The high rate of multi-strategy adaptation confirms the ECB survey finding that inflation
triggered broad-based behavioural adjustment, not merely a single coping response.

---

### 5.2 H1 — Income and Inflation

> *Linked to: "People's understanding of inflation" and "Household perceptions and expectations in the wake of the inflation surge"*

**Hypotheses:**
- H₀: No monotonic association between income quintile and perceived/expected inflation (ρ = 0)
- H₁: Lower-income households report higher perceived and expected inflation (ρ < 0)

**Tests run:** Spearman rank correlation · Kruskal-Wallis · Dunn post-hoc (Bonferroni corrected)

| Test | Statistic | p-value | Decision (α = 0.05) |
|---|---|---|---|
| Spearman ρ — past perception | ρ = −0.070 | p = .002 | **Reject H₀** |
| Spearman ρ — future expectation | ρ = −0.076 | p < .001 | **Reject H₀** |
| Kruskal-Wallis — past | H = 14.21 | p = .007 | **Reject H₀** |
| Kruskal-Wallis — future | H = 16.83 | p = .002 | **Reject H₀** |

**✓ H1 confirmed.** Both correlations are negative and significant. Dunn post-hoc tests show
the 1st vs 5th quintile contrast is the most consistently significant after Bonferroni correction.
Effect sizes are modest (ρ ≈ −0.07) — income alone explains roughly 0.5% of variance in perception,
suggesting country of residence and individual experience are stronger drivers.

**Why does this happen?** Lower-income households allocate a larger share of their budget to food and energy —
the categories that experienced the sharpest price increases in 2022–2024.
Their higher perceived inflation likely reflects genuine differential exposure, not mere misperception.
This is precisely the pattern documented in *"People's understanding of inflation"*.

---

### 5.3 H2 — Expectation Anchoring

> *Linked to: "Latest ECB consumer expectations survey shows surging inflation expectations"*

**Hypotheses:**
- H₀: No monotonic association between past price perceptions and future expectations (ρ = 0)
- H₁: Households who perceived higher past inflation also expect higher future inflation (ρ > 0)  
*(one-tailed — direction predicted by theory)*

**Test run:** Spearman rank correlation

| Test | Statistic | p-value | Decision (α = 0.05) |
|---|---|---|---|
| Spearman ρ — past vs future | ρ = +0.518 | p < .001 | **Reject H₀** |

**✓ H2 confirmed.** This is the strongest finding in the study.
Households who felt prices rose a lot in the past overwhelmingly expect them to keep rising —
the heatmap diagonal is the darkest band.

This phenomenon, known as **expectation anchoring**, has direct monetary policy implications:
if lived experience locks in pessimistic expectations, households may demand higher wages or cut spending
even after actual inflation has subsided — creating self-fulfilling inflationary pressure.
The ECB monitors this effect closely through its Consumer Expectations Survey, which is precisely
the data source used here.

---

### 5.4 H3 — Age Effect

> *Linked to: "People's understanding of inflation"*

**Hypotheses:**
- H₀: No monotonic association between age group and perceived/expected inflation (ρ = 0)
- H₁: Older respondents report higher perceived and expected inflation (ρ ≠ 0)  
*(two-tailed — direction uncertain)*

**Test run:** Spearman rank correlation

| Test | Statistic | p-value | Decision (α = 0.05) |
|---|---|---|---|
| Spearman ρ — age vs past | ρ = −0.004 | p = .874 | **Fail to reject H₀** |
| Spearman ρ — age vs future | ρ = −0.034 | p = .133 | **Fail to reject H₀** |

**✗ H3 not supported.** Mean ordinal scores are essentially flat across age groups (range: 4.25–4.59).
The hypothesis that younger respondents report lower inflation perception is not confirmed in this sample.

This null result is itself informative. One explanation is that the 2022–2024 inflationary surge was so
broad and salient that it generated uniformly high perceptions across all age cohorts, suppressing the
age gradient found in earlier research. The literature finding that younger respondents report lower
inflation may be an artefact of low-inflation periods, not a structural feature of perception.

---

### 5.5 Cross-Country Differences

> *Linked to: "Latest ECB consumer expectations survey shows surging inflation expectations"*

**Test run:** Kruskal-Wallis
- H₀: The distribution of price perceptions is identical across all four countries
- H₁: At least one country differs in its distribution of price perceptions

| Test | Statistic | p-value | Decision (α = 0.05) |
|---|---|---|---|
| Kruskal-Wallis — past | H = 146.14 | p < .001 | **Reject H₀** |
| Kruskal-Wallis — future | H = 89.72 | p < .001 | **Reject H₀** |

**Country mean ordinal scores (1–5 scale):**

| Country | Past perception | Future expectation |
|---|---|---|
| 🇩🇪 Germany | 4.25 | 4.01 |
| 🇬🇷 Greece | **4.72** | **4.26** |
| 🇪🇸 Spain | 4.28 | 3.91 |
| 🇵🇹 Portugal | 4.40 | 4.01 |

Greece reports the highest perceived and expected inflation across both dimensions.
Germany reports the lowest past perceptions. These differences are not sampling noise —
95% confidence intervals show clearly non-overlapping ranges for the extreme cases (DE vs EL).
The pattern maps onto actual differences in energy and food price exposure across Southern
and Northern Europe, as documented in the ECB survey reports.

---

### 5.6 Consequences of Perceived Inflation

> *Linked to: "Household perceptions and expectations in the wake of the inflation surge"*

**Consumption reduction (chi-square test):**
- H₀: Price perception category and consumption reduction are independent
- H₁: Households with higher perceived inflation are more likely to reduce consumption

| Test | Statistic | p-value | Decision (α = 0.05) |
|---|---|---|---|
| Chi-square | χ²(4) = 65.06 | p < .001 | **Reject H₀** |
| Cramér's V | V = 0.18 | — | Small-to-moderate effect |

The share who reduced consumption rises from ~26% among those who perceived stable prices
to ~60% among those who perceived prices rising sharply.

**Financial situation (Spearman correlation):**

| Relationship | ρ | p-value | Decision (α = 0.05) |
|---|---|---|---|
| Past inflation perception → past financial situation | −0.317 | p < .001 | **Reject H₀** |
| Future inflation expectation → future financial situation | −0.342 | p < .001 | **Reject H₀** |

Both relationships are moderate and highly significant. Higher perceived (or expected) inflation
is associated with worse self-reported financial outcomes — both looking back and looking forward.
This confirms the narrative in *"Household perceptions and expectations in the wake of the inflation surge"*
that the inflationary episode caused real and lasting damage to household financial confidence.

---

## 6. Key Findings at a Glance

| Finding | Result | Linked article |
|---|---|---|
| Households overestimate inflation | 88.2% reported rising prices — extreme category dominates | *"Falling inflation still doesn't feel real"* |
| Lower income → higher perceived inflation | Spearman ρ = −0.070, p = .002 ✓ | *"People's understanding of inflation"* |
| Lower income → higher expected inflation | Spearman ρ = −0.076, p < .001 ✓ | *"People's understanding of inflation"* |
| Past perceptions predict future expectations | Spearman ρ = +0.518, p < .001 ✓ | *"ECB consumer expectations survey"* |
| No significant age effect | ρ = −0.004, p = .874 ✗ | *"People's understanding of inflation"* |
| Large cross-country differences | Kruskal-Wallis H = 146.14, p < .001 | *"ECB consumer expectations survey"* |
| Higher perceived inflation → more consumption cuts | χ²(4) = 65.06, p < .001 | *"Wake of the inflation surge"* |
| Higher perceived inflation → worse financial situation | ρ = −0.317, p < .001 | *"Wake of the inflation surge"* |

---

## 7. Connection to the Literature

### "Falling inflation still doesn't feel real for households"
Our data directly supports this headline finding. Even within the survey sample,
88.2% of respondents reported prices rising — and the modal response was the most extreme category.
This perception persists across all income groups and countries, confirming that the *feel* of
inflation lags official statistics. The ECB and national statistics offices may report falling
headline inflation, but households anchor to their lived experience of grocery and energy bills.

### "Latest ECB consumer expectations survey shows surging inflation expectations, lower growth"
Our cross-country Kruskal-Wallis results (H = 146.14, p < .001) and the strong expectation
anchoring finding (ρ = 0.518) both corroborate the ECB survey pattern. Greece's exceptionally
high expectations (mean = 4.26/5) align with ECB findings that Southern European households
maintained higher inflation expectations longer after the peak. The anchoring mechanism
we identify — where past perception directly feeds future expectation — is the
micro-level process that explains the macro-level ECB finding of persistently elevated expectations.

### "Household perceptions and expectations in the wake of the inflation surge"
Our findings on financial situation deterioration (ρ = −0.317 for past, −0.342 for future)
confirm that the inflation surge caused real damage to household financial confidence that
extended beyond the price shock itself. The 26.2% of households adopting two coping strategies
simultaneously, and the strong link between perceived inflation and consumption cuts (χ²= 65.06),
demonstrate the breadth of behavioural adjustment this article describes.

### "People's understanding of inflation"
We confirm the income gradient: lower-income quintiles systematically report higher perceived and
expected inflation (both significant at α = 0.05), consistent with their greater exposure to
food and energy price shocks. However, we do **not** find the age gradient this literature
predicts — a notable divergence worth discussing. Our null age result (p = .874) may reflect
the extraordinary salience of the 2022–2024 inflation episode, which appears to have
overwhelmed the age-based perception differences documented in lower-inflation environments.

---

## 8. Statistical Methods

All tests use **α = 0.05** as the significance threshold.
Where multiple comparisons are performed, p-values are **Bonferroni-corrected**.

| Method | When used | Why appropriate |
|---|---|---|
| **Spearman rank correlation** | Association between two ordinal variables | Does not assume normality or linearity; appropriate for ordinal scales |
| **Kruskal-Wallis test** | Comparing distributions across 3+ groups | Non-parametric alternative to one-way ANOVA; no normality assumption |
| **Dunn post-hoc test** | Pairwise comparisons after Kruskal-Wallis | Identifies which specific pairs differ; Bonferroni correction controls family-wise error rate |
| **Chi-square test of independence** | Association between two categorical variables | Tests whether row and column variables are independent; requires expected cell counts ≥ 5 (verified) |

**Why not parametric tests?**
Pearson correlation and ANOVA both assume interval-level measurement and approximate normality.
Five-point ordinal scales satisfy neither condition — equal spacing between categories cannot be assumed,
and the response distributions are heavily skewed toward the upper end. Non-parametric methods are
more defensible and produce valid inference without these assumptions.

---

## 9. Limitations

1. **No continuous price data** — The open-ended numeric variables (`price_past_open`, `price_exp_open`)
   were absent from this extract. Working with five-point ordinal categories prevents direct comparison
   with official CPI figures and limits quantitative precision.

2. **Ordinal scale assumptions** — Treating ordinal categories as equidistant (for mean comparisons in
   Figures 4 and 6) is an assumption. Rank-based methods are used where possible, but mean scores
   are reported for visual clarity in bar charts.

3. **Cross-sectional design** — Causality cannot be established. We cannot confirm that perceiving
   high inflation *caused* consumption reduction rather than pre-existing financial distress driving both.

4. **Non-random country sample** — The four countries (DE, EL, ES, PT) were pre-selected and are not
   representative of the full EU. Northern and Eastern European countries had distinct inflation trajectories.

5. **Kruskal-Wallis shape assumption** — While Kruskal-Wallis does not require normality, it assumes
   similar distribution shape across groups under H₀. Violations can affect p-value validity, though
   the test is generally robust to moderate shape differences.

---


Install all at once:
```bash
pip install pandas numpy matplotlib seaborn scipy
```

*EU Consumer Expectations Survey (2024) · Statistics Case Study 6*
