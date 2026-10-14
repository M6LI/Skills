# Component catalogue — Quant Research UI v3

Read with `../SKILL.md`, `CONTROL_SELECTION.md`, and `../assets/components.css`. This is an **implementation contract** for an offline agent, not a screenshot analysis. The words **SOURCE** and **ADAPTATION** carry different meanings:

- **SOURCE** = found in MLflow's vendored DuBois tokens or compiled CSS (paths in `MLFLOW_SOURCE_AUDIT.md`). Source styles are not necessarily the final computed cascade on every MLflow page/version.
- **ADAPTATION** = our specified, deterministic default for quant tools. Follow it unless the application has a demonstrable workflow constraint. Do not represent adaptations as source measurements.
- All measurements use CSS pixels at 100% zoom. `--qr-*` tokens and `qr-*` classes implement adaptations; the `--dubois-*` tokens preserve source colour values.
- Every clickable control has a working effect. Use native semantics, keyboard focus, and text alternatives for icons. Never implement an interactive element as a clickable `div` without keyboard behaviour.

## Shared component primitives

**SOURCE palette:** foreground `#E8ECF0`, secondary text `#92A4B3`, dark primary `#11171C`, dark secondary `#1F272D`, outlined-control border `#37444F`, primary action blue `#4299E0`, hover blue `#8ACAFF`, selected row `rgba(189,205,219,.08)`. See `assets/tokens.css` for all tokens.

**SOURCE typography:** vendored CSS references UI system font stack beginning `-apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, 'Helvetica Neue', Arial, 'Noto Sans', sans-serif`; regular button 13px/20px; `du-bois-dark-tabs-tab` 13px. Use 13px regular body as an adaptation rather than asserting the font size is invariant throughout MLflow. Use tabular figures in analytical cells.

**SOURCE dimensions:** regular button 40px high with 16px inline padding and 4px radius; small button 32px high with 16px inline padding and 4px radius. DuBois compiled single-select selector 40px high with 12px inline padding. Tabs primitive 13px text with `4px 0` inner padding. Some CSS rules are legacy and theme tokens may differ from raw `.du-bois-dark-btn-primary` CSS colours; semantic tokens win for our reproduction.

**ADAPTATION grid:** 4px base spacing. Common spaces 4/8/12/16/24. Control label above field 8px. 1px borders. Primary layout uses hard dividers and compact surfaces, not heavy elevation. Default icon size 16px, icon+label gap 8px, control row gap 8px, group gap 12–16px. Focus ring visible, 2px `#8ACAFF` outline with 2px offset. If a field lacks visible label, supply `aria-label`.

## C01 — Application frame / top navigation

- **Anatomy:** top navigation (brand-neutral app title, two to five top-level areas, right-aligned context/help), optional left rail, central work surface. No MLflow name/logo.
- **ADAPTATION dimensions:** topbar 48px, left navigation 224px default (192–256 allowed), main content padding 16px vertical, 24px horizontal. Compact header row 48–56px.
- **Behaviour:** top-level navigation changes the location/view. Active item is labelled and visually distinct. Side rail has independent scrolling if list is long. Preserve useful central width.
- **Use:** pages with experiments, datasets and backtests or at least three destinations. **Avoid:** forcing a sidebar onto a single curve-fitting widget.

## C02 — Breadcrumbs

- **Anatomy:** concise ancestors separated by neutral chevrons; only ancestors interactive; current entity as final segment.
- **ADAPTATION:** 12px, line-height 18px, 8px item gap; secondary text by default. Up to 3 visible ancestors; overflow into menu when deep.
- **Behaviour:** navigate back to list with prior filters/selection retained. Never substitute breadcrumbs for tabs.

## C03 — Page header and metadata line

- **Anatomy:** title 20px/28px semibold; one optional 12px muted metadata row (date, dataset, version, cost basis), trailing primary action if relevant.
- **ADAPTATION:** top and bottom spacing 12/16px; do not exceed 64–88px for most pages.
- **Rule:** title identifies **what** is being analysed; metadata identifies **which data and configuration**. No giant hero headings or marketing taglines.

## C04 — Command/filter toolbar

- **Anatomy:** left scope controls (`Experiment`, `Model`, `Universe`, `Date range`), middle search/filter, right compare/export/refresh; grouped with 8–12px gaps and occasional thin separator.
- **ADAPTATION:** 48px min-height, controls usually 32px when toolbar is dense, expand search input, break to second row before clipping at 1100px; pinned only where genuinely helpful.
- **Behaviour:** filters are reflected in query state; changing scope invalidates dependent selections safely; visible chips summarise nondefault filters. Distinguish immediate selection from costly `Run` action.
- **Do not:** scatter unrelated controls above every chart or repeat one global filter within six panels.

