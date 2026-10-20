# Analysis Brief 

### 1.1 Purpose and scope

**Purpose:** Understand the Fitbit dataset’s structure, coverage,
data quality, and activity patterns before choosing further analyses.

**Target population:** The participants and observation periods
represented in these exports. Conclusions will not be generalized
to all Fitbit users or the wider population.

**Intended decision:** Determine which questions the data can support,
whether the two export periods can be combined, and what to
investigate in Advanced EDA.

**Analysis mode:** Descriptive/data-readiness EDA. No predictive task,
prediction target, or modeling approach has been selected.

**Export handling:** Examine the two periods separately before
deciding whether their tables can be combined.

**Prior context:** The checksum review confirmed that existing
documentation and earlier profiling used identical inputs
(29/29 CSVs). Their findings are treated as claims to verify,
with comparisons made after producing our own results.


### 1.2 Questions

All required fields are **provisional**: they are confirmed in Steps 2–4. 

| ID | Question | Priority | Required fields (provisional) | Answered when… |
|---|---|---|---|---|
| Q1 | For **each file in each export**: what is the grain (one row = ?), the key, the columns and their types? | High | All files | Every file in each export has a verified grain/key or a logged issue |
| Q2 | Are the two exports comparable (same files, columns, participants, dates), and can each table be combined? | High | All files, participant IDs, dates | A combine / keep-separate / needs-a-rule verdict per table type |
| Q3 | How many participants are there, how many days does each contribute, and which measures does each have? | High | Participant IDs, dates, each topic | Participant × measure coverage table, with gaps described |
| Q4 | What do the data show about each field's codes, units and scaling, and which meanings stay unknown? Prior local interpretations are claims to check separately. | High | Coded and unit-bearing fields | Each field marked **supported** (with its evidence) or **Unknown / Unverified**; prior interpretations compared afterwards |
| Q5 | What quality problems exist (missingness, duplicates, invalid or questionable values, and conflicting records)? Every validity rule must be **justified** by provider docs, physical plausibility or a stated rationale. Rules flag; nothing is fixed. | High | All files | Issue log with evidence, the rule used, and proposed options |
| Q6 | How is daily activity distributed across days and participants? | Medium | Daily activity totals (if present) | Profiles at both day and participant level, with counts |
| Q7 | *(Optional)* Are there time-of-day or weekday patterns in activity? | Low | Timestamped activity (if present) | Hour/weekday profiles with contributing participants, **or** deferred to Advanced EDA |
| Q8 | What sleep information exists, and how complete is it? | Medium | Sleep records (if present) | Sleep coverage and basic distributions described |
| Q9 | *(Optional)* What heart-rate information exists: coverage and sampling? | Low | Heart-rate records (if present) | Coverage and interval profile described, **or** deferred to Advanced EDA |

**Stopping rule:** Simple EDA ends when Q1–Q5 are answered (or marked Blocked with a reason),
Q6 and Q8 have a descriptive answer or a documented reason they cannot be answered,
and Q7/Q9 are either answered or explicitly deferred to Advanced EDA.

### 1.3 Metrics 

**Rules for all metrics**
- Calculate each export (P1, P2) separately first.
- Field names, units and dates are not confirmed yet. If one changes, note it before calculating.
- Never change the raw files.
- Always show the top and bottom numbers of a rate. If the bottom is 0, write NA, not 0.
- Missing is not zero. A value that fails to parse is not "missing" either.

