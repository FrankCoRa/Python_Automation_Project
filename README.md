# 25Live → Coursedog Legacy Data Migration

## Historical Scheduling Data Preservation Through Python Automation

### Project Overview

During the migration from **25Live** to **Coursedog**, existing system reports were unable to retrieve all historical scheduling records required for operational continuity. To address this gap, I developed a **Python-based automated data extraction solution** that preserved critical scheduling history and supported a successful enterprise system migration.

The project was developed within a Higher Education Registrar environment to ensure historical reporting, scheduling continuity, and data preservation throughout the transition.

---

## Project Highlights

- Supported the migration from **25Live** to **Coursedog**
- Preserved historical scheduling records unavailable through standard reports
- Automated data extraction using Python and Selenium
- Reduced manual data collection during migration
- Improved data quality through automated validation
- Supported institutional data continuity

---

## Business Problem

The migration from 25Live to Coursedog created a challenge: important historical scheduling records were not available through existing system reports. Without an alternative extraction method, valuable institutional scheduling history would have been lost after retiring the legacy platform.

---

## Solution

Developed a Python and Selenium automation solution that navigated the legacy system, extracted historical scheduling records, validated the collected information, and exported structured datasets to support the migration and preserve institutional data.

---

## Results

- Preserved critical historical scheduling data
- Supported a successful system migration
- Reduced manual extraction effort
- Improved consistency and reliability of historical records
- Created a reusable data extraction framework

---

## Technologies Used

- Python
- Selenium
- Chrome WebDriver
- Jupyter Notebook
- CSV

---

## Solution Architecture

```text
25Live
   │
   ▼
Python Automation
   │
   ▼
Selenium Navigation
   │
   ▼
Historical Data Extraction
   │
   ▼
Data Validation
   │
   ▼
CSV Export
   │
   ▼
Coursedog Migration
```

---

## Code Overview

The automation performs the following tasks:

- Authenticates into the legacy scheduling platform
- Navigates scheduling pages automatically
- Extracts historical scheduling information
- Validates extracted records
- Handles loading delays and exceptions
- Exports structured datasets for migration

---

## Skills Demonstrated

- Python Development
- Process Automation
- Data Extraction
- Legacy System Migration
- Data Quality Validation
- Higher Education Operations
- Process Improvement
- Operational Analytics

---

## Future Enhancements

- Direct database integration
- Incremental extraction workflows
- Automated audit logging
- Interactive migration dashboard
- Migration validation reports

---

## Conclusion

This project demonstrates how Python automation can support enterprise system modernization by preserving historical institutional data during a legacy platform migration. The solution improved operational continuity, reduced manual effort, and ensured critical scheduling records remained available following the transition from **25Live** to **Coursedog**.
