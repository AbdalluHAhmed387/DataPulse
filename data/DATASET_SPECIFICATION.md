# DataPulse Synthetic Test Datasets Specification

This directory contains synthetic benchmark datasets designed specifically for testing, demonstrating, and evaluating **DataPulse – Universal Data Quality & Analytics Platform**.

Each domain includes:
* **`*_clean.csv`**: Baseline dataset adhering 100% to valid domain constraints, proper formats, and zero duplicates.
* **`*_dirty.csv`**: Realistic raw dataset injected with controlled anomalies across the 6 DataPulse Quality Dimensions: **Completeness**, **Uniqueness**, **Validity**, **Consistency**, **Accuracy**, and **Timeliness**.

---

## 1. Healthcare Domain

* **Files:**
  * Clean: `healthcare_patients_clean.csv` (1,000 records)
  * Dirty: `healthcare_patients_dirty.csv` (1,000 records)
* **Schema:**
  * `patient_id` (String): Primary record identifier (Format: `PAT-XXXXX`)
  * `visit_date` (Date): Date of clinical visit (`YYYY-MM-DD`)
  * `age` (Integer): Patient age (Valid range: `0 - 110`)
  * `blood_pressure` (String): Systolic/diastolic measurement (`###/##`)
  * `test_result` (Float): Blood glucose test result in mg/dL (`70.0 - 180.0`)
  * `department` (Categorical): Clinical department
  * `medication_code` (String): Standardized medication reference code
* **Injected Anomalies in Dirty Dataset:**
  1. **Completeness:** 25 missing `patient_id` values (empty strings).
  2. **Uniqueness:** 30 duplicate row records + 20 key collisions with differing data.
  3. **Validity (Dates):** Invalid dates such as `"2026-15-40"`, `"32/13/2026"`, `"2026-02-31"`, and missing dates.
  4. **Validity (Ranges):** Negative ages (`-5`), impossible ages (`250`, `999`), and non-numeric strings (`"twelve"`, `"N/A"`).
  5. **Validity (Format):** Malformed blood pressures (`"350/220"`, `"120/"`, `"/80"`, `"ERR_BP"`).
  6. **Consistency (Text normalization):** Whitespace padding and mixed casings (`"  Cardiology "`, `"cardiology"`, `"CARDIOLOGY"`).
  7. **Accuracy (Reference data):** 30 unapproved medication codes (`"UNKNOWN_DRUG_999"`).
  8. **Anomaly Detection:** Outlier measurements (`9999.9`, `-15.0`, `"ERROR_VAL"`).

---

## 2. Business & Finance Domain

* **Files:**
  * Clean: `finance_transactions_clean.csv` (1,200 records)
  * Dirty: `finance_transactions_dirty.csv` (1,200 records)
* **Schema:**
  * `transaction_id` (String): Unique transaction identifier (`TXN-XXXXX`)
  * `transaction_date` (Date): Transaction timestamp (`YYYY-MM-DD`)
  * `account_id` (String): Account identifier (`ACC-XXXX`)
  * `amount` (Decimal): Monetary value of the transaction
  * `currency` (String): 3-letter ISO currency code (`USD`, `EUR`, `GBP`, etc.)
  * `transaction_type` (Categorical): `Payment`, `Debit`, `Credit`, `Transfer`, `Refund`, `Fee`
  * `status` (Categorical): `Completed`, `Pending`, `Failed`, `Cancelled`
* **Injected Anomalies in Dirty Dataset:**
  1. **Completeness:** 30 missing transaction IDs.
  2. **Uniqueness:** 35 duplicate transactions + 30 ID collisions with conflicting amounts.
  3. **Validity (Currency codes):** Invalid currency codes (`"USDD"`, `"XXY"`, `"$"`, `"usd"`, `" eur "`).
  4. **Validity (Dates):** Non-existent dates (`"2026-02-30"`, `"2026-99-99"`).
  5. **Domain Integrity (Amounts):** Negative amount for non-refund transaction types (e.g. Payment of `-1250.00`).
  6. **Type Inconsistency:** Formatted strings in numeric amount column (`"$1,250.00"`, `"free"`, `"100.50.20"`).
  7. **Consistency (Status):** Whitespace padding and non-standard casing (`" completed "`, `"COMPLETED"`).
  8. **Referential Integrity:** References to non-existent account IDs (`ACC-999999`).

