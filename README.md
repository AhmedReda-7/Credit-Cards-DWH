# 💳 Credit Card Data Warehouse Project

A full-scale Data Warehousing project simulating a real-world banking scenario, built using **Informatica PowerCenter**. This project demonstrates secure and efficient processing of sensitive credit card data — featuring masking, validation, archival, and error handling — following enterprise-grade ETL and data governance best practices.

---

## 🧠 Project Summary

This project implements a robust ETL pipeline that ingests credit card data from various Oracle source systems and flat files, processes and validates it, masks sensitive fields, and categorizes it into **current** or **archived** datasets based on a configurable date. Invalid records are logged and quarantined. The workflow includes file-based triggers and is parameterized for flexibility and automation.

---

## ⚙️ Technologies & Tools

- **ETL Tool**: Informatica PowerCenter
- **Database**: Oracle
- **File Types**: Flat Files (.csv, .out)
- **Languages/Concepts**: SQL, Parameterization, Data Masking, Surrogate Keys
- **Execution Control**: Event Wait File (File Watcher)
- **Output Formats**: Excel, CSV, Out

---

## 📂 Data Flow Overview

### Source Systems

- `DEMO_CREDIT_CARD`
- `DEMO_CUSTOMER`
- `DEMO_EMPLOYEE`
- `DEMO_ACCOUNT`
- `DEMO_DEPT`
- Flat file: `CREDIT_CARD_ALL.out`

### ETL Process Steps

1. **Extraction**: Load raw data from Oracle and flat files.
   - Credit Cards Extraction
3. **Transformation**:
   - Mask sensitive credit card numbers (`CC_NUMBER_MASKED`)
   - Join customer, employee, department data
   - Assign incremental surrogate keys (`CC_SUR`)
4. **Validation**:
   - Filter out invalid credit card numbers (≠ 16 digits)
   - Log rejected records to `REJECTED_DATA.csv`
5. **Routing**:
   - Records before `$$date` → `CREDIT_CARDS_ARCHIVE`
   - Records on/after `$$date` → `CREDIT_CARDS_CURRENT`
6. **Loading**: Append or update to flat file targets without truncation



---

## 🧩 Key Features

- 🔐 **Credit Card Masking**: Keeps first 3 and last 3 digits (e.g. `123XXXXXXXXXXX789`)
- 🧮 **Surrogate Keying**: Persistent, non-reset incremental key
- ⛔ **Rejection Handling**: Logs invalid data to a separate CSV file
- 📁 **Archiving Logic**: Based on dynamic parameter `$$date`
- 🔄 **Update Mode**: Smart inserts/updates (no truncation)
- 📄 **Execution Gate**: Runs only when a `ready` file is detected (Event Wait)

---

## 📁 File Structure

```bash
📁 Project Root
│
├── 📂 Source Files
│   ├── DEMO_CREDIT_CARD table
│   ├── DEMO_CUSTOMER table
|   ├── DEMO_EMPLOYEE table
|   ├── DEMO_ACCOUNT table
|   ├── DEMO_CUSTOMER table
|   ├── DEMO_DEPT table
|   ├── DEMO_CITY table
│   └── CREDIT_CARD_ALL.out
│
├── 📂 Target Outputs
│   ├── CREDIT_CARDS_CURRENT.out
│   ├── CREDIT_CARDS_ARCHIVE.out
│   └── REJECTED_DATA.csv
│
├── 📂 Mappings
│   ├── M_CC_DWH
│   └── M_CC_ARCHIVING
│
├── 📂 Workflows
│   ├── WF_CC_DWH
│   └── WF_CC_ARCHIVING
│
├── parameters.txt
└── README.md
