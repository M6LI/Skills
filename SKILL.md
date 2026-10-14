---
name: quant-research-ui
description: "Reproduce the visual language of MLflow's dark experiment tracking UI at high fidelity in quantitative analytics apps: the same colour tokens, system-font stack, DuBois-inspired controls, icon vocabulary, tables, navigation, and interaction states. Use for backtest viewers, ML training experiments, signal analysis, RFQ model diagnostics and research tools. Offline specification with project-local implementation assets; do not browse while applying it."
compatibility: Can be used offline by GitHub Copilot. Exact DuBois components/icons require assets from MLflow or the design-system package to be present; otherwise fidelity is a documented approximation, not a pixel-perfect claim.
metadata:
  version: "3.0"
  reference: "MLflow dark tracking UI, public master source inspected 2026-10-13"
  fidelity: "source-grounded MLflow/DuBois primitives plus 35 documented quant UI components, interaction rules and five complete reference blueprints; exact original artwork/components not redistributed"
---

# Quant Research UI — high-fidelity MLflow dark UI

**Primary goal: visual fidelity, not reinterpretation.** Implement a research interface that looks like MLflow's dark experiment tracking UI, reusing its dark colour values and DuBois component language instead of restyling it with custom modern-dashboard trends. Change only the information architecture required by the quant application's workflows. The experience should be recognisably MLflow to someone who uses MLflow. Never insert MLflow logos, trademarks, names, or false product affiliations into the user's application.

**Offline instructions:** This package is self-contained: `SKILL.md`, `assets/tokens.css`, `assets/components.css`, four specification references (components, selection, layouts and iconography), a source audit, and a local component gallery. All files are local; no internet access is required to apply the spec. Read the local references when implementing controls. Read `references/MLFLOW_SOURCE_AUDIT.md` for what is verified (source-derived) versus approximated. An agent must not claim that inaccessible proprietary components, source SVGs or fonts are included. When exact assets are required, use vendored `@databricks/design-system` if available and permitted; if not, implement the documented CSS reproduction and explicitly record the gap. Do not substitute Lucide by default and pretend the icons match.

**Priority on conflicts:** source-derived DuBois dark tokens and component specifications in section 2 > `references/COMPONENT_CATALOG.md` and `references/CONTROL_SELECTION.md` > quant-specific page blueprints > examples here > existing project styling. The quant-specific chart and functional requirements remain mandatory. Do not alter numerical computation to achieve style.

## START HERE: v3 design specification (mandatory)

**Every agent call using this skill must first perform this short procedure:**

1. Read **this** `SKILL.md`. Then read `references/CONTROL_SELECTION.md` to decide **which controls** fit the user's operations; read `references/COMPONENT_CATALOG.md` for the **appearance, variants, states, sizes, placement and keyboard behaviour** of all controls used. Read `references/PAGE_BLUEPRINTS.md` to choose the appropriate **complete research layout** (A–E). Read `references/ICONOGRAPHY.md` whenever adding icons. Read `references/MLFLOW_SOURCE_AUDIT.md` if fidelity to the original is being evaluated.
2. Import **both** `assets/tokens.css` and `assets/components.css` (in that order), or translate their documented properties to existing components without altering the design language. For quick offline inspection, open `assets/component-gallery.html` in a browser: it demonstrates the packaged components with labelled synthetic examples and simple working interactions.
3. For any requested selector, decide **navigation vs single choice vs multiselection vs continuous parameter** before rendering. In particular, **do not use view tabs for choosing model families.** See the rules below.
4. Implement real shared state, model-dependent controls, authentic data, chart labels, error/loading/empty states and responsive layout; do not build decorative placeholders.
5. Do not claim exact MLflow glyphs/components are bundled: CSS is an adaptation based on inspected source measurements; reference DuBois icon names are available but original icon artwork is not distributed.

### Non-negotiable interaction choices

| Intent | Required default | Critical usage detail |
|---|---|---|
| Navigate `Overview / Metrics / Diagnostics / Parameters` | Underlined **tabs** | Changes view, not underlying selected model |
| Choose one model from ≤4 fixed short options | **Segmented control** | Only for frequent, inexpensive switching |
| Choose one model from many or dynamic variants | Labelled **single-select dropdown** | Searchable when long list; distinct model version/run |
| Compare multiple models or backtests | **Checkbox run list + comparison basket** | Stable series colours and shared data scope |
| Edit α, λ, half-life, depth, threshold | **Numeric input** (+ optional slider) | Explicit apply/run when computation costly |
| Filter dates, universe, metric and target | **Toolbar selectors** | One shared query state across all panels |
| Inspect selected model/run details | **Side drawer** or detail region | Never obstruct comparisons by default |
| Switch `Gross / Net` or `Absolute / Relative` | **Segmented** | Exactly one presentation variant at a time |

