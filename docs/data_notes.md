# Fitbit data notes

Updated: 2026-10-19

This file records findings checked during our EDA.
It also keeps clearly marked meaning guesses for later checking.
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

**Follow-up in 6.6:** check cross-export overlap for all corresponding file types, not just daily activity. Compare verified keys and values before deciding whether records are identical or conflicting.

### Checks still to do

- **Possible partial days:** P1 minute activity ends April 12 at 10:59, and heart rate at 11:03:15. P2 minute activity ends May 12 at 15:59, and heart rate at 16:20. Daily activity includes both dates. Check coverage for each person before deciding whether any daily totals cover only part of a day. File-level end times alone do not prove this. These end times do not apply to every table: P2 wide files extend into May 13.
- **Sleep overlap (6.6):** P1 minute sleep ends April 12 at 08:35, while P2 starts April 11 at 20:48. Their time ranges overlap. Compare participant IDs, timestamps, log IDs and values using checked keys to see whether records or nights repeat. No repeated records have been established yet.

Evidence: [Simple EDA notebook](<../EDA/Simple EDA.ipynb>), Step 2.4 full-file date report; this also provides the ranges for 2.5.

## 7. Files, columns and guessed meanings

**Status: all meanings below are unverified guesses.** They come from column names and the Step 2.3 previews. Values and formats were observed; meanings, units, codes, scaling and day-assignment rules remain unconfirmed.

The 29 CSV files contain 220 distinct column names. These tables cover every file, with wide column groups shortened below. Files with the same filename share one heading, with a separate table for each export. This does not mean their records can be combined.

For each wide prefix (Calories, Intensity and Steps), only columns 00, 01 and 02 are shown. The other numbered columns follow the same pattern. Whether each slot represents a minute still needs checking.

The examples below show the first two records from each CSV, with fields displayed vertically for easier reading. Both exports are shown when a file exists in P1 and P2. Values and original IDs stay unchanged; `""` means an empty CSV field. These are examples, not representative samples.

**Observed formats and empty cells - saved Step 3.1 results (2026-10-19):**
- The dictionary has 311 file/export/column entries and 220 distinct column names.
- Shared filename/column pairs have matching inferred types across P1 and P2.
- The review shows no mixed/text or all-empty columns.
- Only `Fat` has empty strings: P1 has 2 non-empty and 31 empty values; P2 has 2 non-empty and 65 empty values.
- These are text-format and empty-string checks, not provider-confirmed meanings or a full missing-token audit. The updated Step 3.1 cells were run and saved by the author; code and saved results were reviewed on 2026-10-19 with no saved errors.

Evidence: saved dictionary and comparison outputs in Step 3.1 of [Simple EDA](<../EDA/Simple EDA.ipynb>).


### dailyActivity_merged.csv

**Exports:** P1, P2. **Columns:** 15.

**P1**

| Column | Guessed meaning | Record 1 | Record 2 |
|---|---|---|---|
| `Id` | Participant identifier | `1503960366` | `1503960366` |
| `ActivityDate` | Date of the daily activity record | `3/25/2016` | `3/26/2016` |
| `TotalSteps` | Daily step total | `11004` | `17609` |
| `TotalDistance` | Total distance for the record's period | `7.1100001335144` | `11.5500001907349` |
| `TrackerDistance` | Distance recorded by the tracker | `7.1100001335144` | `11.5500001907349` |
| `LoggedActivitiesDistance` | Distance linked to logged activities | `0` | `0` |
| `VeryActiveDistance` | Distance labelled very active | `2.5699999332428` | `6.92000007629395` |
| `ModeratelyActiveDistance` | Distance labelled moderately active | `0.46000000834465` | `0.730000019073486` |
| `LightActiveDistance` | Distance labelled lightly active | `4.07000017166138` | `3.91000008583069` |
| `SedentaryActiveDistance` | Distance linked to the sedentary category | `0` | `0` |
| `VeryActiveMinutes` | Minutes labelled very active | `33` | `89` |
| `FairlyActiveMinutes` | Minutes labelled fairly active | `12` | `17` |
| `LightlyActiveMinutes` | Minutes labelled lightly active | `205` | `274` |
| `SedentaryMinutes` | Minutes labelled sedentary | `804` | `588` |
| `Calories` | Calorie value for the labelled day; exact definition and unit unverified | `1819` | `2154` |

**P2**

