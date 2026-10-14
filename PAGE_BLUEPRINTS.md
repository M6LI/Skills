# Reference layouts and composition recipes

Normative blueprints for **which components go where**. These layouts are not literal MLflow screenshots; they adapt MLflow's neutral-charcoal, dense experiment-workbench language for quant research. Use the same source tokens/components specified in `COMPONENT_CATALOG.md`, not novel themes per page.

All diagrams illustrate 1440px desktop viewport. `Cxx` labels reference the component catalogue. Chart content must represent real data and the proper units. Do not draw synthetic curves merely to fill the page.

## Blueprint A — Backtest Explorer (default for strategy analysis)

```
┌───────────────────────────────────────────────────────────────────────────┐
│ Quant Research  | Backtests   Experiments   Signals            Help     │ 48
├───────────────┬───────────────────────────────────────────────────────────┤
│ Saved tests   │ Backtests / EUR IG mean reversion        [Run backtest]    │ 56
│ [Search...]   │ v8f1c  ·  EUR IG  ·  net of fees  ·  GBP base             │
│ ▸ Credit      ├───────────────────────────────────────────────────────────┤
│   ✓ Baseline  │ Universe [EUR IG ▾]  Period [2025-01 — 2025-06 ▾]       │ 48
│   ▸ Sweep #17 │ Benchmark [Broad index ▾]  [Gross|Net]       [Export ▾]  │
│ ▸ Rates       ├───────────────────────────────────────────────────────────┤
│               │ Performance   Risk   Trades   Configuration              │ tabs
│               ├───────────────────────────────────────────────────────────┤
│               │ Sharpe 1.28 │ Net P&L £2.4m │ MDD −3.8% │ Trades 1,241  │ metrics
│               ├───────────────────────────────────────────────────────────┤
│               │ Cumulative net P&L (£)                                    │
│               │ ┌───────────────────────────────────────────────────────┐ │
│               │ │                 wide time-series chart                │ │ 300-360
│               │ └───────────────────────────────────────────────────────┘ │
│               ├────────────────────────────┬──────────────────────────────┤
│               │ Drawdown (%)               │ Exposure (DV01)              │ 240
│               │ ┌────────────────────────┐ │ ┌──────────────────────────┐ │
│               │ │                        │ │ │                          │ │
│               │ └────────────────────────┘ │ └──────────────────────────┘ │
│               ├────────────────────────────┴──────────────────────────────┤
│               │ Trades table (sticky heading, sort, filter, inspect)     │
└───────────────┴───────────────────────────────────────────────────────────┘
```

**Composition contract:** top-level page title + immutable provenance line. Toolbar controls define one analysis scope. Tabs change content but not the selected backtest. First metric strip is compact; then **one full-width primary** decision plot; then two supporting plots; finally detailed table. Avoid a 6-card stats grid above all charts. For narrow view, keep P&L full width, stack risk/exposure vertically, fold sidebar. Do not put row-level dates that differ from the chart scope without stating why.

**Typical components:** C01, C02, C03, C04, C05, C07, C09, C19, C23, C24, C25, C21, C27.

**Decision constraints:** Gross/net is segmented only if mutually exclusive evaluation modes. Costs assumption affects all P&L/risk metrics, not just a chart title. Cumulative P&L axis displays currency. Sample period and date inclusive/exclusive conventions remain verifiable.

## Blueprint B — Model Training Experiment Browser (closest to MLflow)

```
┌────────────────────────────────────────────────────────────────────────────┐
│ Research    Backtests    Experiments    Signals                            │
├────────────────┬───────────────────────────────────────────────────────────┤
│ Experiments    │ Experiments / Execution probability                       │
│ [Search...]    │ 2025–2026 RFQ runs · 124 total                           │
│ ◉ Execution    ├───────────────────────────────────────────────────────────┤
│   • Alpha      │ [Search runs................] [Status ▾] [Sort ▾]         │
│   • Probability│ [Metric: Brier ▾]         3 selected   [Compare 3]       │
│                ├───────────────────────────────────────────────────────────┤
│                │ [Table] [Chart]  ← view switch or page tab, not both     │
│                ├───────────────────┬───────────────────────────────────────┤
│                │ Runs              │ Validation loss by step              │
│                │ □ run_019  0.139  │ ┌───────────────────────────────────┐ │
│                │ ☑ run_020  0.131  │ │  3 colour-stable line series      │ │
│                │ ☑ run_021  0.129  │ │  Train dashed, val solid          │ │
│                │ ☑ run_022  0.127  │ └───────────────────────────────────┘ │
│                │ □ run_023  0.127  ├─────────────────────┬─────────────────┤
│                │                   │ Calibration         │ ROC / PR        │
│                │                   │                     │                 │
│                ├───────────────────┴─────────────────────┴─────────────────┤
│                │ [Parameters] [Metrics] [Artifacts] [Provenance]           │
│                │           details of highlighted run, not selected set   │
└────────────────┴───────────────────────────────────────────────────────────┘
```

