---
name: jerusalem-train-voucher
description: >-
  Generates a direct deep-link URL to the Israel Railways voucher booking
  page for Jerusalem line trains. Uses the railil CLI to fetch live
  timetable data, then builds a one-click URL to the trip-reservation
  page with the train number pre-filled. Use when user asks to "reserve
  a shovar", "hazmanat shovar rakevet", "shovar yerushalayim", "book
  jerusalem train voucher", "אני צריך שובר לרכבת", or mentions needing
  a voucher for a train to/from Jerusalem. Do NOT use for general Israel
  Railways ticket purchasing or Rav-Kav loading.
license: MIT
allowed-tools: 'Bash(railil:*) Bash(npx:*)'
compatibility: >-
  Requires Node.js and network access for railil CLI.
  Works with Claude Code, Cursor, GitHub Copilot, Windsurf, OpenCode, Codex.
metadata:
  author: rivka-ungar
  version: 1.0.0
  category: government-services
  tags:
    he:
      - שובר
      - רכבת
      - ירושלים
      - תחבורה
      - ישראל
    en:
      - voucher
      - train
      - jerusalem
      - transportation
      - israel
  display_name:
    he: "שובר נסיעה לרכבת ירושלים"
    en: Jerusalem Train Voucher
  display_description:
    he: "יוצר קישור ישיר לדף הזמנת שובר נסיעה לקו הרכבת לירושלים"
    en: >-
      Generates a direct deep-link URL to the Israel Railways voucher
      booking page for Jerusalem line trains.
  supported_agents:
    - claude-code
    - cursor
    - github-copilot
    - windsurf
    - opencode
    - codex
---

# Jerusalem Train Voucher

## Overview

Israel Railways limits the Jerusalem line to 1,200 passengers per train
due to safety constraints in the underground tunnel (80 meters deep).
Every passenger must hold a free שובר נסיעה (shovar nesia) in addition
to a valid Rav-Kav or ticket.

This skill uses the `railil` CLI to fetch live timetable data, then
builds a direct URL to the voucher booking page — skipping all the
manual navigation on rail.co.il.

## Prerequisites

Install railil (one time):

```bash
npm install -g railil
```

## Instructions

### Step 1: Parse the user's request

Extract from the user's message:

| Field | Example | Notes |
|-------|---------|-------|
| Origin (tachanat motza) | "מירושלים", "from Navon" | Pass to railil --from |
| Destination (tachanat yaad) | "לשלום", "to Tel Aviv" | Pass to railil --to |
| Date (taarich) | "מחר", "20/4/2026" | Convert to YYYY-MM-DD for railil --date |
| Time (shaa) | "ב-8:09", "at 9am" | Pass as HH:MM to railil --time |

### Step 2: Fetch trains using railil

Run railil with JSON output:

```bash
railil --from "Jerusalem" --to "Tel Aviv HaShalom" --time 08:09 --date 2026-04-20 --output json
```

railil supports fuzzy station name matching in both Hebrew and English.
The JSON output contains an array of trains, each with a `trainNumber` field.

### Step 3: Build voucher URLs

For each train in the JSON response, construct this URL:

```
https://www.rail.co.il/?page=trip-reservation&trainNumber={TRAIN_NUMBER}&fromStation={FROM_CODE}&toStation={TO_CODE}&date={YYYY-MM-DD}&time={HH:MM}&scheduleType=2&trainType=empty
```

Station codes for the Jerusalem line:

| Station | Code |
|---------|------|
| ירושלים יצחק נבון | 680 |
| תל אביב השלום | 4600 |
| תל אביב סבידור מרכז | 3700 |
| תל אביב מרכז | 3600 |
| תל אביב הגנה | 3100 |
| תל אביב אוניברסיטה | 2800 |
| הרצליה | 1260 |
| מודיעין מרכז | 400 |
| בן גוריון שדה תעופה | 1220 |
| לוד | 1500 |
| בית שמש | 700 |

