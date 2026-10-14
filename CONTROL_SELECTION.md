# Control selection and interaction rules

This file is **normative**: it tells an agent what component to choose when designing from an ambiguous description. Read `COMPONENT_CATALOG.md` for appearance, states, dimensions and accessibility; read `PAGE_BLUEPRINTS.md` for placement.

## 1. First determine the semantic task

Ask internally: Is the user (a) navigating between information views, (b) selecting one entity, (c) selecting many entities, (d) adjusting a numeric parameter, (e) filtering a population, (f) requesting computation, or (g) opening contextual details? Never choose a component because it merely looks attractive.

| User's action | Default control | If conditions differ | Never substitute |
|---|---|---|---|
| Switch `Overview / Metrics / Parameters` for current run | Underline **tabs** | Use left navigation if major app section | Segmented model picker |
| Choose one model from 3 fixed frequently switched baselines | **Segmented** | Dropdown if long labels, limited space, or metadata | Tabs pretending to be models |
| Choose one model from 5–100 models/versions | Searchable **single select** | Searchable model browser if metadata affects choice | 20 model pill buttons |
| Choose multiple models/runs to overlay | **Checkbox list** + comparison basket | Searchable multiselect if hundreds | Single-select dropdown |
| Choose among detailed alternatives with explanatory text | **Radio group** | Dropdown if many | Segmented short control |
| Turn on/off an independent plot overlay | **Checkbox** or switch | Switch for immediate persistent mode | Tabs |
| Select `Net / Gross` as *one view mode* | **Segmented** | Dropdown if in dense toolbar | Multiple toggles |
| Select `Include costs` as an independent Boolean assumption | **Checkbox** | Switch if instant presentation option | Gross/Net segmented when not mutually exclusive |
| Choose a dynamic metric with >4 entries | **Dropdown** | Searchable if >8–10 | Underline page tabs |
| Choose a continuous hyperparameter | **Numeric input** + optional slider | Log slider for many orders of magnitude, explicitly labelled | Slider alone |
| Select one exact threshold | **Numeric input** | Stepper/slider additionally, if bounded | Dropdown of all floating-point values |
| Filter by dates | **Date range picker** | Two labelled start/end fields for strict backtest periods | Ambiguous single date input |
| Filter table by string | **Search input** | Structured query builder for expression filters | Dropdown of every run name |
| Inspect details of selected row | **Right drawer** | Full detail page for deep history; tooltip for very small definitions | Blocking modal every click |
| Start expensive backtest/training | **Primary action** `Run` | Sticky footer for long form | Immediate recalculation on every keystroke |
| Export chart | Quiet **icon menu** or `Export` button | Split button if export type matters frequently | Prominent primary CTA |

## 2. Application navigation hierarchy

- **Topbar:** global destinations (`Backtests`, `Experiments`, `Signals`) and global environment. Preserve MLflow-like restrained dark shell.
- **Sidebar:** entity navigation within one destination (`Experiments`, `Saved strategies`, `Universes`), searchable and scrollable.
- **Underline tabs:** subviews of one selected entity (`Performance`, `Diagnostics`, `Parameters`). Persist via URL query when feasible.
- **Segmented control:** a compact change to presentation or one small enumerated parameter, *inside* a view (`Absolute / Relative`, `Table / Chart`). It must not impersonate tabs.
- **Dropdown:** scope/selector (`Model: Ridge`), usually in toolbar or configuration form. Never use tabs to list every model.
- **Checkbox list + compare basket:** set selection rather than one selected identity.
- **Inspector drawer:** transitory entity details while maintaining original browsing context.

If two layers appear to do the same thing, eliminate one. Example of **wrong** composition: topbar `Models`, tabs `Ridge / OLS / XGBoost`, segmented `Ridge / OLS / XGBoost`, dropdown `Model: Ridge`. Use one model picker and one separate set of *view* tabs.

## 3. Model selector decision procedure

