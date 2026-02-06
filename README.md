# CYT180 — Project 1: Cleaning Real-World Firewall Logs (15%)

## Overview
In cybersecurity analytics, raw log data is often messy, inconsistent, and difficult to analyze.  
Your goal in this project is to clean a realistic firewall log dataset so it can be used for security analysis and machine learning tasks.

The dataset contains intentional issues such as:
- Mixed timestamp formats  
- Invalid or malformed IP addresses  
- Ports outside valid ranges  
- Category inconsistencies in `protocol` and `action`  
- Byte fields stored as strings, units, or negative values  
- Missing values  
- Duplicate rows  

You will complete two components:
1. **Part A (5%)** — DataCamp Chapter 1: *Common Data Problems*  
2. **Part B (10%)** — Clean the provided firewall log dataset using Python and pandas

----

## Dataset Description

You will work with a synthetic but realistic firewall log dataset:

**File:** `raw_firewall_logs.csv`  
**Rows:** ~2000  
**Format:** CSV (comma‑separated)

### Columns
The dataset contains the following fields:

- **event_time** — Timestamp of the firewall event  
- **src_ip** — Source IP address  
- **dst_ip** — Destination IP address  
- **src_port** — Source port number  
- **dst_port** — Destination port number  
- **protocol** — Network protocol (e.g., TCP, UDP, ICMP)  
- **action** — Firewall decision (e.g., ALLOW, DENY)  
- **bytes_in** — Bytes received by the firewall  
- **bytes_out** — Bytes sent out by the firewall  
- **country** — Country associated with the source IP  
- **device** — Firewall or gateway device that logged the event

### Important Notes
The dataset intentionally contains:
- Mixed timestamp formats  
- Invalid IPs  
- Ports outside the valid range  
- Category inconsistencies (protocol/action variations)  
- Byte fields with units, commas, or negative values  
- Missing values across multiple columns  
- ~1% duplicate rows

You will clean and standardize these fields in **Part B** of the project.

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

## Part B — Data Cleansing

Work on `raw_firewall_logs_light.csv` using Python and pandas.

### 1. Load & profile

- Load the CSV with pandas  
- Display:
  - Shape of the dataset  
  - Column data types  
  - First few rows  
  - Count of missing values per column  


### 2. Identify Data Problems (find at least 8)


Look for issues such as:
- Mixed timestamp formats (3 simple formats)
- A few blank or future timestamps  
- Invalid IPv4 addresses (octets out of range)  
- Ports stored with commas or out of valid range  
- `protocol` values with casing differences (`tcp`, `Udp`)  
- `action` variations (`Allow`, `DENIED`)  
- `bytes_in` / `bytes_out` containing commas or `k` units  
- Lowercase or blank country codes  
- Devices with leading/trailing spaces  
- ~1% duplicate rows  

List the issues you find in your report.


### 3. Clean & Standardize the Data

#### **event_time**
- Parse all timestamp formats  
- Convert to a standard datetime format (UTC not required, consistency is required)  
- Handle blanks or impossible future dates  


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