**Composition contract:** run list and charts coexist for comparison. A run row click opens inspector (C27) or updates details; checkbox adds/removes comparison series (C12). Filtering the list does *not* silently delete selected runs. Keep stable series colours across views. Single model **version selection** occurs in a dropdown when the page is scoped to one model; use the run list for comparisons, not a model dropdown with hidden multi-select semantics.

**Charts:** loss vs step, validation metric vs step, calibration/error distribution/feature importance depending on model. Display run status and training interval. Train/validation use separate stroke patterns; model ID colour encodes run identity.

**Typical components:** C01, C03, C04, C07 or C08 (one view switch), C12, C21, C22, C24, C27, C31.

## Blueprint C — Signal Combination Workbench

```
┌────────────────────────────────────────────────────────────────────────────┐
│ Signals / Forecast combination               [Save configuration] [Run]  │
│ Training 2024-05—2025-06 · Test 2025-07—2025-09 · USD IG · Δspread 5d  │
├────────────────────────────────────────────────────────────────────────────┤
│ Universe [USD IG ▾]  Horizon [5d ▾]  Target [Δspread bps ▾]  [Net|Gross] │
├────────────────────────────────────────────────────────────────────────────┤
│ Overview    Signals    Combination    Attribution    Diagnostics          │
├───────────────────────┬────────────────────────────────────────────────────┤
│ Included signals      │ OOS rolling IC (20 trading days)                │
│ [Search]              │ ┌──────────────────────────────────────────────┐ │
│ ☑ Curve slope 0.14    │ │ Primary model comparison line chart          │ │
│ ☑ Momentum    0.11    │ └──────────────────────────────────────────────┘ │
│ ☑ Carry       0.08    ├────────────────────────┬───────────────────────────┤
│ □ Liquidity   0.02    │ Forecast vs realised   │ Coefficient weights       │
│                       │ scatter (bps)          │ history                   │
│ [Equal|Ridge|NN ▾]    ├────────────────────────┴───────────────────────────┤
│ λ [0.025    ]         │ Comparison metrics: OOS IC, hit rate, turnover  │
│                       ├────────────────────────────────────────────────────┤
│ [Run combination]     │ Holdout breakdown by sector and rating          │
└───────────────────────┴────────────────────────────────────────────────────┘
```

**Composition contract:** left setting/filter rail 272–312px, right flexible analytical viewport. Signal inclusion uses checkboxes (multi-select); combined model uses single-select (C11). If only 3 fixed models, a compact segment C08 is acceptable if labels fit; otherwise dropdown. **Do not place the model choice as page tabs.** Parameter α/λ is numeric input (and optional log slider), with explicit expensive `Run combination`. Show training vs testing boundaries and dataset/target definition persistently.

**Quant guardrails:** no look-ahead leakage in features/labels; show horizon as `5 trading days` if appropriate, not unlabelled `5d` where ambiguous; rolling IC uses defined window and available observations; state missingness and regime segmentation. Predictions and truth same unit/time alignment.

**Typical components:** C03, C04, C07, C09, C11, C14–C16, C21, C23–C26, C34.

## Blueprint D — Curve Fitting / Nelson–Siegel Visualiser

