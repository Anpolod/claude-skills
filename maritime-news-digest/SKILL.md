---
name: maritime-news-digest
description: >
  Produce a daily English-language raw digest of maritime industry news for a
  Telegram channel. Covers six mandatory beats: merchant shipping (tankers,
  bulkers, containers, freight rates), offshore (OSV, rigs, offshore wind,
  subsea), maritime technology (autonomous vessels, green fuels, digitalization),
  crewing & seafarer labor (wages, shortages, MLC/STCW regulation), geopolitics
  & security (sanctions, piracy, Red Sea, Strait of Hormuz), ports & logistics.
  Output is draft-grade — short headline, 2–3 sentence blurb, source link —
  ready for the user to edit before posting. Sources can be in any language;
  posts are always in English. ALWAYS USE THIS SKILL when the user asks for
  a maritime digest, daily shipping news, "новости для канала", "дайджест",
  "news for Telegram", or mentions any combination of shipping / offshore /
  crewing / maritime news for a channel, even without the word "digest".
---

# Maritime News Digest — Daily Raw Feed

Purpose: generate a structured, copy-editable daily digest of maritime news
for a Telegram channel. Optimized for speed and coverage, not for polish —
the user edits and publishes.

## Output contract

The deliverable is a single markdown file saved to
`/mnt/user-data/outputs/maritime-digest-YYYY-MM-DD.md` and presented via
`present_files`.

Structure:

```
# Maritime Digest — {Weekday}, {Month D, YYYY}

_Sources checked: {N} · Items: {M}_

## 🛳️ Merchant shipping
### {Headline in English, ≤80 chars}
{2–3 sentence blurb in English. Include concrete numbers, named companies,
named vessels or routes where relevant. Paraphrase — never quote more than
one short phrase per source.}
→ {Source name} · {URL}

### {next item}
...

## 🛢️ Offshore
...

## ⚙️ Maritime technology
...

## 👨‍✈️ Crewing & seafarer labor
...

## 🌍 Geopolitics & security
...

## 🚢 Ports & logistics
...

---
_Generated {YYYY-MM-DD HH:MM UTC}_
```

Six H2 sections in this exact order with these exact emoji + names. Each
section contains 2–4 items (target 3). If a beat has nothing fresh worth
reporting, keep the H2 header and write a single line:
`_No notable developments in the last 24h._` — do not pad with filler.

## Workflow

### 1. Plan the search pass

Before searching, write a one-line plan listing the six beats and the rough
angle to hunt for today. Skip this in the final output — it is for
reasoning only.

### 2. Run targeted web searches

Use `web_search` with 6–10 queries total. Scale queries to recency: add
"today", "this week", or the current year only when a query would otherwise
return stale results. Prefer short 2–5 word queries.

Suggested query patterns per beat — pick 1–2 each run, rotate so the channel
does not repeat itself:

**Merchant shipping:** `tanker rates today`, `containership orderbook`,
`bulker scrapping`, `BDI index`, `VLCC spot rate`, `shipping companies results`

**Offshore:** `offshore wind contracts`, `OSV day rates`, `jackup rig fixture`,
`subsea cable project`, `FPSO order`

**Maritime technology:** `autonomous vessel trial`, `ammonia bunkering`,
`methanol dual-fuel`, `maritime AI`, `ship digital twin`, `wind-assisted propulsion`

**Crewing & labor:** `seafarer wages`, `STCW amendments`, `MLC 2006`,
`crew shortage officers`, `Filipino seafarers`, `Ukrainian seafarers`

**Geopolitics & security:** `Red Sea attack`, `Houthi tanker`, `shadow fleet
sanctions`, `Strait of Hormuz`, `shipping sanctions Russia`, `piracy West Africa`

**Ports & logistics:** `port congestion`, `Panama Canal transits`, `Suez Canal
traffic`, `terminal strike`, `container throughput`

Prefer primary sources: TradeWinds, Lloyd's List, Splash247, Riviera Maritime,
gCaptain, Offshore Engineer, Maritime Executive, Seatrade Maritime, Upstream,
WindEurope, IMO, BIMCO, ICS, Drewry, Clarksons, Baltic Exchange. Use Reuters /
Bloomberg / FT for breaking geopolitics stories. Skip low-signal aggregators
and SEO farms.

For sources in Ukrainian, Russian, Greek, Norwegian, Chinese, etc. — read
them if `web_search` surfaces them, but render the blurb in English.

### 3. Select items

Target 3 items per beat, minimum 2, maximum 4. Selection criteria in order:

1. **Material** — a named company, named vessel, concrete number, signed
   contract, regulatory change, casualty, or geopolitical event. Not opinion
   pieces, not "experts say" thinkpieces unless the expert is naming a
   concrete forecast number.
2. **Fresh** — published in the last 24–48h. Allow up to 7 days only if the
   story is a slow-moving regulatory or structural development.
3. **Non-duplicated** — one item per underlying story, even if multiple
   outlets covered it. Pick the strongest source.

If two items from the same beat cover related angles (e.g. two different
Red Sea incidents), include both — that's a real pattern, not duplication.

### 4. Write the blurbs

Each blurb is 2–3 sentences. Rules:

- English only, regardless of source language.
- Paraphrase. Maximum one short quote (<15 words) per digest across ALL items
  combined — not per item. Default to zero quotes.
- Lead with the concrete fact: who, what, how much, where. Skip editorializing.
- Use industry-standard units: dwt, TEU, bbl/d, $/day, knots. Prefix dollar
  amounts with $ and use "m" / "bn" (e.g. "$1.2bn", "$45,000/day").
- Name the company and the vessel class (VLCC, Suezmax, Capesize, ULCV, PSV,
  jackup, etc.) when known.
- No Telegram-style emojis inside blurbs. The section-level emoji in the H2
  is the only decoration.
- No hashtags. The user adds those when editing.

### 5. Format sources

Format: `→ {Source name} · {full URL}`

- Source name is the outlet, not the article title (e.g. "TradeWinds", not
  "TradeWinds article about…").
- Use the direct article URL, not the homepage.
- No tracking parameters (`?utm_…`) — strip them.

### 6. Save and present

Save to `/mnt/user-data/outputs/maritime-digest-YYYY-MM-DD.md` using
today's date in UTC, then call `present_files` with that path.

After presenting, end with one short line in chat like: "Draft ready —
{N} items across 6 beats. Edit and post." Do not restate the contents.

## Beat definitions (when in doubt, which section does an item belong to?)

- **Merchant shipping** — cargo-carrying commercial vessels and their
  markets. Freight rates, orderbooks, S&P, scrapping, earnings of listed
  owners, charters, casualties of merchant ships.
- **Offshore** — everything supporting offshore energy: OSVs, AHTS, rigs,
  FPSOs, offshore wind installation, subsea cables, decommissioning.
- **Maritime technology** — propulsion alternatives (LNG, methanol, ammonia,
  hydrogen, wind-assist, nuclear), autonomy, digitalization, maritime AI,
  class society rules for new tech, shipyard innovation.
- **Crewing & seafarer labor** — seafarer wages, shortages, training,
  STCW/MLC amendments, manning agencies, abandonment cases, welfare,
  visa/work-permit issues.
- **Geopolitics & security** — sanctions enforcement, shadow fleet, Red Sea
  / Houthi attacks, Iran detentions, Ukraine-related maritime events,
  piracy, naval escorts, flag-state politics.
- **Ports & logistics** — port throughput, congestion, strikes, terminal
  investments, canal transits (Suez, Panama), inland connectivity, box
  availability, supply chain knock-ons.

Overlap resolution: an item goes in the beat where the story is — a
methanol-fueled containership order is merchant shipping if the news is
the order, maritime tech if the news is the fuel milestone. A Houthi
attack on a tanker is geopolitics, not merchant shipping. A port strike's
impact on liner schedules is ports & logistics.

## Edge cases

- **Slow news day** — if the search pass yields fewer than 12 total items
  across all beats, run one more round targeting the thinnest beats. If
  still thin, ship what you have; do not invent filler.
- **Breaking story dominates** — if a single event (major casualty, war
  escalation) produces 10+ overlapping reports, cover it once in the
  appropriate beat with the strongest source, plus one follow-up item if
  there is a genuinely different angle (e.g. market reaction).
- **User asks for a specific beat only** — e.g. "just offshore today". Drop
  the other H2 sections but keep the same item format. Filename becomes
  `maritime-digest-offshore-YYYY-MM-DD.md`.
- **User asks for a different language** — respect it. Keep the structure
  and beat names identical; translate blurbs.
- **User provides a focus topic** — e.g. "focus on Red Sea today". Keep
  all six beats but weight the search toward the focus.

## Non-goals

- No market commentary in the skill's voice. Stick to reporting what
  happened, not what it means.
- No forecasts unless quoting a named analyst with a concrete number.
- No image generation — this is a text digest.
- No Telegram publishing — output is a markdown file the user copies.

## Reference

See `references/sources.md` for the canonical source whitelist and
`references/telegram-formatting.md` for notes on what works when the user
pastes the digest into Telegram.
