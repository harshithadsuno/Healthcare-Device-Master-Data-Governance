# Healthcare FDA Medical Device Master Data Management
**FDA 510(k) Medical Device Data — End-to-End MDM Pipeline**

---

## What is this project?

I built an end-to-end MDM pipeline using the FDA 510(k) medical device 
dataset — a public registry of device submissions in the US.

The dataset has 99,216 records and looks clean on the surface. But once 
you dig into the manufacturer names, the real MDM problem shows up fast.

The same company appears dozens of different ways:
- "Boston Scientific Corp"
- "Boston Scientific Corporation"
- "BOSTON SCIENTIFIC"
- "Boston Scientific, Inc."

That's 4 versions of the same company. In the full dataset I found 
23,272 unique manufacturer name variations — and "Boston Scientific" 
alone had 21 different spellings.

This is exactly the kind of problem MDM is built to solve.

---

## What I built

A 4-step pipeline that mirrors how enterprise MDM tools like Oracle Cloud 
MDM and Informatica handle this problem:

**Step 1 — Data Profiling**
Loaded the dataset and understood what we're working with. Found missing 
values, duplicate records, and the scale of the naming problem.

**Step 2 — Data Quality Scoring**
Scored every record across 4 MDM dimensions — completeness, uniqueness, 
consistency, and validity. Composite score came out at 99.06%, which sounds 
great until you realize the scores don't capture the naming chaos.

**Step 3 — Duplicate Detection**
Used fuzzy matching (token sort ratio) to find manufacturer names that are 
similar but not identical. Found 117 candidate duplicate pairs. Classified 
them into high confidence (auto-merge candidates) and medium confidence 
(needs human review).

**Step 4 — Golden Record Creation**
Applied survivorship rules to pick the correct name between duplicates. 
Manually reviewed every high confidence pair, removed false positives like 
SRS Medical vs RS Medical (different companies), and standardized 123 records 
into clean golden records.

---

## Key numbers

| What | Result |
|------|--------|
| Total records | 99,216 |
| Unique manufacturer name variations | 23,272 |
| Boston Scientific spellings found | 21 |
| Duplicate candidate pairs | 117 |
| High confidence pairs verified | 26 |
| False positives removed | 3 |
| Records standardized | 123 |
| Medium confidence — steward review queue | 91 |

---

## Why the quality scores don't tell the full story

Completeness scored 100% because the FDA requires those fields for every 
submission. Validity scored 100% for the same reason. High scores here 
don't mean clean data — they mean regulated data.

The real problem only shows up when you do entity-level analysis on the 
manufacturer names. That's where MDM earns its value.

---

## MDM concepts covered

- Data profiling and anomaly detection
- Multi-dimensional data quality scoring
- Fuzzy string matching for entity resolution
- Frequency and casing based survivorship rules
- False positive detection through manual steward review
- Golden record creation and master dataset output
- Confidence-based stewardship workflow queue

---

## How this maps to enterprise MDM

| What I built | Enterprise equivalent |
|---|---|
| Fuzzy matching with RapidFuzz | Match and merge engine |
| HIGH / MEDIUM confidence split | Match threshold configuration |
| Manual review of wrong survivorship | Data steward review queue |
| Golden record output | Master record publication |
| duplicate_candidates.csv | Stewardship workflow queue |

---

## Tools used

Python, Pandas, RapidFuzz, Matplotlib, Seaborn

---

## Data source

FDA 510(k) Premarket Notification Database  
Download: https://www.accessdata.fda.gov/premarket/ftparea/pmn96cur.zip  
Extract pmn96cur.txt and place it in data/raw/ before running the notebooks.

---

## How to run

```bash
git clone https://github.com/harshithadsuno/Healthcare-Device-Master-Data-Governance.git
pip install pandas numpy matplotlib seaborn rapidfuzz
# Download FDA data and place in data/raw/
# Run notebooks 01 through 04 in order
```