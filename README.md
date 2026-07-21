# Medication Trend Analysis (PySpark)

A PySpark-based analysis of hospital prescription data, exploring medication trends by drug frequency, patient gender, and age group. Built on MIMIC-III-style clinical tables (PATIENTS, ADMISSIONS, PRESCRIPTIONS).

## Overview

This project joins patient demographics, hospital admissions, and prescription records to answer questions like:
- Which drugs are prescribed most often?
- How many prescriptions does the average patient receive?
- Do prescription patterns differ by gender or age group?

## Dataset

Expects the following CSV files in the working directory (default: `/content/` for Google Colab):

- `PATIENTS.csv` — `subject_id`, `gender`, `dob`, `dod`, etc.
- `ADMISSIONS.csv` — `subject_id`, `hadm_id`, `admittime`, `dischtime`, etc.
- `PRESCRIPTIONS.csv` — `subject_id`, `hadm_id`, `drug`, `startdate`, etc.
- `DIAGNOSES_ICD.csv` *(optional)* — enables drug-per-disease analysis if present

This matches the schema of the [MIMIC-III Clinical Database](https://physionet.org/content/mimiciii/) (demo or full version).

## Pipeline

1. **Setup** — installs Java + PySpark, starts a Spark session
2. **Load** — reads the three core CSVs with schema inference
3. **Normalize** — lowercases all column names to avoid casing mismatches
4. **Column detection** — dynamically checks which expected columns are present in each table, so the notebook degrades gracefully instead of crashing on missing fields
5. **Timestamp parsing** — converts DOB, admission time, and prescription start date to proper timestamps
6. **Join** — merges prescriptions → admissions → patients on `subject_id` (and `hadm_id` where available)
7. **Clean** — drops rows with missing drug names
8. **Analysis**:
   - Top prescribed drugs
   - Average prescriptions per patient
   - Prescriptions by gender
   - Prescriptions by age group (`0-20`, `21-40`, `41-60`, `60+`)
   - *(Optional)* Top drugs per ICD-9 diagnosis code, if `DIAGNOSES_ICD.csv` is available
9. **Visualization** — bar chart of top 10 drugs, gender pie chart, age-group bar chart
10. **Export** — saves summary tables as CSVs for download

## Requirements
