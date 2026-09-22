# Tembo Hotel & Suites — Data Cleaning Documentation

> **A structured PostgreSQL data-cleaning process focused on data quality, consistency, validation, auditability, and privacy.**

---

## 1. Project Overview

The raw **Tembo Hotel & Suites** dataset contained **286 booking records across 20 columns**.

The objective of the data-cleaning process was to:

* Standardize inconsistent values
* Handle missing and malformed data
* Convert fields into appropriate PostgreSQL data types
* Remove exact duplicate records
* Validate data quality
* Preserve uncertain records for auditability
* Prepare the dataset for reliable business analysis

After cleaning, the final dataset contains **285 records**, following the removal of one exact duplicate booking record.

### Data Cleaning Principles

> **Do not guess when the data cannot support a conclusion.**

Where a value could not be reliably determined, it was retained as `NULL` rather than being replaced with an unsupported assumption.

---

# 2. Cleaning Approach

The cleaning process followed a structured workflow:

* **Inspect** each column for missing values, duplicates, inconsistent formatting, invalid values, and data-type issues.
* **Define** a cleaning rule based on the observed data.
* **Transform** values where cleaning was required.
* **Validate** the transformed values.
* **Preserve uncertainty** by retaining unresolved values as `NULL`.
* **Perform final quality checks** before starting business analysis.

### Cleaning Workflow

```text
Raw Dataset
     ↓
Data Inspection
     ↓
Identify Quality Issues
     ↓
Define Cleaning Rules
     ↓
Apply Transformations
     ↓
Validate Results
     ↓
Handle Unresolved Values
     ↓
Final Data-Quality Checks
     ↓
Clean Dataset
     ↓
Business Analysis
```

---

# 3. Column-Level Data Cleaning

## 3.1 Booking ID — `booking_id`

The raw dataset contained **286 rows but only 285 unique booking IDs**.

The booking ID **BK0006** appeared twice, and both records were identical across all 20 columns.

### Cleaning Action

* Retained one copy of the duplicate.
* Removed the exact duplicate from the cleaned dataset.
* Preserved the original duplicate in the raw table for auditability.

**Result:** 285 unique booking records.

---

## 3.2 Guest Name — `guest_name`

Fourteen records contained leading or trailing whitespace.

Several names also had inconsistent capitalization, including:

* `ALICE MWANGI`
* `brian otieno`
* `grace wanjiru`
* `PETER MWANGI`

### Cleaning Actions

* Removed leading and trailing whitespace using `TRIM()`.
* Standardized capitalization using `INITCAP()`.
* Converted blank values to `NULL`.

No guest names were missing in the raw dataset.

### Privacy Consideration

Guest names contain **personally identifiable information (PII)** and should not be included in a publicly shared GitHub dataset.

---

## 3.3 Guest Phone — `guest_phone`

The raw data contained several phone-number formats:

* Missing or blank phone numbers
* Hyphenated Kenyan numbers
* International `+2547XXXXXXXX` numbers
* Local `07XXXXXXXX` numbers

### Cleaning Actions

* Removed spaces and hyphens.
* Converted valid `+2547XXXXXXXX` numbers to the local `07XXXXXXXX` format.
* Retained valid local `07XXXXXXXX` numbers.
* Converted blank values to `NULL`.

### Result

| Data Quality Metric        |  Result |
| -------------------------- | ------: |
| Valid phone numbers        | **271** |
| Missing phone numbers      |  **14** |
| Invalid formats identified |   **0** |

### Privacy Consideration

Phone numbers are personally identifiable information and should **not be published in a public GitHub repository**.

---

## 3.4 Guest City — `guest_city`

The raw dataset contained:

* 14 blank or whitespace-only city values
* Capitalization inconsistencies such as `NAIROBI` and `kisumu`
* One apparent spelling error: `Thikax`

### Cleaning Actions

* Removed leading and trailing whitespace.
* Standardized capitalization using `INITCAP()`.
* Converted blank values to `NULL`.
* Corrected `Thikax` to `Thika`.

---

## 3.5 Guest Nationality — `guest_nationality`

The column contained no missing values or whitespace issues.

The only inconsistency was capitalization:

* 285 records contained `Kenyan`
* 1 record contained `KENYAN`

### Cleaning Action

Values were standardized using:

```sql
TRIM()
INITCAP()
```

---

## 3.6 Room Number — `room_no`

The dataset contained **286 valid room-number records across 10 distinct rooms**.

No:

* Missing values
* Whitespace issues
* Apparent invalid room numbers

were identified.

### Cleaning Action

Room numbers were retained and converted to an **integer data type** in the cleaned table.

---

## 3.7 Room Type — `room_type`

The raw dataset contained eight variations representing four room categories:

| Raw Value | Standardized Value |
| --------- | ------------------ |
| Standard  | Standard           |
| standard  | Standard           |
| Std       | Standard           |
| Deluxe    | Deluxe             |
| deluxe    | Deluxe             |
| DLX       | Deluxe             |
| Suite     | Suite              |
| Penthouse | Penthouse          |

