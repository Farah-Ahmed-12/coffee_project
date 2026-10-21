# coffee_project

# ☕ Coffee Shop Chain Data Analysis Project

![Python](https://img.shields.io/badge/Python-3873A9?style=for-the-badge&logo=python&logoColor=white)
![Pandas](https://img.shields.io/badge/Pandas-150458?style=for-the-badge&logo=pandas&logoColor=white)
![SQLite](https://img.shields.io/badge/SQLite-003B57?style=for-the-badge&logo=sqlite&logoColor=white)
![Data Cleaning](https://img.shields.io/badge/Data%20Cleaning-Completed-success?style=for-the-badge)

An end-to-end data analysis project for a coffee shop chain's relational database. This phase focuses on thorough data cleaning, validation, memory optimization, and schema preparation using Python (Pandas) across **9 relational datasets** to prepare for SQL ingestion and database modeling.

---

## Relational Datasets Overview

| Original File | Cleaned Output File | Key Primary / Foreign Keys | Description & Status |
| :--- | :--- | :--- | :--- |
| `customers.csv` | `customers_cleaned.csv` | `customer_id` | Customer profiles, dates, and demographic info |
| `Dates.csv` | `Dates_cleaned.csv` | `transaction_date` / `Date_ID` | Time dimension table (Days, Weeks, Quarters, Years) |
| `generations.csv` | `generations_cleaned.csv` | `birth_year` | Generational age brackets |
| `pastry_inventory.csv` | `pastry_inventory_cleaned.csv` | `sales_outlet_id`, `product_id` | Daily pastry stock, sales, and waste tracking |
| `product.csv` | `product_cleaned.csv` | `product_id` | Master product catalog, categories, prices, and promos |
| `sales reciepts.csv` | `sales_receipts_cleaned.csv` | `transaction_id`, `product_id`, etc. | Main fact table containing 49k+ detailed sales transactions |
| `sales targets.csv` | `sales_targets_cleaned.csv` | `sales_outlet_id` | Monthly category revenue goals per outlet |
| `sales_outlet.csv` | `sales_outlet_cleaned.csv` | `sales_outlet_id`, `manager` | Store locations, dimensions, addresses, and manager IDs |
| `staff.csv` | `staff_cleaned.csv` | `staff_id` | Employee profiles, hire dates, positions, and locations |

---

##  Detailed Data Cleaning & Optimization Steps

### 1. `customers.csv`
* **Type Casting:** Converted `customer_since` and `birthdate` to `datetime64[ns]`.
* **Memory Optimization:** Downcasted `customer_id` to `int32` and `birth_year` to `int16`. Converted `gender` to `category`.

### 2. `Dates.csv`
* **Date Parsing:** Standardized `transaction_date` into proper datetime objects.
* **Type Refinement:** Converted identifiers (`Date_ID`, `Week_ID`, `Year_ID`, `Month_ID`, `Quarter_ID`) to compact integer types (`int32`, `int16`, `int8`).

### 3. `generations.csv`
* Validated generational boundaries and converted `generation` into a categorical data type.

### 4. `pastry_inventory.csv`
* Validated waste percentages and quantity counts across outlets and aligned data types for join operations.

### 5. `product.csv`
* **Price Cleaning:** Handled currency symbols (`$`) and trailing spaces in `current_retail_price` using string manipulation and regex, converting it safely to `float64`.
* **Categorical Encoding:** Mass-converted categorical features (`product_group`, `product_category`, `product_type`, `unit_of_measure`, `promo_yn`, `tax_exempt_yn`, `new_product_yn`) to `category` dtype.

### 6. `sales_receipts.csv` (Core Fact Table)
* **SQL Compatibility Fix:** Renamed the reserved SQL keyword column `order` to `order_number` to prevent syntax collisions in SQL queries.
* **String Cleaning:** Removed whitespace padding from categorical flags like `instore_yn` (`.str.strip()`).
* **Mass Type Conversion:** Converted all foreign key columns (`sales_outlet_id`, `staff_id`, `customer_id`, `product_id`, `order_number`, `line_item_id`, `quantity`) to `int32`, `transaction_id` to `int64`, and price/amount fields to `float64`.

### 7. `sales_targets.csv`
* **Column Renaming:** Fixed spacing typo in column header (`merchandise _goal` ➡️ `merchandise_goal`).
* **Bulk Casting:** Transformed all numerical target columns (`beans_goal`, `beverage_goal`, `food_goal`, `merchandise_goal`, `total_goal`) to `int32` in a single operation.

### 8. `sales_outlet.csv`
* **Header Standardization:** Fixed naming convention in `Neighorhood` to lowercase `neighborhood`.
* **Missing Value Handling:** Used Pandas nullable integer type (`Int32`) for the `manager` column to safely store null manager records (`<NA>`) without converting the column to float.

### 9. `staff.csv`
* **Artifact Removal:** Dropped empty unneeded import artifacts (`Unnamed: 6`, `Unnamed: 7`).
* **Date & String Formatting:** Converted `start_date` to `datetime64[ns]` and string attributes (`first_name`, `last_name`, `position`, `location`) to `category`.

---

##  Technical Highlights & Improvements

* **SQL Reserved Keyword Compliance:** Resolved reserved keyword conflicts (e.g., `order` ➡️ `order_number`).
* **Significant Memory Footprint Reduction:** Memory usage reduced across all DataFrames by transforming object/string types into `category` and downcasting integer sizes.
* **Schema Integrity:** Standardized primary and foreign key columns across all 9 datasets for relational joining.
