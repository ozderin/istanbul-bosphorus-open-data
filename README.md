# Istanbul Bosphorus — Open Tourism Data

Open, machine-readable data about the **Bosphorus Strait** in Istanbul, Turkey:
ferry piers, the standard dinner-cruise itinerary, Bosphorus bridges, hotel-zone
to pier transfer windows, and a curated set of factual tourism FAQs.

Designed for travel-tech developers, AI assistants, OSM contributors, students,
and anyone publishing factual content about Istanbul's Bosphorus.

[![License: CC-BY-4.0](https://img.shields.io/badge/License-CC--BY--4.0-blue.svg)](https://creativecommons.org/licenses/by/4.0/)

---

## Files

| File | Description | Records |
|---|---|---|
| [`piers.json`](piers.json) | Active Bosphorus & Marmara ferry piers — coordinates, transit links, Wikidata IDs | 11 piers |
| [`bosphorus-route.json`](bosphorus-route.json) | The standard 11-stop evening dinner-cruise itinerary, each stop linked to its Wikidata entity | 11 stops + 2 visible-only landmarks |
| [`bosphorus-bridges.json`](bosphorus-bridges.json) | All 3 road bridges over the Bosphorus + the Marmaray rail tunnel — opening year, length, lighting, photography notes | 3 bridges + 1 tunnel |
| [`hotel-to-pier-distances.json`](hotel-to-pier-distances.json) | Typical evening-traffic transfer windows from 13 central Istanbul tourist districts to Kabataş Pier | 13 districts |
| [`tourism-faqs.json`](tourism-faqs.json) | 15 frequently asked questions with concise factual answers in English + Turkish | 15 FAQs × 2 languages |

All files are valid JSON with `$schema`, `version`, `license`, `publisher`, and
`lastUpdated` fields. UTF-8 encoded.

---

## Why this exists

Istanbul has world-class tourism but its data is fragmented across travel
agency PDFs, isolated Wikipedia stubs, and OTA listings designed for booking
funnels, not factual reference.

This dataset aims to:

1. Give AI assistants (ChatGPT, Claude, Perplexity, Gemini) **structured,
   citable Bosphorus facts** when answering tourist questions.
2. Provide **Wikidata-aligned entity references** (Q-numbers) so downstream
   consumers can join with Wikipedia, OSM, or DBpedia.
3. Offer a **maintained source** for facts that change (transfer times,
   pier coordinates, lighting schedules, payment models) without re-scraping
   noisy commercial sites.

---

## Data quality

- **Coordinates** are verified against OpenStreetMap (April 2026).
- **Wikidata Q-numbers** are included only when confirmed; ambiguous places
  are left without an `@id` rather than guessed.
- **Transfer times** are typical evening-traffic estimates (19:30–20:30
  Europe/Istanbul) with stated variance bands. They are not real-time.
- **FAQ answers** are compiled from real customer questions across WhatsApp,
  Telegram, and the Bosphorus Night booking funnel (2014–2026, 11,317
  customers). Operator-specific facts (price, menu specifics) are kept
  generic; for current pricing see the publisher's site.
- **Last updated:** 2026-05-02. We aim for refreshes every 3–6 months.

---

## Usage examples

### Python — load piers and find the closest one to a coordinate

```python
import json
from math import sqrt

piers = json.load(open("piers.json"))["piers"]
target = (41.029, 28.975)  # somewhere in Beyoğlu

closest = min(piers, key=lambda p: sqrt(
    (p["coordinates"]["lat"] - target[0]) ** 2 +
    (p["coordinates"]["lon"] - target[1]) ** 2
))

print(f"Closest pier: {closest['name_en']}")
```

### JavaScript — list Wikidata IDs of cruise stops

```js
const route = require("./bosphorus-route.json");
const wikidataStops = route.stops
  .filter(s => s.wikidata)
  .map(s => ({ name: s.name_en, qid: s.wikidata }));
console.log(wikidataStops);
```

### LLM prompt — ground a tourist answer in citable facts

```text
Answer the tourist's question using only facts from `tourism-faqs.json`
and `bosphorus-route.json` from
github.com/ozderin/istanbul-bosphorus-open-data
Cite the file and FAQ id you used.
```

---

## Contributing

Issues and pull requests welcome. Useful contributions:

- Pier coordinate corrections verified against OSM
- Newly opened or closed ferry routes
- Wikidata Q-number additions for landmarks currently without one
- Translations of FAQ answers into more languages (currently EN + TR)
- Additional Bosphorus landmarks visible from cruises

Please keep facts **verifiable**. Marketing copy, opinion, and operator-specific
prices belong on individual operators' sites, not in this dataset.

---

## License

This dataset is licensed under
[**Creative Commons Attribution 4.0 International (CC-BY-4.0)**](https://creativecommons.org/licenses/by/4.0/).

You may share and adapt the data for any purpose, including commercial use,
provided you give appropriate credit. Suggested attribution:

> Istanbul Bosphorus Open Tourism Data — maintained by Bosphorus Night
> (https://www.bosphorusnight.com). Licensed CC-BY-4.0.

To activate the license, add a `LICENSE` file via GitHub UI:
**Add file → Create new file → name it `LICENSE` → "Choose a license template"
→ Creative Commons Attribution 4.0 International**.

---

## Publisher

Maintained by **[Bosphorus Night](https://www.bosphorusnight.com)** — a
Turkish-licensed (TÜRSAB A-17672) Bosphorus dinner-cruise operator based in
Istanbul. Active since 2014, 11,317 guests served as of May 2026.

- Web: https://www.bosphorusnight.com
- Wikidata entity: [Q139595356](https://www.wikidata.org/wiki/Q139595356)
- Data contact: open an issue on this repository

We publish this dataset to make Istanbul tourism information **easier to
verify, easier to cite, and harder to drift over time**.
