# Health-Related Quality of Life (HRQoL) aSAH Measurement Instrument Database — Version 2

> **About this update:** This version updates the **HRQoL Version 1**, incorporating the latest Functional Outcomes interface features. It retains the original HRQoL domain name, data paths, existing instrument-record functionality, and browser viewing-history key. Your existing HRQoL `data/instruments.csv`, `data/fields.csv`, images, and `.nojekyll` remain in place; they are not included in this package. The supplied `badge_styles.csv` is optional.

> **Data-driven tabs:** Domain Match components and Feasibility columns are drawn from your HRQoL `fields.csv` and `instruments.csv`. If your CSVs have no component-level or feasibility fields, the new views show an explanatory empty state until these fields are added. No fictional HRQoL assessments have been inserted.

## Deploying to your HRQoL GitHub repository

1. Replace the existing root `index.html` with the updated HTML in this package.
2. Replace the root `README.md` if you would like the updated configuration guidance.
3. Optionally place the supplied `badge_styles.csv` in the existing `data/` folder to customise the rating colours. **The site does not require this new CSV to load.**
4. **Do not replace** your original `data/instruments.csv`, `data/fields.csv`, `images/` folder, or `.nojekyll` file.

Your existing HRQoL summary table, filters, deep links and record details continue to read their original CSV fields. The new Domain Match and Feasibility views display only fields that exist in your own CSVs: see the `heatmap` and `feasibility` configuration sections below. No invented assessments are included. The app will show an informative empty state for a comparison view until it is configured.

- `data/instruments.csv` contains the instrument records.
- `data/fields.csv` controls which fields appear in the main table, pop-up, filters, and search.
- `images/` contains the SAH HOPE logo and instrument graphics.
- URL fields can render as clickable links with link text configured entirely in `data/fields.csv`.

## Folder structure

```text
index.html
.nojekyll
README.md

data/
  instruments.csv
  fields.csv
  badge_styles.csv     # optional; built-in fallback palette

images/
  team-logo.svg
  example-instrument-graphic.svg
```


| Column | Purpose |
|---|---|
| `field` | Exact column header from `instruments.csv` |
| `label` | Human-readable label shown on the website |
| `table` | `Yes` to show as a main table column |
| `popup` | `Yes` to show in the instrument pop-up |
| `filter` | `Yes` to automatically create a dropdown filter |
| `search` | `Yes` to include this field in free-text search |
| `display_order` | Number controlling left-to-right / top-to-bottom order |
| `format` | How the value is displayed |
| `link_text` | Text displayed for fields using the `url` format; leave blank for other formats |

Supported `format` values:

- `text` — standard text field
- `primary` — primary instrument-name field; automatically shows the acronym beside it
- `badge` — pill-style status display
- `longtext` — full-width text section in the pop-up
- `image` — image displayed near the top of the pop-up
- `url` — safe clickable `http://` or `https://` hyperlink; link text comes from `link_text`
- `hidden` — stored in the CSV but not directly rendered







## Instrument pop-up navigation

The instrument pop-up supports faster browsing through the instruments currently shown by the active search and filters:

- **Sticky footer navigation** keeps `Previous`, the record position (for example, `3 of 14`), and `Next` visible while scrolling through long records.
- **Desktop side arrows** provide an additional visual way to move between records on wider screens. They are hidden on smaller screens to avoid covering content.
- **Keyboard navigation**: while the pop-up is open, use the left and right arrow keys to move to the previous or next instrument.
- Navigation follows the **currently filtered list**, rather than always moving through every instrument in the database.
- Previous/Next controls are automatically disabled at the first and last matching records.

## Enhanced record navigation and deep links

The instrument pop-up includes several navigation aids:

