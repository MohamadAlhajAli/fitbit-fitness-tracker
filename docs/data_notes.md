# Fitbit data notes

Updated: 2026-10-17

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

## 6. Not checked yet

The checks above do not establish:
- Participant counts.
- What one row represents or which keys are valid.
- Actual date ranges or recording gaps.
- Field meanings, units, or data types.
- Missing values, duplicate records, or conflicting values.
- Whether the two exports can be combined.

Add these findings only after the relevant EDA checks.