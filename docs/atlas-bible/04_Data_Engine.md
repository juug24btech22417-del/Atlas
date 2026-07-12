# Project Atlas

# 04 – Data Engine

Version: 1.0

Status: Approved

Owner: Atlas Engineering Team

Related Documents:

* 00_Project_Vision.md
* 01_System_Architecture.md
* 02_Coding_Standards.md
* 03_Kernel.md

---

# 1. Purpose

The Data Engine is responsible for acquiring, validating, normalizing, storing, and distributing market data throughout Atlas.

It acts as the single source of truth for all market-related information.

No module within Atlas may communicate directly with external market data providers.

All market data must pass through the Data Engine.

---

# 2. Objectives

The Data Engine shall:

* Connect to market data providers.
* Acquire historical and live market data.
* Validate incoming data.
* Normalize data into a common format.
* Store historical records.
* Publish market events.
* Prevent duplicate or corrupted data.
* Provide a consistent interface to downstream modules.

---

# 3. Scope

Included in Version 1:

* Historical OHLCV data
* Live market data
* Multiple timeframes
* Data validation
* Data normalization
* Local caching
* PostgreSQL persistence
* Event publication

Excluded from Version 1:

* News feeds
* Alternative data
* Options chain data
* Fundamental financial statements
* Satellite or sentiment datasets

---

# 4. Design Principles

## 4.1 Single Source of Truth

Every module must obtain market data exclusively through the Data Engine.

Direct requests to broker APIs or external providers are prohibited outside this module.

---

## 4.2 Provider Independence

The Data Engine must remain independent of any specific provider.

Supported providers may include:

* 5paisa
* NSE data sources
* Future commercial data providers

Changing a provider must not require changes to downstream modules.

---

## 4.3 Data Integrity

Incoming data must be validated before distribution.

Checks include:

* Missing timestamps
* Duplicate candles
* Invalid prices
* Negative volume
* Out-of-order records
* Corrupted payloads

Invalid data must never be propagated.

---

## 4.4 Normalization

All incoming market data shall be converted into a common internal format.

Regardless of provider, downstream modules must receive identical data structures.

---

# 5. Responsibilities

The Data Engine is responsible for:

* Historical data retrieval
* Live data streaming
* Data validation
* Data normalization
* Local storage
* Data caching
* Event publication

The Data Engine is not responsible for:

* Strategy decisions
* Risk calculations
* Order execution
* Portfolio management

---

# 6. Components

## 6.1 Historical Data Manager

Responsibilities:

* Download historical OHLCV data.
* Resume interrupted downloads.
* Prevent duplicate records.
* Verify data completeness.

---

## 6.2 Live Market Feed

Responsibilities:

* Receive real-time market updates.
* Monitor connection status.
* Detect disconnections.
* Reconnect safely.
* Publish normalized market events.

---

## 6.3 Data Validator

Responsibilities:

* Verify timestamps.
* Verify OHLC relationships.
* Verify volume values.
* Detect missing candles.
* Reject corrupted data.

---

## 6.4 Data Normalizer

Responsibilities:

Convert all incoming data into Atlas Market Candle format.

Required fields:

* Symbol
* Exchange
* Timestamp
* Open
* High
* Low
* Close
* Volume
* Source

---

## 6.5 Cache Manager

Responsibilities:

* Reduce repeated requests.
* Improve performance.
* Store frequently accessed data.
* Manage cache expiration.

---

## 6.6 Database Manager

Responsibilities:

* Store validated market data.
* Query historical records.
* Maintain indexing.
* Support efficient backtesting queries.

Version 1 database:

PostgreSQL

---

# 7. Supported Timeframes

Version 1 supports:

* 1 Minute
* 3 Minute
* 5 Minute
* 15 Minute
* 30 Minute
* 1 Hour
* 1 Day

Future versions may support custom intervals.

---

# 8. Event Integration

The Data Engine publishes:

* MarketDataReceived
* CandleClosed
* MarketOpened
* MarketClosed
* FeedConnected
* FeedDisconnected
* HistoricalDataLoaded

The Data Engine subscribes to:

* ApplicationStarted
* ApplicationStopped

---

# 9. Data Flow

1. Connect to data provider.
2. Receive raw market data.
3. Validate payload.
4. Normalize records.
5. Store validated data.
6. Update cache.
7. Publish market event.
8. Notify subscribed modules.

---

# 10. Database Requirements

The Data Engine stores:

* Instruments
* OHLCV candles
* Trading sessions
* Data source metadata
* Import history

Indexes should prioritize:

* Symbol
* Timestamp
* Timeframe

The database schema must support efficient historical retrieval.

---

# 11. Error Handling

The Data Engine must detect and respond to:

* Provider outages
* Network failures
* Duplicate records
* Corrupted payloads
* Missing candles
* Database failures

Errors must be logged with sufficient context for investigation.

---

# 12. Performance Requirements

The Data Engine should:

* Minimize unnecessary API requests.
* Support efficient historical loading.
* Handle live data with low latency suitable for intraday trading.
* Scale to multiple instruments without degrading significantly.

Performance optimizations must not compromise data integrity.

---

# 13. Security Requirements

The Data Engine must:

* Store provider credentials securely.
* Never log API secrets.
* Validate external input.
* Protect against malformed payloads.

---

# 14. Testing Requirements

Unit tests shall cover:

* Data validation
* Normalization
* Cache behavior
* Database operations

Integration tests shall verify:

* Provider connectivity
* Historical imports
* Live feed handling
* Event publication
* Recovery from connection loss

---

# 15. Acceptance Criteria

The Data Engine is considered complete when:

* Historical data loads correctly.
* Live market data streams reliably.
* Invalid data is rejected.
* Data is normalized consistently.
* PostgreSQL storage functions correctly.
* Events are published successfully.
* Automated tests pass.
* Documentation is complete.

---

# 16. Future Enhancements

Future versions may include:

* Multiple simultaneous data providers.
* Automatic provider failover.
* Tick-level market data.
* Options chain support.
* Market depth (Level II) data.
* Corporate actions processing.
* News and sentiment feeds.
* Distributed data ingestion.

These enhancements require separate design approval and are outside the scope of Version 1.

---

# End of Document