## C05 — Text button: primary, default, tertiary, destructive

- **SOURCE:** standard `40px` high, `13px` text, `0 16px` padding, `4px` radius; small `32px` high. `20px` line-height; regular weight 400. Colour tokens as documented.
- **ADAPTATION anatomy:** optional leading 16px icon, 8px gap, 1px border. `primary`: blue fill/dark label. `default`: transparent fill/outlined border. `tertiary`: no persistent border; blue hover text. `danger`: red text/border for destructive actions.
- **States:** default, hover, pressed, focus-visible, disabled, loading. Disabled uses `#37444F` fill and `#5F7281` text; loading retains button width with spinner, preventing layout shift.
- **Rule:** only **one visible primary action per action group** (e.g. `Run backtest` or `Compare 3`). Use default for exports, refresh and settings. Confirm destructive bulk actions.

## C06 — Icon button / split button / overflow

- **ADAPTATION:** 32×32px in toolbar, 40×40px regular; icon 16px; 4px radius; `aria-label`, tooltip, disabled logic. Hover uses quiet blue tint.
- **Split button:** main action + adjacent caret opens strictly related variants (`Export CSV` / `Export PNG`), not unrelated commands.
- **Overflow:** 32px ellipsis icon opens labelled menu, keyed `Escape` dismissal and click-away. Do not hide frequent primary actions in overflow.

## C07 — Tabs: navigation between analytical views

- **SOURCE:** compiled DuBois tab primitive font 13px and padding `4px 0`; final bar layout varies by page.
- **ADAPTATION:** 20–24px inter-tab gap; tabs live on one bottom rule; 2px blue underline for current tab; 8px vertical final target padding (application choice); inactive text `#92A4B3`; active text `#E8ECF0`. Tab label optional 12px count only where useful.
- **Use:** `Overview / Performance / Diagnostics / Parameters / Artifacts`. Same entity, different category of information. Keep tab identity in URL if possible.
- **Avoid:** tabs for selecting a single model from 20; pill/fill backgrounds for page navigation; nested tab bars at the same visual level.
- **Keyboard:** role `tablist`, `tab`, `tabpanel`; roving arrow focus (left/right), `Home`/`End`, appropriate activation policy and `aria-controls`/`aria-selected`.

## C08 — Segmented control: short mutually exclusive option

- **ADAPTATION (not claimed as literal MLflow):** a joined sequence of 32px controls, 1px bordered, 4px radius only on outer corners, 13px text, 10–12px horizontal padding; selected segment receives `rgba(189,205,219,.12)` with brighter label and subtle bottom/inset indication, **not** filled bright-blue pills.
- **Use:** exactly 2–4 stable options and a very frequent toggle (`Absolute / Relative`, `Gross / Net`, `Table / Chart` when it is truly one layout choice).
- **Avoid:** choosing among >4 model families; a selection that must show long descriptions; multiselect; using both segmented and tabs for the same operation.
- **Keyboard:** use a radiogroup with arrow-key movement or a properly documented single-select button group; never leave multiple selected.

## C09 — Single-select dropdown

- **SOURCE:** dropdown selector 40px tall, 12px inline padding.
- **ADAPTATION:** 32px compact variant (documented as ours), 4px radius, border `#37444F`, 16px chevron, max menu 320px viewport height, searchable after roughly 8–10 options; menu anchored to trigger and minimum same width; active item blue tint and tick.
- **Use:** choice of exactly one among 5+ values, dynamic or long lists, `Model family`, `Target`, `Metric`, `Benchmark`.
- **States:** empty placeholder, selected, open, keyboard-highlighted, disabled, loading; unambiguous label. Search result `No models match` is not an empty application state.
- **Keyboard:** native select or accessible combobox/listbox with up/down, Enter, Escape, `aria-expanded`, `aria-activedescendant` where applicable.

## C10 — Multi-select / searchable entity picker

- **ADAPTATION:** base 40px field; selected values render up to 2 compact inline tokens then `+N`; popover of checkboxes with search and selected count; `Clear` explicit, `Apply` only if option set costly.
- **Use:** several sectors, universes, signals, metrics or models when the set is large. For 2–8 commonly compared runs prefer a persistent selection list instead.
- **Do not:** put 25 individual chips into a one-line toolbar; rely solely on colour to indicate selected series; lose checkboxes on scroll.

