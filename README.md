# Volatility Surface Constructor

## Summary
This project constructs a smooth volatility surface for options data from Yahoo Finance. K-Nearest Neighbours is used to detect outlier IV's, and a Radial Basis Function (RBF) is used for 3D interpolation of the surface.


## Introduction

What is implied vol?
The market's estimate of an asset's future volatility, derived from the price of an option using a model like Black Scholes.

What is a vol surface?
A 3D plot that shows how implied volatility varies with both strike price and time to maturity of options on the same underlying asset. 

Why is it useful?
The vol surface allows users to:
- price exotic options - calibrating a pricing model with IV's means exotic prices are consistent with market prices
- risk management - helps in portfolio hedging and VaR calculations.
- relative value trading - traders can compare IV across strikes/maturities to identify mispricings 

Why is the surface not flat?
- skew - demand for downside protection increases IV for lower strikes (skew), while fatter tails can cause a smile
- term structure - short term events can spike IV for shorter term options

Why do we want a smooth IV surface?
- fitting a volatility surface is a balancing act between minimising pricing errors whilst making sure it is economically plausible (smooth)

What are some common methods to cosntructing a vol surface?
- stochastic volatility models -> e.g. Heston
- parametric models -? e.g. SVI parameterisation
- interpolation models -> e.g. Spline based methods (with constraints)

What is a RBF and why use it?
- A function whose output depends only on the distance from a centre point. RBFs are used to interpolate data points into  a smooth surface by combining weighted averages from basis functions centered at each data point. This model falls udner the more general 'interpolation models' listed above.
  
- It is defined as: ϕ(∥x−c∥)
- x is an input point
- c is the centre
- ∥⋅∥ is the euclidean distance,
- ϕ is the radial function (e.g. gaussian, thin plate spline).
  

## Process

**1/ Data Collection** - collect options data from YF

**2/ Data Cleaning** - e.g. removing options that: - have ultra low IV
                                                   - have NA values
                                                   - are illiquid
                                                   - are extremely OTM/ITM

**3/ Outlier Detection** - moneyness and maturity data is normalised and neighbours are identified
                         - IVs that are more than X std deviations away from average are not considered in interpolation (this helps keep the surface smooth)

**4/ RBF Interpolation and plotting** - a meshgrid for interpolation from normalised moneyness and maturity data is constructed
                                      - the rbf interpolator calcualts model IV values at meshgrid points, meshgrid points are then rescaled
                                      - surface is plotted using Plotly's 3D Surface and Scatter classes




## Strengths 

**1/ Smooth Surface** - The interpolation provides an accurate representation of market behavior, whilst avoiding overfitting to noisy data.

**2/ Clear and Intuitive Visualization** - allows users to intuitively explore and gain insights into the IV of options, distinguishing between normal data points and outliers.

## Limitations

**1/ No direct querying of implied vol** - users cannot input parameters and obtain an interpolated IV as if an option existed at that point. Enhancing the model to allow such queries would improve its usability.

**2/ Quality of fit** - at the short end, the model struggles to distinguish between outliers and a steep IV smirk. The interpolated surface thus ignores these points and the resulting surface is flatter in the short end. 

**3/ Simplistic Approach** -


