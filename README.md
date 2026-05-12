# SecureTrack Logistics Chatbot

A production-grade, privacy-first rule-based chatbot designed for the logistics and courier industry. SecureTrack is built with a strictly controlled pipeline to handle shipment tracking and shipping rate calculations securely, without relying on external APIs or exposing internal database structures.

## Features

*   **Rule-Based Intent Routing:** Deterministic Regex routing for high reliability.
*   **Privacy-First Architecture:** Zero external network calls; no user data leaves the host machine.
*   **Secure Database Operations:** Parameterized SQLite queries to prevent SQL injection.
*   **High-Concurrency Setup:** SQLite configured with Write-Ahead Logging (WAL) to support multiple concurrent read sessions.
*   **Session-Scoped Chat:** Stateless chat history that clears entirely upon page refresh.

## Project Structure

\`\`\`text
securetrack_chatbot/
├── data/               # SQLite database storage (git-ignored)
├── src/                # Main application code
│   ├── core/           # Pipeline components (Sanitiser, Limiter, Router)
│   ├── database/       # DB initialization and parameterized queries
│   ├── handlers/       # Intent-specific logic (Tracking, Rates, Fallback)
│   ├── utils/          # Regex constants and text formatters
│   └── app.py          # Streamlit UI entry point
├── tests/              # Pytest suite
└── README.md
\`\`\`

## 🛠️ Setup & Installation

**1. Clone the repository:**
\`\`\`bash
git clone https://github.com/yourusername/securetrack-chatbot.git
cd securetrack-chatbot
\`\`\`

**2. Install dependencies:**
\`\`\`bash
pip install streamlit
\`\`\`

**3. Initialize the database:**
This will create the `securetrack.db` file with WAL mode enabled and seed it with initial tracking and rate data.
\`\`\`bash
python src/database/init_db.py
\`\`\`

**4. Run the application:**
\`\`\`bash
streamlit run src/app.py
\`\`\`

---
*Classification: Portfolio Project | Version: 1.0*