### v3 source and adaptation boundaries

- **Verified source measurements:** 40px regular button, 32px small button, 13px button and tabs text, 4px button radius, 40px single-select, documented dark DuBois colours and a set of original icon *names*. **These are source CSS/token facts, not a guarantee about the computed styling of every MLflow version.**
- **Prescribed quant extensions:** joined segmented control, 36px table header, sidebar width, chart panel size, numerical editors, comparison basket, parameter rail, all page blueprints. They are deliberate standards, not claimed to be copied pixel-for-pixel from MLflow.
- **Faithfulness constraint:** never install a component framework's stock visual theme over the custom CSS. Reuse genuine DuBois components only if locally available and legitimately usable. Never put MLflow branding into the independent app.

## 1. Non-negotiable identity

The app should resemble a precise scientific instrument: understated, information-dense, quiet, exceptionally legible, desktop-oriented, chart-first, and purpose-built for researchers who scrutinise numbers. UI chrome recedes; data receives attention. Every surface must have a reason to exist.

**A good first impression:** a restrained dark top bar (not a bright/navy brand stripe), dark experiment navigation on the left, a dark central work surface, compact tabs and filters, subtle 1px dividers, restrained blue emphasis, a data table or run selection area, and carefully gridded analytical plots with clearly labelled axes.

**Never build:** a marketing landing page; an admin template with giant KPI cards; coloured gradient/blur/glass backgrounds; neon outlines; floating pill cards everywhere; oversized empty margins; 20px border radii; fake analytics; emoji navigation; celebratory illustration; huge headings; light surfaces in dark mode; or charts with unlabeled axes. Do not decorate merely to fill space.

**Priorities (highest first):** (1) analytical correctness, (2) legibility, (3) easy comparison, (4) coherent visual hierarchy, (5) interaction discoverability, (6) aesthetics. Visual elegance comes from disciplined detail rather than spectacle.

## 2. MLflow/DuBois dark-mode design tokens: source-grounded

These values were taken from `mlflow/server/js/vendor/design-system/dist/dubois-colors.less` in the public MLflow repository, rather than visually sampled. Use `assets/tokens.css`. Distinguish **actual DuBois palette values** from the application-specific assignment of those values to a sidebar, topbar or chart surface, which may depend on the MLflow release.

| Theme role | Exact dark token value | Usage |
|---|---|---|
| backgroundPrimary | `#11171C` | Body and data surface |
| backgroundSecondary | `#1F272D` | Secondary and elevated sections |
| textPrimary | `#E8ECF0` | Main text |
| textSecondary | `#92A4B3` | Secondary UI text |
| textPlaceholder | `#8396A5` | Placeholders |
| border | `#1F272D` | Hairlines on primary background |
| borderAccessible | `#C0CDD8` | High-contrast outline when needed |
| actionPrimaryBackgroundDefault | `#4299E0` | Primary action (theme semantic token) |
| actionPrimaryBackgroundHover | `#8ACAFF` | Primary action hover |
| actionDefaultBorderDefault | `#37444F` | Secondary/default button outline |
| actionDefaultTextDefault | `#E8ECF0` | Button text |
| actionDefaultIconDefault | `#92A4B3` | Neutral icon |
| actionDefaultIconHover | `#8ACAFF` | Icon hover |
| actionLinkDefault | `#4299E0` | Links |
| actionDisabledBackground | `#37444F` | Disabled surface |
| actionDisabledText | `#5F7281` | Disabled label |
| textValidationSuccess | `#3BA65E` | Positive status |
| textValidationWarning | `#DE7921` | Warning status |
| textValidationDanger | `#E65B77` | Error status |
| tableBackgroundUnselectedHover | `rgba(189,205,219,.04)` | Row hover |
| tableBackgroundSelectedDefault | `rgba(189,205,219,.08)` | Selected row |
| tableBackgroundSelectedHover | `rgba(189,205,219,.12)` | Selected hover |
| actionDefaultBackgroundHover | `rgba(138,202,255,.08)` | Quiet default action hover |
| actionDefaultBackgroundPress | `rgba(138,202,255,.16)` | Pressed action |

**Colour semantics:** Use these exact colours for the design-system primitives. Analytical series are a separate identity palette and may use blue/orange/teal/purple provided the hue remains understated. Chart axes must remain legible. Never turn the interface into a navy-led dashboard: the source dark theme has neutral charcoal `#11171C` and `#1F272D` as its base.

### Typography: retain the source's default web UI stack

The vendored `index-dark.css` includes a system stack: `-apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, 'Helvetica Neue', Arial, 'Noto Sans', sans-serif`. For your Windows/IntelliJ workflow, **Segoe UI** is expected to render if available; avoid prescribing Inter or loading Google Fonts. Body size 13px is supported by common DuBois styles. Use numerical tabular alignment. Reserve monospace for IDs, code, and scientific notation.