### Standardization Rules

```text
Standard / standard / Std
            ↓
         Standard

Deluxe / deluxe / DLX
            ↓
          Deluxe
```

### Distribution Before Deduplication

| Room Type | Records |
| --------- | ------: |
| Standard  | **115** |
| Deluxe    |  **90** |
| Suite     |  **56** |
| Penthouse |  **25** |

---

## 3.8 Room Rate Per Night — `room_rate_per_night`

The dataset contained four valid room rates:

* KES 5,500
* KES 8,500
* KES 15,000
* KES 25,000

No missing or whitespace-only values were identified.

### Cleaning Actions

* Validated the values as numeric.
* Removed unnecessary formatting.
* Stored the field as:

```sql
NUMERIC(12,2)
```

---

## 3.9 Check-In & Check-Out Dates

### Columns

```text
check_in_date
check_out_date
```

The raw data contained multiple date formats, including:

* ISO dates
* Slash-separated dates
* Four-digit hyphen-separated dates
* Two-digit hyphen-separated dates

### Cleaning Actions

Valid dates were standardized to PostgreSQL:

```sql
DATE
```

For ambiguous hyphen-separated dates, the available day/month information was used to identify unambiguous formats.

Where both components were 12 or below, the documented default convention was applied rather than claiming that the original convention could be proven from the raw data.

### Data-Quality Exceptions

Two records contain invalid date sequences:

| Booking ID | Issue                            |
| ---------- | -------------------------------- |
| **BK9002** | Check-out occurs before check-in |
| **BK9003** | Check-out occurs before check-in |

The dates were **not artificially changed** because the correct dates could not be established from the available information.

Both records were retained for auditability.

> These records should be excluded from analyses that depend on valid date intervals.

---

## 3.10 Nights Stayed — `nights_stayed`

The cleaned dataset contains stays ranging from **1 to 5 nights**, with an average of **3.01 nights**.

### Validation Results

* Minimum stay: **1 night**
* Maximum stay: **5 nights**
* Average stay: **3.01 nights**
* Zero-night stays: **0**
* Negative stays: **0**

**BK9003** has a missing `nights_stayed` value because its check-in date occurs after its check-out date.

The value was therefore retained as:

```text
NULL
```

rather than being estimated.

---

## 3.11 Staff Name — `staff_name`

The column contains **286 records representing eight distinct staff members**.

No missing values, whitespace issues, or capitalization inconsistencies were identified.

### Cleaning Action

The following functions were applied as a consistency safeguard:

```sql
TRIM()
INITCAP()
```

---

## 3.12 Staff Department — `staff_department`

The dataset contains five departments:

* Front Desk
* Housekeeping
* Management
* Restaurant
* Security

The values were already consistently formatted.

### Cleaning Action

`TRIM()` and `INITCAP()` were applied as a consistency safeguard.

No missing values or apparent spelling inconsistencies were identified.

---

## 3.13 Staff Salary — `staff_salary`

The column contained seven valid salary values ranging from:

**KES 30,000 → KES 120,000**

However, **14 records contained the malformed value:**

```text
KES ,,
```

The malformed values did not contain enough information to determine the actual salaries.

### Cleaning Actions

* Removed currency formatting from valid salary values.
* Converted valid values to numeric.
* Converted malformed `KES ,,` values to `NULL`.
* Did not infer missing salaries.

### Final Data Type

```sql
NUMERIC(12,2)
```

---

## 3.14 Payment Method — `payment_method`

The raw dataset contained inconsistent representations of M-Pesa:

```text
mpesa
M-Pesa
```

### Standardization

Both values were standardized to:

```text
M-Pesa
```

Other payment methods were standardized as:

* Cash
* Card
* Bank Transfer

Whitespace was removed and capitalization was normalized.

---

## 3.15 Booking Status — `booking_status`

Booking statuses were standardized to:

* Checked Out
* Cancelled
* No Show

Whitespace and capitalization were normalized.

### Validation

No missing booking-status values were identified.

---

## 3.16 Total Amount — `total_amount`

The raw column contained numeric, missing, and malformed values.

### Cleaning Process

Currency formatting and non-numeric characters were removed before conversion to:

```sql
NUMERIC(12,2)
```

Initially, **11 records** contained missing `total_amount` values.

Of these:

* **10 values** could be reliably calculated.
* **1 value**, belonging to BK9003, could not be determined.

### Calculation Rule

Where sufficient information was available:

```text
total_amount =
room_rate_per_night × nights_stayed + service_price
```

The 10 reliably derived values were added to the cleaned dataset.

**BK9003 remained `NULL`** because its stay duration could not be determined from the invalid date sequence.

### Validation

No zero or negative total amounts were identified after cleaning.

---

## 3.17 Service Used — `service_used`

The raw dataset contained:

| Service Status      | Records |
| ------------------- | ------: |
| Service recorded    |  **97** |
| No service recorded | **188** |

Missing service values were retained as:

```text
NULL
```

This distinction is important because:

> **A missing service record does not necessarily mean that no service was used.**

### Cleaning Action

Service names were standardized using:

