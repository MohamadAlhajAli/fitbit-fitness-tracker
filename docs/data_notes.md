# Fitbit data notes

Updated: 2026-10-18

This file records findings checked during our EDA.
Findings from the old Docs folder are not included.

## 1. Input inventory

| Export | CSV files | Size in bytes | Size in MB |
|---|---:|---:|---:|
| P1 | 11 | 278,798,829 | 278.8 |
| P2 | 18 | 337,997,716 | 338.0 |
| Total | 29 | 616,796,545 | 616.8 |

Sizes use decimal MB: 1 MB = 1,000,000 bytes.

P1 and P2 are our labels for these folders inside Dataset/:

- P1: mturkfitbit_export_3.12.16-4.11.16/Fitabase Data 3.12.16-4.11.16
- P2: mturkfitbit_export_4.12.16-5.12.16/Fitabase Data 4.12.16-5.12.16

Folder date labels do not establish the actual dates in the records.

The input manifest records each file's export, relative path,
size in bytes, and SHA-256 checksum.

Evidence: [input manifest](../EDA/input_manifest.csv).

## 2. Header checks

All 29 CSV headers were read successfully.

The checks found:
- No repeated column names.
- No empty column names.
- No spaces around column names.

These checks cover column names only, not the data rows.

## 3. Files and column counts

| Filename | In P1 | In P2 | Columns |
|---|---|---|---:|
| dailyActivity_merged.csv | Yes | Yes | 15 |
| dailyCalories_merged.csv | No | Yes | 3 |
| dailyIntensities_merged.csv | No | Yes | 10 |
| dailySteps_merged.csv | No | Yes | 3 |
| heartrate_seconds_merged.csv | Yes | Yes | 3 |
| hourlyCalories_merged.csv | Yes | Yes | 3 |
| hourlyIntensities_merged.csv | Yes | Yes | 4 |
| hourlySteps_merged.csv | Yes | Yes | 3 |
| minuteCaloriesNarrow_merged.csv | Yes | Yes | 3 |
| minuteCaloriesWide_merged.csv | No | Yes | 62 |
| minuteIntensitiesNarrow_merged.csv | Yes | Yes | 3 |
| minuteIntensitiesWide_merged.csv | No | Yes | 62 |
| minuteMETsNarrow_merged.csv | Yes | Yes | 3 |
| minuteSleep_merged.csv | Yes | Yes | 4 |
| minuteStepsNarrow_merged.csv | Yes | Yes | 3 |
| minuteStepsWide_merged.csv | No | Yes | 62 |
| sleepDay_merged.csv | No | Yes | 5 |
| weightLogInfo_merged.csv | Yes | Yes | 8 |

For filenames present in both exports, the column count is the
same in each export.

## 4. Header comparison across exports

- 11 filenames appear in both P1 and P2.
- All 11 have identical column names in the same order.
- Seven filenames appear only in P2.
- No filenames appear only in P1.

The seven P2-only files are absent from the P1 input folder.
The reason for that absence has not been checked.

This is a difference between the exports, not missing values
inside a table. We will not create records to fill that absence.

Matching headers alone do not establish that the data can be combined.

Evidence for sections 2–4:
[Simple EDA notebook](<../EDA/Simple EDA.ipynb>), Step 2 header checks
and cross-export header comparison.

## 5. Row counts and header groups

All 29 files were read record by record.
Headers are excluded from these counts.

