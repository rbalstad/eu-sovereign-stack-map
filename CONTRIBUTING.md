# Contributing

Corrections, additions and disagreements are welcome. A new player, a tier that's wrong, a score you'd argue down, a partnership that closed, a dependency that shifted -- all of it is fair game, as long as it comes with a public source.

## The short version

1. Edit [`data/stack.json`](data/stack.json). It is pretty-printed on purpose, so a change to one node is a small, readable diff.
2. Every factual claim needs a public source URL. No source, no merge.
3. Open a pull request. Say what changed and why, and link the source.

The viewer (`index.html`) reads the data at runtime -- you do not need to touch it to change the map. To see your edit, run a local server in the repo folder (`python -m http.server 8000`, then open `http://localhost:8000/`).

## The one rule: sources

This map is only worth something if every claim can be checked. So:

- A node's `what`, its `maturity`, and any dated claim must be backed by a public source: a Commission or EuroHPC release, a company announcement, a regulator filing, or credible trade press.
- A partnership or dependency edge (`PARTNERS`) carries its own `url` and `src`. Add them.
- A headline in `NEWS` carries its `url`, `source` and `date`. Add them.
- Prefer primary sources (the company, the Commission, the JU) over secondary coverage where both exist.

Vendors are named as public contenders, not as participants in any confidential process. Keep it that way.

## Tier rubric (`tier`)

The tier is the single most contested field, so it has the tightest definition. Tier is about **ownership and control**, not quality and not location of the datacentre.

- `eu` -- **European-owned & controlled.** Cap table and ultimate control sit in the EU, EEA/EFTA, Switzerland or the UK. Note that ownership and jurisdiction are different axes: a Swiss or UK firm is European by ownership but sits outside the EU legal perimeter, which is a `score` penalty (see below), not a tier change.
- `operated` -- **Sovereign by deployment.** A non-EU parent, run through an EU-resident, EU-controlled legal structure (separate staffing, local key control, independent governance). The operator is European; the control plane and IP are not. AWS European Sovereign Cloud, Bleu, Delos, S3NS live here.
- `dependency` -- **Imported dependency.** Owned and controlled outside Europe. This is the thing the sovereignty argument is reacting to -- the incumbent being substituted, or the borrowed foundation underneath. Naming these honestly is what gives the rest credibility.

A European-heritage company owned outside the EU (an EU startup acquired by a US parent, a US-incorporated firm with EU founders) is `dependency`, with the ownership flag stated in `weak`. Heritage is not control.

## Sovereignty score (`score`, 0-5)

The dots are a read across six dimensions: **ownership, jurisdiction, operations, supply chain, substitutability, exit**. Anchor to the EU's own SEAL / Cloud Sovereignty Framework where a SEAL level exists.

It is **not** a quality score and not a "better product" ranking. A low score does not disqualify a player; it says where the risk lives. OVHcloud at 5 is not "better software than AWS"; it scores high on ownership, jurisdiction and exit. A score change needs a reason tied to one of the six dimensions, not a preference.

Rough calibration: **5** clean across all six (e.g. ASML, OVHcloud at SEAL-3). **4** strong but with one real soft axis (Proton -- Swiss, so a jurisdiction notch; SiPearl -- Arm ISA + TSMC fab). **3** genuinely mixed (the EU-operated tier: European operations over a foreign control plane). **2** European in name, dependent in substance. **1** imported foundation (Nvidia/CUDA, hyperscaler defaults, x86).

## Four-axis rubric (`axes`, `axes_note`)

Each card carries an `axes` object that decomposes the score into four explicit, arguable calls. The tier stays the primary cut -- ownership -- and the axes are the why underneath it. Ownership itself stays with the tier rubric above; supply chain shows up as `depends_on` and the dependency edges; exit folds into substitutability. Values are **enums, not numbers**: every call is an editorial judgement that should be correctable by PR, and named values are easier to argue with than invented precision.

```json
"axes": {
  "capability":       "credible",
  "jurisdiction":     "eu",
  "op_control":       "european",
  "substitutability": "hard"
},
"axes_note": "one line explaining the calls, grounded in the card's sourced facts"
```