## C11 — Model choice (single model)

- **Decision, not a visual component by itself:** choose C08 for 2–4 immutable model variants only when instant frequent toggling matters; otherwise C09 for one model. If the user needs to see detailed model health/metadata while choosing, use the C12 model browser.
- **Anatomy:** visible label `Model`, selected value `Ridge`, trailing caret; optional 12px metadata beneath such as `v3 · trained 2026-05-01`; do not insert long identifiers into short trigger.
- **Dependent parameters:** on choosing `Ridge`, show `Regularisation α` in parameter section; `XGBoost` may show `Max depth`, `Learning rate`, `Trees`. Preserve compatible cross-model settings, reset incompatible ones explicitly; no invisible stale inputs.
- **Rule:** *model choice is not model comparison*. Never represent a multiselection task using a mutually exclusive dropdown.

## C12 — Model/run browser and comparison selector

- **Anatomy:** searchable selectable run rows with checkbox, 8px stable-colour swatch, run name, status, optional compact metric. Row min-height 36–40px, 8–12px padding. Selected run details in side pane.
- **Use:** compare 2–6 active runs or choose among many versions with important metadata. Selection tray reads `3 runs selected · Compare` and lists selected names when collapsed.
- **Behaviour:** preserve colour mapping across plots and tables; if model removed from selection, remove its series; when no run selected, explain next action. `Compare` disabled when <2 compatible runs. Show incompatibility reasons rather than hiding valid options.

## C13 — Search and query/filter field

- **ADAPTATION:** 32px toolbar size / 40px standalone; 16px search glyph with 12px leading padding; clear button appears if populated; input border `#37444F`, 4px radius.
- **Use:** search run name, ID, tag, instrument, model, saved experiment. Label `Search runs` or provide equivalent accessible name.
- **Behaviour:** debounce remote queries ~150–300ms if needed; local filtering immediate; preserve query when switching from chart to table if scope persists; show active result count.

## C14 — Numeric / scientific input

- **ADAPTATION:** 40px full, 32px compact; text-aligned right, tabular numerals; inline unit suffix if fixed (`bps`, `%`, `days`, `λ`). Explicit scientific notation display when useful; min/max/step enforced with visible validation.
- **Use:** ridge alpha, half-life, risk aversion, holding period, threshold. Never use a slider as the only input for precise numerical research.
- **Behaviour:** editing string value separate from committed parsed number; do not clamp partially typed value on every keystroke; validate on blur/apply. `0`, negative numbers and exponent notation only when valid. Locale-safe parsing and stated units.

## C15 — Slider paired with numeric input

- **ADAPTATION:** 4px rail, 14px handle, muted track and blue progress, min/median/max ticks sparingly; place 80–104px numeric box on right. Entire row min-height 40px.
- **Use:** exploratory smooth parameters with meaningful bounded range (e.g. risk aversion). Supply numeric direct entry. Show scale explicitly (linear or log); transform between view position and model parameter without silent rounding.
- **Avoid:** selecting a model, dates, or large discrete categories; sensitive values that need more precision than a slider can express.

## C16 — Checkbox / selection checkbox

- **ADAPTATION:** 14–16px box, 3px radius, 1px border, blue fill/tick when checked, full 32px row target. `Indeterminate` reflects partial selected subsets.
- **Use:** independent Boolean choices (`Show baseline`), table multi-row selection, inclusion of multiple models/signals. **Do not** use a radio visual for a multiselect.
- **Behaviour:** clicking label toggles; selection count correct; shift-select row range only when implemented explicitly.

## C17 — Switch (immediate persistent Boolean)

- **ADAPTATION:** 28×16px track, 12px thumb, on state blue; always show text label. Inline description if behavior non-obvious.
- **Use:** immediate view state `Show confidence interval`, `Include costs` *when instantaneous and reversible*. Not suitable for irreversible jobs or ambiguous mutually exclusive options.
- **Note:** model inputs affecting expensive training should be saved or applied explicitly, not silently recomputed on every switch.

## C18 — Radio group

- **ADAPTATION:** 14px circle, 32px row target, 8px label gap; selected ring blue; distinct group label.
- **Use:** 2–4 mutually exclusive options where descriptive text is essential and equal prominence warranted, e.g. `Risk basis: Spread duration / Market value / DV01`.
- **Avoid:** duplicating a segmented choice, and using a radio group for 30 models.