| Filename | P1 rows | P2 rows |
|---|---:|---:|
| dailyActivity_merged.csv | 457 | 940 |
| dailyCalories_merged.csv | — | 940 |
| dailyIntensities_merged.csv | — | 940 |
| dailySteps_merged.csv | — | 940 |
| heartrate_seconds_merged.csv | 1,154,681 | 2,483,658 |
| hourlyCalories_merged.csv | 24,084 | 22,099 |
| hourlyIntensities_merged.csv | 24,084 | 22,099 |
| hourlySteps_merged.csv | 24,084 | 22,099 |
| minuteCaloriesNarrow_merged.csv | 1,445,040 | 1,325,580 |
| minuteCaloriesWide_merged.csv | — | 21,645 |
| minuteIntensitiesNarrow_merged.csv | 1,445,040 | 1,325,580 |
| minuteIntensitiesWide_merged.csv | — | 21,645 |
| minuteMETsNarrow_merged.csv | 1,445,040 | 1,325,580 |
| minuteSleep_merged.csv | 198,559 | 188,521 |
| minuteStepsNarrow_merged.csv | 1,445,040 | 1,325,580 |
| minuteStepsWide_merged.csv | — | 21,645 |
| sleepDay_merged.csv | — | 413 |
| weightLogInfo_merged.csv | 33 | 67 |

`—` means the file is absent, not that it contains zero rows.

### Record checks

- No blank records were found.
- No records had the wrong number of columns.
- Equal row counts do not prove that records match.

### Header groups

Grouping by column names produced 18 groups:

- 11 groups appear in both exports.
- Seven groups appear only in P2.
- No different filenames share the same column set.

These groups describe headers only, not matching values or meanings.

Evidence: [Simple EDA notebook](<../EDA/Simple EDA.ipynb>),
Step 2 row counts and header grouping.

## 6. Date parsing and observed ranges

All 29 files were checked in full using 100,000-row chunks.
Each file matched its earlier row count. Every date value parsed:

- No empty dates.
- No parsing failures; the failure rate was 0% in every file.
- Original date text was left unchanged.
- No timezone was assigned. The source timezone remains unknown.

Formats that worked for all recorded values in the mapped columns:

- `ActivityDate`, `ActivityDay`: `%m/%d/%Y`.
- `Time`, `ActivityHour`, `ActivityMinute`, `date`, `Date`, `SleepDay`: `%m/%d/%Y %I:%M:%S %p`.

### Full-file date ranges

| Export | Filename | Date column | Earliest | Latest |
|---|---|---|---|---|
| P1 | dailyActivity_merged.csv | ActivityDate | 2016-03-12 00:00:00 | 2016-04-12 00:00:00 |
| P1 | heartrate_seconds_merged.csv | Time | 2016-03-29 00:00:05 | 2016-04-12 11:03:15 |
| P1 | hourlyCalories_merged.csv | ActivityHour | 2016-03-12 00:00:00 | 2016-04-12 10:00:00 |
| P1 | hourlyIntensities_merged.csv | ActivityHour | 2016-03-12 00:00:00 | 2016-04-12 10:00:00 |
| P1 | hourlySteps_merged.csv | ActivityHour | 2016-03-12 00:00:00 | 2016-04-12 10:00:00 |
| P1 | minuteCaloriesNarrow_merged.csv | ActivityMinute | 2016-03-12 00:00:00 | 2016-04-12 10:59:00 |
| P1 | minuteIntensitiesNarrow_merged.csv | ActivityMinute | 2016-03-12 00:00:00 | 2016-04-12 10:59:00 |
| P1 | minuteMETsNarrow_merged.csv | ActivityMinute | 2016-03-12 00:00:00 | 2016-04-12 10:59:00 |
| P1 | minuteSleep_merged.csv | date | 2016-03-11 21:19:30 | 2016-04-12 08:35:00 |
| P1 | minuteStepsNarrow_merged.csv | ActivityMinute | 2016-03-12 00:00:00 | 2016-04-12 10:59:00 |
| P1 | weightLogInfo_merged.csv | Date | 2016-03-30 23:59:59 | 2016-04-12 23:59:59 |
| P2 | dailyActivity_merged.csv | ActivityDate | 2016-04-12 00:00:00 | 2016-05-12 00:00:00 |
| P2 | dailyCalories_merged.csv | ActivityDay | 2016-04-12 00:00:00 | 2016-05-12 00:00:00 |
| P2 | dailyIntensities_merged.csv | ActivityDay | 2016-04-12 00:00:00 | 2016-05-12 00:00:00 |
| P2 | dailySteps_merged.csv | ActivityDay | 2016-04-12 00:00:00 | 2016-05-12 00:00:00 |
| P2 | heartrate_seconds_merged.csv | Time | 2016-04-12 00:00:00 | 2016-05-12 16:20:00 |
| P2 | hourlyCalories_merged.csv | ActivityHour | 2016-04-12 00:00:00 | 2016-05-12 15:00:00 |
| P2 | hourlyIntensities_merged.csv | ActivityHour | 2016-04-12 00:00:00 | 2016-05-12 15:00:00 |
| P2 | hourlySteps_merged.csv | ActivityHour | 2016-04-12 00:00:00 | 2016-05-12 15:00:00 |
| P2 | minuteCaloriesNarrow_merged.csv | ActivityMinute | 2016-04-12 00:00:00 | 2016-05-12 15:59:00 |
| P2 | minuteCaloriesWide_merged.csv | ActivityHour | 2016-04-13 00:00:00 | 2016-05-13 08:00:00 |
| P2 | minuteIntensitiesNarrow_merged.csv | ActivityMinute | 2016-04-12 00:00:00 | 2016-05-12 15:59:00 |
| P2 | minuteIntensitiesWide_merged.csv | ActivityHour | 2016-04-13 00:00:00 | 2016-05-13 08:00:00 |
| P2 | minuteMETsNarrow_merged.csv | ActivityMinute | 2016-04-12 00:00:00 | 2016-05-12 15:59:00 |
| P2 | minuteSleep_merged.csv | date | 2016-04-11 20:48:00 | 2016-05-12 09:56:00 |
| P2 | minuteStepsNarrow_merged.csv | ActivityMinute | 2016-04-12 00:00:00 | 2016-05-12 15:59:00 |
| P2 | minuteStepsWide_merged.csv | ActivityHour | 2016-04-13 00:00:00 | 2016-05-13 08:00:00 |
| P2 | sleepDay_merged.csv | SleepDay | 2016-04-12 00:00:00 | 2016-05-12 00:00:00 |
| P2 | weightLogInfo_merged.csv | Date | 2016-04-12 06:47:11 | 2016-05-12 23:59:59 |

