# Validation Report — Country Admin Divisions v1.0.0

**Retrieval / generation date:** 2026-09-26  
**Repository:** blitz-core/country-admin-divisions  
**Primary output:** `data.json`

## 1. Totals

| Metric | Value |
|--------|------:|
| Countries / territories | **249** |
| First-level administrative divisions (regions) | **4,387** |
| Regions with upstream shortCode (ISO 3166-2 style) | **4,208** |
| Countries with zero regions in source | **0** |
| Missing or invalid flag emojis | **0** |
| Duplicate country `id` / `iso2` values | **0** |
| JSON syntax validity | **Valid** |

These counts match the upstream `country-region-data` `data.json` retrieved on 2026-09-26 (249 countries, 4,387 regions).

## 2. Source verification

| Item | Detail |
|------|--------|
| Upstream repository | https://github.com/country-regions/country-region-data |
| Raw data URL | https://raw.githubusercontent.com/country-regions/country-region-data/master/data.json |
| Upstream license | MIT |
| Upstream changelog (relevant) | 4.1.0 (5 Feb 2026) — Uganda regions updated; earlier updates to Hungary, UK, Chile, Norway, Spain, etc. |
| Source file size | ~339 KB |
| Source structure | Array of `{ countryName, countryShortCode, regions: [{ name, shortCode? }] }` |

No countries or regions were omitted or silently dropped. All 249 entries and all 4,387 regions were carried forward.

## 3. Enrichment sources

| Data element | Source | Notes |
|--------------|--------|-------|
| ISO alpha-2 / alpha-3 / numeric, official names, flags | ISO 3166-1 public data (community mirrors + Unicode regional indicators) | Flags generated deterministically from alpha-2 |
| Capitals, phone codes, currency code/name/symbol | dr5hn/countries-states-cities-database (`countries.json`) | ODbL — attribution required for this metadata |
| Administrative division terminology | Heuristic map keyed by ISO2 | Convenience labels for UI; not an official standard |

## 4. Key country checks (sample)

| Country | ISO2 | Flag | Region count | Notes |
|---------|------|------|-------------:|-------|
| Nigeria | NG | 🇳🇬 | 37 | 36 states + FCT (matches expected) |
| Japan | JP | 🇯🇵 | 47 | All prefectures present |
| United States | US | 🇺🇸 | 62 | States + DC + territories as present upstream |
| United Kingdom | GB | 🇬🇧 | 217 | Detailed regions as structured by upstream |
| Canada | CA | 🇨🇦 | 13 | Provinces + territories |
| Australia | AU | 🇦🇺 | 8 | States + territories |
| Germany | DE | 🇩🇪 | 16 | Länder |
| France | FR | 🇫🇷 | 26 | Regions as present upstream |
| India | IN | 🇮🇳 | 36 | States + union territories |
| Brazil | BR | 🇧🇷 | 27 | States + DF |
| Afghanistan | AF | 🇦🇫 | — | Provinces |
| Åland Islands | AX | 🇦🇽 | — | Municipalities |

All flags were verified non-empty and derived from the corresponding ISO alpha-2 code.

## 5. Countries with incomplete or limited subdivision data

- Every country in the source has a `regions` array (none empty after processing).
- A small number of regions lack an upstream `shortCode`; for those, a stable generated `id` is used and `iso3166_2` is set to `null`.
- Some micro-territories and special entities have very short or single-item region lists by nature of the upstream data (expected).
- Administrative type labels are heuristic; consumers needing strict legal classification should consult national sources.

## 6. Known discrepancies and disputed classifications

- **Disputed / partially recognized territories** (Kosovo, Palestine, Taiwan, Western Sahara, etc.) appear according to the upstream source. This dataset does **not** take a political position; apply your own inclusion policy.
- **United Kingdom structure** mixes constituent countries and more granular regions. Treat as address-oriented choices rather than a pure constitutional tree.
- **France / other multi-level systems** may list regions that reflect addressing practice rather than the absolute top legal tier after recent reforms.
- **Currency and phone data** come from a community dataset and should be re-verified for financial or telecom production systems.
- ISO 3166-2 codes are constructed as `ISO2-shortCode` only when a short code existed in the source; they are not independently re-validated against the official ISO 3166-2 database for every entry.

## 7. Data-quality checks performed

- [x] Valid JSON syntax
- [x] Unique country `id` / `iso2`
- [x] Unique region `id` within the file
- [x] Non-empty flag emoji for every country
- [x] Flag emoji matches ISO alpha-2 via regional indicator symbols
- [x] Parent–child integrity (`regions[].parent_id` = country `iso2`)
- [x] No fabricated ISO codes, capitals, currencies, or subdivision names
- [x] Country and region counts match the retrieved upstream source
- [x] Nigeria, Japan, US, UK, Canada, Australia, Germany, France, India, Brazil sampled for expected subdivision counts / presence

## 8. Limitations (explicit)

- Not a 100% complete global legal cadastre.
- Does not include second-level or lower administrative units.
- Does not include population, geometry, or extensive geospatial metadata.
- Administrative type strings are convenience labels, not authoritative.
- Upstream data can lag official boundary or naming changes; periodic re-fetch is recommended.

## 9. File integrity

| Item | Detail |
|------|--------|
| Path | `data.json` |
| Approximate size | 1.3 MB (pretty-printed UTF-8) |
| Parse test | Successful with standard JSON parsers |

## 10. Recommendation for production use

This file is suitable for offline cascading dropdowns and identity-form population. Prefer storing both the country `iso2` and the region `id` (or `iso3166_2` when present). Re-validate high-volume or regulated jurisdictions against national statistical agencies before high-stakes deployments. Keep upstream attribution as documented in `README.md` and `LICENSE`.