| Role | Suggested size/line-height | Status |
|---|---|---|
| Base UI/body | `13px / 20px` | Source component reference |
| Standard button | `13px / 20px` | From vendored CSS |
| Small button | `13px / 20px` | From vendored CSS |
| Small metadata/axis labels | `11–12px / 16px` | Quant extension, not verified MLflow token |
| Panel heading | `13px / 20px`, semibold | Quant extension |
| Page title | `20px / 28px`, semibold | Quant extension |

### Components: don't substitute generic visual libraries

The MLflow frontend's `CLAUDE.md` explicitly requires components from `@databricks/design-system` (DuBois): `Button`, `IconButton`, `Input`, `Select`, `Table`, `Tabs`, `Tooltip`, `Popover`, `Drawer`, `Modal`, `Typography`, and others. Its package.json resolves `@databricks/design-system` through a **vendored local file path** (`file:./vendor/design-system`); do **not** assume a standard public `npm install @databricks/design-system` works in another project.

Use the actual components when legitimately available in the local project. Otherwise reproduce their appearance with the local CSS primitives supplied here. Do not install shadcn, Mantine, MUI or Tailwind defaults and call them 'identical'; using these only for unstyled behaviour is acceptable if completely restyled.

**Buttons:** the vendored dark CSS specifies regular `.du-bois-dark-btn` height **40px**, horizontal padding **16px**, font **13px**, radius **4px**; small button **32px** with padding **16px**, font **13px**, radius **4px**. The compiled CSS also contains legacy-prefixed button variants with different specific colours; prefer current DuBois *semantic* action tokens when reproducing the appearance without its implementation. An icon button must use equal width and height, and an accessible name. Do not use invented 5–6px radius or default 31px button height.

**Selects:** the source's single-select selector height is **40px** with **12px** horizontal padding. Use source-style rectangular controls, clean 1px borders and subtle focus blue. Input family should share the 40px height unless the original page clearly uses a compact variant.

**Tabs:** source `.du-bois-dark-tabs-tab` includes **13px** font and **4px 0** padding at the primitive level; final app spacing depends on the tab bar structure. Favor underlined active tab rather than filled modern segmented-pill controls.

**Table:** the vendored stylesheet contains dark table surfaces and thin separators. Source semantic tokens define exact hover/selected colours above. Retain dense aligned rows and understated header background. Row height, table-header size and sticky mechanics are *application-level approximations*, not asserted MLflow defaults.

**Tooltips:** the vendored CSS includes a legacy `4px` corner radius and `6px 8px` padding for `.du-bois-dark-tooltip-inner`; use small tooltips, not large glossy black cards.

### Iconography: DuBois first, not Lucide

The vendored package contains generated DuBois icon exports; verified examples include `SearchIcon`, `FilterIcon`, `DownloadIcon`, `PlusIcon`, `CloseIcon`, `ChevronDownIcon`, `ChevronRightIcon`, `ChartLineIcon`, `BarChartIcon`, `TableIcon`, `FolderIcon`, `RunIcon`, `PlayIcon`, `PauseIcon`, `ClockIcon`, `CalendarIcon`, `CheckIcon`, `WarningIcon`, `InfoIcon`, `CopyIcon`, and `TrashIcon`. Build the icon vocabulary around these actual export names. For an offline agent without package assets, use local, deliberately drawn simple SVGs **but mark them as approximations** rather than pretending their strokes match. Default icon box **16px**, optical alignment to 13px labels; precise glyph path depends on the real icon package.

### Layout and spacing

Source frontend guidance uses `theme.spacing` and demonstrates 4px, 8px, 16px, 24px and 32px spacing roles. Set a consistent 4px grid in CSS. Geometry for the application's shell, split-panes and plot panels is **an adaptation**: it should be visually validated against a screenshot of the chosen MLflow release if pixel-level fidelity matters. Prefer minimal borders, modest radius, no gradient, and no 'KPI-card' look.

## 3. Macro layout: default research shell

Prefer this geometry at desktop widths 1440–1920px:

```text
┌────────────────────────────────────────────────────────────────────────────────────────┐
│ DARK TOP BAR  48px   App / project · context             search      export / settings  │
├───────────────────┬────────────────────────────────────────────────────────────────────┤
│ LEFT NAV  220px   │ Breadcrumb / context  24px                              20px gutter│
│                   │ Analysis title                         metadata       actions       │
│ ▸ Projects        │ Tabs: Overview   Runs   Charts   Compare   Diagnostics             │
│ ▸ Experiments     ├────────────────────────────────────────────────────────────────────┤
│   • Experiment A  │ Toolbar: search · date · universe · model · filters · reset        │
│   • Experiment B  ├────────────────────────────────────────────────────────────────────┤
│                   │ CONTENT AREA                                                ↑     │
│ ▸ Saved views     │ ▌Run/data selector, sortable table       or resizable left pane     │
│                   │ ├──────────────────────────────────────────────────────────────────│
│                   │ │  Plot A (time series)        │ Plot B (diagnostic)                 │
│                   │ │  x labels + units           │ x labels + units                  │
│                   │ ├─────────────────────────────┴──────────────────────────────────│
│                   │ │  Parameter comparison / residuals / results table               │
│                   │ └──────────────────────────────────────────────────────────────────│
├───────────────────┴────────────────────────────────────────────────────────────────────┤
│ optional thin status strip: dataset · date range · last computed · units · sample size │
└────────────────────────────────────────────────────────────────────────────────────────┘
```

- Top bar approximately 48px in this adapted shell (not a verified source dimension); keep it neutral charcoal. Product name left; only 2–4 top-level destinations, never crowded.
- Left navigation 216–232px expanded, collapse to a 52px icon rail. Background uses the verified secondary charcoal token; divide with a subtle right border. Vertical tree indentation 14–16px. Hover adds subdued translucent grey; active item uses DuBois selected-row colour and a thin blue indicator. Search in nav only if navigation exceeds ~12 entries.
- The **content header is not a hero**. Title 20px, muted concise subtitle/metadata, tabs under it. Header height determined by content (~90–112px for 2 lines plus tabs), not giant fixed vertical space.
- Content area is max-width unconstrained for charts/tables: use all available room with 20–24px gutters. Do NOT clamp analytical charts into a narrow centered marketing column.
- Standard panel header 36–40px, with title left, export/expand/more on right. Content below; plot has own minimum height.
- A wide, sparse chart gets one large panel. Multiple independent diagnostics get a compact two-column grid. Do not put every value into a card.
- Split pane for selecting runs on left and chart canvas on right when comparison is the primary task: selector 250–310px or ~25–30%, chart flexes. Include a drag handle when practical and preserve width while navigating.
- For table-first views, use a full-width table with sticky header and a right-side details drawer on row activation instead of forcing a chart alongside every table.
- At 1200–1440px reduce sidebar to 200px and plot grid to one or two columns depending on axes; at <1000px collapse navigation and stack plots. Desktop is the primary target, but never hide core data on narrow screens.

## 4. Navigation, context and tabs

Use noun-based navigation reflecting analysis objects, e.g. `Projects`, `Backtests`, `Models`, `Signals`, `Saved views`, and contextual tabs `Overview`, `Runs`, `Charts`, `Compare`, `Diagnostics`, `Artifacts`. Do not blindly add all of these: use only those supported by actual functionality. An individual single-purpose visualizer may need no permanent sidebar; use the same header, tabs, toolbar, surfaces, and tokens.

Breadcrumbs `Research / Credit / Signal combination` use 11–12px muted text. Selected tab is white text with a thin blue 2px bottom border. Inactive tabs are grey, with a compact baseline and underlined active selection. The source tab primitive is 13px with 4px vertical padding; page-level height is an adaptation. Do not use large capsule tabs for primary navigation.

Controls should preserve user context (active tab, filters, selection, visible charts, axis mode) in URL query params when reasonable. A user's comparison must not disappear when viewing details and returning.

## 5. Toolbars and controls: compact, aligned and meaningful

- Primary filter row 36–40px high; left aligns search and frequently-used filters; right aligns view toggles, `Export`, and actions. Wrap to a second line rather than horizontally squashing labels.
- Use the source-derived 40px regular / 32px small buttons, with 4px corners and exact DuBois dark action colour tokens. Primary blue-filled button is reserved for **one significant action** (e.g. Run analysis), and only if there is such an action.
- Inputs/selects should generally use the 40px source-referenced control size with 4px radius and 13px text (compact variants only when needed); search icon at left; placeholder specifies search semantics (`Search runs, tags, model IDs`). On focus use a visible blue outline.
- Dropdowns show text plus subtle chevron; use 8px menu row padding, selected checkmark, and a 1px separator between groups.
- Keep the most important filter selectors always visible: date/window, instrument universe, benchmark, target horizon, model. Put rare filters in an `Advanced filters` popover with a visible active-filter count.
- Show filters as inline compact chips only when active; every chip individually removable; `Reset filters` appears only when needed.
- For nontrivial parameter ranges, show exact numeric input **alongside** slider; allow typing scientific notation and negative numbers as relevant.
- Every time range control must show bounds and timezone; every financial amount has currency; every rate has unit (%, bp, etc.).
- Provide a distinct loading state; don't apply a big global spinner for independent panels. Provide cancellation/progress for expensive operations where supported.