1. **Selecting one or many?** If many, use checkbox list/basket (C12), not a dropdown that quietly limits selection.
2. If one, **are options fixed and ≤4, labels short, and switching frequent?** If yes, segmented group C08.
3. Otherwise use labelled single-select C09. If >8–10 options or remote dataset, make it searchable. Distinguish model *family* from saved *version/run*.
4. **Do users need metadata to choose?** If yes, use model browser/table C12 (name, family, trained date, OOS metric, status); a dropdown may narrow the selection first.
5. **Does the selection change parameters?** Show conditional controls in C26; clear stale configuration visibly and validate.
6. **Would changing model trigger expensive computation?** Stage changes in draft state and show explicit `Apply`/`Run`; don't recompute in `onChange`.
7. On selection change update all linked panels; label period and units, and preserve previous run only if explicitly comparing.

### Correct single-model toolbar example

`[Target: Spread Δ 5d ▾]   [Model: Ridge ▾]   [Universe: EUR IG ▾]   [Date: 2025-01–2025-06 ▾]            [Run evaluation]`

- Each field has an accessible name. Tooltips explain financial metric definitions.
- `Model` dropdown shows concise model name; training run, version, and hyperparams live in inspector or context subtitle.
- If selecting `XGBoost`, parameter editor swaps `α` for relevant tree parameters rather than retaining an unused visible input.

### Correct multi-model comparison example

`[Search models…] [Filter: Completed ▾]                         3 selected  [Compare 3]`

Below: a run table with checkboxes, model name, training interval, status, OOS score, features and commit; selection swatches map exactly onto chart colours. Compare may overlay only if target, period, and units align; otherwise prompt for alignment explicitly.

### Correct parameter editor example

```
MODEL CONFIGURATION
Model family     [Ridge                   v]
Training window  [2024-05-01 — 2025-06-30]
Regularisation α [             0.025     ]
Standardise      [x] Standardise features

[Reset]                              [Run backtest]
```

If `α` spans e.g. 1e-5 to 1e3 and exploration is needed, use a *labelled log-scale slider* in addition to numeric input; plot axis/steps must be correct.

## 4. Toolbar hierarchy: location dictates visual treatment

**Global toolbar** (directly under page header): experiment, universe, model, target, date, main action. Use 32px compact controls if it is dense but do not shrink labels below legible sizes. At most one visual primary action per action group.

**Local chart header:** chart title left; hover definition, chart type if truly relevant, and icon overflow right. Do not duplicate global model or date selectors above each graph. A chart-local unit selector is acceptable only if it affects that chart alone.

**Inspector drawer:** metadata and actions regarding selected object; don't let global `Run backtest` action appear in the drawer merely because there is space.

**Comparison basket:** controls that act on selected rows; appears adjacent to list/table and explains number selected. Never let compare run accidentally on entire filtered table when only three rows checked.

**Loading actions:** button remains width-stable. Use clear labels (`Running…`, `Refreshing…`); disable duplicate submissions, retain `Cancel` only if supported. Keep the old plot until the new one is ready or show deliberate stale-context state.

## 5. Three different kinds of state — never conflate

1. **Navigation state:** current app section, entity, tab, selected row. Changes usually immediately.
2. **Query/view state:** dates, filters, display unit, overlays, zoom. Can update immediately if computation is cheap; save URL for sharing.
3. **Research configuration state:** model family, hyperparameters, training split, costs, constraints. Draft changes and commit atomically before expensive train/backtest. Clearly mark `Unsaved changes` or `Configuration modified` when applicable.

Do not mix them inside independent component local states: charts, metric strips and tables must all derive from a shared query/result definition.

Suggested TypeScript state shape:

```ts
type ResearchViewState = {
  navigation: { section: string; entityId: string | null; tab: string };
  query: { universeIds: string[]; dateRange: [string, string]; metrics: string[];
           selectedRunIds: string[]; baselineRunId: string | null; filters: Record<string, unknown> };
  draft: { modelFamily: string; hyperparameters: Record<string, number | string | boolean> };
  committed: { runId: string | null; configHash: string | null };
};
```

Use derived selectors, not mutable duplicated copies of filters in each chart. Selection state is an array/set of stable IDs, not list indices. Preserve stable colour mapping keyed by run ID.

## 6. Direct manipulation and expensive operations

| Operation | Expected latency | UI strategy |
|---|---|---|
| Toggle chart legend / table sort | milliseconds | Apply immediately |
| Switch between cached metrics | milliseconds | Apply immediately |
| Change model/view dropdown with cached result | small | Immediate, preserve loading footprint |
| Filter 100k rows remotely | 100ms–seconds | Debounce, abort stale request, show result count |
| Refit regression / compute expensive loss surface | seconds | Draft parameter form + `Apply`/`Compute` |
| Re-run full backtest / train ML model | minutes–hours | Explicit `Run`, progress and provenance, safe cancellation only if actual backend supports |