## C19 — Date/time range picker

- **ADAPTATION:** 40px, calendar glyph, display `01 May 2024 – 30 Jun 2025` (unambiguous), dropdown calendar panel 320–640px; quick presets compact and optional.
- **Use:** backtest and train/validation/test filters. Label each period. Highlight selected interval, make exclusive endpoints explicit in data query (inclusive/exclusive) even if UI uses human dates.
- **Behaviour:** no inverted ranges; timezone specified if timestamp relevant; show unavailable dates disabled; repeated date constraints share current state.

## C20 — Filter chips / applied-state summary

- **ADAPTATION:** low-height 24px rectangular tags, 4px radius, muted fill, optional close glyph; summary strip 8px row gap.
- **Use:** supplementary display of active filters (`EM`, `IG`, `Net`, `2025`) below query bar. Can remove filters individually.
- **Avoid:** replacing accessible labelled filter controls with unstructured chips or letting filters silently remain applied.

## C21 — Dense analytical table

- **SOURCE:** semantic row hover `rgba(189,205,219,.04)`; selected `rgba(189,205,219,.08)` and hover-selected `.12`. No source guarantee of row height.
- **ADAPTATION:** header 36px, body 36–40px, 12px horizontal cell padding, muted 12px semibold header; 1px horizontal dividers; numeric values right aligned and tabular. Table may sticky-header, horizontally scroll and pin identifying columns.
- **Features:** explicit sort indicator and stable sorting; filter menu on relevant columns, virtualisation for huge runs, checkbox selection; show units in headers (`OOS Sharpe`, `Net P&L (£)`, `IC (20d)`). Tooltips for truncated IDs. Be precise about null/NaN/missing (em dash, tooltip), zero and negative values.
- **Avoid:** zebra stripes unless actual information density needs them, giant padded rows, heatmap fill on every cell, ambiguous unnamed percent numbers.

## C22 — Compare basket / baseline chooser

- **ADAPTATION:** 36–44px compact bar near table or run list reading `3 selected` + coloured swatches + `Compare 3` button; an adjacent default-select may choose a benchmark/baseline.
- **Behaviour:** stable selection across pagination and filter changes (with visible count); require compatible target and date units to overlay. If datasets differ, show warning and alignment policy.
- **Use:** compare multiple backtest strategies, signals, models or hyperparameter runs. **Not** a page-level navigation tab.

## C23 — Metric strip / key statistics

- **ADAPTATION:** 3–6 values inline with 1px vertical separators; 11px label, 18–22px tabular value, 11px context, 12–16px internal padding. Responsive wrap to 2 columns if needed.
- **Use:** top-line decision metrics (Sharpe, net P&L, turnover, hit rate, max drawdown), not more than six and no fabricated figures.
- **Avoid:** oversized cards, sparkles, arrows whose sign semantics aren't defined, mixing return and profit units.

## C24 — Chart panel

- **Anatomy:** 40px compact header (title, info tooltip, right `⋯` action), central plot with axes, optional footnote. Chart border 1px `--qr-border-subtle`, background `--qr-surface`, radius 4px. Panel may span full workspace.
- **ADAPTATION:** panel min-height 280px for key time series; 220px for auxiliary diagnostics; chart top padding 12, plot margins sufficient for axis labels and legends. No decorative shadow.
- **Behaviour:** loading indicator restricted to chart, not entire page; explicit empty state; missing date segments not connected. Full-screen/details affordance where useful.

## C25 — Chart interaction and legend

- **ADAPTATION:** small neutral series legend below/above plot; 8px circular/line swatch, 12px text, clickable series toggle, tooltip with time+exact values and units. Chart action menu includes `Download image`, `Download data`, `Reset zoom` only when implemented.
- **Behaviour:** stable colour per run/signal; shared crosshair across time plots with same domain; brush zoom, scroll zoom only if predictable, reset zoom button; keyboard equivalent or data table where feasible. No interpolation/smoothing without explicit opt-in.

## C26 — Parameter editor / configuration sections

- **Anatomy:** section header 13px semibold, thin dividing line, labelled field groups 8px vertical gap, 16px section spacing. Optional equation and assumptions collapsed in info/disclosure.
- **ADAPTATION:** left settings panel 272–312px for chart-centric apps; full-width table form for many hyperparameters; sticky bottom row `Reset` / `Apply` / `Run` only if expensive.
- **Rule:** model-specific inputs are conditional on current model; preserve compatible values. Show validity/default and range. **Do not** make a slider-only parameter UI or auto-run expensive backtests on each adjustment.

