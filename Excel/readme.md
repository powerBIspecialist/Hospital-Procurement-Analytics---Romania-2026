#Excel

# Public Procurement Data Cleaning Workflow

My main objecive is to clean data and prepare to import in MySQL for future projects.

## Phase 1 – Initial Data Assessment

### 1. Open and Inspect the Excel File

* Download the file from:
 https://data.gov.ro/dataset/72bbb238-1a3f-49c9-bf3e-ca79e8216537/resource/39137cfc-d65b-43e3-8eca-3ec80184f041/download/raport-achizitii-directe_2026_01.xls
* Open the original Excel file.
* Identify all worksheets.
* Determine which worksheet contains the actual dataset.
* Check whether the first rows contain titles, notes, or metadata instead of data.
* Identify where the table begins.

### 2. Document the Dataset Structure

For each column:

* Record the original column name.
* Identify the data type (text, date, number, currency, etc.).
* Write a short description of its meaning.
* Mark whether the column is required for future analysis.

 --- STEP 01: Convert the file to a table format
 --- STEP 02: Define column "Castigator" as castigator range
 --- STEP 03: Remove all  (dots = .) from castigator range.  (CTRL + H) => 62709 replacements
 --- STEP 04: Replace all  "Â" from castigator range with "A".  (CTRL + H) => 537 replacements
 --- STEP 05: Remove all  (ampersamp = &) from castigator range.  (CTRL + H) => 144 replacements
 --- STEP 06: Define column "Castigator_adresa" as castigator_adresa range.
 --- STEP 07: Remove all (commas = ,) from castigator_adresa range.  (CTRL + H) => 4596 replacements
 --- STEP 08: Define column "Autoritate contractanta" as autoritate_contractanta range.
 --- STEP 09: Remove all (quotation marks = "") from autoritate_contractanta.  (CTRL + H) => 2085 replacements
 --- STEP 00: Define column "Numar Contract" as numar_contract range.
 --- STEP 10: Remove all (quotation marks = "") from numar_contract.  (CTRL + H) => 232 replacements
 --- STEP 11: Delete Column "Titlu Contract", "Tip legislatie", "Fond European", "Depozite si garantii",
             "Modalitati Finantare", "Descriere lot" and "Denumire cod CPV"
 --- STEP 12: For "Valoare Contract" row we set it as Number and remove the decimals.
 --- STEP 13: For "Valoare Minima Contract" we set it as Number and remove decimals.
 --- STEP 14: For "Valoare Maxima Contract" we set it as Number and remove decimals.
 --- STEP 15: For "CPV Code ID" we set it as Number and remove decimals.
 --- STEP 16: FOR "Valoare estimata participare" we set it as Number and remove decimals.
 --- STEP 17: Fix every error number in all columns
 --- STEP 18: For "CUI Castigator" 
 
 
Deliverable:

* Data Dictionary spreadsheet.

---

## Phase 2 – Data Quality Review

### 3. Check for Missing Values

Review all columns and identify:

* Empty cells
* NULL values
* "-" placeholders
* "N/A" values
* Other special missing-value indicators

Document:

* Number of missing values per column.

### 4. Check for Duplicate Records

Identify:

* Exact duplicate rows.
* Potential duplicate procurement records.
* Repeated transaction identifiers.

Document:

* Duplicate count.
* Duplicate removal strategy.

### 5. Validate Date Fields

Verify:

* Dates are stored as dates, not text.
* Date formats are consistent.
* No impossible dates exist.

Examples:

* Invalid dates
* Future dates where not expected
* Missing publication dates

### 6. Validate Numeric Fields

Verify:

* Consistent decimal separators.
* Consistent thousands separators.
* No text values in numeric columns.
* No negative values where impossible.

Examples:

* Contract value
* Awarded value
* Quantity fields

---

## Phase 3 – Standardization

### 7. Standardize Column Names

Apply a naming convention:

Example:

```
Contracting Authority
→ contracting_authority
```

Rules:

* Lowercase
* No spaces
* Use underscores
* Remove special characters
* Remove diacritics

### 8. Standardize Text Values

Normalize:

* Leading/trailing spaces
* Multiple spaces
* Uppercase/lowercase inconsistencies

Example:

```
SC ABC SRL
S.C. ABC S.R.L.
ABC SRL
```

Should be standardized into a single format.

### 9. Standardize Company Information

Review:

* Supplier names
* Contracting authority names
* Organization names

Identify:

* Alternative spellings
* Typographical errors
* Abbreviations

Create:

* Standard naming rules.

### 10. Validate Company Identifiers

Review:

* Tax IDs (CUI)
* Registration numbers

Check for:

* Missing values
* Invalid formats
* One identifier linked to multiple company names

---

## Phase 4 – Business Validation

### 11. Validate Procurement Identifiers

Determine:

* Which fields uniquely identify a procurement record.

Possible candidates:

* Procurement number
* Procurement number + contracting authority
* Procurement number + publication date

Create:

* Candidate primary key list.

### 12. Validate Classification Codes

Review:

* CPV codes
* Category codes
* Other classification fields

Check:

* Correct length
* Valid format
* Missing values

### 13. Review Outliers

Identify:

* Extremely large contract values
* Extremely small contract values
* Unusual suppliers
* Suspicious records

Determine:

* Valid outlier or data quality issue.

---

## Phase 5 – Clean Dataset Preparation

### 14. Create a Clean Working Copy

Keep:

```
original_file.xls
```

Create:

```
clean_file.xlsx
```

Never modify the original source file.

### 15. Apply Cleaning Rules

Perform:

* Missing value handling
* Duplicate removal
* Text standardization
* Date corrections
* Numeric corrections

Document every change.

### 16. Produce a Data Quality Report

Include:

* Total rows
* Total columns
* Missing values summary
* Duplicate summary
* Validation issues found
* Cleaning actions performed

---

## Phase 6 – Database Preparation

### 17. Define Database Schema

For each column:

* Database field name
* Data type
* Nullable / Not Null
* Business description

### 18. Define Constraints

Identify:

* Primary key
* Unique constraints
* Required fields

### 19. Define Indexing Strategy

Identify frequently queried columns:

* Supplier Tax ID (CUI)
* Supplier Name
* Contracting Authority
* CPV Code
* Publication Date
* Contract Value

### 20. Import the Clean Dataset into PostgreSQL

Only after:

* Data quality review is complete.
* Cleaning rules are documented.
* Schema design is finalized.

