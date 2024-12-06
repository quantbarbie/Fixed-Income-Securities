# Fixed Income Securities final project

# Part 1. OIS and LIBOR Discount Curve Construction & Forward Swap Rate Calculation

This section constructs the **Overnight Indexed Swap (OIS)** and **LIBOR** discount curves and calculates **Forward Swap Rates** using real-world interest rate data. The implementation includes the following steps:

- Data preprocessing: Importing and cleaning OIS and IRS rate data.
- Discount factor calculation: Using bootstrapping and numerical methods to calculate OIS and LIBOR discount factors.
- Forward swap rate computation: Using OIS and LIBOR discount factors to calculate the forward swap rates.

## Overview

This section provides tools for constructing discount curves for OIS and LIBOR instruments, essential for valuing derivatives and financial instruments. The discount factors are calculated using bootstrapping techniques, and forward swap rates are derived from these discount curves. The calculations are based on the interest rate data from Excel spreadsheets, and the results are visualized and stored in CSV files.

---

## Features

- **OIS Discount Curve**: Computes and visualizes OIS discount factors based on input data.
- **LIBOR Discount Curve**: Computes and visualizes LIBOR discount factors from IRS data.
- **Forward Swap Rates**: Calculates forward swap rates using the OIS and LIBOR discount factors.
- **Interpolation**: Interpolates discount factors for finer granularity between observed tenors.
- **Export Results**: Outputs discount factors and swap rates to CSV files for further analysis.

---

# Part 2. Swaption Pricing and Model Calibration

This section focuses on the calibration and pricing of swaptions using two models:
1. **Displaced Diffusion (DD) Model**
2. **SABR (Stochastic Alpha Beta Rho) Model**

## 1. Displaced Diffusion (DD) Model Calibration

The DD model uses beta-displacement to modify the volatility surface. This section explains how the model is calibrated and used for swaption pricing.

### Features
- **DD Model**: Implements functions for call and put option pricing using the Black-76 formula and its displaced counterpart.
- **Calibration**: Optimizes the beta and sigma parameters to match the market-implied volatilities.

---

### Implementation Details

#### 1. Black-76 Pricing Functions
```python
def Black76Call(F, K, sigma, pvbp, T):
    # Pricing formula for a call option
    ...
def Black76Put(F, K, sigma, pvbp, T):
    # Pricing formula for a put option
    ...
```
#### 2. Displaced Diffusion (DD) pricing.

```python
def DD_Call(F, K, sigma, pvbp, beta, T):
    # Pricing formula for a displaced diffusion call option
    ...
def DD_Put(F, K, sigma, pvbp, beta, T):
    # Pricing formula for a displaced diffusion put option
    ...
```
#### 3. Calibration of DD Model and swaption pricing
Calibration minimizes the error between the market-implied volatilities and model-implied volatilities:

```python
def ddcalibration(beta, F, strikes, vols, pvbp, T):
    ...
```
Then, the swaptions are priced using the calibrated parameters.

## 2. SABR Model Calibration and Swaption Pricing

The SABR model uses stochastic volatility to capture the behavior of the implied volatility surface. This section explains how the model is calibrated and used for swaption pricing.

### Features
- **SABR Model:** Implements functions for calculating implied volatility using the SABR model formula.
- **Calibration:** Optimizes the parameters (𝛼, 𝜌, 𝜈) to match market-implied volatilities.
- **Comparison with DD Model:** Provides a comparison of swaption prices obtained from SABR and DD models.

---

### Implementation Details

#### 1. SABR Model Formula

The SABR model computes implied volatility based on the parameters 𝛼, 𝛽, 𝜌, and 𝜈. It supports both ATM and non-ATM cases:

```python
def SABR(F, K, T, alpha, beta, rho, nu):
    ...
```
#### 2. Calibration of SABR

Calibration minimizes the error between market volatilities and model volatilities

```python
def sabrcalibration(x, strikes, vols, F, T):
    ...
```

#### 3. Output Parameters

The calibrated parameters are stored as DataFrames for different expiries and tenors:

- **SABR_Alpha:** 𝛼 values.
- **SABR_Rho:** 𝜌 values.
- **SABR_Nu:** 𝜈 values.

