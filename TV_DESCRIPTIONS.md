# Republish-Paket (konforme EN-Beschreibungen)

Regeln beachtet: EN zuerst, Konzept+Originalität+Nutzung, KEINE Links/Brands,
Clean Chart (frisches Layout, NUR das jeweilige Skript!). Titel ohne
[KruegerAlgorithms]. Status: 2 von 6 Skripten übersetzt (die beiden Futures-
Volumen-Tools); Queue: fisher_acd_addon, kasse_alerts, kasse_alerts_swing,
onr_multi_index_sessions (+ Gamma-Template in gamma-seed/build_seed.py).

---

## 1) Futures Volume Profile (CFD)

**Titel:** Futures Volume Profile (CFD)

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

## 2) VWAP + StdDev Bands (Futures Volume)

**Titel:** VWAP + StdDev Bands (Futures Volume)

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

## Merkzettel Neu-Publikation
1. Frisches Chart-Layout, NUR das eine Skript, Standard-Farben, keine anderen
   Indikatoren/Zeichnungen.
2. Titel exakt wie oben (keine Versionsnummern noetig, kein Brand).
3. Beschreibung aus diesem Dokument, EN zuerst (DE darf optional darunter).
4. Release Notes kuenftig ebenfalls EN.
