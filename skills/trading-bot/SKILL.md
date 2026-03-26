---
name: trading-bot
description: "Use this skill whenever working on the AI Forex and Gold (XAUUSD) trading bot project. Triggers include: any mention of trading bot, MT5, XAUUSD, forex bot, gold trading, confluence filter, strategy backtest, risk manager, self-learning loop, TradingAgents, prop firm, FTMO, vectorbt, or any file in agents/, strategies/, risk/, execution/, learning/, confluence/ folders. Load this skill before writing any trading logic, risk rules, strategy code, or MT5 execution code."
license: Private project — personal use only
---

# AI Forex + Gold Trading Bot Skill

## Overview

Multi-strategy automated trading bot for Forex and Gold (XAUUSD).
Runs on MetaTrader 5. Uses Claude AI for news sentiment analysis.
Self-learning via genetic algorithm. Target: prop firm funded account.

## Quick Reference

| Component | Location | Purpose |
|-----------|----------|---------|
| Data feeds | `data/feeds/` | MT5, OpenBB, Finnhub |
| AI agents | `agents/` | Claude-powered signal generators |
| Strategies | `strategies/` | EMA trend, mean reversion, session breakout |
| Confluence | `confluence/filter.py` | Decision engine — 2+ signals must agree |
| Risk | `risk/manager.py` | ALL risk rules — immutable |
| Execution | `execution/` | MT5 orders + state machine |
| Learning | `learning/` | Nightly self-improvement loop |
| Docs | `docs/` | Read before building any component |

---

## Phase Gate System
Never start a new phase unless ALL tests from the current phase pass.
Run: `pytest tests/ -v` — all green before advancing.

| Phase | What Gets Built | Gate Test |
|-------|----------------|-----------|
| 0 | Project skeleton, database | All imports resolve |
| 1 | Data layer (MT5, OpenBB, Finnhub) | Live tick from MT5 |
| 2 | Three strategies + backtests | profit_factor > 1.0 each |
| 3 | Claude news agent + confluence | Confluence blocks conflicts |
| 4 | Risk manager + MT5 execution | All 9 risk checks pass |
| 5 | Self-learning loop | Nightly loop runs clean |
| 6 | OpenBB dashboard | Dashboard connects |
| 7 | Paper trade 4+ weeks | Win rate > 50% |
| 8 | Prop firm challenge | Pass FTMO/MyFundedFX |

---

## Critical Rules — Never Break

```
1. No hardcoded API keys — always os.getenv() via python-dotenv
2. No trade placed without passing risk/manager.py first
3. No silent exceptions — always log with loguru then return None
4. No function longer than 50 lines
5. No magic numbers — all constants in config/settings.py
6. Type hints required on every function
7. Docstrings required on every function and class
8. Tests required for every risk rule and strategy
9. Search the web before assuming API behaviour
10. Read docs/ before building any component
```

---

## Risk Limits (Immutable)

```python
MAX_RISK_PER_TRADE    = 0.015   # 1.5% of account
MAX_DAILY_DRAWDOWN    = 0.04    # 4% — bot pauses (FTMO allows 5%)
MAX_TOTAL_DRAWDOWN    = 0.08    # 8% — bot shuts down (FTMO allows 10%)
MAX_TRADES_PER_DAY    = 5
MIN_RISK_REWARD       = 1.5     # Never trade below 1.5R
NEWS_BUFFER_MINUTES   = 30      # No trades around high-impact events
MAX_OPEN_POSITIONS    = 3
```

---

## Data Flow

```
MT5 live feed ──────────────► Strategy A (EMA trend)      ──┐
OpenBB historical ──────────► Strategy B (mean reversion)  ──┼─► Confluence Filter ──► Risk Manager ──► MT5 Execute
Finnhub news ───► Claude API ► News Agent (sentiment)       ──┘         │                                      │
CMC Connect API ─────────────► Pattern signal (bonus vote) ──┘           │                               Trade Logger
Econ calendar ───────────────────────────────────────────────────────────►                               Telegram Alert
                                                                                                               │
                                                                                            Nightly: Learning Loop ◄──┘
```

---

## Signal Object (Always This Shape)

```python
from dataclasses import dataclass
from datetime import datetime

@dataclass
class Signal:
    direction: str       # "BULLISH" | "BEARISH" | "NEUTRAL"
    confidence: float    # 0.0 to 1.0
    strategy_name: str   # "strategy_a" | "strategy_b" | "strategy_c" | "news_agent" | "cmc_pattern"
    timestamp: datetime
    reasoning: str = ""

# On any error or unclear condition — always return this:
NEUTRAL_SIGNAL = Signal(direction="NEUTRAL", confidence=0.0,
                        strategy_name="unknown", timestamp=datetime.utcnow())
```

---

## Strategy Rules

### Strategy A — EMA Trend Following
```python
# BUY when ALL THREE true:
ema20_crossed_above_ema50   # within last 3 bars
price_above_ema200          # long-term trend filter
rsi_between_45_and_65       # not overbought

# SELL when ALL THREE true:
ema20_crossed_below_ema50
price_below_ema200
rsi_between_35_and_55

# Everything else = NEUTRAL
```