| Column | Guessed meaning | Record 1 | Record 2 |
|---|---|---|---|
| `Id` | Participant identifier | `1503960366` | `1503960366` |
| `ActivityDate` | Date of the daily activity record | `4/12/2016` | `4/13/2016` |
| `TotalSteps` | Daily step total | `13162` | `10735` |
| `TotalDistance` | Total distance for the record's period | `8.5` | `6.96999979019165` |
| `TrackerDistance` | Distance recorded by the tracker | `8.5` | `6.96999979019165` |
| `LoggedActivitiesDistance` | Distance linked to logged activities | `0` | `0` |
| `VeryActiveDistance` | Distance labelled very active | `1.87999999523163` | `1.57000005245209` |
| `ModeratelyActiveDistance` | Distance labelled moderately active | `0.550000011920929` | `0.689999997615814` |
| `LightActiveDistance` | Distance labelled lightly active | `6.05999994277954` | `4.71000003814697` |
| `SedentaryActiveDistance` | Distance linked to the sedentary category | `0` | `0` |
| `VeryActiveMinutes` | Minutes labelled very active | `25` | `21` |
| `FairlyActiveMinutes` | Minutes labelled fairly active | `13` | `19` |
| `LightlyActiveMinutes` | Minutes labelled lightly active | `328` | `217` |
| `SedentaryMinutes` | Minutes labelled sedentary | `728` | `776` |
| `Calories` | Calorie value for the labelled day; exact definition and unit unverified | `1985` | `1797` |

### dailyCalories_merged.csv

**Exports:** P2. **Columns:** 3.

**P2**

| Column | Guessed meaning | Record 1 | Record 2 |
|---|---|---|---|
| `Id` | Participant identifier | `1503960366` | `1503960366` |
| `ActivityDay` | Date of the daily record | `4/12/2016` | `4/13/2016` |
| `Calories` | Calorie value for the labelled day; exact definition and unit unverified | `1985` | `1797` |

### dailyIntensities_merged.csv

**Exports:** P2. **Columns:** 10.

**P2**

| Column | Guessed meaning | Record 1 | Record 2 |
|---|---|---|---|
| `Id` | Participant identifier | `1503960366` | `1503960366` |
| `ActivityDay` | Date of the daily record | `4/12/2016` | `4/13/2016` |
| `SedentaryMinutes` | Minutes labelled sedentary | `728` | `776` |
| `LightlyActiveMinutes` | Minutes labelled lightly active | `328` | `217` |
| `FairlyActiveMinutes` | Minutes labelled fairly active | `13` | `19` |
| `VeryActiveMinutes` | Minutes labelled very active | `25` | `21` |
| `SedentaryActiveDistance` | Distance linked to the sedentary category | `0` | `0` |
| `LightActiveDistance` | Distance labelled lightly active | `6.05999994277954` | `4.71000003814697` |
| `ModeratelyActiveDistance` | Distance labelled moderately active | `0.550000011920929` | `0.689999997615814` |
| `VeryActiveDistance` | Distance labelled very active | `1.87999999523163` | `1.57000005245209` |

### dailySteps_merged.csv

**Exports:** P2. **Columns:** 3.

**P2**

| Column | Guessed meaning | Record 1 | Record 2 |
|---|---|---|---|
| `Id` | Participant identifier | `1503960366` | `1503960366` |
| `ActivityDay` | Date of the daily record | `4/12/2016` | `4/13/2016` |
| `StepTotal` | Step total for the record's period | `13162` | `10735` |

### heartrate_seconds_merged.csv

**Exports:** P1, P2. **Columns:** 3.

**P1**

| Column | Guessed meaning | Record 1 | Record 2 |
|---|---|---|---|
| `Id` | Participant identifier | `2022484408` | `2022484408` |
| `Time` | Time linked to the heart-rate reading | `4/1/2016 7:54:00 AM` | `4/1/2016 7:54:05 AM` |
| `Value` | Heart-rate reading; unit unconfirmed | `93` | `91` |

**P2**

| Column | Guessed meaning | Record 1 | Record 2 |
|---|---|---|---|
| `Id` | Participant identifier | `2022484408` | `2022484408` |
| `Time` | Time linked to the heart-rate reading | `4/12/2016 7:21:00 AM` | `4/12/2016 7:21:05 AM` |
| `Value` | Heart-rate reading; unit unconfirmed | `97` | `102` |

### hourlyCalories_merged.csv

**Exports:** P1, P2. **Columns:** 3.

**P1**

| Column | Guessed meaning | Record 1 | Record 2 |
|---|---|---|---|
| `Id` | Participant identifier | `1503960366` | `1503960366` |
| `ActivityHour` | Hour linked to the record | `3/12/2016 12:00:00 AM` | `3/12/2016 1:00:00 AM` |
| `Calories` | Calorie value for the labelled hour; exact definition and unit unverified | `48` | `48` |

