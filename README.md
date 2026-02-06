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
- Invalid IPs, IPv6 entries, and placeholder strings  
- Ports outside the valid range  
- Category inconsistencies (protocol/action variations)  
- Byte fields with units, commas, or negative values  
- Missing values across multiple columns  
- ~3% duplicate rows

You will clean and standardize these fields in **Part B** of the project.
