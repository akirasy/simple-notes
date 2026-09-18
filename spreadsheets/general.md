---
title  : General
layout : default
parent : Spreadsheets
---

# {{ page.title }}
{: .no_toc }

## Table of Contents
{: .no_toc .text-delta }

1. TOC
{:toc}

## Cross-Sheet Filtering & Matching

These formulas use `FILTER` combined with `COUNTIF` or `COUNTIFS` to compare data across different sheets.

### 1. Basic Cross-Sheet Match (Checking for duplicates)

Use this to pull rows from one sheet only if their identifier exists on another sheet.

```
=FILTER(masterlist!A:B, COUNTIF(active!B:B, masterlist!B:B) > 0)
```

> **How it works:** `FILTER` goes through every row in `masterlist`. `COUNTIF` acts as the condition by checking if the ID in `masterlist!B:B` exists anywhere in `active!B:B`. The `> 0` ensures only rows that find at least one match are displayed.

---

### 2. Evaluate if Candidate is in a List

Useful for pulling full records based on a specific master list of names or IDs.

```
=FILTER(masterlist!A2:F, COUNTIF(active!G2:G, masterlist!B2:B) > 0)
```

> **How it works:** Identical logic to the first example, just with specific ranges. It returns data from `masterlist` (Columns A through F) only if the candidate's ID in column `B` is found in the `active` sheet's column `G`.

---

### 3. Match Candidate in List WITH Additional Criteria

Use this when checking against another sheet, but you have specific conditions that apply to that *other* sheet (e.g., a status column must be blank).

```
=FILTER(masterlist!A2:F, COUNTIFS(active!G2:G, masterlist!B2:B, active!D2:D, "") > 0)
```

> **How it works:** By upgrading to `COUNTIFS` (with an "S" for multiple criteria), you can evaluate extra rules without triggering a "mismatched range sizes" error. It evaluates the `active` sheet internally and says: *"Count how many times this ID appears in Col G **AND** has a blank Col D."* If that count is greater than zero, the `FILTER` displays the row.

