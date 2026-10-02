# Country Admin Divisions

Complete, enriched JSON dataset of countries and first-level administrative divisions (states, provinces, regions, prefectures, etc.) with ISO codes, flag emojis, and stable identifiers.

Built for cascading country → state/province dropdowns, identity/registration forms, and offline use.

**Version:** 1.0.0  
**Last updated:** 2026-09-26  
**Primary file:** [`data.json`](./data.json)

## Contents

| File | Description |
|------|-------------|
| `data.json` | Complete machine-readable dataset |
| `README.md` | This documentation |
| `VALIDATION_REPORT.md` | Coverage counts, validation checks, and known gaps |

## Quick start

```bash
# Load in Node / browser
const data = require('./data.json');
// or
fetch('https://raw.githubusercontent.com/blitz-core/country-admin-divisions/main/data.json')
  .then(r => r.json())
  .then(data => { /* use data.countries */ });
```

```js
// Example: populate country dropdown then regions
const countries = data.countries;
const nigeria = countries.find(c => c.iso2 === 'NG');
console.log(nigeria.flag, nigeria.name);           // 🇳🇬 Nigeria
console.log(nigeria.regions.length);               // 37
console.log(nigeria.regions.find(r => r.id === 'NG-ED')); // Edo
```

## Schema

```json
{
  "metadata": {
    "dataset_name": "Country and Administrative Divisions",
    "version": "1.0.0",
    "last_updated": "YYYY-MM-DD",
    "source": "...",
    "license": "...",
    "description": "...",
    "country_count": 249,
    "region_count": 4387
  },
  "countries": [
    {
      "id": "NG",
      "name": "Nigeria",
      "official_name": "Federal Republic of Nigeria",
      "iso2": "NG",
      "iso3": "NGA",
      "numeric_code": "566",
      "flag": "🇳🇬",
      "phone_code": "+234",
      "capital": "Abuja",
      "currency": {
        "code": "NGN",
        "name": "Nigerian naira",
        "symbol": "₦"
      },
      "administrative_division": {
        "name": "State",
        "name_plural": "States",
        "level": 1
      },
      "regions": [
        {
          "id": "NG-ED",
          "name": "Edo",
          "official_name": "Edo",
          "code": "NG-ED",
          "iso3166_2": "NG-ED",
          "type": "state",
          "level": 1,
          "parent_id": "NG"
        }
      ]
    }
  ]
}
```

### Field reference

| Field | Description |
|-------|-------------|
| `id` / `iso2` | ISO 3166-1 alpha-2 (stable primary key) |
| `iso3` | ISO 3166-1 alpha-3 |
| `numeric_code` | ISO 3166-1 numeric |
| `flag` | Unicode regional-indicator flag emoji derived from ISO alpha-2 |
| `phone_code` | International dialing prefix (E.164 style where available) |
| `capital` | Capital city name |
| `currency` | ISO 4217-oriented code, name, and symbol where available |
| `administrative_division` | Country-specific terminology for first-level divisions |
| `regions[].id` | Stable identifier (`ISO2-shortCode` or generated slug) |
| `regions[].iso3166_2` | ISO 3166-2 style code when a short code existed in the source |
| `regions[].type` | Heuristic administrative type (state, province, prefecture, …) |
| `regions[].parent_id` | Always the country `iso2` |

Optional fields that could not be reliably verified are omitted or set to `null`. Values are never fabricated.

## Coverage

| Metric | Count |
|--------|------:|
| Countries / territories | 249 |
| First-level administrative divisions | 4,387 |
| Regions with short code (ISO 3166-2 style) | ~4,208 |

**Examples**

- **Nigeria** — 37 entries (36 states + Federal Capital Territory)
- **Japan** — 47 prefectures
- **United States** — 62 entries (states + DC + territories as present upstream)
- **United Kingdom** — 217 entries (constituent countries and regions as structured upstream)
- **Canada** — 13 (provinces + territories)
- **Australia** — 8 (states + territories)

## Sources & licenses

### Primary hierarchy (countries + subdivisions)

- **Source:** [country-regions/country-region-data](https://github.com/country-regions/country-region-data)
- **License:** MIT
- **Retrieved:** 2026-09-26
- **Upstream note:** Changelog 4.1.0 (Feb 2026) and earlier regional updates

### Enrichment (ISO codes, flags, capitals, currencies, phone codes)

- ISO 3166-1 codes, names, and flags from public ISO 3166-1 data (community mirrors). Flag emojis are generated from alpha-2 using Unicode regional indicator symbols.
- Capitals, phone codes, and currencies cross-referenced with [dr5hn/countries-states-cities-database](https://github.com/dr5hn/countries-states-cities-database) (ODbL v1.0 — **attribution required** when redistributing that metadata).
- Administrative terminology is a best-effort heuristic for UI labels, not an official legal classification.

### License summary

- Subdivision hierarchy → **MIT** (permissive)
- Capital / phone / currency enrichment from dr5hn → **ODbL** (attribution required)
- Flag emojis → Unicode (no additional copyright)
- Do not claim the underlying geographic lists as original research

**Recommended attribution**

```
Country and subdivision data based on country-region-data (MIT)
https://github.com/country-regions/country-region-data
Enrichment metadata partly derived from Countries States Cities Database (ODbL)
https://github.com/dr5hn/countries-states-cities-database
```

## Design decisions & limitations

1. **First-level only** — Focused on the level needed for country → state/province dropdowns. Lower levels (counties, LGAs, municipalities) are not included.
2. **Source fidelity** — Subdivision names and short codes come from the upstream MIT dataset. Enrichment is limited to verified ISO and metadata fields.
3. **Disputed territories** — Inclusion follows the upstream source (common ISO / addressing practice). Kosovo, Palestine, Taiwan, Western Sahara, etc. appear where present upstream. Apply your own geopolitical policy.
4. **Administrative type labels** — Convenience strings for UI; not authoritative national legal classifications.
5. **Missing short codes** — A minority of regions lack an upstream short code; a stable generated `id` is used and `iso3166_2` is `null`.
6. **No fabrication** — Capitals, currencies, phone codes, and ISO codes are never invented.
7. **Multi-level systems** (e.g. UK, France) — Lists reflect address-oriented first-level choices, not always a pure constitutional hierarchy.
8. **Currency / phone data** — Sourced from a community dataset; re-verify for financial or telecom production use.

## Intended use

- Offline cascading dropdowns (country → region)
- Identity and registration forms
- Static data for edge workers, mobile apps, or embedded systems
- Base layer for public or internal APIs

Not intended as a definitive legal or cartographic authority.

## Update process

1. Re-fetch the upstream `data.json` from country-region-data.
2. Re-run enrichment against current ISO 3166-1 and metadata sources.
3. Bump `metadata.version` and `last_updated`.
4. Validate JSON syntax, unique IDs, and flag correctness.
5. Publish a new release with an updated validation report.

## File integrity

- `data.json` is valid UTF-8 JSON (`JSON.parse` / `json.loads` ready).
- Approximate size: **1.3 MB** (pretty-printed). A minified build can be produced if needed.

## License

See [Sources & licenses](#sources--licenses) above. Upstream geographic data remains the work of the original contributors under their respective licenses.
