# Project Atlas

# 02 – Coding Standards

Version: 1.0

Status: Approved

Owner: Atlas Engineering Team

Related Documents:

* 00_Project_Vision.md
* 01_System_Architecture.md

---

# 1. Purpose

This document defines the engineering standards that every source file, module, service, and component within Project Atlas must follow.

The purpose of these standards is to ensure Atlas remains:

* Readable
* Maintainable
* Testable
* Scalable
* Secure
* Consistent

These standards apply to every contributor, whether human or AI.

No implementation may be merged unless it complies with this document.

---

# 2. Engineering Philosophy

Atlas is designed to be a long-term quantitative trading platform rather than a short-term prototype.

Engineering decisions must prioritize:

* Correctness over speed
* Simplicity over cleverness
* Readability over brevity
* Reliability over optimization
* Evidence over assumptions

Every module should be understandable by a new contributor without requiring tribal knowledge.

---

# 3. General Principles

## 3.1 Single Responsibility Principle

Each module should have one clearly defined responsibility.

Example:

Configuration Manager

Responsible only for loading, validating, and providing configuration.

Not responsible for logging.

Not responsible for trading.

---

## 3.2 Loose Coupling

Modules communicate only through defined interfaces or events.

Direct dependencies between unrelated modules are prohibited.

---

## 3.3 High Cohesion

Related functionality must remain together.

Avoid utility classes that accumulate unrelated methods.

---

## 3.4 Composition over Inheritance

Prefer composing small components instead of creating deep inheritance hierarchies.

---

# 4. Python Version

Atlas Version 1 targets:

Python 3.12+

No compatibility with unsupported Python versions is required.

---

# 5. Project Structure

Every source file must belong to a clearly defined package.

Example:

src/

core/

data/

execution/

broker/

analytics/

dashboard/

strategies/

risk/

portfolio/

learning/

shared/

tests/

No source file should exist outside the defined package hierarchy without architectural approval.

---

# 6. Naming Conventions

## Packages

Lowercase.

Example:

broker

execution

analytics

---

## Modules

snake_case.py

Example:

position_manager.py

risk_engine.py

trade_logger.py

---

## Classes

PascalCase

Example:

TradeManager

RiskEngine

PositionSizer

---

## Functions

snake_case()

Example:

calculate_position_size()

validate_order()

generate_signal()

---

## Variables

snake_case

Example:

trade_price

account_balance

risk_score

---

## Constants

UPPER_CASE

Example:

DEFAULT_TIMEOUT

MAX_POSITION_SIZE

---

## Private Members

Prefix with a single underscore.

Example:

_load_configuration()

_internal_state

---

# 7. Type Hints

All public functions must use type hints.

Example:

def calculate_position_size(
capital: float,
risk_percent: float
) -> int:

No public API may omit type annotations.

---

# 8. Docstrings

Every public module, class, and function must include a docstring.

Use Google-style docstrings.

Example:

"""
Calculate position size.

Args:
capital: Available trading capital.
risk_percent: Maximum risk percentage.

Returns:
Number of shares.
"""

Docstrings should explain intent rather than repeat code.

---

# 9. Imports

Imports must follow this order:

1. Standard Library
2. Third-party Packages
3. Internal Atlas Modules

Example:

import os
from pathlib import Path

import pandas as pd

from atlas.core.config import ConfigManager

Wildcard imports are prohibited.

Example:

from module import *

is never allowed.

---

# 10. Formatting

Formatting is enforced automatically.

Tools:

Black

isort

ruff

No manually formatted code should override automated formatting.

---

# 11. Line Length

Maximum line length:

100 characters

Exceptions require justification.

---

# 12. Function Design

Functions should:

Perform one task.

Return predictable outputs.

Avoid side effects.

Recommended maximum length:

50 lines

Longer functions should be refactored.

---

# 13. Class Design

Classes should model one concept.

Avoid "God Objects."

Example:

RiskEngine

Should not calculate indicators.

Should not execute trades.

Should only evaluate risk.

---

# 14. Configuration

Configuration values must never be hardcoded.

Use:

Environment variables

Configuration files

Secret managers (future)

Examples:

Broker credentials

API tokens

Database passwords

must never appear in source code.

---

# 15. Logging

Production code must never use print().

All runtime information must use the Atlas Logging Engine.

Log levels:

DEBUG

INFO

WARNING

ERROR

CRITICAL

Sensitive information must never be logged.

---

# 16. Error Handling

Never suppress exceptions silently.

Bad:

except:
pass

Good:

Catch specific exceptions.

Log context.

Recover where possible.

Fail safely.

Unexpected failures should surface with meaningful messages.

---

# 17. Comments

Write comments only when the intention cannot be understood from the code itself.

Do not narrate obvious operations.

Bad:

# Increment i

Good:

# Retry because the broker may temporarily reject duplicate requests.

---

# 18. Magic Numbers

Magic numbers are prohibited.

Instead:

STOP_LOSS_PERCENT = 0.02

rather than:

if loss > 0.02

---

# 19. Dependencies

Every dependency must be documented.

Avoid unnecessary packages.

Prefer mature, well-maintained libraries.

---

# 20. Code Quality Gates

Every pull request must satisfy:

✓ Ruff passes

✓ Black passes

✓ isort passes

✓ Unit tests pass

✓ Type checking passes

✓ Documentation updated

✓ No hardcoded secrets

✓ No security warnings

---

# 21. Definition of Done

A module is considered complete only if:

* All requirements are implemented.
* Unit tests pass.
* Integration tests pass (where applicable).
* Documentation is updated.
* Code review is completed.
* Logging is implemented.
* Error handling is implemented.
* Configuration is externalized.
* Performance impact has been considered.
* No known critical defects remain.

---

# 22. Engineering Rule

The repository should always be in a deployable state.

No unfinished or partially implemented feature should be merged into the main branch.

Use feature branches for ongoing work.

---

# End of Document
