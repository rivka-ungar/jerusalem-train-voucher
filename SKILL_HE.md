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

# שובר נסיעה לרכבת ירושלים

## סקירה

רכבת ישראל מגבילה את מספר הנוסעים בקו ירושלים ל-1,200 לכל רכבת בשל
אילוצי בטיחות במנהרה התת-קרקעית (עומק 80 מטר). כל נוסע חייב בשובר
נסיעה חינמי בנוסף לכרטיס נסיעה בתוקף.

סקיל זה משתמש בכלי railil כדי לשלוף נתוני לוח זמנים בזמן אמת, ואז
בונה קישור ישיר לדף הזמנת השובר — בלי לנווט ידנית באתר רכבת ישראל.

## דרישות מוקדמות

התקנת railil (פעם אחת):

npm install -g railil

## הוראות

### שלב 1: פירוש בקשת המשתמש

חילוץ מהודעת המשתמש:
- תחנת מוצא — לדוגמה: "מירושלים", "from Navon"
- תחנת יעד — לדוגמה: "לשלום", "to Tel Aviv"
- תאריך — לדוגמה: "מחר", "20/4/2026"
- שעה — לדוגמה: "ב-8:09", "at 9am"

### שלב 2: שליפת רכבות באמצעות railil

הרצה עם פלט JSON:

railil --from "Jerusalem" --to "Tel Aviv HaShalom" --time 08:09 --date 2026-04-20 --output json

railil תומך בהתאמה מטושטשת של שמות תחנות בעברית ובאנגלית.
פלט ה-JSON מכיל מערך רכבות, כל אחת עם שדה trainNumber.

### שלב 3: בניית קישורי שובר

לכל רכבת בתוצאות, בניית הקישור הבא:

https://www.rail.co.il/?page=trip-reservation&trainNumber={TRAIN_NUMBER}&fromStation={FROM_CODE}&toStation={TO_CODE}&date={YYYY-MM-DD}&time={HH:MM}&scheduleType=2&trainType=empty

קודי תחנות קו ירושלים:
- ירושלים יצחק נבון — 680
- תל אביב השלום — 4600
- תל אביב סבידור מרכז — 3700
- תל אביב מרכז — 3600
- תל אביב הגנה — 3100
- תל אביב אוניברסיטה — 2800
- הרצליה — 1260
- מודיעין מרכז — 400
- בן גוריון שדה תעופה — 1220
- לוד — 1500
- בית שמש — 700

### שלב 4: הצגת תוצאות למשתמש

הצגת כל רכבת עם קישור ישיר:

- בנייד: כפתור השובר מסתתר תחת "פעולות"
- השובר חינמי ונפרד מכרטיס הנסיעה — שניהם נדרשים
- אחרי הלחיצה: מספר נייד ← קוד SMS ← אישור ← שובר ב-SMS

### שלב 5: חלופה כש-railil לא זמין

אם railil לא מותקן או נכשל, בניית קישור לדף תוצאות חיפוש:

https://www.rail.co.il/?page=routePlanSearchResults&fromStation={FROM_CODE}&toStation={TO_CODE}&date={YYYY-MM-DD}&hours={HH}&minutes={MM}&scheduleType=2

## דוגמאות

### דוגמה 1: בקשה בעברית

המשתמש: "אני צריכה שובר מירושלים לשלום מחר ב-8:09"

railil --from "Jerusalem" --to "Tel Aviv HaShalom" --time 08:09 --date 2026-04-20 --output json

חילוץ trainNumber מהתוצאה ובניית קישור עם fromStation=680, toStation=4600.

### דוגמה 2: חזרה מסבידור

המשתמש: "שובר מסבידור לירושלים ב-17:00"

railil --from "Tel Aviv Savidor" --to "Jerusalem" --time 17:00 --output json

בניית קישור עם fromStation=3700, toStation=680.

### דוגמה 3: מנתב"ג

המשתמש: "I need a voucher from the airport to Jerusalem at 10:30"

railil --from "Ben Gurion" --to "Jerusalem" --time 10:30 --output json

בניית קישור עם fromStation=1220, toStation=680.

## משאבים מצורפים

### קובצי עזר
- references/station-codes.md — רשימת תחנות קו ירושלים עם קודים. להתייעצות כשצריך למפות שם תחנה לקוד.
- references/voucher-faq.md — שאלות נפוצות על מערכת השוברים. להתייעצות כשלמשתמש שאלות מעבר להזמנה בסיסית.

## קישורי עזר

- דף שוברי ירושלים: https://rail.co.il/?page=vouchers-jerusalem
- תכנון מסלול: https://rail.co.il
- חבילת railil: https://github.com/lirantal/railil
- מוקד שירות: 5770*

## נקודות לתשומת לב

- railil חייב להיות מותקן (npm install -g railil) לפני שהסקיל יעבוד.
- כפתור השובר מופיע רק כשיש מקומות פנויים. רכבת מלאה — הכפתור נעלם.
- בנייד ובאפליקציה: כפתור השובר מסתתר תחת "פעולות".
- מספרי רכבות משתנים לפי יום — חובה לשלוף אותם דרך railil.
- ההגבלה חלה על כל הרכבות בקו ירושלים (נכון לאפריל 2026), לא רק בחגים.
- השובר חינמי ולא מחליף כרטיס נסיעה — שניהם נדרשים.

## פתרון בעיות

### שגיאה: "railil: command not found"
סיבה: railil לא מותקן.
פתרון: הריצו npm install -g railil. דורש Node.js.

### שגיאה: "No trains found"
סיבה: אין רכבות בשעה זו, או שבת/חג.
פתרון: ודאו שהתאריך הוא יום חול עתידי.

### שגיאה: "Network error" או "403"
סיבה: ה-API של רכבת ישראל מושבת זמנית.
פתרון: נסו שוב בעוד מספר דקות. אם נמשך, עדכנו: npm update -g railil.
