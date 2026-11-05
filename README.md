# Credit Card Portfolio Analytics

A reproducible SQL and Python portfolio demonstration of credit card performance by vintage, acquisition channel, and product. All data are synthetic; no employer data, customer information, or proprietary code is included.

## Business questions

- How do receivables, delinquency, losses, and cash flow change over time?
- Which channel/product combinations contribute the most across the observation window?
- How do acquisition vintages compare at the same months on book?
- Why can positive contribution coexist with negative cash flow?

## View the report

**[View the visual report](https://kellamk.github.io/credit-card-portfolio-analytics/)**

The live link becomes available after GitHub Pages is enabled for this repository. For offline viewing, download this repository and open **[portfolio_report.html](reports/portfolio_report.html)** in a browser. The report is self-contained and works offline. GitHub displays the HTML source rather than the rendered page; download the file to view it. You can use your browser's Print function to save a PDF.

Read **[generated findings](reports/findings.md)** directly on GitHub, or inspect the **[calculated metrics](reports/metrics.json)**.

## Run it yourself

Requires Python 3.10 or newer. No external packages, accounts, API keys, or paid tools are required.

```sh
python run_analysis.py
```

The script generates synthetic data in an in-memory SQLite database, executes `analysis.sql`, validates the results, and writes the report, findings, and metrics into `reports/`. Re-running replaces these generated files. Seed 42 makes the results reproducible within the same Python environment.

## What is demonstrated

- **SQL:** aggregation, conditional sums, reusable views, window functions, and denominator protection
- **Python:** reproducible data generation, SQLite orchestration, validation, and report generation
- **Credit analytics:** delinquency, gross charge-offs, channel/product segmentation, and equal-age vintage comparisons
- **Reporting:** an executive-style summary, cash flow visualization, segment comparisons, and interpretation limits

## Dataset

The simulation contains 2,400 accounts opened in six monthly vintages from January through June 2025, observed through June 2026. Each vintage begins with 400 accounts. Acquisition channels are Direct, Marketplace, and Partner; products are Core and Rewards.

Each account-month includes beginning and ending receivables, purchases, principal and interest collections, days past due, charge-offs, recoveries, servicing costs, and acquisition costs. Account identifiers are fictional integers. Defaulted accounts remain in the panel with zero receivables and no subsequent activity.

Channel risk, acquisition costs, product spending, and servicing assumptions are deliberately different so the analysis can illustrate segmentation. These are modeling choices, not empirical conclusions about actual products or channels. The delinquency process is intentionally simplified, not a calibrated credit model.

## Metric definitions

| Metric | Definition |
| --- | --- |
| Ending receivables | Beginning balance + purchases − principal collections − gross charge-offs |
| 30+ delinquency | Ending balances on accounts 30+ days past due / total ending receivables |
| Gross charge-offs | Outstanding principal removed when an account reaches 120 days past due in this simulation |
| Net cash flow | Principal collections + interest collections + recoveries − purchases − servicing costs − acquisition costs |
| Contribution | Interest collections + recoveries − gross charge-offs − servicing costs − acquisition costs |
| Equal-age loss measure | Cumulative gross charge-offs / cumulative purchases at month 12; not an annualized charge-off rate |

Amounts are in illustrative U.S. dollars. Contribution is **not net profit**: funding costs, interchange, rewards, taxes, and overhead are excluded. Balances exclude accrued interest. Recoveries are simplified as an immediate 8% of charge-offs, rather than a realistic delayed recovery process. Purchases are subject to a simulated credit limit; no new purchases occur after default.

## Validation

The script stops if checks fail for:

- Expected distinct account count and unique account-month records
- Balance roll-forward and month-to-month balance continuity
- Nonnegative balances and purchases
- Delinquency ratios between zero and one
- Reconciliation of monthly and segment totals for purchases, charge-offs, contribution, and cash flow

## Interpretation limits

Segment totals are affected by account count, age, risk mix, and product mix. They do not measure causal channel effects. Vintage comparisons use month 12 for every cohort to reduce age-related distortion, but do not control for every difference. A real-world recommendation would require data-quality review, more complete economics, normalized comparisons, and appropriate testing.

## Project files

```text
analysis.sql                 Reporting views and vintage window calculations
run_analysis.py              Data generation, checks, and reporting
reports/portfolio_report.html  Self-contained visual report
reports/findings.md           Generated narrative findings
reports/metrics.json          Calculated monthly, segment, and vintage results
```

## About

Kellam Karst — financial, credit, and marketing analytics.

[LinkedIn](https://www.linkedin.com/in/kellamkarst/) · [GitHub](https://github.com/kellamk)

This is an AI-assisted portfolio demonstration prepared for review and learning. It does not claim deployment in a production environment or measured business results. Review and understand the implementation before presenting it in an interview.
