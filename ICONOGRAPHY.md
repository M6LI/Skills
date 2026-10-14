# Iconography: source vocabulary and offline fallback

The MLflow vendored DuBois package contains declarations for hundreds of generated icon exports in `dist-types/design-system/Icon/__generated/icons/`. This skill does **not** ship that original artwork or claim the icons have been pixel-reproduced. The table below records actual names observed in the public MLflow vendored declarations and recommends their use by meaning.

**Preferred order:** (1) import the legitimately available DuBois icon from local dependencies, (2) use a project-local already licensed matching asset, (3) create a simple documented local inline SVG fallback if necessary. Do not use emoji; do not label Lucide icons "DuBois"; do not fetch icons at runtime. If original paths are unavailable, explicitly report reduced icon fidelity when asked.

| Application use | DuBois export name | Semantics / location |
|---|---|---|
| Search | `SearchIcon` | Search input leading icon |
| Filter | `FilterIcon` | Filter trigger |
| Sort ascending | `SortAscendingIcon` | Table sorted column |
| Sort descending | `SortDescendingIcon` | Table sorted column |
| Sort unset | `SortUnsortedIcon` | Unsorted column header |
| Download | `DownloadIcon` | Chart data/export |
| Add entity | `PlusIcon` | Add run, configuration |
| Remove/close | `CloseIcon`, `CloseSmallIcon` | Chips, drawers, overlays |
| Chevron down | `ChevronDownIcon` | Dropdowns and disclosures |
| Chevron right | `ChevronRightIcon` | Tree, collapsed section |
| Chevron left | `ChevronLeftIcon` | Previous/navigation |
| Trend plot | `ChartLineIcon` | Signals/metric plot view |
| Bar plot | `BarChartIcon` | Aggregated metric view |
| Table | `TableIcon` | Run list/table mode |
| Folder | `FolderIcon` | Experiment hierarchy |
| Execute job | `RunIcon`, `PlayIcon` | New compute, start job |
| Pause job | `PauseIcon` | Pause if backend supports |
| Date | `CalendarIcon` | Date-range selector |
| Clock | `ClockIcon` | Duration/time |
| Confirmation | `CheckIcon` | Checked state if needed |
| Warning | `WarningIcon` | Real risk/validation issue |
| Information | `InfoIcon` | Tooltip/definitions |
| Copy | `CopyIcon` | Run ID, configuration hash |
| Delete | `TrashIcon` | Destructive action |
| Expand | `ExpandMoreIcon` | Expand disclosure |
| Fit to view | `ZoomToFitIcon` | Plot zoom reset |
| Zoom in/out | `ZoomInIcon`, `ZoomOutIcon` | Chart controls |
| Files/artifacts | `FileDocumentIcon`, `FileCodeIcon` | Artifacts/metadata |
| Group/model | `FileModelIcon` | Saved model artifact |
| Success | `CheckCircleIcon` | Completed run |
| Error | `XCircleIcon` | Failed run |
| Upload | `UploadIcon` | Import data |
| Time range | `CalendarRangeIcon` | Date-range filter |
| Checklist | `ChecklistIcon` | Run selection workflows |
| Linear plot | `LinearLineIcon` | Model line/linear trend |

**Geometry recommendation (not verified source icon box sizes):** 16×16px common control glyph; 14px in compact rows and 18px in page-level actions, stroke roughly 1.5–2px if implementing local fallback. Always use consistent size/alignment, treat path stroke/fill per actual original when provided, and do not recolour multicolour artwork arbitrarily. Neutral colour `#92A4B3`; hover `#8ACAFF`; destructive icon `#E65B77` only for destructive actions.

**Accessibility:** icon-only buttons need `aria-label` and a focus ring; decorative icons beside an equivalent text label use `aria-hidden=true`. Tooltip cannot be the sole accessible name. Text labels and icon semantics must not conflict.
