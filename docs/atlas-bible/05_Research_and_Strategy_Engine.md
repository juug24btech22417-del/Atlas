# Project Atlas

# 05 – Strategy Engine

Version: 1.0

Status: Approved

Owner: Atlas Engineering Team

Related Documents:

* 00_Project_Vision.md
* 01_System_Architecture.md
* 02_Coding_Standards.md
* 03_Kernel.md
* 04_Data_Engine.md

---

# 1. Purpose

The Strategy Engine is responsible for transforming processed market data into trading signals.

It evaluates market conditions using one or more trading strategies and produces standardized signals for the Risk Engine.

The Strategy Engine **does not execute trades** and **does not manage risk**.

Its sole responsibility is to identify potential trading opportunities.

---

# 2. Objectives

The Strategy Engine shall:

* Load one or more trading strategies.
* Process market data from the Data Engine.
* Generate buy, sell, or hold signals.
* Support multiple active strategies.
* Allow strategies to be enabled or disabled without code changes.
* Record why each signal was generated.
* Produce deterministic results given identical inputs.

---

# 3. Scope

Included in Version 1:

* Strategy plugin framework
* Signal generation
* Indicator management
* Multi-strategy support
* Signal validation
* Strategy configuration
* Strategy performance tracking

Excluded from Version 1:

* Order execution
* Position sizing
* Risk management
* Portfolio optimization
* Automatic strategy modification

---

# 4. Design Principles

## 4.1 Strategy Isolation

Each strategy must operate independently.

A failure in one strategy must not affect any other strategy.

---

## 4.2 Plugin Architecture

Strategies are plugins.

New strategies can be added without modifying the Strategy Engine.

Each strategy implements a common interface.

---

## 4.3 Deterministic Behaviour

Given identical market data and configuration, the same strategy must always produce the same signals.

---

## 4.4 Explainability

Every signal must include:

* Strategy name
* Strategy version
* Timestamp
* Instrument
* Timeframe
* Signal type
* Confidence (if provided)
* Reasons for the signal

---

# 5. Responsibilities

The Strategy Engine is responsible for:

* Receiving processed market data.
* Updating indicators.
* Evaluating strategy rules.
* Generating trading signals.
* Publishing signals to the Event Bus.
* Recording signal metadata.

The Strategy Engine is **not** responsible for:

* Approving trades
* Rejecting trades
* Calculating position size
* Sending broker orders
* Managing open positions

---

# 6. Strategy Lifecycle

Every strategy follows this lifecycle:

1. Load configuration.
2. Initialize indicators.
3. Receive market data.
4. Update internal state.
5. Evaluate trading conditions.
6. Generate signal (if applicable).
7. Publish signal.
8. Wait for the next market event.

---

# 7. Strategy Interface

Every strategy plugin must implement:

* initialize()
* on_market_open()
* on_market_data()
* generate_signal()
* on_trade_closed()
* reset()
* shutdown()

All strategies must expose metadata including:

* Name
* Version
* Author
* Description
* Supported timeframes
* Supported instruments

---

# 8. Signal Model

Every generated signal must contain:

* Signal ID
* Strategy ID
* Instrument
* Exchange
* Timeframe
* Timestamp
* Signal Type (BUY / SELL / HOLD)
* Entry Price
* Stop Loss (proposed)
* Target (optional)
* Confidence Score (optional)
* Supporting Indicators
* Explanation

Signals must be immutable after publication.

---

# 9. Indicator Management

Indicators are shared resources managed by the Strategy Engine.

Examples include:

* EMA
* SMA
* RSI
* MACD
* VWAP
* ATR
* Bollinger Bands
* Supertrend
* ADX
* Volume Profile (future)

Indicators must:

* Be reusable.
* Be independently testable.
* Avoid duplicate calculations.
* Cache results where appropriate.

---

# 10. Multi-Strategy Support

Atlas Version 1 supports multiple strategies running simultaneously.

Each strategy:

* Operates independently.
* Maintains separate state.
* Produces separate signals.
* Records separate performance statistics.

Signal conflicts are resolved by downstream modules (Risk Engine / Portfolio Engine), not by the Strategy Engine.

---

# 11. Event Integration

The Strategy Engine subscribes to:

* MarketDataUpdated
* MarketOpened
* MarketClosed
* TradingSessionStarted
* TradingSessionEnded

The Strategy Engine publishes:

* SignalGenerated
* StrategyLoaded
* StrategyFailed
* StrategyDisabled

---

# 12. Performance Requirements

The Strategy Engine should:

* Minimize duplicate indicator calculations.
* Handle multiple strategies efficiently.
* Produce signals with low latency suitable for intraday trading.

Performance optimizations must not compromise correctness or reproducibility.

---

# 13. Error Handling

The Strategy Engine must:

* Catch strategy-specific failures.
* Isolate failing strategies.
* Log diagnostic information.
* Continue operating with remaining healthy strategies where safe.

A faulty strategy plugin must never crash the entire platform.

---

# 14. Testing Requirements

Every strategy must include:

* Unit tests
* Signal generation tests
* Indicator validation tests
* Edge case tests
* Historical replay tests

The Strategy Engine must include integration tests covering:

* Multiple strategies
* Plugin loading
* Event publication
* Signal generation

---

# 15. Acceptance Criteria

The Strategy Engine is considered complete when:

* Strategies load correctly.
* Plugins follow the required interface.
* Signals are generated consistently.
* Signal metadata is complete.
* Events are published successfully.
* Multiple strategies can run concurrently.
* All automated tests pass.
* Documentation is complete.

---

# 16. Future Enhancements

Future versions may include:

* Machine learning-assisted signal generation.
* Strategy marketplace.
* Dynamic strategy scheduling.
* Portfolio-aware strategy coordination.
* Cross-asset strategies.
* Distributed strategy execution.
* Genetic algorithm strategy research (research environment only).

These enhancements require separate design approval and are outside the scope of Version 1.

---

# End of Document