---

## 3. Education Domain

* **Files:**
  * Clean: `education_students_clean.csv` (1,000 records)
  * Dirty: `education_students_dirty.csv` (1,000 records)
* **Schema:**
  * `student_id` (String): Student registration ID (`STU-XXXX`)
  * `course_id` (String): Course code (`CS-101`, `MATH-201`, etc.)
  * `enrollment_date` (Date): Date student enrolled (`YYYY-MM-DD`)
  * `attendance_rate` (Decimal): Decimal attendance percentage (`0.00 - 1.00`)
  * `exam_score` (Decimal): Continuous exam score (`0.0 - 100.0`)
  * `semester` (String): Academic term (`Fall 2025`, `Spring 2026`, etc.)
  * `grade` (Categorical): Letter grade (`A`, `B`, `C`, `D`, `F`)
* **Injected Anomalies in Dirty Dataset:**
  1. **Uniqueness (Composite Key):** 35 duplicate enrollments for the same student + course + semester.
  2. **Validity (Attendance):** Rates > 1.0 or < 0.0 (`1.45`, `-0.20`, `"150%"`).
  3. **Validity (Exam scores):** Scores outside `0 - 100` (`-15.0`, `135.0`, `"ABSENT"`).
  4. **Consistency Mismatch:** High exam score (97.0) paired with grade `"F"` and low exam score (40.0) paired with grade `"A"`.
  5. **Normalization:** Inconsistent semester formatting (`"fall 2025"`, `"SPRING 2026"`, `"2026_SP"`).
  6. **Validity (Grades):** Invalid letter grades (`"Z+"`, `"PASS"`, `"A++"`).

---

## 4. E-commerce Domain

* **Files:**
  * Clean: `ecommerce_orders_clean.csv` (1,500 records)
  * Dirty: `ecommerce_orders_dirty.csv` (1,500 records)
* **Schema:**
  * `order_id` (String): Unique order code (`ORD-XXXXX`)
  * `product_id` (String): Product catalog reference (`PRD-XXX`)
  * `customer_id` (String): Customer account reference (`CUST-XXXX`)
  * `order_date` (Date): Date order was placed (`YYYY-MM-DD`)
  * `quantity` (Integer): Number of items purchased (`> 0`)
  * `price` (Decimal): Unit price (`> 0.00`)
  * `total_amount` (Decimal): Order subtotal (`quantity * price`)
  * `order_status` (Categorical): `Delivered`, `Shipped`, `Processing`, `Cancelled`, `Returned`
* **Injected Anomalies in Dirty Dataset:**
  1. **Completeness:** 30 missing `order_id` values and 30 missing `customer_id` values.
  2. **Uniqueness:** 40 duplicate order records.
  3. **Validity (Quantity):** Non-positive or non-integer quantities (`-2`, `0`, `3.5`, `"ten"`).
  4. **Validity (Price):** Negative and zero prices (`-19.99`, `0.00`, `"FREE"`).
  5. **Consistency (Calculation):** 50 records where `total_amount` is hardcoded to `9999.00` instead of `quantity * price`.
  6. **Validity (Status):** Unrecognized order statuses (`"LostInTransit"`, `"GhostOrder"`) and unnormalized casing (`"shipped"`).
  7. **Validity (Dates):** Invalid calendar dates (`"2026-02-31"`).

---

## Testing & Demonstration Usage

Run the verification test script or load into DataPulse:
```bash
python generate_test_datasets.py
```
These datasets allow verification of:
1. **Profiling Engine:** Correctly detecting null counts, distinct counts, schema inference, and statistical distributions.
2. **Cleaning Engine:** Safe deduplication, trimming whitespace, and handling missing values without data loss.
3. **Scoring Engine:** Measuring the drop in quality score from `*_clean.csv` (~98-100%) to `*_dirty.csv` (~65-75%).
4. **Before-and-After Comparison:** Proving that DataPulse improves dataset metrics while leaving raw inputs intact.
