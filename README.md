# Jerusalem Train Voucher

A Claude skill that generates a direct deep-link URL to the Israel Railways voucher booking page for Jerusalem line trains, using the [railil](https://github.com/lirantal/railil) CLI to fetch live timetable data.

Every passenger on the Jerusalem line needs a free שובר נסיעה (voucher) in addition to a valid Rav-Kav or ticket — trains are capped at 1,200 passengers due to safety constraints in the underground tunnel. This skill turns the multi-step booking flow on rail.co.il into a single click.

## Install

Copy this directory into the skills folder of a compatible agent (Claude Code, Cursor, GitHub Copilot, Windsurf, OpenCode, Codex). For Claude Code:

```bash
cp -r jerusalem-train-voucher ~/.claude/skills/
```

Install the railil CLI once:

```bash
npm install -g railil
```

## Usage

Trigger the skill by asking for a voucher in Hebrew or English:

- "אני צריכה שובר מירושלים לשלום מחר ב-8:09"
- "book a voucher from Navon to Tel Aviv at 9am"
- "shovar yerushalayim to Ben Gurion 10:30"

The skill returns a list of trains, each with a direct link that pre-fills the train number, stations, date, and time on rail.co.il.

## Structure

```
jerusalem-train-voucher/
├── README.md               this file
├── SKILL.md                skill instructions (English)
├── SKILL_HE.md             skill instructions (Hebrew)
└── references/
    ├── station-codes.md    Jerusalem line stations → numeric codes
    └── voucher-faq.md      voucher policy, validity, cancellation
```

## License

MIT