### Strategy B — Mean Reversion
```python
# ONLY fires when ATR < 30-day ATR average (ranging market)
# BUY: price touches lower Bollinger Band AND RSI < 35
# SELL: price touches upper Bollinger Band AND RSI > 65
# In trending market (ATR above average) = always NEUTRAL
```

### Strategy C — Session Breakout
```python
# London window: 08:00–11:00 GMT only
# New York window: 13:00–16:00 GMT only
# Range = high/low of first 30 min after session open
# BUY: close above range high + volume > 20-bar average
# SELL: close below range low + volume > 20-bar average
# Outside windows = always NEUTRAL
```

### News Agent — Claude API
```python
# Model: claude-sonnet-4-20250514
# Per headline: direction + confidence + reasoning (JSON)
# Aggregate multiple headlines into single Signal
# Min confidence 0.6 to count as valid vote
# Cache 15 minutes to avoid repeat API calls
# On Claude API failure: return NEUTRAL_SIGNAL
```

---

## Confluence Filter Logic

```python
def should_trade(signals: list[Signal]) -> TradeDecision:
    # Count agreeing signals (confidence >= 0.6 only)
    bullish = [s for s in signals if s.direction == "BULLISH" and s.confidence >= 0.6]
    bearish = [s for s in signals if s.direction == "BEARISH" and s.confidence >= 0.6]

    # Block if news event within 30 minutes
    if not calendar.check_news_buffer():
        return TradeDecision(action="HOLD", reasoning="News buffer active")

    # Minimum 2 must agree, zero conflict
    if len(bullish) >= 2 and len(bearish) == 0:
        return TradeDecision(action="BUY", ...)
    elif len(bearish) >= 2 and len(bullish) == 0:
        return TradeDecision(action="SELL", ...)
    else:
        return TradeDecision(action="HOLD", reasoning="Insufficient confluence")
```

---

## Risk Manager — 9 Checks In Order

```python
# All must return True or trade is blocked + logged
1. check_no_existing_position(symbol)     # one trade per instrument
2. check_open_positions_count()           # max 3 simultaneous
3. check_trade_count()                    # max 5 per day
4. check_daily_drawdown(account_info)     # pause at 4%
5. check_total_drawdown(account_info)     # shutdown at 8%
6. check_news_buffer(calendar)            # 30 min either side
7. check_risk_reward(entry, sl, tp)       # min 1.5:1
8. calculate_position_size(...)           # max 1.5% risk
9. return approved = True                 # all passed
```

---

## MT5 State Machine (execution/mt5_bridge.py)

```
SCANNING ──► (signal detected) ──► ARMED
ARMED ──► (entry condition met) ──► WINDOW_OPEN
WINDOW_OPEN ──► (confirmed) ──► ACTIVE_TRADE
ACTIVE_TRADE ──► (TP or SL hit) ──► CLOSED
CLOSED ──► (log + alert) ──► SCANNING
```

---

## Self-Learning Loop (Runs Midnight UTC)

```
1. Pull last 14 days trades per strategy from SQLite
2. Calculate win rate per strategy
3. Adjust weights: >60% win rate → increase, <40% → decrease, <35% → suspend
4. Sunday night: run genetic algorithm (DEAP) to optimise parameters
5. Only adopt new params if profit_factor improves
6. Send Telegram morning summary
```

---

## API Keys Required (.env)

```bash
MT5_LOGIN=
MT5_PASSWORD=
MT5_SERVER=
ANTHROPIC_API_KEY=
FINNHUB_API_KEY=
CMC_API_KEY=
OPENBB_PAT=
ALPHA_VANTAGE_API_KEY=
TELEGRAM_BOT_TOKEN=
TELEGRAM_CHAT_ID=
```

---

## Source Code References (What We Borrow)

| What | From | GitHub |
|------|------|--------|
| State machine | ilahuerta-IA MT5 bot | github.com/ilahuerta-IA/mt5_live_trading_bot |
| Genetic algorithm | Mo-Khalifa Forex Bot | github.com/Mo-Khalifa96/Forex-Trading-Bot |
| Multi-agent design | TradingAgents | github.com/TauricResearch/TradingAgents |
| Backtesting | AutoTrader | github.com/kieran-mackle/AutoTrader |
| Indicator code | OpenBB Setup Guide PDF | docs.openbb.co |
| Kalman filter | QuantFrame article | quantframe.io/knowledge-hub/journal/pairs-trading-kalman-filter |

---

## Backtest Minimum Requirements

Before any strategy is used in confluence it must show:
- `profit_factor > 1.0`
- `win_rate > 0.45`
- `max_drawdown < 0.15`
- `total_trades > 50`
- Tested on minimum 2 years XAUUSD 5-minute data

---

## Final Products

| Product | What It Is | How You Access |
|---------|-----------|----------------|
| Bot | Python running on VPS | Runs headlessly 24/7 |
| MT5 terminal | Desktop trading app | Download from metatrader5.com |
| OpenBB dashboard | Bloomberg-style web UI | Browser at pro.openbb.co |
| Telegram alerts | Trade notifications | Your phone, instantly |
