# CYT180 — Project 1: Cleaning Real-World Firewall Logs (15%)

## Learning Objectives
This project helps you build practical skills for working with real cybersecurity data. Firewall logs are often messy, inconsistent, and difficult to analyze until they are cleaned. By completing this project, you practice the core steps analysts take before any threat detection or monitoring work can begin.
How This Project Supports CYT180 Learning Objectives: 
- **Work with real security logs:** learn how raw firewall events look before they are cleaned or used in tools.
- **Apply Python to cybersecurity tasks:** use pandas to parse timestamps, clean fields, and standardize log formats.
- **Identify data-quality issues:** spot invalid IPs, malformed ports, inconsistent categories, and missing values.
- **Prepare data for security analysis:** produce a clean dataset suitable for dashboards, SIEM ingestion, and basic analytics.
- **Create basic security visualizations:** build a simple plot (e.g., allow vs deny) to interpret log activity.

## Overview
In cybersecurity analytics, raw log data is often messy, inconsistent, and difficult to analyze.
Your goal in this project is to clean a realistic firewall log dataset so it can be used for security monitoring and analysis.
The dataset intentionally contains several common real‑world issues, including:

- Mixed timestamp formats (two formats)
- Invalid or malformed IPv4 addresses
- Ports containing commas, blanks, or out‑of‑range values
- Inconsistent casing in protocol and action
- Byte fields with commas, k units, blanks, or a few negative values
- Missing values in several columns
- A small number of duplicate rows

You will use Python and pandas to identify these issues, clean the data, standardize all fields, and validate that the cleaned dataset is ready for analysis.
You may complete the project in Jupyter Notebook or Google Colab.

You will complete two components:
1. **Part A (5%)** — DataCamp Chapter 1: *Common Data Problems*  
2. **Part B (10%)** — Clean the provided firewall log dataset using Python and pandas

----

## Dataset Description

You will work with a synthetic but realistic firewall log dataset:

**File:** `raw_firewall_logs.csv`  
**Rows:** ~1,010 (including 10 duplicate rows)  
**Format:** CSV (comma‑separated)

| Column        | Description | Notes on Intentional Messiness |
|---------------|-------------|--------------------------------|
| **event_time** | Timestamp of the firewall event | Two formats (`YYYY-MM-DD HH:MM`, `DD/MM/YYYY HH:MM`); some blank
| **src_ip**     | Source IPv4 address | Some invalid IPv4 values (out-of-range octets or `-`) |
| **dst_ip**     | Destination IPv4 address | Same issues as source IP |
| **src_port**   | Source port number | Contains commas, blanks, or valid numeric values, or missing entirely |
| **dst_port**   | Destination port number | Contains commas, blanks/missing values, and some out-of-range values (>65535) |
| **protocol**   | Network protocol | Case-drift (e.g., `tcp`, `Udp`, `Esp`, `ICMP`) |
| **action**     | Firewall action taken | Variants such as `Allow`, ` deny `, `DENIED`, `ALLOWED` |
| **bytes_in**   | Bytes received | Commas (`1,234`), SI `k` units (`5k`, `0.5k`), blanks, a few negative values |
| **bytes_out**  | Bytes sent | Same issues as `bytes_in` |
| **country**    | Country associated with source IP | Lowercase values, blanks, and a few full country names |
| **device**     | Logging device identifier | Some values contain leading/trailing spaces |


----

## Part A — DataCamp Preparation (5%)

To prepare for the cleaning tasks in Part B, complete:

**DataCamp → Cleaning Data in Python → Chapter 1: Common Data Problems**

Access the course here:  
https://www.datacamp.com/users/sign_in?redirect=http%3A%2F%2Fapp.datacamp.com%2Flearn%2Fcourses%2Fcleaning-data-in-python

This chapter covers the essential skills needed for Part B, including:
- Identifying inconsistent data types  
- Handling missing values  
- Detecting out‑of‑range or impossible values  
- Recognizing duplicates  
- Understanding category drift and formatting issues  

### What to Submit for Part A
Upload a **screenshot** that shows:
- Your name  
- Completion of **Chapter 1**  
- A visible timestamp or progress indicator  

**Part A is worth 5% of the project grade.**

----

## Part B — Firewall Log Cleaning (10%)

In this part of the project, you will work with a synthetic firewall log dataset that intentionally contains realistic data quality issues. Your task is to inspect, clean, and standardize the data so it can be reliably used for security analysis. You will identify inconsistencies, correct formatting problems, validate your cleaned dataset, and create one small visualization based on the final results. The focus of Part B is practical data-cleaning skills using Python and pandas.

----

### Part B — Section 1: Load & Profile the Dataset

Profile the raw firewall log file to understand its structure, data quality signals, and initial issues **before** any cleaning.

1. **Load (initial inspection)**
   - Load the CSV into a DataFrame, confirm the file contains **11 columns** with the expected **column names** (order as listed above).

2. **Report shape**
   - Display the **number of rows** and **columns** (rows × columns).
   - Note that a small number of **duplicate rows** are present by design.

