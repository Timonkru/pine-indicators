# Republish-Paket (konforme EN-Beschreibungen)

Regeln beachtet: EN zuerst, Konzept+Originalität+Nutzung, KEINE Links/Brands,
Clean Chart (frisches Layout, NUR das jeweilige Skript!). Titel ohne
[KruegerAlgorithms]. **Status 16.07. (2. Pass): ALLE 6 Skripte komplett EN —
UI-Inputs, Tooltips, Labels, Tabellen, Alert-Texte, Alert-Titel UND
Code-Kommentare** (der 1. Pass hatte Reste: dt. Tooltips, „Kasse"-Titel im
indicator(), © KasseATB-Zeile, dt. Alert-Meldungen — alles behoben, per Grep
verifiziert). indicator()-Titel = Publikationstitel. Jede Beschreibung hat
jetzt „What makes it original" + „How to use it". Gamma-Template bleibt
bewusst deutsch/privat — statische Tageswerte sind ohnehin nicht publizierbar
(Originality/Usefulness-Regel); falls es publiziert war: NICHT neu publizieren.

---

## EINHEITLICHES SCHEMA (Beschluss 16.07. — „man sieht, es gehört zusammen")

**Titel-Grammatik:** `<Werkzeug> — <Scope>`, Paare teilen sich den Wortstamm:

| Skript | Einheitlicher Titel |
|---|---|
| Futures-Vol-Profil | **Futures Volume Profile — CFD Charts** |
| Futures-Vol-VWAP | **Futures Volume VWAP + Bands — CFD Charts** |
| Futures-Session-TWAP | **Futures Session TWAP + Bands — CFD Charts** |
| Futures-Vol-Delta | **Futures Volume + Delta — CFD Charts** |
| Vol-Confirmed OBs | **Volume-Confirmed Order Blocks — CFD Charts** |
| Sweep-Volumen | **Liquidity Sweep Volume — CFD Charts** |
| Kasse Alerts | **Session Candle Levels & Alerts — Intraday** |
| Kasse Swing | **Session Candle Levels & Alerts — Weekly Swing** |
| Fisher ACD | **Opening Range ACD + Pivot Ranges — Label-Free** |
| ONR Sessions | **Overnight Range & Sessions — Multi-Index** |

Die Paare (Futures Volume ×2, Session Candle Levels ×2, Range ×2) erkennen
sich am Stamm; alle sechs an der Em-Dash-Grammatik.

**Beschreibungs-Struktur (immer gleich):**
1. Ein Satz: was + für wen
2. „What makes it original" (Moderator-Pflichtpunkt!)
3. „How it works" (Bullets)
4. „How to use it" (Absatz)
5. Einheitlicher Schlusssatz (in JEDE Beschreibung, TV-intern = regelkonform):

> *This script is part of a consistent set of open-source session, range and
> volume tools — the companions are on my profile.*

**Moderator-Abschluss (wenn alle 6 neu online sind):** kurz melden, dass alle
Publikationen regelkonform neu veröffentlicht wurden (EN-UI, EN-Beschreibung,
Clean Charts, keine Brands), Liste der 6 neuen anfügen, höflich um Entfernung
der versteckten Alt-Skripte bitten.

## 1) Futures Volume Profile — CFD Charts

**Titel:** Futures Volume Profile — CFD Charts

**Beschreibung (Copy-Paste):**

CFD charts only show broker tick volume, which does not represent real market
participation. This indicator pulls REAL exchange volume from the matching
futures contract and builds a volume profile directly on your CFD chart.

What makes it original: standard volume-profile tools weight by the chart's
own (tick) volume. This one maps futures contract volume into CFD price
coordinates (price = CFD, weight = futures), accumulates the profile
incrementally so month anchors work without lookback limits, and can anchor
the session to the futures trading day instead of CFD broker midnight.