#### 4. Swaption Pricing
Swaptions are then priced using the calibrated parameters for both models:

Displaced Diffusion: Prices are computed using the DD_Call and DD_Put functions.
SABR Model: Implied volatilities are computed first, followed by option pricing using the Black-76 formula.

---
# Part 3. Convexity Correction

## Overview
This section focuses on pricing Constant Maturity Swap (CMS) products using convexity correction and comparing CMS rates with forward swap rates. The calculations leverage previously calibrated SABR parameters and discount factors.

---

## Key Components

### 1. Data Input and Preparation
- **Discount Factors**: OIS and LIBOR discount factors, along with forward LIBOR rates, are loaded from the `Discount Factors.csv` file.
- **SABR Calibration Data**: Calibrated SABR parameters (Alpha, Rho, Nu) for various expiries and tenors are extracted from `data_SABR.csv`.

### 2. Interpolation
- **Linear Interpolation**: Applied to OIS and LIBOR discount factors for intermediate tenors.
- **Cubic Spline Interpolation**: Used for SABR parameters (Alpha, Rho, Nu) across expiries and tenors.

### 3. CMS Pricing
- **Forward Swap Rate Calculation**: The forward swap rate is determined using interpolated discount factors.
- **CMS Rate Calculation**: CMS rates are computed using static replication and convexity correction. The SABR model provides the volatility needed for pricing.
- **Present Value of CMS Leg**: The PV of CMS legs receiving payments semi-annually or quarterly is computed over specified periods.

### 4. Comparison of CMS and Forward Swap Rates
- **CMS vs. Forward Swap Rates**: The project compares CMS rates and forward swap rates for various expiry and tenor combinations:
  - 1Y x 1Y, 1Y x 2Y, ..., 1Y x 10Y
  - 5Y x 1Y, 5Y x 2Y, ..., 5Y x 10Y
  - 10Y x 1Y, 10Y x 2Y, ..., 10Y x 10Y
- **Rate Differences**: The difference between CMS rates and forward swap rates is calculated to highlight convexity adjustments.

---

# Part 4. CMS Pricing with Convexity Correction

## Overview
This section calculates the present value (PV) of CMS payoffs and caplet payoffs using convexity correction, leveraging the SABR model for volatility estimation and static replication techniques.

---

## Key Components

### 1. Black-76 and SABR Models
- **Black-76 Formula**: Used for pricing options, providing the core framework for swaption valuation.
- **SABR Model**: Computes implied volatility for CMS payoffs and caplets, supporting both at-the-money (ATM) and non-ATM cases.

### 2. Convexity Adjustment Using IRR
- **IRR Calculation**:
  - Calculates the Internal Rate of Return (IRR) and its first and second derivatives.
  - These values are critical for convexity adjustment and contribute to the h-functions used in the static replication process.

### 3. CMS Payoff Pricing
- **CMS Payoff PV**:
  - Uses convexity correction by integrating over the payoff distribution.
  - Combines static replication techniques with SABR-calibrated volatilities.
  - Key components include:
    - `term1`: Adjustment based on the IRR-derived h-function.
    - `term3` and `term4`: Integrals of CMS payoffs using the h-function and Black-76 option prices.

### 4. CMS Caplet Payoff Pricing
- **CMS Caplet PV**:
  - Similar to the CMS payoff calculation but focuses on caplet-style payoffs.
  - Incorporates convexity correction and integration of caplet payoffs with SABR volatilities.
  - Key terms:
    - `term5`: Convexity adjustment for caplet-style payoff.
    - `term6`: Integral over caplet payoff distribution.

---

## Results

### 1. PV of CMS Rate Payoff
- **Input Data**:
  - Forward rate (`F`): 0.043634
  - Discount factor (`D`): 0.982184
  - SABR parameters: Alpha = 0.176437, Beta = 0.9, Rho = -0.440701, Nu = 0.494010
  - Calculated SABR volatility: 0.0016
- **Computed PV**: 0.21192

### 2. PV of CMS Caplet Payoff
- **Input Data**:
  - Same forward rate (`F`) and SABR parameters as above.
  - Strike price: 0.0016
- **Computed PV**: Incorporates convexity correction and static replication terms.



