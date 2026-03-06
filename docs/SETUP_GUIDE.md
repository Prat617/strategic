# TradingView Setup Guide

> Step-by-step instructions for loading, configuring, and backtesting the NQ/ES Elite Trading Strategy System on TradingView.

---

## Prerequisites

- TradingView account (free or paid — some features require Pro/Pro+)
- Access to NQ, MNQ, ES, or GC futures data (requires a data subscription or broker connection)
- Pine Script v6 editor (available to all TradingView users)

---

## 1. How to Load a Strategy

### Step 1: Open a Chart
1. Log in to [TradingView.com](https://www.tradingview.com)
2. In the top search bar, type the instrument symbol:
   - NQ futures: `NQ1!` (continuous) or `CME:NQ1!`
   - MNQ futures: `MNQ1!`
   - ES futures: `ES1!`
   - Gold futures: `GC1!` (for Strategy #2)

### Step 2: Set the Timeframe
Each strategy works on a **5-minute chart** for signal entries (the IB is tracked internally via timezone logic):

| Strategy | Recommended Chart Timeframe |
|----------|---------------------------|
| #1 IB Mean Reversion Fade | 5-min |
| #2 Gold IB Retracement | 5-min |
| #3 VWAP Bounce/Rejection | 5-min |
| #4 Volatility Reversion Z-Score | 5-min (pane below chart) |
| #5 ORB Pullback | 5-min |
| #6 PDH/PDL Fade | 5-min |
| #7 IB + VWAP Confluence | 5-min |

### Step 3: Open Pine Script Editor
1. At the bottom of TradingView, click the **"Pine Editor"** tab
2. Click **"Open"** → **"New blank indicator"**
3. Delete all default code in the editor

### Step 4: Paste the Strategy Code
1. Open the desired `.pine` file from the `strategies/` folder
2. Copy all the code (Ctrl+A, Ctrl+C)
3. Paste into the TradingView Pine Editor (Ctrl+V)

### Step 5: Add to Chart
1. Click **"Add to chart"** (the button at the top-right of the editor)
2. The strategy will appear on your chart with default settings
3. A **Strategy Tester** tab will appear at the bottom

---

## 2. Configuring Inputs

Click the **⚙️ Settings** icon on the strategy panel (or double-click the strategy name on the chart) to open the settings.

### IB Settings (Strategies #1, #2, #7)

| Parameter | Default | Explanation |
|-----------|---------|-------------|
| IB Start Hour/Minute | 9:30 | RTH open time in ET. Do not change for US futures. |
| IB End Hour/Minute | 10:30 | End of 60-min IB window. |
| IB Avg Lookback | 20 days | Days used for IB width classification average. |
| Narrow IB Threshold | 70% | IB < 70% of avg = "Narrow" → strategy skips day. |
| Wide IB Threshold | 130% | IB > 130% of avg = "Wide" day classification. |

### Entry Logic (all strategies)

| Parameter | Default | Explanation |
|-----------|---------|-------------|
| Extension Min/Max | 0.5× / 1.0× | Range beyond IB where fade entries are valid. |
| Volume Confirm Multiplier | 1.2× | Entry bar volume must be > 1.2× 20-bar average. |
| MA Type / Length | EMA / 50 | Trend filter; longs only above MA, shorts below. |
| VWAP Proximity % | 0.2–0.5% | How close price must be to VWAP for confluence. |
| Rejection Wick Ratio | 1.5–2.0× | Lower/upper wick must be 1.5–2× the candle body. |

### Exit / Risk Management (all strategies)

| Parameter | Default | Explanation |
|-----------|---------|-------------|
| Stop Loss | 0.5× IB Range | ATR or IB-based stop distance from entry. |
| Take Profit | IB Midpoint / VWAP | Logical target; closest key level to entry. |
| EOD Exit Hour/Minute | 15:45 (3:45 PM ET) | All positions closed before end of day. |
| Max Daily Loss | $500 | Strategy stops taking new trades after this loss. |
| Max Trades Per Day | 2–3 | Prevents overtrading on slow days. |

### Visual Settings (all strategies)

| Parameter | Default | Explanation |
|-----------|---------|-------------|
| Show IB Box | true | Draws a box covering the 60-min IB range. |
| Show Extension Lines | true | Dashed lines at 0.5× and 1.0× IB extensions. |
| Show Dashboard | true | Table in top-right corner with live stats. |
| Show VWAP Line | true | Blue line for session VWAP (where applicable). |

---

## 3. Backtesting Tips

### Recommended Settings
1. Open **Strategy Tester** → click **Settings** (gear icon)
2. Set:
   - **Initial Capital:** $100,000
   - **Order Size:** 1 contract (or adjust to your risk tolerance)
   - **Commission:** $2.50 per contract (one-way) — realistic for ES/NQ retail
   - **Slippage:** 1 tick — conservative for liquid futures
3. Set **Date Range:** Last 2–3 years for statistically significant results

### What to Look For
| Metric | Target | Notes |
|--------|--------|-------|
| Win Rate | ≥ 80% | Core requirement; below 75% suggests parameter over-fit |
| Profit Factor | ≥ 2.0 | Total gross profit / total gross loss |
| Max Drawdown | < 15% of capital | Higher drawdown = more aggressive sizing needed |
| Avg Trade | Positive and > 2× commission | Should clear costs by a wide margin |
| Sharpe Ratio | > 1.5 | Risk-adjusted return quality |
| Total Trades | ≥ 100 | Fewer trades = less statistical significance |

### Common Backtesting Pitfalls
- **Lookahead bias:** All strategies use `process_orders_on_close=false` to prevent unrealistic fills
- **Data quality:** Use `NQ1!` (continuous contract) for long-term backtests; gap adjustments may affect results
- **Overnight gaps:** All strategies exit EOD to avoid gap risk; backtest results should reflect this
- **Holiday sessions:** Low-volume holiday sessions can skew results; consider excluding major holidays

---

## 4. Which Strategy to Use When

### Decision Tree

```
Is it a Trend Day (Narrow IB)? 
    YES → Skip all mean-reversion strategies. Wait for tomorrow.
    NO  → Continue...

Do you have 60-min IB confirmed?
    YES → Use Strategy #1 (IB Fade) or #7 (Confluence)
    NO  → Use Strategy #3 (VWAP Bounce) or #5 (ORB Pullback)

Is price near VWAP?
    YES → Strategy #3 (VWAP Bounce) or #7 (if IB is also complete)
    NO  → Strategy #4 (Z-Score) if Z > 1.8 or Z < -1.8

Is it before 12:30 PM ET near PDH/PDL?
    YES → Strategy #6 (PDH/PDL Fade) — first 3 hours only
    NO  → Stick to VWAP/IB strategies
    
Trading Gold?
    → Strategy #2 (Gold IB Retracement) only
```

### When to Use Each Strategy

| Strategy | Best Conditions | Avoid When |
|----------|----------------|-----------|
| #1 IB Mean Reversion | Normal/Wide IB, medium volatility | Narrow IB, trend days, news days |
| #2 Gold IB Retracement | Gold IB in 0.3–1.5% range | IB range outside filter, major gold news |
| #3 VWAP Bounce | Price testing VWAP with rejection candle | Strong trend day, price far from VWAP |
| #4 Z-Score Reversion | Normal regime (ATR < 80th pct) | High volatility regime, earnings, FOMC |
| #5 ORB Pullback | Confirmed break + pullback to OR level | Inside day, narrow opening range |
| #6 PDH/PDL Fade | Morning session only, clear PDH/PDL spike | After 12:30 PM, sustained trend through PDH/PDL |
| #7 IB+VWAP Confluence | Score ≥ 6/10, medium/wide IB | Low score (< 4), narrow IB, high volatility |

---

## 5. Multi-Strategy Portfolio Approach

For maximum diversification, run multiple strategies simultaneously on different instruments:

| Slot | Strategy | Instrument | Max Daily Risk |
|------|----------|-----------|---------------|
| A | #7 IB+VWAP Confluence | NQ1! / MNQ1! | $500 |
| B | #5 ORB Pullback | ES1! | $300 |
| C | #3 VWAP Bounce | NQ1! | $300 |
| D | #2 Gold IB Retracement | GC1! | $200 |

> Total daily risk cap: ~$1,300. Adjust position sizes to match your account size and risk tolerance.

---

## 6. Common Troubleshooting

### "Strategy shows no trades"
- Verify the chart timeframe is **5-min** (not 1-min or 15-min)
- Check that the date range includes RTH sessions (Mon–Fri, excluding major holidays)
- Ensure the instrument is a **futures contract** (NQ1!, not NDX or QQQ)
- Confirm timezone is correct — all strategies assume US Eastern Time via `"America/New_York"`

### "IB is never completing"
- The IB completes at 10:30 AM ET (60-min) or 10:00 AM ET (30-min ORB)
- If you're testing pre-market data, the strategy waits for the first bar at/after 9:30 AM ET
- Ensure the chart has data during RTH hours (some free accounts have delayed data)

### "Win rate is much lower than documented"
- Check `Commission` and `Slippage` settings — unrealistic values inflate trade count
- Verify the date range is not dominated by a major trend year (2022 bear market may lower WR for mean-reversion)
- Try increasing `IB Avg Lookback` to 30 days for more stable classification

### "Dashboard shows '—' for all values"
- The dashboard populates only after the IB window completes (after 10:30 AM ET)
- For Strategy #4 (Z-Score), the dashboard is in the **indicator pane** (overlay=false)
- Ensure `Show Dashboard` is checked in Visual Settings

### "Pine Script error: division by zero"
- All strategies have division-by-zero guards; if you see this, check that you haven't set any multiplier inputs to 0
- The `IB Avg Lookback` needs at least 1 completed IB day to calculate averages

---

## 7. Alerts Setup

Each strategy includes pre-built `alertcondition()` calls. To activate:

1. With the strategy on your chart, click the **Bell (🔔)** icon in the top toolbar
2. Click **"Add Alert"**
3. Under **Condition**, select the strategy name
4. Choose the specific alert condition (e.g., "IB Complete", "Long Entry", "EOD Exit")
5. Set notification method (email, push notification, webhook)
6. Click **"Create"**

Recommended alerts for live trading:
- **IB Complete** — know when to start watching for entries
- **Long/Short Entry** — real-time trade notification
- **EOD Exit** — reminder to verify flat position
