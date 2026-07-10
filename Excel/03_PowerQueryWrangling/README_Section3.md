# Excel Learning Series — Section 3: Power Query–Based Wrangling

Personal learning notes on **Power Query** — Excel's dedicated data-cleaning and transformation engine — enhanced with screenshots and explanations. This is Section 3 of the ongoing Excel learning series, building on **[01_Basics](../01_Basics/README.md)** and **[Section 2: Excel-Based Wrangling](../Section2/README.md)**.

> **Power Query** is a separate editor built into Excel for importing, cleaning, reshaping, and combining data *before* it ever lands on a worksheet. Unlike formulas in a cell, every action you take in Power Query is recorded as a repeatable **step** — so the whole cleanup process can be re-run automatically if the source data changes (e.g. a monthly report refresh).

## 📑 Table of Contents

1. [Importing Data](#1-importing-data)
2. [Cleaning Raw Data](#2-cleaning-raw-data)
3. [Transforming Data](#3-transforming-data)
4. [Adding Columns](#4-adding-columns)
5. [Merge & Append Queries](#5-merge--append-queries)
6. [Query Steps / Applied Steps](#6-query-steps--applied-steps)
7. [Transpose vs. Pivot vs. Unpivot vs. Transform — The Key Differences](#7-transpose-vs-pivot-vs-unpivot-vs-transform--the-key-differences)

---

## 1. Importing Data

**Path:** `Data tab → Get Data → From File` (or From Web, From Database, etc., depending on the source)

Power Query can pull data from almost anywhere — Excel workbooks, CSVs, text files, databases, and web pages. This is fundamentally different from just opening a file: Power Query treats the import as a **connection**, so if the source file updates later, you can refresh the query instead of re-importing manually.

![Get Data — Data tab options](images/01-get-data-ribbon.png)

When you choose a source, a **Navigator** window opens first. If the source workbook has multiple tables or sheets, the Navigator lets you preview each one and select which to bring in — you can select one or multiple items.

![Navigator window — previewing and selecting tables](images/02-navigator-window.png)

Once selected, you choose either:
- **Load** → brings the data straight into a worksheet as-is
- **Transform Data** → opens the **Power Query Editor**, where you can clean and reshape the data *before* it ever touches a worksheet — this is almost always the better choice for raw/messy data.

![Power Query Editor after choosing Transform Data](images/03-power-query-editor.png)

**Example dataset used throughout this section:** a workbook with three related tables —
- **Raw Data** (the main employee dataset needing cleanup)
- **Department Data** (used later for **merging**)
- **More Employees** (used later for **appending**)

### The Power Query Editor Ribbon

The Power Query Editor has its own separate ribbon with four key tabs:

- **Home** — data source management, removing rows/columns, splitting, merging, and appending queries
- **Transform** — reshaping existing columns: data types, trim/clean, pivot/unpivot, transpose
- **Add Column** — creating brand-new columns (custom, conditional, index, extracted)
- **View** — inspecting data quality, column distribution, and column statistics

![Power Query ribbon tabs overview](images/04-pq-ribbon-tabs.png)

![Home tab in Power Query](images/05-pq-home-tab.png)

![Transform tab in Power Query](images/06-pq-transform-tab.png)

![Add Column tab in Power Query](images/07-pq-add-column-tab.png)

### View Tab — Always Check This First

Before cleaning anything, the **View tab** gives you a quick health check of your dataset:
- **Column Quality** — shows the percentage of Valid, Error, and Empty values in each column
- **Column Distribution** — shows the number of distinct and unique values per column
- **Column Statistics** — min, max, average, and other summary stats for numeric columns

**Example:** in this dataset, the *Department* column showed **19% empty values** — spotted instantly from the Column Quality view, without having to scroll and eyeball the whole table.

![View tab showing column quality and distribution](images/08-view-tab-column-quality.png)

> 💡 **Best practice:** always check the View tab **first**, before doing any cleaning — it tells you exactly which columns need attention (nulls, errors, inconsistent types) so you don't waste time cleaning columns that don't need it.

## 2. Cleaning Raw Data

### Remove Nulls

**Path:** select the column → `Transform tab → Replace Values`

Replaces blank/null entries with a placeholder (e.g. `"NA"`) or removes rows containing them entirely, depending on the approach. Using Replace Values with a blank search value lets you swap every empty cell in a column for a consistent marker, making it easy to filter or flag incomplete records later.

![Remove nulls via Replace Values](images/09-remove-nulls-replace-values.png)

### Trim & Clean

**Path:** select the column → `Transform tab → Format → Trim` (or **Clean**, which additionally strips non-printable characters)

**Example:** the *Full Name* column had inconsistent leading/trailing spaces. Selecting the column and applying **Trim** removed the extra spaces in one click. Every transformation like this is automatically recorded as a step in the **Applied Steps** panel (Query Settings pane on the right) — so it's fully visible and reversible.

![Trim & Clean applied, visible in Applied Steps](images/10-trim-clean-applied-steps.png)

### Replace Errors

**Path:** select the column (e.g. *Email*) → `Transform tab → Replace Values`

When a column contains error values (shown as `#ERROR` in Power Query), you can replace them with a clean placeholder like `"NA"` instead of leaving broken cells in the dataset — keeping the table usable for downstream steps like merging or lookups.

![Replace Errors in the Email column](images/11-replace-errors-dialog.png)

### Fixing Data Types

Power Query stores a **data type per column** (Text, Whole Number, Decimal, Date, etc.), shown as a small icon in each column header. Getting types right matters because calculations, sorting, and merges can behave incorrectly if a numeric or date column is accidentally stored as text.

![Data types shown in column headers](images/12-fixing-data-types-overview.png)

There are two ways to change a column's type:

**1. Quick method** — click the small type icon/dropdown arrow in the column header and pick the correct type directly.

![Changing data type via the column header dropdown](images/13-change-data-type-column-header.png)

**2. Ribbon method** — select the column → `Transform tab → Data Type` dropdown → choose the type.

![Changing data type via the Transform tab](images/14-transform-data-type-menu.png)

### Fill Down

**Path:** select the column → `Transform tab → Fill → Down`

Fills blank cells with the value from the nearest non-blank cell **above** them — commonly needed when a source table only labels the first row of a repeated group (e.g. a category name that's only typed once and left blank for the following rows).

**A related but distinct case covered here:** the *Department Data* table had its real column names sitting in the *first data row* instead of the header row (so column headers just read A, B, C).

![Column headers showing generic A/B/C instead of real names](images/15-row-values-as-headers-need.png)

**Fix:** `Home tab → Use First Row as Headers` — promotes the first row of data into the header row.

![Use First Row as Headers](images/16-use-first-row-as-headers.png)

After this, Power Query also auto-detects and updates the data type for each newly-named column — and both actions appear as separate, individually editable entries in **Applied Steps**.

![Applied Steps showing the header promotion and resulting data type changes](images/17-applied-steps-datatype-change.png)

## 3. Transforming Data

Transforming data means **reshaping its structure** — not just cleaning values, but changing how rows and columns are organized. Power Query's `Add Column` tab is also where **new columns** get created (see [Adding Columns](#4-adding-columns) below), including conditional columns, index columns, duplicate columns, and invoking custom functions.

![Add Column tab — Custom, Conditional, Index, Duplicate, Invoke Custom Function](images/18-add-column-tab-overview.png)

The transformation techniques below — **Split, Rename, Extract, Transpose, Pivot/Unpivot** — are the core tools for reshaping a table's structure or its individual columns.

- **Split** → breaks one column into multiple columns (by delimiter or fixed width), same concept as Text to Columns in Section 2, but as a repeatable, recorded Power Query step.
- **Rename** → simply double-click a column header (or right-click → Rename) to give it a clearer, more meaningful name.
- **Extract** → pulls out a portion of a column's text (e.g. text before/after a delimiter) into a new column — covered in detail below.
- **Transpose** → flips the entire table, turning rows into columns and columns into rows.
- **Pivot / Unpivot** → reshapes data between "wide" and "long" formats — covered in detail in [Section 7](#7-transpose-vs-pivot-vs-unpivot-vs-transform--the-key-differences) below, since this is where most confusion happens.

### Extract

**Path:** select the column → `Add Column tab → Extract → Text Before Delimiter` (or After Delimiter, Between Delimiters, etc.)

**Example:** extracting the username portion of an email address before the `@` symbol into a brand-new column, while keeping the original email column intact.

![Extract — text before delimiter](images/21-extract-text-before-delimiter.png)

Once extracted, the new column can be:
- **Renamed** to something meaningful (e.g. `Username`)
- Have its **null/error values replaced** with a placeholder
- **Dragged** to reposition it anywhere in the column order

![Extracted column — renamed and repositioned](images/22-extract-rename-result.png)

## 4. Adding Columns

### Custom Columns

**Path:** `Add Column tab → Custom Column`

Lets you write a formula (using Power Query's **M language**, similar in spirit to Excel formulas) to generate a brand-new column from existing ones.

**Example:** creating a **Total Salary** column that combines the *Salary* and *Bonus* columns into a single computed value for each row.

![Custom column formula — Total Salary from Salary + Bonus](images/20-custom-column-formula.png)

There's also a **Conditional Column** option (`Add Column tab → Conditional Column`), which is essentially a no-code `IF`/`ELSE` builder — you define one or more conditions (e.g. "if Salary > 50000") and specify the output for each case, without writing any formula syntax.

![Conditional Column — if/else condition builder](images/19-conditional-column-dialog.png)

### Index Columns

**Path:** `Add Column tab → Index Column`

Adds a new column with a simple sequential number for each row (starting from 0 or 1). Useful as a unique row identifier, for preserving original row order after sorting/filtering, or as a join key when no natural unique identifier exists in the data.

## 5. Merge & Append Queries

These two operations both **combine two tables**, but in fundamentally different directions — this distinction matters a lot in practice:

| | Merge | Append |
|---|---|---|
| **What grows** | **Columns** (adds more fields per row) | **Rows** (stacks more records) |
| **Requires** | A common key/basis column between both tables | Matching column structure between both tables |
| **Analogy** | SQL `JOIN` | SQL `UNION` |
| **Watch out for** | Choosing the correct join type (see below) | Mismatched column counts/names → unmatched columns fill with `null` |

### Merging

**Path:** `Home tab → Merge Queries`

Combines two tables side-by-side based on a **common key column** present in both (e.g. matching *Employee ID* in the Raw Data table with *Department Data*).

![Merge Queries dialog](images/27-merge-queries-dialog.png)

You select **which table** to merge with, then specify the **matching column(s)** — the shared basis both tables have in common (like a primary/foreign key relationship in a database).

![Selecting the common key column in both tables](images/28-merge-select-common-key.png)

After merging, the second table's data initially collapses into a single new column of nested tables — you then choose exactly **which columns** from that second table to expand into your main table.

![Choosing which columns to expand after merging](images/29-merge-expand-columns.png)

![Merge result — new columns pulled in from the second table](images/30-merge-result-expanded.png)

> **Key rule:** merging always needs **one column both tables have in common** — this is what Power Query uses to match rows correctly, exactly like a join key in a database.

### Appending

**Path:** select the base table → `Home tab → Append Queries` → choose the second table to append

Stacks the rows of one table underneath another — used when you have the same *kind* of data split across multiple tables (e.g. combining an *Employees* table with a *More Employees* table).

![Append Queries dialog](images/31-append-queries-dialog.png)

**Example caveat encountered:** the *Employees* table (after merging) had *more* columns than the *More Employees* table. Appending them anyway is possible, but any column that only exists in one of the two tables comes through as **null** for every row from the table that didn't have it.

![Appended result — unmatched columns filled with null](images/32-appended-result-with-nulls.png)

> **Key rule:** before appending, check that both tables have the **same column count and names** — mismatches don't cause an error, but they *do* leave gaps of `null` values, which usually isn't the clean result you want.

**Quick summary:**
- **MERGE** → columns increase → requires one common basis column
- **APPEND** → rows increase → check column count/names match first for a clean result

## 6. Query Steps / Applied Steps

Every single action taken in the Power Query Editor — importing, renaming, trimming, changing a data type, merging, appending — is automatically recorded as a **step** in the **Applied Steps** list, found in the **Query Settings** pane on the right-hand side of the editor.

![Query Settings pane — Applied Steps list](images/33-applied-steps-query-settings.png)

This is one of Power Query's most powerful features:
- You can **rename the query/table** directly in Query Settings.
- Every step can be **clicked** to preview the data exactly as it looked at that point in the process.
- Steps can be **edited, reordered, or deleted** — so mistakes don't mean starting over; you can just remove or fix the one step that went wrong.
- Because the whole sequence is preserved, the entire cleanup process can be **re-run automatically** (via Refresh) if the underlying source data changes — this is what makes Power Query fundamentally different from doing the same cleanup manually with formulas.

### Close & Load

Once the data is fully cleaned and shaped, use **Close & Load** to exit the Power Query Editor and load the finished result into the worksheet (as a table or PivotTable, depending on the option chosen).

![Close & Load](images/34-close-and-load.png)

### Queries & Connections Pane

**Path:** `Data tab → Queries & Connections`

After loading, this pane lists every query in the workbook. From here you can view, edit, refresh, duplicate, or delete a query, or reopen the Power Query Editor to adjust its steps — without needing to re-import the data from scratch. You can also toggle things like load behavior and filtering directly from this pane.

![Queries & Connections pane](images/35-queries-and-connections-pane.png)

---

## 7. Transpose vs. Pivot vs. Unpivot vs. Transform — The Key Differences

These four terms get mixed up easily since they all "reshape" data — here's the clear distinction between them.

### Transform (the umbrella term)

**Transform** is the general/overall term for *any* operation that changes a column's values, structure, or type — trimming text, changing a data type, splitting a column, renaming, pivoting, unpivoting, and transposing are all **types of transformations**. It's not one specific action; it's the category that everything in this document belongs to. (This is also literally the name of the ribbon tab that houses most of these tools.)

### Transpose

**What it does:** flips the *entire table* — every row becomes a column, and every column becomes a row. It's a full 90° rotation of the data, with no logic applied to *which* values go where beyond their position.

**When to use it:** when your data is oriented the wrong way for how you want to read or chart it (e.g. dates running across columns instead of down rows).

**Path:** `Transform tab → Transpose`

![Transpose — rows become columns, columns become rows](images/23-transpose-rows-to-columns.png)

### Pivot

**What it does:** takes a "long" table (one row per observation, with a category column and a value column) and turns unique values from one column into **new column headers**, aggregating a value column underneath each. This converts long, repetitive data into a wide, summarized table.

**Example:** a long table with columns `Month`, `Category`, `Sales` becomes a wide table with one column per `Month`, showing `Sales` totals per `Category` in each row.

**Path:** select the column to pivot → `Transform tab → Pivot Column` → choose the values column and aggregation (Sum, Average, Count, etc.)

**When to use it:** when you want a summary view — fewer rows, more columns — similar in spirit to a PivotTable, but as a reusable query step.

![Pivot — new summarized table created from long data](images/24-pivot-new-table.png)

### Unpivot

**What it does:** the exact reverse of Pivot — takes a "wide" table (many separate columns for related categories, e.g. `Jan`, `Feb`, `Mar` as column headers) and collapses them back down into just **two columns**: one holding the former column names (labeled `Attribute` by default) and one holding their corresponding values (labeled `Value` by default).

**Path:** select the columns to unpivot → `Transform tab → Unpivot Columns`

![Unpivot Columns option](images/25-unpivot-columns-menu.png)

You choose which columns form the "basis" to collapse — Power Query then generates the **Attribute** (what used to be the column name) and **Value** (what used to be that column's data) pair for every row.

![Unpivot result — Attribute and Value columns generated](images/26-unpivot-select-attribute.png)

**When to use it:** when your source data is spread wide across many similarly-structured columns (e.g. one column per month, one column per product) and you need it in a clean, "tidy" long format — which is almost always the format required for PivotTables, charts, and further Power Query transformations to work well.

### Side-by-Side Comparison

| | **Transform** | **Transpose** | **Pivot** | **Unpivot** |
|---|---|---|---|---|
| **Scope** | Umbrella term for any change | Whole table | Selected column(s) | Selected column(s) |
| **What happens** | Varies — cleaning, typing, reshaping, etc. | Rows ↔ Columns swap completely | Row values → become new column headers | Column headers → become row values |
| **Row count** | Varies | Becomes old column count | Decreases (rows consolidate) | Increases (rows expand) |
| **Column count** | Varies | Becomes old row count | Increases (new columns per category) | Decreases (collapses into ~2 columns) |
| **Data shape result** | N/A (general category) | Same data, flipped orientation | Long → Wide | Wide → Long |
| **Typical use case** | Any cleanup/reshaping step | Fixing sideways-oriented data | Building a summary table | Preparing tidy data for analysis/PivotTables |

**Simplest way to remember it:**
- **Transform** = the category everything falls under
- **Transpose** = flip the whole grid, like rotating a photo
- **Pivot** = *rows become columns* (long → wide)
- **Unpivot** = *columns become rows* (wide → long) — literally undoes a Pivot

---

## 📌 Key Takeaways from Section 3

- **Always check the View tab first** (Column Quality, Distribution, Statistics) before cleaning anything — it tells you exactly where to focus.
- Power Query steps are **non-destructive and fully recorded** in Applied Steps — every action can be revisited, edited, or deleted without starting over.
- **Merge = more columns** (needs a common key); **Append = more rows** (needs matching column structure) — mixing these up is the most common Power Query mistake.
- **Unpivot** is usually the transformation you need before building a PivotTable or chart from "wide" source data — most raw exports (especially from other systems) come in wide format and need unpivoting first.
- Because Power Query remembers every step, refreshing a query **re-runs the entire cleanup automatically** on new data — this is the real advantage over doing the same steps manually in a worksheet.

---

## 🔜 What's Next

**Section 4** will likely move into PivotTables built from this cleaned Power Query output, along with charts and dashboard-style reporting.

---

*These are personal learning notes, written while studying Power Query fundamentals — screenshots included for visual reference.*
