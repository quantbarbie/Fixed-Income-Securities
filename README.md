# Fixed Income Securities final project

# Part 1. OIS and LIBOR Discount Curve Construction & Forward Swap Rate Calculation

This project constructs the **Overnight Indexed Swap (OIS)** and **LIBOR** discount curves and calculates **Forward Swap Rates** using real-world interest rate data. The implementation includes the following steps:

- Data preprocessing: Importing and cleaning OIS and IRS rate data.
- Discount factor calculation: Using bootstrapping and numerical methods to calculate OIS and LIBOR discount factors.
- Forward swap rate computation: Using OIS and LIBOR discount factors to calculate the forward swap rates.

## Overview

This project provides tools for constructing discount curves for OIS and LIBOR instruments, essential for valuing derivatives and financial instruments. The discount factors are calculated using bootstrapping techniques, and forward swap rates are derived from these discount curves. The calculations are based on the interest rate data from Excel spreadsheets, and the results are visualized and stored in CSV files.

---

## Features

- **OIS Discount Curve**: Computes and visualizes OIS discount factors based on input data.
- **LIBOR Discount Curve**: Computes and visualizes LIBOR discount factors from IRS data.
- **Forward Swap Rates**: Calculates forward swap rates using the OIS and LIBOR discount factors.
- **Interpolation**: Interpolates discount factors for finer granularity between observed tenors.
- **Export Results**: Outputs discount factors and swap rates to CSV files for further analysis.

---

