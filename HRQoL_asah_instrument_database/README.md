# Health-Related Quality of Life aSAH Measurement Instrument Database — prototype

- `data/instruments.csv` contains the instrument records.
- `data/fields.csv` controls which fields appear in the main table, pop-up, filters, and search.
- `images/` contains the replaceable team logo and optional instrument graphics.
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






## Prototype disclaimer

The included instrument records have been identified as potential candidate instruments for measuring the aSAH Core Domain: Health-Related Quality of Life. They should not be treated as the definitive aSAH instrument inventory or as COS endorsements.