How it works:
- The futures contract is auto-detected from the chart symbol (DAX/GER40 ->
  FDAX, NAS100 -> NQ, US30 -> YM, UK100 -> Z, US500 -> ES), or set manually.
- Each chart bar's price range is split into zones; the futures volume of that
  bar is distributed across the zones it covers (price = CFD coordinates,
  weight = futures volume — so the basis offset between CFD and futures is
  handled naturally).
- The profile accumulates incrementally per anchor period (session/week/month)
  and resets at the period change. POC (red), VAH/VAL (blue, dashed) and the
  histogram update live.
- Optional "Daily anchor = futures trading day": the session reset fires at
  the futures day change instead of the CFD broker midnight, so the profile is
  anchored identically whether you chart the CFD or the future itself.
- Bars without futures data (e.g. overnight hours of a 24h CFD) contribute
  nothing — mixing tick volume with contract volume would distort the profile.
  The source label warns you when no futures data is available.

How to use it: treat POC/VAH/VAL as the real participation levels behind your
CFD chart — acceptance above the value area supports continuation, a rejection
back inside favors rotation toward the POC. Thick zones (HVN) act as magnets
and consolidation areas, thin zones (LVN) tend to be traversed quickly, which
makes them useful stop and target references. Zone width is ATR-derived by
default or fixed in points.

---

## 2) Futures Volume VWAP + Bands — CFD Charts

**Titel:** Futures Volume VWAP + Bands — CFD Charts

**Beschreibung (Copy-Paste):**

A session/week/month-anchored VWAP with 1/2/3-sigma bands — with a twist for
CFD traders: the VWAP weighting can use REAL futures exchange volume instead
of broker tick volume.

What makes it original: this is not another VWAP variant — the weighting
source is the auto-detected futures contract while the price stays the CFD's,
bars without futures data are excluded instead of silently falling back to
tick volume, and the daily anchor can follow the futures trading day so the
VWAP matches across CFD and native futures charts.

How it works:
- Price source is this chart's CFD price (hlc3 by default); the weight is the
  volume of the auto-detected futures contract (request.security). Because
  only the WEIGHT comes from the future, the basis offset between CFD and
  futures prices is handled naturally.
- Bands are computed from the volume-weighted variance around the VWAP.
- Optional "Daily anchor = futures trading day" resets the calculation at the
  futures day change instead of CFD broker midnight, so the VWAP matches
  across CFD and native futures charts.
- Bars without futures data do not enter the sums; a status label always
  shows which volume source is active.

How to use it: the standard VWAP playbook applies — price above a rising VWAP
supports longs, reversion trades target the VWAP, and the 2/3-sigma bands mark
statistically stretched zones where momentum entries have poor expectancy. The
difference is that these levels are weighted by real market participation, so
they match what futures traders see instead of broker tick noise. Check the
status label once after loading to confirm the futures feed is active.

---

## 3) Opening Range ACD + Pivot Ranges — Label-Free

**Titel:** Opening Range ACD + Pivot Ranges — Label-Free

**Beschreibung:**

An implementation of Mark B. Fisher's ACD methodology ("The Logical Trader")
combined with daily/3-day/7-day pivot ranges — deliberately label-free: every
signal is expressed through color, line weight and box shading, so the chart
stays readable.

What makes it original: it combines the ACD entry framework with Fisher's
pivot-range bias in one visual system (most public ACD scripts stop at the
A/C levels), auto-derives the stretch from recent opening ranges instead of
requiring a hand-tuned constant, and auto-detects cash open + timezone per
market.

How it works:
- A-up/A-down entry levels are projected from the opening range plus a
  "stretch" (auto-derived from recent opening ranges and capped, or fixed);
  C-levels mark the failure side. Lines thicken once an A-level is confirmed
  for a configurable number of bars, and turn gray after a C-failure.
- The pivot-range box colors the daily bias: open above the range = bullish,
  below = bearish, inside = neutral; alignment of daily, 3-day and 7-day
  ranges is highlighted as "triple" confluence.
