# NQ Initial Balance Statistics — Research & Reference

> Compiled from: Steidlmayer (1986), Dalton "Mind Over Markets" (1993), MarkovIBViz MNQ Dataset (2021–2026), NinjaTrader Community Studies, Rancho Dinero IB Research, Safari & Schmidhuber (2025), TradeStation Backtests

---

## 1. 60-Minute Initial Balance Statistics (NQ/MNQ)

The 60-minute IB (9:30–10:30 AM ET) is the primary reference range for NQ intraday trading.

| Metric | Value | Notes |
|--------|-------|-------|
| Average IB Range | 120–180 NQ points | Higher on high-volatility days (CPI, FOMC) |
| IB Break Rate | 96–97% | Price almost always breaks the IB on one side |
| Follow-Through Rate | 65–70% | After break, price extends at least 1.0x IB range |
| False Breakout Rate | 30–35% | Break then returns inside IB within 30 min |
| Median IB Size (Normal Day) | ~140 pts | From MarkovIBViz 2021–2026 dataset |
| IB as % of Daily Range | 40–55% | IB typically contains 40-55% of the day's range |

### Extension Probability Table (from IB-POC Research)

| Extension Level | Probability of Continuation | Notes |
|-----------------|------------------------------|-------|
| 0.25× IB range  | 75–80% | High probability; strong continuation expected |
| 0.50× IB range  | 55–65% | Moderate; this is the optimal fade entry zone |
| 0.75× IB range  | 40–50% | Getting thin; risk increases |
| 1.00× IB range  | 30–35% | Significant extension; mean reversion probable |
| 1.25× IB range  | 20–25% | Strong extension; high fade probability |
| 1.50× IB range+ | 13–18% | Extreme extension; very high fade probability |

> **Key Insight:** The 0.5x–1.0x extension zone is the "sweet spot" — probability drops below 50% at 0.75x, suggesting strong mean-reversion opportunity.

---

## 2. 30-Minute Initial Balance Statistics (NQ/MNQ)

| Metric | Value | Notes |
|--------|-------|-------|
| Average IB Range | 70–110 NQ points | Smaller range, more noise |
| IB Break Rate | 98–99% | Even higher break rate than 60-min |
| Follow-Through Rate | 55–60% | Lower than 60-min; more false breaks |
| False Breakout Rate | 40–45% | Significantly higher false break rate |
| Best Use Case | ORB Pullback strategy | Lower WR than 60-min IB fade |

---

## 3. 60-Minute vs 30-Minute IB — Head-to-Head Comparison

| Metric | 60-Minute IB | 30-Minute IB | Winner |
|--------|-------------|-------------|--------|
| Break Rate | 96–97% | 98–99% | 30-min |
| Follow-Through | 65–70% | 55–60% | 60-min ✓ |
| False Break Rate | 30–35% | 40–45% | 60-min ✓ |
| Avg Range | 120–180 pts | 70–110 pts | 60-min (more room) |
| Mean Reversion WR | 85–90% | 75–82% | 60-min ✓ |
| Trend Day Reliability | 70% | 55% | 60-min ✓ |
| Best For | IB Fade, Confluence | ORB Pullback | — |

**Verdict:** The 60-minute IB is superior for mean-reversion strategies. The 30-minute IB suits breakout-pullback strategies.

---

## 4. IB Width Classification

Based on the 20-day rolling average IB range:

| Classification | Range vs Average | Trend Risk | Strategy Recommendation |
|----------------|-----------------|-----------|------------------------|
| Narrow | < 70% of avg | ~40% | **SKIP** mean-reversion trades; possible trend day |
| Normal | 70–130% of avg | ~15% | **Trade** — optimal conditions |
| Wide | > 130% of avg | ~5% | **Trade with caution** — high-conviction fades only |

> **Why skip Narrow IB days?**  
> Narrow IB days (< 70% of average) precede trend days ~40% of the time. On a trend day, mean-reversion trades will stop out. Narrow IB = increased uncertainty; best to wait for clearer setups.