**P2**

| Column | Guessed meaning | Record 1 | Record 2 |
|---|---|---|---|
| `Id` | Participant identifier | `1503960366` | `1503960366` |
| `ActivityHour` | Hour linked to the record | `4/12/2016 12:00:00 AM` | `4/12/2016 1:00:00 AM` |
| `Calories` | Calorie value for the labelled hour; exact definition and unit unverified | `81` | `61` |

### hourlyIntensities_merged.csv

**Exports:** P1, P2. **Columns:** 4.

**P1**

| Column | Guessed meaning | Record 1 | Record 2 |
|---|---|---|---|
| `Id` | Participant identifier | `1503960366` | `1503960366` |
| `ActivityHour` | Hour linked to the record | `3/12/2016 12:00:00 AM` | `3/12/2016 1:00:00 AM` |
| `TotalIntensity` | Total activity intensity | `0` | `0` |
| `AverageIntensity` | Average activity intensity | `0` | `0` |

**P2**

| Column | Guessed meaning | Record 1 | Record 2 |
|---|---|---|---|
| `Id` | Participant identifier | `1503960366` | `1503960366` |
| `ActivityHour` | Hour linked to the record | `4/12/2016 12:00:00 AM` | `4/12/2016 1:00:00 AM` |
| `TotalIntensity` | Total activity intensity | `20` | `8` |
| `AverageIntensity` | Average activity intensity | `0.333333` | `0.133333` |

### hourlySteps_merged.csv

**Exports:** P1, P2. **Columns:** 3.

**P1**

| Column | Guessed meaning | Record 1 | Record 2 |
|---|---|---|---|
| `Id` | Participant identifier | `1503960366` | `1503960366` |
| `ActivityHour` | Hour linked to the record | `3/12/2016 12:00:00 AM` | `3/12/2016 1:00:00 AM` |
| `StepTotal` | Step total for the record's period | `0` | `0` |

**P2**

| Column | Guessed meaning | Record 1 | Record 2 |
|---|---|---|---|
| `Id` | Participant identifier | `1503960366` | `1503960366` |
| `ActivityHour` | Hour linked to the record | `4/12/2016 12:00:00 AM` | `4/12/2016 1:00:00 AM` |
| `StepTotal` | Step total for the record's period | `373` | `160` |

### minuteCaloriesNarrow_merged.csv

**Exports:** P1, P2. **Columns:** 3.

**P1**

| Column | Guessed meaning | Record 1 | Record 2 |
|---|---|---|---|
| `Id` | Participant identifier | `1503960366` | `1503960366` |
| `ActivityMinute` | Minute linked to the record | `3/12/2016 12:00:00 AM` | `3/12/2016 12:01:00 AM` |
| `Calories` | Calorie value for the labelled minute; exact definition and unit unverified | `0.797299981117249` | `0.797299981117249` |

**P2**

| Column | Guessed meaning | Record 1 | Record 2 |
|---|---|---|---|
| `Id` | Participant identifier | `1503960366` | `1503960366` |
| `ActivityMinute` | Minute linked to the record | `4/12/2016 12:00:00 AM` | `4/12/2016 12:01:00 AM` |
| `Calories` | Calorie value for the labelled minute; exact definition and unit unverified | `0.786499977111816` | `0.786499977111816` |

### minuteCaloriesWide_merged.csv

**Exports:** P2. **Columns:** 62.

**P2**

| Column | Guessed meaning | Record 1 | Record 2 |
|---|---|---|---|
| `Id` | Participant identifier | `1503960366` | `1503960366` |
| `ActivityHour` | Hour linked to the record | `4/13/2016 12:00:00 AM` | `4/13/2016 1:00:00 AM` |
| `Calories00` | Calories value for slot 00 within the recorded hour; slot duration unknown | `1.88759994506836` | `0.786499977111816` |
| `Calories01` | Calories value for slot 01 within the recorded hour; slot duration unknown | `2.20219993591309` | `0.786499977111816` |
| `Calories02` | Calories value for slot 02 within the recorded hour; slot duration unknown | `0.94379997253418` | `0.786499977111816` |

Only `Calories00`-`Calories02` are shown; `Calories03`-`Calories59` are omitted from this view. The CSV still has 62 columns.

### minuteIntensitiesNarrow_merged.csv

**Exports:** P1, P2. **Columns:** 3.

**P1**

