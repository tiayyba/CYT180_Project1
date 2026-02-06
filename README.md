# CYT180 — Project 1: Cleaning Real-World Firewall Logs (15%)

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
| **event_time** | Timestamp of the firewall event | Two formats (`YYYY-MM-DD HH:MM`, `DD/MM/YYYY HH:MM`); some blank or future timestamps |
| **src_ip**     | Source IPv4 address | Some invalid IPv4 values (out-of-range octets or `-`) |
| **dst_ip**     | Destination IPv4 address | Mix of public/private ranges; some invalid IPv4 values |
| **src_port**   | Source port number | Contains commas, blanks, or valid numeric values |
| **dst_port**   | Destination port number | Contains commas, blanks, and some out-of-range values (>65535) |
| **protocol**   | Network protocol | Casing drift (e.g., `tcp`, `Udp`, `Esp`, `ICMP`) |
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

3. **Report shape**
   - Display the **number of rows** and **columns** (rows × columns).
   - Note that a small number of **duplicate rows** are present by design.

4. **Inspect structure and examples**
   - Show a **random sample of 10 rows**.
   - Ensure examples visibly demonstrate formatting drift (e.g., two timestamp formats, casing differences, commas, blanks).

5. **List raw data types**
   - Display the **current dtypes** (expected to be string/object at this stage).

6. **Profile missingness (raw view)**
   - Treat **empty strings** and **whitespace-only strings** as missing **for profiling purposes only**.
   - Report **missing-value counts per column** in **descending** order.
   - Do **not** impute, drop, or transform values in this section.

7. **Section 1 — Deliverables**
   - **Screenshot 1:** Dataset **shape** and **column names** (verifying 11 expected columns).
   - **Screenshot 2:** **random sample(10)** demonstrating raw variety.
   - **Screenshot 3:** **Missing-value counts per column** (including blanks/whitespace).
   - A brief **2–4 sentence note** summarizing observations about the raw data (e.g., two timestamp formats, port commas, casing drift, presence of duplicates).

----

### Part B — Section 2: Identify Data Problems

Identify and describe at least eight distinct data-quality issues present in the raw firewall log dataset. This section focuses on recognizing patterns of inconsistency, incompleteness, and formatting drift before any cleaning is performed.

1. **Review the profiling results from Section 1**  
   - Use the outputs from your initial inspection (shape, samples, missing-value counts) to determine which fields contain inconsistencies or formatting problems.

2. **Identify issues across multiple columns:**
   
    Look for common data problems such as:
   - **Timestamp inconsistencies** including the two different time formats and any blank or future timestamps.
      - **EXample** Mixed timestamp formats. "2025-04-12 09:30" vs "12/04/2025 09:30". Why it matters: timestamps must be parsed consistently for sorting or plotting.
   - **Invalid IPv4 values**, such as octets outside the range 0–255 or placeholder values like `-`.
   - **Ports with commas, blanks, or invalid ranges**, including destination ports greater than 65535.
   - **Casing drift in `protocol` and `action`**, where the same category appears in different letter-case variations.
   - **Bytes fields containing commas, SI `k` units, blanks, or negative values**, indicating inconsistent data entry or formatting.
   - **Lowercase or blank country codes**, and occasional full country names.
   - **Device identifiers with leading or trailing spaces**, indicating formatting inconsistencies.
   - **Duplicate rows**, which should be identified before cleaning.

4. **Document each issue clearly**
   - Provide **at least eight** distinct issues.  
   - For each issue, include:
     - A **brief description** of the problem.  
     - At least **one example value** taken directly from your dataset (from your head/sample/random sample output).  
     - A short note explaining **why this issue matters** for data quality or analysis.

### Task 2 — Deliverables (include in your report)
- A **list of at least eight identified data problems**, each with a description and example(s) from your dataset.  Add this as a table in your report. 


## Part B — Section 3: Clean & Standardize the Data

### Objective
Transform the raw firewall log into a consistent, analysis‑ready dataset by applying clearly defined cleaning rules to each column. All cleaning actions should be deterministic, documented, and reproducible.

### General Requirements
- Apply cleaning **column by column** using the rules below.
- When a value cannot be cleaned to a valid representation, set it to a **missing value** (`NaN` for non‑datetime; `NaT` for datetime).
- **Do not drop rows** in this section, except when removing **full‑row duplicates** (see *Duplicates*).
- Keep a brief **log of assumptions** and **choices** you make (e.g., how you handle future timestamps).
- Ensure the final DataFrame’s columns retain their original names and order.

---

### 3.1 `event_time`
**Goal:** A single, consistent timezone (UTC) with valid datetimes.

**Rules**
1. Parse timestamps from the **two allowed formats** only:  
   - `YYYY-MM-DD HH:MM`  
   - `DD/MM/YYYY HH:MM`
2. Convert all successfully parsed timestamps to **UTC** (assume the raw values are naive; localize to UTC directly).
3. Set unparseable or blank timestamps to **`NaT`**.
4. **Future timestamps:** choose **one** policy and apply consistently:
   - **Option A (recommended for simplicity):** keep them as valid UTC datetimes.
   - **Option B:** set future timestamps to `NaT`.
   - **Option C:** drop rows with future timestamps (not recommended here; if chosen, document clearly and perform drops at the end of this section).

> **Deliverable note:** State which option you used for future timestamps.

---

### 3.2 `src_ip` / `dst_ip`
**Goal:** Valid IPv4 addresses or missing.

**Rules**
1. Strip whitespace.
2. Validate IPv4 format: four dot‑separated octets; each octet is an integer **0–255**.
3. Replace invalid values (including placeholders like `-`) with **`NaN`**.
4. Leave private ranges (e.g., `10.x.x.x`, `192.168.x.x`, `172.16–31.x.x`) as they are—these are valid.