```
┌────────────────────────────────────────────────────────────────────────────┐
│ Curve Lab / Nelson–Siegel                     [Export] [Save parameters] │
│ Bond universe 01 Oct 2026 · OAS (bps) · fitted on 73 bonds               │
├────────────────────────┬───────────────────────────────────────────────────┤
│ DATA & MODEL           │ Observed OAS vs fitted Nelson–Siegel curve       │
│ Date [2026-10-01 ▾]    │ ┌───────────────────────────────────────────────┐ │
│ Model [NS ▾]           │ │ scatter observations + fitted curve           │ │
│ Weighting [DV01 ▾]     │ │ x=Tenor (years), y=Spread (bps)               │ │
│                        │ └───────────────────────────────────────────────┘ │
│ PARAMETERS             ├───────────────────────────────────────────────────┤
│ β₀ [112.20       ]     │ Residuals by tenor (bps)                          │
│ β₁ [ -16.40      ]     │ ┌───────────────────────────────────────────────┐ │
│ β₂ [ 31.00       ]     │ │ zero reference, signed scatter                │ │
│ τ  [2.40         ]     │ └───────────────────────────────────────────────┘ │
│                        ├───────────────────────────────────────────────────┤
│ [Reset] [Refit curve]  │ Fit summary: weighted RMSE, excluded bonds      │
└────────────────────────┴───────────────────────────────────────────────────┘
```

**Composition contract:** **no forced sidebar** for a standalone tool. 288px settings rail (at 1440), main plot with scientific axes and hover tooltips. Numeric inputs exact; sliders optional. Model selection dropdown even when only NS/NSS if labels have explanations / future additions; segmented if precisely 2 fixed options and fast switching is central. Don't auto-refit on every edit unless computation is demonstrably lightweight, intentional and responsive. Show method, weights and objective in disclosure.

**Typical components:** C03, C05, C09, C14, C15, C24, C25, C26, C30.

## Blueprint E — Multiple Backtest / Model Comparison

```
┌────────────────────────────────────────────────────────────────────────────┐
│ Compare 3 strategies                          [Export comparison]        │
│ All rebased to 100 at 01 Jan 2025 · same USD IG universe                │
├────────────────────────────────────────────────────────────────────────────┤
│ Compare set: ● Equal  ● Ridge  ● XGBoost   [Add or remove models]       │
├────────────────────────────────────────────────────────────────────────────┤
│ Overview     Risk     Attribution     Configuration differences          │
├────────────────────────────────────────────────────────────────────────────┤
│       Normalised cumulative P&L (common time axis, units consistent)     │
│ ┌──────────────────────────────────────────────────────────────────────┐ │
│ │   overlaid 3-series line; transparent legend and shared time cursor │ │
│ └──────────────────────────────────────────────────────────────────────┘ │
├────────────────────────────────────────────────────────────────────────────┤
│ Metric      Equal            Ridge            XGBoost                   │
│ Sharpe       ...              ...              ...                       │
│ Max DD       ...              ...              ...                       │
│ OOS IC       ...              ...              ...                       │
├────────────────────────────────────────────────────────────────────────────┤
│ [x] Only show differing configuration values                            │
│ Feature set   same            same             same                      │
│ α / depth     —               0.025            depth: 6                  │
└────────────────────────────────────────────────────────────────────────────┘
```

**Composition contract:** selection basket at top is explicit. Switch subviews with tabs. Compare metric **rows**, model **columns**. Always show measurement units, sample window, costs and baseline. For disparate model metrics use facet panels instead of one incomparable overlay. When a metric is undefined for one model, show em dash with an explanation; do not create false equivalence.

**Typical components:** C07, C12, C21–C25.

## Common responsive behaviour

- **≥1440px:** sidebar 224px and main content 24px gutter; 2-column support plots where useful.
- **1200–1439px:** maintain sidebar if meaningful, reduce nonessential panel gaps to 12px; primary plot full width.
- **1024–1199px:** sidebar collapsible or shrunk to 192px; toolbar wraps; 2-column support plots may stack. Parameter rail min 256px.
- **<1024px:** intentionally stack configuration above charts or use collapsible drawer, tabs do not clip without visible overflow, model selector remains reachable; internal research tools may optimise desktop but must not become unusable.
- **Height ~768px:** charts don't consume full viewport with no scroll; filtering and run selection visible without hunting. Data table scrolls internally only if a clear bounded panel exists.

## Information hierarchy test

At first glance a researcher should be able to identify, in this order: **(1) what is being analysed, (2) on what dataset/time/units, (3) what is selected, (4) the primary evidence, (5) what is comparable, and (6) how to inspect/modify it.** If the page instead makes them notice a large card, gradient, decoration or four giant KPI boxes first, redesign it.