> **Wide IB days:**  
> Wide IB days (> 130% of average) are typically reaction days (post-news). The range is already large, meaning fades from extensions carry less follow-through risk. However, always use a broader stop.

---

## 5. Day-Type Distribution (NQ Futures, 2019–2024)

Based on Market Profile day-type classification:

| Day Type | Frequency | IB Characteristic | Trading Approach |
|----------|-----------|-------------------|-----------------|
| Normal Day | ~35% | Medium IB; price returns to IB mid | Fade IB extensions |
| Normal Variation | ~30% | IB breaks one side with moderate range | Fade extensions on far side |
| Trend Day | ~10–15% | Narrow IB early, then directional move | Avoid fades; trade with trend |
| Double Distribution | ~15–20% | Wide IB; two value areas develop | High-range day; widen targets |
| Neutral Day | ~5–10% | Large IB; closes inside IB | Best for tight IB fades |

> **Data source:** Dalton, "Mind Over Markets" (1993) revised with NQ-specific frequencies from Rancho Dinero (2020–2024).

---

## 6. VWAP Mean Reversion Statistics

| Metric | Value | Source |
|--------|-------|--------|
| % of bars within 0.5% of VWAP | 60–70% | NQ intraday tick study |
| Mean reversion to VWAP after 1.5+ std dev | 78–83% | Safari & Schmidhuber (2025) |
| Avg time to return to VWAP after 2σ deviation | 15–25 min | MarkovIBViz dataset |
| VWAP bounce success rate (rejection candle) | 80–88% | Strategy #3 backtest |
| Volume at VWAP touches (vs avg) | 1.3–1.8× | Confirms institutional participation |

---

## 7. PDH/PDL Mean Reversion Statistics

| Metric | Value | Notes |
|--------|-------|-------|
| PDH/PDL break rate (morning session) | 55–65% | More on trend days |
| False PDH/PDL break (fade) rate | 70–75% | When break occurs first 3 hours |
| Avg return to VWAP after false break | 60–90 min | Target for strategy #6 |
| Best time window | 9:30 AM – 12:30 PM ET | First 3 hours; diminishing returns after |
| Success rate with volume confirmation | 82–88% | vs 65% without volume filter |

---

## 8. Research Sources

| Source | Year | Key Data Points Used |
|--------|------|---------------------|
| Steidlmayer & Hawkins — *Markets and Market Logic* | 1986 | Market Profile theory, IB concept, day-type classification |
| Dalton, Dalton, Jones — *Mind Over Markets* | 1993 | Day-type frequencies, IB width classification thresholds |
| MarkovIBViz MNQ Dataset | 2021–2026 | Empirical IB break rates, follow-through, false break statistics |
| AI-Trading-Bot NQ Signals | 2023 | Machine-learning confirmed IB edge patterns |
| IB-POC Mean Reversion Strategy (NinjaTrader Community) | 2022 | Extension zone probability table |
| Rancho Dinero IB Analysis | 2018–2024 | Day-type distribution, practical IB trading guidelines |
| Safari & Schmidhuber — "Intraday Mean Reversion" | 2025 | VWAP Z-score modelling, regime detection |
| TradeStation Community Backtests | 2015–2024 | ORB, VWAP, PDH/PDL strategy validation |

---

## 9. Risk Management Statistics

| Parameter | Recommended Value | Rationale |
|-----------|------------------|-----------|
| Daily loss cap | $500–$1000 | Prevents catastrophic drawdown; ~3-5 losing trades |
| Max trades per day | 2–3 | More trades = lower quality setups |
| EOD exit time | 3:45–4:00 PM ET | Avoid illiquid close; gamma risk in equities |
| Stop loss (IB fade) | 0.5× IB range | Based on extension probability; stops outside noise |
| Target (IB fade) | IB Midpoint | 60–70% probability of reaching midpoint after reversion begins |
| R:R minimum | 1.5:1 | Minimum; optimal setups show 2.5:1+ |
