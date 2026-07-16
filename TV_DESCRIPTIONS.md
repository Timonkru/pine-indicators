# Republish-Paket (konforme EN-Beschreibungen)

Regeln beachtet: EN zuerst, Konzept+Originalität+Nutzung, KEINE Links/Brands,
Clean Chart (frisches Layout, NUR das jeweilige Skript!). Titel ohne
[KruegerAlgorithms]. **Status 16.07.: ALLE 6 Skripte UI-komplett EN** (inkl.
der im Code verglichenen Options-Strings). Gamma-Template (gamma-seed) bleibt
bewusst deutsch/privat — statische Tageswerte sind ohnehin nicht publizierbar
(Originality/Usefulness-Regel); falls es publiziert war: NICHT neu publizieren.

---

## EINHEITLICHES SCHEMA (Beschluss 16.07. — „man sieht, es gehört zusammen")

**Titel-Grammatik:** `<Werkzeug> — <Scope>`, Paare teilen sich den Wortstamm:

| Skript | Einheitlicher Titel |
|---|---|
| Futures-Vol-Profil | **Futures Volume Profile — CFD Charts** |
| Futures-Vol-VWAP | **Futures Volume VWAP + Bands — CFD Charts** |
| Kasse Alerts | **Session Candle Levels & Alerts — Intraday** |
| Kasse Swing | **Session Candle Levels & Alerts — Weekly Swing** |
| Fisher ACD | **Opening Range ACD + Pivot Ranges — Label-Free** |
| ONR Sessions | **Overnight Range & Sessions — Multi-Index** |

Die Paare (Futures Volume ×2, Session Candle Levels ×2, Range ×2) erkennen
sich am Stamm; alle sechs an der Em-Dash-Grammatik.

**Beschreibungs-Struktur (immer gleich):**
1. Ein Satz: was + für wen
2. „How it works" (Bullets)
3. „How to use it" (2-3 Sätze)
4. Einheitlicher Schlusssatz (in JEDE Beschreibung, TV-intern = regelkonform):

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

Usage: treat POC/VAH/VAL as the real participation levels behind your CFD
chart. Zone width is ATR-derived by default or fixed in points.

---

## 2) Futures Volume VWAP + Bands — CFD Charts

**Titel:** Futures Volume VWAP + Bands — CFD Charts

**Beschreibung (Copy-Paste):**

A session/week/month-anchored VWAP with 1/2/3-sigma bands — with a twist for
CFD traders: the VWAP weighting can use REAL futures exchange volume instead
of broker tick volume.

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

Usage: standard VWAP playbook (mean reversion toward VWAP, band extremes as
stretch zones) — but weighted by real market participation.

---

## 3) Opening Range ACD + Pivot Ranges — Label-Free

**Titel:** Opening Range ACD + Pivot Ranges — Label-Free

**Beschreibung:**

An implementation of Mark B. Fisher's ACD methodology ("The Logical Trader")
combined with daily/3-day/7-day pivot ranges — deliberately label-free: every
signal is expressed through color, line weight and box shading, so the chart
stays readable.

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
Alerts for confirmed A-levels, C-failures and pivot-range crosses.

## 4) Overnight Range & Sessions — Multi-Index

**Titel:** Overnight Range & Sessions — Multi-Index

**Beschreibung:**

Draws the overnight range (ONR) of the instrument's own overnight session —
not a generic fixed window — plus EU/US/Asia session boxes and the previous
day's cash close as a gap reference.

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
Use the ONR high/low as the breakout frame for the cash session.

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
All levels come with alert conditions, so the chart can stay closed.

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
Designed for daily/4h charts; all signals available as alert conditions.

## Merkzettel Neu-Publikation
1. Frisches Chart-Layout, NUR das eine Skript, Standard-Farben, keine anderen
   Indikatoren/Zeichnungen.
2. Titel exakt wie oben (keine Versionsnummern noetig, kein Brand).
3. Beschreibung aus diesem Dokument, EN zuerst (DE darf optional darunter).
4. Release Notes kuenftig ebenfalls EN.