### Step 4: Present results to the user

Show each train with its direct voucher link:

> הנה הרכבות עם קישור ישיר להזמנת שובר:
>
> 08:09 → 08:38  רכבת 6708
> [להזמנת שובר](URL)
>
> 08:39 → 09:08  רכבת 6710
> [להזמנת שובר](URL)

Always remind the user:
- On mobile (nayad): the voucher button hides under "פעולות" (peulot)
- The voucher is free (chinam) and separate from the travel ticket
- After clicking: enter phone → SMS code → confirm → voucher arrives by SMS

### Step 5: Fallback if railil is unavailable

If railil fails or is not installed, build a search results URL instead:

```
https://www.rail.co.il/?page=routePlanSearchResults&fromStation={FROM_CODE}&toStation={TO_CODE}&date={YYYY-MM-DD}&hours={HH}&minutes={MM}&scheduleType=2
```

This opens rail.co.il with the route pre-filled. The user clicks
"הזמנת שובר נסיעה" on the desired train manually.

## Examples

### Example 1: Hebrew request

User says: "אני צריכה שובר מירושלים לשלום מחר ב-8:09"

```bash
railil --from "Jerusalem" --to "Tel Aviv HaShalom" --time 08:09 --date 2026-04-20 --output json
```

Extract trainNumber from response, build:
`https://www.rail.co.il/?page=trip-reservation&trainNumber=6708&fromStation=680&toStation=4600&date=2026-04-20&time=08:09&scheduleType=2&trainType=empty`

### Example 2: Return trip

User says: "שובר מסבידור לירושלים ב-17:00"

```bash
railil --from "Tel Aviv Savidor" --to "Jerusalem" --time 17:00 --output json
```

Build URL with fromStation=3700, toStation=680.

### Example 3: Ben Gurion Airport

User says: "I need a voucher from the airport to Jerusalem at 10:30"

```bash
railil --from "Ben Gurion" --to "Jerusalem" --time 10:30 --output json
```

Build URL with fromStation=1220, toStation=680.

## Bundled Resources

### References
- `references/station-codes.md` -- Complete list of Jerusalem line stations with codes and aliases. Consult when mapping station names to codes for the voucher URL.
- `references/voucher-faq.md` -- FAQ about the voucher system: validity, group bookings, cancellation, mobile UI. Consult when user has questions beyond basic booking.

## Reference Links

| Source | URL | What to Check |
|--------|-----|---------------|
| Israel Railways voucher page | https://rail.co.il/?page=vouchers-jerusalem | Current voucher policy |
| Israel Railways route planner | https://rail.co.il | Timetable and stations |
| railil npm package | https://github.com/lirantal/railil | CLI updates and fixes |
| Customer service | tel:*5770 | Phone support |

## Gotchas

- railil must be installed (`npm install -g railil`) before the skill can work. If not installed, fall back to the search results URL.
- The voucher button on rail.co.il only appears when seats are available. If the train is full (1,200 passengers), the button disappears.
- On mobile and the Israel Railways app, the voucher button hides under "פעולות" (Actions). Always mention this.
- Train numbers change per day — never hardcode them. Always fetch via railil.
- The voucher restriction applies to ALL trains on the Jerusalem line (as of April 2026), not just holidays.
- The voucher is free and does not replace the travel ticket — both are required.

## Troubleshooting

### Error: "railil: command not found"
Cause: railil CLI not installed.
Solution: Run `npm install -g railil`. Requires Node.js.

### Error: "No trains found"
Cause: No trains at that time, or date is Shabbat/holiday.
Solution: Verify the date is a future weekday. Trains don't run on Shabbat.

### Error: "Network error" or "403"
Cause: Israel Railways API may be temporarily down or blocking requests.
Solution: Try again in a few minutes. If persistent, check for railil updates: `npm update -g railil`.
