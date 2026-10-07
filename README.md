# Health-Related Quality of Life aSAH Measurement Instrument Database — prototype

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

## Prototype disclaimer

The included instrument records have been identified as potential candidate instruments for measuring the aSAH Core Domain: Health-Related Quality of Life. They should not be treated as the definitive aSAH instrument inventory or as COS endorsements.