Loading: don't allow a previous request's late response to overwrite results from a newer selection. Surface last-updated date and a stale indicator when useful.

## 7. Selection and comparison invariants

- Model/run names have stable IDs (versioned). Display data ID and commit in inspector.
- Series colour is allocated by stable ID once per comparison workspace, not by render order.
- User sees if comparing **gross vs net**, **different currencies**, **different time horizons**, **different sample definitions**, **different target construction**, or **nonoverlapping periods**. Do not combine in same plot by accident.
- The plot legend always describes what the line means; `Train`/`Validation` uses line style or grouped legend, while model identity uses colour. Do not overencode every dimension with a new saturated colour.
- Compare table can show `Only differences` and `Show all` if comparing parameters; unchanged values de-emphasised.
- Clicking a row to inspect is distinct from checking its box to select for comparison. Row hover cannot be mistaken for checked state.

## 8. Error / empty / edge-case behaviour

- **No model fits filter:** dropdown says `No matches`; `Clear filters` available without wiping the experiment.
- **No runs selected:** chart explains `Select a run to view metrics`; no fake demo series.
- **Only one selected:** show its diagnostics, disable compare if two required.
- **Model has zero observations:** distinguish `0` from missing and `No observations`.
- **Training in progress:** chart may show partial metrics with `Running` state; never mark as final.
- **Run failed:** name/metadata stay inspectable; explain error and allow valid retry rather than silently removing row.
- **More than six overlay series:** ask user to narrow or switch to table/small multiples; not a rainbow chart.
- **Narrow viewport:** side pane can collapse; toolbar wraps; legend and units remain visible, no horizontal clipping of primary action.
- **Keyboard and screen reader:** tab role semantics, correct select ARIA, row selection checkbox labels, icon-button names, focus restoration on dialog close, escape/click-away on menus.

## 9. Design cues that coding agents must internalise

**Minimal but information-rich.** Dense does not mean cramped: 13px body, 32/40px controls, 12px chart/table gaps, 1px dividers, 16–24px main gutters. Avoid 24px cards separated by 36px blank space.

**Visual hierarchy by structure, not saturated colour.** Use page title → toolbar → underlined views → panels → metadata. Reserve blue for actions, links and selected states; plot colours independently. Only failure, warning and success borrow status colours.

**Tables and plots share one analysis contract.** Same filters/date scope and clear units. Place one large decision-driving chart prominently, rather than six equally weighted chart cards when a hierarchy exists.

**Reproducibility is a design feature.** Keep the chosen model, dataset version, train/test cutoffs, parameter settings, costs and code revision discoverable, not buried in devtools.

**Scientifically truthful defaults.** Plot sample count, uncertainty, NaNs, costs, position sign, horizon, currency and axis transformation transparently. Don't invent missing results for visual polish.

**Restraint beats novelty.** No gradients, blur, marketing layout, oversized rounded cards, AI sparkles, playful emoji, glossy effects, or universal pill buttons. A restrained UI is distinguished by alignment, exact controls, clean typography and responsive chart interaction.

## 10. Executable review scenarios (agent must check)

1. **One-model selection:** choose `Ridge` (via dropdown or segment). Model-dependent `alpha` appears; changing to `XGBoost` replaces it. A cached metric immediately updates or a run action clearly awaits execution.
2. **Multi-run compare:** check two rows, verify `Compare 2` enables. Check third, verify count and stable colours across table, chart and legend. Uncheck second, ensure its plot series disappears.
3. **View tabs:** open `Diagnostics`, then return to `Performance`; entity/filter selection persists, active underline and URL reflect correct view.
4. **Date change:** chart, metrics and table all use same range; stale results visually identified until backend returns.
5. **Error:** one chart fails but other panels remain; show a panel-scoped retry and preserve model selector selection.
6. **Keyboard:** reach and activate tabs, dropdowns, checkboxes and apply button; escape from menu and restore focus.
7. **Overflow:** view at 1024×768; toolbar wraps or condenses intentionally; no clipped chart labels or inaccessible export menu.
