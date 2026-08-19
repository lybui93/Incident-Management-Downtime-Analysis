# Operational Resilience Analytics
**CPS 230–Aligned Incident & Vendor Risk Dashboard | Power BI | Synthetic Data**

A portfolio project simulating an operational resilience monitoring system for a bank — tracking service incidents, tolerance-limit breaches, and third-party vendor risk.

> 📄 Full technical documentation: [`/document/Operational_Resilience_Project_Documentation.pdf`](./document/Operational_Resilience_Project_Documentation.pdf)
> 📊 Dashboard screenshots: [`/dashboard/Incident_Management_Downtime_Analysis.pdf`](./dashboard/Incident_Management_Downtime_Analysis.pdf)
> 🐍 Synthetic data generator: [`/python_data_generator/generate_synthetic_data.ipynb`](./python_data_generator/generate_synthetic_data.ipynb)

---

## Problem

Operational incidents across services and third-party vendors can cause significant financial loss and regulatory exposure. This project answers three questions leadership typically asks:

1. Which services are affected, and do they breach tolerance limits?
2. How severe is the impact — financially and operationally?
3. What's the root cause — internal system, vendor, or data logging error?

---

## Data & Architecture

**Synthetic dataset** (1,543 raw records / 1,500 unique incidents, 2024–2025) generated via Python, modeled as a **Star Schema**:

| Table | Purpose |
|---|---|
| `Fact_Incidents` | Core incident log — downtime, MTTR, financial loss, impacted transactions |
| `Dim_Operation` | Service catalog with Tier 1/2 criticality |
| `Dim_Tolerance_Limit` | Max allowed downtime / failed transactions per service |
| `Dim_Vendor` | Third-party vendor details (SLA tier, contract status) |
| `Dim_Date` | Time dimension |

---

## Methodology

Every incident is classified into one of **8 operational patterns** based on the relationship between Downtime and MTTR (e.g., *Successful Resolution*, *Self-Resolved*, *Outlier Downtime*, *Logging Error*). Anomaly thresholds are calculated automatically using the **IQR method**, not fixed cutoffs.

Records with unreliable Downtime values (negative, or suspected unit-entry errors) are excluded from aggregate statistics — full rules in the [documentation](./document/Operational_Resilience_Project_Documentation.pdf).

**Vendor Risk Score** — a Min-Max normalized, weighted score combining Financial Loss (40%), Incident Count (20%), Avg Downtime (25%), and P95 Downtime (15%).

---

## Dashboard

6 pages: Overview · Admin Delay Analysis · SLA Breach Monitor · Vendor Risk Profile · Outlier Deep-Dive · Data Quality Reference.

*(insert 1–2 key screenshots here)*

---

## Limitations

- Synthetic data — no real time-of-day timestamps, only Peak/Off-Peak flag
- Financial Loss is mathematically derived from Downtime, not an independent metric
- No designed seasonality (random date distribution)
- Small sample size at some vendor/SLA-tier combinations

---

## Tech Stack
Python (data generation) · Power BI (data model, DAX, dashboard) · Power Query (ETL & cleaning)
