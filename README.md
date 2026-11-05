# Prayer Clock — mosque timetables

Public prayer timetables of individual mosques, used by the Prayer Clock
("Settings › Prayer times › Your mosque › Mosques near you"). Clocks download
their country's list and the one mosque they follow, and check once a day for
corrections and next year's timetable.

Published with GitHub Pages: `https://elokidhx.github.io/prayer-clock-mosques/`

## Layout

```
<cc>/index.json              the mosques of one country (cc = ISO code, lower case: gb, de, fr…)
<cc>/<mosque-id>/<year>.csv  one mosque's timetable for one year
```

### index.json

```json
{
  "version": 1,
  "country": "GB",
  "updated": "2026-10-10",
  "mosques": [
    {
      "id": "gb-london-east-london-mosque",
      "name": "East London Mosque",
      "city": "London",
      "country": "GB",
      "lat": 51.5175,
      "lng": -0.0652,
      "years": { "2026": "2026-10-10" },
      "source": "https://…",
      "via": "musally"
    }
  ],
  "removed": [
    { "id": "gb-some-mosque", "date": "2026-11-06" }
  ]
}
```

- `source`: where the mosque publishes its timetable.
- `via` (optional): who passed the times on, when it wasn't the mosque
  itself. Clocks (app 0.84.0 and later) show "Times: <name> via <via>"; without
  it, "Times: <name>".
- `removed` (optional): mosques that asked to be taken off. **To remove a
  mosque, delete its entry from `mosques`, add its id here, and delete its
  folder.** Clocks following it stop at their next daily check (within a
  day), delete its timetable and go back to the usual times, with a notice.
  Deleting the entry alone is not enough: a mosque that is simply missing is
  kept on clocks, in case the list was changed by mistake.

`years` maps each available year to the date its file last changed. **Change
that date whenever a year's file is corrected**: clocks download a year again
only when its date changes.

### <year>.csv

The clock's timetable format: local times, 24-hour, start times (not
congregation times).

```
# city: gb-london-east-london-mosque
# source: East London Mosque, published 2026 timetable
date,fajr,sunrise,dhuhr,asr,maghrib,isha
2026-01-01,06:26,08:03,12:09,13:46,16:05,17:42
```

Times must be in order on each day. Use the local time in force that day
(summer time included).

A mosque's table may also give its Iqama and Friday Jumu'ah times (app
0.84.0 and later), as extra columns named in the header row: any of
`fajr_iqama`, `dhuhr_iqama`, `asr_iqama`, `maghrib_iqama`, `isha_iqama`,
`jumuah`, in any order. Leave a cell empty when it isn't given that day.

```
date,fajr,sunrise,dhuhr,asr,maghrib,isha,fajr_iqama,dhuhr_iqama,asr_iqama,maghrib_iqama,isha_iqama,jumuah
2026-10-02,05:32,07:04,12:52,15:48,18:29,19:50,06:00,13:15,16:15,18:34,20:15,13:30
```

Each Iqama must be after its Adhan, before the next time, and at most 2 hours
later; Jumu'ah between Dhuhr and Asr. Clocks ignore anything else (and use the
family's Iqama minutes that day), so check them when converting. Older clocks
ignore the extra columns.

## Adding a mosque

1. Convert its published timetable to `<cc>/<mosque-id>/<year>.csv`.
2. Check it: every sunrise and Maghrib near the real sun, times in order,
   no day-to-day jumps except the summer/winter clock change.
3. Add (or update) its entry in `<cc>/index.json`.

## Sources and corrections

The times belong to the mosques that publish them; each entry links its
source. To correct or remove a mosque, open an issue in this repository.
Removals are done within a day (see `removed` above).