- **capability** -- `none | emerging | credible | proven`. Does this node field a working capability today, at what maturity? `emerging` = pre-product, prototype, design phase. `credible` = shipping or deployed, not yet at incumbent scale. `proven` = production at scale. This axis grades the node itself, whatever its flag -- an imported incumbent is honestly `proven`.
- **jurisdiction** -- `eu | mixed | extra_eu`. Whose law reaches the controlling entity? `eu` = the EU legal perimeter. `mixed` = material extra-EU legal exposure on a European entity: Swiss or UK domicile (Proton), a pending extra-EU acquisition (Aleph Alpha), or an EU-controlled structure under a US parent (the operated tier). `extra_eu` = controlled from outside the perimeter -- CLOUD Act and export-control reach (Microsoft 365, Arm, Nvidia).
- **op_control** -- `european | shared | foreign`. Who holds root, the keys and the off-switch for what the node delivers in Europe? `shared` = a European operator over a foreign control plane or parent technology (Bleu, Delos, S3NS, AWS ESC -- and SAP RISE where the hosted service inherits the cloud beneath).
- **substitutability** -- `swappable | hard | locked`. If this node's function had to be re-sourced tomorrow, how fast could Europe do it? `swappable` = credible alternates exist (EU IaaS, open-source workplace). `hard` = alternates exist at real cost in years and capability (leaving M365, replacing SAP). `locked` = no substitute on any near horizon -- the two hard breaks (the AI accelerator, the leading-edge fab), but also Europe's own sole-source concentrations (ASML, Eviden's Angers plant): `locked` flags concentration, not villainy.

Rules of the road: leave an axis `null` rather than guess -- `null` renders as "not scored" and is an honest answer. Policy instruments and case-study nodes take `null` on axes that are not meaningful for them (substitutability of a rulebook). Every set of calls carries an `axes_note`: one line, grounded in the card's already-sourced facts, saying why. To argue a call, cite the enum definition and a public source -- exactly like a tier or score change. A change to these definitions is a scoring-model change and must update this rubric in the same PR.

## proven vs option (build & break)

In a goal, a node is listed under `proven` only if there is a real, documented, dated deployment for that use, with a source. Everything else is a `cand` -- a credible European option, not a packaged product. This is the honesty rule; do not promote an option to `proven` on a roadmap or an announcement. A signed, shipped, in-production deployment, or it stays an option.

## build & break goals (`GOALS`)

Each goal maps the layers a stack needs to the nodes that can fill them, and names the layers where Europe has no answer.

```json
{
  "id": "llm",
  "name": "Sovereign LLM",
  "verdict": "one honest line",
  "needs":  [ { "label": "Model", "cand": ["mistral","aleph"], "proven": ["mistral"] } ],
  "breaks": [ { "label": "AI accelerator (GPU)", "node": "sys-dep", "why": "no European GPU at scale" } ]
}
```

`needs[].cand` and `proven` are node ids. `breaks[].node` is the node id of the imported dependency that blocks the layer. The goal -> layer mapping is the most editorial part of the whole map -- if you think a break lands on the wrong rung, that is exactly the kind of argued, sourced change a PR is for.

## Data shape

```
LAYERS[]   { key, name, verdict }
CARDS[]    { id, layer, tier, name, score(0-5), tag, what, maturity, weak,
             axes{capability,jurisdiction,op_control,substitutability}, axes_note,
             roadmap{claim,year,source,url}?, depends_on[]? }
PARTNERS[] { a, b, label, year, type, src, url }     // a,b are node ids
NEWS{}     { <nodeId>: { date, headline, source, url } }
GOALS[]    { id, name, question, verdict, needs[], breaks[] }   // see above
```

`roadmap` is an optional, forward-looking public milestone -- a vendor claim
with a primary-source URL, labelled as claimed, never endorsed. No public dated
statement = no field; the viewer renders that as "no public roadmap date".
Currently used on the chip-goal design candidates only. `question` is the
plain-language phrasing of a goal shown in the map's simple view.

`PARTNERS[].type` is one of `invest, partner, codev, deploy, award, dependency, policy, risk` (it sets the wire colour). `layer` on a card must match a `LAYERS[].key`.

## How calls are made

This is a maintained snapshot, not a committee. The maintainer's call on tiers and scores is final -- but it is made against the rubric above, in the open, so it can be argued with. If you think a call is wrong, cite the rubric and the source; "this feels low" is not an argument, "jurisdiction is EU now because of X, here's the filing" is. Vendor self-promotion without a sourced, rubric-based case will be closed.