---

### 3.3 `src_port` / `dst_port`
**Goal:** Numeric ports within the valid range.

**Rules**
1. Remove commas (e.g., `1,234` → `1234`).
2. Coerce to numeric (non‑numeric becomes `NaN`).
3. Keep only values in **0–65535**; out‑of‑range values → **`NaN`**.
4. Preserve as numeric type after cleaning.

---

### 3.4 `protocol`
**Goal:** Consistent, constrained categories.

**Rules**
1. Trim whitespace and convert to uppercase.
2. Keep only the set `{TCP, UDP, ICMP, GRE, ESP}`.
3. Values outside this set → **`NaN`**.

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
2. Convert **SI `k` units** to numbers by multiplying by **1000** (e.g., `5k` → `5000`, `0.5k` → `500`).
3. Coerce to numeric (`NaN` for blanks or unparseable).
4. **Clip negatives to 0** (retain zeros as valid).
5. Store as numeric type after cleaning.

> **Scope note:** No MB/GB or binary (Ki/Mi) units are present or required.

---

### 3.7 `country`
**Goal:** Uppercased, trimmed values with blanks as missing.

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

### 3.9 Duplicates
**Goal:** Remove exact duplicate records.

**Rules**
1. Identify **full‑row duplicates** (all columns identical).


### 3. Clean & Standardize the Data

#### **event_time**
- Parse all timestamp formats  
- Convert to a standard datetime format (UTC not required, consistency is required)  
- Handle blanks or impossible future dates  

### **src_ip / dst_ip**
- Validate IPv4 format  
- Replace invalid IPs (e.g., out-of-range octets) with NaN
  
### **src_port / dst_port**
- Remove commas  
- Convert to numeric  
- Keep only values in **0–65535**; invalid → NaN  

### **protocol**
- Normalize casing to one of: `TCP`, `UDP`, `ICMP`, `GRE`, `ESP`

### **action**
- Map small variants to:
  - `ALLOW`
  - `DENY`

#### **bytes_in / bytes_out**
- Remove commas  
- Convert simple `kk` units to integers (e.g., `"5k"` → 5000 or 5\*1024; choose one method and state your choice)  
- Convert to numeric  
- Ensure non‑negative values

#### **country**
- Convert to uppercase  
- Replace blanks with NaN
    
#### **device**
- Trim leading/trailing spaces
- 
#### **duplicates**
- Identify and drop duplicate rows  
- State your chosen duplication rule (e.g., full-row duplicates)
  
### 4. Validation Checks
Provide **at least 3** validation steps, such as:
- All timestamps successfully parsed  
- No ports outside 0–65535  
- All protocol values are in the valid set  
- No negative or non-numeric byte values  
- Duplicate rows removed

### 5. Simple Visualization
Create **one** small, security‑oriented plot based on the cleaned dataset.

Examples:
- Top 10 source IPs  
- Count of ALLOW vs DENY  
- Most common destination ports  
- Event count per day  

Include the plot in your report.

  
**Quick snapshot from the summary:**
- `rows`: 2000  
- `future_times`: 16  
- `null_times`: 20  
- `src_ip_invalid_examples`: 244 (light signals—many are simple invalid octets)  
- `dst_ip_invalid_examples`: 101  
- `src_port_commas`: 24, `dst_port_commas`: 22  
- `bytes_in_commas`: 63, `bytes_out_commas`: 46  
- `bytes_in_units_k`: 33, `bytes_out_units_k`: 32  
- `country_blank`: 43, `country_lower`: 114  
- `protocol_case_var`: 1622 (mostly casing drift), `action_variants`: 949 (Allow/Deny/ALLOWED/DENIED)  
- `duplicates_injected`: 20 (~1%)

---

## SUBMISSION DETAILS

###  `report.pdf`
A short written summary (2 pages) that includes:
- The data problems you found  
- The main cleaning steps you applied  
- Any assumptions you made  
- Three or more validation checks
- Clear screenshots of your pandas code
- Screenshots of outputs showing the cleaning steps  
- Screenshot of your visualization  
- Text explanations of what you did
- Your plot (embedded or attached)

---

## Rubric (15% Total)

### **Part A — DataCamp (5%)**
- Screenshot showing completion of **Chapter 1: Common Data Problems** (name + timestamp visible)

### **Part B — Firewall Log Cleaning (10%)**
- 1. Identifying Data Issues — **2.0 pts**
  - Correctly identifies **at least 8** issues in the dataset  
  - Issues align with the light messiness (timestamps, IPs, ports, casing drift, bytes formatting, duplicates, etc.)

- 2. Cleaning & Standardization — **4.0 pts**
  - screenshots included in the report  
    - Parsed timestamps  
    - IP validation  
    - Port correction  
    - Protocol/action normalization  
    - Byte conversions  
    - Country/device cleanup  
    - Duplicate removal  

- 3. Validation Checks — **1.5 pts**
  - At least **3 meaningful checks**, such as:
    - Valid range for ports  
    - Valid protocol/action categories  
    - No negative or non‑numeric bytes  
    - No future timestamps  
    - Duplicates removed  

- 4. Visualization — **1.0 pt**
  - One simple, correct plot (e.g., top IPs, allow vs deny, common ports)  
  - Must be generated *after* cleaning  

5. Report Quality & Explanation — **1.5 pts**
  - Clear, concise summary (1–2 pages)  
  - Explains the problems found, cleaning steps, assumptions, and validations  
  - Demonstrates understanding of the cleaning logic  

----


### Academic Integrity
- The work you submit must be your own.
- You may discuss ideas with classmates, but you must write your own code and explanations.
- All screenshots must show the current date time, unique username
- Submissions may be checked for similarity.

