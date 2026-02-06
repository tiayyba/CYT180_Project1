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

### 1) Load & profile
- Read the CSV
- Show: shape, dtypes, first 5 rows
- Count missing values by column

### 2) Identify issues (find ≥ 8)
Examples you are likely to see:
- Mixed timestamp formats (3 formats)
- A few **future** or **blank** timestamps
- Invalid IPv4s (octets out of range)
- Ports stored with commas or slightly out of range
- `protocol` casing drift only (e.g., `tcp`, `Udp`)
- `action` small variants (e.g., `Allow`, `DENIED`)
- `bytes_*` with commas or simple `k` units
- Lowercase or blank `country`
- `device` values with leading/trailing spaces
- ~1% duplicate rows

### 3) Clean & standardize

**event_time**
- Parse the 3 formats and convert to **UTC**
- Remove or fix future timestamps; handle blanks

**src_ip / dst_ip**
- Validate IPv4; set invalid entries to NaN

**src_port / dst_port**
- Convert to numeric
- Enforce valid range **0–65535**
- Remove commas; coerce invalid to NaN

**protocol**
- Normalize casing to one of: `TCP`, `UDP`, `ICMP`, `GRE`, `ESP`

**action**
- Map small variants to:
  - `ALLOW`
  - `DENY`

**bytes_in / bytes_out**
- Remove commas
- Convert `\"NNNk\"` to integer bytes (kB → bytes or kB → integer k; state your choice)
- Ensure non‑negative integers; coerce invalid to NaN

**country**
- Normalize to **uppercase 2‑letter** codes where possible
- Leave blanks as NaN if unknown

**device**
- Trim whitespace

**duplicates**
- Identify duplicates using a reasonable subsetLove it—**light** is the right call for Project 1.

I’ve regenerated a **lighter** firewall dataset with **3 timestamp formats**, **no IPv6**, and **reduced messiness** (~1% duplicates, minimal out‑of‑range and unit issues).

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

## Update to the GitHub‑Markdown (Part B Overview stays, but scope is lighter)
Let’s adjust **SECTION 4** (Part B Overview) to reflect the simplified workload:

```markdown
## Part B — Firewall Log Cleaning (10%) — Light Messiness

You will clean **raw_firewall_logs_light.csv** using Python and pandas.

Scope is intentionally **light**:
- **Timestamps:** 3 formats; very few future/blank values
- **IP addresses:** only IPv4; some invalid octets (no IPv6)
- **Ports:** a few comma-formatted strings and a few out-of-range values
- **Protocol/Action:** casing drift and minor synonyms only
- **Bytes:** some commas or simple `k` units (no MB/GB); no negatives
- **Country:** some lowercase codes and occasional blanks
- **Device:** occasional leading/trailing spaces
- **Duplicates:** ~1% of rows

Your goal: produce a **clean, analytics-ready** CSV suitable for basic security analysis (e.g., top source IPs, allow/deny patterns).
