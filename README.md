# MSCS 634 Lab 6: Association Rule Mining with Apriori and FP-Growth

## Purpose

This lab explores frequent itemset mining and association rules using the public UCI Online Retail dataset. It compares Apriori and FP-Growth on the same transaction-product basket, analyzes support, confidence, and lift, and uses Seaborn visualizations to make item frequencies, co-occurrences, frequent itemsets, and rules easier to interpret.

## Contents

- `nmetha_lab6.ipynb`: Complete analysis and visualizations.
- `README.md`: Lab summary and run instructions.

## Approach

The notebook loads the UCI Online Retail data (541,909 invoice lines), cleans it (cancelled invoices, missing fields, non-positive quantities and prices, postage/fee lines), and keeps United Kingdom purchases: 484,040 lines across 17,903 invoices. It samples 5,000 invoices (`random_state=42`) and builds a Boolean basket of the 25 most frequent products. Apriori and FP-Growth both run at 1% minimum support with itemsets of at most three items, and rules use a 50% minimum confidence.

## Key Insights

- **Frequent items:** `WHITE HANGING HEART T-LIGHT HOLDER` is the most common product, in 600 of 5,000 invoices (12.0%). About 41% of sampled invoices contain none of the 25 selected products.
- **Algorithms agree:** Apriori and FP-Growth found the same 237 itemsets at 1% support and the same 95 rules. Itemset counts also matched at every tested threshold: 825, 237, 61, 34, and 25 at 0.5%, 1%, 2%, 3%, and 5%.
- **Runtime:** Using the median of five runs at 1% support, Apriori took 0.0179 s and FP-Growth 1.0558 s (about 59 times slower). Apriori was faster at all five thresholds. This applies to this small basket and the `mlxtend` implementation, not to all data.
- **Rules:** All 95 rules have lift above 1 (about 5.5 to 11.8). The strongest link variants of one product family (lunch bags, jam making sets, jumbo bags), and two-item antecedents give higher lift than one-item ones.
- **Top rule:** `{JUMBO BAG PINK POLKADOT, JUMBO SHOPPER VINTAGE RED PAISLEY} -> {JUMBO STORAGE BAG SUKI}` has support 1.50% (75 invoices), confidence 67.57%, and lift 11.81. It shows an association, not causation.

Results describe 5,000 sampled UK invoices and 25 products, so rare products are not covered.

## Challenges and Decisions

- **Size:** The full workbook is large, so I used a seeded sample and the 25 most frequent products. This keeps mining fast but misses rare associations.
- **Support denominator:** I kept sampled invoices with none of the selected products in the basket, so 1% support means at least 50 of 5,000 invoices.
- **Missing `CustomerID`:** About 25% of raw lines lack it, but invoices and product descriptions are enough, so those rows were kept.
- **Support threshold:** I swept 0.5% to 5%. At 5% no multi-item itemsets remain, so 1% was chosen.
- **Timing:** A single timing was unreliable, so I used the median of five runs.

## How to Run

Open `nmetha_lab6.ipynb` in VS Code and select the project `.venv` as the Python kernel. If the environment does not exist yet, create it from the project root and install the notebook dependencies:

```bash
python3 -m venv .venv
source .venv/bin/activate
python3 -m pip install pandas matplotlib seaborn requests openpyxl mlxtend ipykernel
```

Run all cells from top to bottom. The first data cell downloads the UCI archive, so an internet connection is required.