- Cash open and timezone are auto-detected per market (EU/US/UK/Nikkei/HSI)
  with a manual override, so the opening range starts at the real cash open.
- Optional compression background when today's pivot range is unusually
  narrow (expansion expected).

How to use it: wait for the opening range to complete, then treat a confirmed
A-up/A-down (the line thickens) as the intraday bias trigger in that
direction; a C-failure (line turns gray) cancels or flips the bias. Alignment
matters more than any single level — an A-up confirmation while the open is
above a bullish pivot range, ideally with "triple" confluence, is the
highest-quality condition, while signals against the pivot bias deserve
smaller size. The compression background flags days where a range expansion
is more likely than usual. Alerts cover confirmed A-levels, C-failures and
pivot-range crosses.

## 4) Overnight Range & Sessions — Multi-Index

**Titel:** Overnight Range & Sessions — Multi-Index

**Beschreibung:**

Draws the overnight range (ONR) of the instrument's own overnight session —
not a generic fixed window — plus EU/US/Asia session boxes and the previous
day's cash close as a gap reference.

What makes it original: generic session tools apply one fixed overnight
window to everything. This one carries per-instrument overnight definitions
(indices, metals, oil, forex), supports fragmented quiet-phase sessions for
24h markets, detects US/EU DST shifts automatically from the Berlin-New York
time difference, and captures the cash close timeframe-independently so the
gap reference works on any chart resolution.

How it works:
- The overnight window is auto-selected per instrument (DAX, FTSE, US indices,
  Russell, gold, silver, copper, oil, Nikkei, HK, forex) with a manual
  override and a custom session input.
- Commodities/forex can use fragmented sessions: several small quiet-phase
  boxes instead of one large overnight range.
- US/EU daylight-saving shifts are detected automatically from the
  Berlin-New York time difference (manual override available).
- An info table shows the detected index, session window and mode; the
  previous day cash close is drawn as a line for gap-up/gap-down context.

How to use it: before the cash open, read the overnight high/low as the first
breakout frame of the day — a cash-session break out of the ONR sets the
directional tone, while repeated rejections at the ONR edges frame fade
setups back into the range. Combine with the previous cash close line for gap
context (open above both ONR high and prior close is a very different day
than an open inside the range), and use the session boxes to see which region
built the current move.

## 5) Session Candle Levels & Alerts — Intraday

**Titel:** Session Candle Levels & Alerts — Intraday

**Beschreibung:**

Marks the intraday reference candles many index day-traders work with and
fires volume-confirmed breakout alerts on their highs/lows.

How it works:
- 15-minute reference candles after the EU and US cash open (2nd and 7th
  candle) with configurable colors per region.
- 5-minute references: 1st/4th candle levels, plus the "8 Ball" (8th 5-min
  candle) and "Teenager" (13th) known from Tom Hougaard's FTSE approach —
  including an orderly-range filter that skips outlier bars.
- Pre-market levels (30-min low / 10-min high-low before the cash open),
  special sessions for gold, forex, HK50 and JPN225, and an optional
  FOMC/NFP event mode that marks post-release reference candles.
- Breakouts can require volume above a configurable multiple of the average
  ("volume-confirmed") before a signal counts; retest labels/alerts optional.
- US/EU DST shifts are handled automatically.

What makes it original: this is not a generic session-open marker — it encodes
a specific, curated set of reference candles (2nd/7th 15-min, 1st/4th 5-min,
8 Ball/Teenager) with the orderly-range outlier filter and volume confirmation
built in, plus automatic US/EU DST alignment so the levels stay correct across
the March/November transition weeks.

How to use it: pick your market (EU or US index, gold, forex, HK50 or JPN225),
enable only the reference candles you actually trade, and set alerts on their
highs/lows — the script is built so the chart can stay closed until an alert
fires. A volume-confirmed break of a reference level signals real
participation behind the move; the optional retest labels/alerts catch the
second entry back at the level. On FOMC/NFP days the event mode replaces the
standard references with post-release reference candles.

