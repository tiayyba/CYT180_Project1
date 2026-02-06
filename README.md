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
