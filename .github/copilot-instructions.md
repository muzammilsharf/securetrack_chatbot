# SecureTrack Copilot Instructions

## Project
Rule-based logistics chatbot. Python + Streamlit + SQLite. No LLMs, no external APIs.

## Hard Rules (never violate)
- ALL database queries MUST use parameterized queries (e.g., `cursor.execute("... WHERE id = ?", (val,))`). NEVER use f-strings, `.format()`, or string concatenation for SQL queries.
-  All database and critical operations must be wrapped in `try/except Exception as e` blocks. Log the error using the `logging` module and return a generic, user-friendly fallback string. Never expose internal paths or variable names to the UI.
- Assume all input is malicious. Rely on the `sanitiser.py` layer to enforce the 500-character limit and uppercase normalization before processing.
- NEVER suggest external API calls or LLM integrations. This is intentionally offline.
- Input sanitisation (strip, uppercase, 500-char cap) must run BEFORE any regex matching.
- All regex patterns must be defined as named constants at the top of bot_logic.py, never inline.

## Architecture Layers (in order)
- The system uses a strict linear pipeline: UI -> Sanitiser -> Rate Limiter -> Alias Resolver -> Regex Router -> Specific Handler -> DB -> Formatter. Do not bypass layers.

## DB Rules
- city_aliases table handles abbreviation resolution (ISB=ISLAMABAD etc.)
- Routes in shipping_rates are NOT symmetric. Both directions stored separately.
- Every new SQLite connection MUST execute `PRAGMA journal_mode=WAL;` and `PRAGMA busy_timeout=3000;` to allow concurrent reads without blocking.

## Streamlit & State Management
- Chat history and rate-limiting counters must be stored strictly in `st.session_state`.
- Do not write chat logs to the SQLite database. History dies when the browser refreshes.

## Response Rules
- Never expose table names, column names, file paths or exception types in chat responses.
- All monetary values formatted as PKR X,XXX.XX

## Code Style
- Write clean, modular Python 3.9+ code.
- Include docstrings for all functions referencing the relevant "REQ" number from the SRS when applicable.
- Use standard library tools wherever possible.
- Type hints on all functions
- Docstring on every function
- No magic numbers; use named constants