| ID | Metric | For | How to calculate | Notes |
|---|---|---|---|---|
| M1 | Rows per file | Q1, Q2 | Count data rows (no header) | Per file, per export |
| M2 | Key is unique? | Q1 | Unique full keys ÷ rows with a full key | Rows with part of the key empty are counted separately |
| M3 | Participants | Q2, Q3 | Count different IDs | Don't count empty IDs. Export total = IDs from all its files together, not added up |
| M4 | Days per participant | Q3 | Count dates with at least one record | Show median, min, max across participants |
| M5 | Missing rate | Q5 | Rows with a missing token ÷ all rows | Decide the missing tokens per column (include "" if used) |
| M6 | Duplicate rate | Q5 | Extra copies ÷ all rows | Compare original data columns only (not the added source/row labels). 3 identical rows = 2 extra copies. Same key with different values is a separate problem |
| M7 | Flag rate per rule | Q5 | Flagged rows ÷ rows the rule can check | Each rule needs a reason |
| M8 | Daily activity (one per field, later) | Q6 | Mean of all days, **and** mean of each person's mean | Use one checked daily value per person and date. Show people and days used. Medians allowed, labelled separately |
| M9a | By hour of day (optional) | Q7 | Each person's mean per hour, then mean across people | Use one checked hourly value per person, date and hour. Show people and observations used. Missing hours are not zero |
| M9b | By weekday (optional) | Q7 | Each person's mean per weekday (daily values), then mean across people | Use one checked daily value per person and date. Show people and observations used. Missing days are not zero |
| M10 | Who has sleep data | Q8 | People in daily activity **and** sleep tables ÷ people in daily activity table | List sleep people missing from the daily activity table |
| M11 | Heart-rate gaps (optional) | Q9 | Seconds between readings, per person | Sort valid times within each person, export and measurement source. Report duplicate times first. Keep all gaps; just label long ones |
| M12 | Sleep minutes per day | Q8 | One daily sleep total per person-day | Report duplicates or conflicts separately |


### 1.4 Pooled vs per-person averages

- **Pooled mean:** average all eligible daily values together.
  People with more recorded days have more weight.
- **Per-person mean:** average each person's eligible days first,
  then average those means. Each contributing person has equal weight.
- **M8:** report both averages and label them clearly.
- **M9a/M9b:** use per-person averages within each hour or weekday,
  as defined in 1.3.
- If the averages differ greatly, check whether people with more
  recorded days have different activity levels.

### 1.5 What the source says and what remains unknown

**The source says:**
- Participants shared Fitbit data through an Amazon Mechanical Turk
  survey in 2016.
- It reports 30 consenting users. We will count the IDs ourselves.
- Topics include activity, heart rate and sleep, with some minute-level data.
- Different tracker types and tracking habits contributed to variation.

**Unknown until checked:**
- Food intake or calories eaten.
- Age, sex, height and other personal details.
- Device model for each person.
- Health conditions or diagnoses.
- Whether reliable wear-time information exists.

Check the meaning of any calories field before using it.
Recorded activity alone does not prove the device was worn.

Source claims describe what to expect; the files determine what
is actually available.

### 1.6 Budget and completion

**Budget:** no fixed time. Move optional or deeper work to Advanced EDA
if it grows too large. Required checks must be answered or marked
Blocked with a reason.

**Done when:** the stopping rule in 1.2 is met:
- Q1–Q5 answered, or marked Blocked with a reason.
- Q6 and Q8 answered, or a reason they can't be.
- Q7 and Q9 answered or deferred to Advanced EDA.

**Not a reason to stop:** finding an interesting pattern early.

**Changes to the brief:** update the affected question or metric,
then add a dated note saying what changed and why.

**Change notes**
- 2026-10-19: Q4 reworded. The separate provider-documentation search was removed
  from Step 3, so meanings are marked supported only with evidence; otherwise
  they stay Unknown / Unverified.


### 1.7 Prediction 

**Not selected now.** This is descriptive EDA (see 1.1); there is no target and no model. 

**If we choose prediction later**, before doing it we will: 
- Define the target, when the prediction is made, and what data is available at that moment.
- Split the data by person and/or by time (not random rows), and keep a test set untouched.
- Record which data and earlier results we have already seen. A later split does not undo that exposure. 


### 1.8 Analysis data

- We use the inputs recorded in `EDA/input_manifest.csv` (file sizes and SHA-256 checksums).
- No train/test split: this is descriptive EDA, and there is no model to test.
- If an input's checksum changes, we stop and review before continuing.
- If prediction is chosen later, we follow 1.7.