## 6. Data tables are first-class, not afterthoughts

A research table is a primary view and must feel as polished as the charts.

- Header height 32–34px; row height 34–38px, user-adjustable density optional. Full-width 1px soft separators, **not individual boxed cells**. No zebra backgrounds by default. Subtle hover background `--bg-hover`.
- Background remains nearly flat; table header gets `--bg-surface` with sticky positioning. Freeze identifying columns on horizontal scroll when many metrics exist.
- Left-align names/IDs; right-align numerical values and headers; use `font-variant-numeric: tabular-nums`. Decimal align when feasible. Dates consistently formatted; ID truncation includes full tooltip/copy action.
- Numeric formatting is domain-aware: `+12.4 bp`, `−0.36%`, `£1.24m`, `0.823`, `3.2×10⁻⁵`. Avoid arbitrary four decimal places everywhere. Negative numbers are not automatically red unless they denote a bad outcome in that specific metric.
- Rows support hover, selection via checkbox, sorting, and detail on click. Distinguish checkbox selection for multi-compare from row opening details. The chart series corresponding to checked rows match stable series colours.
- Table toolbar: query/filter, column visibility, sort, density, export visible rows (and optionally full dataset). Do not show disabled controls as if they work.
- Visually distinguish train/validation/test, in-sample/out-of-sample, successful/failed, and baseline/candidate using explicit text plus restrained tags/dots, never colour alone.
- For large tables (>~500 rows) use virtualisation or server pagination; keep header/selection stable and preserve scroll intelligently.
- Include an empty-results message that names the current filters and offers a working reset control.

## 7. Chart house style: this is the most important section

Every chart must answer a specific research question. Use ECharts when starting a new rich chart-based React app; Plotly where scientific graph interaction is paramount; existing chart libraries are acceptable if styled to match these requirements. Never import charts from CDNs in an offline project; install dependencies if project/network policy allows or work with existing packages. If installing is impossible, implement a functional native SVG/Canvas alternative for simple charts. **Chart semantics, correct units and accessible interactions outrank library choice.**

### Universal chart construction

- Chart is in a rectangular `--bg-surface` panel with 1px `--border-soft` border and 7px radius; no gradient, noise texture, or drop shadow.
- Title inside top-left is 12px/600; short description beneath only if needed to disambiguate (`Cumulative mark-to-market P&L · net of costs`). Top-right: compact compare selector, expand/export, `⋯` if actions exist.
- Default panel height ~325px; plot actual drawable height at least 225px. Major timeseries may be 380–460px. Chart content padding approx left 42–62, right 15–22, top 18–25, bottom 38–55 **inside** panel after header. Increase margins for long ticks, not by clipping labels.
- Horizontal gridlines 1px `--chart-grid`, opacity 0.7; usually 4–6 ticks. Vertical gridlines only when needed for time interpretation and subdued. Axes have no thick dark spines. Axis labels 10–11px in `--chart-axis`; units in axis or caption, NEVER implicit.
- Series lines width ~2px (1.5px for dense multi-run); no dot markers along every dense sample. On hover display marker and thin vertical crosshair. Use 0.1–0.18 alpha fill only when area fill is meaningful.
- Legend uses coloured 8px line/key + readable text, located below chart or in a compact top strip. If legend is long (>6 entries) use a list with scroll/search. Clicking toggles a series; Shift-click (where implemented) isolates; muted styles show hidden series. Tooltip repeats series names alongside values.
- Tooltip dark raised surface, thin border, 6px radius, 12px text, subtle shadow, tabular values, explicit timestamp (and timezone as relevant), precise units, series swatches. Keep tooltip within viewport. For compare plots support shared x-axis hover across panels when x domains align.
- Interactions: hover, zoom/brush, reset zoom, double click to reset (if documented), toggle series, download PNG/SVG and CSV data when sensible, full-size plot view. Keyboard-accessible equivalents to key actions when feasible.
- Don't connect time series across missing observations; distinguish missingness from zero. Render NaN/failed results honestly. Time axes must sort chronologically and avoid accidental categorical rendering.
- Never apply smoothing, resampling, rolling means, log transforms, clipping, or interpolation silently. Label them and make raw results accessible.
- For comparative series, ensure same axis units, same sample definitions and common date range unless user explicitly chooses otherwise. Label in-sample/out-of-sample boundary and major event markers discreetly when informative.

### Chart types by research question

