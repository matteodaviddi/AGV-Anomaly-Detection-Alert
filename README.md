# 🤖 AGV Anomaly Detection & Alert
### Make · OpenAI GPT-4 · Google Sheets · Gmail

An AI-powered anomaly detection and alerting system for industrial AGV (Automated Guided Vehicles) fleets. The system automatically monitors operational data every hour, detects anomalies and critical situations, and delivers a professional HTML alert report via email — designed as a proof of concept for **E80 Group**, a world leader in intralogistics automation.

---

## 🏭 Industrial Context

E80 Group designs and installs AGV fleets, LGV systems and automated warehouses for clients such as Barilla, Nestlé and Coca-Cola. Their vehicles operate 24/7 — every unexpected breakdown can cost thousands of euros per minute. This system detects anomalies before they become critical failures, enabling proactive maintenance and reducing downtime.

---

## 🚀 How It Works

1. **Every hour** the scenario triggers automatically via Make scheduler
2. **Reads** operational data from all AGVs in Google Sheets
3. **Aggregates** all measurements into a single structured text
4. **Analyzes** the data with OpenAI GPT-4 against predefined thresholds
5. **Delivers** a professional HTML alert email with per-AGV anomaly breakdown

---

## 🛠️ Tech Stack

| Tool | Purpose |
|------|---------|
| [Make](https://make.com) | Workflow automation |
| Google Sheets | AGV operational data source |
| OpenAI API (GPT-4o) | AI-powered anomaly analysis |
| Gmail | HTML alert email delivery |

---

## 📐 Workflow Architecture

```
Schedule (Every Hour)
        ↓
Google Sheets — Search Rows
        ↓
Tools — Text Aggregator
        ↓
OpenAI — Generate Completion (GPT-4o)
        ↓
Gmail — Send HTML Alert
```

---

## 📊 Monitored Metrics & Thresholds

| Metric | Normal Range | Critical Level |
|--------|-------------|----------------|
| Speed | 1.4 – 2.0 m/s | < 0.5 m/s |
| Battery Temperature | 25 – 45°C | > 65°C |
| Errors/hour | 0 – 1 | > 5 |
| Completed Cycles | 10 – 20/hour | < 5 |
| Load | 500 – 1000 kg | > 1100 kg |

---

## 📋 Google Sheets Structure

### Tab: `Metriche` — Operational data

| Timestamp | AGV_ID | Velocita | Temp_Batteria | Carico | Errori | Cicli | Stato |
|-----------|--------|----------|---------------|--------|--------|-------|-------|
| 2024-04-01 09:00 | AGV-02 | 0.4 | 58 | 920 | 3 | 8 | ANOMALIA |
| 2024-04-01 10:00 | AGV-02 | 0.2 | 71 | 920 | 7 | 4 | CRITICO |

> ⚠️ Column names must be simple with no spaces, special characters or symbols. Make cannot correctly read columns named `Velocità (m/s)` — use `Velocita` instead.

---

## 📧 Alert Email Structure

The HTML alert email includes:

- **Header** — Red (CRITICAL) / Orange (ANOMALY) / Green (OK)
- **KPI Cards** — Total AGVs monitored, anomalies detected, critical count
- **Per-AGV Detail** — Problem, probable cause, immediate action
- **Priority Actions Box** — 3 prioritized interventions
- **Automatic footer** with timestamp

---

## 🤖 OpenAI Prompt

```
Analyze this AGV monitoring data and write an analysis in Italian.

NORMAL THRESHOLDS:
- Speed: 1.4-2.0 m/s | CRITICAL if < 0.5
- Battery temperature: 25-45°C | CRITICAL if > 65
- Errors: 0-1 | CRITICAL if > 5
- Cycles: 10-20/hour | CRITICAL if < 5

DATA:
{{text_aggregator}}

For each AGV with anomalies write EXACTLY in this format:

AGV-XX:
• Problem: [brief description with exact values]
• Cause: [probable cause]
• Action: [immediate action required]

If an AGV is normal write:
AGV-XX:
• Status: OK — all parameters within normal range
```

---

## ⚙️ Setup Guide

### Prerequisites
- [Make account](https://make.com)
- Google account with Google Sheets
- [OpenAI API key](https://platform.openai.com)
- Gmail account

### Steps

1. **Create the Google Sheet** with the structure above — tab named `Metriche`
2. **Import the blueprint** on Make: ⋮ → Import Blueprint → upload `blueprint.json`
3. **Connect your accounts**: Google Sheets, OpenAI, Gmail
4. **Update the Google Sheets module** with your file and sheet name
5. **Run once** to test → check your inbox
6. **Activate** the scenario toggle for automatic hourly execution

---

## 🔑 Key Design Decisions

**Why is the HTML template in Gmail and not in OpenAI?**
OpenAI slightly varies HTML output on each call even with the same prompt. By putting the fixed template directly in Gmail and using OpenAI only for the anomaly analysis text, the report layout is guaranteed to be identical every time.

**Why simple column names without special characters?**
Make cannot correctly read columns with names like `Velocità (m/s)` or `Temperatura (°C)`. Using simple names like `Velocita` and `Temp_Batteria` avoids parsing errors.

**Why Text Aggregator?**
Search Rows returns one bundle per row (12 AGVs = 12 bundles). The Text Aggregator collapses them into a single text that OpenAI receives in one message, preventing 12 separate emails.

---

## ⚠️ Common Issues

| Problem | Cause | Solution |
|---------|-------|----------|
| Empty values in email | Variables not linked after column rename | Re-drag variables from panel in Text Aggregator |
| "Data not available" message | `{{2.text}}` is empty | Reselect text variable from OpenAI panel |
| Multiple emails sent | Text Aggregator misconfigured | Verify Source Module = Search Rows |
| `Unable to parse range` | Sheet name typed manually | Always select from dropdown |
| Layout changes between runs | OpenAI generating HTML | HTML template must be in Gmail, not OpenAI |

---

## 🔧 Adapting for Real Clients

To connect the system to real AGV data:

1. Replace Google Sheets with the actual AGV operational database (via API or automated export)
2. Update thresholds in the OpenAI prompt with real parameters for each AGV model
3. Add Slack/Teams alerts in addition to email
4. Integrate with maintenance ticketing system for automatic work order creation

---

## 📄 License

MIT — feel free to use, modify and share.

---

## 👤 Author

**Matteo Daviddi**
Data Analyst & Process Automation
[LinkedIn](https://www.linkedin.com/in/matteodaviddi) · [GitHub](https://github.com/matteodaviddi)