### Findings and limits

- Daily activity includes April 12 in both exports. Duplicate participant records have not been established.
- P1 minute sleep starts on March 11; P2 minute sleep starts on April 11, before their folder start labels. The reason has not been checked.
- P2 wide files span April 13-May 13; P2 narrow minute files span April 12-May 12. The reason for this difference is not yet known.
- Coverage differs by file. P1 heart-rate data starts on March 29, while daily activity starts on March 12.
- Keep the observed timestamps unchanged. Do not trim or shift them to match folder labels.
- Successful parsing does not prove correct timestamp alignment. Minimum and maximum dates do not prove continuous coverage.

**Follow-up in 5.7:** check cross-export overlap for all corresponding file types, not just daily activity. Compare verified keys and values before deciding whether records are identical or conflicting.

### Checks still to do

- **Possible partial days:** P1 minute activity ends April 12 at 10:59, and heart rate at 11:03:15. P2 minute activity ends May 12 at 15:59, and heart rate at 16:20. Daily activity includes both dates. Check coverage for each person before deciding whether any daily totals cover only part of a day. File-level end times alone do not prove this. These end times do not apply to every table: P2 wide files extend into May 13.
- **Sleep overlap (5.7):** P1 minute sleep ends April 12 at 08:35, while P2 starts April 11 at 20:48. Their time ranges overlap. Compare participant IDs, timestamps, log IDs and values using checked keys to see whether records or nights repeat. No repeated records have been established yet.

Evidence: [Simple EDA notebook](<../EDA/Simple EDA.ipynb>), Step 2.4 full-file date report; this also provides the ranges for 2.5.

## 7. Not checked yet

The checks above do not establish:
- Participant counts.
- What one row represents or which keys are valid.
- Recording gaps and per-participant date coverage.
- Field meanings, units, and non-date value types.
- Missing values in non-date fields, duplicate records, or conflicting values.
- Whether the two exports can be combined.

Add these findings only after the relevant EDA checks.