# Jerusalem Line Station Codes

Consult when: mapping station names to numeric codes for the voucher URL.

| Code | Hebrew | English | Aliases |
|------|--------|---------|---------|
| 680 | ירושלים יצחק נבון | Jerusalem Yitzhak Navon | ירושלים, נבון, navon |
| 4600 | תל אביב השלום | Tel Aviv HaShalom | השלום, שלום, אזריאלי |
| 3700 | תל אביב סבידור מרכז | Tel Aviv Savidor Center | סבידור, ארלוזורוב |
| 3600 | תל אביב מרכז | Tel Aviv Center | |
| 3100 | תל אביב הגנה | Tel Aviv HaHagana | הגנה |
| 2800 | תל אביב אוניברסיטה | Tel Aviv University | אוניברסיטה |
| 1260 | הרצליה | Herzliya | |
| 400 | מודיעין מרכז | Modi'in Center | מודיעין |
| 300 | פאתי מודיעין | Pa'ate Modi'in | פאתי |
| 1220 | בן גוריון שדה תעופה | Ben Gurion Airport | נתבג, airport |
| 1500 | לוד | Lod | |
| 700 | בית שמש | Beit Shemesh | |

## Voucher URL pattern

```
https://www.rail.co.il/?page=trip-reservation&trainNumber={TRAIN}&fromStation={FROM}&toStation={TO}&date={YYYY-MM-DD}&time={HH:MM}&scheduleType=2&trainType=empty
```

A voucher is required for any train stopping at station 680 (Jerusalem Navon).

## Sources

Station codes from the NextTrain SQLite database and Hasadna OpenTrain API docs.
