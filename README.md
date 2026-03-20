# flight-tracker-roundtrip

Automated **round-trip** flight price monitor using SerpApi + GitHub Actions.

## What it does

- Queries Google Flights for a **true round-trip fare** (outbound + return in a single API call, `type=1`)
- Logs the combined price to a single CSV file (`history_roundtrip.csv`)
- Sends a push notification via [ntfy.sh](https://ntfy.sh) when Google rates the price as a deal
- Runs automatically every **6 hours** via GitHub Actions

## Route

| Field | Value |
|---|---|
| Origin | GYE (Guayaquil, Ecuador) |
| Destination | YYZ (Toronto, Canada) |
| Outbound date | 2026-05-11 |
| Return date | 2026-06-08 |
| Currency | CAD |

## How it differs from `flight-price-tracker`

| Feature | flight-price-tracker | flight-tracker-roundtrip |
|---|---|---|
| API call type | One-way (`type=2`) x2 jobs | Round-trip (`type=1`) x1 job |
| Jobs per run | 2 (GYE→YYZ + YYZ→GYE) | 1 (GYE↔YYZ) |
| History files | 2 CSVs | 1 CSV (`history_roundtrip.csv`) |
| Price tracked | Each leg separately | Combined round-trip fare |

## Setup

### 1. Add GitHub Secrets

Go to **Settings → Secrets and variables → Actions** and add:

| Secret | Description |
|---|---|
| `SERPAPI_KEY` | Your SerpApi API key |
| `NTFY_TOPIC` | Your ntfy.sh topic name |

### 2. Done!

The workflow runs automatically. You can also trigger it manually from the **Actions** tab.

## Files

```
flight-tracker-roundtrip/
├── .github/
│   └── workflows/
│       └── check_flights.yml   # GitHub Actions workflow (every 6 hours)
├── flight_tracker.py           # Main tracker script
├── history_roundtrip.csv       # Price history log (auto-generated)
└── README.md
```
