# Predictive Credit Risk Modeling & Loan Portfolio Diagnostics
> **Underwriting Diagnostics Case Study:** Optimising automated credit-scoring algorithms for uncollateralised digital micro-lending.

---


### Purpose and Project Goals
The fundamental purpose of this project is to fix a blind automated credit-scoring algorithm for a digital consumer lending application, directly reducing the platform’s **Non-Performing Loan (NPL) Ratio** down to a sustainable banking threshold. 

Uncollateralised digital lending platforms face an existential threat: they issue capital in minutes without physical assets (like houses or cars) backing the loans. If the app's internal scoring logic cannot accurately separate healthy borrowers from defaults, bad debt rapidly accumulates, destroying company profitability. 

This project acts as a diagnostic audit. By analyzing historical loan performance data, the goal is to pinpoint exactly where capital is leaking, establish the hidden drivers of loan defaults, and provide data-backed modifications to the engineering team to restructure the app's automated underwriting rules.

### Project Objectives
*   **Identify Risk Concentrations:** Uncover default patterns across critical behavioral dimensions, specifically banking channels, employment types, interest rate pricing tiers, and borrower lifecycles.
*   **Quantify Exposure and Portfolio at Risk (PAR):** Establish the baseline historical default rate of the platform to measure overall financial health against standard central banking criteria.
*   **Formulate Algorithmic Rules:** Move from passive observation to active engineering by translating statistical defaults into concrete, code-ready conditional logic (underwriting rules) to prevent future cash bleeds.

###  Why I Selected This Project
As a Computer Science graduate pursuing a career in data and business analytics, I chose this project because credit risk management is the absolute lifeblood of the Nigerian financial ecosystem. Top fintechs and tier-1 banks do not just hire analysts who can build charts; they recruit professionals who can directly protect their balance sheets from toxic debt assets. 

Furthermore, this project perfectly bridges my technical computing background with commercial business intelligence. It allowed me to look past flat transactional columns and analyze the complex, real-world human behavior behind financial defaults in an emerging market.

### Core Challenges Faced
*   **The Profile Anonymity Void:** Managing heavy missing data structures within the onboarding pipelines. A significant portion of defaulted rows contained blank or "Unknown" employment statuses, which initially skewed demographic segments until isolated as a distinct structural risk behavior.
*   **The Volume vs. Rate Paradox:** Discovered that low-risk percentages can hide massive absolute cash losses if the segment handles high transaction volumes (e.g., massive user segments behaving averagely can bleed more total cash than tiny segments behaving horribly).
*   **Algorithmic Blindness Interpretation:** Resolving the counter-intuitive revelation that full-time salaried workers and historically loyal, repeat borrowers were out-defaulting informal traders required shifting away from simple data scanning into deep, behavioral credit-stacking diagnostics.

---


### Data Source Description 
The primary data source utilized for this diagnostic is the historical **Data Science Nigeria (DSN) / OneFi Credit Risk Dataset**. OneFi is the pioneer institution behind **Carbon** (formerly Paylater), one of Nigeria's earliest and most successful consumer fintech platforms based in Lagos. 

This dataset was specifically selected because it consists of real, historical, uncollateralised transactional data from a live Nigerian lending app. It includes thousands of rows tracking actual borrower ages, verified bank account channels, real interest markups, exact sequential loan frequencies, and true loan settlement outcomes ("good" vs. "bad" loan flags). Using this dataset ensures that the project outcomes are grounded in local emerging-market realities rather than generic, synthetic data.

### Technologies Utilized
*   **Microsoft Excel 2016:** Selected as the primary processing engine to replicate production environments found in corporate offices and banking institutions.
*   **INDEX & MATCH Engine (Excel 2016 Optimization):** Deployed for relational lookups instead of memory-heavy legacy functions, ensuring lightweight workbook performance and column-rearrangement stability.
*   **Nested Logical Arrays (`IF`, `IFERROR`):** Utilized for dynamic segment engineering, text cleaning, and blank-cell data transformation.
*   **Pivot Table Aggregate Summaries:** Used to compress data points into isolated behavioral risk matrices.
*   **Interactive Slicers & Presentation Dashboard:** Leveraged to construct an executive reporting interface with report connections linked across the entire diagnostic infrastructure.

---

 Approach & Analytical Methodology

This section outlines the precise, sequential process utilized to clean the raw inputs, engineer strategic columns, and execute the four credit underwriting diagnostic tests.

### Step 1: Data Preparation, Cleaning, and Merging
The raw data was delivered in two separate tables: a performance file containing loan numbers and financial calculations, and a demographic file containing user metadata. To perform a unified diagnostic, these sheets were relinked inside Excel 2016 using an un-anchored lookup structure.

To eliminate data entry anomalies where blank cells were pulled as literal `0` characters (which corrupts categorical text filters), a text-coercion wrapper (`&""`) was applied to the string extractions. The formulas deployed in row 2 of the master sheet were:

*   **Borrower Bank Channel Integration:**
    ```excel
    =IFERROR(INDEX(Demographics!F:F, MATCH(A2, Demographics!A:A, 0)) & "", "Unspecified Bank")
    ```
