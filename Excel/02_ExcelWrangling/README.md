# Excel Learning Series — Section 2: Excel-Based Data Wrangling

Personal learning notes on cleaning, transforming, and wrangling data in Excel — enhanced with screenshots and explanations. This is Section 2 of the ongoing Excel learning series, building on the fundamentals from **[01_Basics](../01_Basics/README.md)**.

> **Data Wrangling** is the process of cleaning, transforming, combining, and organizing raw data so it's suitable for analysis and reporting — this section covers the tools and functions that make that possible in Excel.

## 📑 Table of Contents

1. [Trim, Duplicate, Find & Replace](#1-trim-duplicate-find--replace)
2. [Case Transformations: Upper, Lower, Proper](#2-case-transformations-upper-lower-proper)
3. [Data Extraction Techniques: LEFT, RIGHT, MID, LEN](#3-data-extraction-techniques-left-right-mid-len)
4. [Combining Data with `&` / `CONCAT` / `TEXTJOIN`](#4-combining-data-with--concat-textjoin)
5. [Splitting Columns: Delimiters & Fixed Width](#5-splitting-columns-delimiters--fixed-width)
6. [Flash Fill](#6-flash-fill)
7. [Merging, Consolidating, and Appending Data](#7-merging-consolidating-and-appending-data)
8. [Date and Time](#8-date-and-time)
9. [Logical Functions](#9-logical-functions)
10. [Arithmetic Functions](#10-arithmetic-functions)
11. [Lookup Functions](#11-lookup-functions)
12. [Error Handling](#12-error-handling)
13. [Sorting, Filtering & Grouping](#13-sorting-filtering--grouping)

---

## 1. Trim, Duplicate, Find & Replace

### Remove Duplicates

Found under **Data tab → Data Tools → Remove Duplicates**. Select your data range, click Remove Duplicates, then choose which column(s) to check for duplicate values — Excel deletes any repeated rows based on those columns.

![Remove Duplicates — Data tab](images/01-remove-duplicates-data-tab.png)

![Remove Duplicates dialog — select columns to check](images/02-remove-duplicates-dialog.png)

### Find & Replace

Shortcut: select your range with `Ctrl + A`, then go to **Home tab → Find & Select → Find & Replace** (or just press `Ctrl + H`). This lets you search for a value across the sheet and replace all (or selected) occurrences at once — useful for fixing typos or standardizing values in bulk.

![Find and Replace dialog](images/03-find-and-replace.png)

### TRIM Function

Removes unwanted extra spaces (leading, trailing, or repeated spaces between words) from text — a very common first step when cleaning imported or copy-pasted data.

**Syntax:** `=TRIM(cell)`

Select the cell, type `=TRIM(` then click the cell to reference, close the bracket, and press Enter. Like most Excel formulas, this uses **cell referencing** so it updates automatically if the source cell changes.

![TRIM function removing extra spaces](images/04-trim-function.png)

### Hide / Unhide Columns

Right-click a column header and choose **Hide** to temporarily tuck a column out of view without deleting its data — right-click across the adjacent columns and choose **Unhide** to bring it back. Useful for decluttering a working sheet without losing helper columns.

![Hide and unhide columns](images/05-hide-unhide-columns.png)

## 2. Case Transformations: Upper, Lower, Proper

Three functions to standardize the capitalization of text:

| Function | Effect | Example |
|---|---|---|
| `=UPPER(cell)` | ALL CAPS | `john doe` → `JOHN DOE` |
| `=LOWER(cell)` | all lowercase | `JOHN DOE` → `john doe` |
| `=PROPER(cell)` | Capitalizes Each Word | `john doe` → `John Doe` |

`PROPER` is especially handy for cleaning up names — turning inconsistent entries like `JOHN doe` or `john DOE` into a consistent `John Doe` format.

![Case transformations — PROPER, UPPER, LOWER](images/06-case-transformations-proper-upper-lower.png)

## 3. Data Extraction Techniques: LEFT, RIGHT, MID, LEN

These functions pull specific characters out of a text string — useful for splitting codes, IDs, or any structured text without a delimiter.

| Function | What it does | Syntax |
|---|---|---|
| `LEFT` | Extracts characters from the **start** of the text | `=LEFT(cell, num_chars)` |
| `RIGHT` | Extracts characters from the **end** of the text | `=RIGHT(cell, num_chars)` |
| `MID` | Extracts characters from the **middle**, given a starting position | `=MID(cell, start_num, num_chars)` |
| `LEN` | Returns the **total number of characters** in a cell | `=LEN(cell)` |

`LEN` is often paired with the other three — for example, using `LEN` to dynamically calculate how many characters `RIGHT` or `LEFT` should pull, instead of hardcoding a fixed number.

![LEFT, RIGHT, MID, LEN in use](images/07-left-right-mid-len.png)

![MID function extracting from the middle of text](images/08-mid-function-example.png)

## 4. Combining Data with `&` / `CONCAT` / `TEXTJOIN`

Three different ways to combine (concatenate) text from multiple cells into one.

### `&` (Ampersand Operator)

The simplest way to join text or cell values.

**Syntax:** `=A1 & B1`

| A | B | Formula | Output |
|---|---|---|---|
| John | Doe | `=A1&B1` | `JohnDoe` |
| John | Doe | `=A1&" "&B1` | `John Doe` |

![Combining with the & operator](images/09-ampersand-combine.png)

**Best for:** joining just a few cells, adding spaces/symbols manually between values.

### `CONCAT()`

Joins multiple text strings or cell values — and can also combine an entire range at once.

**Syntax:** `=CONCAT(text1, text2, ...)`

```
=CONCAT(A1," ",B1)     → John Doe
=CONCAT(A1:C1)          → joins the whole range
```

![CONCAT function](images/10-concat-function.png)

**Best for:** joining multiple cells/ranges at once — this is the modern replacement for the older `CONCATENATE()` function. Note it does **not** automatically insert separators.

### `TEXTJOIN()`

Joins text using a specified separator, with the option to skip empty cells.

**Syntax:** `=TEXTJOIN(delimiter, ignore_empty, text1, ...)`

- **Delimiter** — the character inserted between values (space, comma, dash, etc.)
- **Ignore Empty** — `TRUE` or `FALSE`, decides whether blank cells are skipped
- **Text1...** — the cells or range to combine

**Example** — given column A: `Apple`, *(empty)*, `Mango`, `Banana`:

```
=TEXTJOIN(", ", TRUE, A1:A4)   → Apple, Mango, Banana        (empty cell skipped)
=TEXTJOIN(", ", FALSE, A1:A4)  → Apple, , Mango, Banana       (empty cell kept as a blank entry)
```

![TEXTJOIN function with delimiter](images/11-textjoin-function.png)

### Comparison

| Feature | `&` | `CONCAT()` | `TEXTJOIN()` |
|---|:---:|:---:|:---:|
| Combine text | ✅ | ✅ | ✅ |
| Join ranges | ❌ | ✅ | ✅ |
| Automatic separator | ❌ | ❌ | ✅ |
| Ignore empty cells | ❌ | ❌ | ✅ (TRUE/FALSE) |

**Summary:**
- **`&`** → best for combining a handful of cells
- **`CONCAT()`** → best for combining multiple cells or full ranges
- **`TEXTJOIN()`** → best for lists that need separators and empty-cell handling

## 5. Splitting Columns: Delimiters & Fixed Width

Found under **Data tab → Text to Columns**. This splits a single column of combined data into multiple columns, using one of two modes:

- **Delimited** — splits based on a specific character (comma, space, `@`, tab, etc.). Useful for things like splitting `Name, City, Region` stored in one cell, or separating an email username from its domain at the `@` symbol.
- **Fixed Width** — splits based on consistent character positions rather than a delimiter — useful when every entry follows the same fixed-length structure (e.g. a product code where the first 3 characters are always the category).

![Text to Columns — delimiter option](images/12-text-to-columns-delimiter.png)

**Example:** splitting an email address at the `@` delimiter to separate the username from the domain:

![Splitting an email column by the @ delimiter](images/13-split-email-delimiter-example.png)

![Result after splitting the email column](images/14-split-email-result.png)

## 6. Flash Fill

**Flash Fill** automatically recognizes a pattern in your data and fills in the rest for you — you just need to type **one or two examples** and Excel infers the rest.

**Access it via:** Home tab → Fill → Flash Fill (last option in the dropdown)
**Shortcut:** `Ctrl + E`

![Flash Fill option in the Fill menu](images/15-flash-fill-menu.png)

**Example use case:** if column A has full names like `John Doe` and you type `John` (just the first name) in column B for the first row, pressing `Ctrl + E` will detect the pattern and auto-fill first names for every remaining row.

![Flash Fill example — pattern auto-completed](images/16-flash-fill-example.png)

## 7. Merging, Consolidating, and Appending Data

Found under **Data tab → Consolidate** (also related to Power Query's **Combine** features on the Home tab in newer versions).

| Term | Meaning |
|---|---|
| **Merging** | Combining tables using a **common key column** (e.g. joining an Employees table and a Salary table on Employee ID) |
| **Appending** | Combining datasets by **stacking rows** one below another (e.g. combining Jan, Feb, and Mar sales sheets into one) |
| **Consolidating** | Combining and **summarizing** data from multiple sources into a single report (e.g. department-wise salary totals) |

**Example:** Consolidating salary data by department to get a categorical sum — **Data tab → Consolidate → Sum**.

## 8. Date and Time

### Core Date & Time Functions

```
=TODAY()          // Current date
=TODAY()-1        // Yesterday
=TODAY()+1        // Tomorrow
=NOW()             // Current date & time
=YEAR(A1)          // Extract the year from a date
=MONTH(A1)         // Extract the month from a date
=DAY(A1)           // Extract the day from a date
=HOUR(cell)         // Extract the hour from a time
=MINUTE(cell)       // Extract the minute from a time
=SECOND(cell)       // Extract the second from a time
```

![Date and time functions](images/17-date-time-functions.png)

![Date functions applied to a dataset](images/18-date-functions-example.png)

### DATEDIF, NETWORKDAYS, WORKDAY

**`DATEDIF()`** — calculates the difference between two dates, in years, months, or days.

```
=DATEDIF(A1,TODAY(),"Y")   // Full years between A1 and today
=DATEDIF(A1,B1,"M")         // Full months between A1 and B1
=DATEDIF(A1,B1,"D")         // Full days between A1 and B1
```

**`NETWORKDAYS()`** — calculates working days between two dates (automatically excludes weekends).

```
=NETWORKDAYS(A1,B1)
```

**`WORKDAY()`** — returns the date that falls a given number of *working* days before or after a start date.

```
=WORKDAY(A1,10)    // 10 working days after A1
=WORKDAY(A1,-10)   // 10 working days before A1
```

### Gantt Chart-Style Project Tracking

A **Gantt Chart** is a visual project-management chart showing each task's **start date, end date, duration, and progress** — helping track what's completed, ongoing, or pending. Excel can simulate one using a stacked bar chart (or conditional formatting for a lighter-weight version).

**Steps to build a Gantt Chart:**
1. Create columns: **Task**, **Start Date**, **End Date**.
2. Calculate Duration: `=C2-B2`
3. Select the data.
4. Go to **Insert → Bar Chart → Stacked Bar Chart**.
5. Set the **Start Date** series fill to **No Fill** (this is the trick that makes the bars "float" to their correct start position).
6. Reverse the task order via **Format Axis** (so the first task appears at the top).
7. Adjust colors and date formatting.

**Useful formulas for tracking:**
```
=TODAY()               // Current date, to compare against progress
=NETWORKDAYS(B2,C2)     // Task duration in working days
=C2-B2                  // Days between start and end dates
```

**Result:** a timeline chart visually showing each task's start date and duration.

![Gantt chart-style project tracking](images/19-gantt-chart-tracking.png)

## 9. Logical Functions

### IF

Tests a condition and returns one value if true, another if false.

**Syntax:** `=IF(condition, result_if_true, result_if_false)`

```
=IF(C2>50000, "High", "Low")
```

Drag/fill this down the column to apply the same logic to every row.

### IFS

Handles **multiple conditions** in a single formula, without nesting several `IF`s inside each other.

**Syntax:** `=IFS(condition1, result1, condition2, result2, ...)`

**Example** — mapping a numeric rating to a label:
```
1 = Excellent
2 = Good
3 = Average
4 = Fine
5 = Poor

=IFS(cell=1,"Excellent", cell=2,"Good", cell=3,"Average", cell=4,"Fine", cell=5,"Poor")
```

### SWITCH

Similar goal to `IFS`, but compares one value against a list of possible matches — generally **less repetitive** to write when you're checking the same cell against many exact values.

**Syntax:** `=SWITCH(cell, value1, result1, value2, result2, ...)`

```
=SWITCH(cell, 1,"Excellent", 2,"Good", 3,"Average", 4,"Fine", 5,"Poor")
```

![IF, IFS, and SWITCH in use](images/20-if-ifs-switch.png)

### AND / OR

Used to test **multiple conditions at once**, usually nested inside an `IF`:

- `=AND(condition1, condition2, ...)` → `TRUE` only if **all** conditions are true
- `=OR(condition1, condition2, ...)` → `TRUE` if **at least one** condition is true

![AND / OR functions](images/21-and-or-functions.png)

**Combined with IF** — this is where logical functions become genuinely powerful, e.g. flagging a row only when *both* (or *either*) of two conditions are met:

```
=IF(AND(C2>50000, D2="Full-Time"), "Eligible", "Not Eligible")
=IF(OR(C2>50000, D2="Manager"), "Eligible", "Not Eligible")
```

![IF combined with AND / OR](images/22-if-with-and-or.png)

## 10. Arithmetic Functions

The core aggregation functions used constantly across Excel:

| Function | Purpose | Syntax |
|---|---|---|
| `SUM` | Adds up a range of numbers | `=SUM(A1:A10)` |
| `AVERAGE` | Calculates the mean of a range | `=AVERAGE(A1:A10)` |
| `PRODUCT` | Multiplies all numbers in a range | `=PRODUCT(A1:A10)` |

![SUM, AVERAGE, PRODUCT](images/23-sum-average-product.png)

## 11. Lookup Functions

Functions that search for a value in one place and return a related value from another — the backbone of connecting related tables in Excel.

### VLOOKUP / HLOOKUP *(Legacy)*

- **`VLOOKUP`** searches **vertically** (down a column) for a value and returns data from a specified column in the same row.
- **`HLOOKUP`** searches **horizontally** (across a row) and returns data from a specified row in the same column.

```
=VLOOKUP("John Doe", full_table_range, 3, FALSE)
```
Looks up `"John Doe"`, searches the first column of `full_table_range`, and returns the value from the **3rd column** of the matching row. `FALSE` means an exact match is required (not an approximate one).

```
=HLOOKUP("Basic Salary", full_table_range, 3, FALSE)
```
Same idea, but searches across the top row and pulls from the 3rd **row** down.

> **Note:** these are considered legacy functions — still common in older spreadsheets, but **not the industry-preferred choice** today, mainly because `VLOOKUP` can only look to the right of its search column and breaks easily if columns are inserted/reordered.

![VLOOKUP and HLOOKUP in use](images/24-vlookup-hlookup.png)

### INDEX + MATCH *(Industry-Preferred)*

A more flexible and robust combination that avoids VLOOKUP's limitations — it can look in any direction and doesn't break when columns are rearranged.

```
=INDEX(salary_column, MATCH("Jane Smith", name_column, 0))
```
`MATCH` finds the **position** of `"Jane Smith"` within `name_column`, and `INDEX` returns the value at that same position from `salary_column`.

### XLOOKUP *(Modern & Powerful)*

The newest and most versatile lookup function — combines the simplicity of `VLOOKUP` with the flexibility of `INDEX`+`MATCH`, and searches in any direction by default.

```
=XLOOKUP("Bob Brown", name_column, salary_column)
```
Looks up `"Bob Brown"` in `name_column` and returns the corresponding value from `salary_column` — no column-index counting required.

### LOOKUP

A simpler, older function that works on a **sorted range** of data — it's less commonly used today but worth knowing. Data must be sorted for it to return accurate results.

```
=LOOKUP(52000, salary_range, name_column)
```
Searches for the value `52000` (or the closest value below it) within `salary_range`, and returns the corresponding entry from `name_column`.

![LOOKUP function on sorted data](images/25-lookup-function.png)

**Quick comparison:**

| Function | Direction | Exact match | Robust to column changes | Industry status |
|---|---|---|---|---|
| VLOOKUP | Right-only | Optional | ❌ | Legacy |
| HLOOKUP | Down-only | Optional | ❌ | Legacy |
| INDEX + MATCH | Any | Yes | ✅ | Preferred (pre-2019 Excel) |
| XLOOKUP | Any | Yes (default) | ✅ | Preferred (modern Excel/365) |
| LOOKUP | Sorted data only | Approximate | ❌ | Rarely used now |

## 12. Error Handling

### IFERROR

Wraps around another formula to catch errors (like `#N/A`, `#DIV/0!`, `#VALUE!`) and return a clean, friendly output instead of an intimidating error code — makes spreadsheets much more presentable and easier to debug.

**Syntax:** `=IFERROR(formula, value_if_error)`

```
=IFERROR(VLOOKUP("John Doe", full_table_range, 3, FALSE), "Not Found")
```
If the `VLOOKUP` fails to find a match, instead of showing `#N/A`, the cell will simply display `"Not Found"`.

![IFERROR handling a lookup that might fail](images/26-iferror-handling.png)

## 13. Sorting, Filtering & Grouping

### Data Sorting

Available in both the **Home tab** (Sort & Filter) and the **Data tab** (Sort). Supports:
- **Simple sort** — ascending/descending by a single column
- **Custom sort** — sort by a specific field, e.g. by Name
- **Multi-level sort** — sort by more than one column at once (e.g. sort by Name, then by Salary within each name group)

### Filter / Advanced Filter

Also found in both the **Home** and **Data** tabs.
- **Filter** — adds dropdown arrows to column headers so you can quickly show/hide rows matching selected values.
- **Advanced Filter** — lets you define a separate **criteria range** elsewhere on the sheet for more complex, multi-condition filtering than the standard dropdown filter allows.

### Group / Ungroup Rows & Columns

Found under **Data tab → Outline → Group / Ungroup**. Lets you collapse related rows or columns into an expandable/collapsible section — useful for hiding detail rows while keeping summary rows visible (e.g. collapsing daily data down to a monthly total).

![Grouping columns](images/27-group-ungroup-columns.png)

![Grouping rows](images/28-group-ungroup-rows.png)

### Subtotals with Outline

Found under **Data tab → Subtotal**. Automatically inserts subtotal rows whenever a chosen column's value changes (after sorting by that column first), and builds a collapsible outline structure alongside it — a fast way to get grouped summaries (e.g. subtotal of Sales per Region) without writing any formulas manually.

![Subtotals with outline structure](images/29-subtotals-with-outline.png)

---

## 📌 Key Takeaways from Section 2

- **Clean before you analyze**: `TRIM`, `Remove Duplicates`, and case functions (`PROPER`/`UPPER`/`LOWER`) are usually the first steps on any raw dataset.
- **`TEXTJOIN()` beats `&` and `CONCAT()`** whenever you need separators and want empty cells handled gracefully.
- **Text to Columns** and **Flash Fill** (`Ctrl + E`) are the fastest ways to split or reshape messy text columns — try Flash Fill first for pattern-based cleanup.
- For lookups, prefer **`XLOOKUP`** (or **`INDEX`+`MATCH`** in older Excel versions) over `VLOOKUP`/`HLOOKUP` — they're more flexible and don't break when columns are rearranged.
- Wrap risky formulas in **`IFERROR`** so your sheet shows clean messages instead of raw error codes.
- **Subtotals** and **Group/Ungroup** turn a flat table into a readable, collapsible summary without needing a PivotTable.

---

## 🔜 What's Next

**Section 3** will move into PivotTables, charts, and more advanced data analysis techniques building on this cleaned and organized data.

---

*These are personal learning notes, written while studying Excel data wrangling techniques — screenshots included for visual reference.*
