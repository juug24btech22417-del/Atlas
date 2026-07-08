# Project Atlas

# System Architecture

Version: 1.0

Status: Draft

Owner: Atlas Architecture Team

Related Documents:

* 00_Project_Vision.md

---

# 1. Purpose

This document defines the architecture of Project Atlas.

It establishes how every software component interacts, the responsibilities of each layer, and the engineering principles that govern the platform.

The objective is to ensure that Atlas remains modular, maintainable, scalable, testable, and extensible throughout its lifecycle.

This document serves as the single source of truth for architectural decisions.

No implementation should violate this document without an approved Architecture Decision Record (ADR).

---

# 2. Architectural Principles

Atlas follows these architectural principles.

## 2.1 Modularity

Every subsystem must be independently replaceable.

Examples:

* Database can be replaced.
* Broker can be replaced.
* Dashboard can be replaced.
* Strategy Engine can be replaced.

No module should depend directly on implementation details of another module.

---

## 2.2 Separation of Responsibilities

Every module has exactly one primary responsibility.

Examples:

Risk Engine

Responsible only for validating risk.

Strategy Engine

Responsible only for generating trading signals.

Execution Engine

Responsible only for sending and managing orders.

Learning Engine

Responsible only for research and performance analysis.

No module may assume responsibilities belonging to another module.

---

## 2.3 Event-Driven Communication

Modules communicate through events whenever possible.

Example:

New Candle Received

↓

Indicators Updated

↓

Strategy Evaluated

↓

Risk Validated

↓

Portfolio Approved

↓

Order Submitted

↓

Trade Recorded

↓

Analytics Updated

↓

Learning Updated

This architecture minimizes coupling and improves maintainability.

---

## 2.4 Explainability

Every trading decision must be explainable.

Atlas must be capable of reconstructing:

* Input data
* Indicator values
* Strategy decision
* Risk evaluation
* Portfolio decision
* Execution details

for every trade.

---

## 2.5 Deterministic Core

The core trading engine must remain deterministic.

Given identical market data and configuration, Atlas must produce identical trading decisions.

AI services may provide additional information but must not introduce non-deterministic behaviour into the execution engine.

---

## 2.6 Testability

Every component must support independent unit testing.

Integration testing must verify communication between components.

No component should require the full application to be running in order to be tested.

---

# 3. High-Level System Architecture

Atlas is organized into independent architectural layers.

+------------------------------------------------------+
| Dashboard Layer |
+------------------------------------------------------+

↓

+------------------------------------------------------+
| API Layer |
+------------------------------------------------------+

↓

+------------------------------------------------------+
| Decision Layer |
+------------------------------------------------------+

↓

+------------------------------------------------------+
| Research Layer |
+------------------------------------------------------+

↓

+------------------------------------------------------+
| Data Layer |
+------------------------------------------------------+

↓

+------------------------------------------------------+
| Atlas Kernel |
+------------------------------------------------------+

Each layer communicates only through defined interfaces.

Cross-layer dependencies are prohibited unless explicitly documented.

---

# 4. Layered Architecture

## 4.1 Atlas Kernel

Purpose:

Provide platform infrastructure.

Responsibilities:

* Configuration
* Logging
* Event Bus
* Dependency Injection
* Lifecycle
* Health Monitoring
* Service Discovery

The kernel contains no trading logic.

---

## 4.2 Data Layer

Purpose:

Acquire, validate and normalize market data.

Responsibilities:

* Historical data
* Live market feed
* Corporate actions
* Broker market data
* Feature generation

Outputs standardized market events.

---

## 4.3 Research Layer

Purpose:

Evaluate trading ideas.

Responsibilities:

* Indicator calculation
* Feature engineering
* Strategy development
* Backtesting
* Walk-forward analysis
* Monte Carlo simulation
* Experiment tracking

Produces validated research artefacts.

---

## 4.4 Decision Layer

Purpose:

Transform market information into trading decisions.

Components:

* Strategy Engine
* Risk Engine
* Portfolio Manager
* Position Sizing
* AI Confidence Service

The Decision Layer may reject trades at any stage if predefined conditions are not satisfied.

---

## 4.5 Execution Layer

Purpose:

Execute approved orders.

Responsibilities:

* Paper trading
* Broker integration
* Order management
* Trade synchronization
* Position updates

For Version 1, the execution layer will support the 5paisa broker through an adapter implementation.

---

## 4.6 Analytics Layer

Purpose:

Measure and explain system performance.

Responsibilities:

* Trade history
* Performance reports
* Equity curve
* Drawdown
* Trade replay
* Explainability reports

---

## 4.7 Learning Layer

Purpose:

Generate research insights from historical trades.

Responsibilities:

* Pattern discovery
* Performance clustering
* Strategy comparison
* Improvement proposals

The Learning Layer may recommend strategy improvements but must never modify production trading logic automatically.

---

## 4.8 Dashboard Layer

Purpose:

Provide user interaction.

Version 1 Dashboard:

* React frontend
* FastAPI backend
* Live WebSocket updates
* Portfolio overview
* Open positions
* Trade history
* Performance analytics
* System health
* Learning reports

The Dashboard must remain independent from trading logic and interact only through public APIs.

---

End of Sections 1–4.
Version 1.0 Draft.