```sql
TRIM()
INITCAP()
```

---

## 3.18 Service Price — `service_price`

Service prices were validated as numeric values and stored as:

```sql
NUMERIC(12,2)
```

The **188 records without a recorded service** also had missing service prices.

These values were retained as:

```text
NULL
```

Valid service prices correspond to the recorded service types.

---

## 3.19 Guest Rating — `guest_rating`

The rating field was validated against the expected **1–5 range**.

All non-null ratings were valid:

```text
1.0
2.0
3.0
4.0
5.0
```

### Validation Results

| Metric             |  Result |
| ------------------ | ------: |
| Valid rating range | **1–5** |
| Missing ratings    |  **15** |
| Invalid ratings    |   **0** |

Missing ratings were retained as `NULL` rather than being estimated.

---

# 4. Missing-Value Treatment

Missing values were **not automatically replaced with zero or another default value**.

The final cleaned dataset contains the following missing values:

| Column          | Missing Values |
| --------------- | -------------: |
| `guest_phone`   |         **14** |
| `guest_city`    |         **14** |
| `nights_stayed` |          **1** |
| `staff_salary`  |         **14** |
| `total_amount`  |          **1** |
| `service_used`  |        **188** |
| `service_price` |        **188** |
| `guest_rating`  |         **15** |

All other columns contain no missing values.

### Missing-Value Principle

> **NULL was retained whenever the underlying information could not be reliably determined from the available data.**

This prevents the cleaning process from introducing unsupported assumptions into the dataset.

---

# 5. Records Retained for Auditability

Two records require special attention.

## BK9002

The check-out date occurs before the check-in date.

The dates were not modified because the correct dates cannot be established from the available data.

## BK9003

The check-out date occurs before the check-in date.

As a result:

```text
nights_stayed = NULL
total_amount  = NULL
```

Both records remain in the cleaned table for **auditability and transparency**.

However, they should be excluded from analyses that require valid stay intervals.

---

# 6. Final Data-Quality Validation

The following checks were performed after cleaning:

| Data Quality Check           |  Result |
| ---------------------------- | ------: |
| Duplicate booking IDs        |   **0** |
| Final record count           | **285** |
| Invalid nights stayed values |   **0** |
| Invalid guest ratings        |   **0** |
| Invalid room rates           |   **0** |
| Invalid total amounts        |   **0** |
| Invalid service prices       |   **0** |
| Invalid date sequences       |   **2** |

### Final Validation Status

The cleaned dataset passed the final data-quality checks, with **two documented date exceptions** — BK9002 and BK9003 — intentionally retained for auditability.

---

# 7. Final Dataset

The cleaned table is:

```sql
tembo_hotel.clean_bookings
```

### Dataset Summary

| Attribute                   |   Value |
| --------------------------- | ------: |
| Original records            | **286** |
| Cleaned records             | **285** |
| Columns                     |  **20** |
| Duplicate booking IDs       |   **0** |
| Documented date exceptions  |   **2** |
| PostgreSQL-ready            | **Yes** |
| Ready for business analysis | **Yes** |

The final dataset contains standardized values, appropriate PostgreSQL data types, documented missing values, and documented data-quality exceptions.

It is now ready for:

* PostgreSQL business analysis
* SQL querying
* Power BI visualization
* Revenue analysis
* Occupancy analysis
* Guest analysis
* Cancellation analysis
* Hotel performance reporting

---

# 8. Data Privacy & Responsible Data Handling

The raw dataset contains **personally identifiable information (PII)**, including:

* Guest names
* Guest phone numbers

The raw CSV **should not be uploaded to a public GitHub repository**.

For the portfolio version, guest names and phone numbers should be:

* Removed
* Anonymized
* Replaced with non-identifying surrogate values

### Recommended Public Dataset Structure

Instead of exposing:

```text
guest_name
guest_phone
```

a public portfolio dataset could use:

```text
guest_id
```

For example:

```text
GUEST_001
GUEST_002
GUEST_003
```

This preserves analytical relationships without exposing personally identifiable information.

---

# 9. Key Data-Cleaning Outcomes

The cleaning process transformed the raw dataset into a structured, analysis-ready PostgreSQL table by addressing:

**Duplicates → Formatting → Data Types → Missing Values → Invalid Values → Validation → Privacy**

### Final Outcome

> **285 booking records | 20 columns | Standardized values | Validated data types | Documented exceptions | Privacy-aware handling**

The resulting `tembo_hotel.clean_bookings` table provides a reliable foundation for the next stage of the project: **business analysis and visualization.**

---

## Tools & Technologies

* **PostgreSQL** — Data cleaning, transformation, validation, and storage
* **SQL** — Data standardization and quality checks
* **Data Cleaning** — Missing-value treatment, deduplication, validation, and formatting
* **Business Analytics** — Preparing the dataset for hotel performance analysis
* **Power BI** — Planned visualization and reporting layer

---

## Project Focus

**Data Cleaning | PostgreSQL | SQL | Data Quality | Business Analytics | Hospitality Analytics | Data Privacy**