| Column | Guessed meaning | Record 1 | Record 2 |
|---|---|---|---|
| `Id` | Participant identifier | `1503960366` | `1503960366` |
| `ActivityMinute` | Minute linked to the record | `3/12/2016 12:00:00 AM` | `3/12/2016 12:01:00 AM` |
| `Intensity` | Activity intensity value or code | `0` | `0` |

**P2**

| Column | Guessed meaning | Record 1 | Record 2 |
|---|---|---|---|
| `Id` | Participant identifier | `1503960366` | `1503960366` |
| `ActivityMinute` | Minute linked to the record | `4/12/2016 12:00:00 AM` | `4/12/2016 12:01:00 AM` |
| `Intensity` | Activity intensity value or code | `0` | `0` |

### minuteIntensitiesWide_merged.csv

**Exports:** P2. **Columns:** 62.

**P2**

| Column | Guessed meaning | Record 1 | Record 2 |
|---|---|---|---|
| `Id` | Participant identifier | `1503960366` | `1503960366` |
| `ActivityHour` | Hour linked to the record | `4/13/2016 12:00:00 AM` | `4/13/2016 1:00:00 AM` |
| `Intensity00` | Intensity value for slot 00 within the recorded hour; slot duration unknown | `1` | `0` |
| `Intensity01` | Intensity value for slot 01 within the recorded hour; slot duration unknown | `1` | `0` |
| `Intensity02` | Intensity value for slot 02 within the recorded hour; slot duration unknown | `0` | `0` |

Only `Intensity00`-`Intensity02` are shown; `Intensity03`-`Intensity59` are omitted from this view. The CSV still has 62 columns.

### minuteMETsNarrow_merged.csv

**Exports:** P1, P2. **Columns:** 3.

**P1**

| Column | Guessed meaning | Record 1 | Record 2 |
|---|---|---|---|
| `Id` | Participant identifier | `1503960366` | `1503960366` |
| `ActivityMinute` | Minute linked to the record | `3/12/2016 12:00:00 AM` | `3/12/2016 12:01:00 AM` |
| `METs` | MET-related activity measure; scaling unknown | `10` | `10` |

**P2**

| Column | Guessed meaning | Record 1 | Record 2 |
|---|---|---|---|
| `Id` | Participant identifier | `1503960366` | `1503960366` |
| `ActivityMinute` | Minute linked to the record | `4/12/2016 12:00:00 AM` | `4/12/2016 12:01:00 AM` |
| `METs` | MET-related activity measure; scaling unknown | `10` | `10` |

### minuteSleep_merged.csv

**Exports:** P1, P2. **Columns:** 4.

**P1**

| Column | Guessed meaning | Record 1 | Record 2 |
|---|---|---|---|
| `Id` | Participant identifier | `1503960366` | `1503960366` |
| `date` | Time linked to the sleep record | `3/13/2016 2:39:30 AM` | `3/13/2016 2:40:30 AM` |
| `value` | Sleep-related value or code; meaning unknown | `1` | `1` |
| `logId` | Sleep log identifier | `11114919637` | `11114919637` |

**P2**

| Column | Guessed meaning | Record 1 | Record 2 |
|---|---|---|---|
| `Id` | Participant identifier | `1503960366` | `1503960366` |
| `date` | Time linked to the sleep record | `4/12/2016 2:47:30 AM` | `4/12/2016 2:48:30 AM` |
| `value` | Sleep-related value or code; meaning unknown | `3` | `2` |
| `logId` | Sleep log identifier | `11380564589` | `11380564589` |

### minuteStepsNarrow_merged.csv

**Exports:** P1, P2. **Columns:** 3.

**P1**

| Column | Guessed meaning | Record 1 | Record 2 |
|---|---|---|---|
| `Id` | Participant identifier | `1503960366` | `1503960366` |
| `ActivityMinute` | Minute linked to the record | `3/12/2016 12:00:00 AM` | `3/12/2016 12:01:00 AM` |
| `Steps` | Steps for the record's period | `0` | `0` |

**P2**

| Column | Guessed meaning | Record 1 | Record 2 |
|---|---|---|---|
| `Id` | Participant identifier | `1503960366` | `1503960366` |
| `ActivityMinute` | Minute linked to the record | `4/12/2016 12:00:00 AM` | `4/12/2016 12:01:00 AM` |
| `Steps` | Steps for the record's period | `0` | `0` |

### minuteStepsWide_merged.csv

**Exports:** P2. **Columns:** 62.

**P2**