| Question | Preferred representation | Specific requirement |
|---|---|---|
| Backtest performance over time | Wide cumulative P&L/equity line chart | State base capital or P&L currency, costs included/excluded, evaluation period, baseline |
| Risk accumulation | Drawdown line/filled area, usually negative | Zero baseline; peak-to-trough convention; annotate worst drawdown if available |
| Return distribution | Histogram + optional reference density | State sampling interval, bin width, number of observations; don't imply normality |
| Two strategies compared | Overlay with stable colour + benchmark | Common axis and horizon; allow rebasing / normalisation when comparing scale |
| Predicted vs realised move | Scatter with y=x reference | Axes same units; alpha or hexbin for overplotting; sample count |
| Forecast calibration | Reliability diagram + ideal line | Prediction bins, empirical rate, bin counts, uncertainty where available |
| Residual errors | Time plot, histogram and residual-vs-prediction | Preserve signed errors and outliers |
| Train and validation learning | Loss vs steps line plot | Separate train/val, log scale toggle if appropriate; mark best checkpoint |
| Hyperparameter sweep | Scatter/heatmap/parallel coordinates | Legend for colour encoding; don't map arbitrary model IDs to continuous gradients |
| Signal predictive strength | Rolling IC or alpha-decile returns | Explicit horizon, windows, transaction cost assumptions, confidence |
| Model explanation | Sorted feature importance bars | State method (gain, SHAP, permutation); avoid causal interpretation |
| Correlation/covariance | Heatmap with diverging scale | Centred at 0 for correlation; visible colourbar and units/normalisation |
| RFQ execution likelihood | Calibration plot and binned residuals | Include number of observations, hit-rate base rate, and segment filters |

Do not include all plots by default. Choose those that support the user's exact analytical objective. Prefer the **two or three decision-driving plots** first and move residual diagnostics into a secondary tab.

### Visual encoding discipline

- Sequential positive quantities: desaturated blue scale. Signed differences: balanced blue-to-muted-red diverging scale centred on zero. Categories: stable six-series palette. Don't use a rainbow gradient or green-vs-red alone.
- Colour meaning consistent across table rows, legend, detail drawer and all charts.
- Reserve red/pink for real negative status or risk, and yellow for alerts; positive/negative numerical values may need neutral text depending on context.
- For bar charts: zero baseline unless explicitly justified; categories ordered meaningfully; horizontal bars for long labels. For line charts: zero baseline optional (choose meaningful y domain; never distort without clear axis ticks). For density estimates: clarify bandwidth. For heatmaps: label the colour legend.
- Show sample sizes and data provenance in the title subtitle, caption or detail drawer. Warn about sparse series and incompatible date alignment.

## 8. Domain-specific page recipes

### A. Backtest explorer

Header: `Backtests / Strategy name`; top metadata line: strategy version, dataset, date range, currency, costs assumptions. Filters: universe, strategy configuration, dates, benchmark, gross/net. The content hierarchy is (1) cumulative P&L full-width or 2/3 width, (2) drawdown / exposure, (3) compact metrics strip or metrics table, (4) trades/results table, (5) full detail drawer. Use Sharpe, realised vol, max drawdown, turnover only if defined and available. Clearly distinguish bp, % return, cash P&L, and spread move; do not exchange one for another.

### B. Model training experiment browser

Left pane lists training runs with checkbox, status dot, colour key and searchable name. Top selects experiment and metric. Main right area has model loss by step, validation metric by step, optional training systems metrics. Tabs `Charts`, `Parameters`, `Diagnostics`, `Artifacts`. The selected run exposes train data interval, feature set/version, model family, hyperparameters, metric definitions, commit/run identifier. Support 2–6 run comparisons without shifting colour identities. A full-width comparison table can show parameter differences with unchanged values de-emphasised.

### C. Signal research workbench

Header exposes instrument universe, observation unit, signal horizon, target definition, training/validation/test dates. Tabs `Overview`, `Signals`, `Combination`, `Attribution`, `Diagnostics`. Primary comparison: selected signals and combined predictor vs realised future change, with rolling IC, quantile portfolio performance and turnover/coverage. Left column lists selectable signals and weights with uncertainty or validation score. Show lag/alignment caveats; never compute or display features using future information by accident. A weight comparison table and rolling coefficient chart are often more useful than oversized summary cards.

### D. Curve/model visualiser

Often no sidebar is needed. Left settings rail 270–320px, right 1–2 large charts. Settings grouped into small labelled sections: dataset/time, parameters, fit settings. Right plot overlays observed points and fitted curve with residuals beneath. Cursor crosshair and parameter sensitivity controls. Numeric parameter values remain editable while adjusting sliders. Display model definition and objective/loss in a small disclosure, not a prominent decorative card.

## 9. Dense analytical components

**Metric strip (when useful):** 3–6 values in a single horizontal container separated by vertical 1px rules, NOT separate giant cards. Each has muted 11px label, 18–24px numeric value and optional 11px context (window, comparison to baseline). Use comparative arrows only with unambiguous meaning. Avoid mixing scales without units.