3. **Inspect structure and examples**
   - Show a **random sample of 10 rows**. Use df.sample(10) before any cleaning.
   - Ensure examples visibly demonstrate formatting drift (e.g., two timestamp formats, casing differences, commas, blanks).

4. **List raw data types**
   - Display the **current dtypes**. You may show either `df.dtypes` or `df.info()`.

5. **Profile missingness (raw view)**
   - Treat **empty strings** and **whitespace-only strings** as missing **for profiling purposes only**.
   - Do not modify the DataFrame yet — only count them as missing in your output.
   - Sort the missing-value counts in descending 
   - Do **not** impute, drop, or transform values in this section.

6. **Section 1 — Deliverables**
   - **Screenshot 1:** Dataset **shape** and **column names** (verifying 11 expected columns).
   - **Screenshot 2:** **random sample(10)** demonstrating raw variety.
   - **Screenshot 3:** **Missing-value counts per column** (including blanks/whitespace).
   - A brief **2–4 sentence note** summarizing observations about the raw data (e.g., two timestamp formats, port commas, casing drift, presence of duplicates).

----

### Part B — Section 2: Identify Data Problems

Before cleaning the dataset, you must identify the data-quality issues that appear in the raw firewall logs. This section focuses only on recognizing problems, not fixing them.
1. **Review the profiling results from Section 1**  
   - Use the outputs you generated earlier—such as:
      - the dataset sample,
      - missing-value counts,
      - raw dtypes,
      - and any unusual values you noticed—
   to guide your observations about what is wrong with the data.
     
2. **Identify issues across multiple columns:**
   Look for common data problems such as:
   
   - **Timestamp inconsistencies** including the two different time formats and any blank or future timestamps.
      - Example: "2025-04-12 09:30" vs "12/04/2025 09:30"
   - **Invalid IPv4 values**
      - Example: octets outside 0–255, negative numbers, or placeholder values like "-" 
   - **Ports with formatting issues**
      -  Example: commas ("1,234"), blanks, or out‑of‑range values like ports greater than 65535.
   - **Casing drift in `protocol` and `action`**
      -  Example: "tcp", "Udp", "ESP", "Allow", " deny "
   - **Bytes fields with formatting drift**
      -  Example: commas, "5k" units, blanks, or negative values
   - **Country values in inconsistent formats**
      -  Example: lowercase ("us"), blanks, or full names like "United States"
   - **Device names with extra spaces**
      - Example: "  FW-01 "
   - **Duplicate rows**
      - Full-row duplicates are intentionally included in the dataset

4. **Document each issue clearly**
   - Provide **at least eight** distinct issues.  
   - For each issue, include:
     - A **brief description** of the problem.  
     - At least **one example value** taken directly from your dataset (from your head/sample/random sample output).  
     - A short note explaining **why this issue matters** for data quality or analysis.

5. **Deliverables (include in your report)**
   - Create a table in your report (any format is fine—Markdown, Word table, or text grid) with at least eight issues, each with:
      - Problem description
      - Example value(s) from your dataset
      - Why it matters

----

## Part B — Section 3: Clean & Standardize the Data

In this section, you will clean each column of the firewall dataset to produce a consistent, analysis‑ready DataFrame.
Apply the rules column by column as described below.
Your cleaning steps must be deterministic, documented, and reproducible.
### General Requirements
- Apply cleaning **column by column** using the rules below.
- When a value cannot be cleaned to a valid representation, set it to a **missing value** (`NaN` for non‑datetime; `NaT` for datetime).
- **Do not drop rows** in this section, except when removing **full‑row duplicates** (see *Duplicates*).
- Keep a brief **log of assumptions** and **choices** you make (e.g., how you handle future timestamps).
- Ensure the final DataFrame’s columns retain their original names and order.

---

### 3.1 `event_time`
**Goal:** Convert all timestamps to valid UTC datetimes.

**Rules**
1. Parse timestamps from **only these two formats**:  
   - `YYYY-MM-DD HH:MM`  
   - `DD/MM/YYYY HH:MM`
2. Convert all successfully parsed timestamps to **UTC** (assume the raw values are naive; localize to UTC directly).
3. Set unparseable or blank timestamps to **`NaT`**.

---

### 3.2 `src_ip` / `dst_ip`
**Goal:** Valid IPv4 addresses only.

**Rules**
1. Strip any leading/trailing whitespace.
2. Validate IPv4 format:
   - four dot‑separated octets
   - each octet is an integer **0–255**.
3. Replace invalid values (including placeholders like `-`) with **`NaN`**.

---

### 3.3 `src_port` and `dst_port`
**Goal:** Numeric ports within the valid range.

**Rules**
1. Remove commas (e.g., `1,234` → `1234`).
2. Coerce to numeric (non‑numeric becomes `NaN`).
3. Keep only values in **0–65535**; out‑of‑range values → **`NaN`**.
4. Leave ports as numeric data after cleaning.