| Column | Guessed meaning | Record 1 | Record 2 |
|---|---|---|---|
| `Id` | Participant identifier | `1503960366` | `1503960366` |
| `ActivityHour` | Hour linked to the record | `4/13/2016 12:00:00 AM` | `4/13/2016 1:00:00 AM` |
| `Steps00` | Steps value for slot 00 within the recorded hour; slot duration unknown | `4` | `0` |
| `Steps01` | Steps value for slot 01 within the recorded hour; slot duration unknown | `16` | `0` |
| `Steps02` | Steps value for slot 02 within the recorded hour; slot duration unknown | `0` | `0` |

Only `Steps00`-`Steps02` are shown; `Steps03`-`Steps59` are omitted from this view. The CSV still has 62 columns.

### sleepDay_merged.csv

**Exports:** P2. **Columns:** 5.

**P2**

| Column | Guessed meaning | Record 1 | Record 2 |
|---|---|---|---|
| `Id` | Participant identifier | `1503960366` | `1503960366` |
| `SleepDay` | Date assigned to the sleep summary | `4/12/2016 12:00:00 AM` | `4/13/2016 12:00:00 AM` |
| `TotalSleepRecords` | Number of sleep records in the daily summary | `1` | `2` |
| `TotalMinutesAsleep` | Daily summary of minutes asleep | `327` | `384` |
| `TotalTimeInBed` | Daily summary of time in bed; unit unconfirmed | `346` | `407` |

### weightLogInfo_merged.csv

**Exports:** P1, P2. **Columns:** 8.

**P1**

| Column | Guessed meaning | Record 1 | Record 2 |
|---|---|---|---|
| `Id` | Participant identifier | `1503960366` | `1927972279` |
| `Date` | Time linked to the weight record | `4/5/2016 11:59:59 PM` | `4/10/2016 6:33:26 PM` |
| `WeightKg` | Weight labelled in kilograms | `53.2999992370605` | `129.600006103516` |
| `WeightPounds` | Weight labelled in pounds | `117.506384062611` | `285.719105247709` |
| `Fat` | Body-fat measure; unit unknown | `22` | `""` |
| `BMI` | Body mass index value | `22.9699993133545` | `46.1699981689453` |
| `IsManualReport` | Whether the record was entered manually | `True` | `False` |
| `LogId` | Weight log identifier | `1459900799000` | `1460313206000` |

**P2**

| Column | Guessed meaning | Record 1 | Record 2 |
|---|---|---|---|
| `Id` | Participant identifier | `1503960366` | `1503960366` |
| `Date` | Time linked to the weight record | `5/2/2016 11:59:59 PM` | `5/3/2016 11:59:59 PM` |
| `WeightKg` | Weight labelled in kilograms | `52.5999984741211` | `52.5999984741211` |
| `WeightPounds` | Weight labelled in pounds | `115.963146545323` | `115.963146545323` |
| `Fat` | Body-fat measure; unit unknown | `22` | `""` |
| `BMI` | Body mass index value | `22.6499996185303` | `22.6499996185303` |
| `IsManualReport` | Whether the record was entered manually | `True` | `True` |
| `LogId` | Weight log identifier | `1462233599000` | `1462319999000` |

Source: `EDA/input_manifest.csv`, the listed CSV headers, and the `MEANING_GUESSES` cell in [Simple EDA](<../EDA/Simple EDA.ipynb>). This section does not confirm types, keys or field meanings.

### Possible repeated measures across files - Step 6.3

Noted 2026-10-19 from the field names and previews. These are comparisons to do, not confirmed duplicate records.

| Measure to compare | Files in P2 | Check later |
|---|---|---|
| `Calories` | `dailyCalories_merged.csv` and `dailyActivity_merged.csv` | Compare the values for each matching participant and day. |
| `SedentaryMinutes` and other shared activity fields | `dailyIntensities_merged.csv` and `dailyActivity_merged.csv` | Compare all shared minute and distance fields, starting with `SedentaryMinutes`. |
| Steps: `StepTotal` versus `TotalSteps` | `dailySteps_merged.csv` and `dailyActivity_merged.csv` | Check whether the differently named fields contain the same daily values. |

Use the keys checked in Step 4. Match `Id` and the day (`ActivityDay` in the separate daily files, `ActivityDate` in daily activity). Report matching values, unmatched keys and conflicting values before deciding whether a file or field is redundant. Other possible repeated measures are also checked in Step 6.

## 8. Not checked yet

The checks above do not establish:
- Participant counts.
- What one row represents or which keys are valid.
- Recording gaps and per-participant date coverage.
- Supported field meanings, units, scaling and code definitions.
- Missing-value tokens beyond empty strings, duplicate records, or conflicting values.
- Whether the two exports can be combined.

Add these findings only after the relevant EDA checks.