## 6) Session Candle Levels & Alerts — Weekly Swing

**Titel:** Session Candle Levels & Alerts — Weekly Swing

**Beschreibung:**

The swing companion to the intraday version: draws the weekly reference
levels swing traders track and alerts on volume-confirmed breaks.

How it works:
- Monday/Wednesday high-low, previous week and previous month high-low,
  weekly and monthly opens — each with a configurable lookback of how many
  past occurrences stay on the chart.
- Thu-Fri-Mon pattern: when Friday's high stays below Thursday's high, the
  setup arms and targets Friday's low the following week (alerted).
- Breaks can require volume above a configurable multiple of average volume
  before they count ("volume-confirmed").
- Optional COT panel (CFTC Commitment of Traders via TradingView's COT
  library): managed-money and retail net positioning with weekly change,
  auto-mapped from the chart symbol.

What makes it original: the COT layer goes beyond plotting net positions — it
percentile-ranks managed money against ~2 years of history and flips it
contrarian at extremes (crowded trades), and it maps DAX/FTSE to EUR/GBP COT
data with an inverted score, since there is no direct COT series for those
indices.

How to use it: on a daily or 4h chart, watch how price behaves at the
Monday/Wednesday and previous week/month extremes — a volume-confirmed close
beyond a level signals continuation, a rejection signals range rotation. The
Thu-Fri-Mon pattern arms automatically at the Monday open and alerts when
Friday's low is hit. Use the COT background/table as a weekly positioning
filter for swing direction, not as an entry trigger.

## 7) Gamma Exposure Profile — Manual Chain Input

**⚠ TIMING: Erst publizieren, wenn der Moderator die 6 Republishes abgenickt
hat** — kein neues Skript, solange der Account unter Beobachtung steht.

**Titel:** Gamma Exposure Profile — Manual Chain Input

**Beschreibung (Copy-Paste):**

A self-contained dealer-gamma calculator. Pine cannot fetch external data, so
instead of shipping stale hardcoded levels, this script takes the option
chain as USER INPUT: paste strike rows into the settings and it computes the
full gamma exposure profile on your chart — no republishing, never stale.

What makes it original: most "GEX" scripts on TradingView approximate gamma
from price or volume. This one implements the actual model — Black-Scholes
gamma per strike from open interest and implied volatility, aggregated with
the standard dealer sign convention (long call gamma, short put gamma) — and
derives the flip point by scanning where total exposure changes sign, the
same way institutional GEX tools do.

How it works:
- Input format (one paste): a header line
  `#spot=24900;dte=21;mult=5;iv=0.18;date=2026-07-17`
  followed by one line per strike: `strike;callOI;putOI[;iv]`.
- Per strike, the script computes Black-Scholes gamma (r = 0) and the signed
  dealer exposure `gamma x (callOI - putOI) x multiplier x spot² x 1%`.
- Gamma flip = the zero crossing of total exposure over a spot grid (closest
  to spot); call/put walls = strikes with the largest positive / most
  negative exposure; second walls dashed; expected move from ATM IV.
- A second optional text area takes a near-dated chain (e.g. 0-5 DTE) for
  the short-term walls, drawn dotted — structure vs day-weather separation.
- The histogram next to price shows the signed per-strike profile; the
  background colors the regime (above flip = long gamma, below = short
  gamma); a date in the header enables a staleness warning after 36h.
- Alerts: flip cross up/down, put-wall break/reclaim, call-wall break.

