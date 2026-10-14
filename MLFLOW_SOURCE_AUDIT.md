# MLflow fidelity source audit — 14 October 2026 (v3)

This file records what the skill can truthfully reproduce offline, and what requires a screenshot comparison or the actual vendored component implementation. It is **not** a claim that all applications built with this skill are pixel-perfect reproductions.

## Source repository

- MLflow frontend instructions: https://github.com/mlflow/mlflow/blob/master/mlflow/server/js/CLAUDE.md
- MLflow app dependencies: https://github.com/mlflow/mlflow/blob/master/mlflow/server/js/package.json
- DuBois vendored package: https://github.com/mlflow/mlflow/blob/master/mlflow/server/js/vendor/design-system/package.json
- DuBois token LESS: https://github.com/mlflow/mlflow/blob/master/mlflow/server/js/vendor/design-system/dist/dubois-colors.less
- DuBois dark stylesheet: https://github.com/mlflow/mlflow/blob/master/mlflow/server/js/vendor/design-system/dist/index-dark.css
- MLflow open source project: https://github.com/mlflow/mlflow

All paths point to `master`, which changes over time. For strict reproducibility, pin a Git commit SHA and verify the screenshots for the intended version. Exact original proprietary branding should not be used in independent work apps.

## Source-grounded findings

1. MLflow frontend code uses `@databricks/design-system` as its component family and instructs developers not to invent UI components when DuBois ones exist.
2. `package.json` resolves `@databricks/design-system` via `file:./vendor/design-system`, not necessarily via a public npm package.
3. Verified DuBois dark colour values (source `dubois-colors.less`): `backgroundPrimary #11171C`, `backgroundSecondary #1F272D`, `textPrimary #E8ECF0`, `textSecondary #92A4B3`, `actionPrimaryBackgroundDefault #4299E0`, `actionPrimaryBackgroundHover #8ACAFF`, `actionDefaultBorderDefault #37444F`, status colours `#3BA65E` / `#DE7921` / `#E65B77`; row hover and select alphas given in the tokens CSS.
4. Vendored compiled dark CSS includes `.du-bois-dark-btn` at 40px high, 16px inline padding, 13px font, 4px radius; `.du-bois-dark-btn-sm` at 32px high, 16px inline padding, 13px font, 4px radius. CSS also contains legacy button-specific colours that can differ from semantic theme tokens; actual shipped component takes precedence.
5. Compiled dark CSS shows single-select selector height 40px, horizontal padding 12px, and tab primitive 13px font with `4px 0` padding.
6. Compiled dark CSS includes system UI font stack headed `-apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, 'Helvetica Neue', Arial, 'Noto Sans'` in select text fields. Applying that app-wide is a high-fidelity approximation unless verified against the specific MLflow page.
7. The vendored TypeScript icon declarations list 451 generated icon names. Selected verified exports: `SearchIcon`, `FilterIcon`, `DownloadIcon`, `PlusIcon`, `CloseIcon`, `ChevronDownIcon`, `ChevronRightIcon`, `ChartLineIcon`, `BarChartIcon`, `TableIcon`, `FolderIcon`, `RunIcon`, `PlayIcon`, `PauseIcon`, `ClockIcon`, `CalendarIcon`, `CheckIcon`, `WarningIcon`, `InfoIcon`, `CopyIcon`, `TrashIcon`.

## Fidelity boundaries

- Dark source colour tokens: **verified**.
- Some base button, select and tabs rules: **verified at CSS source level**. Actual resolved CSS cascade may differ.
- App-wide font face and weight: **partially verified**; system stack observed in vendor CSS, not a computed browser screenshot.
- DuBois icon names: **verified**, but this package does not redistribute the actual SVG path data. `lucide-react` will not match precisely.
- MLflow navbar/sidebar widths, row heights, per-page layout and chart margins: **adaptive recommendations**. No single screenshot/version has been pixel-audited.
- Whole-app 'identical MLflow': **not guaranteed** until original bundled components or exact paths are used and screenshot comparisons pass at common viewports.

## Offline agent protocol

1. Check the local repository for `@databricks/design-system` or licensed vendored DuBois assets. If present, use the matching version directly.
2. If not present, import `assets/tokens.css` and follow the MLflow source-grounded rules above.
3. For icons, prefer actual DuBois assets from local dependencies. If absent, use simple local SVG stand-ins and mark those differences, rather than making a false exact-match claim.
4. Prefer the supplied theme colours to changing tokens based on taste. Preserve dark mode.
5. Compare screen captures side-by-side when possible. Record unresolved differences: fonts, icon paths, component border/radius, row heights, chart spacing.
6. Don't claim all metrics/plots resemble MLflow solely because the chrome does; domain-specific plotting remains the application's own responsibility.

## Additions in v3

The following new resources are **implementation standards written for quant analytics**, not materials copied from the Databricks or MLflow design system:

- `references/COMPONENT_CATALOG.md`: 35 component contracts, including model selectors, tabs, comparison baskets, chart panels and state/keyboard rules. Each source-derived measurement is identified as SOURCE; others are ADAPTATION.
- `references/CONTROL_SELECTION.md`: semantic decision rules and an execution/test contract for tabs vs segmented controls vs single-select vs checkboxes vs numeric editors.
- `references/PAGE_BLUEPRINTS.md`: five complete quantified page compositions and viewport constraints.
- `references/ICONOGRAPHY.md`: verified DuBois icon **export names** and fallback policy; it does not contain original SVG artwork.
- `assets/components.css`: independent `qr-*` component implementation conforming to this specification, **not** original DuBois CSS.
- `assets/component-gallery.html`: illustrative standalone interactive sample. Metrics are labelled as synthetic examples; no proprietary MLflow font or icon binaries are embedded.

`assets/tokens.css` contains the earlier v2 `quant-*` helper CSS for compatibility. For new code prefer `assets/components.css` and its `.qr-app`-scoped `qr-*` classes. Load `tokens.css` first, then `components.css`.

The screenshots consulted when considering general page organisation came from MLflow tracking documentation and historical MLflow UI demonstrations. The page blueprints are new quant-analytics compositions: they are not asserted to be exact replicas of those screenshots, nor do the 2026-vendored theme tokens guarantee rendering parity with older MLflow releases (e.g. v2.14). For actual pixel parity, pick a pinned MLflow release, capture its computed CSS/layout and legally reuse or reproduce each component and SVG asset.