*   **Employment Status Profile Integration:**
    ```excel
    =IFERROR(INDEX(Demographics!H:H, MATCH(A2, Demographics!A:A, 0)) & "", "Unknown Status")
    ```

    ### Step 2: Feature Engineering & Risk Metric Transformation
To shift from basic reporting into predictive risk diagnostics, three entirely new metric parameters were engineered to capture financial behavior:

1.  **Interest Rate Markup % (Product Aggressiveness):** Calculated to see if high borrowing fees drive defaults.
    ```excel
    =(Total Due - Loan Amount) / Loan Amount
    ```
2.  **Repayment Status Flag (Binary Conversion):** Converted text variables ("good" vs. "bad") into binary formats (`1` vs. `0`). This allows Pivot Tables to seamlessly compute exact mathematical averages (NPL Ratios) rather than just counting rows.
    ```excel
    =IF(good_bad_flag="bad", 1, 0)
    ```
3.  **Borrower Vintage Lifecycle Tier:** Grouped borrowers based on their successful repeat loan history to track credit farming trends.
    ```excel
    =IF(loannumber=1, "1. First-Time Applicant", IF(loannumber<=3, "2. Early Repeat (2-3)", "3. High Velocity Loyalty (4+)"))
    ```

---

Diagnostic Test Outcomes & Executive Summary

By dropping the engineered master table into our diagnostic modeling grid, the overall portfolio health revealed a baseline **Grand Total NPL Ratio of 21.8%**, indicating extreme systemic stress across four core categories:

| Diagnostic Layer | Key Strategic Data Finding |
| :--- | :--- |
| **1. Bank Channels** | GTBank & Unspecified Bank channels capture 52.5% of all bad debt (500 / 952 total defaults). |
| **2. Employment Sectors** | Full-time Permanent staff represent the primary volumetric cash drain with 481 defaults (21% NPL). |
| **3. Product Pricing** | Aggressive 30% interest markups trigger adverse selection, driving 508 defaults (29% NPL). |
| **4. User Lifecycles** | Credit Farming: Early & High Velocity repeat users represent 100% of all portfolio losses. |

### Test 1: Banking Ecosystem Risk Findings
The data disproved the theory that popular commercial banks are inherently secure. While **GTBank** had an average default rate of **22%**, its immense popularity created an exposure trap, generating **261 bad loans**. Combined with **Unspecified Bank channels (239 bad loans)**, these two pipelines alone generated **52.5% of the total bad debt on the platform**. Conversely, severe structural risk was found in **Sterling Bank (36% NPL Ratio)** and **Skye Bank (28% NPL Ratio)**, indicating channels that require strict credit tightening.

### Test 2: Income & Employment Stability Risk Findings
The metrics shattered the traditional credit assumption that formal salary earners are low-risk assets. Full-time, **Permanent Staff** emerged as the primary source of portfolio defaults, contributing **481 bad loans**. This indicates that our automated underwriting engine was blind to debt-stacking behaviors and salary-delay windows across employers. High-intensity risk was also mapped to **Students (27% NPL)** due to their lack of structured revenue pipelines.

### Test 3: Product Pricing Risk Findings
The diagnostic proved that aggressive interest markups directly trigger defaults. Loans carrying a **30% interest rate markup** accounted for **508 out of 952 total default rows (53.3% of the bad loan book)** with an elevated **29% NPL Ratio**. This mathematically proves that over-charging fees strips away a borrower's capacity to pay, leading to a predatory default trap rather than increased corporate margins.

### Test 4: Vintage Loyalty & Credit Lifecycle Risk Findings
The most critical algorithmic blind spot identified was that **repeat borrowers accounted for the entirety of the portfolio's losses**. This confirms a "Credit Farming" loop where malicious users deliberately pay back tiny initial amounts on time to trick the app's automated system into lifting their limits, executing a strategic default once major capital limits are unlocked.

---

## Section 5: Strategic Risk-Mitigation Recommendations

Based on the empirical findings of this data audit, the following algorithmic modifications are proposed to the engineering team:

1.  **De-Automate Progressive Limit Scaling:** Immediately disable the app logic that automatically increases loan limits based on simple on-time repayments. Implement a secondary bank-statement and credit bureau sweep at the 3rd and 4th loan lifecycles to detect debt stacking.
2.  **Deploy Bank-Channel Credit Adjustments:** Program a conditional underwriting rule inside the application layer that automatically applies a **40% reduction in approved loan principal** for first-time applicants banking with high-volume channels (GTBank) or high-risk accounts (Sterling Bank).
3.  **Introduce Mandatory Onboarding Validation:** Re-engineer the application onboarding script to make fields like `Employment Status` strictly mandatory. Freeze automated loan processing for any profiles that attempt to submit blank fields until their profile is complete.
4.  **Implement Risk-Based Pricing Caps:** Cap maximum short-term interest markups at **15% to 20%** for standard user pools to keep the repayment values within verifiable salary limits, directly lowering the NPL ratio while preserving long-term portfolio velocity.