How to use it: pull the option chain of the underlying index (your broker
platform or the exchange's public chain), paste the strike rows once per day,
and read the chart: above the flip, dealer hedging dampens moves (mean
reversion toward walls, pinning); below the flip it amplifies them (trend
days, trapdoor under the put wall). The levels are model estimates from your
own snapshot — context, not trade signals.

*This script is part of a consistent set of open-source session, range and
volume tools — the companions are on my profile.*

---

## 8) Futures Session TWAP + Bands — CFD Charts

**Titel:** Futures Session TWAP + Bands — CFD Charts

**Beschreibung (Copy-Paste):**

An anchored TWAP (time-weighted average price) with 1/2/3-sigma bands that
knows when the real market is actually open — built for CFD and cash-index
charts whose 24h quotes distort classic session averages.

What makes it original: a TWAP weighs every bar equally, so on a 24h CFD chart
the thin overnight bars count as much as the liquid session and drag the line
away from the number execution desks reference. This indicator pulls the
volume of the auto-detected futures contract and uses it as a SESSION GATE:
only bars where the future actually traded are counted. It also plots an
optional futures-volume VWAP on the same anchor, so the TWAP-vs-VWAP spread
becomes readable at a glance — that spread is the point of the pair.

How it works:
- TWAP = equal-weight average of the chart's price (hlc3 by default) over the
  anchor period (session/week/month); bands from the time-weighted variance.
- Session gate (optional, on by default): bars without futures volume are
  skipped, so the TWAP covers the real trading session. Only session/volume
  information is borrowed from the future — the price stays this chart's
  price, so the futures-vs-cash basis cannot distort the level.
- The futures contract is auto-detected from the chart symbol (DAX/GER40 ->
  FDAX, NAS100 -> NQ, US30 -> YM, UK100 -> Z, US500 -> ES), or set manually.
- Optional "Daily anchor = futures trading day" resets at the futures day
  change instead of CFD broker midnight, matching the sibling VWAP tool.
- A status label shows the active source, the gate state and the current
  VWAP-minus-TWAP spread.

How to use it: the TWAP is the fair time-average of the session — the line an
evenly-sliced execution would achieve. Compare it with the futures-volume
VWAP: VWAP above TWAP means volume was concentrated above the time average
(participants paid up), VWAP below TWAP means volume traded below it. The two
lines glued together signals balanced rotation; a widening spread marks
one-sided participation. The 2/3-sigma bands frame statistically stretched
zones relative to the session mean. Check the status label once after loading
to confirm the futures feed is active.

*This script is part of a consistent set of open-source session, range and
volume tools — the companions are on my profile.*

---

## 9) Futures Volume + Delta — CFD Charts

**Titel:** Futures Volume + Delta — CFD Charts

**Beschreibung (Copy-Paste):**

A volume pane for CFD and cash-index charts that shows the REAL traded volume
of the matching futures contract — plus an estimated buy/sell delta and
absorption flags for effort-without-result bars.

What makes it original: CFD "volume" is broker tick count, not market
participation. This pane never uses it — every column is the exchange volume
of the auto-detected futures contract. The delta column is built from the
futures contract's own lower-timeframe candles (up-candle volume counts as
buying, down-candle volume as selling): the volume is real, only the side
attribution is an estimate, and it is labeled as such. Absorption flags then
combine both axes — volume percentile high while the price range percentile is
low — the candle-data footprint that hidden passive interest (iceberg-style
execution) leaves behind. Pine has no order book, so this is explicitly a
footprint proxy, not order-book detection.

How it works:
- The futures contract is auto-detected from the chart symbol (DAX/GER40 ->
  FDAX, NAS100 -> NQ, US30 -> YM, UK100 -> Z, US500 -> ES), or set manually.
- Histogram = futures volume per chart bar; columns tint with bar direction,
  a configurable MA marks the average.
- Delta = buy-minus-sell futures volume from 1/5/15-minute intrabars
  (auto-selected by chart timeframe, manual override). Lower-timeframe
  history is limited, so the delta reaches less far back than the histogram.
- Absorption flag (orange diamond + alert): volume percentile >= X and range
  percentile <= Y over a rolling lookback — both thresholds adjustable.
- A status label confirms the active source and the delta resolution.