**Details drawer:** right aligned, 340–430px, full-height below topbar, raised background, left border. Includes entity name, close, small metadata, tabbed sections, key-value grid, link to full page. Does not obscure the context of table selection unnecessarily. Escape closes; URL/back restores state.

**Status:** subtle coloured 6–7px dot + text (`Completed`, `Running`, `Failed`), or muted outlined tag with full text. Never status-by-colour only.

**Empty state:** one sentence identifying what's missing and a real recovery action. No illustration, motivational copy, or generic "get started" animation.

**Notification:** contextual inline message for data quality, stale timestamp, failed job, invalid compare, no data. Keep non-blocking except for destructive decisions.

**Tooltips:** provide definitions for potentially unfamiliar metrics and window calculations. State how `Sharpe`, `IC`, `bps`, `hit ratio`, `turnover`, `p-value` or `CV score` were computed if used. Never invent methodology.

## 10. Interaction contract: a research app must behave correctly

- Search, sort, filter, date selection, checkbox selection, tabs, hover and chart legends work; no decorative fake controls. A user should always know which population is being analysed.
- All charts, tables and metric summaries respond consistently to filter and run selection. Shared state is centralised rather than independently duplicated in each component.
- If actions trigger computation, separate `Apply / Run` from transient form edits when recalculation is expensive; for cheap transforms update immediately with no flashing.
- Loading/empty/error/partial-data states are designed for every data-dependent panel. A failed panel should not destroy unrelated successful results.
- Preserve reproducibility: expose configuration, model/data version, date range, timestamp, metric formula and, where practical, a shareable deep link. Do not claim performance results not backed by calculations.
- For comparable plots, synchronise time cursors; provide benchmark toggle, zoom reset and exact value readout.
- Include keyboard focus, `aria-label` for icon buttons, semantics on tabs/tables, and minimum 4.5:1 contrast for normal text where practical. Check contrast of muted labels on dark panels (never sacrifice legibility for subdued aesthetics).

## 11. Technology strategy

Default for new standalone apps: **React + TypeScript + Vite + CSS variables**, optional Tailwind if already adopted. Use shadcn/Radix primitives selectively for menus, selects and dialogs, only for behaviour when DuBois is unavailable; completely override the library visual styles with these tokens and measurements. For charts prefer **Apache ECharts** (dataZoom, linked axes, scatter, heatmap, dense time-series) or an existing mature plotting library. For serious tables use **TanStack Table** plus virtualisation when needed; AG Grid is an option if already licensed/installed and its theming is controlled. Prefer the existing DuBois icon exports if they are in the project. If not, use the included icon vocabulary as a guide; any replacement icons are approximate and must not be described as exact.

If React already exists, use it; do not rewrite a functioning app merely to follow a preferred stack. If a Python plotting framework is the existing app, apply tokens and layout where possible; avoid unjustified cross-stack migration. Where the environment is disconnected, reuse installed dependencies and avoid network APIs, remote fonts, CDN-only assets or screenshots. Use included CSS tokens rather than remote design-system instructions.

Prefer components organised as:

```text
src/
  design/tokens.css                 // global palette, spacing, typography
  components/layout/{AppShell,Topbar,Sidebar,PageHeader,Toolbar}.tsx
  components/data/{ResearchTable,MetricStrip,FilterChips}.tsx
  components/charts/{ChartPanel,SeriesLegend,ChartToolbar}.tsx
  components/details/{DetailDrawer,KeyValueGrid}.tsx
  features/<domain>/{...}          // business logic and domain-specific views
```

Keep calculations and domain semantics separate from UI; chart components accept clean typed data, units, display formats, and stable series IDs. Avoid hardcoding data into JSX or placing complex business transformations in render functions.

## 12. Concrete CSS and component behaviour

**Import `assets/tokens.css` globally first and `assets/components.css` second.** For new components prefer the v3 `.qr-app`-scoped `.qr-btn`, `.qr-btn-primary`, `.qr-btn-sm`, `.qr-input`, `.qr-select`, `.qr-tabs`, `.qr-table`, `.qr-panel`, `.qr-segments`, `.qr-metric-strip` and shell helpers. The older `.quant-*` helpers in `tokens.css` remain for backward compatibility; do not mix both class families on the same element. Map these measurements to existing React components if appropriate. Read `references/MLFLOW_SOURCE_AUDIT.md` before calling any detail "pixel-identical".

Apply `box-sizing:border-box` globally and the source-derived system font stack. Use a 4px grid; body background `#11171C`; main text `#E8ECF0`; 13px base. No gradients, glass, oversized rounded cards or purple sidebar accents. Topbar/sidebar positioning and panel dimensions are app-level recommendations, not MLflow source claims.