- **Sticky instrument header** keeps the instrument title, `Back to results`, `Copy link`, and close controls visible while scrolling.
- **Back to results** closes the record, restores the prior results-page position, and highlights the originating row to help users re-orient.
- **Viewed markers** are stored only in the visitor's browser. Opened instruments display `✓ Viewed`; `Clear viewed markers` resets this local history.
- **Record jump menu** lists all instruments in the currently filtered/search result set. It uses the full instrument name and adds the acronym in parentheses only when an acronym exists.
- **Keyboard hints** in the footer advertise the available shortcuts: left/right arrows browse records and `Esc` closes the dialog.
- **Direct instrument links** use `?instrument=INSTRUMENT_ID`. Visiting such a URL automatically opens that instrument record. Instrument names in the results table are also real links, so users can copy them or open them in a new tab.
- **Copy link** copies the direct URL for the currently open record.

Viewed status uses browser `localStorage`; it does not require an account or server-side viewing history.


## Full-screen instrument images

Instrument graphics in the pop-up can be opened in a full-screen viewer:

- **Click the image** to enlarge it.
- With the image focused, press **Enter** or **Space** to enlarge it.
- Select the **×** button, click the dark background, or press **Esc** to close the full-screen image.
- Focus returns to the instrument image after the full-screen viewer closes.
- A short `Click image to enlarge` hint is displayed below instrument graphics.

The full-screen viewer uses the existing `image` and `image_alt` values from `data/instruments.csv`; no additional CSV fields are required.


## Configurable badge colours

Badge colours may be controlled by `data/badge_styles.csv`; if that file is absent, the HTML uses built-in defaults. Colours can therefore be changed without editing `index.html`.

The optional file uses four columns:

| Column | Purpose |
|---|---|
| `field` | Exact field name from `instruments.csv`, or `*` for a global rule |
| `value` | Exact category/response value to match |
| `background` | Badge background colour |
| `text` | Badge text colour |

Example:

```csv
field,value,background,text
*,Yes,#e8f5ee,#23613b
*,Partially / with concerns,#fff6dd,#7a5a00
*,No,#fdecec,#9b2c2c
*,Unable to judge,#eef2f6,#475467
status,Retained,#e8f5ee,#23613b
status,Identified,#edf4ff,#285ea8
```

Rules are matched case-insensitively.

- A **field-specific** rule takes priority over a wildcard (`*`) rule.
- A wildcard rule lets the same category use the same colour across multiple assessment columns.
- If no matching rule is found, the HTML uses the built-in palette where applicable, otherwise neutral grey. Extended HRQoL status values beginning with Identified, Retained, or Under retain their original colours.
- A field must still use `format=badge` in `data/fields.csv` for these styles to be applied.

This makes it possible to use consistent traffic-light-style colours for responses such as `Yes`, `Partially / with concerns`, `No`, and `Unable to judge`, while giving fields such as `status` their own colour scheme.


## Badge rationale pop-ups

Any field displayed with `format = badge` can have an optional rationale. The rationale is stored in `data/instruments.csv` using the naming convention:

```text
<badge_field>_rationale
```

For a Domain Match field named `domain_match`, add a column named:

```text
domain_match_rationale
```

Example:

```csv
id,name,domain_match,domain_match_rationale
HRQ001,Example Instrument,Yes,"Directly assesses the intended Health-Related Quality of Life domain."
HRQ002,Another Instrument,Partially / with concerns,"Measures some relevant aspects of quality of life, but does not cover the full intended construct."
```

If `domain_match_rationale` is also listed in `data/fields.csv`, configure it as a hidden field:

```text
field = domain_match_rationale
format = hidden
table = No
popup = No
filter = No
search = No
```

When a badge has a non-empty rationale:

- the badge itself indicates that a rationale is available through hover, keyboard focus, click, or tap;
- **hovering** over the badge shows the rationale;
- **keyboard focus** shows the rationale;
- **clicking or tapping** pins the rationale open for touch-screen users;
- clicking elsewhere closes it;
- `Esc` closes the rationale first while an instrument record is open.

The feature is generic. For example, a badge field named `feasibility` can use `feasibility_rationale`.

Badge colours still come from the optional `data/badge_styles.csv` or the HTML fallback palette, so the rationale feature does not change the configurable colour system.


### Rationale pop-ups inside the instrument window