How to use it: read it like a footprint-lite. Rising price on rising futures
volume = participation confirms the move. An absorption diamond after an
extended run — heavy contracts traded, no price progress — marks where passive
interest is absorbing the aggression; combined with a one-sided delta it is a
common exhaustion/iceberg footprint. Delayed futures feeds confirm bars a few
minutes late; the historical picture is complete.

*This script is part of a consistent set of open-source session, range and
volume tools — the companions are on my profile.*

---

## 10) Volume-Confirmed Order Blocks — CFD Charts

**Titel:** Volume-Confirmed Order Blocks — CFD Charts

**Beschreibung (Copy-Paste):**

Order-block zones for CFD and cash-index charts — kept only when the impulse
that created them carried above-average REAL futures volume.

What makes it original: classic order-block tools mark every last opposite
candle before a fast move, which paints charts full of untested zones. This
one validates the impulse against the exchange volume of the auto-detected
futures contract (never the CFD's tick volume): a zone only exists where real
contracts changed hands. Price coordinates stay this chart's prices — only the
volume is borrowed, so the futures-vs-cash basis cannot shift a zone.

How it works:
- Impulse = candle range > k x ATR with a decisive body (both adjustable).
- The last opposite candle within a few bars before the impulse becomes the
  order block; its high/low span the zone.
- Volume confirmation: the impulse candle needs >= x times the average
  futures volume (set the multiplier to 0 for classic, unfiltered blocks).
- Zones extend right until price closes through the far side (mitigation),
  then they are removed; the number of kept zones is capped.
- Label-free chart output — zones read from color and geometry alone. Alerts
  fire on new bullish/bearish zones.

How to use it: treat the zones as inventory areas of real participation —
pullbacks into a fresh volume-confirmed zone are the classic retest context,
a close through the far side invalidates it (and removes it from the chart).
The volume filter is the quality dial: raise the multiplier to keep only the
zones where the futures market genuinely committed. Delayed futures feeds
confirm new zones a few minutes late; history is complete.

*This script is part of a consistent set of open-source session, range and
volume tools — the companions are on my profile.*

---

## 11) Liquidity Sweep Volume — CFD Charts

**Titel:** Liquidity Sweep Volume — CFD Charts

**Beschreibung (Copy-Paste):**

A liquidity-sweep detector that does not just mark the wick — it measures it:
how many REAL futures contracts traded beyond the swept level, with what
buy/sell delta, and how that compares to average volume.

What makes it original: sweep indicators usually paint every wick through a
swing point. This one does two things differently. First, the definition is
strict and testable: the excursion beyond the level may last several bars but
NO bar may close beyond it (a close beyond is a breakout, not a sweep), and
after the reclaim the excursion extreme must HOLD for a configurable number
of bars — if it is retaken, the sweep is invalidated and stays on the chart
grayed out instead of being silently deleted. Second, every surviving sweep
is quantified with the exchange volume of the auto-detected futures contract
(the chart's broker tick volume is never used): the excursion minutes are
located on this chart's own lower-timeframe prices (CFD price against CFD
level — no futures/cash basis mixing), and the volume of exactly those
minutes is taken from the futures contract.

How it works:
- Reference levels: confirmed swing highs/lows (pivot length adjustable) and,
  optionally, the prior-day high/low.
- Sweep = up to N bars trading beyond the level (none closing beyond), then a
  reclaim, then K quiet bars that do not retake the excursion extreme. N and
  K are inputs; confirmation therefore prints K bars after the reclaim by
  design — that is the repaint-free price of the hold rule.
- A line connects the swept swing to the sweep, so you see WHICH liquidity
  was taken. Invalidated sweeps stay visible in gray (honest history).
- Each confirmed sweep prints: excursion volume (futures contracts traded
  beyond the level, summed over all excursion bars), its ratio to average bar
  volume, and the excursion delta estimated from the futures lower-timeframe
  candles (volume real, side approximated — Pine has no order book). If the
  intrabar grids of chart and future differ, the tool falls back to full-bar
  volume and marks the value with "~".
- Solid label + alert when the sweep volume clears an adjustable multiple of
  average volume; gray label for low-volume sweeps. Auto-detected futures
  (DAX/GER40 -> FDAX, NAS100 -> NQ, US30 -> YM, UK100 -> Z, US500 -> ES),
  manual override, status label.

How to use it: treat high-volume confirmed sweeps as measured absorption at
the level — the classic stop-hunt-then-reverse context, now with a number
attached instead of a guess. Low-volume sweeps are cosmetic wicks; fading
them has no participation behind it. The delta adds the direction of the
trapped side: a low swept on heavy selling that closes back above means
aggressive sellers were absorbed below the level. Delayed futures feeds
confirm labels a few minutes late; history is complete.

*This script is part of a consistent set of open-source session, range and
volume tools — the companions are on my profile.*

---

## Kategorien & Tags (beim Publizieren)

Kategorie = TV-Dropdown (fix; nächstliegenden nehmen, falls Name abweicht).
Tags = Freitext, klein, ohne Brand/Personennamen.

| Skript | Kategorie | Tags |
|---|---|---|
| Futures Volume Profile | Volume | volumeprofile, futures, cfd, poc, valuearea, sessions, dax, nasdaq, orderflow |
| Futures Volume VWAP + Bands | VWAP (sonst Volume) | vwap, futures, cfd, volume, bands, standarddeviation, meanreversion, intraday |
| Futures Session TWAP + Bands | VWAP (sonst Volume) | twap, vwap, futures, cfd, sessions, bands, standarddeviation, meanreversion, execution, intraday |
| Futures Volume + Delta | Volume | volume, volumedelta, futures, cfd, absorption, orderflow, footprint, exhaustion, intraday |
| Volume-Confirmed Order Blocks | Support and Resistance | orderblock, smartmoney, futures, volume, cfd, supplydemand, mitigation, breakout, intraday |
| Liquidity Sweep Volume | Support and Resistance | liquiditysweep, stophunt, sweep, futures, volume, absorption, wick, reversal, smartmoney, intraday |
| Session Candle Levels — Intraday | Support and Resistance | sessions, intraday, daytrading, breakout, alerts, keylevels, premarket, fomc, indices |
| Session Candle Levels — Weekly Swing | Support and Resistance | swingtrading, weeklylevels, monthlylevels, cot, breakout, alerts, keylevels |
| Opening Range ACD + Pivot Ranges | Pivot points and levels | acd, openingrange, orb, pivotrange, breakout, daytrading, bias, daytradingsystem |
| Overnight Range & Sessions | Support and Resistance | overnightrange, sessions, gap, indices, breakout, asiasession, premarket |
| Gamma Exposure Profile (NACH Moderator-OK) | Volatility (sonst Support and Resistance) | gamma, gex, options, openinterest, dealerpositioning, gammaflip, callwall, putwall, volatility |

Begründung: Futures-Tools dorthin, wo die Volume-Zielgruppe sucht; die drei
Level-Tools teilen sich Support and Resistance (Familien-Eindruck im Profil);
„hougaard"/„fisher" NICHT als Tag (Promotion-Risiko — in der Beschreibung als
Methodenquelle ist es okay).

## Merkzettel Neu-Publikation
1. Frisches Chart-Layout, NUR das eine Skript, Standard-Farben, keine anderen
   Indikatoren/Zeichnungen.
2. Code aus dem Repo frisch in den Pine-Editor kopieren und als NEUES Skript
   speichern — der Skriptname im Editor wird der Publikationstitel-Vorschlag,
   die indicator()-Titel sind bereits exakt die Zieltitel.
3. Titel exakt wie oben (keine Versionsnummern noetig, kein Brand).
4. Beschreibung aus diesem Dokument, EN zuerst (DE darf optional darunter).
5. Release Notes kuenftig ebenfalls EN.