**Buttons:** base 40px high; 13px font; 16px inline padding; radius 4px; secondary border `#37444F`. Small is 32px high. Primary uses DuBois semantic blue `#4299E0`. Exact visuals may differ if a specific vendored component ships additional scoped overrides. Use visible hover, focus, pressed and disabled styles.

**Inputs and selects:** 40px high; 12px inline padding; 1px border; no pill radius. Tabs use compact 13px labels and 4px underlying primitive vertical padding with a subtle active underline. Tables use dark main background and source hover/selection alphas.

**Charts:** retain analytical requirements in §7 but keep plots neutral and rectangular. Align borders and axis typography with the same DuBois palette; do not use an unrelated plotting-library theme. Colorful plots must not recolour the UI chrome.

**Final validation:** take screenshots at 1440×900 and compare at equal scale against a screenshot of the targeted MLflow release. Review typeface, topbar/sidebar colours, selected states, icon glyphs, buttons, tabs, input borders, table density and plot details. This is necessary to claim pixel fidelity. With no reference screenshot available, report "source-grounded MLflow style" rather than "exact replica".

## 13. Agent procedure: how to apply the skill autonomously

Before coding:

1. Identify the user's analytical objective and the real data inputs/outputs. Establish the central question each screen answers, not just a list of widgets.
2. Inspect project structure, existing UI, styles, dependencies, charts, state management and business logic. Preserve existing useful functionality and existing tests.
3. Select an appropriate full-page blueprint from `references/PAGE_BLUEPRINTS.md` (and consult §8 for domain details). Decide whether a sidebar helps; do not force it into a single-page fitting tool. Read `references/CONTROL_SELECTION.md` before choosing a selector or tab.
4. Define data model: runs, strategies, metrics, timestamps, units, sample identifiers, missing values, groups and benchmarks. Ensure correct transformations and no leakage.
5. Create or reuse design tokens, AppShell, consistent chart panel, toolbar and data table. Design the **most data-dense screen first**; visual consistency elsewhere follows it.

During implementation:

6. Implement all controls and chart interactions using actual application state. Use truthful sample/mock data only when explicitly developing a demonstration and label it `Demo data`; never silently replace real data with generated numbers.
7. Make chart axis labels, units, tooltip formatters, legends, empty states and panel heights consistent. This step is mandatory, not polish.
8. Check at desktop 1440×900 and 1920×1080, plus a narrower ~1100px viewport. Check chart resizing, long identifiers, large numbers, no results, dense tables, missing values, and a 6+ run comparison.
9. Verify accessibility, focus states, contrast, keyboard access, and that selection, filters, sorting and charts all agree.
10. Run project lint/build/tests when available. Test the seven scenarios in `references/CONTROL_SELECTION.md` (model choice, multi-run comparison, tab switching, date filter, error, keyboard and overflow). Report what was tested and what remains unverified; don't claim visual QA if you cannot render or inspect the application.

When user's request is purely to restyle an existing app, do not alter research calculations, model inputs, or data transformation logic except to fix explicitly requested defects. Avoid unrelated functional scope expansion.

## 14. Final UI self-review — fix failures before declaring done

- [ ] Looks recognisably like the MLflow dark UI (not merely another quiet dark dashboard); fidelity limitations are documented.
- [ ] Component choice follows `references/CONTROL_SELECTION.md` (tabs for views, dropdown/segmented for single-model choice, checkboxes for comparisons).
- [ ] Every component used follows its source/adaptation spec in `references/COMPONENT_CATALOG.md`.
- [ ] Layout comes from one of the five full reference blueprints unless there is a documented reason to depart.
- [ ] Topbar, sidebar (if needed), tabs, toolbar, panels, table and charts use the source DuBois dark palette.
- [ ] Colour used sparsely, primarily for active controls, series identity and genuine statuses.
- [ ] At 1440px there is useful visible analysis above the fold; no oversized header or blank space.
- [ ] Every figure has a meaningful unit, axis, date range and metric definition as applicable.
- [ ] No chart silently smooths/aggregates/truncates or renders missing data as zero.
- [ ] Series colour mapping is stable everywhere; legend toggling and selection work.
- [ ] Numerical columns line up, sorting/filtering work, and table rows are compact.
- [ ] Side panels and charts resize correctly; panel labels never collide or clip.
- [ ] Actions really work; empty, loading, error and partial-data states are handled.
- [ ] Text remains readable; keyboard focus is visible; every icon-only control is labelled.
- [ ] Does not depend on live web searches, image URLs, remote fonts or unavailable UI assets; any missing exact icon assets are labelled as a fidelity gap.

**Design directive to remember:** The reference is MLflow's **actual** dark experiment tracking style. The target is a faithful, data-dense **quantitative research workbench** with minimal visual noise. If uncertain, match the source design language first, then choose the simplest layout consistent with correct analytical data.