The rationale viewer now uses the browser's native **Popover API** (`popover="manual"`). The instrument record itself is a modal `<dialog>`, and ordinary page elements can be hidden behind that dialog even with a high `z-index`.

Native popovers are also placed in the browser's **top layer**, so the rationale can appear above an open instrument record as intended. A fallback is included for browsers without Popover API support.

## Domain Match heat map view

The results page now includes a three-option view toggle:

- **Standard view** — preserves the existing configurable summary table and row-to-record navigation.
- **Domain Match** — compares component-level Domain Match ratings across the same instruments currently shown by the active search and filters.
- **Feasibility** — provides a horizontally scrollable comparison table for categorical feasibility ratings and open-text information, with all configured fields shown in one view.

The selected view is saved in the visitor's browser using `localStorage`, so returning to the page restores the last-used view when no view is specified in the URL.

### Shareable links to individual tabs

Each results tab now has its own direct URL. Replace `https://YOUR-SITE-URL/` with your actual GitHub Pages (or other hosted) database address:

- **Standard view:** `https://YOUR-SITE-URL/?view=standard`
- **Domain Match:** `https://YOUR-SITE-URL/?view=domain-match`
- **Feasibility:** `https://YOUR-SITE-URL/?view=feasibility`

Selecting a tab updates the browser address automatically, without reloading the database. To share a tab, copy its URL directly from the browser address bar (there is no separate Copy tab link button). When someone follows a link, the specified tab opens directly even if they previously selected another tab. Browser Back and Forward buttons also switch between previously visited tabs.

Instrument deep links still work: `?view=domain-match&instrument=HRQ001` opens instrument `HRQ001` with Domain Match selected behind the pop-up. For a link to the tab alone, copy the browser URL when no instrument record is open (i.e., when the URL does not contain `instrument=...`). The separate Copy link button inside an instrument record is retained.

URL links do not require CSV edits. Existing bookmarks using `?instrument=...` continue to work, and view-specific links work with the same hosted `index.html` file.

### Configuring heat map columns

The preferred approach is to add two optional columns to `data/fields.csv`:

| Column | Purpose |
|---|---|
| `heatmap` | `Yes` to include the field as a heat map column |
| `heatmap_order` | Optional numeric order for heat map columns; falls back to `display_order` |
| `heatmap_role` | Optional; set to `overall` to visually separate the overall judgement from component ratings |

Example:

```csv
field,label,table,popup,filter,search,display_order,format,link_text,heatmap,heatmap_order,heatmap_role
domain_match_concept,Concept coverage,No,Yes,No,No,40,badge,,Yes,1,
domain_match_population,Applicability,No,Yes,No,No,41,badge,,Yes,2,
domain_match_interpretation,Interpretability,No,Yes,No,No,42,badge,,Yes,3,
domain_match,Overall Domain Match,Yes,Yes,Yes,Yes,20,badge,,Yes,99,overall
```

If no fields are explicitly marked `heatmap=Yes`, the page automatically falls back to badge fields named `domain_match` or beginning with `domain_match_`. Fields ending in `_rationale` are excluded automatically.

### Heat map rationales

Each component can use the same rationale naming convention as the existing badges:

```text
<heatmap_field>_rationale
```

For example:

```text
domain_match_concept
domain_match_concept_rationale
```

When a rationale is available, the coloured heat map cell can be hovered, keyboard-focused, clicked, or tapped to open the existing rationale popover. The cell itself does not require an additional information icon.

### Heat map colours and legend

Heat map cells use the same rules in `data/badge_styles.csv` as ordinary badges. This keeps the colour meaning consistent between the standard table, instrument pop-up, and heat map.

The heat map automatically builds a legend from rating values present in the currently filtered result set. The instrument-name column remains sticky while horizontally scrolling, and the overall Domain Match column is visually separated from the component columns.

### Filtering and instrument records

The heat map is not a separate dataset. It uses `filteredRows()`, so all existing search and dropdown filters apply identically in both views. Selecting an instrument name in the heat map opens the same detailed instrument record, including viewed markers and record navigation.

## Feasibility comparison view

