# Power BI Performance Report – Gross Profit Dashboard 📊

This repository contains a simplified, easy‑to‑read Power BI report that analyzes **Gross Profit**, **Sales**, and **Quantity** performance using **Power Query**, **data modeling**, and **advanced DAX measures**. The dashboard helps explain *why* a company may experience profit drops, changing margins, or underperforming markets.

---

## 1. Overview

![Dashboard Preview](https://github.com/Mahyarosi/Performance-Report/blob/main/Performance%20Report.png)


This Power BI report breaks down how the business is performing **this year (YTD)** compared to **last year (PYTD)**. It highlights:

* Total YTD results
* A big **-5M decline vs PYTD** 😬
* Margin performance (GP%)
* Worst‑performing countries
* Monthly trends
* Product type breakdown
* Account‑level profitability

The visuals help identify where things went wrong and what parts of the business need attention.

---

## 2. Data Preparation (Power Query)

Power Query was used to clean and shape the data:

* Fixing date formats
* Creating a proper **Dim_Date** table with an `Inpast` flag
* Removing bad rows and standardizing tables
* Preparing Fact tables for modeling

Clean data ensures accurate time calculations and smooth visuals.

---

## 3. Data Modeling

The model uses a **star‑schema** structure:

* **Fact_Sales**: Sales, Quantity, Gross Profit, Date
* **Dim_Date**: Calendar fields used for time intelligence
* Additional dimension tables (Country, Product, Customer)

The disconnected slicer for metric switching keeps the UI clean and flexible.

---

## 4. Key DAX Measures

Below are the most important measures powering the dashboards. If multiple measures follow the same logic, only one is fully explained.

### **GP%**

```
GP% = DIVIDE([Gross Profit], [Sales])
```

Shows how efficiently the business converts sales into profit. A falling GP% often means increasing costs or heavy discounting.

---

### **YTD Measures**

Example:

```
YTD_GrossProfit = TOTALYTD(
    [Gross Profit],
    Fact_Sales[Date_Time]
)
```

This calculates performance from the start of the year up to the selected date.

**Why it matters:** It reveals seasonal patterns and helps track cumulative business progress.

Other YTD versions (same logic):

* `YTD_Sales`
* `YTD_Quantity`

---

### **PYTD Measures**

Example:

```
PYTD_GrossProfit = 
CALCULATE(
    [Gross Profit],
    SAMEPERIODLASTYEAR(Dim_Date[Date]),
    Dim_Date[Inpast] = TRUE
)
```

This returns last year’s performance for the same period.

**Why it matters:** It highlights whether the business is improving or falling behind. In this case, PYTD comparison shows a **-5M gap**, signaling serious performance issues.

Other PYTD versions:

* `PYTD_Sales`
* `PYTD_Quantity`

---

### **Dynamic Selector Measures (S_PYTD & S_YTD)**

These measures switch between Sales, Quantity, and Gross Profit depending on a slicer.

Example:

```
S_PYTD = 
VAR selected_value = SELECTEDVALUE('Slc Values'[Values])
VAR result = SWITCH(selected_value,
    "Sales", [PYTD_Sales],
    "Quantity", [PYTD_Quantity],
    "Gross Profit", [PYTD_GrossProfit],
    BLANK()
)
RETURN
result
```

This keeps visuals clean and dynamic.

---

## 5. Visuals & What They Reveal

### **Treemap – Bottom 10 Countries 🌍**

Shows the biggest negative contributors. China leads with almost **-2M**. Potential reasons:

* Competition
* Cost increases
* Operational inefficiencies

### **Waterfall – YTD vs PYTD by Month**

Month‑by‑month breakdown of the 5M drop. Most months are red, showing ongoing struggles instead of isolated issues.

### **Monthly Trend Chart**

Blue bars (YTD) compared to orange line (PYTD). Gaps explain which months the company underperformed.

### **Scatter Plot – GP% vs Value**

Useful for finding:

* High‑value low‑margin accounts (danger zone)
* Small accounts with strong profit potential

---

## 6. Why the Company Might Be Underperforming ❗

Based on the report insights:

* Gross Profit dropped by **5M** vs last year
* GP% sits at **40%**—possibly too low for the industry
* Big markets (China, Brazil) are dragging results down
* Seasonal dips in March & September
* Product mix might be shifting toward lower margin categories

The dashboard makes these issues visible quickly.

---

## 7. Tools Used

* Power BI Desktop
* Power Query
* DAX (time intelligence + dynamic switches)
* Star‑schema modeling

---

## 8. How to Run the Report

1. Download the PBIX file.
2. Ensure you have:

   * Fact_Sales
   * Dim_Date
   * Sales, Quantity, GP fields
3. Refresh and explore using slicers.

---

## 9. Summary

This Power BI project gives a clear, simplified view into business performance using smart modeling and time‑based DAX. It helps teams quickly identify declining markets, margin pressure, and monthly performance issues.

The structure works for any finance or profitability‑based analytics project. 🚀
