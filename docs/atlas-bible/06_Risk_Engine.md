# Project Atlas

# 06 – Risk Engine

Version: 1.0

Status: Approved

Owner: Atlas Engineering Team

Related Documents:

* 00_Project_Vision.md
* 01_System_Architecture.md
* 02_Coding_Standards.md
* 03_Kernel.md
* 04_Data_Engine.md
* 05_Strategy_Engine.md

---

# 1. Purpose

The Risk Engine is responsible for protecting trading capital by evaluating every proposed trade before execution.

No trade may be executed without approval from the Risk Engine.

The Risk Engine does not generate trading signals and does not place broker orders.

Its sole responsibility is to evaluate whether a proposed trade satisfies all predefined risk rules.

---

# 2. Objectives

The Risk Engine shall:

* Validate every incoming trading signal.
* Calculate position size.
* Verify account risk limits.
* Enforce stop-loss requirements.
* Enforce daily risk limits.
* Monitor portfolio exposure.
* Approve or reject trades.
* Publish risk decisions.

---

# 3. Scope

Included in Version 1:

* Position sizing
* Risk per trade
* Maximum daily loss
* Maximum open positions
* Portfolio exposure
* Stop-loss validation
* Trade approval and rejection
* Risk event logging

Excluded from Version 1:

* Hedging
* Options Greeks
* Value at Risk (VaR)
* Margin optimization
* Multi-account allocation
* Cross-market exposure management

---

# 4. Design Principles

## 4.1 Capital Protection First

The primary objective of the Risk Engine is capital preservation.

Protecting trading capital always takes precedence over maximizing returns.

---

## 4.2 Independent Validation

The Risk Engine evaluates trades independently of the Strategy Engine.

It may reject any signal regardless of the confidence assigned by the strategy.

---

## 4.3 Deterministic Decisions

Given identical inputs and configuration, the Risk Engine must always produce the same decision.

---

## 4.4 Explainability

Every approval or rejection must include one or more reasons.

Examples:

* Position size exceeds limit.
* Daily loss limit reached.
* Missing stop-loss.
* Maximum exposure exceeded.
* Risk per trade exceeded.

---

# 5. Responsibilities

The Risk Engine is responsible for:

* Validating trade proposals.
* Calculating position size.
* Enforcing portfolio limits.
* Monitoring realized and unrealized risk.
* Publishing approval or rejection events.

The Risk Engine is not responsible for:

* Generating signals.
* Executing trades.
* Fetching market data.
* Learning or optimization.

---

# 6. Risk Evaluation Pipeline

Every proposed trade shall pass through the following sequence:

1. Validate signal.
2. Verify account status.
3. Verify market session.
4. Validate stop-loss.
5. Calculate position size.
6. Check maximum risk per trade.
7. Check daily loss limit.
8. Check portfolio exposure.
9. Check maximum open positions.
10. Approve or reject trade.

No step may be skipped.

---

# 7. Position Sizing

Version 1 shall support configurable position sizing methods.

Supported methods:

* Fixed quantity
* Fixed capital allocation
* Percentage of account equity
* Risk-based position sizing

Risk-based sizing shall be the default method.

---

# 8. Risk Rules

The following limits shall be configurable:

* Maximum risk per trade
* Maximum daily loss
* Maximum weekly loss (future)
* Maximum monthly loss (future)
* Maximum open positions
* Maximum portfolio exposure
* Maximum position value

The Risk Engine must reject trades that violate any configured rule.

---

# 9. Stop-Loss Validation

Every trade must include a proposed stop-loss.

The Risk Engine shall verify:

* Stop-loss exists.
* Stop-loss direction is valid.
* Stop-loss distance is reasonable.
* Calculated risk is within limits.

Trades without a valid stop-loss shall be rejected.

---

# 10. Portfolio Exposure

The Risk Engine shall monitor:

* Total capital deployed.
* Sector exposure (future).
* Instrument exposure.
* Available buying power.
* Cash balance.

Exposure limits must be configurable.

---

# 11. Event Integration

The Risk Engine subscribes to:

* SignalGenerated
* PositionOpened
* PositionClosed
* MarketClosed
* TradingSessionStarted
* TradingSessionEnded

The Risk Engine publishes:

* TradeApproved
* TradeRejected
* PositionSizeCalculated
* RiskLimitReached

---

# 12. Decision Model

Every decision shall include:

* Decision ID
* Timestamp
* Signal ID
* Instrument
* Decision (Approved / Rejected)
* Position Size
* Estimated Risk
* Estimated Reward (optional)
* Risk-Reward Ratio (if available)
* Reasons
* Applied Rules

All decisions shall be immutable after publication.

---

# 13. Error Handling

The Risk Engine must detect and report:

* Invalid account data
* Missing stop-loss
* Invalid position size
* Configuration errors
* Calculation failures

On failure, the default action is to reject the trade safely.

---

# 14. Performance Requirements

The Risk Engine shall:

* Evaluate trades with minimal latency.
* Support multiple concurrent signals.
* Produce deterministic results.
* Avoid unnecessary recalculations.

Performance improvements must never reduce correctness.

---

# 15. Security Requirements

The Risk Engine shall:

* Never expose account credentials.
* Never bypass configured limits.
* Log all approvals and rejections.
* Maintain an audit trail for every decision.

Risk rules may only be changed through configuration, not by trading strategies.

---

# 16. Testing Requirements

Unit tests shall cover:

* Position sizing calculations.
* Risk limit validation.
* Stop-loss validation.
* Exposure calculations.
* Trade approval logic.
* Trade rejection logic.

Integration tests shall verify:

* Signal processing.
* Event publication.
* Interaction with the Execution Engine.
* Portfolio updates.

Edge case testing shall include:

* Zero account balance.
* Large account sizes.
* Invalid stop-loss values.
* Simultaneous trade requests.

---

# 17. Acceptance Criteria

The Risk Engine is considered complete when:

* Every trade is evaluated.
* Position sizing is calculated correctly.
* Invalid trades are rejected.
* Valid trades are approved.
* Risk decisions are published.
* Audit logs are generated.
* Automated tests pass.
* Documentation is complete.

---

# 18. Future Enhancements

Future versions may include:

* Dynamic volatility-based risk limits.
* Portfolio optimization.
* Correlation-aware exposure management.
* Value at Risk (VaR).
* Kelly Criterion sizing.
* Machine learning-assisted risk scoring.
* Multi-account risk management.
* Real-time stress testing.

These enhancements require separate design approval and are outside the scope of Version 1.

---

# End of Document
