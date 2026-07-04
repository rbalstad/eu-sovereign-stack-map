# European Tech Sovereignty Stack

An opinionated, sourced map of where Europe's technology stack is actually sovereign -- silicon to market -- and where it is still renting American kit with a European label.

It is deliberately **not** a definitive register. It is a snapshot (dated **June 2026**), built from public material, with editorial judgement in every tier call and every score. The point of open-sourcing it is to make those judgements correctable: if a node is wrong, [open a PR with a source](CONTRIBUTING.md).

**Live map:** `https://rbalstad.github.io/eu-sovereign-stack-map/` (live once GitHub Pages is enabled)
**Straight to the punchline:** add `#build-goal-llm` to the URL.

## What it shows

Fifty-three players across the eight layers of the stack -- market and policy, applications, data and AI, cloud and compute, datacentre, hardware and systems, chip design & IP, fabrication & equipment -- each tagged by tier, scored for sovereignty, and tied to a dated public source. Named partnerships and dependencies are wired between them.

The headline interaction is **build & break**. Pick a goal -- a sovereign workplace, a sovereign cloud, a sovereign LLM, a national supercomputer, a European-designed AI chip -- and the map lights every European option it has at each layer the goal needs, then turns red at the layers where it has none. The soft goals come up all green. The AI and compute goals snap red at the same two rungs every time: the AI accelerator and the leading-edge fab. That repeat is the whole argument.

## How to read it

- **Tier** (the card's colour and label): `European-owned & controlled`, `Sovereign by deployment` (EU-operated, non-EU parent), or `Imported dependency`. Ownership and jurisdiction are different axes -- a Swiss or UK firm is European by cap table yet sits outside the EU legal perimeter, and scores accordingly.
- **Sovereignty score** (the dots, 0-5): a read across ownership, jurisdiction, operations, supply chain, substitutability and exit. A low score is not a disqualification; it tells you where the risk lives. It is **not** a quality or "better product" score.
- **options, not winners** (in build & break): a lit ring marks a credible European option, not a packaged sovereign product you can buy off a shelf. The map names the contenders at each layer; it deliberately doesn't crown one as *the* proven choice. If you can show a real, documented, dated deployment for a given use, that sourced case is exactly what a PR is for (see [CONTRIBUTING.md](CONTRIBUTING.md)).

The scoring is anchored to the EU's own SEAL / Cloud Sovereignty Framework where it can be. The full rubric is in [CONTRIBUTING.md](CONTRIBUTING.md).

## Viewing it locally

The viewer loads its data from `data/stack.json` at runtime, so browsers block opening `index.html` straight off disk (`file://`). Either use the live map link above, or run a local server in the repo folder:

```sh
python -m http.server 8000
# then open http://localhost:8000/
```

## Contributing

Corrections and additions are welcome, and disagreement with a tier or a score is a legitimate contribution -- with a public source behind it. The data lives in one file, [`data/stack.json`](data/stack.json), pretty-printed so diffs stay readable. Read [CONTRIBUTING.md](CONTRIBUTING.md) first: it covers the sourcing requirement, the tier and score rubric, and how calls are made.

This is a maintained snapshot, not a live standard. The maintainer's call on tiers and scores is final, and the rubric it is made against is written down so it can be argued with.

## Structure

```
index.html         the interactive viewer (no build step, no dependencies)
data/stack.json    the data: layers, nodes, partnerships, news, goals
CONTRIBUTING.md     sourcing rules + tier/score rubric
LICENSE             MIT (the code)
LICENSE-DATA.txt    CC BY 4.0 (the data)
```

## Licence

The code (`index.html`) is [MIT](LICENSE). The data (`data/stack.json`) is [CC BY 4.0](LICENSE-DATA.txt) -- reuse it freely, with attribution. Each claim in the data carries its own source link; those sources belong to their publishers.

## Maintainer

Built and maintained by Reidar Balstad, RB Consult AB. LinkedIn: https://www.linkedin.com/in/rbalstad/ Sources are public Commission and EuroHPC releases, company announcements and credible trade press as of June 2026. Vendors are named as public contenders, not as participants in any confidential process.