## C27 — Drawer / inspector panel

- **ADAPTATION:** right drawer width 360–420px, top-aligned below app header, subtle left divider, background secondary; clear title and close; compact `Overview / Parameters / Provenance` sub-tabs when useful.
- **Use:** inspect selected run without losing table context. Retain keyboard focus, allow Escape and return focus to opener; scroll content independently. Avoid drawer for a one-line tooltip.

## C28 — Tooltip / popover / contextual help

- **SOURCE tooltip example:** compiled legacy style uses 4px radius and `6px 8px` padding; one tooltip variant has nonsemantic legacy colour; prefer semantic dark surface/contrast for our own app.
- **ADAPTATION tooltip:** 12px/16px, max-width 320px, 6–8px padding, small shadow if needed. **Popover** may have controls, max-width 360px, 12px padding, border `#37444F`; must be dismissible.
- **Use tooltip:** definitions like `IC = corr(signal_t, future_return_{t+h})`, units, truncated values. **Use popover:** search/filter controls. Don't hide essential context behind hover-only tooltips.

## C29 — Modal / confirmation

- **ADAPTATION:** width 420–600px, radius 4px, background `#1F272D`, header 16px title, divider, 16–24px content padding, 16px gap above footer, right-aligned actions. 45% dark overlay.
- **Use:** destructive confirmation or complex one-off operation. Dismiss with Escape when non-destructive and restore focus; keep `Delete` distinctly dangerous. Prefer inline settings or drawer for everyday model selection.

## C30 — Accordion / collapsible details

- **ADAPTATION:** 36px row header, right chevron, 13px semibold; 1px dividing rule; 12px body padding.
- **Use:** infrequently consulted formulas, run provenance and advanced parameter groups. Keep critical comparisons expanded by default. Respect keyboard/aria-expanded.

## C31 — Status indicator / badge

- **ADAPTATION:** 7px dot + 12px label; success green `#3BA65E`, warning amber `#DE7921`, error rose `#E65B77`, neutral muted. Full text and tooltip for terminal state.
- **Use:** `Completed`, `Running`, `Failed`, `Queued`, `Not evaluated`. Distinguish successful training from successful validation (not the same).
- **Avoid:** huge coloured badges, red/green-only sign encodings; treat time-series negative values as numbers, not necessarily red alerts.

## C32 — Alert / toast / validation

- **ADAPTATION:** thin left accent, subtle tinted background, 12px text, action link inline. Toast max width 360px bottom/right for transient success; inline persistent alert for invalid methodology or stale data.
- **Behaviour:** error explains what failed and how to recover; destructive failures persist and can be copied. `Data covers only 62% of requested universe` belongs near relevant plot, not buried in global toast.

## C33 — Pagination / load more / virtualised lists

- **ADAPTATION:** 32px controls, right aligned with `Showing 1–50 of 5,412 runs`; page index + page size when feasible. Virtualised rows retain sorting/filter semantics and keyboard selection.
- **Rule:** never silently truncate data; make data completeness visible. Preserve selections across pages.

## C34 — Loading, empty, no-results, error, stale states

- **Loading:** skeleton in exact panel footprint, no layout jank; button-level spinner for actions. **Empty:** state specifically what is missing and next action (`Select at least one run`). **No results:** retain query and offer clear-filter. **Error:** exact failure and retry. **Stale:** last-updated timestamp and manual refresh when relevant.
- **ADAPTATION:** one-line muted empty-state heading plus 1–2 sentence guidance; no decorative illustrations.

## C35 — Export / reproducibility controls

- **ADAPTATION:** export via outlined button in top toolbar or chart `⋯`, with options CSV, JSON, SVG, PNG as relevant. Always include plot series units and date filter in exports.
- **Rule:** users should be able to recover which experiment, training interval, model/commit, universe, currency, metric formula, date range and costs produced a result. Don't label a file `final_results` if there are multiple versions.

## Visual QA per component

For every component used, verify: geometry (height/padding/radius), source vs adapted token, default/hover/focus/disabled states, keyboard semantics, label, empty/error and overflow handling, action wiring, chart/table cross-filtering, date/units consistency, and 1280×800 + 1440×900 + narrow 1024×768 view. Do not mark fidelity complete if using placeholder icons, a default third-party theme, fake buttons or inaccessible dropdowns.
