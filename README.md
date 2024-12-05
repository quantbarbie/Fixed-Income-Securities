# Fixed Income Securities final project

# Part 1. OIS and LIBOR Discount Curve Construction & Forward Swap Rate Calculation

This project constructs the **Overnight Indexed Swap (OIS)** and **LIBOR** discount curves and calculates **Forward Swap Rates** using real-world interest rate data. The implementation includes the following steps:

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