---

### 3.4 `protocol`
**Goal:** Standard, uppercase protocol categories.

**Rules**
1. Trim whitespace and convert to uppercase.
2. Keep only:
   - TCP, UDP, ICMP, GRE, ESP
4. Values outside these categories → **`NaN`**.

---

### 3.5 `action`
**Goal:** Canonical action labels.

**Rules**
1. Trim whitespace.
2. Normalize variants by mapping:
   - `ALLOWED` → `ALLOW`
   - `DENIED` → `DENY`
3. Convert to uppercase.
4. Keep only `{ALLOW, DENY}`; anything else → **`NaN`**.

---

### 3.6 `bytes_in` / `bytes_out`
**Goal:** Non‑negative numeric bytes.

**Rules**
1. Remove commas (e.g., `1,234` → `1234`).
2. Convert values ending in "k" (e.g., "5k", "0.5k") to their numeric value × 1000.
   - `5k` → `5000`
   - `0.5k` → `500`
4. Convert everything to numeric (NaN if unparseable).
5. Negative values → set to 0.
6. Leave columns as numeric types

---

### 3.7 `country`
**Goal:** Clean, uppercase country field.

**Rules**
1. Trim whitespace.
2. Convert to **uppercase** (e.g., `us` → `US`, `United States` → `UNITED STATES`).
3. Replace blank values with **`NaN`**.
4. You are **not required** to map full names to ISO codes.

---

### 3.8 `device`
**Goal:** Remove formatting artifacts.

**Rules**
1. Trim leading and trailing whitespace.
2. Leave device identifiers as simple strings after trimming.

---

### 3.9 `Duplicates`
**Goal:** Remove full-row duplicates.

**Rules**
1. Identify duplicate rows based on all columns.
2. Remove exact duplicates only.
3. Report how many duplicates were removed.

---

### 3.10  `Validation Checks`
Provide **at least 3** validation steps, such as:
- All timestamps successfully parsed  
- No ports outside 0–65535  
- All protocol values are in the valid set  
- No negative or non-numeric byte values  
- Duplicate rows removed

### 3.11 `Simple Visualization`
Create **one** small, security‑oriented plot based on the cleaned dataset. Include the plot in your report.

Examples:
- Top 10 source IPs  
- Count of ALLOW vs DENY  
- Most common destination ports  
- Event count per day  
  
---

## SUBMISSION DETAILS

You must submit a single PDF file named: `username_project1_report.pdf`
Your report must include all deliverables listed in the project:

- Part A: Screenshot of DataCamp Chapter 1 completion.
- Part B — Section 1: screenshots along with 2–4 sentence summary describing key observations about the raw dataset.
- Part B — Section 2: a table listing at least eight data-quality issues.
- Part B — Section 3 :Include screenshots showing your cleaning steps for:
   - Parsed timestamps
   - IP address validation
   - Port cleaning
   - Protocol/action normalization
   - Byte conversions (k units, commas, negatives)
   - Country/device cleanup
   - Duplicate removal (show before/after counts)
- Part B — Validation Checks
   - Include at least three validation checks.
   - Each must include:
      - A description of what you validated
      - A supporting screenshot showing the output
- Part B — Visualization Deliverable
   - One visualization created from the cleaned dataset
   - A short 1–2 sentence explanation of what the plot shows

---

## Rubric (15% Total)

### **Part A — DataCamp (5%)**
- Screenshot showing completion of **Chapter 1: Common Data Problems** (name + timestamp visible)

### **Part B — Firewall Log Cleaning (10%)**
1. Identifying Data Issues — **2.0 pts**
  - Correctly identifies **at least 8** issues in the dataset  
2. Cleaning & Standardization — **4.0 pts**
  - screenshots included in the report  
    - Parsed timestamps  
    - IP validation  
    - Port correction  
    - Protocol/action normalization  
    - Byte conversions  
    - Country/device cleanup  
    - Duplicate removal  
3. Validation Checks — **1.5 pts**
  - At least **3 meaningful checks**, such as:
    - Valid range for ports  
    - Valid protocol/action categories  
    - No negative or non‑numeric bytes  
    - No future timestamps  
    - Duplicates removed  

4. Visualization — **1.0 pt**
  - One simple, correct plot (e.g., top IPs, allow vs deny, common ports)  
  - Must be generated *after* cleaning  

5. Report Quality & Explanation — **1.5 pts**
  - Clear, concise summary (1–2 pages)  
  - Explains the problems found, cleaning steps, assumptions, and validations  
  - Demonstrates understanding of the cleaning logic  

----


### Academic Integrity
- The work you submit must be your own.
- You may discuss ideas with classmates, but you must write your own code, explanations, and analysis.
- You may use AI‑based tools for support, but:
   - Your submission must not be entirely AI‑generated.
   - You are responsible for understanding, verifying, and being able to explain all work you submit.
- All screenshots must show the current date/time and your unique username.
- Submissions may be checked for similarity, AI‑generated patterns, or unusual code structure.