The **Feasibility** tab is designed for wider mixed-format assessments. It uses a sticky instrument-name column, native horizontal scrolling, and dedicated **Scroll left / Scroll right** buttons.

All fields configured for the Feasibility tab appear together, including categorical ratings, scores, comments, and open-text information. There is no Compact/Detailed toggle.

Open-text values display in full directly in their Feasibility cells, regardless of length. Text wraps within the column and preserves line breaks. Taller rows may result when the text is long; the table retains horizontal scrolling.

### Configuring feasibility columns

The preferred approach is to add optional feasibility columns to `data/fields.csv`:

| Column | Purpose |
|---|---|
| `feasibility` | `Yes` to include the field in the Feasibility tab |
| `feasibility_order` | Optional numeric left-to-right order; falls back to `display_order` |
| `feasibility_group` | Optional short group label shown above the column label, such as `Practical implementation`, `Patient considerations`, or `Overall` |
| `feasibility_role` | Optional role. Use `overall` to visually separate an overall judgement/score; use `comment`, `narrative`, `details`, or `open text` for open-text fields |

Example:

```csv
field,label,table,popup,filter,search,display_order,format,link_text,feasibility,feasibility_order,feasibility_group,feasibility_role
feasibility_ease,Ease of use,No,Yes,No,No,50,badge,,Yes,1,Practical implementation,
feasibility_time,Time required,No,Yes,No,No,51,badge,,Yes,2,Practical implementation,
feasibility_equipment,Equipment,No,Yes,No,No,52,badge,,Yes,3,Practical implementation,
feasibility_training,Training,No,Yes,No,No,53,badge,,Yes,4,Practical implementation,
feasibility_cost,Cost,No,Yes,No,No,54,badge,,Yes,5,Practical implementation,
feasibility_burden,Patient burden / risk,No,Yes,No,No,55,badge,,Yes,6,Patient considerations,
feasibility_overall,Overall feasibility,No,Yes,No,No,56,badge,,Yes,90,Overall,overall
feasibility_score,Overall score,No,Yes,No,No,57,text,,Yes,91,Overall,overall
feasibility_comments,Comments,No,Yes,No,Yes,58,longtext,,Yes,100,Overall,comment
```

If no fields are explicitly marked `feasibility=Yes`, the page automatically falls back to configured fields named `feasibility` or beginning with `feasibility_`. Fields ending in `_rationale` are excluded.

### Categorical ratings and rationales

The Feasibility legend is fixed, regardless of the ratings present in the filtered instruments:

- **0 Green:** practical advantage / manageable requirement
- **1 Amber:** additional requirement or concern
- **2 Red:** documented substantial obstacle
- **U White:** missing/conflicting information prevents assessment

The optional `data/badge_styles.csv` gives feasibility ratings these matching colours. For older HRQoL repositories without `data/badge_styles.csv`, the HTML uses the same default colours. You can upload the supplied badge styles file to override or extend them. Keep your own `data/instruments.csv` and `data/fields.csv` files.

Fields using `format=badge` use the same colour rules from `data/badge_styles.csv` as the Standard and Domain Match views. A matching `<field>_rationale` column continues to provide hover, keyboard-focus, click, and tap rationales without adding an information icon.

### Open-text fields

Open text can use `format=text` or `format=longtext`. All fields marked `feasibility=Yes` appear in the Feasibility tab. There is no character limit or truncation in the Feasibility comparison table: all stored text is shown inline with wrapping and preserved line breaks. For CSV data containing commas or line breaks, enclose the value in double quotes.

Older `fields.csv` files may still have `feasibility_mode` or `feasibility_detail` columns. These are now ignored; you do not need to remove them from the CSV. To hide an item from the Feasibility tab, change its `feasibility` setting to `No` (when other feasibility fields are explicitly selected).

## Prototype disclaimer

The instrument records in your existing HRQoL repository have been identified as potential candidate instruments for measuring the aSAH Core Domain: Health-Related Quality of Life. They should not be treated as the definitive aSAH instrument inventory or as COS endorsements.

